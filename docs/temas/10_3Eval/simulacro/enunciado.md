# Simulacro 3ª Evaluación — Liga de Baloncesto

**Módulo:** Programación (PROG) | **Duración:** 3 horas<br>
**Calificación:** RA5 Ficheros: 10 puntos · RA6 Colecciones: 10 puntos<br>
**Material permitido:** IntelliJ IDEA, ejercicios propios ya resueltos<br>

---

## Proyecto base

Para realizar el simulacro descarga el proyecto base desde el siguiente enlace: [Proyecto Base](proyecto_base.zip)



El proyecto base se encuentra en la carpeta `proyecto_base/`.

## Contexto

Una empresa de estadísticas deportivas almacena los resultados de la liga de baloncesto en un fichero cifrado con el **algoritmo César**. Tu tarea es construir la aplicación que descifre el fichero, calcule la clasificación y exporte los resultados.

Se proporciona el proyecto base en `proyecto_base/`. Debes completar únicamente los métodos marcados con `// TODO`. **No modifiques** las clases dadas ni el `App.java`.

**Flujo de datos:**

```
data/partidos_cifrado.txt
        │
       [F1] CifradoCesar.descifrar()   (llamado desde F2)
        │
   CSV en claro
        │
       [F2] FicheroLiga.cargarPartidos()
        │
   List<Partido>
        │
       [C1] calcularClasificacion()
       [C2] ordenarClasificacion()     ──► pantalla
       [C3] filtrarInvictos()
       [C4] obtenerMaximoAnotador()
       [C5] agruparPorVictorias()
        │
       [F3] guardarClasificacion()
        │
   data/clasificacion.txt
```

---

## Cifrado César — Referencia

El fichero `data/partidos_cifrado.txt` contiene el CSV con las **letras desplazadas 2 posiciones** hacia adelante en el alfabeto. Los números y separadores (`;`, saltos de línea) **no están cifrados**.

**Regla de cifrado:** cada letra se desplaza +2 posiciones (circular).
**Regla de descifrado:** cada letra se desplaza -2 posiciones (circular).

```
Cifrado (desplazamiento +2):   A→C  B→D  ...  X→Z  Y→A  Z→B
Descifrado (desplazamiento -2): C→A  D→B  ...  Z→X  A→Y  B→Z
```

**Ejemplo:**

| CSV cifrado | CSV descifrado |
|---|---|
| `1;NCMGTU;DWNNU;110;95` | `1;LAKERS;BULLS;110;95` |
| `1;EGNVKEU;YCTTKTQU;105;98` | `1;CELTICS;WARRIORS;105;98` |

**Verificación manual:**
```
N(14) -2 = L(12)  C(3) -2 = A(1)  M(13) -2 = K(11)  G(7) -2 = E(5)  T(20) -2 = R(18)  U(21) -2 = S(19)
→ NCMGTU = LAKERS ✓
```

**Formato CSV descifrado:**
```
jornada;equipoLocal;equipoVisitante;puntosLocal;puntosVisitante
```

---

## Estructura del proyecto base

```
proyecto_base/
├── pom.xml
├── data/
│   └── partidos_cifrado.txt        ← datos de entrada (no modificar)
└── src/main/java/com/pg/ligabasket/
    ├── App.java                    ← main (no modificar)
    ├── Equipo.java                 ← clase dada (no modificar)
    ├── Partido.java                ← clase dada (no modificar)
    ├── FilaClasificacion.java      ← clase dada (no modificar)
    ├── FicheroLiga.java            ← ★ IMPLEMENTAR: F2, F3
    ├── LigaService.java            ← ★ IMPLEMENTAR: C1, C2, C3, C4, C5
    └── cifrado/
        └── CifradoCesar.java       ← ★ IMPLEMENTAR: F1
```

---

## Ejercicios RA5 — Ficheros (10 puntos)

### F1 — `CifradoCesar.descifrar()` (3 puntos)

Implementa el método `descifrar(String textoCifrado, int desplazamiento)` en `cifrado/CifradoCesar.java`.

Solo se descifran las letras (mayúsculas y minúsculas). Los números y símbolos se copian tal cual.
El descifrado es circular: si al restar el desplazamiento el resultado se sale del rango del alfabeto, se da la vuelta.

**Ejemplo de descifrado con desplazamiento 2:**

```
Entrada:  1;NCMGTU;DWNNU;110;95
Salida:   1;LAKERS;BULLS;110;95
```

---

### F2 — `FicheroLiga.cargarPartidos()` (5 puntos)

Implementa `cargarPartidos()` en `FicheroLiga.java`.

Debe leer `data/partidos_cifrado.txt`, llamar a `cifrador.descifrar()` con `DESPLAZAMIENTO=2` y construir la `List<Partido>`.

**Salida esperada por consola:**

```
--- Partidos cargados ---
Total partidos: 6
[J1] LAKERS 110 - 95 BULLS
[J1] CELTICS 105 - 98 WARRIORS
[J2] LAKERS 88 - 102 CELTICS
[J2] BULLS 94 - 87 WARRIORS
[J3] LAKERS 115 - 107 WARRIORS
[J3] BULLS 90 - 95 CELTICS
```

---

### F3 — `FicheroLiga.guardarClasificacion()` (2 puntos)

Implementa `guardarClasificacion(List<FilaClasificacion> clasificacion)`.

Guarda la clasificación en `data/clasificacion.txt`.
Una línea por equipo. Usa `FilaClasificacion.toString()` que ya tiene el formato correcto.

**Contenido esperado de `data/clasificacion.txt`:**

```
CELTICS;6;302;276;3;0
LAKERS;4;313;304;2;1
BULLS;2;279;292;1;2
WARRIORS;0;292;314;0;3
```

*(formato: nombre;puntos;puntosAFavor;puntosEnContra;victorias;derrotas)*

---

## Ejercicios RA6 — Colecciones y API funcional (10 puntos)

### C1 — `calcularClasificacion()` (2 puntos) — Implementación libre

A partir de `List<Partido>`, devuelve una `List<FilaClasificacion>` con una entrada por equipo.

Para cada partido llama a `FilaClasificacion.actualizarStats(misPuntos, susPuntos)` tanto para el equipo local como para el visitante.

En baloncesto no hay empates: el equipo que anota más puntos gana (gana 2 puntos en la clasificación).

Puede implementarse de forma imperativa o con Streams.

---

### C2 — `ordenarClasificacion()` (3 puntos) — **Streams obligatorio**

Ordena la clasificación con este criterio **usando Streams y Comparator**:

1. Puntos de clasificación **descendente** (victorias × 2)
2. Diferencia de puntos anotados **descendente**
3. Nombre del equipo **ascendente** (alfabético)

**Salida esperada por consola:**

```
--- Clasificación ---
Pos Equipo             Pts     PAF     PAC   Dif
---------------------------------------------
1   CELTICS              6     302     276   +26
2   LAKERS               4     313     304    +9
3   BULLS                2     279     292   -13
4   WARRIORS             0     292     314   -22
```

---

### C3 — `filtrarInvictos()` (2 puntos) — **Streams obligatorio**

Devuelve los equipos con `derrotas == 0` usando **Streams**.

**Salida esperada por consola:**

```
--- Equipos invictos ---
- CELTICS
```

---

### C4 — `obtenerMaximoAnotador()` (1.5 puntos) — **Stream.max() obligatorio**

Devuelve el equipo con más `puntosAFavor` usando `Stream.max()`.

**Salida esperada por consola:**

```
--- Máximo anotador ---
Equipo: LAKERS
```

---

### C5 — `agruparPorVictorias()` (1.5 puntos) — **Collectors.groupingBy() obligatorio**

Agrupa los nombres de equipo según su número de victorias usando `Collectors.groupingBy()`.

**Salida esperada por consola:**

```
--- Equipos por victorias ---
0 victoria(s): [WARRIORS]
1 victoria(s): [BULLS]
2 victoria(s): [LAKERS]
3 victoria(s): [CELTICS]
```

---

## Tabla de calificación

| Apartado | Clase | RA | Puntos | Restricción |
|----------|-------|----|--------|-------------|
| F1 `descifrar()` | `CifradoCesar` | RA5 | 3 | — |
| F2 `cargarPartidos()` | `FicheroLiga` | RA5 | 5 | Requiere F1 |
| F3 `guardarClasificacion()` | `FicheroLiga` | RA5 | 2 | — |
| C1 `calcularClasificacion()` | `LigaService` | RA6 | 2 | Libre |
| C2 `ordenarClasificacion()` | `LigaService` | RA6 | 3 | **Streams obligatorio** |
| C3 `filtrarInvictos()` | `LigaService` | RA6 | 2 | **Streams obligatorio** |
| C4 `obtenerMaximoAnotador()` | `LigaService` | RA6 | 1.5 | **Stream.max() obligatorio** |
| C5 `agruparPorVictorias()` | `LigaService` | RA6 | 1.5 | **groupingBy() obligatorio** |

> Cada RA se evalúa de forma independiente. Es necesario obtener al menos **5/10 en cada RA** para aprobar.
> El uso de API imperativa en apartados marcados como "Streams obligatorio" supone **0 puntos** en ese apartado.
