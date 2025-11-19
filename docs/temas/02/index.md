# **UT2. Elementos Básicos de Java**

!!! tip "Información de la unidad"

    === "Contenidos"

        Fundamentos de la Programación Orientada a Objetos (POO):

        - Conceptos básicos: objeto, clase, encapsulamiento, herencia (introducción).
        - Instanciación de objetos a partir de clases predefinidas.
        - Uso de métodos y propiedades de objetos.
        - Llamadas a métodos estáticos.
        - Manejo de parámetros en métodos.
        - Incorporación y uso de librerías de objetos.
        - Uso de constructores.
        - Creación y compilación de programas simples en el IDE.

    === "Propuesta didáctica"

        En esta unidad vamos a comenzar a trabajar el **RA2: Escribe y prueba programas sencillos, reconociendo y aplicando los fundamentos de la programación orientada a objetos.**

        Criterios de evaluación clave que abordaremos:

        - **CE2a**: Se han identificado los fundamentos de la programación orientada a objetos.
        - **CE2b**: Se han escrito programas simples.
        - **CE2c**: Se han instanciado objetos a partir de clases predefinidas.
        - **CE2d**: Se han utilizado métodos y propiedades de los objetos.
        - **CE2e**: Se han escrito llamadas a métodos estáticos.
        - **CE2f**: Se han utilizado parámetros en la llamada a métodos.
        - **CE2g**: Se han incorporado y utilizado librerías de objetos.
        - **CE2h**: Se han utilizado constructores.
        - **CE2i**: Se ha utilizado el entorno integrado de desarrollo en la creación y compilación de programas simples.

    === "Planificación de Aula"

        Esta unidad se imparte en la **primera evaluación**, con una duración estimada de **6 sesiones lectivas**, aproximadamente entre la **1ª y 2ª semana de octubre de 2025**.

        | Sesión | Contenidos                                                                    | Criterios trabajados   |
        | ------ | ----------------------------------------------------------------------------- | ---------------------- |
        | 1      | Fundamentos de POO: objeto, clase, encapsulamiento. Instanciación de objetos. | CE2a, CE2c             |
        | 2      | Uso de métodos y propiedades de objetos predefinidos.                         | CE2d                   |
        | 3      | Llamadas a métodos estáticos. Uso de parámetros en métodos.                   | CE2e, CE2f             |
        | 4      | Incorporación y uso de librerías de objetos.                                  | CE2g                   |
        | 5      | Uso de constructores. Creación y compilación de programas simples.            | CE2h, CE2i             |
        | 6      | Práctica guiada y autónoma: manipulación de objetos simples y librerías.      | CE2b, CE2c, CE2d, CE2g |

En esta sección se abordan los conceptos fundamentales para comenzar a programar en Java, desde la estructura básica de un programa hasta el uso de variables, constantes y operaciones.

## **2.1. Del Código Fuente a la Ejecución**

El ciclo de vida de un programa Java sigue un proceso claro:

1.  **Código Fuente**: Es el archivo de texto con extensión `.java` que escribe el programador.
2.  **Compilador**: El código fuente es procesado por el compilador de Java.
3.  **Bytecode**: El compilador traduce el código a este formato intermedio, que es independiente de la plataforma.
4.  **Ejecución**: La Máquina Virtual de Java (JVM) interpreta y ejecuta el Bytecode, produciendo la **salida** final del programa.

Durante la programación, se manejan valores básicos como los booleanos `true` y `false`, y el valor especial `null`, que representa la ausencia de un valor

## **2.2. Comentarios**

Los comentarios son textos dentro del código fuente que **el compilador ignora por completo** [1]. Su función es puramente informativa, permitiendo al programador dejar notas, explicaciones sobre algoritmos complejos o recordatorios para sí mismo o para otros desarrolladores que trabajen en el mismo código [1].

**Ejemplos de comentarios:**

```java
// Esto es un comentario de una sola línea.

/*
  Esto es un comentario
  de múltiples líneas. Se usa para
  explicaciones más extensas.
*/
int edad = 30; // También se puede comentar al final de una línea de código.
```

## 2.3. Variables

Una variable es un concepto central en la programación.

### 2.3.1. Definición

Una variable es una **porción de memoria a la que se le asigna un nombre (identificador)**. Su propósito es almacenar un valor que puede ser utilizado y modificado a lo largo de la ejecución del programa.

### 2.3.2. Reglas para Nombrar Variables

Los identificadores de las variables en Java deben seguir unas reglas específicas:

- Deben comenzar por una **letra (preferiblemente minúscula), el carácter de guion bajo (\*\***\_\***\*) o el símbolo de dólar (\*\***$\***\*)**.

- Después del primer carácter, pueden contener una combinación de **letras, dígitos, guiones bajos o símbolos de dólar**.

- Java es **Case Sensitive**, lo que significa que distingue entre mayúsculas y minúsculas.

- No existe una longitud máxima para el nombre de una variable.

**Ejemplos:**

- **Nombres válidos**: `edad`, `maxValor`, `notaMediaTrimestre`, `_usuario`, `$departamento`.

- **Nombres no válidos**: `1numero` (no puede empezar por un dígito), `nota-media` (no se permite el guion medio), `class` (es una palabra reservada del lenguaje).

### 2.3.3. Tipos de Datos y Declaración

Para usar una variable, primero se debe declarar, indicando su tipo de dato seguido de su nombre. Java ofrece diversos tipos de datos para distintos propósitos, y un mismo valor podría ser válido para varios tipos. Es importante destacar que `String` **no es un tipo de dato primitivo** como los numéricos.

**Ejemplo de declaración:**

```
// Declaración de variables de diferentes tipos
int numeroDeEstudiantes;
double precioProducto;
boolean esValido;
char inicial;
String nombreUsuario;

```

### 2.3.4. Valores Literales

Los valores literales son valores fijos escritos directamente en el código. Java los interpreta de la siguiente manera:

- Un valor entre **comillas dobles (\*\***"\***\*)** es un `String`. Ejemplo: `"Hola Mundo"`.

- Un valor entre **comillas simples (\*\***'\***\*)** es un `char`. Ejemplo: `'A'`.

- Un valor sin comillas es interpretado como **numérico**.

- Por defecto, un número entero es de tipo `int`. Para indicar que es de tipo `long`, se añade una `l` al final. Ejemplo: `long numeroGrande = 1234567890L;`.

- Por defecto, un número con decimales es de tipo `double`. Ejemplo: `3.14159`.

### 2.3.5. Valores por Defecto

Si se declara una variable pero no se le asigna un valor inicial, Java le asigna uno por defecto:

- **Tipos numéricos**: `0`.

- **Tipo** **boolean**: `false`.

- **Tipo** **char**: valor nulo (`null`).

- **Tipos de objeto** (como `String`): `null`.

### 2.3.6. Ámbito de las Variables

El ámbito se refiere al **bloque de código en el que una variable es accesible y puede ser utilizada**. La regla general es que una variable solo existe desde el punto donde se declara hasta el final de su bloque.

- **Variables globales**: Se declaran al inicio del programa, perduran durante toda su ejecución y pueden ser usadas desde cualquier parte del mismo.

- **Variables locales**: Se declaran dentro de un bloque específico y solo existen y pueden ser utilizadas dentro de ese bloque.

**Ejemplo de ámbito:**

```
public class EjemploAmbito {
    static int variableGlobal = 10; // Variable global (de clase)

    public static void miMetodo() {
        int variableLocal = 20; // Variable local al método
        System.out.println(variableGlobal); // Válido: Se puede acceder a la global
        System.out.println(variableLocal); // Válido: Se puede acceder a la local
    }

    public static void main(String[] args) {
        miMetodo();
        System.out.println(variableGlobal); // Válido
        // System.out.println(variableLocal); // ERROR: No se puede acceder a variableLocal aquí
    }
}

```

## 2.4. Conversión de Tipos (Casting)

Generalmente, es posible convertir un valor de un tipo a otro, siempre que la conversión sea lógica.

- **Conversión implícita**: Java la realiza automáticamente cuando la conversión es trivial.

- **Conversión explícita (casting)**: Se debe forzar cuando puede haber pérdida de información, indicando el nuevo tipo entre paréntesis. Solo funciona entre tipos compatibles.

```
double precio = 99.99;
int precioEntero = (int) precio; // Casting explícito, precioEntero ahora es 99

```

### **2.5. Constantes**

Una constante es una variable cuyo valor **no puede ser modificado una vez asignado**. Se usan para definir valores fijos, evitando el uso de "números mágicos" y facilitando su mantenimiento. La convención para nombrarlas es usar **mayúsculas y guiones bajos**.

**Ejemplo de declaración de constante:**

```
// La palabra clave 'final' convierte una variable en constante.
final int DIAS_SEMANA = 7;
final double IVA_GENERAL = 0.21;

```

## **2.6. Convenciones de Nombres (CamelCase)**

Java utiliza la convención **CamelCase** para nombrar identificadores:

- **Clases**: La primera letra de cada palabra va en mayúscula (`UpperCamelCase`). Ejemplos: `App`, `LectorArchivos`, `InterfazGrafica`.

- **Métodos y Variables**: La primera letra de la primera palabra va en minúscula, y la primera de las siguientes en mayúscula (`lowerCamelCase`).

◦ **Métodos**: `sumarNumeros()`, `importarTexto()`.

◦ **Variables**: `radio`, `resultadoSuma`.

## **2.7. Operaciones Básicas**

Java soporta varios tipos de operadores para realizar cálculos y comparaciones:

- Operaciones **aritméticas**.

- Operaciones **relacionales**.

- Operaciones **lógicas**.

- Operaciones de **asignación**.

## **2.8. Clases Fundamentales de la API de Java**

La API de Java proporciona clases útiles para tareas comunes.

### 2.8.1. Clase\*\* \*\*Scanner

Permite **leer datos introducidos por el usuario** a través de la consola, por ejemplo, para almacenarlos en una variable.

**Ejemplo de uso:**

```
import java.util.Scanner; // Es necesario importarla

public class LeerEntrada {
    public static void main(String[] args) {
        Scanner teclado = new Scanner(System.in);
        System.out.print("Introduce tu edad: ");
        int edad = teclado.nextInt();
        System.out.println("Tu edad es: " + edad);
        teclado.close();
    }
}

```

### 2.8.2. Clase **Random**

Facilita la **generación de números pseudoaleatorios** de forma más versátil que `Math.random()`, permitiendo crear enteros, booleanos o números dentro de un rango.

**Ejemplo de uso:**

```
import java.util.Random; // Es necesario importarla

public class GenerarAleatorio {
    public static void main(String[] args) {
        Random generador = new Random();
        int numeroAleatorio = generador.nextInt(10); // Genera un entero entre 0 y 9
        System.out.println("Número aleatorio: " + numeroAleatorio);
    }
}

```

### 2.8.3. Clase **Math**

Proporciona **métodos estáticos para operaciones matemáticas** comunes, como raíces cuadradas, potencias, redondeos, etc..

**Ejemplo de uso:**

```
public class OperacionesMath {
    public static void main(String[] args) {
        double raizCuadrada = Math.sqrt(16.0); // raizCuadrada es 4.0
        double potencia = Math.pow(2, 3); // potencia es 8.0 (2 elevado a 3)
        System.out.println("La raíz cuadrada de 16 es " + raizCuadrada);
        System.out.println("2 elevado a 3 es " + potencia);
    }
}

```

## **2.9. Sentencias Básicas**

Además de los elementos anteriores, la programación en Java se estructura a través de **sentencias básicas** que definen el flujo y las acciones del programa
