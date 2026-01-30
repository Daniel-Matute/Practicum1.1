# ProyectoIntegrador_PFR
Repositorio del proyecto integrador - Programación funcional y reactica

  -Desarrollar los siguiente ítems


  -Tablas de datos (nombre de columna, tipo, propósito y observaciones) - README.md
# Diccionario de Datos

## 5.1 Tablas de Datos
Descripción detallada de las columnas, tipos de datos, propósitos y observaciones del esquema de base de datos.

### 🎬 Entidad: MOVIES (Principal)
| Columna | Tipo de Dato | Propósito | Observaciones |
| :--- | :--- | :--- | :--- |
| **id** | Numérico (PK) | Identificador único de la película | Clave primaria |
| **imdb_id** | Texto | Identificador de la película en IMDb | Puede contener valores nulos |
| **title** | Texto | Título comercial de la película | Usado para visualización |
| **original_title** | Texto | Título en su idioma original | Puede coincidir con “title” |
| **overview** | Texto largo | Descripción general de la trama | Texto libre |
| **tagline** | Texto | Frase promocional | Muchos valores nulos |
| **status** | Texto | Estado de la película (Released, etc.) | Categoría controlada |
| **release_date** | Fecha | Fecha de estreno | Útil para análisis temporal |
| **runtime** | Numérico | Duración de la película en minutos | Puede presentar valores atípicos |
| **budget** | Numérico | Presupuesto de producción | Alta presencia de valores nulos |
| **revenue** | Numérico | Recaudación obtenida | Valores nulos frecuentes |
| **popularity** | Numérico | Índice de popularidad | Métrica relativa |
| **vote_average** | Numérico | Promedio de votaciones | Escala normalizada |
| **vote_count** | Numérico | Número total de votos | Relacionado con popularidad |
| **adult** | Booleano | Indica si es contenido para adultos | Verdadero/Falso |
| **video** | Booleano | Indica si es contenido en formato video | Poco frecuente |
| **original_language**| Texto | Idioma original de la película | Código ISO |
| **homepage** | Texto | Página web oficial | Mayormente nulos |
| **poster_path** | Texto | Ruta de la imagen promocional | Uso visual |

### 🏢 Entidad: PRODUCTION_COMPANIES
| Columna | Tipo | Propósito | Observaciones |
| :--- | :--- | :--- | :--- |
| **company_id** | Numérico (PK) | Identificador de la productora | Clave primaria |
| **company_name** | Texto | Nombre de la productora | Texto descriptivo |

### 🌍 Entidad: PRODUCTION_COUNTRIES
| Columna | Tipo | Propósito | Observaciones |
| :--- | :--- | :--- | :--- |
| **country_code** | Texto | Código del país | Basado en estándar internacional |
| **country_name** | Texto | Nombre del país | Información descriptiva |

### 🗣️ Entidad: LANGUAGES
| Columna | Tipo | Propósito | Observaciones |
| :--- | :--- | :--- | :--- |
| **language_code** | Texto | Código del idioma | Formato ISO 639-1 |
| **language_name** | Texto | Nombre del idioma | Texto descriptivo |

### 🎭 Entidad: GENRES
| Columna | Tipo | Propósito | Observaciones |
| :--- | :--- | :--- | :--- |
| **genre_id** | Numérico (PK) | Identificador del género | Clave primaria |
| **genre_name** | Texto | Nombre del género | Categoría temática |

### 🏷️ Entidad: KEYWORDS
| Columna | Tipo | Propósito | Observaciones |
| :--- | :--- | :--- | :--- |
| **keyword_id** | Numérico (PK) | Identificador de la palabra clave | Clave primaria |
| **keyword_name** | Texto | Palabra clave asociada a la película | Texto descriptivo |

### 👥 Entidad: CAST
| Columna | Tipo | Propósito | Observaciones |
| :--- | :--- | :--- | :--- |
| **cast_id** | Numérico (PK) | Identificador del actor | Clave primaria |
| **actor_name** | Texto | Nombre del actor | Texto libre |
| **character_name** | Texto | Personaje interpretado | Puede repetirse |
| **gender** | Numérico | Género del actor | Codificación interna |

### 🎥 Entidad: CREW
| Columna | Tipo | Propósito | Observaciones |
| :--- | :--- | :--- | :--- |
| **crew_id** | Numérico (PK) | Identificador del miembro del equipo | Clave primaria |
| **crew_name** | Texto | Nombre del miembro del equipo | Texto libre |
| **job** | Texto | Rol desempeñado | Ej. Director, Producer |
| **department** | Texto | Área de trabajo | Categoría descriptiva |

### 📦 Entidad: BELONGS_TO_COLLECTION
| Columna | Tipo | Propósito | Observaciones |
| :--- | :--- | :--- | :--- |
| **collection_id** | Numérico (PK) | Identificador de la colección | Clave primaria |
| **collection_name** | Texto | Nombre de la colección | Puede ser nulo |
| **poster_path** | Texto | Imagen de la colección | Uso visual |
| **backdrop_path** | Texto | Imagen de fondo | Opcional |

### ⭐ Entidad: RATINGS
| Columna | Tipo | Propósito | Observaciones |
| :--- | :--- | :--- | :--- |
| **rating_id** | Numérico (PK) | Identificador del rating | Clave primaria |
| **rating_source** | Texto | Fuente de la calificación | Ej. usuarios |
| **rating_value** | Numérico | Valor del rating | Escala numérica |



 5.2 Lectura de columnas numéricas
```scala
package Avance1



import cats.effect.{IO, IOApp}
import fs2.io.file.{Files, Path}
import fs2.text

object NumericAnalysis extends IOApp.Simple {

  val inputPath =
    Path("C:\\Programacion Funcional y Reactiva\\proyectopracticum\\src\\main\\resources\\data\\pi_movies_complete.csv")

  val delimiter = ";"

  val numericColumns = Map(
    "budget"       -> 2,
    "popularity"   -> 10,
    "revenue"      -> 15,
    "runtime"      -> 16,
    "vote_average" -> 22,
    "vote_count"   -> 23
  )

  def mean(xs: List[Double]): Double =
    if xs.isEmpty then 0.0 else xs.sum / xs.size

  def std(xs: List[Double]): Double = {
    val m = mean(xs)
    math.sqrt(xs.map(x => math.pow(x - m, 2)).sum / xs.size)
  }

  def run: IO[Unit] =
    Files[IO]
      .readAll(inputPath)
      .through(text.utf8.decode)
      .through(text.lines)
      .drop(1)
      .compile
      .toList
      .flatMap { lines =>

        val rows = lines.map(_.split(";", -1).toVector)

        IO {
          println(s"Filas leídas: ${rows.size}")
          println(s"Columnas detectadas: ${rows.headOption.map(_.length)}")

          numericColumns.foreach { case (name, idx) =>
            val values =
              rows.flatMap(_.lift(idx).flatMap(_.toDoubleOption))

            println(
              s"""
                 |Columna: $name
                 |Count: ${values.size}
                 |Mean: ${mean(values)}
                 |Min: ${values.minOption.getOrElse(0.0)}
                 |Max: ${values.maxOption.getOrElse(0.0)}
                 |Std: ${std(values)}
                 |""".stripMargin
            )
          }
        }
      }
}
```


5.3 Análisis de datos en columnas numéricas (estadísticas básicas)


<img width="284" height="682" alt="image" src="https://github.com/user-attachments/assets/0d484fd4-446e-4c2b-a5d6-4b2bf61f0562" />

<img width="265" height="681" alt="image" src="https://github.com/user-attachments/assets/dfb14e0f-fb90-4541-b812-64e17221bcc6" />



Al examinar las estadísticas descriptivas de las columnas numéricas de mi dataset, he identificado varios patrones clave sobre la naturaleza de los datos:

En cuanto a las métricas financieras, existe una enorme dispersión. El presupuesto (budget) promedio es de aproximadamente 7.3 millones de dólares, pero la desviación estándar (24.6 millones) es muy superior a la media, lo que me indica una gran brecha entre producciones de bajo costo y grandes superproducciones. Lo mismo ocurre con los ingresos (revenue), donde el máximo alcanza los 1.1 billones, evidenciando el éxito masivo de ciertos "blockbusters" frente al resto del catálogo.

Respecto a la recepción de la audiencia, la calificación promedio (vote_average) se sitúa en 5.67 sobre 10, lo que sugiere un catálogo de calidad media-regular. Además, la columna vote_count muestra que, aunque el promedio de votos es bajo (228), hay películas con hasta 14,075 votos, confirmando que la atención del público se concentra en unos pocos títulos muy populares.

Finalmente, he detectado una anomalía crítica en la variable runtime. El valor máximo (4.22E8) y una media de 285,464 son inverosímiles para representar la duración en minutos. Esto señala claramente la presencia de "datos sucios" o valores atípicos extremos (posiblemente en milisegundos o errores de captura) que deberé tratar en la fase de limpieza para no sesgar el análisis temporal.

5.4 Análisis de datos en columnas tipo texto (algunas col. - distribución de frecuencia). ja 
  

# Análisis de Variables de Texto (No Estructuradas)

El dataset pi_movies_complete incluye diversas columnas de tipo texto que permiten describir características generales y contextuales de las películas. Estas variables no se utilizan para cálculos estadísticos directos, pero resultan fundamentales para el análisis descriptivo, la segmentación de datos y la identificación de patrones cualitativos.
Para este análisis se consideran únicamente columnas de texto simples y se excluyen aquellas que almacenan información estructurada en formato JSON, como géneros, reparto, países de producción o palabras clave.
Las principales columnas de tipo texto analizadas son las siguientes:
-	Idioma original
original_language
Esta columna indica el idioma original de la película, representado mediante códigos normalizados ISO 639-1. 
No obstante, también se identifican películas en otros idiomas como francés, alemán, italiano, japonés y español, lo que evidencia una diversidad lingüística.
-	Fechas de estreno
release_date 
La columna de fechas de estreno corresponde a un atributo textual que posteriormente puede transformarse a tipo fecha. 
Se observa un aumento progresivo en la cantidad de registros a partir de la segunda mitad del siglo XX, con una mayor concentración en las últimas décadas.
-	Estado de la película
status
Esta variable categórica indica el estado de producción de la película. La distribución de frecuencia evidencia que la gran mayoría de los registros se encuentran en estado Released, mientras que una proporción menor corresponde a estados como Post Production o In Production.
-	Clasificación de contenido
adult
Aunque se trata de una variable booleana, su análisis se realiza como una categoría textual. La distribución muestra que la gran mayoría de las películas no están clasificadas como contenido para adultos, mientras que solo un porcentaje muy reducido pertenece a esta categoría.
-	Títulos de la película
title-original_title
Ambas columnas corresponden a atributos nominales utilizados para identificar la película.
-	Descripción y elementos promocionales
overview-tagline
Estas columnas almacenan texto libre de longitud variable.
-	overview contiene una descripción general de la trama.
-	tagline corresponde a una frase promocional asociada a la película.
Recursos visuales y enlaces
•	poster_path
•	homepage
Estas columnas contienen rutas o enlaces asociados a material visual o informativo.
-	poster_path almacena la ruta de la imagen promocional.
-	homepage indica la existencia de un sitio web oficial.
Indicador de formato
-	video
Aunque es una variable booleana, se trata como categórica textual. Su frecuencia indica si una película fue distribuida principalmente en formato video.

5.4 Análisis de datos en columnas tipo texto (algunas col. - distribución de frecuencia) 

```scala
package Avance1


import cats.effect.{IO, IOApp}
import cats.implicits.*
import fs2.text
import fs2.io.file.{Files, Path}

object TextAnalysis extends IOApp.Simple {

  val inputPath =
    Path("C:\\Programacion Funcional y Reactiva\\proyectopracticum\\src\\main\\resources\\data\\pi_movies_complete.csv")


  val textColumns = Map(
    "original_language" -> 7,
    "status"            -> 18,
    "title"             -> 20,
    "tagline"           -> 19,
    "adult"             -> 0
  )

  def run: IO[Unit] =
    Files[IO]
      .readAll(inputPath)
      .through(text.utf8.decode)
      .through(text.lines)
      .drop(1)
      .compile
      .toList
      .flatMap { lines =>

        val rows = lines.map(_.split(";", -1))

        textColumns.toList.traverse_ { case (name, idx) =>

          val values =
            rows.flatMap(_.lift(idx))
              .map(_.trim)
              .filter(_.nonEmpty)

          val total = rows.size
          val empty = total - values.size

          val freq =
            values
              .groupBy(identity)
              .view
              .mapValues(_.size)
              .toList
              .sortBy(-_._2)
              .take(10)

          IO.println(
            s"""
               |Columna: $name
               |Total filas: $total
               |Vacíos: $empty
               |Top 10 valores:
               |${freq.map { case (v, c) => s"$v -> $c" }.mkString("\n")}
               |""".stripMargin
          )
        }
      }
}
```

<img width="338" height="467" alt="image" src="https://github.com/user-attachments/assets/4cdc4110-53f0-411a-a41a-fa9c52119a01" />

<img width="886" height="441" alt="image" src="https://github.com/user-attachments/assets/9415b6ff-5365-418f-9f9b-504fbf4f842c" />

<img width="886" height="221" alt="image" src="https://github.com/user-attachments/assets/49fc6762-21c1-47fc-af60-20ea65938591" />

<img width="484" height="477" alt="image" src="https://github.com/user-attachments/assets/2dc2b39f-7750-45ff-9861-9818c9d040ec" />

<img width="886" height="425" alt="image" src="https://github.com/user-attachments/assets/cdde6ce5-6190-4437-a744-32e4dcdad5e6" />

Tras aplicar la detección de outliers, he confirmado que el ruido afectaba severamente mis métricas. El caso más evidente es runtime, donde al eliminar solo 9 valores extremos, el promedio se desplomó de ~285,000 a ~10,000, aunque esta cifra sigue siendo inusualmente alta, lo que me sugiere que aún debo revisar la unidad de medida (posiblemente milisegundos).

Por otro lado, identifiqué que el éxito financiero y la popularidad son excepciones y no la regla: encontré 66 outliers en revenue y 71 en vote_count, lo que inflaba las medias artificialmente. En contraste, vote_average demostró ser el dato más robusto y consistente, ya que no presentó ningún valor atípico, manteniendo su promedio fijo en 5.67.

5.5 Limpieza de datos (columnas con valores nulos, valores atípicos, etc.)

```scala
package Avance1



import cats.effect.{IO, IOApp}
import cats.implicits.*
import fs2.text
import fs2.io.file.{Files, Path}

object DataCleaning extends IOApp.Simple {

  val inputPath =
    Path("C:\\Programacion Funcional y Reactiva\\proyectopracticum\\src\\main\\resources\\data\\pi_movies_complete.csv")

  val numericColumns = Map(
    "budget"       -> 2,
    "popularity"   -> 10,
    "revenue"      -> 15,
    "runtime"      -> 16,
    "vote_average" -> 22,
    "vote_count"   -> 23
  )

  def mean(xs: List[Double]): Double =
    if xs.isEmpty then 0.0 else xs.sum / xs.size

  def std(xs: List[Double]): Double = {
    val m = mean(xs)
    math.sqrt(xs.map(x => math.pow(x - m, 2)).sum / xs.size)
  }

  def isValid(name: String, v: Double): Boolean = name match {
    case "budget" | "runtime" => v > 0
    case "vote_average"       => v >= 0 && v <= 10
    case _                    => v >= 0
  }

  def run: IO[Unit] =
    Files[IO]
      .readAll(inputPath)
      .through(text.utf8.decode)
      .through(text.lines)
      .drop(1)
      .compile
      .toList
      .flatMap { lines =>

        val rows = lines.map(_.split(";", -1))

        numericColumns.toList.traverse_ { case (name, idx) =>

          val raw =
            rows.flatMap(_.lift(idx))
              .flatMap(_.toDoubleOption)
              .filter(v => isValid(name, v))

          val m = mean(raw)
          val s = std(raw)

          val cleaned =
            raw.filter(v => math.abs(v - m) <= 3 * s)

          IO.println(
            s"""
               |Columna: $name
               |Valores válidos: ${raw.size}
               |Outliers detectados: ${raw.size - cleaned.size}
               |Media limpia: ${mean(cleaned)}
               |Std limpia: ${std(cleaned)}
               |""".stripMargin
          )
        }
      }
}
```

<img width="434" height="605" alt="image" src="https://github.com/user-attachments/assets/c9e0d360-f704-4a9a-b0a8-31cee534bb2b" />

<img width="399" height="606" alt="image" src="https://github.com/user-attachments/assets/28f5eee5-7ef7-453f-8c21-7d2f6b97363d" />



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


```scala
import io.circe._
import io.circe.generic.auto._
import io.circe.parser._

object EjemploCrew extends App {

  // 1. Definimos la estructura (Case Class)
  // Los nombres deben ser iguales al JSON: id, job, department, name
  case class MiembroEquipo(id: Int, job: String, department: String, name: String)

  // 2. Tu JSON (con el dato vacío al final)
  val jsonRaw = """
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
  """

  println("--- 1. Decodificación Básica ---")

  // Convertimos el String a una Lista de objetos Scala
  val resultado = decode[List[MiembroEquipo]](jsonRaw)

  resultado match {
    case Left(error) =>
      println(s"Error: $error")

    case Right(lista) =>
      // Aquí ya tenemos la lista en Scala.
      // Vamos a recorrerla e imprimir solo los que tienen nombre (filtramos el vacío)

      println(s"Total de registros encontrados: ${lista.size}")

      println("\n--- 2. Datos Limpios (Sin vacíos) ---")

      val equipoValido = lista.filter(persona => persona.name.nonEmpty)

      equipoValido.foreach { persona =>
        println(s"Rol: ${persona.job} | Nombre: ${persona.name}")
      }

      // Ejemplo extra: Buscar al Director
      println("\n--- 3. Buscar al Director ---")
      lista.find(_.job == "Director").foreach { director =>
        println(s"El director es: ${director.name}")
      }
  }
}
```

<img width="1847" height="1043" alt="image" src="https://github.com/user-attachments/assets/b4339181-a029-4246-8308-a1f4c115db9e" />

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



```scala
package Avance3

import cats.effect.{IO, IOApp}
import fs2.Stream
import fs2.io.file.{Files, Path}
import fs2.text
import fs2.data.csv.*
import fs2.data.csv.lenient.*
import models.{CollectionJson, PeliculaInput, PeliculaLimpia}
import io.circe.parser.parse

import java.time.LocalDate
import scala.util.Try

// Objeto interno para reemplazar el package utilities
object ReglasSanitizacion {
  def limpiarTexto(s: String): String = {
    if (s == null) ""
    else s.trim.replaceAll("[\r\n]+", " ").replaceAll("\"", "'").trim
  }

  def validarBool(s: String): Option[Boolean] = s.trim.toLowerCase match {
    case "true" => Some(true)
    case "false" => Some(false)
    case _ => None
  }

  def validarDouble(s: String): Option[Double] = s.trim.toDoubleOption.filter(_ >= 0)

  def validarUrl(opt: Option[String]): Option[String] = opt.map(_.trim).filter(_.startsWith("http"))

  def validarEnteroId(s: String): Option[Int] = {
    val limpio = s.trim.takeWhile(_ != '.')
    limpio.toIntOption.filter(_ > 0)
  }

  def validarImdb(opt: Option[String]): Option[String] = opt.map(_.trim).filter(_.startsWith("tt"))

  def validarLenguaje(s: String): Option[String] = {
    val t = s.trim
    if (t.matches("^[a-zA-Z]{2}$")) Some(t) else None
  }

  def validarEstado(opt: Option[String]): Option[String] = {
    val ok = Set("In Production", "Post Production", "Released", "Rumored")
    opt.map(_.trim).flatMap { s =>
      if (ok.contains(s)) Some(s)
      else if (s == "In Produ") Some("In Production")
      else if (s == "Post Pro") Some("Post Production")
      else None
    }
  }

  def validarRuta(opt: Option[String]): Option[String] = opt.map(_.trim).filter(_.startsWith("/"))

  def validarFecha(opt: Option[String]): Option[String] = opt.map(_.trim).filter(_.matches("^\\d{4}-\\d{2}-\\d{2}$"))

  // Pequeño helper para JSON simple de colecciones aquí mismo
  def extraerIdColeccion(jsonRaw: String): Option[Int] = {
    val limpio = jsonRaw.replace("'", "\"").replaceAll("None", "null")
    parse(limpio).flatMap(_.as[CollectionJson]).map(_.id).toOption
  }
}

object ProcesamientoBase extends IOApp.Simple {


  val rutaEntrada = Path("C:\\Programacion Funcional y Reactiva\\proyectopracticum\\src\\main\\resources\\data\\pi_movies_complete.csv")
  val rutaSalida = Path("C:\\Programacion Funcional y Reactiva\\proyectopracticum\\src\\main\\resources\\data\\entidad_peliculas.csv")
  def procesarFila(raw: PeliculaInput): Option[PeliculaLimpia] = {
    val idCheck = ReglasSanitizacion.validarEnteroId(raw.id)

    idCheck.map { id =>
      val colId = raw.belongs_to_collection.flatMap(ReglasSanitizacion.extraerIdColeccion)

      PeliculaLimpia(
        adult             = ReglasSanitizacion.validarBool(raw.adult),
        budget            = ReglasSanitizacion.validarDouble(raw.budget),
        homepage          = ReglasSanitizacion.validarUrl(raw.homepage),
        id                = id,
        imdb_id           = ReglasSanitizacion.validarImdb(raw.imdb_id),
        original_language = ReglasSanitizacion.validarLenguaje(raw.original_language),
        original_title    = ReglasSanitizacion.limpiarTexto(if(raw.original_title.trim.nonEmpty) raw.original_title else "Sin Titulo"),
        overview          = raw.overview.map(ReglasSanitizacion.limpiarTexto).filter(_.nonEmpty),
        popularity        = ReglasSanitizacion.validarDouble(raw.popularity),
        poster_path       = ReglasSanitizacion.validarRuta(raw.poster_path),
        release_date      = ReglasSanitizacion.validarFecha(raw.release_date),
        revenue           = ReglasSanitizacion.validarDouble(raw.revenue),
        runtime           = raw.runtime.flatMap(ReglasSanitizacion.validarDouble),
        status            = ReglasSanitizacion.validarEstado(raw.status),
        tagline           = raw.tagline.map(ReglasSanitizacion.limpiarTexto).filter(_.nonEmpty),
        title             = ReglasSanitizacion.limpiarTexto(if(raw.title.trim.nonEmpty) raw.title else "Sin Titulo"),
        video             = ReglasSanitizacion.validarBool(raw.video),
        vote_average      = ReglasSanitizacion.validarDouble(raw.vote_average),
        vote_count        = ReglasSanitizacion.validarDouble(raw.vote_count),
        collection_id     = colId
      )
    }
  }

  val run: IO[Unit] = {
    IO.println("--- INICIANDO PROCESO DE DEPURACIÓN DE DATOS ---")

    val flujoLectura = Files[IO].readAll(rutaEntrada)
      .through(text.utf8.decode)
      .through(attemptDecodeUsingHeaders[PeliculaInput](';'))
      .collect { case Right(p) => p }

    flujoLectura.compile.toList.flatMap { listaBruta =>
      println(s"-> Registros leídos: ${listaBruta.size}")

      val validas = listaBruta.flatMap(procesarFila)

      // Deduplicar
      val unicas = validas
        .groupBy(_.id)
        .map { case (_, items) => items.head }
        .toList
        .sortBy(_.id)

      println(s"-> Registros válidos y únicos: ${unicas.size}")

      if (unicas.nonEmpty) {
        Stream.emits(unicas)
          .covary[IO]
          .through(encodeUsingFirstHeaders(fullRows = true))
          .through(text.utf8.encode)
          .through(Files[IO].writeAll(rutaSalida))
          .compile.drain *> IO.println(s"-> Archivo generado exitosamente en: $rutaSalida")
      } else {
        IO.println("-> ALERTA: No se generaron datos.")
      }
    }
  }
}

```

En ProcesamientoBase.scala establecemos el estándar de calidad de los datos. El proceso funciona como un embudo de tres niveles:

Nivel Sintáctico: Usamos expresiones regulares para limpiar texto basura y corregir formatos de fecha y booleanos.

Nivel Lógico: Aplicamos una regla de negocio estricta: 'Sin ID, no hay película'. Cualquier registro que no tenga un identificador numérico válido es descartado inmediatamente.

Nivel de Integridad: Antes de guardar, eliminamos duplicados agrupando por ID, asegurando que el archivo resultante entidad_peliculas.csv sea 100% compatible con la clave primaria de nuestra base de datos SQL.".


```scala
package Avance3

import cats.effect.{IO, IOApp}
import fs2.Stream
import fs2.io.file.{Files, Path}
import fs2.text
import fs2.data.csv._
import fs2.data.csv.lenient._
import models._
import io.circe.parser._
import io.circe.Decoder

// Lógica de limpieza JSON integrada aquí para no usar 'utilities'
object ReparadorJson {
  def leerLista[T](raw: String)(implicit d: Decoder[List[T]]): List[T] = {
    val safe = arreglarFormato(raw, esLista = true)
    parse(safe).flatMap(_.as[List[T]]).getOrElse(Nil)
  }

  def leerObjeto[T](raw: String)(implicit d: Decoder[T]): Option[T] = {
    val safe = arreglarFormato(raw, esLista = false)
    parse(safe).flatMap(_.as[T]).toOption
  }

  private def arreglarFormato(raw: String, esLista: Boolean): String = {
    var txt = raw.trim
    if (txt.isEmpty || txt == "[]" || txt == "{}") return if (esLista) "[]" else "{}"

    txt = txt.replaceAll("None", "null")
      .replaceAll("True", "true")
      .replaceAll("False", "false")
      .replace("'", "\"")

    if (esLista && txt.startsWith("[") && !txt.endsWith("]")) {
      val cierre = txt.lastIndexOf("}")
      if (cierre != -1) txt = txt.substring(0, cierre + 1) + "]"
      else txt = "[]"
    }
    txt
  }
}

object ExtractorRelaciones extends IOApp.Simple {

  val origen = Path("C:\\Programacion Funcional y Reactiva\\proyectopracticum\\src\\main\\resources\\data\\pi_movies_complete.csv")
  // Guardamos directo en data
  val carpetaDestino = "C:\\Programacion Funcional y Reactiva\\proyectopracticum\\src\\main\\resources\\data"

  def exportarCsv[T](lista: List[T], nombre: String)(implicit enc: CsvRowEncoder[T, String]): IO[Unit] = {
    if (lista.isEmpty) IO.println(s"Aviso: $nombre no tiene datos.")
    else {
      val p = Path(carpetaDestino) / nombre
      Stream.emits(lista).covary[IO]
        .through(encodeUsingFirstHeaders(fullRows = true))
        .through(text.utf8.encode)
        .through(Files[IO].writeAll(p))
        .compile.drain *> IO.println(s"  [OK] Exportado: $nombre (${lista.size} filas)")
    }
  }

  // Helpers de deduplicación
  def unicosPorInt[T](l: List[T])(fn: T => Int): List[T] = l.groupBy(fn).map(_._2.head).toList.sortBy(fn)
  def unicosPorStr[T](l: List[T])(fn: T => String): List[T] = l.groupBy(fn).map(_._2.head).toList.sortBy(fn)

  val run: IO[Unit] = {
    println("--- COMIENZO DE EXTRACCIÓN Y NORMALIZACIÓN DE ENTIDADES ---")

    val streamLectura = Files[IO].readAll(origen)
      .through(text.utf8.decode)
      .through(attemptDecodeUsingHeaders[PeliculaInput](';'))
      .collect { case Right(x) => x }

    streamLectura.compile.toList.flatMap { crudos =>

      // Filtro inicial de IDs
      val validos = crudos.filter(_.id.trim.toIntOption.exists(_ > 0))
      val peliculas = validos.groupBy(_.id.trim.toInt).map(_._2.head).toList.sortBy(_.id.trim.toInt)

      println(s"Procesando ${peliculas.size} películas únicas...")

      // 1. GENEROS
      val dataGeneros = peliculas.flatMap { p =>
        val pid = p.id.trim.toInt
        ReparadorJson.leerLista[GenreJson](p.genres).map { g =>
          (GenreEntity(g.id, g.name), MovieGenreLink(pid, g.id))
        }
      }
      val generosUnicos = unicosPorInt(dataGeneros.map(_._1))(_.genre_id)
      val linksGeneros = dataGeneros.map(_._2)

      // 2. COMPAÑIAS
      val dataComps = peliculas.flatMap { p =>
        val pid = p.id.trim.toInt
        ReparadorJson.leerLista[CompanyJson](p.production_companies).map { c =>
          (CompanyEntity(c.id, c.name), MovieCompanyLink(pid, c.id))
        }
      }
      val compUnicas = unicosPorInt(dataComps.map(_._1))(_.company_id)
      val linksComps = dataComps.map(_._2)

      // 3. PAISES
      val dataPaises = peliculas.flatMap { p =>
        val pid = p.id.trim.toInt
        ReparadorJson.leerLista[CountryJson](p.production_countries).map { c =>
          (CountryEntity(c.iso_3166_1, c.name), MovieCountryLink(pid, c.iso_3166_1))
        }
      }
      val paisesUnicos = unicosPorStr(dataPaises.map(_._1))(_.iso_3166_1)
      val linksPaises = dataPaises.map(_._2)

      // 4. IDIOMAS
      val dataIdiomas = peliculas.flatMap { p =>
        val pid = p.id.trim.toInt
        ReparadorJson.leerLista[LanguageJson](p.spoken_languages).map { l =>
          (LanguageEntity(l.iso_639_1, l.name), MovieLanguageLink(pid, l.iso_639_1))
        }
      }
      val idiomasUnicos = unicosPorStr(dataIdiomas.map(_._1))(_.iso_639_1)
      val linksIdiomas = dataIdiomas.map(_._2)

      // 5. KEYWORDS
      val dataKeys = peliculas.flatMap { p =>
        val pid = p.id.trim.toInt
        ReparadorJson.leerLista[KeywordJson](p.keywords).map { k =>
          (KeywordEntity(k.id, k.name), MovieKeywordLink(pid, k.id))
        }
      }
      val keysUnicas = unicosPorInt(dataKeys.map(_._1))(_.keyword_id)
      val linksKeys = dataKeys.map(_._2)

      // 6. COLECCIONES
      val dataCol = peliculas.flatMap { p =>
        p.belongs_to_collection.flatMap(r => ReparadorJson.leerObjeto[CollectionJson](r)).map { c =>
          CollectionEntity(c.id, c.name, c.poster_path.getOrElse(""), c.backdrop_path.getOrElse(""))
        }
      }
      val colUnicas = unicosPorInt(dataCol)(_.collection_id)

      // 7. PERSONAS (CAST & CREW)
      val dataCast = peliculas.flatMap { p =>
        val pid = p.id.trim.toInt
        ReparadorJson.leerLista[CastJson](p.cast).map { c =>
          (PersonEntity(c.id, c.name, c.gender.getOrElse(0), c.profile_path.getOrElse("")),
            MovieCastLink(pid, c.id, c.character, c.order, c.credit_id, c.cast_id))
        }
      }
      val dataCrew = peliculas.flatMap { p =>
        val pid = p.id.trim.toInt
        ReparadorJson.leerLista[CrewJson](p.crew).map { c =>
          (PersonEntity(c.id, c.name, c.gender.getOrElse(0), c.profile_path.getOrElse("")),
            MovieCrewLink(pid, c.id, c.department, c.job, c.credit_id))
        }
      }
      val personasUnicas = unicosPorInt((dataCast.map(_._1) ++ dataCrew.map(_._1)))(_.person_id)
      val linksCast = dataCast.map(_._2)
      val linksCrew = dataCrew.map(_._2)

      // 8. USUARIOS Y RATINGS
      val dataRatings = peliculas.flatMap { p =>
        val pid = p.id.trim.toInt
        ReparadorJson.leerLista[RatingJson](p.ratings).map { r =>
          (UserEntity(r.userId), RatingLink(pid, r.userId, r.rating, r.timestamp))
        }
      }
      val usuariosUnicos = dataRatings.map(_._1).distinct
      val linksRatings = dataRatings.map(_._2)

      println(s"Guardando archivos CSV en: $carpetaDestino")

      for {
        // === ENTIDADES (entidad_nombre.csv) ===
        _ <- exportarCsv(colUnicas,   "entidad_colecciones.csv")
        _ <- exportarCsv(compUnicas,  "entidad_companias.csv")
        _ <- exportarCsv(paisesUnicos,"entidad_paises.csv")
        _ <- exportarCsv(generosUnicos, "entidad_generos.csv")
        _ <- exportarCsv(keysUnicas,    "entidad_keywords.csv")
        _ <- exportarCsv(idiomasUnicos, "entidad_idiomas.csv")
        _ <- exportarCsv(personasUnicas,"entidad_personas.csv")
        _ <- exportarCsv(usuariosUnicos,"entidad_usuarios.csv")

        // === RELACIONES (relacion_nombre1_nombre2.csv) ===
        _ <- exportarCsv(linksCast,    "relacion_pelicula_elenco.csv")
        _ <- exportarCsv(linksComps,   "relacion_pelicula_compania.csv")
        _ <- exportarCsv(linksPaises,  "relacion_pelicula_pais.csv")
        _ <- exportarCsv(linksCrew,    "relacion_pelicula_staff.csv")
        _ <- exportarCsv(linksGeneros, "relacion_pelicula_genero.csv")
        _ <- exportarCsv(linksKeys,    "relacion_pelicula_keyword.csv")
        _ <- exportarCsv(linksIdiomas, "relacion_pelicula_idioma.csv")
        _ <- exportarCsv(linksRatings, "relacion_pelicula_rating.csv")

        _ <- IO.println("\n*** PROCESO TERMINADO CORRECTAMENTE ***")
      } yield ()
    }
  }
}
```


"El código se divide en tres fases lógicas:

Preparación: Leemos el archivo y usamos un 'Reparador' para corregir la sintaxis de los JSONs.

Extracción y Transformación: Recorremos las películas y usamos flatMap para desagregar las listas internas. Para cada elemento (como un actor o un género), generamos simultáneamente su registro de 'Entidad' y su registro de 'Relación'.

Consolidación: Deduplicamos las entidades para crear catálogos únicos (por ejemplo, que un actor aparezca una sola vez en la base de datos aunque actúe en 10 películas) y finalmente exportamos todo en 16 archivos CSV separados, logrando así la normalización completa del dataset."

### Paso 5: Presentación de resultados

Finalmente, se mostraron los resultados obtenidos después de la limpieza:

CREW
<img width="1917" height="1027" alt="image" src="https://github.com/user-attachments/assets/050099b4-f9a0-4e04-977e-f0e778006b6d" />


Limpiezza de datos

<img width="1600" height="899" alt="image" src="https://github.com/user-attachments/assets/726bed56-37e4-4ef3-8096-71e6a28ea6a8" />



### Avance 3
### Trabajo poblar Base de datos
### Opción 1: Usando Scripts SQL (generados desde Scala)
### Opción 2: Sentencias INSERT INTO a través de librería (Scala)

```scala
package Avance3

import cats.effect.{IO, IOApp}
import cats.syntax.all.*
import fs2.text
import fs2.Stream
import fs2.io.file.{Files, Path}
import fs2.data.csv.*
import fs2.data.csv.generic.semiauto.*
import doobie.*
import doobie.implicits.*
import models._

object MigracionSQL extends IOApp.Simple {

  // ===========================================
  // 1. CONFIGURACIÓN DE CONEXIÓN
  // ===========================================

  // Conexión Root para crear el Schema
  private val conRaiz = Transactor.fromDriverManager[IO](
    driver = "com.mysql.cj.jdbc.Driver",
    url = "jdbc:mysql://localhost:3306/",
    user = "root",
    password = "Suco2018@",
    logHandler = None
  )

  // Conexión a la Base de Datos específica
  val conApp = Transactor.fromDriverManager[IO](
    driver = "com.mysql.cj.jdbc.Driver",
    url = "jdbc:mysql://localhost:3306/MoviesDB",
    user = "root",
    password = "Suco2018@",
    logHandler = None
  )

  // ===========================================
  // 2. DEFINICIÓN DE ESQUEMA (DDL)
  // ===========================================
  private def inicializarTablas(): ConnectionIO[Unit] = {
    val sentencias = List(
      sql"""SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0""".update.run,
      sql"""SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0""".update.run,
      sql"""SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION'""".update.run,

      // Crear Base de Datos
      sql"""CREATE SCHEMA IF NOT EXISTS MoviesDB DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci""".update.run,
      sql"""USE MoviesDB""".update.run,

      // Tabla: Collection
      sql"""
      CREATE TABLE IF NOT EXISTS MoviesDB.Collection (
        collection_id INT NOT NULL,
        name VARCHAR(255) NOT NULL,
        poster_path VARCHAR(255) NOT NULL,
        backdrop_path VARCHAR(255) NOT NULL,
        PRIMARY KEY (collection_id)
      ) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci
      """.update.run,

      // Tabla: Movie
      sql"""
      CREATE TABLE IF NOT EXISTS MoviesDB.Movie (
        movie_id INT NOT NULL,
        title VARCHAR(255) NULL,
        original_title VARCHAR(255) NOT NULL,
        original_language VARCHAR(10) NULL DEFAULT NULL,
        overview TEXT NULL DEFAULT NULL,
        release_date DATE NULL DEFAULT NULL,
        runtime INT NOT NULL,
        budget DECIMAL(15,2) NULL DEFAULT 0,
        revenue DECIMAL(15,2) NULL DEFAULT 0,
        popularity DECIMAL(10,2) NULL DEFAULT 0.0,
        vote_average DECIMAL(3,1) NULL DEFAULT 0,
        vote_count INT NULL DEFAULT NULL,
        adult TINYINT(1) NOT NULL,
        video TINYINT(1) NULL DEFAULT NULL,
        status VARCHAR(50) NULL DEFAULT NULL,
        homepage VARCHAR(255) NULL DEFAULT NULL,
        imdb_id VARCHAR(20) NULL,
        poster_path VARCHAR(255) NULL DEFAULT NULL,
        tagline VARCHAR(255) NULL DEFAULT NULL,
        collection_id INT NULL,
        PRIMARY KEY (movie_id),
        CONSTRAINT fk_movie_collection1
          FOREIGN KEY (collection_id)
          REFERENCES MoviesDB.Collection (collection_id)
          ON DELETE NO ACTION
          ON UPDATE NO ACTION
      ) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci
      """.update.run,
      sql"""CREATE INDEX fk_movie_collection1_idx ON MoviesDB.Movie (collection_id ASC) VISIBLE""".update.run,

      // Tabla: Person
      sql"""
      CREATE TABLE IF NOT EXISTS MoviesDB.Person (
        person_id INT NOT NULL,
        name VARCHAR(255) NOT NULL,
        gender INT NOT NULL,
        profile_path VARCHAR(255) NULL DEFAULT NULL,
        PRIMARY KEY (person_id)
      ) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci
      """.update.run,

      // Tabla: Cast
      sql"""
      CREATE TABLE IF NOT EXISTS MoviesDB.Cast (
        movie_id INT NOT NULL,
        person_id INT NOT NULL,
        character_name VARCHAR(255) NOT NULL,
        cast_order INT NOT NULL,
        credit_id VARCHAR(50) NOT NULL,
        cast_id INT NOT NULL,
        PRIMARY KEY (movie_id, person_id, cast_id),
        CONSTRAINT cast_ibfk_1
          FOREIGN KEY (movie_id)
          REFERENCES MoviesDB.Movie (movie_id),
        CONSTRAINT cast_ibfk_2
          FOREIGN KEY (person_id)
          REFERENCES MoviesDB.Person (person_id)
      ) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci
      """.update.run,
      sql"""CREATE INDEX person_id ON MoviesDB.Cast (person_id ASC) VISIBLE""".update.run,

      // Tabla: Crew
      sql"""
      CREATE TABLE IF NOT EXISTS MoviesDB.Crew (
        movie_id INT NOT NULL,
        person_id INT NOT NULL,
        department VARCHAR(100) NOT NULL,
        job VARCHAR(100) NOT NULL,
        credit_id VARCHAR(50) NOT NULL,
        PRIMARY KEY (movie_id, person_id, job),
        CONSTRAINT crew_ibfk_1
          FOREIGN KEY (movie_id)
          REFERENCES MoviesDB.Movie (movie_id),
        CONSTRAINT crew_ibfk_2
          FOREIGN KEY (person_id)
          REFERENCES MoviesDB.Person (person_id)
      ) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci
      """.update.run,
      sql"""CREATE INDEX person_id ON MoviesDB.Crew (person_id ASC) VISIBLE""".update.run,

      // Tabla: Genre
      sql"""
      CREATE TABLE IF NOT EXISTS MoviesDB.Genre (
        genre_id INT NOT NULL,
        name VARCHAR(50) NOT NULL,
        PRIMARY KEY (genre_id)
      ) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci
      """.update.run,

      // Tabla: Keyword
      sql"""
      CREATE TABLE IF NOT EXISTS MoviesDB.Keyword (
        keyword_id INT NOT NULL,
        name VARCHAR(100) NOT NULL,
        PRIMARY KEY (keyword_id)
      ) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci
      """.update.run,

      // Tabla: SpokenLanguage
      sql"""
      CREATE TABLE IF NOT EXISTS MoviesDB.SpokenLanguage (
        iso_639_1 VARCHAR(10) NOT NULL,
        name VARCHAR(100) NOT NULL,
        PRIMARY KEY (iso_639_1)
      ) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci
      """.update.run,

      // Tabla: Movie_Genre
      sql"""
      CREATE TABLE IF NOT EXISTS MoviesDB.Movie_Genre (
        movie_id INT NOT NULL,
        genre_id INT NOT NULL,
        PRIMARY KEY (movie_id, genre_id),
        CONSTRAINT movie_genre_ibfk_1
          FOREIGN KEY (movie_id)
          REFERENCES MoviesDB.Movie (movie_id),
        CONSTRAINT movie_genre_ibfk_2
          FOREIGN KEY (genre_id)
          REFERENCES MoviesDB.Genre (genre_id)
      ) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci
      """.update.run,
      sql"""CREATE INDEX genre_id ON MoviesDB.Movie_Genre (genre_id ASC) VISIBLE""".update.run,

      // Tabla: Movie_Keyword
      sql"""
      CREATE TABLE IF NOT EXISTS MoviesDB.Movie_Keyword (
        movie_id INT NOT NULL,
        keyword_id INT NOT NULL,
        PRIMARY KEY (movie_id, keyword_id),
        CONSTRAINT movie_keyword_ibfk_1
          FOREIGN KEY (movie_id)
          REFERENCES MoviesDB.Movie (movie_id),
        CONSTRAINT movie_keyword_ibfk_2
          FOREIGN KEY (keyword_id)
          REFERENCES MoviesDB.Keyword (keyword_id)
      ) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci
      """.update.run,
      sql"""CREATE INDEX keyword_id ON MoviesDB.Movie_Keyword (keyword_id ASC) VISIBLE""".update.run,

      // Tabla: Movie_SpokenLanguage
      sql"""
      CREATE TABLE IF NOT EXISTS MoviesDB.Movie_SpokenLanguage (
        movie_id INT NOT NULL,
        language_code VARCHAR(10) NOT NULL,
        PRIMARY KEY (movie_id, language_code),
        CONSTRAINT movie_language_ibfk_1
          FOREIGN KEY (movie_id)
          REFERENCES MoviesDB.Movie (movie_id),
        CONSTRAINT movie_language_ibfk_2
          FOREIGN KEY (language_code)
          REFERENCES MoviesDB.SpokenLanguage (iso_639_1)
      ) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci
      """.update.run,
      sql"""CREATE INDEX language_code ON MoviesDB.Movie_SpokenLanguage (language_code ASC) VISIBLE""".update.run,

      // Tabla: ProductionCompany
      sql"""
      CREATE TABLE IF NOT EXISTS MoviesDB.ProductionCompany (
        company_id INT NOT NULL,
        name VARCHAR(255) NOT NULL,
        PRIMARY KEY (company_id)
      ) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci
      """.update.run,

      // Tabla: Movie_ProductionCompany
      sql"""
      CREATE TABLE IF NOT EXISTS MoviesDB.Movie_ProductionCompany (
        movie_id INT NOT NULL,
        company_id INT NOT NULL,
        PRIMARY KEY (movie_id, company_id),
        CONSTRAINT movie_productioncompany_ibfk_1
          FOREIGN KEY (movie_id)
          REFERENCES MoviesDB.Movie (movie_id),
        CONSTRAINT movie_productioncompany_ibfk_2
          FOREIGN KEY (company_id)
          REFERENCES MoviesDB.ProductionCompany (company_id)
      ) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci
      """.update.run,
      sql"""CREATE INDEX company_id ON MoviesDB.Movie_ProductionCompany (company_id ASC) VISIBLE""".update.run,

      // Tabla: ProductionCountry
      sql"""
      CREATE TABLE IF NOT EXISTS MoviesDB.ProductionCountry (
        iso_3166_1 CHAR(2) NOT NULL,
        name VARCHAR(100) NOT NULL,
        PRIMARY KEY (iso_3166_1)
      ) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci
      """.update.run,

      // Tabla: Movie_ProductionCountry
      sql"""
      CREATE TABLE IF NOT EXISTS MoviesDB.Movie_ProductionCountry (
        movie_id INT NOT NULL,
        country_code CHAR(2) NOT NULL,
        PRIMARY KEY (movie_id, country_code),
        CONSTRAINT movie_productioncountry_ibfk_1
          FOREIGN KEY (movie_id)
          REFERENCES MoviesDB.Movie (movie_id),
        CONSTRAINT movie_productioncountry_ibfk_2
          FOREIGN KEY (country_code)
          REFERENCES MoviesDB.ProductionCountry (iso_3166_1)
      ) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci
      """.update.run,
      sql"""CREATE INDEX country_code ON MoviesDB.Movie_ProductionCountry (country_code ASC) VISIBLE""".update.run,

      // Tabla: User
      sql"""
      CREATE TABLE IF NOT EXISTS MoviesDB.User (
        user_id INT NOT NULL,
        PRIMARY KEY (user_id)
      ) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci
      """.update.run,

      // Tabla: Rating
      sql"""
      CREATE TABLE IF NOT EXISTS MoviesDB.Rating (
        user_id INT NOT NULL,
        movie_id INT NOT NULL,
        rating DECIMAL(2,1) NOT NULL,
        timestamp BIGINT NOT NULL,
        PRIMARY KEY (user_id, movie_id),
        CONSTRAINT rating_ibfk_1
          FOREIGN KEY (user_id)
          REFERENCES MoviesDB.User (user_id),
        CONSTRAINT rating_ibfk_2
          FOREIGN KEY (movie_id)
          REFERENCES MoviesDB.Movie (movie_id)
      ) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci
      """.update.run,
      sql"""CREATE INDEX movie_id ON MoviesDB.Rating (movie_id ASC) VISIBLE""".update.run,

      sql"""SET SQL_MODE=@OLD_SQL_MODE""".update.run,
      sql"""SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS""".update.run,
      sql"""SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS""".update.run
    )

    sentencias.traverse_(identity)
  }

  // ===========================================
  // 3. FUNCIONES DE INSERCIÓN INDIVIDUAL
  // ===========================================

  // Película (Usando el nuevo modelo PeliculaLimpia)
  private def insertarPelicula(p: PeliculaLimpia): ConnectionIO[Int] =
    sql"""
      INSERT INTO Movie (
        movie_id, title, original_title, original_language, overview, release_date,
        runtime, budget, revenue, popularity, vote_average, vote_count,
        adult, video, status, homepage, imdb_id, poster_path, tagline, collection_id
      )
      VALUES (
        ${p.id}, ${p.title}, ${p.original_title}, ${Option(p.original_language).filterNot(_.isEmpty)},
        ${p.overview}, ${p.release_date.map(d => java.sql.Date.valueOf(d))},
        ${p.runtime.map(_.toInt).getOrElse(0)}, ${p.budget}, ${p.revenue}, ${p.popularity},
        ${p.vote_average}, ${p.vote_count}, ${p.adult}, ${p.video}, ${p.status},
        ${p.homepage}, ${p.imdb_id}, ${p.poster_path}, ${p.tagline}, ${p.collection_id}
      )
    """.update.run

  // Entidades Principales
  private def insertarGenero(g: GenreEntity): ConnectionIO[Int] =
    sql"INSERT INTO Genre (genre_id, name) VALUES (${g.genre_id}, ${g.name})".update.run

  private def insertarCompania(c: CompanyEntity): ConnectionIO[Int] =
    sql"INSERT INTO ProductionCompany (company_id, name) VALUES (${c.company_id}, ${c.name})".update.run

  private def insertarPais(c: CountryEntity): ConnectionIO[Int] =
    sql"INSERT INTO ProductionCountry (iso_3166_1, name) VALUES (${c.iso_3166_1}, ${c.name})".update.run

  private def insertarIdioma(l: LanguageEntity): ConnectionIO[Int] =
    sql"INSERT INTO SpokenLanguage (iso_639_1, name) VALUES (${l.iso_639_1}, ${l.name})".update.run

  private def insertarKeyword(k: KeywordEntity): ConnectionIO[Int] =
    sql"INSERT INTO Keyword (keyword_id, name) VALUES (${k.keyword_id}, ${k.name})".update.run

  private def insertarColeccion(c: CollectionEntity): ConnectionIO[Int] =
    sql"INSERT INTO Collection (collection_id, name, poster_path, backdrop_path) VALUES (${c.collection_id}, ${c.name}, ${c.poster}, ${c.backdrop})".update.run

  private def insertarPersona(p: PersonEntity): ConnectionIO[Int] =
    sql"INSERT INTO Person (person_id, name, gender, profile_path) VALUES (${p.person_id}, ${p.name}, ${p.gender}, ${p.profile_path})".update.run

  private def insertarUsuario(u: UserEntity): ConnectionIO[Int] =
    sql"INSERT INTO User (user_id) VALUES (${u.user_id})".update.run

  // Relaciones (Links)
  private def insertarLinkGenero(l: MovieGenreLink): ConnectionIO[Int] =
    sql"INSERT INTO Movie_Genre (movie_id, genre_id) VALUES (${l.movie_id}, ${l.genre_id})".update.run

  private def insertarLinkCompania(l: MovieCompanyLink): ConnectionIO[Int] =
    sql"INSERT INTO Movie_ProductionCompany (movie_id, company_id) VALUES (${l.movie_id}, ${l.company_id})".update.run

  private def insertarLinkPais(l: MovieCountryLink): ConnectionIO[Int] =
    sql"INSERT INTO Movie_ProductionCountry (movie_id, country_code) VALUES (${l.movie_id}, ${l.iso_3166_1})".update.run

  private def insertarLinkIdioma(l: MovieLanguageLink): ConnectionIO[Int] =
    sql"INSERT INTO Movie_SpokenLanguage (movie_id, language_code) VALUES (${l.movie_id}, ${l.iso_639_1})".update.run

  private def insertarLinkKeyword(l: MovieKeywordLink): ConnectionIO[Int] =
    sql"INSERT INTO Movie_Keyword (movie_id, keyword_id) VALUES (${l.movie_id}, ${l.keyword_id})".update.run

  private def insertarLinkElenco(l: MovieCastLink): ConnectionIO[Int] =
    sql"INSERT INTO Cast (movie_id, person_id, character_name, cast_order, credit_id, cast_id) VALUES (${l.movie_id}, ${l.person_id}, ${l.character_name}, ${l.order_cast}, ${l.credit_id}, ${l.cast_id})".update.run

  private def insertarLinkStaff(l: MovieCrewLink): ConnectionIO[Int] =
    sql"INSERT INTO Crew (movie_id, person_id, department, job, credit_id) VALUES (${l.movie_id}, ${l.person_id}, ${l.department}, ${l.job}, ${l.credit_id})".update.run

  private def insertarRating(r: RatingLink): ConnectionIO[Int] =
    sql"INSERT INTO Rating (user_id, movie_id, rating, timestamp) VALUES (${r.user_id}, ${r.movie_id}, ${r.rating}, ${r.timestamp})".update.run

  // ===========================================
  // 4. FUNCIONES DE PROCESAMIENTO EN LOTE (BATCH)
  // ===========================================
  // Reciben Listas y ejecutan en una sola transacción pequeña

  private def lotePeliculas(xs: List[PeliculaLimpia]): ConnectionIO[Int] = xs.traverse(insertarPelicula).map(_.sum)
  private def loteGeneros(xs: List[GenreEntity]): ConnectionIO[Int] = xs.traverse(insertarGenero).map(_.sum)
  private def loteCompanias(xs: List[CompanyEntity]): ConnectionIO[Int] = xs.traverse(insertarCompania).map(_.sum)
  private def lotePaises(xs: List[CountryEntity]): ConnectionIO[Int] = xs.traverse(insertarPais).map(_.sum)
  private def loteIdiomas(xs: List[LanguageEntity]): ConnectionIO[Int] = xs.traverse(insertarIdioma).map(_.sum)
  private def loteKeywords(xs: List[KeywordEntity]): ConnectionIO[Int] = xs.traverse(insertarKeyword).map(_.sum)
  private def loteColecciones(xs: List[CollectionEntity]): ConnectionIO[Int] = xs.traverse(insertarColeccion).map(_.sum)
  private def lotePersonas(xs: List[PersonEntity]): ConnectionIO[Int] = xs.traverse(insertarPersona).map(_.sum)
  private def loteUsuarios(xs: List[UserEntity]): ConnectionIO[Int] = xs.traverse(insertarUsuario).map(_.sum)

  // Lotes de Relaciones
  private def loteLinkGeneros(xs: List[MovieGenreLink]): ConnectionIO[Int] = xs.traverse(insertarLinkGenero).map(_.sum)
  private def loteLinkCompanias(xs: List[MovieCompanyLink]): ConnectionIO[Int] = xs.traverse(insertarLinkCompania).map(_.sum)
  private def loteLinkPaises(xs: List[MovieCountryLink]): ConnectionIO[Int] = xs.traverse(insertarLinkPais).map(_.sum)
  private def loteLinkIdiomas(xs: List[MovieLanguageLink]): ConnectionIO[Int] = xs.traverse(insertarLinkIdioma).map(_.sum)
  private def loteLinkKeywords(xs: List[MovieKeywordLink]): ConnectionIO[Int] = xs.traverse(insertarLinkKeyword).map(_.sum)
  private def loteLinkElenco(xs: List[MovieCastLink]): ConnectionIO[Int] = xs.traverse(insertarLinkElenco).map(_.sum)
  private def loteLinkStaff(xs: List[MovieCrewLink]): ConnectionIO[Int] = xs.traverse(insertarLinkStaff).map(_.sum)
  private def loteRatings(xs: List[RatingLink]): ConnectionIO[Int] = xs.traverse(insertarRating).map(_.sum)

  // ===========================================
  // 5. LECTURA DE CSV (Desde nueva ruta plana 'data/')
  // ===========================================
  def leerDatos[T](archivo: String)(using decoder: CsvRowDecoder[T, String]): IO[List[T]] = {
    // Apuntamos directo a data/, ya no existe 'normalized'
    val ruta = Path(s"src/main/resources/data/$archivo")
    Files[IO]
      .readAll(ruta)
      .through(text.utf8.decode)
      .through(decodeUsingHeaders[T](','))
      .compile
      .toList
  }

  // ===========================================
  // 6. FLUJO PRINCIPAL (MAIN)
  // ===========================================
  override def run: IO[Unit] = for {
    _ <- IO.println("\n" + "="*60)
    _ <- IO.println("      INICIANDO MIGRACIÓN DE DATOS A MYSQL")
    _ <- IO.println("="*60 + "\n")

    // 1. Crear Tablas
    _ <- IO.println("[1/4] Preparando esquema de base de datos...")
    _ <- inicializarTablas().transact(conRaiz)
    _ <- IO.println("      ✓ Tablas verificadas/creadas.")

    // 2. Cargar Entidades (Catálogos)
    _ <- IO.println("\n[2/4] Cargando catálogos maestros...")

    lGeneros <- leerDatos[GenreEntity]("entidad_generos.csv")
    _ <- Stream.emits(lGeneros).covary[IO].chunkN(1000).evalMap(c => loteGeneros(c.toList).transact(conApp)).compile.drain
    _ <- IO.println(s"      -> Géneros: ${lGeneros.size}")

    lCompanias <- leerDatos[CompanyEntity]("entidad_companias.csv")
    _ <- Stream.emits(lCompanias).covary[IO].chunkN(1000).evalMap(c => loteCompanias(c.toList).transact(conApp)).compile.drain
    _ <- IO.println(s"      -> Compañías: ${lCompanias.size}")

    lPaises <- leerDatos[CountryEntity]("entidad_paises.csv")
    _ <- Stream.emits(lPaises).covary[IO].chunkN(1000).evalMap(c => lotePaises(c.toList).transact(conApp)).compile.drain
    _ <- IO.println(s"      -> Países: ${lPaises.size}")

    lIdiomas <- leerDatos[LanguageEntity]("entidad_idiomas.csv")
    _ <- Stream.emits(lIdiomas).covary[IO].chunkN(1000).evalMap(c => loteIdiomas(c.toList).transact(conApp)).compile.drain
    _ <- IO.println(s"      -> Idiomas: ${lIdiomas.size}")

    lKeywords <- leerDatos[KeywordEntity]("entidad_keywords.csv")
    _ <- Stream.emits(lKeywords).covary[IO].chunkN(1000).evalMap(c => loteKeywords(c.toList).transact(conApp)).compile.drain
    _ <- IO.println(s"      -> Keywords: ${lKeywords.size}")

    lColecciones <- leerDatos[CollectionEntity]("entidad_colecciones.csv")
    _ <- Stream.emits(lColecciones).covary[IO].chunkN(1000).evalMap(c => loteColecciones(c.toList).transact(conApp)).compile.drain
    _ <- IO.println(s"      -> Colecciones: ${lColecciones.size}")

    lPersonas <- leerDatos[PersonEntity]("entidad_personas.csv")
    _ <- Stream.emits(lPersonas).covary[IO].chunkN(1000).evalMap(c => lotePersonas(c.toList).transact(conApp)).compile.drain
    _ <- IO.println(s"      -> Personas: ${lPersonas.size}")

    lUsuarios <- leerDatos[UserEntity]("entidad_usuarios.csv")
    _ <- Stream.emits(lUsuarios).covary[IO].chunkN(1000).evalMap(c => loteUsuarios(c.toList).transact(conApp)).compile.drain
    _ <- IO.println(s"      -> Usuarios: ${lUsuarios.size}")

    // 3. Cargar Películas
    _ <- IO.println("\n[3/4] Cargando base de películas...")
    lPeliculas <- leerDatos[PeliculaLimpia]("entidad_peliculas.csv")
    _ <- Stream.emits(lPeliculas).covary[IO].chunkN(500).evalMap(c => lotePeliculas(c.toList).transact(conApp)).compile.drain
    _ <- IO.println(s"      -> Películas: ${lPeliculas.size}")

    // 4. Cargar Relaciones
    _ <- IO.println("\n[4/4] Estableciendo relaciones...")

    rGen <- leerDatos[MovieGenreLink]("relacion_pelicula_genero.csv")
    _ <- Stream.emits(rGen).covary[IO].chunkN(1000).evalMap(c => loteLinkGeneros(c.toList).transact(conApp)).compile.drain
    _ <- IO.println(s"      -> Relaciones Género: ${rGen.size}")

    rComp <- leerDatos[MovieCompanyLink]("relacion_pelicula_compania.csv")
    _ <- Stream.emits(rComp).covary[IO].chunkN(1000).evalMap(c => loteLinkCompanias(c.toList).transact(conApp)).compile.drain
    _ <- IO.println(s"      -> Relaciones Compañía: ${rComp.size}")

    rPais <- leerDatos[MovieCountryLink]("relacion_pelicula_pais.csv")
    _ <- Stream.emits(rPais).covary[IO].chunkN(1000).evalMap(c => loteLinkPaises(c.toList).transact(conApp)).compile.drain
    _ <- IO.println(s"      -> Relaciones País: ${rPais.size}")

    rIdio <- leerDatos[MovieLanguageLink]("relacion_pelicula_idioma.csv")
    _ <- Stream.emits(rIdio).covary[IO].chunkN(1000).evalMap(c => loteLinkIdiomas(c.toList).transact(conApp)).compile.drain
    _ <- IO.println(s"      -> Relaciones Idioma: ${rIdio.size}")

    rKeys <- leerDatos[MovieKeywordLink]("relacion_pelicula_keyword.csv")
    _ <- Stream.emits(rKeys).covary[IO].chunkN(1000).evalMap(c => loteLinkKeywords(c.toList).transact(conApp)).compile.drain
    _ <- IO.println(s"      -> Relaciones Keyword: ${rKeys.size}")

    rCast <- leerDatos[MovieCastLink]("relacion_pelicula_elenco.csv")
    _ <- Stream.emits(rCast).covary[IO].chunkN(1000).evalMap(c => loteLinkElenco(c.toList).transact(conApp)).compile.drain
    _ <- IO.println(s"      -> Relaciones Elenco: ${rCast.size}")

    rCrew <- leerDatos[MovieCrewLink]("relacion_pelicula_staff.csv")
    _ <- Stream.emits(rCrew).covary[IO].chunkN(1000).evalMap(c => loteLinkStaff(c.toList).transact(conApp)).compile.drain
    _ <- IO.println(s"      -> Relaciones Staff: ${rCrew.size}")

    rRat <- leerDatos[RatingLink]("relacion_pelicula_rating.csv")
    _ <- Stream.emits(rRat).covary[IO].chunkN(2000).evalMap(c => loteRatings(c.toList).transact(conApp)).compile.drain
    _ <- IO.println(s"      -> Ratings: ${rRat.size}")

    _ <- IO.println("\n" + "="*60)
    _ <- IO.println("      ¡MIGRACIÓN FINALIZADA CON ÉXITO!")
    _ <- IO.println("="*60)
  } yield ()
}
```
Este es el módulo final de migración y persistencia de datos. Su objetivo es tomar todos los archivos CSV limpios que generé en los pasos anteriores e insertarlos en mi base de datos MySQL llamada MoviesDB.

Para lograrlo, utilicé la librería Doobie, que me permite ejecutar sentencias SQL de forma segura desde Scala. El flujo de trabajo que implementé consta de tres fases:

Construcción del Esquema: El código se conecta al servidor y ejecuta los comandos CREATE TABLE para levantar toda la estructura relacional (tablas maestras, tablas pivote y claves foráneas).

Lectura de Fuentes: Lee secuencialmente los archivos CSV procesados desde la carpeta data/, deserializándolos en objetos de Scala.

Inserción Optimizada: Para asegurar un alto rendimiento, no inserto los registros uno por uno. En su lugar, implementé una carga por lotes (batch processing), enviando bloques de datos (por ejemplo, de 1000 en 1000) a la base de datos en una sola transacción.


```scala
package Avance3.models

//Aquí va solo lo que lees del CSV original
import fs2.data.csv.*
import fs2.data.csv.generic.semiauto.*

case class PeliculaInput(
                          adult: String,
                          belongs_to_collection: Option[String],
                          budget: String,
                          genres: String,
                          homepage: Option[String],
                          id: String,
                          imdb_id: Option[String],
                          original_language: String,
                          original_title: String,
                          overview: Option[String],
                          popularity: String,
                          poster_path: Option[String],
                          production_companies: String,
                          production_countries: String,
                          release_date: Option[String],
                          revenue: String,
                          runtime: Option[String],
                          spoken_languages: String,
                          status: Option[String],
                          tagline: Option[String],
                          title: String,
                          video: String,
                          vote_average: String,
                          vote_count: String,
                          keywords: String,
                          cast: String,
                          crew: String,
                          ratings: String
                        )

object PeliculaInput {
  given CellDecoder[Option[String]] = CellDecoder[String].map(s => if (s.trim.isEmpty) None else Some(s.trim))
  given CsvRowDecoder[PeliculaInput, String] = deriveCsvRowDecoder[PeliculaInput]
}
```

```scala
package Avance3.models

//El objeto principal de la película ya procesada
import fs2.data.csv.*
import fs2.data.csv.generic.semiauto.*

case class PeliculaLimpia(
                           adult: Option[Boolean],
                           budget: Option[Double],
                           homepage: Option[String],
                           id: Int,
                           imdb_id: Option[String],
                           original_language: Option[String],
                           original_title: String,
                           overview: Option[String],
                           popularity: Option[Double],
                           poster_path: Option[String],
                           release_date: Option[String],
                           revenue: Option[Double],
                           runtime: Option[Double],
                           status: Option[String],
                           tagline: Option[String],
                           title: String,
                           video: Option[Boolean],
                           vote_average: Option[Double],
                           vote_count: Option[Double],
                           collection_id: Option[Int]
                         )

object PeliculaLimpia {
  given CsvRowEncoder[PeliculaLimpia, String] = deriveCsvRowEncoder
  given CsvRowDecoder[PeliculaLimpia, String] = deriveCsvRowDecoder
}
```

```scala
package Avance3.models

//Solo las clases para parsear el JSON string
import io.circe.Decoder
import io.circe.generic.semiauto.deriveDecoder

case class GenreJson(id: Int, name: String)
object GenreJson { implicit val decoder: Decoder[GenreJson] = deriveDecoder }

case class CompanyJson(id: Int, name: String)
object CompanyJson { implicit val decoder: Decoder[CompanyJson] = deriveDecoder }

case class CountryJson(iso_3166_1: String, name: String)
object CountryJson { implicit val decoder: Decoder[CountryJson] = deriveDecoder }

case class LanguageJson(iso_639_1: String, name: String)
object LanguageJson { implicit val decoder: Decoder[LanguageJson] = deriveDecoder }

case class KeywordJson(id: Int, name: String)
object KeywordJson { implicit val decoder: Decoder[KeywordJson] = deriveDecoder }

case class CollectionJson(id: Int, name: String, poster_path: Option[String], backdrop_path: Option[String])
object CollectionJson { implicit val decoder: Decoder[CollectionJson] = deriveDecoder }

case class CastJson(cast_id: Int, character: String, credit_id: String, gender: Option[Int], id: Int, name: String, order: Int, profile_path: Option[String])
object CastJson { implicit val decoder: Decoder[CastJson] = deriveDecoder }

case class CrewJson(credit_id: String, department: String, gender: Option[Int], id: Int, job: String, name: String, profile_path: Option[String])
object CrewJson { implicit val decoder: Decoder[CrewJson] = deriveDecoder }

case class RatingJson(userId: Int, rating: Double, timestamp: Long)
object RatingJson { implicit val decoder: Decoder[RatingJson] = deriveDecoder }
```

```scala
package Avance3.models

//Lo que se exportará a CSV para las tablas maestras
import Avance3.*
import Avance3.models.*
import fs2.data.csv.*
import fs2.data.csv.generic.semiauto.*

case class GenreEntity(genre_id: Int, name: String)
object GenreEntity { given CsvRowEncoder[GenreEntity, String] = deriveCsvRowEncoder; given CsvRowDecoder[GenreEntity, String] = deriveCsvRowDecoder }

case class CompanyEntity(company_id: Int, name: String)
object CompanyEntity { given CsvRowEncoder[CompanyEntity, String] = deriveCsvRowEncoder; given CsvRowDecoder[CompanyEntity, String] = deriveCsvRowDecoder }

case class CountryEntity(iso_3166_1: String, name: String)
object CountryEntity { given CsvRowEncoder[CountryEntity, String] = deriveCsvRowEncoder; given CsvRowDecoder[CountryEntity, String] = deriveCsvRowDecoder }

case class LanguageEntity(iso_639_1: String, name: String)
object LanguageEntity { given CsvRowEncoder[LanguageEntity, String] = deriveCsvRowEncoder; given CsvRowDecoder[LanguageEntity, String] = deriveCsvRowDecoder }

case class KeywordEntity(keyword_id: Int, name: String)
object KeywordEntity { given CsvRowEncoder[KeywordEntity, String] = deriveCsvRowEncoder; given CsvRowDecoder[KeywordEntity, String] = deriveCsvRowDecoder }

case class CollectionEntity(collection_id: Int, name: String, poster: String, backdrop: String)
object CollectionEntity { given CsvRowEncoder[CollectionEntity, String] = deriveCsvRowEncoder; given CsvRowDecoder[CollectionEntity, String] = deriveCsvRowDecoder }

case class PersonEntity(person_id: Int, name: String, gender: Int, profile_path: String)
object PersonEntity { given CsvRowEncoder[PersonEntity, String] = deriveCsvRowEncoder; given CsvRowDecoder[PersonEntity, String] = deriveCsvRowDecoder }

case class UserEntity(user_id: Int)
object UserEntity { given CsvRowEncoder[UserEntity, String] = deriveCsvRowEncoder; given CsvRowDecoder[UserEntity, String] = deriveCsvRowDecoder }
```

```scala
package Avance3.models

//Lo que se exportará a CSV para las tablas pivote/links

import fs2.data.csv.*
import fs2.data.csv.generic.semiauto.*

case class MovieGenreLink(movie_id: Int, genre_id: Int)
object MovieGenreLink { given CsvRowEncoder[MovieGenreLink, String] = deriveCsvRowEncoder; given CsvRowDecoder[MovieGenreLink, String] = deriveCsvRowDecoder }

case class MovieCompanyLink(movie_id: Int, company_id: Int)
object MovieCompanyLink { given CsvRowEncoder[MovieCompanyLink, String] = deriveCsvRowEncoder; given CsvRowDecoder[MovieCompanyLink, String] = deriveCsvRowDecoder }

case class MovieCountryLink(movie_id: Int, iso_3166_1: String)
object MovieCountryLink { given CsvRowEncoder[MovieCountryLink, String] = deriveCsvRowEncoder; given CsvRowDecoder[MovieCountryLink, String] = deriveCsvRowDecoder }

case class MovieLanguageLink(movie_id: Int, iso_639_1: String)
object MovieLanguageLink { given CsvRowEncoder[MovieLanguageLink, String] = deriveCsvRowEncoder; given CsvRowDecoder[MovieLanguageLink, String] = deriveCsvRowDecoder }

case class MovieKeywordLink(movie_id: Int, keyword_id: Int)
object MovieKeywordLink { given CsvRowEncoder[MovieKeywordLink, String] = deriveCsvRowEncoder; given CsvRowDecoder[MovieKeywordLink, String] = deriveCsvRowDecoder }

case class MovieCastLink(movie_id: Int, person_id: Int, character_name: String, order_cast: Int, credit_id: String, cast_id: Int)
object MovieCastLink { given CsvRowEncoder[MovieCastLink, String] = deriveCsvRowEncoder; given CsvRowDecoder[MovieCastLink, String] = deriveCsvRowDecoder }

case class MovieCrewLink(movie_id: Int, person_id: Int, department: String, job: String, credit_id: String)
object MovieCrewLink { given CsvRowEncoder[MovieCrewLink, String] = deriveCsvRowEncoder; given CsvRowDecoder[MovieCrewLink, String] = deriveCsvRowDecoder }

case class RatingLink(movie_id: Int, user_id: Int, rating: Double, timestamp: Long)
object RatingLink { given CsvRowEncoder[RatingLink, String] = deriveCsvRowEncoder; given CsvRowDecoder[RatingLink, String] = deriveCsvRowDecoder }
```


Entradas.scala: Define cómo leer el CSV sucio original.

Limpios.scala: Define la estructura de una película limpia.

JsonIntermedios.scala: Ayuda a interpretar los strings JSON internos.

EntidadesFinales.scala: Define las tablas maestras (Género, País, etc.).

RelacionesFinales.scala: Define las tablas intermedias/pivote.


### Resultados

<img width="1600" height="899" alt="image" src="https://github.com/user-attachments/assets/8c2fcd57-01ec-4022-8aeb-425fe5ee78fe" />

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/62e5a2a1-3067-41ad-8c83-4b25263671f9" />
