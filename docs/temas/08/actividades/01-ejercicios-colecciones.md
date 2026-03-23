# Actividades de Colecciones: Proyecto Base con Tests

En lugar de crear un menú principal, en esta unidad trabajaremos mediante **Desarrollo Guiado por Tests (TDD)**. Tienes a tu disposición un proyecto base con la estructura de paquetes y una serie de tests unitarios que validarán si tu código es correcto.

## Estructura del Proyecto

El código base se encuentra en la carpeta [`01-ejercicios-colecciones`](./01-base-ejercicio-colecciones.zip). Debes implementar la lógica en las clases dentro de `src/main/java`.

---

## Ejercicio 1: `GestorNotas` (`ArrayList`)

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

**Tarea**: Completa las clases `Producto` e `Inventario` en el paquete `ejercicio2`.

- **Métodos de Inventario**:
    - `void agregar(Producto p)`: Añade el producto.
    - `Producto buscarPorId(int id)`: Retorna el objeto o `null`.
    - `boolean actualizarStock(String nombre, int nuevoStock)`: Busca por nombre exacto y modifica su stock.
    - `void eliminar(int id)`: Elimina el producto por su identificador.

**Validación**: Ejecuta `InventarioTest.java`.

---

## Ejercicio 3: `ControlAcceso` (`HashSet`)

**Tarea**: Implementa el registro de asistentes únicos.

- **Métodos**:
    - `void registrar(String nombre)`: Registra al asistente (no debe duplicarse).
    - `int totalUnicos()`: Retorna cuántas personas distintas hay.
    - `boolean estaEnLista(String nombre)`: Verifica presencia.

---

## Ejercicio 4: `Diccionario` (`HashMap`)

**Tarea**: Implementa la lógica del traductor.

- **Métodos**:
    - `void guardarTraduccion(String espanol, String ingles)`: Mapea la palabra.
    - `String traducir(String palabra)`: Retorna el valor o `null` si no existe.

---

## Ejercicio 5: `Navegador` (`Deque`)

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
