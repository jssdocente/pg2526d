# **UT8: Estructuras de datos dinámicas**

!!! tip "Información de la unidad"

    === "Contenidos"

        Colecciones dinámicas:

        - Listas (arrays dinámicos): concepto, operaciones básicas (añadir, eliminar, buscar, insertar).
        - Iteradores para recorrer colecciones.
        - Otras colecciones: conjuntos (sets), mapas (dictionaries/hash maps) – características y ventajas.
        - Clases y métodos genéricos.

        Expresiones regulares:

        - Búsqueda de patrones en cadenas de texto.
        - Clases y librerías para tratamiento de documentos (JSON, XML, etc.).
        - Manipulación de documentos de intercambio de datos.

    === "Propuesta didáctica"

        En esta unidad vamos a seguir trabajando el **RA6: Escribe programas que manipulen información seleccionando y utilizando tipos avanzados de datos.**

        Criterios de evaluación clave que abordaremos:

        - **CE6b**: Se han reconocido las librerías de clases relacionadas con tipos de datos avanzados.
        - **CE6f**: Se han creado clases y métodos genéricos.
        - **CE6g**: Se han utilizado expresiones regulares en la búsqueda de patrones en cadenas de texto.
        - **CE6h**: Se han identificado las clases relacionadas con el tratamiento de documentos escritos en diferentes lenguajes de intercambio de datos.
        - **CE6i**: Se han realizado programas que realicen manipulaciones sobre documentos escritos en diferentes lenguajes de intercambio de datos.
        - **CE6j**: Se han utilizado operaciones agregadas para el manejo de información almacenada en colecciones.

    === "Programación de Aula"

        Esta unidad se imparte en la **segunda evaluación**, con una duración estimada de **15 sesiones lectivas**, aproximadamente entre la **2ª semana de febrero y la 1ª semana de marzo de 2026**. (Se impartirían a razón de 5-6 sesiones por semana).

        | Sesión | Contenidos | Criterios trabajados |
        |---|---|---|
        | 1-3 | Listas (arrays dinámicos): concepto, operaciones básicas y avanzadas. Iteradores. | CE6c, CE6d |
        | 4-6 | Otras colecciones: conjuntos (sets) y mapas (dictionaries/hash maps). Comparativa. | CE6e |
        | 7-8 | Clases y métodos genéricos: concepto, utilidad e implementación. | CE6f |
        | 9-11 | Expresiones regulares: sintaxis, búsqueda de patrones en texto. | CE6g |
        | 12-14 | Clases y librerías para tratamiento de documentos (JSON, XML). Manipulación. | CE6h, CE6i |
        | 15 | Operaciones agregadas para el manejo de información en colecciones. Práctica integradora. | CE6j, CE6c-j (Refuerzo) |


Este tema abarca los conceptos fundamentales para gestionar datos de forma flexible y eficiente en Java, permitiendo que tus aplicaciones crezcan sin las limitaciones de tamaño fijo de los arrays tradicionales.

## Contenidos del Tema

1.  **[Genéricos](02-genericos.md)**: Aprenderás a escribir código reutilizable y seguro (type-safe), evitando duplicaciones y errores de conversión (casting) en tiempo de ejecución.
2.  **[Ordenación y Comparación](03-ordenacion-comparacion.md)**: Dominarás las interfaces `Comparable` y `Comparator` para definir el orden natural y personalizado de tus objetos.
3.  **[Colecciones (JCF)](04-colecciones.md)**: Estudiarás el **Java Collections Framework**, la librería estándar de Java para manejar Listas, Conjuntos, Mapas y Colas.
4.  **[Anexo: Programación Funcional](99-anexo_programacion-funcional.md)**: Una introducción avanzada al uso de **Lambdas** y el **Stream API** para procesar datos de forma declarativa.

---

## 1. Introducción

En el desarrollo de software moderno, rara vez sabemos de antemano cuántos datos vamos a procesar. La capacidad de Java para manejar estructuras que crecen y se contraen dinámicamente es crucial.

Estos tres pilares están profundamente interrelacionados:
- **Los genéricos** proporcionan la estructura segura (Type-Safety).
- **Las colecciones** proporcionan el almacenamiento físico de los objetos.
- **La programación funcional** (Streams) permite operar sobre esas colecciones de forma elegante.

### 1.1. Ejemplo: De lo imperativo a lo funcional

Imagina que tienes una lista de números y quieres filtrar los pares, duplicarlos y sumarlos.

**Enfoque Tradicional (Imperativo):**
```java
List<Integer> numeros = List.of(1, 2, 3, 4, 5, 6);
int suma = 0;
for (Integer n : numeros) {
    if (n % 2 == 0) {
        suma += n * 2;
    }
}
```

**Enfoque Moderno (Declarativo/Funcional):**
```java
List<Integer> numeros = List.of(1, 2, 3, 4, 5, 6);
int suma = numeros.stream()
    .filter(n -> n % 2 == 0)  // Filtrar pares
    .map(n -> n * 2)          // Duplicar
    .reduce(0, Integer::sum); // Sumar
```

### 1.2. Paradigmas de Programación en Java

Java es un lenguaje **multiparadigma**:

1.  **Imperativo**: Instrucciones paso a paso y control manual del estado.
2.  **Orientado a Objetos (POO)**: Organización en clases, encapsulamiento y jerarquías.
3.  **Funcional (Java 8+)**: Tratamiento de funciones como parámetros y procesamiento de flujos de datos (Streams).

!!! tip "Consejo del Profesor"
    No te limites a un solo paradigma. Un buen programador sabe cuándo usar un bucle `for` tradicional por rendimiento o claridad, y cuándo usar un `Stream` para operaciones de filtrado y transformación complejas.

---

## 📂 Anexos
Para profundizar en el procesado de datos moderno, consulta el **[Anexo de Programación Funcional](99-anexo_programacion-funcional.md)**, donde se explican en detalle las Lambdas, las Referencias a Métodos (`::`) y el motor de Streams.
