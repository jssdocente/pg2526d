# Actividad 04: Programación Funcional (Ejercicios Resueltos)

En esta actividad practicaremos la refactorización de código imperativo a funcional utilizando la temática de una aplicación musical: **Spotify-Lite**.

---

## 🎵 El Modelo de Datos

Usaremos el siguiente `record` para representar las canciones de nuestra biblioteca:

```java
public record Cancion(
    String titulo, 
    String artista, 
    int duracionSegundos, 
    String genero, 
    boolean esFavorita
) {}
```

---

## 🟢 Ejercicio 1: Tus Favoritas (`filter`)

**Reto**: Obtener una lista de todas las canciones marcadas como favoritas.

=== "Código Imperativo"
    ```java
    List<Cancion> favoritas = new ArrayList<>();
    for (Cancion c : biblioteca) {
        if (c.esFavorita()) {
            favoritas.add(c);
        }
    }
    ```

??? success "Solución Funcional"
    ```java
    List<Cancion> favoritas = biblioteca.stream()
        .filter(c -> c.esFavorita())
        .toList();
    ```

---

## 🟢 Ejercicio 2: El Elenco de Artistas (`map` + `distinct`)

**Reto**: Obtener una lista con los nombres de todos los artistas que aparecen en la biblioteca, sin repetir ninguno.

=== "Código Imperativo"
    ```java
    Set<String> artistasSet = new HashSet<>();
    for (Cancion c : biblioteca) {
        artistasSet.add(c.artista());
    }
    List<String> artistas = new ArrayList<>(artistasSet);
    ```

??? success "Solución Funcional"
    ```java
    List<String> artistas = biblioteca.stream()
        .map(c -> c.artista())
        .distinct()
        .toList();
    ```

---

## 🟡 Ejercicio 3: Maratón de un Artista (`filter` + `map`)

**Reto**: Obtener los títulos de las canciones de un artista específico (ej. "Quevedo") que duren más de 3 minutos (180 segundos).

=== "Código Imperativo"
    ```java
    List<String> titulos = new ArrayList<>();
    for (Cancion c : biblioteca) {
        if (c.artista().equals("Quevedo") && c.duracionSegundos() > 180) {
            titulos.add(c.titulo());
        }
    }
    ```

??? success "Solución Funcional"
    ```java
    List<String> titulos = biblioteca.stream()
        .filter(c -> c.artista().equals("Quevedo"))
        .filter(c -> c.duracionSegundos() > 180)
        .map(c -> c.titulo())
        .toList();
    ```

---

## 🟠 Ejercicio 4: Duración Total del Álbum (`mapToInt` + `sum`)

**Reto**: Calcular cuántos segundos de música hay en total en toda la biblioteca.

=== "Código Imperativo"
    ```java
    int totalSegundos = 0;
    for (Cancion c : biblioteca) {
        totalSegundos += c.duracionSegundos();
    }
    ```

??? success "Solución Funcional"
    ```java
    int totalSegundos = biblioteca.stream()
        .mapToInt(c -> c.duracionSegundos())
        .sum();
    ```

---

## 🔴 Ejercicio 5: El Podcast de Éxitos (`Pipeline Completo`)

**Reto**: Crear un `String` que contenga los títulos de las **3 canciones más largas** de género "Pop", ordenados de forma descendente por duración, con el formato: `Título1 | Título2 | Título3`.

=== "Código Imperativo"
    ```java
    List<Cancion> pop = new ArrayList<>();
    for (Cancion c : biblioteca) {
        if (c.genero().equals("Pop")) pop.add(c);
    }
    // Usamos una clase anónima para implementar Comparator al vuelo.
    // Esto ahorra tener que crear un archivo .java externo para la ordenación, 
    // pero ensucia el código con mucha sintaxis repetitiva.
    Collections.sort(pop, new Comparator<Cancion>() {
        @Override
        public int compare(Cancion c1, Cancion c2) {
            return Integer.compare(c2.duracionSegundos(), c1.duracionSegundos());
        }
    });
    
    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < Math.min(3, pop.size()); i++) {
        sb.append(pop.get(i).titulo());
        if (i < 2 && i < pop.size() - 1) sb.append(" | ");
    }
    String resultado = sb.toString();
    ```

??? success "Solución Funcional"
    ```java
    String resultado = biblioteca.stream()
        .filter(c -> c.genero().equals("Pop"))
        .sorted((c1, c2) -> Integer.compare(c2.duracionSegundos(), c1.duracionSegundos()))
        .limit(3)
        .map(c -> c.titulo())
        .collect(Collectors.joining(" | "));
    ```

---

!!! info "Clave del éxito"
    Fíjate cómo en el Ejercicio 5, lo que antes requería crear listas intermedias, usar `sort` manualmente y gestionar un `StringBuilder`, ahora se resuelve en un solo bloque de lógica fluida y fácil de leer.
