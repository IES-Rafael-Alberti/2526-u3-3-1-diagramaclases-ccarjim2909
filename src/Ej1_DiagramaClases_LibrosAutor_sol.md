Solución: Ejercicio 1: Diagrama de Clases - Sistema de Libros y Autores
=============================================================
=====================================

📚 Análisis del Problema y Clases
---------------------------------

Este sistema es sencillo y se centra en dos entidades principales: **Autor** y **Libro**. La clase coordinadora (`JuegoNIM` en el ejemplo anterior) no es necesaria aquí, ya que el sistema es un modelo de datos.

### 1\. Identificación de Clases

-   **Autor:** Entidad que crea el contenido. Necesita almacenar datos personales.

-   **Libro:** Entidad que almacena la información del contenido.

### 2\. Análisis de Relaciones

La relación clave es la que conecta al escritor con su obra:

-   **Tipo de Relación:** Asociación simple (Unidireccional o Bidireccional, pero siguiendo la pauta "Autor escribe Libro", se modelará como una asociación clara).

-   **Cardinalidad:**

    -   Un **Autor** puede escribir **uno o varios Libros** (`1..*`).

    -   Un **Libro** es escrito por **un único Autor** (`1`).

-   **Roles:** El rol desde `Autor` hacia `Libro` es **"escribe"**.

* * * * *

🧩 Diagrama de Clases UML
-------------------------

### Tabla de Clases, Propiedades y Métodos

| **Clase** | **Propiedades (Visibilidad)** | **Métodos (Visibilidad)** |
| --- | --- | --- |
| **Autor** | -nombre: String | +escribir(): void |
|  | -apellido: String | +getNombreCompleto(): String {derived} |
|  | -nacionalidad: String |  |
|  | -fechaNacimiento: Date |  |
| **Libro** | -titulo: String | +leer(): void |
|  | -isbn: String | +getTitulo(): String |
|  | -numeroPaginas: Int | +getPrecio(): Decimal |
|  | -precio: Decimal |  |

### Código PlantUML

Fragmento de código

```
@startuml SistemaBiblioteca

skinparam classAttributeIconSize 0
skinparam class {
    BackgroundColor WhiteSmoke
    BorderColor Black
    ArrowColor Black
}

' Clase Autor
class Autor {
    - nombre: String
    - apellido: String
    - nacionalidad: String
    - fechaNacimiento: Date
    --
    + escribir(): void
    + getNombreCompleto(): String {derived}
}

' Clase Libro
class Libro {
    - titulo: String
    - isbn: String
    - numeroPaginas: Int
    - precio: Decimal
    --
    + leer(): void
    + getTitulo(): String
    + getPrecio(): Decimal
}

' Relación entre Autor y Libro
' Autor escribe 1 o más Libros (1..*)
' Libro es escrito por 1 Autor (1)

Autor "1" -- "1..*" Libro : escribe >

note right of Autor::getNombreCompleto
    Campo derivado:
    Se calcula a partir de
    {nombre + " " + apellido}
end note

@enduml

```

### 🖼️ Diagrama Generado

* * * * *

🔍 Justificación de Decisiones
------------------------------

1.  **Visibilidad y Encapsulación:** Todos los atributos se han definido como **privados (`-`)** para proteger el estado interno de las clases, mientras que los métodos de comportamiento y acceso (getters) son **públicos (`+`)**.

2.  **Campo Derivado (`{derived}`):** El método `getNombreCompleto()` se ha marcado con la restricción `{derived}` para indicar que su valor no se almacena directamente, sino que se calcula en tiempo de ejecución a partir de los atributos `-nombre` y `-apellido`.

3.  **Cardinalidad:**

    -   `Autor "1"`: Un libro **debe** tener un autor.

    -   `Libro "1..*"`: Un autor puede escribir **uno o más** libros. La notación `1..*` (uno a muchos) modela el requisito de que "un Autor escribe uno o varios Libros".

4.  **Rol:** La relación se etiqueta con el rol **"escribe"** y la flecha de dirección indica el flujo de la acción.

* * * * *

💡 Lógica Clave / Pseudocódigo
------------------------------

### Pseudocódigo para `getNombreCompleto()` (Clase Autor)

```
fun getNombreCompleto(): String
    retornar nombre + " " + apellido

```

### Pseudocódigo para el Algoritmo de Relación

La relación de **uno a muchos (1..*)** se implementa internamente en la clase `Autor` mediante una lista o colección de objetos `Libro`.

```
class Autor {
    // ... atributos ...
    private librosEscritos: List<Libro>  // Colección interna para la relación 1..*

    fun escribirLibro(libro: Libro): void
        this.librosEscritos.agregar(libro)
        libro.asignarAutor(this)
}
```