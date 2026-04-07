# Actividades de Genéricos: Clases y Métodos

En esta unidad profundizaremos en el uso de los **Tipos Genéricos** en Java. El objetivo es que aprendas a crear tus propias clases y métodos que puedan trabajar con cualquier tipo de objeto, garantizando la seguridad de tipos (type-safety) en tiempo de compilación.

---

## Ejercicio 1: El Contenedor Genérico

??? note "Concepto: Clases Genéricas"

    Una clase genérica se define utilizando un parámetro de tipo, normalmente representado por la letra `T` (Type). Este parámetro actúa como un "placeholder" que se sustituye por un tipo real cuando instanciamos la clase.
    
    **Sintaxis básica:**
    ```java
    public class MiClase<T> {
        private T dato;
        // ...
    }
    ```

**Objetivo**: Crear una clase "envoltorio" (wrapper) que sirva para almacenar un único objeto de cualquier tipo, manteniendo la seguridad de tipos.

**Tarea**: Crea la clase `Contenedor<T>` en el paquete `genericos.ejercicio1`.

Imagina que necesitas una caja de almacenamiento. Si la defines como `Contenedor<String>`, Java solo te dejará guardar textos. Si es `Contenedor<Integer>`, solo números. Esto evita errores de conversión en el futuro.

- **Atributos**: Un atributo privado llamado `contenido` de tipo `T`.
- **Métodos**:
    - **Constructor**: Debe inicializar el contenedor con un objeto.
    - `void guardar(T objeto)`: Permite cambiar el objeto almacenado por uno nuevo del mismo tipo.
    - `T extraer()`: Recupera el objeto almacenado. Fíjate que el tipo de retorno es `T`.
    - `boolean estaVacio()`: Comprueba si el contenedor no tiene nada (si el atributo es `null`).

---

## Ejercicio 2: Intercambio de Posiciones (Método Genérico)

??? note "Concepto: Métodos Genéricos"

    Un método puede ser genérico sin necesidad de que la clase que lo contiene lo sea. Para ello, debemos declarar el parámetro de tipo `<T>` **justo antes** del tipo de retorno del método.
    
    **Sintaxis:**
    ```java
    public static <T> void miMetodo(T parametro) { ... }
    ```

**Objetivo**: Implementar una utilidad que funcione para cualquier tipo de array de objetos sin tener que repetir el código para cada tipo (String, Integer, etc.).

**Tarea**: Crea una clase `UtilidadesArray` con un método estático llamado `intercambiar`.

Seguro que alguna vez has tenido que intercambiar la posición de dos elementos en un array usando una variable temporal. Con genéricos, puedes escribir ese algoritmo **una sola vez** y que sirva para cualquier array de objetos. 

*Nota: Recuerda que los genéricos no funcionan con tipos primitivos (int, double), solo con sus clases envoltorio (Integer, Double).*

- **Firma**: `public static <T> void intercambiar(T[] array, int i, int j)`
- **Lógica**: Debe tomar el elemento en la posición `i` y ponerlo en la `j`, y viceversa, usando una variable temporal de tipo `T`. Si los índices están fuera de rango, el método no debe hacer nada.

---

## Ejercicio 3: Pares de Datos con Records (Multitipo)

??? note "Concepto: Múltiples Parámetros de Tipo"

    A veces necesitamos trabajar con más de un tipo genérico a la vez. Por convención se suelen usar letras como `K` (Key) y `V` (Value). Desde Java 14, los `records` son excelentes para definir estructuras genéricas inmutables de forma muy compacta.

**Objetivo**: Aprender a gestionar dos tipos genéricos distintos en una misma estructura inmutable.

**Tarea**: Crea un `record` llamado `Par<K, V>` en el paquete `genericos.ejercicio3`.

A menudo necesitamos agrupar dos datos relacionados que no tienen por qué ser del mismo tipo. Por ejemplo: un nombre (`String`) y una edad (`Integer`), o un producto y su stock. En lugar de crear una clase para cada combinación, usamos `K` (Key) y `V` (Value) para representar cualquier par de datos.

- **Componentes del Record**: Una clave `first` de tipo `K` y un valor `second` de tipo `V`.
- **Método Adicional**: Sobrescribe el método `toString()` para que devuelva un formato legible como: `"(Clave): Valor"`.

---

## Ejercicio 4: Limitando el Tipo (Bounded Generics)

??? note "Concepto: Tipos Delimitados (Extends)"

    A veces no queremos que una clase acepte **cualquier** tipo, sino solo aquellos que heredan de una clase base o implementan una interfaz. Esto se hace con `<T extends ClaseBase>`.

**Objetivo**: Restringir el tipo genérico para que solo acepte clases que tengan un comportamiento determinado (en este caso, comportamiento numérico).

**Tarea**: Crea una clase `CalculadoraEstadistica<T extends Number>`.

Si queremos hacer una calculadora que sume elementos, no tiene sentido que nos pasen una lista de `String` o `Gato`. Usando `extends Number`, le decimos a Java: "Acepto cualquier tipo `T`, siempre y cuando sea un número (`Integer`, `Double`, etc.)". Gracias a esto, Java nos permite usar métodos de la clase `Number`.

- **Atributos**: Una lista (`List<T>`) de elementos llamados `valores`.
- **Métodos**:
    - `void agregar(T valor)`: Añade un elemento a la lista.
    - `double sumarTodos()`: Recorre la lista y suma todos los elementos. 
    - **Pista**: Como `T` hereda de `Number`, puedes llamar a `valor.doubleValue()` para obtener el valor numérico y poder sumarlo, sea cual sea el tipo original.

---

## 🛠️ Cómo validar
Para probar estos ejercicios, crea una clase `Main` y trata de instanciar las clases con diferentes tipos (por ejemplo, un contenedor de `Integer` y otro de `String`). Si intentas meter un `String` en un contenedor de `Integer`, el compilador debería darte un error antes de ejecutar el programa.
