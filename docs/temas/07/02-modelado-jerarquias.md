# 2. Modelado de Jerarquías y Abstracción

En esta sección exploraremos cómo diseñar jerarquías de clases robustas y coherentes, utilizando la abstracción para definir plantillas generales y la herencia para concretar comportamientos específicos.

---

## 2.1 Clases abstractas y métodos abstractos

Una **clase abstracta** es una clase que no se puede instanciar (no se pueden crear objetos de ella directamente) y que está diseñada para ser una clase base para otras clases. Se utiliza para definir una interfaz común y un comportamiento compartido que deben implementar sus clases derivadas.

Un **método abstracto** es un método declarado en una clase abstracta que no tiene implementación en esa clase. Las clases derivadas que no sean abstractas están obligadas a proporcionar una implementación para todos los métodos abstractos heredados.

### Conceptos Clave

- Una clase con al menos un método abstracto **debe** marcarse como abstracta usando la palabra clave `abstract`.
- Sirven como "contrato" o "plantilla" para las subclases.
- Permiten centralizar lógica común mientras delegan los detalles específicos a los hijos.

---

## 2.2 Métodos virtuales y redefinición

A diferencia de los métodos abstractos, los métodos no abstractos tienen una implementación en la clase base. En Java, por defecto, todos los métodos no estáticos y no privados son **virtuales**, lo que significa que permiten que las clases derivadas los modifiquen (sobrescriban).

### Métodos virtuales y `@Override`

- **Comportamiento por defecto**: En Java no hace falta la palabra clave `virtual`. Cualquier método de instancia es sobrescribible salvo que se marque como `final`.
- **`@Override`**: Se usa en la clase derivada para indicar explícitamente que estamos sobrescribiendo un método. Aunque es opcional, es altamente recomendable para que el compilador verifique que la firma coincide.

### Tipos de retorno covariantes

Desde Java 5, un método sobrescrito puede devolver un tipo más específico (subclase) que el método de la clase base. Esto mejora la expresividad y seguridad del código.

```java
public abstract class Creador {
    public abstract Animal crear();
}

public class CreadorDePerros extends Creador {
    // Devuelve Perro en lugar de Animal. Esto es válido y se llama covarianza.
    @Override
    public Perro crear() {
        return new Perro();
    }
}
```

---

## 2.3 Jerarquía Sellada

### ¿Qué es una jerarquía sellada?

Una **jerarquía sellada** es un diseño donde controlamos exactamente qué clases pueden heredar de una base. En Java (desde la versión 17), esto se logra mediante las **sealed classes** (clases selladas).

- **`final`**: Impide que *nadie* herede de la clase.
- **`sealed`**: Permite que *solo clases específicas* hereden de ella (usando `permits`).

### ¿Para qué se usa este patrón?

1.  **Seguridad**: Evitas que otros programadores extiendan clases de manera no controlada.
2.  **Modelado de Dominio**: Define un conjunto finito y conocido de posibilidades.
3.  **Pattern Matching Exhaustivo**: Permite al compilador saber que ha cubierto todos los casos posibles en un `switch`.

### Ejemplo: Modelando Tipos de Vehículos con `sealed` clases

```java
// 'permits' lista explícitamente las únicas clases permitidas para heredar
public sealed abstract class Vehiculo permits Coche, Moto, Camion { }

// Las subclases de una clase sealed deben ser final, sealed o non-sealed
public final class Coche extends Vehiculo { }
public final class Moto extends Vehiculo { }
public final class Camion extends Vehiculo { }
```

### Manejo exhaustivo con `switch` y pattern matching

Al tener una jerarquía sellada, podemos usar `switch` (con Pattern Matching, funcionalidad moderna de Java) para gestionar todos los tipos posibles de forma segura. Si cubrimos todos los casos, no necesitamos `default`.

```java
public void describirVehiculo(Vehiculo vehiculo) {
    switch (vehiculo) {
        case Coche c -> System.out.println("Es un coche.");
        case Moto m -> System.out.println("Es una moto.");
        case Camion t -> System.out.println("Es un camión.");
        // No necesitamos 'default' porque el compilador sabe que no hay más tipos de Vehiculo.
    }
}
```

---

## 2.4 Herencia en la Práctica: Excepciones Personalizadas

### ¿Por qué crear excepciones propias?

Crear tus propias clases de excepción (heredando de `Exception` o `RuntimeException`) permite:
- Capturar errores específicos de tu aplicación de forma diferenciada.
- Añadir información extra al objeto de error.
- Mejorar la legibilidad del bloque `try-catch`.

### Ejemplo de Implementación

```java
public class EdadInvalidaException extends Exception {
    private int edadIntento;
    
    public EdadInvalidaException(int edad) {
        super("La edad " + edad + " no es válida.");
        this.edadIntento = edad;
    }
    
    public int getEdadIntento() {
        return edadIntento;
    }
}

public void registrarUsuario(String nombre, int edad) throws EdadInvalidaException {
    if (edad < 0 || edad > 120) {
        throw new EdadInvalidaException(edad);
    }
    // Lógica de registro...
}
```

---

!!! tip "Matiz del Profesor: ¿Cuándo cerrar una jerarquía?"
    El uso de `final` (o `sealed`) no es solo por seguridad, sino por **diseño semántico**. 
    
    *   **Clase Abstracta**: Es una "promesa" de comportamiento.
    *   **Clase Final**: Es el "punto final". Indica que la lógica es completa y no debe ser alterada.

    ```mermaid
    graph LR
        A["Animal (Abstract)"] --> B["Mamífero (Extendable)"]
        B --> C["Humano (Final)"]
        style C fill:#f96,stroke:#333
    ```
