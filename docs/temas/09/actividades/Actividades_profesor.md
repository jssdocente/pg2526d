# **UT9: Actividades — Guía para el Profesor**

Este documento recopila los criterios de evaluación detallados para cada actividad de la unidad de Ficheros, junto con sus enunciados correspondientes.

---

## 🟢 Nivel Básico

---

### **Actividad 01 — Organizador de Espacio de Trabajo**

!!! abstract "Descripción"

    Al arrancar cualquier aplicación de gestión, es habitual que necesite **inicializar su estructura de directorios y ficheros** si no existen. En esta actividad crearás ese sistema de inicialización.

#### Enunciado

Desarrolla la clase `OrganizadorEspacio` que, al ejecutarse, cree la siguiente estructura de directorios y ficheros si no existen ya:

```
proyecto/
├── datos/
│   ├── alumnos.txt
│   └── notas.csv
├── logs/
│   └── actividad.log
└── backups/
```

**Requisitos:**

1. Si un directorio o fichero **ya existe**, mostrar un mensaje indicándolo (no lanzar error).
2. Si se **crea por primera vez**, mostrar un mensaje de confirmación.
3. Usar `Files.createDirectories()` para los directorios y `Files.createFile()` para los ficheros.
4. Gestionar correctamente la `IOException`.
5. Al final, mostrar un resumen: cuántos directorios y ficheros se han creado.

!!! example "Ejemplo de ejecución (primera vez)"

    ```
    === Inicializando espacio de trabajo ===
    [CREADO] Directorio: proyecto/datos
    [CREADO] Directorio: proyecto/logs
    [CREADO] Directorio: proyecto/backups
    [CREADO] Fichero: proyecto/datos/alumnos.txt
    [CREADO] Fichero: proyecto/datos/notas.csv
    [CREADO] Fichero: proyecto/logs/actividad.log
    ─────────────────────────────────────────
    Resumen: 3 directorio(s) y 3 fichero(s) creados.
    ```

!!! example "Ejemplo de ejecución (segunda vez)"

    ```
    === Inicializando espacio de trabajo ===
    [OK] Directorio ya existe: proyecto/datos
    [OK] Directorio ya existe: proyecto/logs
    [OK] Directorio ya existe: proyecto/backups
    [OK] Fichero ya existe: proyecto/datos/alumnos.txt
    [OK] Fichero ya existe: proyecto/datos/notas.csv
    [OK] Fichero ya existe: proyecto/logs/actividad.log
    ─────────────────────────────────────────
    Resumen: 0 directorio(s) y 0 fichero(s) creados.
    ```

!!! success "Criterios de evaluación"

    - [ ] La estructura se crea correctamente en disco.
    - [ ] No se lanza excepción si ya existe.
    - [ ] Los mensajes distinguen entre "creado" y "ya existía".
    - [ ] El resumen final es correcto.
    - [ ] Las excepciones están correctamente capturadas.

---

### **Actividad 02 — Lector y Buscador de Ficheros de Texto**

!!! abstract "Descripción"

    Leer el contenido de un fichero de texto y mostrarlo formateado es una de las operaciones más frecuentes. En esta actividad crearás un lector que además permite **buscar palabras clave** en el contenido.

#### Enunciado

Crea la clase `LectorBuscador` que lea un fichero de texto y ofrezca dos funcionalidades:

1. **Mostrar el fichero numerado:** Muestra cada línea precedida de su número de línea.
2. **Buscar una palabra:** Muestra todas las líneas que contienen una palabra dada (búsqueda case-insensitive), indicando el número de línea.

**Requisitos:**

1. El nombre del fichero y la palabra de búsqueda se pasan como argumentos al `main` (o se definen como constantes si no has visto `args` aún).
2. Si el fichero no existe, mostrar un mensaje claro y terminar.
3. El número de línea en la visualización ocupa siempre **3 caracteres** (formato `%3d`).
4. Si la búsqueda no encuentra resultados, indicarlo explícitamente.
5. Mostrar al final el total de líneas del fichero y el total de coincidencias encontradas.

**Fichero de prueba `poema.txt`:**

```
La primavera besó suavemente la colina,
y la llamó con su tierna voz de amante,
y a los labios de la tierra, la mañana
respondió con un beso de diamante.
La primavera pasó como un suspiro,
y el eco de su voz quedó en el viento,
mientras la tierra, absorta en su retiro,
soñaba con el próximo momento.
```

!!! example "Ejemplo: mostrar numerado"

    ```
      1 | La primavera besó suavemente la colina,
      2 | y la llamó con su tierna voz de amante,
      3 | y a los labios de la tierra, la mañana
      4 | respondió con un beso de diamante.
      5 | La primavera pasó como un suspiro,
      6 | y el eco de su voz quedó en el viento,
      7 | mientras la tierra, absorta en su retiro,
      8 | soñaba con el próximo momento.
    ─────────────────────────────
    Total: 8 líneas.
    ```

!!! example "Ejemplo: buscar 'tierra'"

    ```
    Búsqueda de "tierra" en poema.txt:
    ─────────────────────────────────
      3 | y a los labios de la tierra, la mañana
      7 | mientras la tierra, absorta en su retiro,
    ─────────────────────────────────
    2 coincidencia(s) encontrada(s).
    ```

!!! success "Criterios de evaluación"

    - [ ] La numeración de líneas está correctamente formateada (alineada a la derecha).
    - [ ] La búsqueda no distingue mayúsculas/minúsculas.
    - [ ] Se informa si el fichero no existe.
    - [ ] Se muestran correctamente los totales.
    - [ ] El código gestiona la `IOException`.

---

### **Actividad 03 — Diario de Eventos**

!!! abstract "Descripción"

    Muchas aplicaciones necesitan registrar eventos en un fichero de log. Lo importante es que cada nueva entrada se **añada al final** sin borrar lo existente, y que el fichero muestre la información con el **formato adecuado**.

#### Enunciado

Desarrolla la clase `DiarioEventos` con los siguientes métodos:

1. **`registrar(String categoria, String mensaje)`** — Añade una entrada al fichero `diario.log` con el formato:
   `[YYYY-MM-DD HH:mm:ss] [CATEGORIA  ] Mensaje`
2. **`mostrar()`** — Lee el diario y lo muestra por consola con todas sus entradas.
3. **`mostrarPorCategoria(String categoria)`** — Muestra solo las entradas de una categoría concreta.
4. **`contarPorCategoria()`** — Muestra cuántas entradas hay de cada categoría.

**Categorías disponibles:** `INFO`, `AVISO`, `ERROR`
**Requisitos:**

1. Si el fichero no existe, se crea automáticamente al registrar la primera entrada.
2. La columna de categoría ocupa siempre **8 caracteres** (rellenar con espacios si es más corta).
3. La fecha y hora se formatea con `DateTimeFormatter`.
4. Usar `StandardOpenOption.APPEND` para no sobreescribir entradas anteriores.

!!! example "Ejemplo de ejecución del main"

    ```java
    DiarioEventos.registrar("INFO",  "Aplicación iniciada.");
    DiarioEventos.registrar("INFO",  "Usuario 'admin' ha iniciado sesión.");
    DiarioEventos.registrar("AVISO", "Disco al 85% de capacidad.");
    DiarioEventos.registrar("ERROR", "No se pudo conectar con la BBDD.");
    DiarioEventos.registrar("INFO",  "Conexión BBDD restablecida.");
    DiarioEventos.mostrar();
    DiarioEventos.contarPorCategoria();
    ```

    **Contenido de `diario.log` y salida por consola:**
    ```
    [2026-03-15 10:01:05] [INFO    ] Aplicación iniciada.
    [2026-03-15 10:01:12] [INFO    ] Usuario 'admin' ha iniciado sesión.
    [2026-03-15 10:05:33] [AVISO   ] Disco al 85% de capacidad.
    [2026-03-15 10:07:44] [ERROR   ] No se pudo conectar con la BBDD.
    [2026-03-15 10:08:01] [INFO    ] Conexión BBDD restablecida.

    Entradas por categoría:
      INFO  : 3
      AVISO : 1
      ERROR : 1
    ```

!!! success "Criterios de evaluación"

    - [ ] El fichero se crea si no existe y las entradas se acumulan correctamente (APPEND).
    - [ ] El formato de fecha/hora es correcto.
    - [ ] La alineación de columnas es consistente.
    - [ ] `mostrarPorCategoria` filtra correctamente.
    - [ ] `contarPorCategoria` devuelve conteos correctos.

---

### **Actividad 04 — Gestor de Ficheros de un Proyecto**

!!! abstract "Descripción"

    Un explorador de ficheros básico permite listar, inspeccionar y organizar los ficheros de un directorio. En esta actividad construirás uno que combina varias operaciones sobre el sistema de archivos.

#### Enunciado

Crea la clase `GestorFicheros` con los siguientes métodos estáticos:

1. **`listarDirectorio(Path dir)`** — Lista todos los ficheros del directorio mostrando: nombre, tamaño en KB, y fecha de última modificación. Al final, muestra el total de ficheros y el tamaño total.

2. **`copiarFichero(Path origen, Path destino)`** — Copia un fichero. Si el destino ya existe, lo sobreescribe. Muestra el resultado.

3. **`renombrar(Path fichero, String nuevoNombre)`** — Renombra un fichero en el mismo directorio.

4. **`eliminarFicherosTmp(Path dir)`** — Elimina todos los ficheros del directorio que terminen en `.tmp`, mostrando cuántos se eliminaron.

**Requisitos:**

1. El listado muestra el tamaño con 1 decimal en KB: `1.5 KB`.
2. La fecha de modificación se muestra en formato `dd/MM/yyyy`.
3. Si un directorio no existe, mostrar un error descriptivo.
4. Usar `DirectoryStream` con filtro glob para `eliminarFicherosTmp`.

!!! example "Ejemplo de listado"

    ```
    Contenido de: proyecto/datos/
    ──────────────────────────────────────────────────────
    Nombre                         Tamaño    Modificado
    ──────────────────────────────────────────────────────
    alumnos.csv                     2.3 KB   12/03/2026
    notas_1T.txt                    1.1 KB   15/03/2026
    backup_antiguo.tmp              0.4 KB   01/03/2026
    configuracion.properties        0.2 KB   10/03/2026
    ──────────────────────────────────────────────────────
    4 fichero(s) — Tamaño total: 4.0 KB
    ```

!!! success "Criterios de evaluación"

    - [ ] El listado muestra correctamente nombre, tamaño y fecha.
    - [ ] El tamaño total es correcto.
    - [ ] La copia sobreescribe sin error.
    - [ ] El renombrado funciona correctamente.
    - [ ] `eliminarFicherosTmp` solo elimina los `.tmp` y muestra el conteo.

---

## 🟡 Nivel Intermedio

---

### Actividad 05 — Copias de Seguridad con try-with-resources

!!! abstract "Descripción"

    La gestión correcta de recursos es crucial en aplicaciones robustas. En esta actividad trabajarás con `BufferedReader` y `BufferedWriter` para copiar y transformar ficheros, asegurando siempre la **liberación de recursos** con `try-with-resources`.

#### Enunciado

Desarrolla la clase `GestorBackup` con estos métodos:

1. **`copiarConTransformacion(Path origen, Path destino, String prefijo)`** — Copia el fichero `origen` al `destino`, añadiendo el `prefijo` al principio de cada línea.

2. **`anonimizar(Path entrada, Path salida)`** — Lee un CSV con la estructura `nombre,telefono,email,nota` y escribe un nuevo CSV donde el teléfono está enmascarado (`XXX-XXX-XXX`) y el email muestra solo el dominio (`***@gmail.com`).

3. **`contarLineasYPalabras(Path fichero)`** — Devuelve un `int[]` de dos elementos: `{numLineas, numPalabras}`.

**Requisitos:**

1. **Todos** los métodos deben usar `try-with-resources`.
2. En `copiarConTransformacion`, ambos recursos (lector y escritor) se declaran en el **mismo** `try`.
3. En `anonimizar`, la máscara del email conserva el dominio: `ana.garcia@correo.es` → `***@correo.es`.
4. Las líneas vacías o comentarios (`#`) se copian sin transformar.

!!! example "Ejemplo de anonimización"

    **Entrada `alumnos.csv`:**
    ```
    nombre,telefono,email,nota
    Ana García,612345678,ana.garcia@gmail.com,8.5
    Carlos Pérez,698765432,carlos.p@hotmail.com,6.0
    ```

    **Salida `alumnos_anon.csv`:**
    ```
    nombre,telefono,email,nota
    Ana García,XXX-XXX-XXX,***@gmail.com,8.5
    Carlos Pérez,XXX-XXX-XXX,***@hotmail.com,6.0
    ```

!!! success "Criterios de evaluación"

    - [ ] Todos los métodos usan `try-with-resources` correctamente.
    - [ ] No hay ningún `reader.close()` o `writer.close()` explícito.
    - [ ] La transformación de prefijo funciona correctamente.
    - [ ] La anonimización enmascara teléfono y email manteniendo el dominio.
    - [ ] `contarLineasYPalabras` devuelve valores correctos.

---

### Actividad 06 — Procesador de Notas CSV

!!! abstract "Descripción"

    Leer un CSV y transformar su contenido en objetos del dominio es el patrón más frecuente en aplicaciones reales. Esta actividad te lleva por el ciclo completo: **leer → parsear → validar → calcular → mostrar**.

#### Enunciado

Dado el siguiente fichero CSV de notas de alumnos, desarrolla la clase `ProcesadorNotas`:

**Fichero `notas.csv`:**
```
# Notas del módulo Programación - 1º DAW
# Formato: nombre,t1,t2,t3 (nota de cada trimestre, 0-10)
nombre,t1,t2,t3
Ana García López,8.5,7.0,9.0
Carlos Pérez Ruiz,4.0,5.5,6.0
María Torres Vega,7.0,8.0,7.5
Javier Sánchez,3.5,4.0,
Lucía Martín,6.0,7.5,8.0
Pedro Romero,10.0,9.5,9.0
```

> **Nota:** Javier tiene el T3 vacío (aún no evaluado). El programa debe manejarlo.

Implementa los siguientes métodos:

1. **`cargarNotas(Path ruta)`** → `List<Alumno>`: Lee el CSV, salta cabecera y comentarios, parsea cada fila. Si una nota está vacía, se almacena como `-1` (no evaluada).

2. **`mostrarTabla(List<Alumno> alumnos)`**: Muestra una tabla formateada con nombre, T1, T2, T3, media y estado (APROBADO / SUSPENSO / PENDIENTE si hay nota -1).

3. **`generarResumen(List<Alumno> alumnos)`**: Muestra: total alumnos, aprobados, suspensos, pendientes, nota media de la clase, alumno con mejor y peor nota.

4. **`exportarAprobados(List<Alumno> alumnos, Path destino)`**: Escribe un nuevo CSV con solo los alumnos aprobados (media ≥ 5), ordenados de mayor a menor nota.

**Requisitos:**

1. La tabla muestra `---` en la celda si la nota es `-1`.
2. El estado es `PENDIENTE` si alguna nota es `-1`, `APROBADO` si media ≥ 5, `SUSPENSO` en caso contrario.
3. Las líneas malformadas (< 4 campos) se ignoran con un aviso en `System.err`.

!!! example "Ejemplo de tabla"

    ```
    ══════════════════════════════════════════════════════════════════
    Nombre                      T1     T2     T3    Media  Estado
    ──────────────────────────────────────────────────────────────────
    Ana García López            8.5    7.0    9.0    8.2   APROBADO
    Carlos Pérez Ruiz           4.0    5.5    6.0    5.2   APROBADO
    María Torres Vega           7.0    8.0    7.5    7.5   APROBADO
    Javier Sánchez              3.5    4.0    ---    ---   PENDIENTE
    Lucía Martín                6.0    7.5    8.0    7.2   APROBADO
    Pedro Romero               10.0    9.5    9.0    9.5   APROBADO
    ══════════════════════════════════════════════════════════════════
    Aprobados: 5  Pendientes: 1  Suspensos: 0
    Nota media de la clase: 7.5  |  Mejor: Pedro Romero (9.5)
    ```

!!! success "Criterios de evaluación"

    - [ ] El CSV se parsea correctamente, ignorando cabecera y comentarios.
    - [ ] Las notas vacías se manejan como `-1` y muestran `---`.
    - [ ] La tabla está correctamente alineada.
    - [ ] El estado PENDIENTE / APROBADO / SUSPENSO es correcto.
    - [ ] El fichero de aprobados se genera y está ordenado.

---

### Actividad 07 — Análisis de Logs con Stream API

!!! abstract "Descripción"

    La API Stream permite procesar grandes volúmenes de datos de forma declarativa y eficiente. En esta actividad la aplicarás al análisis de un fichero de log de servidor, construyendo pipelines que filtran, transforman y agregan información.

#### Enunciado

Dado el fichero `servidor.log` con el formato:
```
[YYYY-MM-DD HH:mm:ss] [NIVEL] [MODULO] Mensaje del evento
```

**Fichero `servidor.log` de ejemplo:**
```
[2026-03-15 08:00:01] [INFO ] [AUTH  ] Servidor iniciado correctamente
[2026-03-15 08:01:15] [INFO ] [AUTH  ] Usuario 'admin' ha iniciado sesión
[2026-03-15 08:05:33] [WARN ] [DB    ] Consulta lenta detectada: 2300ms
[2026-03-15 08:07:44] [ERROR] [DB    ] Timeout en conexión a base de datos
[2026-03-15 08:08:01] [INFO ] [DB    ] Conexión restablecida
[2026-03-15 08:12:20] [WARN ] [AUTH  ] Intento de acceso fallido: usuario 'guest'
[2026-03-15 08:15:05] [ERROR] [AUTH  ] Bloqueo por múltiples intentos fallidos: IP 192.168.1.50
[2026-03-15 08:20:33] [INFO ] [APP   ] Proceso batch completado: 1523 registros
[2026-03-15 08:25:11] [WARN ] [APP   ] Memoria al 78%
[2026-03-15 08:30:00] [ERROR] [APP   ] OutOfMemoryError en proceso de exportación
```

Implementa la clase `AnalizadorLog` con estos métodos, **todos usando `Files.lines()` y operaciones Stream**:

1. **`contarPorNivel(Path log)`** → muestra cuántas entradas hay de cada nivel (INFO, WARN, ERROR).
2. **`obtenerErrores(Path log)`** → devuelve `List<String>` con las líneas de nivel ERROR.
3. **`modulosAfectados(Path log)`** → devuelve `List<String>` con los módulos únicos que tienen errores, ordenados alfabéticamente.
4. **`primerasEntradas(Path log, int n)`** → devuelve las primeras `n` entradas de nivel WARN o ERROR.
5. **`tieneErroresCriticos(Path log, String palabra)`** → devuelve `true` si alguna línea ERROR contiene la `palabra` dada.

**Requisitos:**

1. Cada método abre su propio stream con `try-with-resources`.
2. Ningún método usa bucles `for` ni `while`. Solo operaciones Stream.
3. El método `contarPorNivel` extrae el nivel buscando el patrón `[NIVEL]` en la línea.

!!! example "Salida esperada de contarPorNivel"

    ```
    Análisis de niveles en servidor.log:
      INFO  → 4 entradas
      WARN  → 3 entradas
      ERROR → 3 entradas
    ```

!!! example "Salida esperada de modulosAfectados"

    ```
    Módulos con errores: [APP, AUTH, DB]
    ```

!!! success "Criterios de evaluación"

    - [ ] Todos los métodos usan exclusivamente operaciones Stream (sin bucles).
    - [ ] Cada método usa `try-with-resources` correctamente.
    - [ ] Los conteos por nivel son correctos.
    - [ ] La lista de módulos está ordenada y sin duplicados.
    - [ ] `tieneErroresCriticos` funciona correctamente con distintas palabras.

---

## 🔴 Nivel Avanzado

---

### Actividad 08 — Estadísticas Avanzadas con Collectors

!!! abstract "Descripción"

    Los `Collectors` de la API Stream permiten realizar **agregaciones complejas** en una sola pasada sobre los datos. En esta actividad procesarás el CSV de notas para generar estadísticas que en código imperativo requerirían múltiples bucles.

#### Enunciado

Utiliza el fichero `notas_completo.csv` con el formato: `nombre,modulo,t1,t2,t3`

```
nombre,modulo,t1,t2,t3
Ana García,Programación,8.5,7.0,9.0
Carlos Pérez,Programación,4.0,5.5,6.0
María Torres,Bases de Datos,7.0,8.0,7.5
Javier Sánchez,Bases de Datos,3.5,4.0,3.0
Lucía Martín,Programación,6.0,7.5,8.0
Pedro Romero,Bases de Datos,10.0,9.5,9.0
Sara López,Programación,5.5,6.0,5.0
David Núñez,Bases de Datos,4.5,5.0,4.0
```

Implementa la clase `EstadisticasAlumnos` con los siguientes métodos, **todos usando Collectors**:

1. **`notaMediaPorModulo(List<Alumno> alumnos)`** → `Map<String, Double>`: Nota media por módulo.
2. **`alumnosPorModulo(List<Alumno> alumnos)`** → `Map<String, List<String>>`: Nombres agrupados por módulo.
3. **`particionarAprobadosSuspensos(List<Alumno> alumnos)`** → `Map<Boolean, Long>`: Cuántos aprobados (true) y suspensos (false).
4. **`rankingModulos(List<Alumno> alumnos)`** → muestra cada módulo ordenado de mejor a peor nota media.
5. **`generarCsvResumen(List<Alumno> alumnos, Path destino)`** → Escribe un CSV con `nombre,media,estado` usando `Collectors.joining`.

**Requisitos:**

1. La clase `Alumno` debe incluir `record Alumno(String nombre, String modulo, double t1, double t2, double t3)` con un método `media()` que calcule `(t1+t2+t3)/3.0` y `aprobado()` que devuelva `media() >= 5.0`.
2. Ningún método `1-4` usa bucles. Solo Collectors.
3. El CSV de resumen tiene cabecera y los alumnos ordenados por media descendente.

!!! example "Salida esperada de rankingModulos"

    ```
    Ranking de módulos por nota media:
      1. Bases de Datos  →  7.58
      2. Programación    →  6.54
    ```

!!! example "CSV generado por generarCsvResumen"

    ```
    nombre,media,estado
    Pedro Romero,9.50,APROBADO
    Ana García,8.17,APROBADO
    María Torres,7.50,APROBADO
    ...
    ```

!!! success "Criterios de evaluación"

    - [ ] Todos los métodos usan Collectors (sin bucles).
    - [ ] `notaMediaPorModulo` devuelve las medias correctas.
    - [ ] `particionarAprobadosSuspensos` cuenta correctamente.
    - [ ] El CSV de resumen está ordenado y tiene el formato correcto.
    - [ ] El ranking muestra los módulos en orden correcto.

---

### Actividad 09 — Serialización: Gestor de Inventario

!!! abstract "Descripción"

    La serialización es el mecanismo de Java para **persistir objetos** completos entre ejecuciones. En esta actividad crearás un gestor de inventario que guarda y recupera el estado completo de la aplicación.

#### Enunciado

Desarrolla el sistema de inventario de una tienda de tecnología con las siguientes clases:

**`Producto implements Serializable`:**
```
- id: int
- nombre: String
- precio: double
- stock: int
- categoria: String  ("Portátil", "Periférico", "Componente")
```

**`Inventario`** (no necesita ser Serializable, gestiona la persistencia):

1. **`guardar(List<Producto> productos, Path fichero)`** — Serializa la lista completa.
2. **`cargar(Path fichero)`** → `List<Producto>` — Deserializa y devuelve la lista.
3. **`mostrar(List<Producto> productos)`** — Muestra tabla formateada con todos los campos.
4. **`buscarPorCategoria(List<Producto> productos, String categoria)`** → `List<Producto>`.
5. **`actualizarStock(List<Producto> productos, int id, int nuevoStock)`** — Busca el producto por id y actualiza su stock. Devuelve `true` si se encontró, `false` si no.

**Requisitos:**

1. `Producto` declara `serialVersionUID = 1L`.
2. `guardar` y `cargar` usan `try-with-resources`.
3. `cargar` devuelve una lista vacía si el fichero no existe (sin lanzar excepción).
4. El `main` debe demostrar el ciclo completo: crear productos → guardar → cerrar (simular nueva ejecución con nueva lista vacía) → cargar → modificar stock → guardar de nuevo → mostrar.

!!! example "Ejemplo de ejecución del main"

    ```
    === Primera ejecución: creando inventario ===
    Guardados 4 productos en inventario.dat
    
    === Segunda ejecución: cargando inventario ===
    Inventario cargado: 4 producto(s).
    
    ID   Nombre                    Precio    Stock  Categoría
    ──────────────────────────────────────────────────────────
    001  MacBook Air M3           1299.00€     15   Portátil
    002  Ratón inalámbrico          25.99€    200   Periférico
    003  SSD 1TB NVMe              89.99€     80   Componente
    004  Monitor 27" 4K           349.00€     30   Portátil
    
    Actualizando stock de producto 002...
    Stock actualizado: Ratón inalámbrico → 185 unidades.
    
    Guardando inventario actualizado...
    ✓ Inventario guardado correctamente.
    ```

!!! success "Criterios de evaluación"

    - [ ] `Producto` implementa `Serializable` con `serialVersionUID`.
    - [ ] El ciclo guardar/cargar funciona correctamente entre "ejecuciones".
    - [ ] `cargar` devuelve lista vacía si el fichero no existe.
    - [ ] `actualizarStock` modifica el objeto correctamente.
    - [ ] La tabla de productos está correctamente formateada.
    - [ ] Todas las operaciones de I/O usan `try-with-resources`.

---

### Actividad 10 — Buscador Recursivo con `Files.walk()`

!!! abstract "Descripción"

    Las aplicaciones frecuentemente necesitan buscar ficheros en estructuras de directorios anidados. `Files.walk()` junto con Stream API permite hacerlo de forma elegante y eficiente. En esta actividad construirás una herramienta de búsqueda y análisis de proyectos.

#### Enunciado

Desarrolla la clase `AnalizadorProyecto` que recibe como parámetro la **ruta raíz de un proyecto Java** y ofrece los siguientes análisis:

1. **`contarFicherosPorExtension(Path raiz)`** → `Map<String, Long>`: Cuenta cuántos ficheros hay de cada extensión (`.java`, `.xml`, `.properties`, etc.).

2. **`buscarFicheros(Path raiz, String patron)`** → `List<Path>`: Devuelve todos los ficheros cuyo nombre contenga el patrón dado (case-insensitive).

3. **`calcularTamañoTotal(Path raiz)`** → `long`: Tamaño total en bytes de todos los ficheros del proyecto.

4. **`encontrarFicherosGrandes(Path raiz, long umbralKB)`** → `List<Path>`: Lista de ficheros que superan el umbral, ordenados de mayor a menor tamaño.

5. **`generarInforme(Path raiz, Path informe)`**: Escribe un fichero de texto con el resumen completo del análisis.

**Requisitos:**

1. Todos los métodos usan `Files.walk()` con `try-with-resources`.
2. Ningún método usa bucles explícitos.
3. El informe incluye: fecha de análisis, total de ficheros, desglose por extensión y los 5 ficheros más grandes.
4. Las extensiones desconocidas (sin punto) se agrupan como `"sin extensión"`.

!!! example "Ejemplo de informe generado"

    ```
    INFORME DE ANÁLISIS DE PROYECTO
    Ruta analizada : C:\MisProyectos\GestionBiblioteca
    Fecha análisis : 16/04/2026 10:30:05
    ══════════════════════════════════════════════════════
    RESUMEN DE FICHEROS
    ──────────────────────────────────────────────────────
    Total ficheros : 47
    Tamaño total   : 1.23 MB
    
    Distribución por extensión:
      .java        → 32 fichero(s)
      .xml         →  5 fichero(s)
      .properties  →  4 fichero(s)
      .md          →  3 fichero(s)
      .class       →  3 fichero(s)
    
    Top 5 ficheros más grandes:
      1. target/GestionBiblioteca.jar          (245 KB)
      2. src/main/java/datos/DatosIniciales.java ( 18 KB)
      3. src/main/resources/schema.sql          ( 12 KB)
      4. README.md                              (  8 KB)
      5. src/main/java/app/Main.java            (  5 KB)
    ══════════════════════════════════════════════════════
    ```

!!! success "Criterios de evaluación"

    - [ ] `contarFicherosPorExtension` agrupa correctamente incluyendo "sin extensión".
    - [ ] `buscarFicheros` es case-insensitive y funciona con patrones parciales.
    - [ ] `calcularTamañoTotal` devuelve el valor correcto.
    - [ ] `encontrarFicherosGrandes` está ordenado correctamente.
    - [ ] El informe se genera con el formato especificado.
    - [ ] Todo el código usa `Files.walk()` con `try-with-resources` y sin bucles.

---

## 🏆 Nivel Integrador

---

### Actividad 11 — Sistema de Gestión de una Biblioteca

!!! abstract "Descripción"

    Esta actividad integra **todos los conceptos** del tema en un proyecto de aplicación real. Desarrollarás un sistema de gestión para una pequeña biblioteca que combina operaciones con el sistema de archivos, lectura/escritura de texto, procesamiento de CSV con Stream, serialización y consultas con Collectors.

#### Enunciado

#### Descripción del sistema

La biblioteca maneja dos entidades:

**`Libro implements Serializable`:**
```
- isbn: String
- titulo: String
- autor: String
- anio: int
- disponible: boolean
```

**`Prestamo implements Serializable`:**
```
- id: int
- isbnLibro: String
- nombreSocio: String
- fechaPrestamo: LocalDate
- fechaDevolucion: LocalDate  (null si aún no devuelto)
```

---

#### Funcionalidades a implementar

**Módulo 1 — Inicialización (Sección 2)**

- Clase `InicializadorBiblioteca`: crea la estructura `biblioteca/datos/`, `biblioteca/logs/`, `biblioteca/exportaciones/` si no existen.

**Módulo 2 — Catálogo desde CSV (Sección 3)**

- Clase `GestorCatalogo`:
    - `importarCatalogo(Path csv)` → `List<Libro>`: Lee el CSV del catálogo y parsea los libros.
    - `exportarCatalogo(List<Libro> libros, Path csv)`: Escribe el catálogo actualizado al CSV.
    - `buscarPorAutor(List<Libro> libros, String autor)` → `List<Libro>`: Usando Stream + `filter`.
    - `librosDisponibles(List<Libro> libros)` → `List<Libro>`: Solo los marcados como disponibles.

**Módulo 3 — Préstamos con Serialización (Sección 4)**

- Clase `GestorPrestamos`:
    - `guardarPrestamos(List<Prestamo> prestamos, Path fichero)`: Serializa la lista.
    - `cargarPrestamos(Path fichero)` → `List<Prestamo>`: Deserializa. Lista vacía si no existe.
    - `realizarPrestamo(List<Prestamo> prestamos, List<Libro> libros, String isbn, String socio)`: Crea un `Prestamo`, marca el libro como no disponible. Lanza excepción si el libro no está disponible.
    - `devolverLibro(List<Prestamo> prestamos, List<Libro> libros, int idPrestamo)`: Registra la fecha de devolución y marca el libro como disponible.

**Módulo 4 — Estadísticas con Stream (Sección 5)**

- Clase `EstadisticasBiblioteca`:
    - `librosMasPrestados(List<Prestamo> prestamos, int top)` → `List<String>`: Los ISBN más frecuentes, con Stream + Collectors.
    - `prestamosActivosPorSocio(List<Prestamo> prestamos)` → `Map<String, Long>`: Préstamos sin devolver por cada socio.
    - `generarInformeDiario(List<Libro> libros, List<Prestamo> prestamos, Path fichero)`: Escribe el informe del día al fichero de log con APPEND.

**Módulo 5 — Registro de actividad (Sección 3.2)**

- Clase `Logger`: Registra en `biblioteca/logs/actividad.log` (con APPEND) cada operación importante con timestamp.

---

#### Ficheros de datos iniciales

**`catalogo.csv`:**
```
isbn,titulo,autor,anio,disponible
978-0-06-112008-4,Matar a un ruiseñor,Harper Lee,1960,true
978-0-7432-7356-5,1984,George Orwell,1949,true
978-0-14-028329-7,El Gran Gatsby,F. Scott Fitzgerald,1925,true
978-0-06-093546-9,El Señor de los Anillos,J.R.R. Tolkien,1954,true
978-0-14-017739-8,Cien años de soledad,Gabriel García Márquez,1967,true
```

---

#### Programa principal

El `main` de la clase `BibliotecaApp` debe demostrar el ciclo completo.

!!! example "Ejemplo de salida del informe diario"

    ```
    INFORME DIARIO BIBLIOTECA - 16/04/2026
    ════════════════════════════════════════════════════════
    CATÁLOGO
      Total libros    : 5
      Disponibles     : 3
      En préstamo     : 2
    
    PRÉSTAMOS ACTIVOS
      Ana García      → "1984" (desde 14/04/2026, 2 días)
      Carlos López    → "El Gran Gatsby" (desde 16/04/2026, 0 días)
    
    TOP LIBROS MÁS PRESTADOS
      1. 978-0-7432-7356-5  (1984)           → 2 veces
      2. 978-0-14-028329-7  (El Gran Gatsby) → 1 vez
    
    LOG DE ACTIVIDAD HOY
      [10:15:02] Préstamo realizado: ISBN 978-0-7432-7356-5 → Ana García
      [10:16:44] Préstamo realizado: ISBN 978-0-14-028329-7 → Carlos López
      [10:18:33] Devolución: ISBN 978-0-7432-7356-5 ← Pedro Martín
    ════════════════════════════════════════════════════════
    ```

!!! success "Criterios de evaluación"

    - [ ] **Módulo 1:** La estructura de directorios se crea correctamente.
    - [ ] **Módulo 2:** El catálogo se importa y exporta desde/a CSV sin perder datos.
    - [ ] **Módulo 2:** `buscarPorAutor` y `librosDisponibles` usan Stream.
    - [ ] **Módulo 3:** La serialización guarda y recupera correctamente los préstamos entre ejecuciones.
    - [ ] **Módulo 3:** `realizarPrestamo` actualiza la disponibilidad del libro.
    - [ ] **Módulo 4:** `librosMasPrestados` usa Collectors y devuelve el ranking correcto.
    - [ ] **Módulo 5:** El log de actividad se acumula con APPEND.
    - [ ] **General:** Todos los recursos de I/O usan `try-with-resources`.
    - [ ] **General:** El código está organizado en clases con responsabilidad única.
    - [ ] **General:** El `main` demuestra el ciclo completo de forma coherente.
