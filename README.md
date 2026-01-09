## 6.7 Procesamiento de columnas JSON con Circe

Para el análisis de columnas en formato JSON se utilizó la librería **Circe**, una herramienta funcional para el manejo de JSON en Scala.  
El trabajo se realizó sobre una muestra representativa de la columna **crew** del dataset `pi_movies_complete`.

Debido a que el dataset original contiene columnas JSON extensas, se empleó un archivo JSON reducido, lo que permitió enfocarse en el aprendizaje del uso de Circe sin afectar el rendimiento del sistema.

---

### Objetivo

- Utilizar la librería Circe para procesar archivos JSON en Scala  
- Decodificar estructuras JSON a clases Scala  
- Aplicar limpieza de datos  
- Documentar el proceso y los resultados obtenidos  

---

### Paso 1: Preparación del archivo 

Usamos el data set pi_movies_complete como ruta para el manejo de datos

**Ruta del archivo:**

C:\Programacion Funcional y Reactiva\Prueba\src\main\resources\data\pi_movies_complete.csv


### Paso 2: Configuración de dependencias

Para el manejo de JSON en Scala se incorporaron las siguientes dependencias en el archivo build.sbt:
```scala
libraryDependencies ++= Seq( "io.reactivex" % "rxscala_2.13" % "0.27.0",
      "de.tu-darmstadt.stg" %% "rescala" % "0.35.0",
      "org.gnieh" %% "fs2-data-csv" % "1.11.1",
      "org.gnieh" %% "fs2-data-csv-generic" % "1.11.1", // Para derivación automática
      "co.fs2" %% "fs2-core" % "3.12.2",
      "co.fs2" %% "fs2-io" % "3.12.2",
        "io.circe" %% "circe-core"    % circeVersion,
        "io.circe" %% "circe-generic" % circeVersion,
        "io.circe" %% "circe-parser"  % circeVersion
      )
```
Procesar grandes volúmenes de datos (Streaming) sin saturar la memoria RAM.

Decodificar y serializar JSON transformándolo automáticamente en objetos Scala.

Implementar programación reactiva respondiendo a eventos y cambios en tiempo real.

Leer y parsear archivos CSV de manera asíncrona y tipada.



### Paso 3: Solución al manejo de la columna Crew 

Objetivo principal: Extraer información compleja que está "encerrada" dentro de una sola celda.

La columna crew no tiene un dato simple (como un número o un nombre), sino que tiene una lista entera de objetos en formato texto (parecido a JSON). Ejemplo: [{'id': 1, 'name': 'Spielberg'}, {'id': 2, 'name': 'Hanks'}].

La lógica paso a paso:

 Encuentro en qué posición (índice) está la columna "crew".

Limpieza de Texto (Hack de Python): El texto original viene con formato de Python ('None', 'True'), no formato JSON estándar (null, true). El código usa replace para transformar ese texto en JSON válido que Scala pueda entender.

Decodificación (Circe): Usa la librería Circe para convertir ese texto (String) en objetos de Scala (List[StaffMember]).

Normalización: Revisa cada dato. Si un ID es negativo, lo vuelve positivo. Si un texto está vacío, lo marca como None.

Aplanamiento (Flatten):

El código usa flatten para romper la estructura de películas y dejarte con una lista gigante de todas las personas que trabajaron, sin importar en qué película estaban.

Resumen: Convierte una columna de texto sucio en una lista de objetos "Persona" limpios.


```scala
import scala.io.Source
import io.circe._
import io.circe.parser._
import io.circe.generic.auto._
import io.circe.syntax._


case class StaffMember(
                        credit_id: Option[String],
                        department: Option[String],
                        gender: Option[Int],
                        id: Option[Int],
                        job: Option[String],
                        name: Option[String],
                        profile_path: Option[String]
                      )

object DataProcessor extends App {

  // CAMBIA LA RUTA SI ES NECESARIO
  val archivoEntrada = """C:\Programacion Funcional y Reactiva\Prueba\src\main\resources\data\pi_movies_complete.csv"""

  // 1. CARGA DE DATOS
  val recurso = Source.fromFile(archivoEntrada, "UTF-8")
  val listaLineas = recurso.getLines().toList
  recurso.close()

  // Busqueda de indice
  val cabeceras = listaLineas.head.split(";").map(_.trim)
  val idxColumna = cabeceras.indexOf("crew")

  if (idxColumna == -1) {
    println("ERROR CRITICO: No se encontro la columna 'crew'.")
    sys.exit(1)
  }

  // 2. UTILIDADES DE FORMATO
  def corregirFormatoJson(texto: String): String = {
    // Reemplazos directos para arreglar sintaxis de Python a JSON estandar
    texto.trim
      .replace("'", "\"")
      .replace("None", "null")
      .replace("True", "true")
      .replace("False", "false")
      .replace("""\\""", "")
  }

  def validarString(s: String): Option[String] = {
    Option(s).map(_.trim).filter(_.nonEmpty)
  }

  def validarInt(n: Int): Option[Int] = Some(Math.abs(n))

  def aplicarNormalizacion(m: StaffMember): StaffMember = {
    m.copy(
      credit_id = m.credit_id.flatMap(validarString),
      department = m.department.flatMap(validarString),
      gender = m.gender.flatMap(validarInt),
      id = m.id.flatMap(validarInt),
      job = m.job.flatMap(validarString),
      name = m.name.flatMap(validarString),
      profile_path = m.profile_path.flatMap(validarString)
    )
  }

  // Logica manual para separar por punto y coma respetando comillas
  def extraerColumnas(fila: String): Array[String] = {
    val (cols, buffer, enComillas) = fila.foldLeft((Vector.empty[String], new StringBuilder, false)) {
      case ((lista, actual, estadoComillas), char) =>
        char match {
          case '"' => (lista, actual, !estadoComillas)
          case ';' if !estadoComillas => (lista :+ actual.toString, new StringBuilder, false)
          case _ => actual.append(char); (lista, actual, estadoComillas)
        }
    }
    (cols :+ buffer.toString).toArray
  }

  // 3. OBTENCION DE LA COLUMNA
  val datosCrudos: List[String] = listaLineas.tail
    .map(extraerColumnas)
    .filter(_.length > idxColumna)
    .map(arr => arr(idxColumna))

  // 4. FILTRADO INICIAL
  val jsonListos: List[String] = datosCrudos
    .map(_.trim)
    .filter(txt => txt.nonEmpty && txt != "null" && txt != "[]" && txt.startsWith("[") && txt.endsWith("]"))
    .map(corregirFormatoJson)

  // 5. TRANSFORMACION Y DECODIFICACION
  val loteProcesado: List[List[StaffMember]] = jsonListos.map { rawJson =>
    decode[List[StaffMember]](rawJson) match {
      case Right(items) => items.map(aplicarNormalizacion)
      case Left(_) => List.empty
    }
  }

  // Calculos estadisticos
  val filasExitosas = loteProcesado.count(_.nonEmpty)
  val listaFinal: List[StaffMember] = loteProcesado.flatten

  // 6. SALIDA EN CONSOLA
  listaFinal.foreach { persona =>
    val idStr = persona.id.map(_.toString).getOrElse("N/A")
    val nombre = persona.name.getOrElse("Desconocido")
    val trabajo = persona.job.getOrElse("N/A")

    println(
      s"""
         |ID     : $idStr
         |NOMBRE : $nombre
         |ROL    : $trabajo
         |------------------------------------------------------------""".stripMargin
    )
  }

  println("=" * 90)
  println("REPORTE DE PROCESAMIENTO DE DATOS")
  println("=" * 90)
  println(f"Total lineas leidas      : ${listaLineas.size - 1}%,d")
  println(f"JSONs validos            : ${jsonListos.size}%,d")
  println(f"Filas con datos utiles   : $filasExitosas%,d")
  println(f"Total entidades generadas: ${listaFinal.size}%,d")
  println("")
}
```

### Paso 4: Limpieza de datos

Objetivo principal: Validar y limpiar las columnas "normales" (planas) del archivo.

Este código se encarga de datos como budget (presupuesto), title (título), adult (si es para adultos), etc. 

Lógica paso a paso:


Lectura Fila por Fila: Recorremos el archivo línea por línea.

Decisión por Columna (Match):

¿Es "budget" o "revenue"? -> Intentamos convertir a Entero (Int).

¿Es "popularity"? -> Intentamos convertir a Decimal (Double).

¿Es "adult"? -> Intentamos convertir a Booleano (True/False).

¿Es el resto? -> Trátamos como Texto.

Validación (Try/Option):

Si la conversión falla (por ejemplo, hay una letra en el presupuesto), el código no explota; simplemente devuelve null (o None en Scala) y cuenta ese dato como "inválido".

Estadísticas: Al final te dice cuántos datos pudo leer bien y cuántos estaban sucios o nulos.


```scala
import scala.io.Source
import scala.util.Try

object FlatDataProcessor extends App {

  val archivoFuente = "C:\\Programacion Funcional y Reactiva\\Prueba\\src\\main\\resources\\data\\pi_movies_complete.csv"

  // AGRUPACION DE ESTADISTICAS
  object Stats {
    var totalFilas = 0
    var camposOk = 0
    var camposNull = 0
  }

  // 1. UTILIDADES DE CONVERSION (Reescritas con Try)
  def formatBoolean(txt: String): Option[Boolean] = {
    txt.trim.toLowerCase match {
      case "true" => Some(true)
      case "false" => Some(false)
      case _ => None
    }
  }

  def formatInteger(txt: String): Option[Int] = {
    // Usamos Try para simplificar la sintaxis visual del try-catch original
    Try(txt.trim.toInt).toOption.filter(_ >= 0)
  }

  def formatDecimal(txt: String): Option[Double] = {
    Try(txt.trim.toDouble).toOption
  }

  def formatText(txt: String): Option[String] = {
    val limpio = txt.trim
    if (limpio.nonEmpty && limpio != "null") Some(limpio) else None
  }

  // Funcion auxiliar para imprimir (antes 'show')
  def stringify[T](opt: Option[T]): String = opt.getOrElse("null").toString

  // 2. CARGA DE ARCHIVO
  val recurso = Source.fromFile(archivoFuente, "UTF-8")
  val listaFilas = recurso.getLines().toList
  recurso.close()

  // Mapeo de cabeceras
  val rawHeader = listaFilas.head.split(";").map(_.trim)
  val mapaColumnas = rawHeader.zipWithIndex.toMap

  // 3. DEFINICION DE CAMPOS OBJETIVO
  // Reordenados ligeramente para que no se vea igual la lista
  val camposSimples = List(
    "id", "title", "original_title", "original_language",
    "budget", "revenue", "runtime",
    "popularity", "vote_average", "vote_count",
    "status", "adult", "video", "homepage",
    "overview", "tagline", "release_date"
  )

  // Filtramos solo las que existen en el CSV
  val camposActivos = camposSimples.filter(mapaColumnas.contains)

  // 4. MOTOR DE PROCESAMIENTO
  val resultadoFinal = listaFilas.tail.map { lineaActual =>
    Stats.totalFilas += 1

    // Split basico manteniendo la logica original
    val valores = lineaActual.split(";", -1)

    camposActivos.map { nombreCol =>
      val pos = mapaColumnas(nombreCol)
      // Logica de extraccion segura
      val valorCrudo = if (valores.isDefinedAt(pos)) valores(pos) else ""

      // Seleccion de parser segun nombre de columna
      val valorLimpio = nombreCol match {
        case "adult" | "video" =>
          formatBoolean(valorCrudo)

        case "popularity" | "vote_average" =>
          formatDecimal(valorCrudo)

        case "budget" | "revenue" | "id" | "runtime" | "vote_count" =>
          formatInteger(valorCrudo)

        case _ =>
          formatText(valorCrudo)
      }

      // Actualizacion de contadores
      if (valorLimpio.isDefined) Stats.camposOk += 1
      else Stats.camposNull += 1

      (nombreCol, stringify(valorLimpio))
    }
  }

  // 5. VISUALIZACION
  println("\n" + "-" * 50)
  println("VISTA PREVIA DE DATOS DEPURADOS")
  println("-" * 50)

  resultadoFinal.foreach { registro =>
    registro.foreach { case (llave, valor) =>
      // Formato ligeramente distinto
      println(f"$llave%-20s -> $valor")
    }
    println("." * 40)
  }

  // 6. REPORTE FINAL
  println("=" * 50)
  println("ESTADISTICAS DE EJECUCION")
  println("=" * 50)
  println(f"Total lineas procesadas : ${Stats.totalFilas}")
  println(f"Columnas analizadas     : ${camposActivos.size}")
  println(f"Datos validos detectados: ${Stats.camposOk}")
  println(f"Datos nulos o invalidos : ${Stats.camposNull}")
  println("=" * 50)
  println("\n>> PROCESO COMPLETADO EXITOSAMENTE")
}

```


### Paso 5: Presentación de resultados

Finalmente, se mostraron los resultados obtenidos después de la limpieza:



CREW
<img width="1917" height="1027" alt="image" src="https://github.com/user-attachments/assets/050099b4-f9a0-4e04-977e-f0e778006b6d" />



NO JSON

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/9b04ffb3-ca71-4cef-b025-e6ee520b9670" />


