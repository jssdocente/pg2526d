# **UT9: Ficheros**

!!! tip "Información de la unidad"

    === "Contenidos"

        Entrada/Salida por consola:

        - Lectura de datos desde teclado.
        - Visualización de información en pantalla y aplicación de formatos.

        Manejo de ficheros:

        - Concepto de flujo de datos (streams).
        - Apertura, lectura, escritura y cierre de ficheros.
        - Diferentes métodos de acceso al contenido de los ficheros (secuencial, aleatorio).
        - Gestión de excepciones en operaciones de ficheros.

    === "Propuesta didáctica"

        En esta unidad vamos a comenzar a trabajar el **RA5: Realiza operaciones de entrada y salida de información, utilizando procedimientos específicos del lenguaje y librerías de clases.**

        Criterios de evaluación clave que abordaremos:

        - **CE5a**: Se ha utilizado la consola para realizar operaciones de entrada y salida de información.
        - **CE5b**: Se han aplicado formatos en la visualización de la información.
        - **CE5c**: Se han reconocido las posibilidades de entrada / salida del lenguaje y las librerías asociadas.
        - **CE5d**: Se han utilizado ficheros para almacenar y recuperar información.
        - **CE5e**: Se han creado programas que utilicen diversos métodos de acceso al contenido de los ficheros.


    === "Programación de Aula"

        Esta unidad se imparte en la **segunda/tercera evaluación**, con una duración estimada de **15 sesiones lectivas**, aproximadamente entre la **1ª semana de marzo y la 3ª semana de marzo de 2026**. (Se impartirían a razón de 5-6 sesiones por semana).

        | Sesión | Contenidos | Criterios trabajados |
        |---|---|---|
        | 1-2 | Entrada/Salida por consola: lectura, visualización y formatos. | CE5a, CE5b |
        | 3-5 | Ficheros de texto: concepto de flujo, apertura, lectura, escritura y cierre. | CE5c, CE5d |
        | 6-8 | Métodos de acceso: secuencial y aleatorio. Gestión de excepciones. | CE5e, CE3d (Refuerzo) |
        | 9-11 | Ficheros binarios: lectura, escritura. Serialización/deserialización de objetos. | CE5d |
        | 12-14 | Práctica avanzada: gestión de datos en ficheros para una aplicación. | CE5d, CE5e |
        | 15 | Revisión y evaluación práctica de la unidad. | CE5a-e |

---

## 1. Introducción

El manejo de ficheros en Java es esencial para **persistir datos** más allá de la ejecución de un programa. Sin ficheros, toda la información almacenada en memoria desaparece al cerrar la aplicación.

Java ofrece dos grandes APIs para trabajar con el sistema de archivos:

| API | Paquete | Versión | Descripción |
|-----|---------|---------|-------------|
| **Clásica** | `java.io` | Java 1.0 | Basada en la clase `File`. Limitada y menos expresiva. |
| **Moderna (NIO.2)** | `java.nio.file` | Java 7+ | Basada en `Path`, `Paths` y `Files`. Más potente y recomendada. |

A través de estas herramientas, los programadores pueden:

- Crear, copiar, mover y eliminar archivos y directorios
- Navegar por estructuras de directorios
- Comprobar la existencia y obtener metadatos de archivos
- Leer y escribir contenido en texto plano o formato binario
- **Persistir objetos** mediante serialización
- Gestionar errores robustamente mediante `IOException`

!!! info "¿Por qué `java.nio.file` en lugar de `java.io.File`?"
    La clase antigua `java.io.File` tiene limitaciones importantes: sus métodos de error devuelven `false` en lugar de lanzar excepciones, no distingue bien entre errores de permisos o de red, y carece de soporte para atributos avanzados. **`java.nio.file` soluciona todo esto** y es la forma moderna y recomendada de trabajar con ficheros en Java.

---

## 2. Operaciones con Archivos

La API `java.nio.file` (introducida en Java 7) ofrece una interfaz más moderna y expresiva que la antigua `java.io.File`.

### Herramientas básicas

| Clase/Interfaz | Descripción |
|----------------|-------------|
| `Path` | Representa una ruta (absoluta o relativa) en el sistema de archivos |
| `Paths` | Factoría estática para crear instancias de `Path` |
| `Files` | Clase de utilidad con métodos estáticos para todas las operaciones |
| `DirectoryStream<Path>` | Permite iterar sobre el contenido de un directorio |
| `BasicFileAttributes` | Proporciona acceso a los metadatos de un archivo |

```java title="Creación de instancias Path"
// Ruta relativa al directorio de trabajo del proyecto
Path rutaRelativa = Paths.get("datos/alumnos.txt");

// Ruta absoluta
Path rutaAbsoluta = Paths.get("C:/MisDocumentos/notas.txt");

// Construcción con múltiples segmentos (independiente del SO)
Path rutaSegmentada = Paths.get("proyecto", "src", "main", "datos.csv");
```

!!! note "Separadores de ruta"
    Al usar `Paths.get("segmento1", "segmento2")` con múltiples argumentos, Java construye la ruta usando el separador del sistema operativo automáticamente (`/` en Linux/Mac, `\` en Windows). Esto hace el código **portable entre sistemas operativos**.

---

### 2.1 Crear Archivos y Directorios

Se utilizan los métodos `Files.createFile()` y `Files.createDirectories()`.

!!! warning "Importante"
    `createFile()` lanzará `FileAlreadyExistsException` si el fichero ya existe. Siempre comprueba la existencia antes de crear, o captura la excepción.

!!! example "Gestión de la carpeta de datos de una aplicación"

    Imagina que desarrollas una aplicación de gestión de una biblioteca. Al arrancar, necesitas asegurarte de que existe la carpeta `datos/` y el fichero `catalogo.txt`. Si no existen, los creas.

    ```java title="InicializadorBiblioteca.java"
    import java.io.IOException;
    import java.nio.file.*;

    public class InicializadorBiblioteca {

        public static void inicializar() {
            Path dirDatos    = Paths.get("datos");
            Path fichCatalog = Paths.get("datos", "catalogo.txt");
            Path fichPrest   = Paths.get("datos", "prestamos.txt");

            try {
                // Crear el directorio si no existe (createDirectories no falla si ya existe)
                if (!Files.exists(dirDatos)) {
                    Files.createDirectories(dirDatos);
                    System.out.println("Directorio 'datos' creado correctamente.");
                } else {
                    System.out.println("El directorio 'datos' ya existía.");
                }

                // Crear ficheros solo si no existen
                if (!Files.exists(fichCatalog)) {
                    Files.createFile(fichCatalog);
                    System.out.println("Fichero 'catalogo.txt' creado.");
                }
                if (!Files.exists(fichPrest)) {
                    Files.createFile(fichPrest);
                    System.out.println("Fichero 'prestamos.txt' creado.");
                }

            } catch (IOException e) {
                System.err.println("Error al inicializar la aplicación: " + e.getMessage());
            }
        }

        public static void main(String[] args) {
            inicializar();
            System.out.println("Aplicación lista.");
        }
    }
    ```

    **Salida esperada (primera ejecución):**
    ```
    Directorio 'datos' creado correctamente.
    Fichero 'catalogo.txt' creado.
    Fichero 'prestamos.txt' creado.
    Aplicación lista.
    ```

    **Salida esperada (segunda ejecución):**
    ```
    El directorio 'datos' ya existía.
    Aplicación lista.
    ```

---

### 2.2 Borrar Archivos

Existen dos métodos para eliminar:

| Método | Comportamiento si no existe |
|--------|----------------------------|
| `Files.delete(Path)` | Lanza `NoSuchFileException` |
| `Files.deleteIfExists(Path)` | Devuelve `false`, sin excepción |

!!! example "Sistema de limpieza de ficheros temporales"

    Supón que tu aplicación genera ficheros de log temporales que deben eliminarse al cerrar. Algunos pueden haberse borrado ya manualmente.

    ```java title="LimpiezaTemporales.java"
    import java.io.IOException;
    import java.nio.file.*;
    import java.util.List;

    public class LimpiezaTemporales {

        public static void main(String[] args) {
            List<String> temporales = List.of(
                "logs/sesion_001.tmp",
                "logs/sesion_002.tmp",
                "logs/sesion_003.tmp"  // Este ya fue borrado manualmente
            );

            int eliminados = 0;

            for (String nombre : temporales) {
                Path ruta = Paths.get(nombre);
                try {
                    boolean borrado = Files.deleteIfExists(ruta);
                    if (borrado) {
                        System.out.println("Eliminado: " + nombre);
                        eliminados++;
                    } else {
                        System.out.println("No encontrado (ya eliminado): " + nombre);
                    }
                } catch (IOException e) {
                    // Puede ocurrir si el fichero está en uso o sin permisos
                    System.err.println("No se pudo eliminar " + nombre + ": " + e.getMessage());
                }
            }

            System.out.println("\nResumen: " + eliminados + " fichero(s) eliminado(s).");
        }
    }
    ```

---

### 2.3 Copiar Archivos

El método `Files.copy(origen, destino, opciones...)` permite duplicar un fichero.

Las opciones más comunes son de la enumeración `StandardCopyOption`:

| Opción | Descripción |
|--------|-------------|
| `REPLACE_EXISTING` | Sobreescribe el destino si existe |
| `COPY_ATTRIBUTES` | Copia también los metadatos (fecha de creación, etc.) |

!!! example "Sistema de copias de seguridad automáticas"

    Al guardar el fichero de configuración de una aplicación, primero se hace una copia de seguridad `.bak` del fichero anterior.

    ```java title="GestorBackup.java"
    import java.io.IOException;
    import java.nio.file.*;
    import java.time.LocalDateTime;
    import java.time.format.DateTimeFormatter;

    public class GestorBackup {

        public static void realizarBackup(String nombreFichero) {
            Path origen  = Paths.get("config", nombreFichero);
            String timestamp = LocalDateTime.now()
                .format(DateTimeFormatter.ofPattern("yyyyMMdd_HHmmss"));
            Path destino = Paths.get("config", "backup",
                nombreFichero + "." + timestamp + ".bak");

            try {
                // Aseguramos que existe el directorio de backups
                Files.createDirectories(destino.getParent());

                // Copiamos preservando atributos del fichero original
                Files.copy(origen, destino,
                    StandardCopyOption.REPLACE_EXISTING,
                    StandardCopyOption.COPY_ATTRIBUTES);

                System.out.println("Backup realizado: " + destino.getFileName());

            } catch (NoSuchFileException e) {
                System.err.println("El fichero original no existe: " + origen);
            } catch (IOException e) {
                System.err.println("Error al realizar el backup: " + e.getMessage());
            }
        }

        public static void main(String[] args) {
            realizarBackup("app.properties");
        }
    }
    ```

---

### 2.4 Mover y Renombrar Archivos

El método `Files.move(origen, destino, opciones...)` mueve o renombra un fichero:

- Si el directorio de origen y destino **es el mismo** → **renombra** el fichero.
- Si los directorios **son distintos** → **mueve** el fichero.

!!! example "Organización automática de ficheros descargados"

    Una aplicación descarga facturas y las clasifica en carpetas por año al procesarlas.

    ```java title="OrganizadorFacturas.java"
    import java.io.IOException;
    import java.nio.file.*;

    public class OrganizadorFacturas {

        public static void moverFactura(String nombreFactura, String anio) {
            Path origen  = Paths.get("descargas", nombreFactura);
            Path destino = Paths.get("facturas", anio, nombreFactura);

            try {
                Files.createDirectories(destino.getParent());

                Files.move(origen, destino, StandardCopyOption.REPLACE_EXISTING);
                System.out.println("Factura movida a: " + destino);

            } catch (NoSuchFileException e) {
                System.err.println("La factura no existe en descargas: " + nombreFactura);
            } catch (IOException e) {
                System.err.println("Error al mover: " + e.getMessage());
            }
        }

        public static void renombrarFactura(String nombreOriginal, String nombreNuevo) {
            Path origen  = Paths.get("facturas", nombreOriginal);
            Path destino = Paths.get("facturas", nombreNuevo); // Mismo directorio = renombrar

            try {
                Files.move(origen, destino, StandardCopyOption.REPLACE_EXISTING);
                System.out.println("Renombrado: " + nombreOriginal + " → " + nombreNuevo);
            } catch (IOException e) {
                System.err.println("Error al renombrar: " + e.getMessage());
            }
        }

        public static void main(String[] args) {
            moverFactura("factura_2024_001.pdf", "2024");
            moverFactura("factura_2025_042.pdf", "2025");
            renombrarFactura("factura_2024_001.pdf", "FRA-2024-001.pdf");
        }
    }
    ```

---

### 2.5 Consultar Características de un Archivo

Métodos de la clase `Files` para verificar el estado de un `Path`:

| Método | Retorna | Descripción |
|--------|---------|-------------|
| `Files.exists(path)` | `boolean` | Verdadero si el fichero o directorio existe |
| `Files.notExists(path)` | `boolean` | Verdadero si **no** existe |
| `Files.isDirectory(path)` | `boolean` | Verdadero si es un directorio |
| `Files.isRegularFile(path)` | `boolean` | Verdadero si es un fichero normal (no directorio ni enlace) |
| `Files.isReadable(path)` | `boolean` | Verdadero si se puede leer |
| `Files.isWritable(path)` | `boolean` | Verdadero si se puede escribir |
| `Files.isHidden(path)` | `boolean` | Verdadero si es un fichero oculto |

!!! example "Validador de rutas antes de procesar"

    Antes de abrir un fichero de datos del usuario, verificamos todas sus propiedades para dar mensajes de error claros.

    ```java title="ValidadorRuta.java"
    import java.io.IOException;
    import java.nio.file.*;

    public class ValidadorRuta {

        public static boolean validarFicheroEntrada(Path ruta) {
            System.out.println("=== Validando: " + ruta + " ===");

            if (Files.notExists(ruta)) {
                System.err.println("ERROR: El fichero no existe.");
                return false;
            }

            if (Files.isDirectory(ruta)) {
                System.err.println("ERROR: La ruta es un directorio, no un fichero.");
                return false;
            }

            if (!Files.isRegularFile(ruta)) {
                System.err.println("ERROR: No es un fichero regular (puede ser un enlace simbólico).");
                return false;
            }

            if (!Files.isReadable(ruta)) {
                System.err.println("ERROR: No tienes permisos de lectura sobre el fichero.");
                return false;
            }

            System.out.println("OK: El fichero es válido y legible.");
            return true;
        }

        public static void main(String[] args) {
            Path ficheroNotas  = Paths.get("datos", "notas.csv");
            Path carpetaDatos  = Paths.get("datos");

            if (validarFicheroEntrada(ficheroNotas)) {
                System.out.println("Procediendo a procesar el fichero...");
            }

            validarFicheroEntrada(carpetaDatos); // Dará error: es directorio
        }
    }
    ```

---

### 2.6 Atributos de Archivo

Mediante `Files.readAttributes()` se pueden obtener metadatos como tamaño, fecha de creación y última modificación. Se usa con la interfaz `BasicFileAttributes`.

!!! example "Visor de información de ficheros de un proyecto"

    Mostramos los metadatos de los ficheros de un proyecto para un informe de auditoría.

    ```java title="InformeFicheros.java"
    import java.io.IOException;
    import java.nio.file.*;
    import java.nio.file.attribute.*;
    import java.time.*;
    import java.time.format.DateTimeFormatter;

    public class InformeFicheros {

        static DateTimeFormatter FORMATO = DateTimeFormatter
            .ofPattern("dd/MM/yyyy HH:mm:ss")
            .withZone(ZoneId.systemDefault());

        public static void mostrarAtributos(Path ruta) {
            try {
                BasicFileAttributes attrs = Files.readAttributes(ruta, BasicFileAttributes.class);

                System.out.println("──────────────────────────────────────");
                System.out.println("Fichero   : " + ruta.getFileName());
                System.out.println("Tamaño    : " + attrs.size() + " bytes ("
                    + (attrs.size() / 1024) + " KB)");
                System.out.println("Creado    : " + FORMATO.format(attrs.creationTime().toInstant()));
                System.out.println("Modificado: " + FORMATO.format(attrs.lastModifiedTime().toInstant()));
                System.out.println("¿Directorio?: " + attrs.isDirectory());
                System.out.println("¿Fichero?   : " + attrs.isRegularFile());

            } catch (IOException e) {
                System.err.println("No se pudieron leer los atributos de: " + ruta);
            }
        }

        public static void main(String[] args) {
            mostrarAtributos(Paths.get("datos", "catalogo.txt"));
            mostrarAtributos(Paths.get("datos", "prestamos.txt"));
        }
    }
    ```

    **Salida de ejemplo:**
    ```
    ──────────────────────────────────────
    Fichero   : catalogo.txt
    Tamaño    : 2048 bytes (2 KB)
    Creado    : 12/03/2026 09:15:32
    Modificado: 15/03/2026 11:42:05
    ¿Directorio?: false
    ¿Fichero?   : true
    ```

---

### 2.7 Contenido de un Directorio

`DirectoryStream<Path>` permite iterar de forma eficiente sobre los elementos de un directorio. Admite **filtros glob** para seleccionar solo ciertos tipos de ficheros.

!!! note "¿Qué es un patrón glob?"
    Un glob es un patrón de búsqueda similar al que usamos en la terminal. Por ejemplo `*.txt` filtra solo ficheros `.txt`, `**.java` filtra todos los `.java` en subdirectorios recursivos.

!!! example "Listado de ficheros CSV en la carpeta de exportaciones"

    Una aplicación de contabilidad lista todos los ficheros CSV generados en la carpeta de exportaciones para mostrarlos al usuario antes de importarlos.

    ```java title="ListadorExportaciones.java"
    import java.io.IOException;
    import java.nio.file.*;

    public class ListadorExportaciones {

        public static void listarCSV(Path directorio) {
            System.out.println("Ficheros CSV disponibles en: " + directorio.toAbsolutePath());
            System.out.println("──────────────────────────────────────");

            int contador = 0;

            // El filtro glob "*.csv" solo selecciona ficheros que terminan en .csv
            try (DirectoryStream<Path> stream = Files.newDirectoryStream(directorio, "*.csv")) {
                for (Path fichero : stream) {
                    long tamañoKB = Files.size(fichero) / 1024;
                    System.out.printf("  [%d] %-30s  %5d KB%n",
                        ++contador, fichero.getFileName(), tamañoKB);
                }
            } catch (NotDirectoryException e) {
                System.err.println("La ruta indicada no es un directorio.");
            } catch (IOException e) {
                System.err.println("Error al leer el directorio: " + e.getMessage());
            }

            if (contador == 0) {
                System.out.println("  (No hay ficheros CSV en esta carpeta)");
            } else {
                System.out.println("──────────────────────────────────────");
                System.out.println("Total: " + contador + " fichero(s) encontrado(s).");
            }
        }

        public static void main(String[] args) {
            listarCSV(Paths.get("exportaciones"));
        }
    }
    ```

    **Salida de ejemplo:**
    ```
    Ficheros CSV disponibles en: C:\proyecto\exportaciones
    ──────────────────────────────────────
      [1] ventas_enero_2026.csv           128 KB
      [2] ventas_febrero_2026.csv          95 KB
      [3] clientes_activos.csv             42 KB
    ──────────────────────────────────────
    Total: 3 fichero(s) encontrado(s).
    ```

---

### 2.8 Gestión de Recursos: try-with-resources

Cuando abrimos un fichero, estamos reservando un **recurso del sistema operativo** (un descriptor de fichero). Es imprescindible liberarlo siempre al terminar, aunque ocurra un error. Si no lo hacemos:

- El fichero puede quedar **bloqueado** (especialmente en Windows, que impide modificarlo desde otro proceso)
- Se pueden agotar los **descriptores de fichero** disponibles del sistema
- El contenido escrito puede no volcarse al disco si el buffer no se vacía

#### ¿Cómo se cierra un recurso?

Java 7 introdujo la sentencia **`try-with-resources`**, que cierra automáticamente cualquier objeto que implemente la interfaz `AutoCloseable`. El método `close()` se llama al salir del bloque, **independientemente de si hubo excepción o no**.

```
try (Recurso r = new Recurso()) {
    // usar r
}
// r.close() se llama SIEMPRE aquí, incluso si hubo excepción
```

#### Múltiples recursos

Se pueden declarar varios recursos en un solo `try`, separados por `;`. Se cierran en **orden inverso** al de declaración:

```java title="Múltiples recursos en un try"
// Primero se cierra escritor, luego lector
try (BufferedReader lector  = Files.newBufferedReader(origen);
     BufferedWriter escritor = Files.newBufferedWriter(destino)) {

    String linea;
    while ((linea = lector.readLine()) != null) {
        escritor.write(linea);
        escritor.newLine();
    }
} catch (IOException e) {
    System.err.println("Error: " + e.getMessage());
}
```

#### Clases de ficheros que son AutoCloseable

Todas las clases de streams de ficheros en Java implementan `AutoCloseable`:

| Clase | Uso |
|-------|-----|
| `BufferedReader` / `BufferedWriter` | Lectura/escritura de texto línea a línea |
| `ObjectInputStream` / `ObjectOutputStream` | Serialización de objetos |
| `DirectoryStream<Path>` | Iteración sobre directorios |
| `Stream<String>` (`Files.lines()`) | Stream de líneas de un fichero |

!!! warning "Files.lines() y try-with-resources"
    `Files.lines()` devuelve un `Stream<String>` que **mantiene abierto el fichero** internamente. Si no lo cierras con `try-with-resources`, el fichero quedará bloqueado hasta que el recolector de basura actúe. **Úsalo siempre en un try-with-resources.**

!!! example "Lectura de fichero de configuración: estilo antiguo vs moderno"

    === "Sin try-with-resources (estilo antiguo)"

        El enfoque clásico con `finally` es verboso y propenso a errores. Si `close()` también lanza excepción, se necesita otro bloque `try` anidado.

        ```java title="LectorConfigAntiguo.java"
        import java.io.*;
        import java.nio.file.*;

        public class LectorConfigAntiguo {

            public static String buscarValor(Path config, String clave) {
                BufferedReader reader = null;
                try {
                    reader = Files.newBufferedReader(config);
                    String linea;
                    while ((linea = reader.readLine()) != null) {
                        if (linea.startsWith(clave + "=")) {
                            return linea.substring(clave.length() + 1).trim();
                        }
                    }
                } catch (IOException e) {
                    System.err.println("Error leyendo config: " + e.getMessage());
                } finally {
                    // Hay que cerrar aunque haya excepción... pero close() también puede lanzar
                    if (reader != null) {
                        try {
                            reader.close();
                        } catch (IOException e) {
                            System.err.println("Error al cerrar: " + e.getMessage());
                        }
                    }
                }
                return null;
            }

            public static void main(String[] args) {
                Path config = Paths.get("config", "app.properties");
                System.out.println("host = " + buscarValor(config, "host"));
                System.out.println("port = " + buscarValor(config, "port"));
            }
        }
        ```

    === "Con try-with-resources (estilo moderno)"

        El recurso se declara en el paréntesis del `try`. Java llama a `close()` automáticamente, incluso si se lanza una excepción o se ejecuta un `return`.

        ```java title="LectorConfigModerno.java"
        import java.io.*;
        import java.nio.file.*;

        public class LectorConfigModerno {

            public static String buscarValor(Path config, String clave) {
                // El BufferedReader se cierra automáticamente al salir del try
                try (BufferedReader reader = Files.newBufferedReader(config)) {
                    String linea;
                    while ((linea = reader.readLine()) != null) {
                        if (linea.startsWith(clave + "=")) {
                            return linea.substring(clave.length() + 1).trim();
                        }
                    }
                } catch (IOException e) {
                    System.err.println("Error leyendo config: " + e.getMessage());
                }
                return null;
            }

            public static void main(String[] args) {
                Path config = Paths.get("config", "app.properties");
                System.out.println("host = " + buscarValor(config, "host"));
                System.out.println("port = " + buscarValor(config, "port"));
            }
        }
        ```

        Contenido de `app.properties`:
        ```
        host=localhost
        port=8080
        debug=false
        ```

        **Salida:**
        ```
        host = localhost
        port = 8080
        ```

---

## 3. Archivos de Texto

Java permite procesar archivos de texto de forma robusta. Es importante especificar siempre la **codificación de caracteres** (`StandardCharsets.UTF_8`) para evitar problemas con tildes y caracteres especiales.

!!! warning "Codificación de caracteres"
    Si no especificas la codificación al leer o escribir, Java usa la codificación por defecto del sistema operativo, que puede variar (UTF-8 en Linux/Mac, Windows-1252 en Windows). Esto puede causar que las tildes y la `ñ` aparezcan corruptas. **Usa siempre `StandardCharsets.UTF_8`**.

---

### 3.1 Leer Archivos de Texto

Existen varios métodos según la cantidad de datos y el uso que les daremos:

| Método | Cuándo usarlo |
|--------|---------------|
| `Files.readAllLines(path, charset)` | Ficheros pequeños/medianos: carga todo en una `List<String>` |
| `Files.readString(path, charset)` | Ficheros pequeños: carga todo como un único `String` |
| `Files.lines(path, charset)` | Ficheros grandes: procesa línea a línea con `Stream<String>` (lazy) |
| `BufferedReader` + `Files.newBufferedReader()` | Control total línea a línea con try-with-resources |

!!! example "Lectura del catálogo de productos desde un fichero CSV"

    Un programa de gestión de un supermercado lee el catálogo de productos almacenado en un CSV. Cada línea tiene el formato: `código,nombre,precio,stock`.

    **Contenido del fichero `catalogo.csv`:**
    ```
    # Catálogo de productos - Supermercado El Ahorrador
    P001,Leche entera 1L,1.25,500
    P002,Pan de molde integral,1.89,200
    P003,Aceite de oliva 750ml,5.99,150
    P004,Pasta espagueti 500g,0.79,300
    ```

    === "Imperativa"

        Leemos todas las líneas en una lista y las procesamos con un bucle `for`, manejando errores línea a línea con `continue`.

        ```java title="LectorCatalogo.java"
        import java.io.IOException;
        import java.nio.charset.StandardCharsets;
        import java.nio.file.*;
        import java.util.ArrayList;
        import java.util.List;

        public class LectorCatalogo {

            record Producto(String codigo, String nombre, double precio, int stock) {}

            public static List<Producto> cargarCatalogo(Path rutaCsv) {
                List<Producto> productos = new ArrayList<>();

                try {
                    List<String> lineas = Files.readAllLines(rutaCsv, StandardCharsets.UTF_8);

                    for (String linea : lineas) {
                        if (linea.isBlank() || linea.startsWith("#")) continue;

                        String[] campos = linea.split(",");
                        if (campos.length != 4) {
                            System.err.println("Línea malformada, ignorada: " + linea);
                            continue;
                        }
                        try {
                            productos.add(new Producto(
                                campos[0].trim(),
                                campos[1].trim(),
                                Double.parseDouble(campos[2].trim()),
                                Integer.parseInt(campos[3].trim())
                            ));
                        } catch (NumberFormatException e) {
                            System.err.println("Dato numérico inválido en: " + linea);
                        }
                    }

                } catch (IOException e) {
                    System.err.println("Error al leer: " + e.getMessage());
                }
                return productos;
            }

            public static void main(String[] args) {
                List<Producto> catalogo = cargarCatalogo(Paths.get("datos", "catalogo.csv"));

                System.out.printf("%-8s %-25s %8s %6s%n", "Código", "Nombre", "Precio", "Stock");
                System.out.println("─".repeat(52));
                for (Producto p : catalogo) {
                    System.out.printf("%-8s %-25s %7.2f€ %5d ud%n",
                        p.codigo(), p.nombre(), p.precio(), p.stock());
                }
                System.out.println("─".repeat(52));
                System.out.println("Total: " + catalogo.size() + " producto(s).");
            }
        }
        ```

    === "Declarativa (Stream)"

        Con `Files.lines()` construimos un pipeline: filtramos comentarios, transformamos cada línea a un `Producto` con `map()` y recogemos en una lista con `collect()`.

        ```java title="LectorCatalogoStream.java"
        import java.io.IOException;
        import java.nio.charset.StandardCharsets;
        import java.nio.file.*;
        import java.util.List;
        import java.util.stream.Collectors;

        public class LectorCatalogoStream {

            record Producto(String codigo, String nombre, double precio, int stock) {}

            private static Producto parsearLinea(String linea) {
                String[] c = linea.split(",");
                return new Producto(
                    c[0].trim(), c[1].trim(),
                    Double.parseDouble(c[2].trim()),
                    Integer.parseInt(c[3].trim())
                );
            }

            public static List<Producto> cargarCatalogo(Path rutaCsv) {
                try (var lineas = Files.lines(rutaCsv, StandardCharsets.UTF_8)) {
                    return lineas
                        .filter(l -> !l.isBlank() && !l.startsWith("#")) // quitar comentarios
                        .filter(l -> l.split(",").length == 4)           // descartar malformadas
                        .map(LectorCatalogoStream::parsearLinea)          // String → Producto
                        .collect(Collectors.toList());                    // resultado en lista
                } catch (IOException e) {
                    System.err.println("Error al leer: " + e.getMessage());
                    return List.of();
                }
            }

            public static void main(String[] args) {
                List<Producto> catalogo = cargarCatalogo(Paths.get("datos", "catalogo.csv"));

                System.out.printf("%-8s %-25s %8s %6s%n", "Código", "Nombre", "Precio", "Stock");
                System.out.println("─".repeat(52));
                catalogo.forEach(p -> System.out.printf("%-8s %-25s %7.2f€ %5d ud%n",
                    p.codigo(), p.nombre(), p.precio(), p.stock()));
                System.out.println("─".repeat(52));
                System.out.println("Total: " + catalogo.size() + " producto(s).");
            }
        }
        ```

    **Salida esperada (ambas versiones):**
    ```
    Código   Nombre                    Precio  Stock
    ────────────────────────────────────────────────────
    P001     Leche entera 1L            1.25€   500 ud
    P002     Pan de molde integral      1.89€   200 ud
    P003     Aceite de oliva 750ml      5.99€   150 ud
    P004     Pasta espagueti 500g       0.79€   300 ud
    ────────────────────────────────────────────────────
    Total: 4 producto(s).
    ```

!!! example "Análisis de un fichero de log de servidor"

    Para ficheros con millones de líneas (logs de servidor, datos históricos), cargar todo en memoria es inviable. `Files.lines()` procesa las líneas bajo demanda (*lazy evaluation*).

    === "Imperativa"

        Cargamos todas las líneas en memoria con `readAllLines` y recorremos la lista con un `for`.

        ```java title="AnalizadorLogs.java"
        import java.io.IOException;
        import java.nio.charset.StandardCharsets;
        import java.nio.file.*;
        import java.util.List;

        public class AnalizadorLogs {

            public static void analizarLog(Path rutaLog) {
                System.out.println("Analizando log: " + rutaLog.getFileName());

                try {
                    List<String> lineas = Files.readAllLines(rutaLog, StandardCharsets.UTF_8);

                    long errores   = 0;
                    long warnings  = 0;
                    int  mostradas = 0;

                    System.out.println("Últimas líneas de ERROR:");
                    for (String linea : lineas) {
                        if (linea.contains("[ERROR]")) {
                            errores++;
                            if (mostradas < 3) {
                                System.out.println("  " + linea);
                                mostradas++;
                            }
                        } else if (linea.contains("[WARN]")) {
                            warnings++;
                        }
                    }

                    System.out.printf("%nResumen: %d error(es), %d warning(s)%n",
                        errores, warnings);

                } catch (IOException e) {
                    System.err.println("Error al leer el log: " + e.getMessage());
                }
            }

            public static void main(String[] args) {
                analizarLog(Paths.get("logs", "app_2026_03_15.log"));
            }
        }
        ```

    === "Declarativa (Stream)"

        `Files.lines()` genera un `Stream<String>` *lazy*: solo lee del disco lo que necesita. El fichero queda cerrado al salir del `try-with-resources`.

        ```java title="AnalizadorLogsStream.java"
        import java.io.IOException;
        import java.nio.charset.StandardCharsets;
        import java.nio.file.*;

        public class AnalizadorLogsStream {

            public static void analizarLog(Path rutaLog) {
                System.out.println("Analizando log: " + rutaLog.getFileName());

                try (var lineas = Files.lines(rutaLog, StandardCharsets.UTF_8)) {
                    // Primero contamos errores (consume el stream)
                    long errores = lineas.filter(l -> l.contains("[ERROR]")).count();
                    System.out.println("Total errores: " + errores);
                } catch (IOException e) {
                    System.err.println("Error: " + e.getMessage());
                }

                // Segundo stream independiente para mostrar las primeras líneas de error
                System.out.println("Primeras líneas de ERROR:");
                try (var lineas = Files.lines(rutaLog, StandardCharsets.UTF_8)) {
                    lineas
                        .filter(l -> l.contains("[ERROR]"))
                        .limit(3)
                        .forEach(l -> System.out.println("  " + l));
                } catch (IOException e) {
                    System.err.println("Error: " + e.getMessage());
                }
            }

            public static void main(String[] args) {
                analizarLog(Paths.get("logs", "app_2026_03_15.log"));
            }
        }
        ```

        !!! note "Los streams solo se pueden consumir una vez"
            Un `Stream` no es reutilizable. Una vez que llamas a una operación terminal (`count`, `forEach`, `collect`...) queda **cerrado**. Si necesitas hacer varias operaciones distintas sobre el mismo fichero, abre un nuevo stream para cada una.

---

### 3.2 Escribir Archivos de Texto

Existen dos estrategias principales:

#### Escritura completa con `Files.write()`

Ideal cuando tenemos todos los datos listos en memoria.

| Opción `OpenOption` | Descripción |
|---------------------|-------------|
| `StandardOpenOption.CREATE` | Crea el fichero si no existe |
| `StandardOpenOption.TRUNCATE_EXISTING` | Vacía el fichero antes de escribir (comportamiento por defecto) |
| `StandardOpenOption.APPEND` | Añade contenido al final sin borrar lo existente |

!!! example "Exportar resultados de alumnos a un fichero de texto"

    Al finalizar el trimestre, el programa exporta las notas de los alumnos a un fichero de texto listo para imprimir.

    === "Imperativa"

        Construimos la lista de líneas del informe iterando con un bucle `for` y contando aprobados con un contador manual.

        ```java title="ExportadorNotas.java"
        import java.io.IOException;
        import java.nio.charset.StandardCharsets;
        import java.nio.file.*;
        import java.time.LocalDate;
        import java.time.format.DateTimeFormatter;
        import java.util.ArrayList;
        import java.util.List;

        public class ExportadorNotas {

            record Alumno(String nombre, double notaExamen, double notaPractica) {
                double notaFinal() { return notaExamen * 0.6 + notaPractica * 0.4; }
                String estado()    { return notaFinal() >= 5.0 ? "APROBADO" : "SUSPENSO"; }
            }

            public static void exportarInforme(List<Alumno> alumnos, Path rutaSalida) {
                List<String> lineas = new ArrayList<>();
                String fecha = LocalDate.now().format(DateTimeFormatter.ofPattern("dd/MM/yyyy"));

                lineas.add("INFORME DE NOTAS - 1º DAW - MÓDULO PROGRAMACIÓN");
                lineas.add("Fecha de emisión: " + fecha);
                lineas.add("═".repeat(65));
                lineas.add(String.format("%-25s %8s %10s %8s %-10s",
                    "Alumno/a", "Examen", "Práctica", "Final", "Estado"));
                lineas.add("─".repeat(65));

                long aprobados = 0;
                for (Alumno a : alumnos) {
                    lineas.add(String.format("%-25s %7.1f  %9.1f  %7.1f  %-10s",
                        a.nombre(), a.notaExamen(), a.notaPractica(),
                        a.notaFinal(), a.estado()));
                    if (a.notaFinal() >= 5.0) aprobados++;
                }

                lineas.add("═".repeat(65));
                lineas.add(String.format("Aprobados: %d / %d  (%.1f%%)",
                    aprobados, alumnos.size(), (aprobados * 100.0) / alumnos.size()));

                try {
                    Files.createDirectories(rutaSalida.getParent());
                    Files.write(rutaSalida, lineas, StandardCharsets.UTF_8,
                        StandardOpenOption.CREATE, StandardOpenOption.TRUNCATE_EXISTING);
                    System.out.println("Informe exportado: " + rutaSalida);
                } catch (IOException e) {
                    System.err.println("Error al exportar: " + e.getMessage());
                }
            }

            public static void main(String[] args) {
                List<Alumno> clase = List.of(
                    new Alumno("Ana García López",     8.5, 9.0),
                    new Alumno("Carlos Pérez Ruiz",    4.0, 6.5),
                    new Alumno("María Torres Vega",    7.0, 8.0),
                    new Alumno("Javier Sánchez Díaz",  3.5, 4.0),
                    new Alumno("Lucía Martín Flores",  6.0, 7.5)
                );
                exportarInforme(clase, Paths.get("informes", "notas_1T.txt"));
            }
        }
        ```

    === "Declarativa (Stream)"

        Usamos `stream()` sobre la lista de alumnos, `map()` para generar cada línea del informe y `Collectors.joining("\n")` para unirlas. Para las estadísticas usamos `Collectors.counting()`.

        ```java title="ExportadorNotasStream.java"
        import java.io.IOException;
        import java.nio.charset.StandardCharsets;
        import java.nio.file.*;
        import java.time.LocalDate;
        import java.time.format.DateTimeFormatter;
        import java.util.List;
        import java.util.stream.Collectors;

        public class ExportadorNotasStream {

            record Alumno(String nombre, double notaExamen, double notaPractica) {
                double notaFinal() { return notaExamen * 0.6 + notaPractica * 0.4; }
                String estado()    { return notaFinal() >= 5.0 ? "APROBADO" : "SUSPENSO"; }
            }

            public static void exportarInforme(List<Alumno> alumnos, Path rutaSalida) {
                String fecha = LocalDate.now().format(DateTimeFormatter.ofPattern("dd/MM/yyyy"));

                // Generamos las filas de datos del informe con map()
                String filasAlumnos = alumnos.stream()
                    .map(a -> String.format("%-25s %7.1f  %9.1f  %7.1f  %-10s",
                        a.nombre(), a.notaExamen(), a.notaPractica(),
                        a.notaFinal(), a.estado()))
                    .collect(Collectors.joining("\n"));

                // Contamos aprobados con filter() + count()
                long aprobados = alumnos.stream()
                    .filter(a -> a.notaFinal() >= 5.0)
                    .count();

                // Construimos el informe completo
                String informe = String.join("\n",
                    "INFORME DE NOTAS - 1º DAW - MÓDULO PROGRAMACIÓN",
                    "Fecha de emisión: " + fecha,
                    "═".repeat(65),
                    String.format("%-25s %8s %10s %8s %-10s",
                        "Alumno/a", "Examen", "Práctica", "Final", "Estado"),
                    "─".repeat(65),
                    filasAlumnos,
                    "═".repeat(65),
                    String.format("Aprobados: %d / %d  (%.1f%%)",
                        aprobados, alumnos.size(), (aprobados * 100.0) / alumnos.size())
                );

                try {
                    Files.createDirectories(rutaSalida.getParent());
                    Files.writeString(rutaSalida, informe, StandardCharsets.UTF_8,
                        StandardOpenOption.CREATE, StandardOpenOption.TRUNCATE_EXISTING);
                    System.out.println("Informe exportado: " + rutaSalida);
                } catch (IOException e) {
                    System.err.println("Error al exportar: " + e.getMessage());
                }
            }

            public static void main(String[] args) {
                List<Alumno> clase = List.of(
                    new Alumno("Ana García López",     8.5, 9.0),
                    new Alumno("Carlos Pérez Ruiz",    4.0, 6.5),
                    new Alumno("María Torres Vega",    7.0, 8.0),
                    new Alumno("Javier Sánchez Díaz",  3.5, 4.0),
                    new Alumno("Lucía Martín Flores",  6.0, 7.5)
                );
                exportarInforme(clase, Paths.get("informes", "notas_1T.txt"));
            }
        }
        ```

    **Fichero generado `notas_1T.txt` (ambas versiones):**
    ```
    INFORME DE NOTAS - 1º DAW - MÓDULO PROGRAMACIÓN
    Fecha de emisión: 16/04/2026
    ═════════════════════════════════════════════════════════════════
    Alumno/a                   Examen   Práctica    Final Estado
    ─────────────────────────────────────────────────────────────────
    Ana García López              8.5        9.0      8.7  APROBADO
    Carlos Pérez Ruiz             4.0        6.5      5.0  APROBADO
    María Torres Vega             7.0        8.0      7.4  APROBADO
    Javier Sánchez Díaz           3.5        4.0      3.7  SUSPENSO
    Lucía Martín Flores           6.0        7.5      6.6  APROBADO
    ═════════════════════════════════════════════════════════════════
    Aprobados: 4 / 5  (80.0%)
    ```

!!! example "Registro de eventos con modo APPEND (añadir al final)"

    Un sistema de registro de accesos añade cada nuevo evento al final del fichero de log sin borrar lo anterior.

    ```java title="RegistroAccesos.java"
    import java.io.IOException;
    import java.nio.charset.StandardCharsets;
    import java.nio.file.*;
    import java.time.LocalDateTime;
    import java.time.format.DateTimeFormatter;
    import java.util.List;

    public class RegistroAccesos {

        private static final Path FICHERO_LOG = Paths.get("logs", "accesos.log");
        private static final DateTimeFormatter FMT =
            DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");

        public static void registrar(String usuario, String accion, boolean exito) {
            String timestamp = LocalDateTime.now().format(FMT);
            String estado    = exito ? "OK  " : "FAIL";
            String linea     = String.format("[%s] [%s] Usuario: %-15s Acción: %s%n",
                timestamp, estado, usuario, accion);

            try {
                Files.createDirectories(FICHERO_LOG.getParent());
                // APPEND: nunca borra el contenido existente
                Files.write(FICHERO_LOG, List.of(linea), StandardCharsets.UTF_8,
                    StandardOpenOption.CREATE, StandardOpenOption.APPEND);
            } catch (IOException e) {
                System.err.println("Error al escribir en el log: " + e.getMessage());
            }
        }

        public static void main(String[] args) {
            registrar("admin",       "LOGIN",        true);
            registrar("alumno01",    "LOGIN",        true);
            registrar("alumno02",    "LOGIN",        false);
            registrar("alumno01",    "VER_NOTAS",    true);
            registrar("alumno_hack", "ACCESO_ADMIN", false);
            System.out.println("Log actualizado: " + FICHERO_LOG);
        }
    }
    ```

    **Contenido del fichero `accesos.log`:**
    ```
    [2026-03-15 09:01:12] [OK  ] Usuario: admin           Acción: LOGIN
    [2026-03-15 09:02:45] [OK  ] Usuario: alumno01        Acción: LOGIN
    [2026-03-15 09:03:11] [FAIL] Usuario: alumno02        Acción: LOGIN
    [2026-03-15 09:05:30] [OK  ] Usuario: alumno01        Acción: VER_NOTAS
    [2026-03-15 09:07:02] [FAIL] Usuario: alumno_hack     Acción: ACCESO_ADMIN
    ```

---

### 3.3 Procesamiento de Ficheros de Texto: CSV y Transformación de Datos

Leer un fichero de texto es solo el primer paso. En la práctica, los datos deben **parsearse**, **validarse**, **transformarse** en objetos del dominio y, frecuentemente, **filtrarse**, **ordenarse** y **exportarse** de nuevo.

#### Estructura típica de un CSV

Un **CSV** (*Comma-Separated Values*) es un formato de texto donde cada línea es un registro y los campos se separan con comas (u otro delimitador). Puede incluir:

- Una **cabecera** en la primera línea con los nombres de las columnas
- Líneas de **comentario** que empiezan con `#`
- Líneas en blanco que deben ignorarse

```
# Fichero: notas_alumnos.csv
# Generado: 2026-03-15
nombre,nota_examen,nota_practica,modulo
Ana García López,8.5,9.0,Programación
Carlos Pérez Ruiz,4.0,6.5,Programación
María Torres Vega,7.0,8.0,Programación
Javier Sánchez Díaz,3.5,4.0,Programación
Lucía Martín Flores,6.0,7.5,Programación
```

#### Patrón de procesamiento: Leer → Parsear → Transformar → Escribir

!!! example "Pipeline completo: notas CSV → filtrar → ordenar → exportar informe"

    Cargamos el CSV de notas, lo transformamos a objetos `Alumno`, filtramos los aprobados, los ordenamos por nota final descendente, y generamos un nuevo CSV de resultados.

    === "Imperativa"

        Cada paso es explícito: bucles `for`, condicionales `if`, ordenación con `Comparator`.

        ```java title="ProcesadorNotas.java"
        import java.io.IOException;
        import java.nio.charset.StandardCharsets;
        import java.nio.file.*;
        import java.util.*;

        public class ProcesadorNotas {

            record Alumno(String nombre, double examen, double practica, String modulo) {
                double notaFinal() { return examen * 0.6 + practica * 0.4; }
                boolean aprobado() { return notaFinal() >= 5.0; }
            }

            // PASO 1: Leer y parsear el CSV a objetos de dominio
            public static List<Alumno> parsearCSV(Path ruta) throws IOException {
                List<Alumno> alumnos = new ArrayList<>();
                List<String> lineas  = Files.readAllLines(ruta, StandardCharsets.UTF_8);
                boolean primeraLinea = true;

                for (String linea : lineas) {
                    // Saltar comentarios y líneas vacías
                    if (linea.isBlank() || linea.startsWith("#")) continue;
                    // Saltar la cabecera
                    if (primeraLinea) { primeraLinea = false; continue; }

                    String[] campos = linea.split(",");
                    if (campos.length < 4) {
                        System.err.println("Línea ignorada (faltan campos): " + linea);
                        continue;
                    }
                    try {
                        alumnos.add(new Alumno(
                            campos[0].trim(),
                            Double.parseDouble(campos[1].trim()),
                            Double.parseDouble(campos[2].trim()),
                            campos[3].trim()
                        ));
                    } catch (NumberFormatException e) {
                        System.err.println("Error numérico en línea: " + linea);
                    }
                }
                return alumnos;
            }

            // PASO 2: Filtrar solo aprobados y ordenar por nota descendente
            public static List<Alumno> filtrarYOrdenar(List<Alumno> alumnos) {
                List<Alumno> aprobados = new ArrayList<>();
                for (Alumno a : alumnos) {
                    if (a.aprobado()) aprobados.add(a);
                }
                // Ordenar de mayor a menor nota final
                aprobados.sort(Comparator.comparingDouble(Alumno::notaFinal).reversed());
                return aprobados;
            }

            // PASO 3: Escribir el resultado como nuevo CSV
            public static void escribirResultados(List<Alumno> aprobados, Path destino)
                    throws IOException {
                List<String> lineas = new ArrayList<>();
                lineas.add("nombre,nota_final,estado");       // cabecera del CSV de salida
                for (Alumno a : aprobados) {
                    lineas.add(String.format("%s,%.2f,APROBADO", a.nombre(), a.notaFinal()));
                }
                Files.createDirectories(destino.getParent());
                Files.write(destino, lineas, StandardCharsets.UTF_8,
                    StandardOpenOption.CREATE, StandardOpenOption.TRUNCATE_EXISTING);
            }

            public static void main(String[] args) {
                Path entrada = Paths.get("datos", "notas_alumnos.csv");
                Path salida  = Paths.get("resultados", "aprobados.csv");

                try {
                    List<Alumno> todos      = parsearCSV(entrada);
                    List<Alumno> aprobados  = filtrarYOrdenar(todos);
                    escribirResultados(aprobados, salida);

                    System.out.printf("Procesados: %d alumnos → %d aprobados%n",
                        todos.size(), aprobados.size());
                    System.out.println("Resultado en: " + salida);

                } catch (IOException e) {
                    System.err.println("Error: " + e.getMessage());
                }
            }
        }
        ```

    === "Declarativa (Stream)"

        El pipeline de operaciones es directo: `filter` para saltar cabecera/comentarios, `map` para parsear, `filter` para aprobados, `sorted` para ordenar, `collect` para el CSV de salida.

        ```java title="ProcesadorNotasStream.java"
        import java.io.IOException;
        import java.nio.charset.StandardCharsets;
        import java.nio.file.*;
        import java.util.Comparator;
        import java.util.List;
        import java.util.concurrent.atomic.AtomicBoolean;
        import java.util.stream.Collectors;

        public class ProcesadorNotasStream {

            record Alumno(String nombre, double examen, double practica, String modulo) {
                double notaFinal() { return examen * 0.6 + practica * 0.4; }
                boolean aprobado() { return notaFinal() >= 5.0; }
            }

            private static Alumno parsear(String linea) {
                String[] c = linea.split(",");
                return new Alumno(c[0].trim(),
                    Double.parseDouble(c[1].trim()),
                    Double.parseDouble(c[2].trim()),
                    c[3].trim());
            }

            public static void procesar(Path entrada, Path salida) {
                // AtomicBoolean permite modificar la variable dentro de una lambda
                AtomicBoolean cabeceraVista = new AtomicBoolean(false);

                List<Alumno> aprobados;

                // PASO 1+2: Leer, parsear, filtrar y ordenar en un solo pipeline
                try (var lineas = Files.lines(entrada, StandardCharsets.UTF_8)) {
                    aprobados = lineas
                        .filter(l -> !l.isBlank() && !l.startsWith("#"))  // quitar comentarios
                        .filter(l -> {                                      // saltar cabecera
                            if (!cabeceraVista.get()) {
                                cabeceraVista.set(true);
                                return false; // descarta la primera línea de datos
                            }
                            return true;
                        })
                        .filter(l -> l.split(",").length >= 4)             // líneas válidas
                        .map(ProcesadorNotasStream::parsear)                // → Alumno
                        .filter(Alumno::aprobado)                          // solo aprobados
                        .sorted(Comparator.comparingDouble(Alumno::notaFinal).reversed())
                        .collect(Collectors.toList());

                } catch (IOException e) {
                    System.err.println("Error al leer: " + e.getMessage());
                    return;
                }

                // PASO 3: Escribir CSV de resultados
                // Generamos las líneas del CSV con map() + joining()
                String csvSalida = "nombre,nota_final,estado\n" +
                    aprobados.stream()
                        .map(a -> String.format("%s,%.2f,APROBADO", a.nombre(), a.notaFinal()))
                        .collect(Collectors.joining("\n"));

                try {
                    Files.createDirectories(salida.getParent());
                    Files.writeString(salida, csvSalida, StandardCharsets.UTF_8,
                        StandardOpenOption.CREATE, StandardOpenOption.TRUNCATE_EXISTING);

                    System.out.printf("Procesados: %d aprobados%n", aprobados.size());
                    System.out.println("Resultado en: " + salida);

                } catch (IOException e) {
                    System.err.println("Error al escribir: " + e.getMessage());
                }
            }

            public static void main(String[] args) {
                procesar(
                    Paths.get("datos", "notas_alumnos.csv"),
                    Paths.get("resultados", "aprobados.csv")
                );
            }
        }
        ```

    **Fichero de entrada `notas_alumnos.csv`:**
    ```
    # Fichero: notas_alumnos.csv
    nombre,nota_examen,nota_practica,modulo
    Ana García López,8.5,9.0,Programación
    Carlos Pérez Ruiz,4.0,6.5,Programación
    María Torres Vega,7.0,8.0,Programación
    Javier Sánchez Díaz,3.5,4.0,Programación
    Lucía Martín Flores,6.0,7.5,Programación
    ```

    **Fichero generado `aprobados.csv`:**
    ```
    nombre,nota_final,estado
    Ana García López,8.70,APROBADO
    María Torres Vega,7.40,APROBADO
    Lucía Martín Flores,6.60,APROBADO
    Carlos Pérez Ruiz,5.00,APROBADO
    ```

---

## 4. Archivos Binarios y Serialización

Los archivos binarios almacenan datos en formato binario (bytes), no en texto legible. En Java, el mecanismo principal para guardar objetos en ficheros binarios es la **serialización**.

### ¿Qué es la serialización?

**Serializar** un objeto significa convertirlo a una secuencia de bytes que puede guardarse en un fichero o transmitirse por red. **Deserializar** es el proceso inverso: reconstruir el objeto a partir de esos bytes.

```
Objeto en memoria  ──serializar──▶  Bytes en fichero
Bytes en fichero  ──deserializar──▶  Objeto en memoria
```

### Requisitos para serializar

1. La clase debe **implementar la interfaz `Serializable`** (es una interfaz marcador, sin métodos).
2. Todos los atributos de la clase también deben ser serializables (los tipos primitivos y `String` ya lo son).
3. Se recomienda declarar un campo `serialVersionUID` para el control de versiones.

```java title="Clase Serializable básica"
import java.io.Serializable;

public class Producto implements Serializable {

    // Identificador de versión de la clase (muy recomendado)
    private static final long serialVersionUID = 1L;

    private String codigo;
    private String nombre;
    private double precio;

    // Constructor, getters, toString...
}
```

!!! note "¿Para qué sirve `serialVersionUID`?"
    Si modificas la clase después de serializar objetos (añadir/quitar campos), el `serialVersionUID` permite a Java detectar si la versión del fichero es compatible con la clase actual. Si no lo declaras, Java genera uno automático que puede cambiar al recompilar, haciendo los ficheros ilegibles.

---

### 4.1 Grabar Objetos (Serialización)

Se emplea `ObjectOutputStream` en combinación con `Files.newOutputStream()`.

!!! example "Guardar el carrito de la compra de un usuario"

    Una tienda online guarda el carrito del usuario cuando cierra sesión para recuperarlo al volver.

    ```java title="GestorCarrito.java"
    import java.io.*;
    import java.nio.file.*;
    import java.util.ArrayList;
    import java.util.List;

    // Clase Producto (debe estar en su propio fichero en un proyecto real)
    class Producto implements Serializable {
        private static final long serialVersionUID = 1L;

        private final String codigo;
        private final String nombre;
        private final double precio;
        private int cantidad;

        public Producto(String codigo, String nombre, double precio, int cantidad) {
            this.codigo   = codigo;
            this.nombre   = nombre;
            this.precio   = precio;
            this.cantidad = cantidad;
        }

        public String getCodigo()  { return codigo; }
        public String getNombre()  { return nombre; }
        public double getPrecio()  { return precio; }
        public int    getCantidad(){ return cantidad; }
        public double getSubtotal(){ return precio * cantidad; }

        @Override
        public String toString() {
            return String.format("%s - %-20s x%d  %.2f€",
                codigo, nombre, cantidad, getSubtotal());
        }
    }

    public class GestorCarrito {

        private static final Path DIR_CARRITOS = Paths.get("datos", "carritos");

        public static void guardarCarrito(String usuario, List<Producto> carrito) {
            Path rutaFichero = DIR_CARRITOS.resolve(usuario + "_carrito.dat");

            try {
                Files.createDirectories(DIR_CARRITOS);

                // ObjectOutputStream envuelve a OutputStream para serializar objetos
                try (ObjectOutputStream oos = new ObjectOutputStream(
                        Files.newOutputStream(rutaFichero))) {

                    oos.writeObject(carrito); // Serializa la lista completa
                    System.out.println("Carrito de '" + usuario + "' guardado ("
                        + carrito.size() + " producto(s)).");
                }

            } catch (IOException e) {
                System.err.println("Error al guardar el carrito: " + e.getMessage());
            }
        }

        public static void main(String[] args) {
            List<Producto> carrito = new ArrayList<>();
            carrito.add(new Producto("P001", "Leche entera 1L",      1.25, 3));
            carrito.add(new Producto("P003", "Aceite de oliva 750ml", 5.99, 1));
            carrito.add(new Producto("P004", "Pasta espagueti 500g",  0.79, 4));

            guardarCarrito("alumno01", carrito);
        }
    }
    ```

---

### 4.2 Recuperar Objetos (Deserialización)

Se emplean `ObjectInputStream` y `Files.newInputStream()` para reconstruir los objetos originales.

!!! warning "La excepción `ClassNotFoundException`"
    Al deserializar, Java necesita encontrar la clase del objeto en el classpath. Si la clase no está disponible, lanza `ClassNotFoundException`. Siempre captura esta excepción al deserializar.

!!! example "Recuperar el carrito de la compra al iniciar sesión"

    Continuando con el ejemplo anterior, al identificarse el usuario recuperamos su carrito guardado.

    ```java title="RecuperarCarrito.java"
    import java.io.*;
    import java.nio.file.*;
    import java.util.List;

    public class RecuperarCarrito {

        private static final Path DIR_CARRITOS = Paths.get("datos", "carritos");

        @SuppressWarnings("unchecked")
        public static List<Producto> cargarCarrito(String usuario) {
            Path rutaFichero = DIR_CARRITOS.resolve(usuario + "_carrito.dat");

            if (!Files.exists(rutaFichero)) {
                System.out.println("No hay carrito guardado para: " + usuario);
                return new java.util.ArrayList<>();
            }

            try (ObjectInputStream ois = new ObjectInputStream(
                    Files.newInputStream(rutaFichero))) {

                // El cast es necesario; la anotación suprime el warning del compilador
                List<Producto> carrito = (List<Producto>) ois.readObject();
                System.out.println("Carrito recuperado para '" + usuario
                    + "' (" + carrito.size() + " producto(s)):");
                return carrito;

            } catch (ClassNotFoundException e) {
                System.err.println("ERROR: Clase no encontrada al deserializar: " + e.getMessage());
            } catch (InvalidClassException e) {
                System.err.println("ERROR: El fichero no es compatible con la versión actual de la clase.");
            } catch (IOException e) {
                System.err.println("ERROR de lectura: " + e.getMessage());
            }

            return new java.util.ArrayList<>();
        }

        public static void mostrarCarrito(List<Producto> carrito) {
            if (carrito.isEmpty()) {
                System.out.println("  (carrito vacío)");
                return;
            }
            System.out.println("─".repeat(50));
            double total = 0;
            for (Producto p : carrito) {
                System.out.println("  " + p);
                total += p.getSubtotal();
            }
            System.out.println("─".repeat(50));
            System.out.printf("  TOTAL: %.2f€%n", total);
        }

        public static void main(String[] args) {
            List<Producto> carrito = cargarCarrito("alumno01");
            mostrarCarrito(carrito);
        }
    }
    ```

    **Salida esperada:**
    ```
    Carrito recuperado para 'alumno01' (3 producto(s)):
    ──────────────────────────────────────────────────
      P001 - Leche entera 1L          x3   3.75€
      P003 - Aceite de oliva 750ml    x1   5.99€
      P004 - Pasta espagueti 500g     x4   3.16€
    ──────────────────────────────────────────────────
      TOTAL: 12.90€
    ```

!!! example "Sistema completo de persistencia de una agenda de contactos"

    Ejemplo integrador: una agenda que serializa y deserializa una lista de contactos.

    ```java title="AgendaContactos.java"
    import java.io.*;
    import java.nio.file.*;
    import java.util.*;

    class Contacto implements Serializable {
        private static final long serialVersionUID = 1L;

        private String nombre;
        private String telefono;
        private String email;

        public Contacto(String nombre, String telefono, String email) {
            this.nombre   = nombre;
            this.telefono = telefono;
            this.email    = email;
        }

        @Override
        public String toString() {
            return String.format("%-20s %-15s %s", nombre, telefono, email);
        }
    }

    public class AgendaContactos {

        private static final Path FICHERO = Paths.get("datos", "agenda.dat");
        private final List<Contacto> contactos = new ArrayList<>();

        public void cargar() {
            if (!Files.exists(FICHERO)) {
                System.out.println("Agenda nueva (sin datos previos).");
                return;
            }
            try (ObjectInputStream ois = new ObjectInputStream(
                    Files.newInputStream(FICHERO))) {

                @SuppressWarnings("unchecked")
                List<Contacto> cargados = (List<Contacto>) ois.readObject();
                contactos.addAll(cargados);
                System.out.println(contactos.size() + " contacto(s) cargado(s).");

            } catch (ClassNotFoundException | IOException e) {
                System.err.println("Error al cargar la agenda: " + e.getMessage());
            }
        }

        public void guardar() {
            try {
                Files.createDirectories(FICHERO.getParent());
                try (ObjectOutputStream oos = new ObjectOutputStream(
                        Files.newOutputStream(FICHERO))) {
                    oos.writeObject(contactos);
                    System.out.println("Agenda guardada (" + contactos.size() + " contacto(s)).");
                }
            } catch (IOException e) {
                System.err.println("Error al guardar la agenda: " + e.getMessage());
            }
        }

        public void agregar(Contacto c) {
            contactos.add(c);
            System.out.println("Contacto añadido: " + c.toString().split(" ")[0]);
        }

        public void listar() {
            if (contactos.isEmpty()) {
                System.out.println("La agenda está vacía.");
                return;
            }
            System.out.println("─".repeat(60));
            System.out.printf("%-20s %-15s %s%n", "Nombre", "Teléfono", "Email");
            System.out.println("─".repeat(60));
            contactos.forEach(c -> System.out.println(c));
            System.out.println("─".repeat(60));
        }

        public static void main(String[] args) {
            AgendaContactos agenda = new AgendaContactos();
            agenda.cargar();
            agenda.agregar(new Contacto("Ana García",   "612 345 678", "ana@email.com"));
            agenda.agregar(new Contacto("Carlos Pérez", "698 765 432", "carlos@email.com"));
            agenda.agregar(new Contacto("María Torres", "655 111 222", "maria@email.com"));
            System.out.println("\n=== AGENDA ACTUAL ===");
            agenda.listar();
            agenda.guardar();
        }
    }
    ```

---

## 5. API Stream de Java Aplicada a Ficheros

La **API Stream** (introducida en Java 8) permite procesar colecciones de datos de forma **declarativa** y **funcional**, construyendo *pipelines* de operaciones que se ejecutan de forma lazy (solo cuando es necesario).

En el contexto de ficheros, el punto de entrada principal es `Files.lines()`, que devuelve un `Stream<String>` donde cada elemento es una línea del fichero.

```
Files.lines(path)         ← Fuente: cada línea es un elemento del Stream
    .filter(...)          ← Operación intermedia: filtra elementos
    .map(...)             ← Operación intermedia: transforma cada elemento
    .sorted(...)          ← Operación intermedia: ordena
    .collect(...)         ← Operación terminal: produce el resultado
```

!!! info "Diferencias clave entre Stream y colección"
    - Una **colección** almacena datos en memoria. Un **Stream** describe operaciones sobre datos.
    - Los Streams son **lazy**: las operaciones intermedias no se ejecutan hasta que hay una operación terminal.
    - Un Stream **no es reutilizable**: una vez consumido (operación terminal ejecutada), no puede volver a usarse.
    - Deben **cerrarse** si tienen fuente de I/O (usa siempre `try-with-resources`).

---

### 5.1 Operaciones Intermedias

Las operaciones intermedias **devuelven un nuevo Stream** y no se ejecutan hasta que hay una operación terminal. Son la base del pipeline.

| Método | Firma | Descripción |
|--------|-------|-------------|
| `filter` | `filter(Predicate<T>)` | Conserva solo los elementos que cumplen la condición |
| `map` | `map(Function<T,R>)` | Transforma cada elemento de tipo `T` a tipo `R` |
| `flatMap` | `flatMap(Function<T, Stream<R>>)` | Como `map`, pero aplana streams anidados |
| `sorted` | `sorted()` / `sorted(Comparator<T>)` | Ordena los elementos |
| `distinct` | `distinct()` | Elimina duplicados (usa `equals`) |
| `limit` | `limit(long n)` | Toma como máximo los primeros `n` elementos |
| `skip` | `skip(long n)` | Descarta los primeros `n` elementos |
| `peek` | `peek(Consumer<T>)` | Efecto lateral (útil para depurar) sin modificar el stream |

```java title="Ejemplos de operaciones intermedias"
// filter: solo líneas que contienen "ERROR"
stream.filter(linea -> linea.contains("[ERROR]"))

// map: transformar String → Integer (longitud de cada línea)
stream.map(String::length)

// map: transformar String → objeto del dominio
stream.map(linea -> new Alumno(linea.split(",")))

// sorted: ordenar líneas alfabéticamente
stream.sorted()

// sorted con Comparator: ordenar alumnos por nota descendente
stream.sorted(Comparator.comparingDouble(Alumno::notaFinal).reversed())

// distinct: eliminar líneas duplicadas
stream.distinct()

// limit + skip: paginación (página 2, 10 elementos por página)
stream.skip(10).limit(10)

// peek: depuración (imprime cada elemento sin modificarlo)
stream.peek(System.out::println)
```

---

### 5.2 Operaciones Terminales

Las operaciones terminales **consumen el Stream** y producen un resultado (o efecto). Después de llamarlas, el stream queda cerrado.

| Método | Descripción |
|--------|-------------|
| `forEach(Consumer)` | Ejecuta una acción para cada elemento |
| `collect(Collector)` | Recoge los elementos en una colección u objeto |
| `count()` | Cuenta el número de elementos |
| `findFirst()` | Devuelve el primer elemento como `Optional<T>` |
| `anyMatch(Predicate)` | `true` si algún elemento cumple la condición |
| `allMatch(Predicate)` | `true` si todos los elementos cumplen la condición |
| `noneMatch(Predicate)` | `true` si ningún elemento cumple la condición |
| `min(Comparator)` | Devuelve el elemento mínimo como `Optional<T>` |
| `max(Comparator)` | Devuelve el elemento máximo como `Optional<T>` |
| `reduce(identity, BinaryOperator)` | Combina todos los elementos en uno solo |
| `toList()` | Atajo de `collect(Collectors.toList())` (Java 16+) |

```java title="Ejemplos de operaciones terminales"
// forEach: imprimir cada línea
stream.forEach(System.out::println);

// count: número de líneas de error
long errores = stream.filter(l -> l.contains("[ERROR]")).count();

// findFirst: primera línea que contiene "WARN"
Optional<String> primerWarning = stream.filter(l -> l.contains("[WARN]")).findFirst();
primerWarning.ifPresent(System.out::println);

// anyMatch: ¿hay algún error crítico?
boolean hayCritico = stream.anyMatch(l -> l.contains("[CRITICAL]"));

// min/max: alumno con menor/mayor nota
Optional<Alumno> mejorAlumno = stream.max(Comparator.comparingDouble(Alumno::notaFinal));
```

---

### 5.3 Collectors: Recoger Resultados

`Collectors` es una clase de utilidad que proporciona los recolectores más comunes para usar con `collect()`.

| Collector | Resultado | Descripción |
|-----------|-----------|-------------|
| `Collectors.toList()` | `List<T>` | Lista mutable con todos los elementos |
| `Collectors.toUnmodifiableList()` | `List<T>` | Lista inmutable |
| `Collectors.joining(delim)` | `String` | Une todos los `String` con el delimitador |
| `Collectors.joining(delim, prefix, suffix)` | `String` | Une con delimitador, prefijo y sufijo |
| `Collectors.groupingBy(classifier)` | `Map<K, List<T>>` | Agrupa elementos por clave |
| `Collectors.counting()` | `Long` | Cuenta elementos (útil con `groupingBy`) |
| `Collectors.summingDouble(mapper)` | `Double` | Suma los valores double de cada elemento |
| `Collectors.averagingDouble(mapper)` | `Double` | Calcula la media de los valores double |
| `Collectors.toMap(keyMapper, valueMapper)` | `Map<K,V>` | Crea un mapa desde los elementos |

!!! example "Collectors en acción: estadísticas de un CSV de notas"

    === "Imperativa"

        Calculamos estadísticas de notas con bucles y contadores manuales.

        ```java title="EstadisticasNotas.java"
        import java.io.IOException;
        import java.nio.charset.StandardCharsets;
        import java.nio.file.*;
        import java.util.*;

        public class EstadisticasNotas {

            record Alumno(String nombre, double nota, String modulo) {}

            public static void main(String[] args) throws IOException {
                Path ruta = Paths.get("datos", "notas_alumnos.csv");
                List<String> lineas = Files.readAllLines(ruta, StandardCharsets.UTF_8);

                List<Alumno> alumnos = new ArrayList<>();
                boolean cabecera = true;

                for (String linea : lineas) {
                    if (linea.isBlank() || linea.startsWith("#")) continue;
                    if (cabecera) { cabecera = false; continue; }
                    String[] c = linea.split(",");
                    if (c.length >= 3) {
                        alumnos.add(new Alumno(c[0].trim(),
                            Double.parseDouble(c[1].trim()), c[3].trim()));
                    }
                }

                // Media de notas
                double suma = 0;
                for (Alumno a : alumnos) suma += a.nota();
                double media = suma / alumnos.size();

                // Agrupar por módulo (manual)
                Map<String, List<Alumno>> porModulo = new HashMap<>();
                for (Alumno a : alumnos) {
                    porModulo.computeIfAbsent(a.modulo(), k -> new ArrayList<>()).add(a);
                }

                // Contar aprobados
                int aprobados = 0;
                for (Alumno a : alumnos) if (a.nota() >= 5.0) aprobados++;

                System.out.printf("Total alumnos : %d%n", alumnos.size());
                System.out.printf("Media notas   : %.2f%n", media);
                System.out.printf("Aprobados     : %d%n", aprobados);
                System.out.printf("Suspensos     : %d%n", alumnos.size() - aprobados);
                System.out.println("Módulos       : " + porModulo.keySet());
            }
        }
        ```

    === "Declarativa (Stream)"

        Los Collectors permiten calcular estadísticas complejas en pocas líneas.

        ```java title="EstadisticasNotasStream.java"
        import java.io.IOException;
        import java.nio.charset.StandardCharsets;
        import java.nio.file.*;
        import java.util.*;
        import java.util.concurrent.atomic.AtomicBoolean;
        import java.util.stream.Collectors;

        public class EstadisticasNotasStream {

            record Alumno(String nombre, double nota, String modulo) {}

            public static void main(String[] args) throws IOException {
                Path ruta = Paths.get("datos", "notas_alumnos.csv");
                AtomicBoolean cabecera = new AtomicBoolean(true);

                List<Alumno> alumnos;
                try (var lineas = Files.lines(ruta, StandardCharsets.UTF_8)) {
                    alumnos = lineas
                        .filter(l -> !l.isBlank() && !l.startsWith("#"))
                        .filter(l -> { // saltar cabecera
                            if (cabecera.get()) { cabecera.set(false); return false; }
                            return true;
                        })
                        .filter(l -> l.split(",").length >= 4)
                        .map(l -> {
                            String[] c = l.split(",");
                            return new Alumno(c[0].trim(),
                                Double.parseDouble(c[1].trim()), c[3].trim());
                        })
                        .collect(Collectors.toList());
                }

                // Media con averagingDouble
                double media = alumnos.stream()
                    .collect(Collectors.averagingDouble(Alumno::nota));

                // Aprobados / suspensos con partitioningBy
                Map<Boolean, Long> particion = alumnos.stream()
                    .collect(Collectors.partitioningBy(
                        a -> a.nota() >= 5.0, Collectors.counting()));

                // Agrupación por módulo con groupingBy
                Map<String, List<Alumno>> porModulo = alumnos.stream()
                    .collect(Collectors.groupingBy(Alumno::modulo));

                // Nombre del mejor alumno con max()
                String mejor = alumnos.stream()
                    .max(Comparator.comparingDouble(Alumno::nota))
                    .map(Alumno::nombre)
                    .orElse("N/A");

                System.out.printf("Total alumnos : %d%n", alumnos.size());
                System.out.printf("Media notas   : %.2f%n", media);
                System.out.printf("Aprobados     : %d%n", particion.get(true));
                System.out.printf("Suspensos     : %d%n", particion.get(false));
                System.out.printf("Mejor alumno  : %s%n", mejor);
                System.out.println("Módulos       : " + porModulo.keySet());
            }
        }
        ```

---

### 5.4 `Files.walk()` — Recorrido Recursivo de Directorios

`Files.walk(Path)` devuelve un `Stream<Path>` que recorre recursivamente un directorio y todos sus subdirectorios. Es la alternativa funcional y moderna a la recursión manual.

| Método | Descripción |
|--------|-------------|
| `Files.walk(path)` | Recorre recursivamente (profundidad ilimitada) |
| `Files.walk(path, maxDepth)` | Recorre hasta `maxDepth` niveles de profundidad |
| `Files.list(path)` | Solo el nivel inmediato del directorio (sin recursión) |
| `Files.find(path, depth, BiPredicate)` | Recorre aplicando un filtro de atributos |

!!! example "Buscar todos los ficheros .java de un proyecto y calcular su tamaño total"

    === "Imperativa"

        Usamos una cola y un bucle para implementar el recorrido BFS del árbol de directorios.

        ```java title="BuscadorFicherosJava.java"
        import java.io.IOException;
        import java.nio.file.*;
        import java.util.*;

        public class BuscadorFicherosJava {

            public static void main(String[] args) throws IOException {
                Path raiz       = Paths.get("src");
                long totalBytes = 0;
                int  contador   = 0;

                // Usamos una cola para BFS (recorrido en anchura del árbol de directorios)
                Deque<Path> cola = new ArrayDeque<>();
                cola.push(raiz);

                while (!cola.isEmpty()) {
                    Path actual = cola.pop();

                    if (Files.isDirectory(actual)) {
                        try (DirectoryStream<Path> hijos = Files.newDirectoryStream(actual)) {
                            for (Path hijo : hijos) {
                                cola.push(hijo);
                            }
                        }
                    } else if (actual.toString().endsWith(".java")) {
                        long tam = Files.size(actual);
                        System.out.printf("  %s  (%d bytes)%n", actual, tam);
                        totalBytes += tam;
                        contador++;
                    }
                }

                System.out.printf("%n%d fichero(s) .java — Total: %d KB%n",
                    contador, totalBytes / 1024);
            }
        }
        ```

    === "Declarativa (Stream)"

        `Files.walk()` hace el recorrido recursivo internamente. Solo nos ocupamos del pipeline de filtrado.

        ```java title="BuscadorFicherosJavaStream.java"
        import java.io.IOException;
        import java.nio.file.*;
        import java.util.stream.Collectors;

        public class BuscadorFicherosJavaStream {

            public static void main(String[] args) throws IOException {
                Path raiz = Paths.get("src");

                try (var ficheros = Files.walk(raiz)) {
                    var javaFiles = ficheros
                        .filter(Files::isRegularFile)                       // solo ficheros
                        .filter(p -> p.toString().endsWith(".java"))         // solo .java
                        .collect(Collectors.toList());

                    // Mostrar cada fichero con su tamaño
                    javaFiles.forEach(p -> {
                        try {
                            System.out.printf("  %s  (%d bytes)%n", p, Files.size(p));
                        } catch (IOException e) {
                            System.err.println("No se pudo leer: " + p);
                        }
                    });

                    // Tamaño total con reduce (o con mapToLong + sum)
                    long totalBytes = javaFiles.stream()
                        .mapToLong(p -> {
                            try { return Files.size(p); }
                            catch (IOException e) { return 0L; }
                        })
                        .sum();

                    System.out.printf("%n%d fichero(s) .java — Total: %d KB%n",
                        javaFiles.size(), totalBytes / 1024);
                }
            }
        }
        ```

    **Salida de ejemplo:**
    ```
      src/main/java/App.java  (1245 bytes)
      src/main/java/modelo/Alumno.java  (892 bytes)
      src/main/java/modelo/Producto.java  (1103 bytes)
      src/test/java/AppTest.java  (548 bytes)

    4 fichero(s) .java — Total: 3 KB
    ```

---

## 6. Resumen: ¿Qué API usar en cada caso?

| Necesidad | Clase/Método recomendado |
|-----------|--------------------------|
| Crear directorios | `Files.createDirectories(path)` |
| Crear fichero vacío | `Files.createFile(path)` |
| Borrar fichero | `Files.deleteIfExists(path)` |
| Copiar fichero | `Files.copy(origen, destino, opciones)` |
| Mover/renombrar | `Files.move(origen, destino, opciones)` |
| Comprobar existencia | `Files.exists(path)` |
| Leer atributos | `Files.readAttributes(path, BasicFileAttributes.class)` |
| Listar directorio (un nivel) | `Files.newDirectoryStream(path, filtro)` |
| Recorrer directorio recursivo | `Files.walk(path)` |
| Leer texto (fichero pequeño) | `Files.readAllLines(path, UTF_8)` |
| Leer texto (fichero grande / lazy) | `Files.lines(path, UTF_8)` + `try-with-resources` |
| Escribir texto (sobreescribir) | `Files.write(path, lineas, UTF_8, CREATE, TRUNCATE_EXISTING)` |
| Escribir texto (añadir al final) | `Files.write(path, lineas, UTF_8, CREATE, APPEND)` |
| Leer y cerrar recurso con seguridad | `try-with-resources` con `BufferedReader` |
| Parsear CSV a objetos | `Files.lines()` + `map()` + `collect()` |
| Filtrar y ordenar datos de fichero | `filter()` + `sorted()` + `collect()` |
| Agrupar datos por clave | `Collectors.groupingBy(classifier)` |
| Calcular estadísticas | `Collectors.averagingDouble()` / `summingDouble()` |
| Serializar objetos | `ObjectOutputStream` + `Files.newOutputStream()` |
| Deserializar objetos | `ObjectInputStream` + `Files.newInputStream()` |
