# **3ª Evaluación: Actividades Repaso**

## Actividad 01

Diseñar una aplicación Java que permita gestionar una colección de trabajadores en memoria. Los trabajadores pueden ser asalariados y voluntarios.
De cada trabajador se guardan una serie de atributos comunes, como son id, apellidos, nombre y
teléfono y otros específicos:
- de los asalariados, se necesita salario y puesto de trabajo
- de los voluntarios, su turno y una descripción de la función que desempeñan.

Se pide:

1. diseñar un sistema de clases para la gestión de estos trabajadores.
2. crear una colección con un número indefinido de elementos que permita almacenar trabajadores. El acceso a los datos de la colección se llevará a cabo a través de los métodos que trabajan con la misma.

Métodos que trabajan con la colección:

3. añadir trabajadores a esta colección
4. obtener una lista de los trabajadores de un tipo que se pasará como parámetro.
5. obtener una lista de los trabajadores (asalariados) con un salario superior a una cantidad
pasada.
6. obtener el valor medio de los salarios de los trabajadores (asalariados).
7. volcar en un archivo de texto los datos de los trabajadores de un tipo que será pasado como parámetro. Este archivo tendrá el nombre “archivo_trabajadores_XXX.txt”, donde XXX será el tipo de trabajador asociado al archivo. Ejemplo,
“archivo_trabajadores_asalariados.txt”.

Cada línea del archivo contendrá los atributos comunes y específicos de cada trabajador.

Un ejemplo de línea sería el siguiente:

```text
1000;Ramos Salva;Ana;622123999;2300:Jefa Sección
```

8. crear algún mecanismo que obligue a que los métodos citados sean desarrollados.

## Actividad 02

Diseñar una aplicación Java que permita gestionar los datos de lluvia registrados en un archivo de texto con el nombre “lluvias.txt”. Cada línea del archivo (exceptuando la cabecera) guarda la siguiente información: `población, mes, día y litros`.

Un ejemplo del contenido de una línea sería el siguiente:
```text
Badajoz;1;12;15.5
```
Como se observa, los datos están separados por punto y coma (;) y la primera línea del fichero es una cabecera que comienza por #.

Para ello, el alumno debe realizar lo siguiente:

1. **Crear una clase `RegistroLluvia`** que permita almacenar la información de cada línea del fichero (debe contener atributos para población, mes, día y litros).
2. **Cargar los datos** del archivo en una colección en memoria (por ejemplo, una lista de objetos `RegistroLluvia`).
3. **Implementar una interfaz o clase abstracta** que defina los métodos de consulta citados a continuación, asegurando que la gestión de la colección se realice a través de estos métodos.

**Consultas a desarrollar:**

4. Mostrar por pantalla todos los registros de lluvia de una población pasada como parámetro.
5. Retornar el valor medio de los litros caídos en un mes determinado (1-12) pasado como parámetro.
6. Retornar una colección con los registros que superen una cantidad de litros determinada.
7. Volcar en un archivo de texto los datos de una población específica. El archivo generado debe tener el nombre “lluvias_XXX.txt”, donde “XXX” será el nombre de la población.
8. Mostrar los datos agrupados por meses, indicando el total de litros caídos en cada uno.




## Actividad 03

Realizar un programa en JAVA  que almacene en una estructura de datos dinámica los litros de lluvia diarios caídos en la ciudad de Plasencia durante el mes de febrero. 

1. Implementa una función que muestre un menú de opciones. Mostrará por pantalla las opciones, leerá la entrada de teclado y, en función de ésta, llamará a la función que
realice la operación correspondiente. Después de que el usuario seleccione una opción y se realice la operación elegida, volverá a mostrarse el menú para que el usuario pueda
volver a seleccionar otra opción, y así sucesivamente hasta que el usuario pulse 5 y se finalice la ejecución. Ejemplo de ejecución:

```text
**** ESTADÍSTICA DE LLUVIAS EN PLASENCIA - FEBRERO ****
Elija una opción:
1 - Cargar los litros de agua por metro cuadrado diarios.
2 - Mostrar los litros diarios de agua del mes.
3 - Mostrar la media de litros diarios del mes.
4 - Mostrar el día o días más lluviosos.
5 - Salir del programa.
```

2. Implementa una función que permita al usuario rellenar la matriz con los litros de lluvia diarios, pidiéndolos uno a uno. Ejemplo de ejecución:

```text


