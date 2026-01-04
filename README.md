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

### Paso 1: Preparación del archivo JSON

Se creó un archivo JSON pequeño que representa una muestra de la columna `crew` del dataset original.  
Este archivo contiene registros válidos e incompletos para aplicar procesos de limpieza de datos.

**Ruta del archivo:**

src/main/resources/data/crew.json


**Estructura de ejemplo:**

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

### Paso 2: Configuración de dependencias

Para el manejo de JSON en Scala se incorporaron las siguientes dependencias en el archivo build.sbt:
```scala
libraryDependencies ++= Seq(
  "io.circe" %% "circe-core" % "0.14.6",
  "io.circe" %% "circe-generic" % "0.14.6",
  "io.circe" %% "circe-parser" % "0.14.6"
)
```
Estas librerías permiten:

Decodificar JSON a clases Scala

Manejar errores de forma segura

Trabajar bajo un enfoque funcional

### Paso 3: Definición del modelo de datos

Se definió una clase de dominio que representa la estructura de cada elemento de la columna crew:
```scala
case class CrewMember(
  id: Int,
  job: String,
  department: String,
  name: String
)
```
Este modelo permite mapear directamente los datos del archivo JSON a objetos Scala tipados.

### Paso 4: Lectura y decodificación del archivo JSON

El archivo JSON fue leído desde el sistema de archivos y posteriormente decodificado utilizando Circe.

El proceso incluyó:

Lectura del archivo como texto.

Decodificación del contenido JSON a una lista de objetos CrewMember.

Manejo de posibles errores de parsing.

Este enfoque garantiza una lectura segura de datos semi-estructurados.

### Paso 5: Limpieza de datos

Una vez decodificados los datos, se aplicó un proceso de limpieza con los siguientes criterios:

Se eliminaron registros con campos obligatorios vacíos (name o job).

Solo se conservaron los registros válidos para el análisis.

Este paso es fundamental para asegurar la calidad de los datos antes de su análisis.

```scala
import io.circe.*
import io.circe.generic.auto.*
import io.circe.parser.*

import scala.io.Source

object CirceCrewExample extends App {

  val path = "src/main/resources/data/crew.json"

  // 1. Leer archivo JSON
  val jsonString = Source.fromFile(path).getLines().mkString

  // 2. Parsear JSON
  val decoded = decode[List[CrewMember]](jsonString)

  decoded match {
    case Left(error) =>
      println(s"❌ Error al parsear JSON: $error")

    case Right(crewList) =>

      println(s"Total registros leídos: ${crewList.size}")

      // 3. Limpieza de datos
      val crewLimpio = crewList.filter(c =>
        c.name.nonEmpty && c.job.nonEmpty
      )

      println(s"Registros válidos después de limpieza: ${crewLimpio.size}")

      // 4. Resultados
      println("\n🎬 CREW VÁLIDO:")
      crewLimpio.foreach { c =>
        println(s"- ${c.name} | ${c.job} | ${c.department}")
      }
  }
}
```

### Paso 6: Presentación de resultados

Finalmente, se mostraron los resultados obtenidos después de la limpieza:

Total de registros leídos desde el archivo JSON.

Cantidad de registros válidos luego del proceso de limpieza.

Listado de los miembros del crew con información válida (nombre, rol y departamento).

Este análisis permitió observar cómo Circe facilita el manejo y validación de datos en formato JSON dentro de un proyecto en Scala.


<img width="1909" height="1033" alt="image" src="https://github.com/user-attachments/assets/e05080b9-deef-4a97-9ee5-cef70306a167" />




### Conclusión

El uso de la librería Circe permitió:

Procesar datos semi-estructurados de forma segura.

Aplicar técnicas de limpieza de datos.

Comprender el manejo práctico de columnas JSON presentes en datasets reales.

Este enfoque cumple con los objetivos planteados en la actividad y sienta las bases para el análisis de estructuras JSON más complejas en proyectos futuros.
