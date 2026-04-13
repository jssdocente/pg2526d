# Actividad 05: Programación Funcional (Streams API)

En esta sesión dejaremos de escribir código basado en **"Cómo"** se hacen las cosas (bucles, contadores, variables auxiliares) para centrarnos en **"Qué"** queremos obtener. 

Usaremos como base un sistema de gestión de videojuegos llamado **Steam-Pro**.

En lugar de crear el código desde cero, trabajarás sobre un proyecto base utilizando **Desarrollo Guiado por Tests (TDD)**. Tienes a tu disposición el código con los métodos ya implementados de forma **imperativa** y deberás refactorizarlos a su versión **funcional**.

---

## 🛠️ Estructura del Proyecto

El código base se encuentra en el archivo [`05-base-steam-pro.zip`](./05-base-steam-pro.zip). Debes implementar la lógica funcional en la clase `GestorCatalogo` y validar tus avances ejecutando los tests en `src/test/java`.

## 🎮 El Modelo de Datos

Imagina que tienes una `List<Videojuego> catalogo` con los siguientes datos:

```java
public record Videojuego(
    String titulo, 
    String genero, 
    double precio, 
    int valoracion, // de 0 a 100
    boolean esMultijugador
) {}
```

---

## 🟢 Ejercicio 1: El Buscador de "Free-to-Play" (`filter`)

**Misión**: El usuario quiere ver una lista de todos los juegos que son gratuitos (precio 0).

=== "Código Imperativo (Antiguo)"
    ```java
    List<Videojuego> gratuitos = new ArrayList<>();
    for (Videojuego v : catalogo) {
        if (v.precio() == 0) {
            gratuitos.add(v);
        }
    }
    ```

**Tarea**: Implementa la misma lógica usando `.stream()`, `.filter()` y `.toList()`.

---

## 🟢 Ejercicio 2: Newsletter de Títulos (`map`)

**Misión**: Queremos enviar un correo y solo necesitamos una `List<String>` con los nombres (`titulo`) de todos los juegos del catálogo.

=== "Código Imperativo (Antiguo)"
    ```java
    List<String> nombres = new ArrayList<>();
    for (Videojuego v : catalogo) {
        nombres.add(v.titulo());
    }
    ```

**Tarea**: Implementa la misma lógica usando `.stream()`, `.map()` y `.toList()`.

---

## 🟡 Ejercicio 3: Los Mejores de Terror (`filter` + `map` + `sorted`)

**Misión**: Queremos obtener los títulos de los juegos de género **"Terror"** que tengan una valoración superior a **80**, ordenados alfabéticamente.

=== "Código Imperativo (Antiguo)"
    ```java
    List<String> resultados = new ArrayList<>();
    for (Videojuego v : catalogo) {
        if (v.genero().equals("Terror") && v.valoracion() > 80) {
            resultados.add(v.titulo());
        }
    }
    Collections.sort(resultados);
    ```

**Tarea**: Crea un pipeline de Stream que filtre, transforme y ordene los datos.

---

## 🟠 Ejercicio 4: Valoración Media (`mapToInt` + `average`)

**Misión**: Queremos saber cuál es la valoración media de toda nuestra biblioteca de juegos.

=== "Código Imperativo (Antiguo)"
    ```java
    if (catalogo.isEmpty()) return 0;
    double suma = 0;
    for (Videojuego v : catalogo) {
        suma += v.valoracion();
    }
    double media = suma / catalogo.size();
    ```

**Tarea**: Usa `.stream()`, `.mapToInt()` y el método especial `.average()`. 
*(Pista: El resultado de average es un `OptionalDouble`, usa `.orElse(0)`).*

---

## 🔴 Ejercicio 5: Informe de Joyas Ocultas (Pipeline Completo)

**Misión**: Necesitamos un único `String` que contenga los nombres de los **3 juegos multijugador más baratos**, separados por un guion ` - `.

=== "Código Imperativo (Antiguo)"
    ```java
    // 1. Filtrar multijugador
    List<Videojuego> multi = new ArrayList<>();
    for (Videojuego v : catalogo) {
        if (v.esMultijugador()) multi.add(v);
    }
    // 2. Ordenar por precio (ascendente) de forma IMPERATIVA.
    // Usamos una clase anónima para no tener que crear un archivo aparte (ej. ComparadorPrecio.java).
    // Evitamos crear muchos archivos, pero llenamos el código de "ruido" (boilerplate).
    Collections.sort(multi, new Comparator<Videojuego>() {
        @Override
        public int compare(Videojuego v1, Videojuego v2) {
            return Double.compare(v1.precio(), v2.precio());
        }
    });
    // 3. Coger los 3 primeros y unir nombres
    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < Math.min(3, multi.size()); i++) {
        sb.append(multi.get(i).titulo());
        if (i < 2 && i < multi.size() - 1) sb.append(" - ");
    }
    String resultado = sb.toString();
    ```

**Tarea**: Implementa este proceso complejo en un solo pipeline usando `filter`, `sorted`, `limit`, `map` y `collect(Collectors.joining(" - "))`.

---

!!! tip "Recuerda"
    En programación funcional, cada paso del pipeline recibe una "corriente" de datos, la procesas y la pasas al siguiente nivel. ¡No modifiques la lista original!
