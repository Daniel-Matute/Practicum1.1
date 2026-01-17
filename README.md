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

<img width="886" height="553" alt="image" src="https://github.com/user-attachments/assets/30410350-adce-4e15-b0e3-f53e175818bb" />

<img width="783" height="748" alt="image" src="https://github.com/user-attachments/assets/ba3363df-e03e-484c-a2ad-78d9d6c34510" />

<img width="850" height="630" alt="image" src="https://github.com/user-attachments/assets/19f2b451-6e3a-40a9-a92a-1d305196daa6" />

5.3 Análisis de datos en columnas numéricas (estadísticas básicas)


<img width="284" height="682" alt="image" src="https://github.com/user-attachments/assets/0d484fd4-446e-4c2b-a5d6-4b2bf61f0562" />

<img width="265" height="681" alt="image" src="https://github.com/user-attachments/assets/dfb14e0f-fb90-4541-b812-64e17221bcc6" />



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

<img width="862" height="411" alt="image" src="https://github.com/user-attachments/assets/e6fda583-3384-46ec-a667-c900a9589bb9" />

<img width="609" height="669" alt="image" src="https://github.com/user-attachments/assets/0a678c65-48b4-4ee8-a2ab-0f48382b4c3f" />

<img width="741" height="614" alt="image" src="https://github.com/user-attachments/assets/ac257e11-80af-4d69-8469-605eeef64421" />

<img width="338" height="467" alt="image" src="https://github.com/user-attachments/assets/4cdc4110-53f0-411a-a41a-fa9c52119a01" />

<img width="886" height="441" alt="image" src="https://github.com/user-attachments/assets/9415b6ff-5365-418f-9f9b-504fbf4f842c" />

<img width="886" height="221" alt="image" src="https://github.com/user-attachments/assets/49fc6762-21c1-47fc-af60-20ea65938591" />

<img width="484" height="477" alt="image" src="https://github.com/user-attachments/assets/2dc2b39f-7750-45ff-9861-9818c9d040ec" />

<img width="886" height="425" alt="image" src="https://github.com/user-attachments/assets/cdde6ce5-6190-4437-a744-32e4dcdad5e6" />

5.5 Limpieza de datos (columnas con valores nulos, valores atípicos, etc.)

<img width="841" height="565" alt="image" src="https://github.com/user-attachments/assets/2bbd79f3-cec5-4c0b-a92f-31f5c9e04b4e" />

<img width="708" height="749" alt="image" src="https://github.com/user-attachments/assets/b31827c5-eda4-4d62-967e-c2a0031a9831" />

<img width="713" height="608" alt="image" src="https://github.com/user-attachments/assets/8cd97042-820d-4262-a09b-3eb8bf0bfc02" />

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

Este codigo en Resumen hace:
Lectura: Lee el archivo pi_movies_complete.csv línea por línea.

Preservación del Header: Deja la primera línea (encabezados) intacta.

Normalización: Para el resto de las filas, asegura que siempre haya 28 columnas:

* Si hay más, las corta.

* Si hay menos, rellena con vacío.

* Limpieza de texto (cleanField):

* Elimina espacios en blanco al inicio/final y reduce espacios dobles.

* Reemplaza comillas simples (') por dobles (") (útil para JSON).

* Convierte la palabra "none" en "null".

* Escritura: Guarda el resultado limpio en un nuevo archivo pi_movies_complete_clean.csv.


```scala
package models

import cats.effect.{IO, IOApp}
import fs2.*
import fs2.io.file.{Files, Path}
import fs2.text

object CleanCSV extends IOApp.Simple {

  val inputPath: Path =
    Path("C:\\Programacion Funcional y Reactiva\\Prueeba3\\src\\main\\resources\\data\\pi_movies_complete.csv")

  val outputPath: Path =
    Path("C:\\Programacion Funcional y Reactiva\\Prueeba3\\src\\main\\resources\\data\\pi_movies_complete_clean.csv")

  def cleanField(raw: String): String = {
    val trimmed = raw.trim
    if (trimmed.isEmpty) ""
    else
      trimmed
        .replace("'", "\"")
        .replaceAll("(?i)\\bnone\\b", "null")
        .replaceAll("\\s+", " ")
  }
  override def run: IO[Unit] =
    Files[IO]
      .readAll(inputPath)
      .through(text.utf8.decode)
      .through(text.lines)
      .zipWithIndex
      .map { case (line, index) =>

        // HEADER: se deja exactamente igual
        if (index == 0) line
        else {
          val columns = line.split("\t", -1)

          // Limpieza campo por campo
          val cleanedColumns =
            columns
              .take(28) // si hay más, cortamos
              .padTo(28, "") // si hay menos, rellenamos
              .map(cleanField)

          cleanedColumns.mkString("\t")
        }
      }
      .intersperse("\n")
      .through(text.utf8.encode)
      .through(Files[IO].writeAll(outputPath))
      .compile
      .drain
}

```


### Paso 5: Presentación de resultados

Finalmente, se mostraron los resultados obtenidos después de la limpieza:



CREW
<img width="1917" height="1027" alt="image" src="https://github.com/user-attachments/assets/050099b4-f9a0-4e04-977e-f0e778006b6d" />



Limpieza completa del csv

<img width="1919" height="1028" alt="image" src="https://github.com/user-attachments/assets/8d636c8e-567c-423d-b07d-21ec4c1e5586" />





