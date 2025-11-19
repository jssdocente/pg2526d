# **UT4 Programación modular**

!!! tip "Información de la unidad"

    === "Contenidos"

        Modularización y funciones/métodos:

        - Concepto de función/método, parámetros y valor de retorno.
        - Declaración y llamada a funciones/métodos.
        - Ámbito de variables (local y global).
        - Sobrecarga de funciones/métodos.
        - Paso de argumentos por valor y por referencia.
        - Depuración y testeo de funciones.
        - Documentación de funciones.

    === "Propuesta didáctica"

        En esta unidad vamos a profundizar en aspectos del **RA3: Escribe y depura código, analizando y utilizando las estructuras de control del lenguaje**, aplicado a la modularización de programas. Aunque no tiene un RA propio en tus datos, la modularidad es clave para organizar el código y se relaciona con la depuración y estructuración.

        Criterios de evaluación clave que abordaremos (con énfasis en la modularidad):

        - **CE3e**: Se han creado programas ejecutables utilizando diferentes estructuras de control (aplicado a funciones).
        - **CE3f**: Se han probado y depurado los programas (especialmente funciones).
        - **CE3g**: Se ha comentado y documentado el código (documentación de funciones).

La **programación modular** es un paradigma que consiste en dividir un programa grande y complejo en partes más pequeñas, manejables e independientes, llamadas **módulos**. Cada módulo se encarga de una tarea específica. En un lenguaje de programación, estos módulos se implementan como **funciones** y **procedimientos**.

Las ventajas que ofrece la programación modular son:

- Facilita la resolución del problema.
- Aumenta la claridad y legibilidad de todo el programa.
- Permite que varios programadores trabajen en el mismo proyecto.
- Reduce el tiempo de desarrollo ya que se pueden reutilizar esos módulos en varios programas.
- Aumenta la fiabilidad porque es más sencillo diseñar y depurar módulos y el mantenimiento en mas fácil.

La descomposición modular se basa en la técnica “Divide y Vencerás” (DAC o Divide And Conquer), esta técnica tiene dos pasos:

- Identificación de los subproblemas y construcción de los módulos que lo resuelven.
- Combinación de los módulos para resolver el problema original.

<figcaption>
    <img src="./images/funciones.jpg" width="100%" align="center"/>
    <figcaption align="center">Concepto de función</figcaption>
</figcaption>

## 4.1 Funciones y Procedimientos

### Funciones

Una **función** es un bloque de código que realiza una tarea específica y devuelve un valor. Las funciones pueden recibir datos de entrada (argumentos) y siempre devuelven un resultado mediante la sentencia `return`.

!!! tipo "Pseucódigo DAW"

    Ejemplo en pseudocódigo de la sintáxis de una función:

    ```csharp
    function int sumar(int a, int b) {
        // Esta función toma dos enteros como argumentos y devuelve su suma.
        return a + b; // Devuelve la suma de a y b
    }

    Main {
        // Llamamos a la función sumar y almacenamos el resultado en la variable resultado
        int resultado = sumar(5, 3);
        System.out.println("La suma es: " + resultado); // Imprime "La suma es: 8"
    }
    ```

En java la sintaxis es diferente:

```java
public static int sumar(int a, int b) {
    return a + b;
}

public static void main(String[] args) {
    int resultado = sumar(5, 3);
    System.out.println("La suma es: " + resultado);
}
```

> En Java no se usa la palabra reservada `function` como usan en otros lenguajes.<br>
> La palabra `static` es necesaria si la función se declara dentro de un contexto `static`.

Compresión de la sintáxis:

- public: indica que accesible desdel el exterior de esta contexto (la clase), un modificador de acceso. Es opcional.
- static: indica si se puede acceder directamente sin estar vinculado a una instancia espécifica. Es opcional.
- nombre: indica el nombre de la función. A través del nombre es como se va a poder llamar o utilizar la función.
- {int}: indica el tipo de datos de retorno.
- Lo que va dentro de los paréntesis son los parámetros o información que se puede pasar a la función.

<figcaption>
    <img src="./images/function-structure.png" width="100%" align="center"/>
    <figcaption align="center">Estructura de una función</figcaption>
</figcaption>

### Procedimientos

Un **procedimiento** es similar a una función, pero no devuelve ningún valor. Se utiliza para ejecutar una serie de instrucciones que realizan una tarea específica. Los procedimientos pueden recibir datos de entrada (argumentos) pero no tienen una sentencia `return`.

!!! tipo "Pseucódigo DAW"

    ```csharp
    procedure saludar(string nombre) {
        // Este procedimiento toma un nombre como argumento y muestra un saludo personalizado.
        System.out.println("Hola, " + nombre + "! Bienvenido al programa.");
    }

    Main {
        // Llamamos al procedimiento saludar con el nombre "Ana"
        saludar("Ana"); // Imprime "Hola, Ana! Bienvenido al programa."
    }
    ```

En java la sintaxis es diferente:

```java
public void saludar(string nombre) {
    System.out.println("Hola, " + nombre + "! Bienvenido al programa.");
}

public static void main(String[] args) {
    int resultado = sumar(5, 3);
    saludar("Ana"); // Imprime "Hola, Ana! Bienvenido al programa."
}
```

> 💡 La diferencia principal está en la utilización de `void` que indica que no devuelve ningún valor.<br>
> En java, la diferencia entre una `función y un procedmimiento` radica en si devuelve o no valores.

### Parámetros y Argumentos

Los **parámetros** son las variables que se definen en la declaración de una función o procedimiento. Actúan como "marcadores de posición" para los valores que se pasarán cuando se llame a la función o procedimiento.

Los **argumentos** son los valores reales que se pasan a la función o procedimiento cuando se llama. Estos valores se asignan a los parámetros correspondientes.

```csharp
int multiplicar(int x, int y) {
    // x e y son los parámetros de la función
    return x * y; // Devuelve el producto de x e y
}

public static void main(String[] args) {
    // 4 y 5 son los argumentos que se pasan a la función
    int resultado = multiplicar(4, 5);

    System.out.println("El producto es: " + resultado); // Imprime "El producto es: 20"
}
```

#### La Regla de la Exactitud de Tipos

En el Lenguaje Java, se aplica una política **estricta** de **coincidencia de tipos** para garantizar la seguridad y previsibilidad del código. Esto significa que el tipo de cada argumento pasado debe **coincidir exactamente** con el tipo de su parámetro correspondiente.

**Conversiones (Ampliación o Estrechamiento)**

Si un argumento es de un tipo diferente al parámetro, se considera un error a menos que se use un **_casting_ explícito** para forzar la conversión. Esta regla se aplica incluso en las llamadas "conversiones de ampliación" donde no hay pérdida de datos (por ejemplo de entero a decimal, en este caso no lo permitiremos aunque haya lenguajes que sí, solo por fines didácticos), forzando al programador/a a ser consciente de la transformación de datos.

- Conversiones por ampliación (de un tipo "más pequeño" a uno "más grande"): `int` a `decimal` (no permitido sin _casting_ explícito).
- Conversiones por estrechamiento (de un tipo "más grande" a uno "más pequeño"): `decimal` a `int` (no permitido sin _casting_ explícito).

Estas conversiones pueden provocar pérdida de datos y, por lo tanto, siempre requieren _casting_ explícito.

| Parámetro Esperado | Argumento Pasado | ¿Válido? | Acción Requerida                                     |
| :----------------: | :--------------: | :------: | :--------------------------------------------------- |
|     `decimal`      |      `int`       |  **NO**  | Requiere **Casting Explícito**: `(decimal)mi_entero` |
|       `int`        |    `decimal`     |  **NO**  | Requiere **Casting Explícito**: `(int)mi_decimal`    |
|       `int`        |      `int`       |  **SÍ**  | Paso Directo (Coincidencia Exacta)                   |
|     `decimal`      |    `decimal`     |  **SÍ**  | Paso Directo (Coincidencia Exacta)                   |
|      `string`      |     `string`     |  **SÍ**  | Paso Directo (Coincidencia Exacta)                   |
|      `string`      |      `int`       |  **NO**  | Requiere **Casting Explícito**: `(string)mi_char`    |

```csharp
decimal calcularMedia(decimal a, decimal b) {
    return (a + b) / 2.0;
}

Main {
    int nota1 = 7;
    int nota2 = 8;

    // ERROR: Se espera decimal, se pasa int. Requiere casting.
    // decimal resultado = calcularMedia(nota1, nota2);

    // SOLUCIÓN: Usar casting explícito para forzar la conversión segura
    decimal resultado = calcularMedia((decimal)nota1, (decimal)nota2);
    System.out.println("Media: " + resultado); // Imprime "Media: 7.5"
}
```

**Resumen de la Regla de la Exactitud de Tipos**

La exactitud con los tipos de datos que pasas a una función (`int`, `string`, `decimal`, etc.) permite que tu código sea **seguro, limpio y fácil de entender** desde el principio.

<u>Regla de Oro: Control Total sobre la Transformación (Tipos Diferentes)</u>

Si pasas un dato de un tipo a otro (ej. de `int` a `decimal`, o de `string` a `int`), **siempre** debes usar el **`Casting` Explícito** (`(tipo)valor`). La regla es que los tipos deben coincidir exactamente, y si no lo hacen, **tú tienes que forzar la conversión**.

| Escenario de Conversión                                            | ¿Por qué te obligamos al `casting`?                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| :----------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Transformación Segura** (`int` a `decimal`)                      | Queremos que **seas consciente** de que la forma en que se guarda el dato en la memoria cambia. Al usar `(decimal)mi_entero`, confirmas tu intención de transformar el dato, incluso si no hay riesgo de pérdida. Realmente esta sería una transformación segura por ampliación en casi todos los lenguajes. Queremos que seas consciente de que la forma en que se guarda el dato cambia. Al escribir (decimal)5, confirmas que quieres que el 5 se convierta en 5.0. ¡No queremos magia, queremos control! |
| **Transformación Peligrosa** (`decimal` a `int`, `string` a `int`) | Hay un alto riesgo de **pérdida de información** (ej. quitar decimales) o un **fallo** (ej. si el `string` es "hola"). El _casting_ te obliga a **asumir la responsabilidad** de que el valor pueda ser corrupto o causar un error. en definitiva, estás haciendo una Conversión Peligrosa (estrechamiento). Quitas los decimales (truncamiento). Esto es una pérdida de datos y no suele estar permitida por los lenguajes. El casting obligatorio te obliga a asumir la responsabilidad de esa pérdida.    |

**Conclusión:** El _casting_ explícito siempre es tu herramienta para **controlar** y **documentar** cualquier cambio en el formato de los datos. Si los tipos no son idénticos, ¡el control es tuyo! Es una cuestión de disciplina y claridad.

### 👉 Paso por valor y paso por referencia

<iframe width="560" height="315" src="https://www.youtube.com/embed/e6YyiZyE2O4" title="Programación Estructurada y Modular. Paso por valor o referencia" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Esta es una de las distinciones más importantes sobre cómo se pasan los argumentos a los módulos:

- **Paso por valor (por defecto)**: Cuando pasas un argumento por valor, la función recibe una **copia** del dato original. Cualquier modificación que hagas al parámetro dentro de la función **no afectará a la variable original** fuera de ella. Este es el comportamiento por defecto en la mayoría de los lenguajes de programación.

```csharp
  public int incrementar(int numero) {
      // Esta función recibe una copia del valor original.
      numero = numero + 1; // Incrementa la copia, no el original.
      return numero; // Devuelve el valor incrementado.
  }

  Main {
      var valorOriginal = 10;

      System.out.println("Valor original antes de la función: " + valorOriginal); // Imprime 10

      var nuevoValor = incrementar(valorOriginal);

      System.out.println("Valor devuelto por la función: " + nuevoValor); // Imprime 11
      System.out.println("Valor original después de la función: " + valorOriginal); // Sigue imprimiendo 10
  }
```

- **Paso por referencia**: Cuando pasas un argumento por referencia, en lugar de una copia, la función recibe la **dirección de memoria** de la variable original. Esto significa que cualquier cambio que hagas al parámetro dentro de la función **modificará directamente la variable original**.

//Crear ejemplo java de paso por referencia

    ```csharp
    void duplicar(int numero) {
        // Esta función recibe una referencia al valor original.
        numero = numero * 2; // Modifica directamente el valor original.
    }

    Main {
        String valorOriginal = 10;

        System.out.println("Valor original antes de la función: " + valorOriginal); // Imprime 10
        duplicar(valorOriginal); // Pasamos la variable por referencia
        System.out.println("Valor original después de la función: " + valorOriginal); // Ahora imprime 20
    }
    ```

    ![Paso por valor y paso por referencia](./images/parametros.gif)

En Java, el mecanismo de paso de parámetros a los métodos es a menudo una fuente de confusión:

1.  **Tipos Primitivos (como `int`, `boolean`, `double`, etc.):** Se pasan **siempre por valor**. Se crea una copia del valor de la variable original y esa copia es la que se pasa al método. Cualquier modificación dentro del método afecta solo a esa copia local, no a la variable original. \*
2.  **Objetos (Tipos de Referencia, como instancias de clases):** Se pasan **por valor de la referencia** (a veces llamado "paso por copia de la referencia" o _pass-by-value of the reference_). Esto significa:

    - La variable que se pasa al método **contiene la dirección de memoria** (la referencia) donde reside el objeto.
    - El método recibe una **copia de esta referencia** (la dirección de memoria).
    - Si usas esta copia de la referencia dentro del método para **modificar el estado interno del objeto** (por ejemplo, llamar a un _setter_), el objeto original **sí** se verá afectado, porque ambas referencias apuntan al mismo objeto en la memoria _heap_.
    - Sin embargo, si dentro del método **reasignas la variable de referencia** a un _nuevo_ objeto, esto solo cambiará la referencia local dentro del método; la referencia original en el código llamador **permanecerá sin cambios**, apuntando al objeto original.

> 💡 Un `String` no es un objeto primitivo y por tanto se pasa por _referencia_. Además los `String` son inmutables. No se pueden modificar, si queremos modificar se necesita crear una copia de ellos.

## 4.2 Ámbito de las variables

El **ámbito** (o alcance) de una variable determina dónde puede ser accedida o modificada dentro del código.

- **Ámbito global**: Las variables declaradas fuera de cualquier función o procedimiento tienen ámbito global. Esto significa que pueden ser accedidas y modificadas desde cualquier parte del programa, incluyendo dentro de funciones y procedimientos. Sin embargo, abusar de las variables globales puede llevar a código difícil de mantener y depurar, ya que cualquier parte del programa puede cambiar su valor. **Usa las variables globales con moderación y solo cuando sea absolutamente necesario, aunque es mejor no hacerlo**.

- **Ámbito local**: Las variables declaradas dentro de una función o procedimiento tienen ámbito local. Solo pueden ser accedidas y modificadas dentro de ese bloque específico. Esto ayuda a evitar conflictos de nombres y hace que el código sea más modular y fácil de entender.

```csharp
int contadorGlobal = 0; // Variable global, OJO que puedes suspender por usarlas, porque siempre se puede usar paso por referencia si lo necesitas

void incrementarContador() {
    int contadorLocal = 0; // Variable local

    contadorLocal = contadorLocal + 1; // Incrementa la variable local
    contadorGlobal = contadorGlobal + 1; // Incrementa la variable global

    System.out.println("Contador local: " + contadorLocal); // Siempre imprimirá 1
    System.out.println("Contador global: " + contadorGlobal); // Incrementa cada vez que se llama a la función
}

Main {
    incrementarContador(); // Llama a la función
    incrementarContador(); // Llama a la función de nuevo
    incrementarContador(); // Llama a la función de nuevo
}
```

Es importante entender que puedes tener variables que se llamen igual en diferentes ámbitos (una en Main y otra local, ya sea función/procedimiento y bucle, o if), pero son variables completamente distintas. La variable local "oculta" a la global dentro de su ámbito.

```csharp
int valor = 100; // Variable global

void mostrarValor() {
    int valor = 50; // Variable local, oculta a la global
    System.out.println("Valor dentro de la función: " + valor); // Imprime 50
}

Main {
    int valor = 75; // Variable local en Main, oculta a la global

    System.out.println("Valor en Main: " + valor); // Imprime 75

    mostrarValor(); // Llama a la función que imprime 50

    System.out.println("Valor global: " + valor); // Imprime 75, la variable global sigue siendo 100
    System.out.println("Valor global accedido directamente: " + 100);

    if (valor > 50) {
        int valor = 25; // Variable local en el if, oculta a la de Main

        // Para evitar estas cosas nombra bien tus variables y no repitas nombres
        System.out.println("Valor dentro del if: " + valor); // Imprime 25
    }

    System.out.println("Valor en Main después del if: " + valor); // Imprime

}
```

## 4.3 Sobrecarga de funciones y procedimientos

La **sobrecarga** permite definir múltiples funciones o procedimientos con el mismo nombre, pero con diferentes listas de parámetros (diferente número o tipos de parámetros). El compilador determina cuál función llamar en función de los argumentos proporcionados.

```csharp
int calcularArea(int lado) {
    // Calcula el área de un cuadrado
    return lado * lado;
}

decimal calcularArea(decimal radio) {
    // Calcula el área de un círculo
    return 3.1416 * radio * radio;
}

Main {
    int areaCuadrado = calcularArea(5); // Llama a la función para cuadrado
    decimal areaCirculo = calcularArea(3.5); // Llama a la función para círculo

    System.out.println("Área del cuadrado: " + areaCuadrado); // Imprime 25
    System.out.println("Área del círculo: " + areaCirculo); // Imprime aproximadamente 38.4846
}
```

En cualquier caso, la sobrecarga debe usarse con moderación para evitar confusiones. Asegúrate de que las funciones sobrecargadas tengan una lógica clara y distinta para que su uso sea intuitivo. Si no siempre podrás usar nombres diferentes para cada función o procedimiento o usar parámetros opcionales o por defecto y nombrados para evitar tanto su uso.

!!! tip "Parámetros de salida (no disponible en Java)"

    Permiten que una función o procedimiento devuelva múltiples valores.

    En otros lenguajes diferentes a Java, existe el concepto de parámetros de salida, y estos se indican con la palabra reservada `out` o `ref`.

    <u>*¿Cómo funcionan?*</u>

    Se declaran con la palabra clave `out` y deben ser asignados dentro de la función antes de que esta termine. Al llamar a la función, no es necesario inicializar las variables que se pasan como parámetros de salida.

    ¿Que diferencia hay en usar `out` o `ref`? Que con `ref` la variable debe estar inicializada antes de pasarla a la función, mientras que con `out` no es necesario inicializarla, pero dentro de la función debe ser asignada antes de salir de ella.

    ```csharp
    function void obtenerDatos(out string nombre, out int edad) {
        // Asignamos valores a los parámetros de salida
        nombre = "Ana";
        edad = 25;
    }

    Main {
        string nombre;
        int edad;

        //Permiten devolver más de un valor
        obtenerDatos(out nombre, out edad);

        System.out.println("Nombre: " + nombre);
        System.out.println("Edad: " + edad);
    }
    ```

    Aquí la diferencia con `ref` es que no es necesario inicializar `nombre` y `edad` antes de pasarlas a la función, pero dentro de la función deben ser asignadas antes de que esta termine.

    ```csharp
    function void obtenerDatos(ref string nombre, ref int edad) {
        // Asignamos valores a los parámetros de referencia
        nombre = "Ana";
        edad = 25;
    }

    Main {
        string nombre = ""; // Debe estar inicializada
        int edad = 0; // Debe estar inicializada
        obtenerDatos(ref nombre, ref edad);
        System.out.println("Nombre: " + nombre);
        System.out.println("Edad: " + edad);
    }
    ```

    **Paso por Referencia vs. Paso por Salida (`ref` vs. `out`)**

    Tanto `ref` como `out` son mecanismos de **Paso por Referencia**, lo que significa que en lugar de pasar una copia del valor, la función trabaja directamente con la **variable original** en la memoria, permitiendo que el módulo modifique su valor.

    Sin embargo, **no son sinónimos**. La diferencia clave está en su **propósito** (semántica) y, lo más importante, en el **Control de Compilación** que imponen para garantizar la seguridad.

    | Característica                 | 🔑 **`ref` (Referencia)**                                                                     | 🔑 **`out` (Salida)**                                                                                                      |
    | :----------------------------- | :------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------ |
    | **Flujo de Datos**             | **Entrada y Salida (I/O)**. Modifica un valor existente.                                     | **Solo Salida (O)**. Se usa para obtener un resultado.                                                                    |
    | **Inicialización (en `Main`)** | El argumento **DEBE estar inicializado** antes de la llamada.                                | El argumento **NO necesita estar inicializado**.                                                                          |
    | **Asignación (en Función)**    | La función **puede** leer y modificar el parámetro, pero **no está obligada** a modificarlo. | La función **DEBE asignar un valor** antes de finalizar su ejecución. **¡Es una obligación impuesta por el compilador\!** |
    | **Concepto Clave**             | Modificar un valor existente.                                                                | Devolver uno o varios resultados adicionales.                                                                             |



    ##### 1\. Uso de `ref`: Modificar una Entrada Existente

    `ref` se utiliza cuando el **valor inicial del argumento es importante** para la función, la cual lo usará como entrada y luego, opcionalmente, lo modificará.

    ```csharp
    // 'saldo' entra valiendo 100, y sale valiendo 150.
    procedure depositar(ref decimal saldo, decimal cantidad) {
        // La función LEYÓ el valor de 'saldo' (100) para calcular la suma.
        saldo = saldo + cantidad;
    }

    Main {
        decimal miCuenta = 100.0m; // DEBE estar inicializada
        depositar(ref miCuenta, 50.0m);
        // miCuenta ahora vale 150.0m.
    }
    ```

    **Regla esencial:** La variable `miCuenta` **debe** haber sido inicializada antes de la llamada, pues la función `depositar` necesita leer su valor inicial.

    ##### 2\. Uso de `out`: Garantizar un Resultado Obligatorio

    `out` se utiliza para **devolver resultados adicionales** y es la forma principal de devolver múltiples valores. Es superior a `ref` en este contexto porque el compilador lo supervisa:

    * **Semántica (Intención):** Al usar `out`, aclaras que esa variable solo se usará para obtener un resultado, ignorando si tenía un valor inicial en el `Main`.
    * **Control de Compilación (Seguridad):** La regla de `out` obliga a que **todos los caminos posibles** dentro de la función asignen un valor a ese parámetro. Esto previene que la función termine sin haber devuelto un resultado válido.

    ```csharp
    // 'resultado' solo sirve como contenedor para DEVOLVER un valor.
    function bool intentarDividir(int num, int den, out decimal resultado) {
        if (den == 0) {
            resultado = 0.0m; // Asignación obligatoria 1
            return false;
        }
        resultado = (decimal)num / den; // Asignación obligatoria 2
        return true;
    }

    Main {
        decimal division; // No es necesario inicializarla

        bool exito = intentarDividir(10, 2, out division);
        // division ahora vale 5.0m.
    }
    ```

    **Contraejemplo: La Obligación de Asignación de `out`**

    Si no asignas un valor al parámetro `out` en **todos los flujos de control** (por ejemplo, en un `if` pero no en el `else`), el compilador generará un error, incluso si la variable tuviera un valor inicial en el `Main` (ese valor inicial es ignorado).

    **El siguiente código produciría un ERROR DE COMPILACIÓN:**

    ```csharp
    // ¡ERROR! Si 'dato' es menor que 10, la función sale sin asignar 'mensaje'.
    function bool procesarDatos(int dato, out string mensaje) {
        if (dato > 10) {
            mensaje = "Dato procesado con éxito."; // Asignación OK
            return true;
        }

        // El compilador no permite salir por aquí porque 'mensaje'
        // no ha recibido un valor garantizado.
        return false;
    }
    ```

### Resumen

| Concepto Clave            | Palabra Clave DAW            | Regla / Uso Obligatorio                                                                                                    | Impacto en el Código                                                                                                    |
| :------------------------ | :--------------------------- | :------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------- |
| **Declaración**           | `indicar tipo-dato devuelve` | **Función:** Debe devolver un valor (`return`). **Procedimiento:** No devuelve valor (similar a `void`).                   | Define la estructura de tu módulo de código.                                                                            |
| ---                       | ---                          | ---                                                                                                                        | ---                                                                                                                     |
| **Sobrecarga**            | _(Misma función)_            | Definir **varias funciones/procedimientos** con el **mismo nombre**, pero con **diferentes parámetros** (cantidad o tipo). | Permite usar un nombre simple (`Sumar`) para operaciones que manejan diferentes tipos de datos (ej. `int` o `decimal`). |
| **Parámetros Variables**  | **`params tipo nombre`**     | Permite que el último parámetro de la función reciba un **número indeterminado** de argumentos del mismo tipo.             | Crea funciones muy flexibles que pueden aceptar 1, 5 o 100 argumentos. Recorremos con `foreach`.                        |
| ---                       | ---                          | ---                                                                                                                        | ---                                                                                                                     |
| **Coincidencia de Tipos** | N/A                          | Los tipos de los argumentos y parámetros deben ser **EXACTOS**.                                                            | Garantiza la máxima seguridad y control.                                                                                |
| **Conversión de Tipos**   | `(tipo)`                     | **Casting Explícito Obligatorio** si los tipos son diferentes (ej. `(decimal)mi_int`).                                     | Te obliga a ser consciente de la transformación y pérdida de datos.                                                     |
| ---                       | ---                          | ---                                                                                                                        | ---                                                                                                                     |
| **Paso por Valor**        | _(Por defecto)_              | Pasa una **COPIA** del valor del argumento.                                                                                | Los cambios dentro de la función **NO afectan** a la variable original.                                                 |
| **Paso por Referencia**   | _(Por defecto)_              | Depende del tipo de dato. Primitivos por valor, Resto por referencia                                                       | Los cambios dentro de la función **SÍ afectan** a la variable original.                                                 |

## 4.4 Tipos en la Interfaz Modular: El Contrato Estricto

En la **Programación Modular**, el paso de argumentos a una función o procedimiento es diferente. Aquí, la **regla de oro es la estrictez** y la **coincidencia exacta** de tipos, incluso para las conversiones seguras (`int` a `decimal`) que antes eran válidas o parecen triviales (por conversión por ampliación).

Esta decisión pedagógica, aunque pueda parecer una "inconsistencia" con el `Main`, es la base de la **disciplina de programación superior** que necesitas aprender. ¿Por qué? Esto nos prepara para el desarrollo profesional, donde las librerías y APIs exigen contratos claros y sin ambigüedades.

##### A. ¿Por qué es Sagrado el Contrato de una Función?

La llamada a una función es el **contrato** entre tu código y el módulo que has creado. Este contrato debe ser **inviolable y explícito** para evitar ambigüedades y errores.

| Regla Estricta                            | Por qué Ayuda al Programador                                                                                                                                                                                                                         |
| :---------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Evitar Ambigüedad en Sobrecarga**       | Al forzar el _casting_ explícito (ej. `calcular((decimal)x, y)`), le estás diciendo al compilador **exactamente** qué versión de la función quieres usar si existen varias con el mismo nombre (`sobrecarga`). Esto elimina las dudas de la máquina. |
| **Fomentar la Conciencia de la Interfaz** | Para saber si necesitas o no un _casting_, te obligas a consultar la **firma** (nombre y parámetros) de la función. Un buen desarrollador siempre conoce los requerimientos de los módulos que utiliza.                                              |
| **Preparación para el Mundo Real**        | Las librerías y APIs profesionales exigen esta **estrictez de tipado**. Adquirir este hábito te prepara para crear _software_ sólido y mantenible, es decir código robusto, predecible yofesional                                                    |

### Regla de Oro de la Modularidad

> "Los **módulos son más estrictos** porque exigen el **máximo control**. En una función o procedimiento, no puedes dar nada por sentado.
> La **disciplina y la exactitud** son obligatorias en las interfaces y módulos (procedimientos y funciones) para que tu programa sea robusto y escalable.
>
> **Tu código solo pierde el control** cuando no sabes exactamente qué valor o tipo está procesando un módulo. Al ser estricto con el **contrato** de la función, garantizas la **fiabilidad** de la interfaz y previenes que los errores de tipo se propaguen silenciosamente por tu programa.
>
> Cuando llames a una función, la **interfaz es sagrada**. Con ello, evitas errores en las llamadas a funciones sobrecargadas y te aseguras de que sabes **exactamente** qué estás pasando, con ello aprenderemos a hacer código seguro, predecible y profesional."

### Early Return para simplificar condicionales

Este apartado, aunque conceptualmente es una técnica de diseño que se aplica dentro de la Programación Modular (ya que el `return` se utiliza para la salida de funciones), su objetivo es simplificar drásticamente las estructuras de control condicionales (`if-else` anidadas).

Aquí teniendo funciones y procedimientos ya definidos, podemos usar `return` para salir anticipadamente de una función cuando se cumple una condición específica, evitando así anidar múltiples niveles de `if-else`.

El Teorema Fundamental de la Programación Estructurada establece que todo programa debe tener un **único punto de entrada y un único punto de salida**. Si bien el uso del `return` fuera del final de una función (salida anticipada) técnicamente podría considerarse una desviación de este ideal teórico para fines de flujo de control, en la práctica moderna es aceptado para mejorar la legibilidad y evitar la complejidad del "efecto cascada" en las estructuras `if-else`.

La técnica de **Salida Anticipada** (_Early Return_) consiste en utilizar la sentencia `return` para terminar inmediatamente la ejecución de una **función** o **procedimiento** tan pronto como se haya cumplido una condición que hace innecesaria la ejecución del código restante.

#### Ventajas del Early Return para Condicionales

Esta técnica es especialmente útil para:

1.  **Validación de Entradas (Guard Clauses):** Permite verificar condiciones fallidas o datos inválidos al comienzo de una función y salir de inmediato.
2.  **Aplanar Estructuras Anidadas:** Evita la necesidad de anidar estructuras `if-else if-else` complejas, haciendo el código más claro y reduciendo los niveles de indentación, evitar el efecto cascada o "Hadouken".

Dado que la sentencia `return` solo es válida dentro de funciones, los ejemplos deben implementarse dentro de funciones llamadas desde `Main`.

!!! example "Ejemplo 1: Evitar anidamiento en la validación"

    Supongamos que necesitamos calcular algo solo si dos números son positivos.

    **Sin Salida Anticipada (Anidamiento complejo):**

    ```csharp
    Main {
        System.out.println("Ejemplo 1A: Sin Early Return");

        // Llamada a la función con anidamiento
        int resultado = calcularSiPositivos(5, 3);
        System.out.println("Resultado: " + resultado);

        resultado = calcularSiPositivos(-5, 3);
        System.out.println("Resultado: " + resultado);
    }

    // Función que calcula la suma solo si ambos son positivos (mucho anidamiento)
    int calcularSiPositivos(int a, int b) {
        if (a > 0)
        {
            if (b > 0)
            {
                // Lógica principal
                return a + b;
            }
            else
            {
                System.out.println("Error: El segundo número no es positivo.");
                return 0;
            }
        }
        else
        {
            System.out.println("Error: El primer número no es positivo.");
            return 0;
        }
    }
    ```

    **Con Salida Anticipada (Lógica aplanada):**

    ```csharp
    Main {
        System.out.println("Ejemplo 1B: Con Early Return");

        // Llamada a la función con salida anticipada
        int resultado = calcularConEarlyReturn(5, 3);
        System.out.println("Resultado: " + resultado);

        resultado = calcularConEarlyReturn(-5, 3);
        System.out.println("Resultado: " + resultado);
    }

    // Función que valida y usa early return
    int calcularConEarlyReturn(int a, int b) {
        // 1. Validación del primer error (Salida anticipada)
        if (a <= 0)
        {
            System.out.println("Error: El primer número no es positivo.");
            return 0; // Se sale de la función inmediatamente
        }

        // 2. Validación del segundo error (Salida anticipada)
        if (b <= 0)
        {
            System.out.println("Error: El segundo número no es positivo.");
            return 0; // Se sale de la función inmediatamente
        }

        // Lógica principal: Solo se ejecuta si NINGUNA de las condiciones anteriores se cumplió
        System.out.println("Validación exitosa. Calculando...");
        return a + b;
    }
    ```

!!! example "Ejemplo 2: Romper una cadena de validación compleja"

    La salida anticipada permite gestionar los errores más comunes o condiciones triviales al principio de la función.

    ```csharp
    Main {
        System.out.println("Ejemplo 2: Validar un PIN");

        // Declaración de variables dentro de Main
        int pin = 1234;
        bool esValido = validarPinEarlyReturn(pin);
        System.out.println("PIN 1234 es válido: " + esValido);

        pin = 55;
        esValido = validarPinEarlyReturn(pin);
        System.out.println("PIN 55 es válido: " + esValido);
    }

    // Procedimiento de validación que devuelve un booleano (true/false)
    bool validarPinEarlyReturn(int pin) {
        // 1. Salida Anticipada: El PIN debe ser positivo
        if (pin <= 0)
        {
            System.out.println("PIN no puede ser negativo o cero.");
            return false;
        }

        // 2. Salida Anticipada: El PIN debe tener 4 cifras (simulación)
        if (pin < 1000 || pin > 9999)
        {
            System.out.println("El PIN debe ser de 4 cifras.");
            return false;
        }

        // 3.2. Lógica principal: Si llegamos aquí, el PIN cumple todas las reglas de formato, no ha salido antes
        System.out.println("PIN con formato correcto.");
        return true;
    }

    // Ahora mira mos cómo sería sin Early Return
    bool validarPinSinEarlyReturn(int pin) {
        if (pin <= 0) {
            // 1. Validación del primer error
            System.out.println("PIN no puede ser negativo o cero.");
            return false;
        } else {
                // 2. Validación del segundo error
            if (pin < 1000 || pin > 9999) {
                System.out.println("El PIN debe ser de 4 cifras.");
                return false;
            } else {
                // 3.2. Lógica principal
                System.out.println("PIN con formato correcto.");
                return true;
            }
        }
    }
    ```

    Recuerda que el uso de `return` para salir anticipadamente solo es válido dentro de funciones y procedimientos, no en bloques de código como `if`, `for`, o `while`. Por lo tanto, esta técnica se aplica exclusivamente en el contexto de funciones y procedimientos.

    ![img](./images/indent-hadouken.jpg)

    ![img](./images/early-return.jpeg)

## **2.5 Recursividad**

La recursividad es una técnica que consiste en llamar a una función o procedimiento dentro de sí mismo. La función o procedimiento se llama a sí misma hasta que se cumple una condición que hace que la función o procedimiento deje de llamarse a sí misma (condición de parada o salida). Es importante siempre mostrar la condición de parada.

![](./images/recursividad.gif)

Muchos problemas son mucho más sencillos de resolver con recursividad que con iteración. Por ejemplo, el cálculo de un factorial es mucho más sencillo de resolver con recursividad que con iteración.

```csharp
function int factorial(int n) {
    // Condición de parada
    if (n <= 1) {
        return 1;
    } else {
        // Llamada recursiva
        return n * factorial(n - 1);
    }
}
Main {
    int numero = 5;
    int resultado = factorial(numero);
    System.out.println("El factorial de " + numero + " es " + resultado); // Imprime 120
}
```

![](./images/recursividad.webp)

## **2.6 📦 Paquete o módulo**

Un **paquete** es un archivo o conjunto de archivos que agrupa funciones, procedimientos y tipos de datos relacionados. Sirven para organizar el código por funcionalidades y poder reutilizarlo fácilmente en distintos proyectos. Hay muchas librerías estándar que vienen con el lenguaje DAW, y también puedes crear tus propias librerías para compartir código entre diferentes programas, una de ellas es `Math` que contiene funciones matemáticas comunes como `sqrt` (raíz cuadrada) y `random` (número aleatorio) entre otras: sin(), cos(), tan(), pow(), log(), etc.

Para usar las funcionalidades de un módulo en nuestro programa, primero debemos importarlo. En el lenguaje Java, usaremos la palabra clave `import`.

```csharp
// Imaginemos que existe un módulo llamado "Math".AGru
import Math;

Main {
    // --- Ejemplo con la función sqrt ---
    decimal numero = 16.0;
    // Usamos la función sqrt del módulo Math
    decimal raiz = Math.sqrt(numero);
    System.out.println("La raíz cuadrada de " + numero + " es " + raiz);

    // --- Ejemplo con la función random ---
    // Generamos un número entero aleatorio entre 1 y 6 (ambos inclusive)
    int numeroAleatorio = Math.random(1, 6);
    System.out.println("Lanzamiento de un dado: " + numeroAleatorio);
}
```

Los paquetes cumplen tres propósitos fundamentales que hacen que la programación en Java sea manejable:

1. **Organización (Modularidad)**

   Agrupa `elementos` que están lógicamente relacionadas. Por ejemplo, todas los elementos para manejar la base de datos irán en un paquete (bbdd), y para la interfaz de usuario irán en otro (ui).

   Esto hace que el código sea más fácil de mantener y de encontrar.

2. **Prevención de Colisiones de Nombres**

   Imagina que tú escribes una clase llamada Lista.java y otro desarrollador, cuyo código vas a usar, también tiene una clase llamada Lista.java. ¿Cuál usa el compilador?

   Los paquetes resuelven esto creando espacios de nombres únicos.

   - Un componente: com.tusoluciones.datos.Lista
   - El otro componente: org.externo.utilidades.Lista

   Incluso si los nombres son iguales, sus nombres completos (el nombre del paquete más el nombre de la clase) son diferentes, eliminando la ambigüedad.

3. **Control de Acceso (Protección)**

   Java tiene un nivel de acceso llamado "acceso por defecto" (a veces llamado package-private).

   Si no especificas un modificador de acceso (public, private, protected), un miembro (variable o método) solo será visible para otras elementos dentro del mismo paquete. Esto te permite ocultar detalles de implementación de clases externas al paquete.

<br><br>

## **2.7 🙈 Control de excepciones**

<iframe width="560" height="315" src="https://www.youtube.com/embed/LVKQKNZsv2o" title="Programación Estructurada y Modular. Excepciones" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

El **control de excepciones** es una técnica de programación esencial para manejar errores que ocurren durante la ejecución de un programa de forma inesperada. En lugar de que el programa se detenga abruptamente, las excepciones permiten capturar y gestionar estos errores de manera controlada. Esto es crucial para la robustez y seguridad de las aplicaciones.

### La jerarquía de las excepciones

En la mayoría de los lenguajes, incluida nuestra versión de DAW, todas las excepciones se basan en una clase principal: la clase **`Exception`**. Esta es la base de la jerarquía de excepciones y todas las excepciones que creamos o utilizamos, como `DivideByZeroException`, heredan de ella. Esto significa que un bloque `catch` que capture `Exception` puede manejar cualquier tipo de excepción.

### Excepciones requeridas vs. no requeridas

En el mundo de la programación, las excepciones se dividen en dos categorías principales:

1.  **Excepciones requeridas (o `checked exceptions`)**: Son errores que el compilador te obliga a manejar. Se utilizan para condiciones previsibles y recuperables, como un archivo que no se encuentra o un problema de red. Si no las manejas, el código no compilará. Esto fomenta la creación de código robusto, pero puede llevar a una gran cantidad de bloques `try-catch`, lo que a veces dificulta la lectura del código.
2.  **Excepciones no requeridas (o `unchecked exceptions`)**: Son errores de los que el compilador no te obliga a ocuparte. Se utilizan para errores de programación (como dividir por cero) o fallos inesperados del sistema. La idea es que son errores que el programa no puede recuperar y que deben ser corregidos por el desarrollador.

#### Bloques `try`, `catch` y `finally`

Para manejar excepciones, se usan los siguientes bloques de código:

1.  **`try`**: Contiene las sentencias que podrían generar una excepción. El programa "intenta" ejecutar este código.
2.  **`catch`**: Si una excepción ocurre en el bloque `try`, el control salta a este bloque para "capturar" y gestionar el error.
3.  **`finally`**: Un bloque opcional que se ejecuta **siempre**, tanto si se produce una excepción como si no. Es ideal para tareas de limpieza (cerrar archivos, conexiones). Es opcional.

#### Lanzar excepciones manualmente (`throw`)

La palabra clave `throw` te permite lanzar una excepción de forma explícita. Esto es útil para errores de lógica de negocio que tú mismo quieres definir, por ejemplo, que el divisor sea cero.

**Ejemplo 1: Función que lanza una excepción si el divisor es cero.**

```csharp
function decimal dividirConThrow(int numerador, int divisor) {
    if (divisor == 0) {
        // Lanzamos una excepción de tipo DivideByZeroException
        throw new DivideByZeroException("No se puede dividir por cero.");
    }
    return (decimal)numerador / divisor;
}
```

#### Aserciones (`assert`)

Las **aserciones** son una herramienta de depuración que verifica que una condición sea verdadera. Si la condición es falsa, se lanza una excepción de tipo **`AssertionException`**. Se usan para verificar supuestos sobre el estado del programa durante el desarrollo. No es recomendable dejar aserciones en el código de producción, ya que están destinadas a detectar errores lógicos durante la fase de desarrollo y prueba.

**Ejemplo 2: Función que usa una aserción para verificar el divisor.**

```csharp
function decimal dividirConAssert(int numerador, int divisor) {
    // La aserción verifica que el divisor no sea cero.
    // Si la condición es falsa, se lanza una 'AssertionException'.
    assert(divisor != 0, "El divisor no puede ser cero.");

    return (decimal)numerador / divisor;
}
```

Ambas funciones pueden ser llamadas desde el bloque `Main` y sus excepciones pueden ser capturadas en los bloques `try-catch`.

```csharp
Main {
    var num = 10;
    var div = 0;

    // Consumimos la función con `throw`
    try {
        System.out.println("Intentando dividir con 'throw'...");
        var resultadoThrow = dividirConThrow(num, div);
        System.out.println("Resultado: " + resultadoThrow);
    } catch (DivideByZeroException e) { // <-- Se captura un tipo de excepción específico
        System.out.println("¡Catch! Error capturado: " + e.message);
    } finally {
        System.out.println("Fin del ejemplo 'throw'.");
    }

    System.out.println("---");

    // Consumimos la función con `assert`
    try {
        System.out.println("Intentando dividir con 'assert'...");
        var resultadoAssert = dividirConAssert(num, div);
        System.out.println("Resultado: " + resultadoAssert);
    } catch (AssertionException e) { // <-- Se captura un tipo de excepción específico
        System.out.println("¡Catch! Error de aserción capturado: " + e.message);
    } finally {
        System.out.println("Fin del ejemplo 'assert'.");
    }
}
```

El uso de **excepciones** y **aserciones** mejora la fiabilidad del programa, haciendo que la detección y corrección de errores sea más sencilla, lo cual es vital para los procesos de **depuración y prueba**.

#### Buenas prácticas para el control de excepciones

1.  **Captura solo las excepciones que puedes manejar**: No uses bloques `catch` genéricos que capturen todas las excepciones a menos que tengas una razón específica para hacerlo. Captura solo las excepciones que sabes cómo manejar.
2.  **Usa `finally` para limpieza**: Si tienes recursos que deben ser liberados (como archivos o conexiones de red), usa el bloque `finally` para asegurarte de que siempre se liberen, independientemente de si ocurrió una excepción o no.
    3.2. **No abuses de las excepciones**: Las excepciones deben usarse para manejar errores excepcionales, no para el flujo normal del programa. No uses excepciones para controlar la lógica del programa.
3.  **Proporciona información útil en las excepciones**: Al lanzar excepciones, proporciona mensajes de error claros y útiles que ayuden a identificar la causa del problema. Esto facilitará la depuración y el mantenimiento del código.

!!! bug "**IMPORTANTE**"

    Imagina que vas a dividir dos números, pero el divisor puede ser cero. En lugar de dejar que el programa falle, puedes manejar la excepción y mostrar un mensaje amigable al usuario.

    *¿Por qué es malo que el programa falle sin control o simplemente dejar que falle?* Porque el usuario no sabrá qué ha pasado y el programa se cerrará abruptamente, perdiendo cualquier dato no guardado. Y aunque gestiones las excepciones, si no lo haces bien, puedes ocultar errores importantes que deberían ser corregidos, al final tu programa está fallando y puede que al levantarlo en la clausula catch no sepas qué ha pasado realmente o simplemente ignores el error y no haces nada, lo cual es peor o no lo hagas correctamente.

```csharp
Main {
    var num = 10;
    var div = 0;

    // aquí usamos throw para lanzar una excepción si el divisor es cero
    // Es la mejor manera de hacerlo?
    try {
        var resultado = dividirConThrow(num, div);
        System.out.println("Resultado: " + resultado);
    } catch (DivideByZeroException e) {
        System.out.println("Error: " + e.message);
        // Aquí podrías manejar el error, por ejemplo, pedir otro divisor
    } finally {
        System.out.println("Fin del intento de división.");
    }

    // aquí el if es la manera recomendada de hacerlo
    // Para qe dejar que el programe falle y se reobre si se puede evitar?
    if (div != 0) {
        var resultado = (decimal)num / div;
        System.out.println("Resultado: " + resultado);
    } else {
        System.out.println("No se puede dividir por cero.");
        // Aquí podrías manejar el error, por ejemplo, pedir otro divisor
    }


}
```

#### Resumen del control de excepciones

| Concepto Clave          | Palabra Clave DAW             | Regla / Uso / Propósito                                                                                                                                             | Diferencia Clave / Impacto                                                                                           |
| :---------------------- | :---------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :------------------------------------------------------------------------------------------------------------------- |
| **Bloque de Prueba**    | **`try`**                     | Contiene el código que **podría generar una excepción** (el código "arriesgado").                                                                                   | El flujo normal del programa comienza aquí. Si falla, salta a `catch`.                                               |
| **Bloque de Captura**   | **`catch (Exception e)`**     | Captura y **gestiona el error** (`e`). Permite recuperar el programa en lugar de que se detenga.                                                                    | Permite capturar la clase base `Exception` o tipos específicos (ej. `DivideByZeroException`).                        |
| **Bloque de Limpieza**  | **`finally`**                 | Bloque **opcional** que se ejecuta **SIEMPRE**, haya o no excepción.                                                                                                | Ideal para tareas de limpieza (cerrar archivos, liberar recursos).                                                   |
| ---                     | ---                           | ---                                                                                                                                                                 | ---                                                                                                                  |
| **Lanzar Excepción**    | **`throw`**                   | Lanza una excepción **manualmente** (generalmente, una nueva) si se detecta una condición de error en la lógica de negocio (ej. `if (divisor == 0) { throw ... }`). | Se usa para indicar errores de lógica que deben ser manejados por el código llamante.                                |
| **Comprobación Lógica** | **`assert(condición)`**       | Herramienta de **depuración**. Verifica que una condición sea verdadera; si no lo es, lanza una `AssertionException`.                                               | **No debe usarse en código final (producción)**. Sirve para detectar errores lógicos durante la fase de prueba.      |
| ---                     | ---                           | ---                                                                                                                                                                 | ---                                                                                                                  |
| **Jerarquía Base**      | **`Exception`**               | Clase padre de la que heredan todas las excepciones.                                                                                                                | Un `catch (Exception e)` captura **cualquier** error.                                                                |
| **Tipos de Excepción**  | N/A                           | En DAW, las excepciones son **NO REQUERIDAS** (`unchecked`).                                                                                                        | El compilador **no te obliga** a usar `try-catch`. La responsabilidad de manejar el error recae en el desarrollador. |
| **Mejor Práctica**      | **`if` antes de `try-catch`** | Es mejor **prevenir el error** con un `if` (ej. `if (divisor != 0)`) antes que dejar que falle y usar `try-catch`.                                                  | Las excepciones son para errores **inesperados**, no para validar el flujo normal del programa.                      |
