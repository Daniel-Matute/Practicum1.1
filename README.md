# ProyectoIntegrador_PFR
Repositorio del proyecto integrador - Programación funcional y reactica

  -Desarrollar los siguiente ítems


  -Tablas de datos (nombre de columna, tipo, propósito y observaciones) - README.md

  Entidad:Movies

| Columna           | Tipo de dato  | Propósito                               | Observaciones                    |
| ----------------- | ------------- | --------------------------------------- | -------------------------------- |
| id                | Numérico (PK) | Identificador único de la película      | Clave primaria                   |
| imdb_id           | Texto         | Identificador de la película en IMDb    | Puede contener valores nulos     |
| title             | Texto         | Título comercial de la película         | Usado para visualización         |
| original_title    | Texto         | Título en su idioma original            | Puede coincidir con `title`      |
| overview          | Texto largo   | Descripción general de la trama         | Texto libre                      |
| tagline           | Texto         | Frase promocional                       | Muchos valores nulos             |
| status            | Texto         | Estado de la película (Released, etc.)  | Categoría controlada             |
| release_date      | Fecha         | Fecha de estreno                        | Útil para análisis temporal      |
| runtime           | Numérico      | Duración de la película en minutos      | Puede presentar valores atípicos |
| budget            | Numérico      | Presupuesto de producción               | Alta presencia de valores nulos  |
| revenue           | Numérico      | Recaudación obtenida                    | Valores nulos frecuentes         |
| popularity        | Numérico      | Índice de popularidad                   | Métrica relativa                 |
| vote_average      | Numérico      | Promedio de votaciones                  | Escala normalizada               |
| vote_count        | Numérico      | Número total de votos                   | Relacionado con popularidad      |
| adult             | Booleano      | Indica si es contenido para adultos     | Verdadero/Falso                  |
| video             | Booleano      | Indica si es contenido en formato video | Poco frecuente                   |
| original_language | Texto         | Idioma original de la película          | Código ISO 639-1                 |
| homepage          | Texto         | Página web oficial                      | Mayormente nula                  |
| poster_path       | Texto         | Ruta de la imagen promocional           | Uso visual                       |





Entidad: PRODUCTION_COMPANIES

| Columna      | Tipo          | Propósito                      | Observaciones     |
| ------------ | ------------- | ------------------------------ | ----------------- |
| company_id   | Numérico (PK) | Identificador de la productora | Clave primaria    |
| company_name | Texto         | Nombre de la productora        | Texto descriptivo |




Entidad: PRODUCTION_COUNTRIES

| Columna      | Tipo  | Propósito       | Observaciones                    |
| ------------ | ----- | --------------- | -------------------------------- |
| country_code | Texto | Código del país | Basado en estándar internacional |
| country_name | Texto | Nombre del país | Información descriptiva          |




Entidad: LANGUAGES

| Columna       | Tipo  | Propósito         | Observaciones     |
| ------------- | ----- | ----------------- | ----------------- |
| language_code | Texto | Código del idioma | Formato ISO 639-1 |
| language_name | Texto | Nombre del idioma | Texto descriptivo |




Entidad: GENRES

| Columna    | Tipo          | Propósito                | Observaciones      |
| ---------- | ------------- | ------------------------ | ------------------ |
| genre_id   | Numérico (PK) | Identificador del género | Clave primaria     |
| genre_name | Texto         | Nombre del género        | Categoría temática |




Entidad: KEYWORDS

| Columna      | Tipo          | Propósito                            | Observaciones     |
| ------------ | ------------- | ------------------------------------ | ----------------- |
| keyword_id   | Numérico (PK) | Identificador de la palabra clave    | Clave primaria    |
| keyword_name | Texto         | Palabra clave asociada a la película | Texto descriptivo |




Entidad: CAST

| Columna        | Tipo          | Propósito               | Observaciones        |
| -------------- | ------------- | ----------------------- | -------------------- |
| cast_id        | Numérico (PK) | Identificador del actor | Clave primaria       |
| actor_name     | Texto         | Nombre del actor        | Texto libre          |
| character_name | Texto         | Personaje interpretado  | Puede repetirse      |
| gender         | Numérico      | Género del actor        | Codificación interna |




Entidad: CREW

| Columna    | Tipo          | Propósito                            | Observaciones          |
| ---------- | ------------- | ------------------------------------ | ---------------------- |
| crew_id    | Numérico (PK) | Identificador del miembro del equipo | Clave primaria         |
| crew_name  | Texto         | Nombre del miembro del equipo        | Texto libre            |
| job        | Texto         | Rol desempeñado                      | Ej. Director, Producer |
| department | Texto         | Área de trabajo                      | Categoría descriptiva  |




Entidad: BELONGS_TO_COLLECTION

| Columna         | Tipo          | Propósito                     | Observaciones  |
| --------------- | ------------- | ----------------------------- | -------------- |
| collection_id   | Numérico (PK) | Identificador de la colección | Clave primaria |
| collection_name | Texto         | Nombre de la colección        | Puede ser nulo |
| poster_path     | Texto         | Imagen de la colección        | Uso visual     |
| backdrop_path   | Texto         | Imagen de fondo               | Opcional       |




Entidad: RATINGS

| Columna       | Tipo          | Propósito                 | Observaciones   |
| ------------- | ------------- | ------------------------- | --------------- |
| rating_id     | Numérico (PK) | Identificador del rating  | Clave primaria  |
| rating_source | Texto         | Fuente de la calificación | Ej. usuarios    |
| rating_value  | Numérico      | Valor del rating          | Escala numérica |



 5.2 Lectura de columnas numéricas

 Lectura de columnas numéricas del dataset

El dataset pi_movies_complete contiene varias columnas de tipo numérico que permiten analizar aspectos económicos, temporales y de popularidad de las películas. Estas variables son clave para realizar análisis estadísticos y comparativos posteriores.


<img width="618" height="577" alt="image" src="https://github.com/user-attachments/assets/2f18798d-539e-4eb3-948e-2e80b2b49549" />

<img width="644" height="533" alt="image" src="https://github.com/user-attachments/assets/1427f64d-637f-4f27-9bf2-2b7c7279dfab" />


<img width="735" height="711" alt="image" src="https://github.com/user-attachments/assets/771e6d6c-6da3-469c-884b-e9d206289a42" />




5.3 Análisis de datos en columnas numéricas (estadísticas básicas)

 Análisis de datos en columnas numéricas

Para comprender el comportamiento general de las variables numéricas del dataset pi_movies_complete, se realizó un análisis descriptivo utilizando estadísticas básicas como media, mediana, valores mínimos y máximos. Este análisis permite identificar patrones generales, dispersión de los datos y posibles valores atípicos.

<img width="710" height="617" alt="image" src="https://github.com/user-attachments/assets/8eb87c4a-b08c-4838-a25c-c89807cc31b4" />

<img width="646" height="679" alt="image" src="https://github.com/user-attachments/assets/a31c196c-083a-419b-99db-5e816bc31021" />


5.4 Análisis de datos en columnas tipo texto (algunas col. - distribución de frecuencia). ja 
  

# Análisis de Variables de Texto (No Estructuradas)

El dataset `pi_movies_complete` incluye diversas columnas de tipo texto que permiten describir características generales y contextuales de las películas. Estas variables no se utilizan para cálculos estadísticos directos, pero resultan fundamentales para el análisis descriptivo, la segmentación de datos y la identificación de patrones cualitativos.

Para este análisis se consideran únicamente columnas de texto simples y se excluyen aquellas que almacenan información estructurada en formato JSON, como géneros, reparto, países de producción o palabras clave.

Las principales columnas de tipo texto analizadas son las siguientes:

---

### Idioma original (`original_language`)
Esta columna indica el idioma original de la película, representado mediante códigos normalizados ISO 639-1. El análisis de distribución de frecuencia muestra una clara concentración en el idioma inglés, lo que refleja la fuerte presencia de la industria cinematográfica anglosajona dentro del dataset.

No obstante, también se identifican películas en otros idiomas como francés, alemán, italiano, japonés y español, lo que evidencia una diversidad lingüística.

### Fechas de estreno (`release_date`)
La columna de fechas de estreno corresponde a un atributo textual que posteriormente puede transformarse a tipo fecha. El análisis de frecuencia por año muestra que las películas abarcan un periodo histórico amplio, desde finales del siglo XIX hasta producciones recientes del siglo XXI.

Se observa un aumento progresivo en la cantidad de registros a partir de la segunda mitad del siglo XX, con una mayor concentración en las últimas décadas.

### Estado de la película (`status`)
Esta variable categórica indica el estado de producción de la película. La distribución de frecuencia evidencia que la gran mayoría de los registros se encuentran en estado **Released**, mientras que una proporción menor corresponde a estados como **Post Production** o **In Production**.

### Clasificación de contenido (`adult`)
Aunque se trata de una variable booleana, su análisis se realiza como una categoría textual. La distribución muestra que la gran mayoría de las películas no están clasificadas como contenido para adultos, mientras que solo un porcentaje muy reducido pertenece a esta categoría.

### Presencia en línea (`homepage`)
Esta columna indica si la película cuenta con un sitio web oficial. El análisis muestra que una proporción significativa de registros presenta valores nulos, lo cual es común en películas antiguas o producciones con menor difusión comercial.

### Títulos de la película (`title` - `original_title`)
Ambas columnas corresponden a atributos nominales utilizados para identificar la película.
* El campo `title` representa el título comercial.
* Mientras que `original_title` conserva el título en el idioma original.

### Descripción y elementos promocionales (`overview` - `tagline`)
Estas columnas almacenan texto libre de longitud variable.
* `overview` contiene una descripción general de la trama.
* `tagline` corresponde a una frase promocional asociada a la película.

### Recursos visuales y enlaces (`poster_path`)
Estas columnas contienen rutas o enlaces asociados a material visual o informativo.
* `poster_path` almacena la ruta de la imagen promocional.
* `homepage` indica la existencia de un sitio web oficial.

### Indicador de formato (`video`)
Aunque es una variable booleana, se trata como categórica textual. Su frecuencia indica si una película fue distribuida principalmente en formato video.

<img width="637" height="580" alt="image" src="https://github.com/user-attachments/assets/4501432a-dbf3-488e-abdb-523ee22fb969" />

<img width="645" height="383" alt="image" src="https://github.com/user-attachments/assets/94ec416a-f838-4178-8964-4e25f6a3152b" />

<img width="634" height="562" alt="image" src="https://github.com/user-attachments/assets/97165b45-3bf0-462b-b97f-094f960ca7f0" />

<img width="631" height="404" alt="image" src="https://github.com/user-attachments/assets/85b55967-59a3-446b-94d6-482621dbdde3" />

<img width="641" height="712" alt="image" src="https://github.com/user-attachments/assets/2e3569e5-d8d5-4602-9494-521d7dd4636f" />

<img width="639" height="231" alt="image" src="https://github.com/user-attachments/assets/92cf7a63-49a5-43ee-8379-7bc0dbeca5ed" />



5.5 Limpieza de datos (columnas con valores nulos, valores atípicos, etc.) 


<img width="642" height="431" alt="image" src="https://github.com/user-attachments/assets/f752f07d-2bcf-4a73-a326-db5212142e48" />

<img width="668" height="522" alt="image" src="https://github.com/user-attachments/assets/50a68bd1-47a2-49c1-837f-393e3997b8e3" />

<img width="639" height="646" alt="image" src="https://github.com/user-attachments/assets/d4dcf024-cc6d-4c11-ae8d-e4cfd3a357fc" />
<img width="646" height="279" alt="image" src="https://github.com/user-attachments/assets/a057fcfc-8b26-4a4a-a9e1-34c449194c70" />
<img width="643" height="698" alt="image" src="https://github.com/user-attachments/assets/fe0f2ca2-2814-4998-bcf0-1be3955667f2" />

<img width="643" height="249" alt="image" src="https://github.com/user-attachments/assets/5d6853f8-3e6d-4d2d-8076-d217dd7bc37e" />

<img width="644" height="671" alt="image" src="https://github.com/user-attachments/assets/7014c603-9287-4c2b-910c-4a07ab36dac0" />

  <img width="619" height="275" alt="image" src="https://github.com/user-attachments/assets/a7d119ae-1240-4216-b7f2-8eaed1d8c79a" />


<img width="645" height="651" alt="image" src="https://github.com/user-attachments/assets/55bbda2f-5f40-4424-9de3-abc40bf8b23a" />


### Avance 2

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

### Usar cualquier JSON pequeño para aprender circe 

 Estructura de ejmplo:

 ```json
[
  {
    "id": 1,
    "job": "Director",
    "department": "Directing",
    "name": "Christopher Nolan"
  },
  {
    "id": 2,
    "job": "Producer",
    "department": "Production",
    "name": "Emma Thomas"
  },
  {
    "id": 3,
    "job": "",
    "department": "Production",
    "name": ""
  }
]
```
### Usar en algunas columnas JSON  para obtener datos. 

Se definió una clase de dominio que representa la estructura de cada elemento de la columna crew:
```scala
case class CrewMember(
  id: Int,
  job: String,
  department: String,
  name: String
)
```
```scala
import io.circe.*
import io.circe.generic.auto.*
import io.circe.parser.*

import scala.io.Source

object CirceCrewExample extends App {

  val path = "src/main/resources/data/crew.json"

  // 1. Lectura del archivo JSON
  val jsonString = Source.fromFile(path).getLines().mkString

  // 2. Decodificación del JSON
  val decoded = decode[List[CrewMember]](jsonString)

  decoded match {
    case Left(error) =>
      println(s"❌ Error al parsear JSON: $error")

    case Right(crewList) =>

      println(s"📥 Total de registros leídos: ${crewList.size}")

      // 3. Limpieza de datos
      val crewLimpio = crewList.filter(c =>
        c.name.nonEmpty && c.job.nonEmpty
      )

      println(s"🧹 Registros válidos tras limpieza: ${crewLimpio.size}")

      // 4. Resultados finales
      println("\n🎬 CREW VÁLIDO:")
      crewLimpio.foreach { c =>
        println(s"- ${c.name} | ${c.job} | ${c.department}")
      }
  }
}
```
Este código permite:

Leer un archivo JSON completo desde el disco y cargarlo en memoria.

Decodificar (Parsear) automáticamente el texto JSON a una lista de objetos Scala (List[CrewMember]).

Manejar errores de forma segura, separando el éxito (Right) del fallo (Left) al leer el archivo.

Filtrar y limpiar datos, descartando los registros que no tengan nombre o trabajo definido.

Visualizar la información final procesada y limpia en la consola.


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





