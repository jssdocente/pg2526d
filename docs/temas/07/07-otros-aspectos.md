# 7. Otros aspectos de Java

En esta sección final exploraremos cómo maneja Java la memoria al pasar variables a métodos y cómo configurar nuestros proyectos de forma profesional.

---

## 7.1. Paso de parámetros en Java (Por Valor)

Es una duda muy común: "¿Java pasa los objetos por referencia?". La respuesta técnica rigurosa es: **NO. Java SIEMPRE pasa los parámetros POR VALOR.**

### Conceptos Fundamentales

1. **Tipos Primitivos (`int`, `boolean`...)**: La variable contiene el valor directamente. Se pasa una **copia del valor**.
2. **Tipos Objeto (`String`, Arrays, Clases...)**: La variable contiene una **referencia** (dirección de memoria) al objeto. Cuando pasas la variable, se pasa una **copia de la referencia**.

!!! warning "Regla de oro"
    Java copia lo que hay dentro de la variable. Si es un primitivo, copia el número. Si es un objeto, copia el "mando a distancia" (puntero) que apunta al objeto.

### Comportamiento con Tipos Primitivos

Si cambias el valor dentro de la función, fuera **no pasa nada**, porque estás modificando una fotocopia.

```java
public void modificarValor(int numero) {
    numero = 99; // Cambio local a la copia
}

int x = 5;
modificarValor(x);
System.out.println(x); // Imprime 5
```

### Comportamiento con Objetos (Referencias)

Al pasar un objeto, entregas una copia de las llaves de casa (la referencia).

#### Efecto A: Modificar estado (Sí se ve fuera)
Puedes usar tus llaves (copia) para entrar y cambiar los muebles.

```java
public void modificarArray(int[] numeros) {
    numeros[0] = 99; // Accedemos a la memoria compartida
}

int[] arr = { 1, 2, 3 };
modificarArray(arr);
System.out.println(arr[0]); // Imprime 99
```

#### Efecto B: Reasignar referencia (NO se ve fuera)
Si intentas cambiar la casa entera (`new`), solo cambias tu juego de llaves. La persona de fuera sigue teniendo sus llaves originales apuntando a la casa vieja. **Esto es lo que diferencia a Java de lenguajes con `ref`.**

```java
public void reasignarArray(int[] numeros) {
    // Esto solo cambia a dónde apunta la variable LOCAL 'numeros'
    numeros = new int[]{ 100, 200 }; 
}

int[] arr = { 1, 2, 3 };
reasignarArray(arr);
System.out.println(arr.length); // Imprime 3 (El array de fuera NO cambió)
```
