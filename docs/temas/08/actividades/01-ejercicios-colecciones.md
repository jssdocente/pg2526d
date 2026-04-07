# Actividades de Colecciones: Proyecto Base con Tests

En lugar de crear un menú principal, en esta unidad trabajaremos mediante **Desarrollo Guiado por Tests (TDD)**. Tienes a tu disposición un proyecto base con la estructura de paquetes y una serie de tests unitarios que validarán si tu código es correcto.

## Estructura del Proyecto

El código base se encuentra en la carpeta [`01-ejercicios-colecciones`](./01-base-ejercicio-colecciones.zip). Debes implementar la lógica en las clases dentro de `src/main/java`.

---

## Ejercicio 1: `GestorNotas` (`ArrayList`)

??? note "Concepto: ArrayList"

    Un `ArrayList` es una lista dinámica que puede crecer o encogerse según sea necesario. A diferencia de un array convencional, no necesitas definir su tamaño al inicio. Es ideal cuando necesitas acceso rápido a los elementos mediante un índice, aunque insertar o eliminar elementos en medio de la lista puede ser más costoso en términos de rendimiento.

    **Métodos más importantes:**

    - `add(E e)`: Añade un elemento al final.
    - `get(int index)`: Obtiene el elemento en esa posición (O(1)).
    - `remove(int index)`: Elimina el elemento de la posición indicada.
    - `size()`: Devuelve el número de elementos actuales.
    - `isEmpty()`: Indica si la lista tiene elementos o no.

??? tip "Solución"

    ```java
    import java.util.ArrayList;
    import java.util.List;

    public class GestorNotas {
        private List<Double> notas = new ArrayList<>();

        public void addNota(double nota) {
            notas.add(nota);
        }

        public double calcularMedia() {
            if (notas.isEmpty()) return 0;

            double suma = 0;
            for (double n : notas) {
                suma += n;
            }
            
            return suma / notas.size();
        }

        public boolean eliminarNotaMasBaja() {
            if (notas.isEmpty()) return false;
            double min = notas.get(0);
            int indexMin = 0;
            for (int i = 1; i < notas.size(); i++) {
                if (notas.get(i) < min) {
                    min = notas.get(i);
                    indexMin = i;
                }
            }
            notas.remove(indexMin);
            return true;
        }

        public int totalNotas() {
            return notas.size();
        }
    }
    ```

**Tarea**: Implementa la clase `GestorNotas` en el paquete `ejercicio1`.

- **Atributos**: Una lista dinámica de números reales.
- **Métodos a implementar**:
    - `void addNota(double nota)`: Añade la nota a la colección.
    - `double calcularMedia()`: Devuelve el promedio de todas las notas. Si no hay notas, devuelve 0.
    - `boolean eliminarNotaMasBaja()`: Busca y elimina la nota con menor valor. Retorna `true` si se eliminó.
    - `int totalNotas()`: Retorna el tamaño de la lista.

**Validación**: Ejecuta `GestorNotasTest.java`.

---

## Ejercicio 2: `Inventario` (CRUD con Objetos)

??? note "Concepto: CRUD con Colecciones de Objetos"
    En las aplicaciones reales, raramente guardamos tipos simples (como `int` o `String`) solos; solemos guardar **Objetos**. Para trabajar con ellos en colecciones, debemos saber buscar dentro de la lista comparando atributos (como un `id` o un `nombre`). Este ejercicio simula las operaciones básicas de una base de datos: Crear, Leer, Actualizar y Borrar (CRUD).

    **Métodos clave para manipular objetos:**

    - `add(E e)`: Almacena el objeto en la colección.
    - Búsqueda manual: Recorrer con un `for` y comparar con `.equals()` o un identificador único.
    - `remove(Object o)`: Elimina el objeto si lo encuentra (usa `equals()`).
    - `contains(Object o)`: Comprueba si existe (usa `equals()`).

    > 💡 **Tip**: Cuando comparas objetos propios en una lista, es fundamental que tu clase tenga sobrescrito el método `equals()` correctamente.

??? tip "Solución"

    ```java
    import java.util.ArrayList;
    import java.util.List;
    import java.util.Objects;

    class Producto {
        private int id;
        private String nombre;
        private int stock;

        public Producto(int id, String nombre, int stock) {
            this.id = id;
            this.nombre = nombre;
            this.stock = stock;
        }

        public int getId() { return id; }
        public String getNombre() { return nombre; }
        public int getStock() { return stock; }
        public void setStock(int stock) { this.stock = stock; }

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (o == null || getClass() != o.getClass()) return false;
            Producto producto = (Producto) o;
            return id == producto.id;
        }

        @Override
        public int hashCode() {
            return Objects.hash(id);
        }
    }

    public class Inventario {
        private List<Producto> productos = new ArrayList<>();

        public void agregar(Producto p) {
            productos.add(p);
        }

        public Producto buscarPorId(int id) {
            for (Producto p : productos) {
                if (p.getId() == id) return p;
            }
            return null;
        }

        public boolean actualizarStock(String nombre, int nuevoStock) {
            for (Producto p : productos) {
                if (p.getNombre().equals(nombre)) {
                    p.setStock(nuevoStock);
                    return true;
                }
            }
            return false;
        }

        public void eliminar(int id) {
            Producto p = buscarPorId(id);
            if (p != null) {
                productos.remove(p);
            }
        }
    }
    ```

**Tarea**: Completa las clases `Producto` e `Inventario` en el paquete `ejercicio2`.

- **Métodos de Inventario**:
    - `void agregar(Producto p)`: Añade el producto.
    - `Producto buscarPorId(int id)`: Retorna el objeto o `null`.
    - `boolean actualizarStock(String nombre, int nuevoStock)`: Busca por nombre exacto y modifica su stock.
    - `void eliminar(int id)`: Elimina el producto por su identificador.

**Validación**: Ejecuta `InventarioTest.java`.

---

## Ejercicio 3: `ControlAcceso` (`HashSet`)

??? note "Concepto: HashSet"
    Un `HashSet` es una colección que **no permite duplicados**. Si intentas añadir dos veces el mismo elemento (por ejemplo, el mismo nombre de usuario), el conjunto simplemente lo ignorará. Además, no garantiza ningún orden en los elementos. Es la herramienta perfecta cuando lo único que te importa es saber si algo está "dentro" o "fuera" de una lista sin repeticiones.

    **Métodos más importantes:**

    - `add(E e)`: Añade si no existe. Devuelve `false` si ya estaba.
    - `contains(Object o)`: Comprobación ultra-rápida (O(1)).
    - `remove(Object o)`: Elimina el elemento.
    - `size()`: Total de elementos únicos.

    **¿Cómo sabe qué un elemento está repetido?**

    Para detectar duplicados de forma eficiente, Java utiliza dos métodos:
    1.  `hashCode()`: Genera un número (huella digital) del objeto para saber en qué zona buscar.
    2.  `equals()`: Si dos objetos tienen la misma "huella", comprueba si son idénticos campo por campo.
    **Si no sobrescribes ambos, el HashSet no podrá detectar duplicados de tus clases personalizadas.**

**Tarea**: Implementa el registro de asistentes únicos.

- **Métodos**:
    - `void registrar(String nombre)`: Registra al asistente (no debe duplicarse).
    - `int totalUnicos()`: Retorna cuántas personas distintas hay.
    - `boolean estaEnLista(String nombre)`: Verifica presencia.

---

## Ejercicio 4: `Diccionario` (`HashMap`)

??? note "Concepto: HashMap (Mapas)"
    Un `HashMap` funciona como un diccionario de la vida real: tienes una **Clave** (la palabra en español) que te lleva directamente a un **Valor** (su traducción en inglés). La búsqueda por clave es instantánea (O(1)), lo que los hace extremadamente eficientes para buscar asociaciones de datos. Las claves siempre son únicas.

    **Métodos más importantes:**

    - `put(K clave, V valor)`: Guarda la asociación. Si la clave ya existía, sobrescribe el valor.
    - `get(Object clave)`: Recupera el valor asociado. Devuelve `null` si la clave no existe.
    - `containsKey(Object clave)`: Comprueba si existe la clave.
    - `remove(Object clave)`: Elimina la entrada asociada a esa clave.

**Tarea**: Implementa la lógica del traductor.

- **Métodos**:
    - `void guardarTraduccion(String espanol, String ingles)`: Mapea la palabra.
    - `String traducir(String palabra)`: Retorna el valor o `null` si no existe.

---

## Ejercicio 5: `Navegador` (`Deque`)

??? note "Concepto: Deque como Pila (Stack)"
    Aunque Java tiene una clase `Stack`, hoy en día se recomienda usar la interfaz `Deque` (Double Ended Queue). En este ejercicio la usaremos como una **Pila (LIFO - Last In, First Out)**, tal como funciona el botón "Atrás" de un navegador: la última página visitada es la primera en recuperarse al retroceder.

    **Métodos más importantes (Lógica de Pila):**
    
    - `push(E e)`: Apila un elemento (lo pone arriba de todo).
    - `pop()`: Desapila el elemento superior (lo extrae y lo devuelve). Lanzo excepción si está vacío.
    - `peek()`: Mira el elemento superior sin extraerlo.
    - `isEmpty()`: Para comprobar si hay elementos antes de hacer un `pop`.

**Tarea**: Simula el historial de navegación "Atrás".

- **Métodos**:
    - `void navegar(String url)`: Añade la URL al historial.
    - `String retroceder()`: Extrae y retorna la última URL (LIFO).

---

## 🛠️ Cómo empezar

1. Importa el proyecto en tu IDE (IntelliJ o VS Code).
2. Abre la clase del ejercicio bajo `src/main/java`.
3. Implementa los métodos.
4. Ejecuta el test correspondiente bajo `src/test/java`. Si el test sale en **VERDE**, el ejercicio está bien realizado.
