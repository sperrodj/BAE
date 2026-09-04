# Tema 4. Normalización

> **Módulo:** Bases de Datos · **Curso:** 1.º DAM  
> Cómo detectar redundancias perjudiciales y descomponer relaciones sin perder información.

## Objetivos de aprendizaje

Al finalizar este tema serás capaz de:

- identificar redundancias y anomalías de inserción, actualización y borrado;
- formular e interpretar dependencias funcionales;
- calcular cierres de atributos;
- obtener superclaves y claves candidatas a partir de dependencias;
- distinguir atributos primos y no primos;
- comprobar la primera, segunda y tercera forma normal;
- comprobar la forma normal de Boyce-Codd;
- descomponer relaciones justificando cada paso;
- verificar si una descomposición tiene unión sin pérdida;
- analizar si conserva las dependencias funcionales;
- reconocer dependencias multivaluadas y la cuarta forma normal;
- explicar el propósito de la quinta forma normal;
- valorar cuándo una desnormalización está justificada.

---

## 1. Qué es normalizar

La **normalización** es un proceso formal de diseño del modelo relacional. Analiza las dependencias entre atributos para organizar los datos en relaciones que reduzcan redundancias perjudiciales y anomalías.

Normalizar no significa:

- dividir una tabla hasta que quede pequeña;
- crear una tabla por cada columna;
- eliminar toda repetición posible;
- mejorar automáticamente el rendimiento;
- aplicar recetas sin conocer las reglas de negocio.

La normalización persigue principalmente:

- representar cada hecho en un lugar apropiado;
- evitar inconsistencias al modificar datos repetidos;
- impedir que el alta de un hecho dependa de otro hecho innecesario;
- evitar la pérdida accidental de información al borrar filas;
- expresar claves y restricciones con claridad;
- facilitar el mantenimiento lógico del esquema.

> [!IMPORTANT]
> Las formas normales dependen de las **reglas del dominio**, no solo de los datos que aparecen hoy en una tabla. Una dependencia funcional debe cumplirse en todos los estados válidos posibles.

### 1.1. Normalización y rendimiento

Un esquema normalizado puede necesitar más combinaciones entre relaciones. En algunos sistemas analíticos se acepta redundancia controlada para acelerar lecturas; esto se denomina **desnormalización**.

Primero se diseña correctamente. Después se mide. Solo se desnormaliza si existe una necesidad demostrada y se establecen mecanismos para mantener la consistencia.

---

## 2. Redundancia y anomalías

Consideremos esta relación:

```text
MATRICULA_INICIAL(
  id_alumno,
  nombre_alumno,
  id_edicion,
  nombre_curso,
  id_docente,
  nombre_docente,
  nota
)
```

| id_alumno | nombre_alumno | id_edicion | nombre_curso | id_docente | nombre_docente | nota |
|---:|---|---:|---|---:|---|---:|
| 101 | Ana Torres | 20 | Bases de Datos | 7 | Sara Pérez | 8.5 |
| 102 | Luis Díaz | 20 | Bases de Datos | 7 | Sara Pérez | 7.0 |
| 101 | Ana Torres | 21 | Programación | 9 | Iván Martín | 9.0 |

Se repiten nombres de alumnado, cursos y docentes.

### 2.1. Anomalía de actualización

Si cambia el nombre del docente 7, debemos modificar varias filas. Si olvidamos alguna, la base queda inconsistente.

### 2.2. Anomalía de inserción

Quizá no podamos registrar una edición nueva hasta que exista alguna matrícula, porque todos los datos comparten la misma relación.

### 2.3. Anomalía de borrado

Si eliminamos la última matrícula de una edición, podríamos perder también la única información sobre esa edición y su curso.

### 2.4. El problema no es repetir cualquier valor

Una FK repetida, como `id_edicion = 20`, no es necesariamente redundancia perjudicial. Expresa que varias matrículas pertenecen a la misma edición. El problema aparece cuando repetimos hechos descriptivos que dependen de otra entidad y pueden quedar descoordinados.

---

## 3. Dependencias funcionales

Sea una relación $R$. Una **dependencia funcional** se escribe:

$$
X \rightarrow Y
$$

y se lee «$X$ determina funcionalmente a $Y$».

Significa que, en todo estado válido de $R$, dos tuplas que coincidan en los atributos de $X$ deben coincidir también en los atributos de $Y$.

- $X$ es el **determinante**.
- $Y$ es el **dependiente**.

Ejemplo:

$$
id\_alumno \rightarrow nombre\_alumno, correo
$$

Conocer `id_alumno` determina un único nombre y correo dentro del dominio modelado.

### 3.1. Las dependencias proceden de reglas

Supongamos estos datos:

| codigo_postal | municipio |
|---|---|
| 35500 | Arrecife |
| 35510 | Tías |

No basta para afirmar que `municipio -> codigo_postal`: podrían existir varios códigos postales en el mismo municipio. Debemos consultar la regla del dominio.

> [!WARNING]
> Una coincidencia en una muestra de datos no demuestra una dependencia funcional. Un contraejemplo sí puede demostrar que una dependencia propuesta es falsa.

### 3.2. Dependencia trivial

$X \rightarrow Y$ es **trivial** si $Y \subseteq X$.

Ejemplo:

$$
\{id\_alumno, id\_edicion\} \rightarrow id\_alumno
$$

Se cumple por definición y no revela una regla nueva.

### 3.3. Dependencia no trivial

Es no trivial si $Y \nsubseteq X$.

$$
id\_alumno \rightarrow nombre
$$

### 3.4. Dependencia completa

$Y$ depende **completamente** de un conjunto $X$ si depende de todo $X$ y no de ningún subconjunto propio de $X$.

Ejemplo:

$$
\{id\_alumno, id\_edicion\} \rightarrow nota
$$

si ni `id_alumno` ni `id_edicion` por separado determinan la nota.

### 3.5. Dependencia parcial

Existe cuando un atributo depende de una parte propia de una clave compuesta.

$$
\{id\_alumno, id\_edicion\} \rightarrow nombre\_alumno
$$

pero también:

$$
id\_alumno \rightarrow nombre\_alumno
$$

### 3.6. Dependencia transitiva

Existe un camino de dependencias:

$$
X \rightarrow Y \quad\text{e}\quad Y \rightarrow Z
$$

por lo que $X \rightarrow Z$ se deduce por transitividad.

Por ejemplo:

$$
id\_edicion \rightarrow id\_curso
$$

$$
id\_curso \rightarrow nombre\_curso
$$

Entonces:

$$
id\_edicion \rightarrow nombre\_curso
$$

El problema para 3FN aparece bajo condiciones concretas que veremos después; no toda cadena transitiva obliga por sí sola a descomponer.

---

## 4. Reglas de inferencia

Las dependencias dadas permiten deducir otras. Las reglas básicas se conocen como **axiomas de Armstrong**.

### 4.1. Reflexividad

Si $Y \subseteq X$, entonces:

$$
X \rightarrow Y
$$

### 4.2. Aumentación

Si $X \rightarrow Y$, entonces:

$$
XZ \rightarrow YZ
$$

### 4.3. Transitividad

Si $X \rightarrow Y$ e $Y \rightarrow Z$, entonces:

$$
X \rightarrow Z
$$

De estas reglas se derivan otras útiles:

- **unión:** si $X \rightarrow Y$ y $X \rightarrow Z$, entonces $X \rightarrow YZ$;
- **descomposición:** si $X \rightarrow YZ$, entonces $X \rightarrow Y$ y $X \rightarrow Z$;
- **pseudotransitividad:** si $X \rightarrow Y$ y $WY \rightarrow Z$, entonces $WX \rightarrow Z$.

Estas reglas permiten justificar resultados sin depender de los datos de ejemplo.

---

## 5. Cierre de un conjunto de atributos

El **cierre de $X$ respecto de $F$**, escrito $X^+$, es el conjunto de atributos que pueden determinarse a partir de $X$ usando las dependencias de $F$.

Sirve para:

- comprobar si $X$ es superclave;
- verificar si una dependencia se deduce de $F$;
- localizar claves candidatas;
- analizar una descomposición.

### 5.1. Algoritmo

```text
entrada: conjunto X y dependencias F
resultado := X

repetir
  para cada dependencia A -> B de F
    si A está contenido en resultado
      añadir B a resultado
hasta que resultado no cambie

devolver resultado
```

### 5.2. Ejemplo

Sea:

```text
R(A, B, C, D, E)
F = {
  A -> B,
  B -> C,
  AC -> D,
  D -> E
}
```

Calculamos $A^+$:

1. Comenzamos con `{A}`.
2. Por `A -> B`, añadimos `B`.
3. Por `B -> C`, añadimos `C`.
4. Ya tenemos `A` y `C`; por `AC -> D`, añadimos `D`.
5. Por `D -> E`, añadimos `E`.

$$
A^+ = \{A,B,C,D,E\}
$$

Por tanto, `A` es superclave. Como ningún subconjunto propio de `{A}` puede serlo, también es clave candidata.

### 5.3. Comprobar una dependencia

$X \rightarrow Y$ se deduce de $F$ si $Y \subseteq X^+$.

---

## 6. Claves candidatas y atributos primos

Una **clave candidata** es una superclave mínima.

Para localizar candidatas:

1. identifica atributos que nunca aparecen a la derecha de una DF no trivial; suelen tener que formar parte de toda clave;
2. calcula su cierre;
3. añade atributos hasta obtener todos los de la relación;
4. elimina atributos sobrantes;
5. busca alternativas sistemáticamente.

### 6.1. Atributos primos

- **Atributo primo:** pertenece a alguna clave candidata.
- **Atributo no primo:** no pertenece a ninguna clave candidata.

«Primo» no significa «pertenece a la clave primaria». Debemos considerar todas las claves candidatas.

### 6.2. Ejemplo con dos claves candidatas

```text
R(ALUMNO, ASIGNATURA, PROFESOR)
F = {
  PROFESOR -> ASIGNATURA,
  ALUMNO ASIGNATURA -> PROFESOR
}
```

- `{ALUMNO, ASIGNATURA}` determina todo.
- `{ALUMNO, PROFESOR}` también determina todo porque `PROFESOR -> ASIGNATURA`.

Ambas son claves candidatas. Por ello los tres atributos son primos.

---

## 7. Cobertura mínima

Una **cobertura mínima** o canónica es un conjunto equivalente de dependencias sin elementos redundantes. Resulta útil para sintetizar relaciones y comprobar restricciones.

Debe cumplir:

1. cada DF tiene un único atributo a la derecha;
2. ningún atributo de la izquierda es extráneo;
3. ninguna DF completa es redundante.

### 7.1. Procedimiento

1. Descomponer la parte derecha: `A -> BC` pasa a `A -> B` y `A -> C`.
2. Eliminar atributos extráneos de determinantes compuestos usando cierres.
3. Eliminar dependencias que se deduzcan de las demás.

> [!NOTE]
> Puede haber coberturas mínimas diferentes pero equivalentes.

---

## 8. Primera forma normal — 1FN

Una relación está en **primera forma normal** cuando cada atributo toma un único valor de su dominio por tupla y no contiene grupos repetitivos dentro de una celda.

Tabla problemática:

| id_alumno | nombre | telefonos |
|---:|---|---|
| 101 | Ana | 600111222, 922333444 |

Guardar una lista dificulta validar, buscar, referenciar o describir cada teléfono.

Descomposición:

```text
ALUMNO(id_alumno PK, nombre)
ALUMNO_TELEFONO(id_alumno PK FK -> ALUMNO, telefono PK, tipo)
```

### 8.1. Atomicidad dependiente del dominio

«Atómico» no significa físicamente indivisible. Una fecha contiene año, mes y día; puede seguir siendo un valor único del dominio `FECHA`. Dividimos un valor cuando sus componentes tienen significado y operaciones independientes para el sistema.

### 8.2. Columnas repetidas

Esto tampoco es una solución estable:

```text
telefono_1, telefono_2, telefono_3
```

Introduce un límite artificial, columnas vacías y consultas repetitivas. Si la cantidad es variable, debe modelarse como otra relación.

### 8.3. Requisitos prácticos de 1FN

- un valor por atributo y tupla;
- ausencia de listas o grupos repetitivos;
- dominio coherente por atributo;
- posibilidad de distinguir las tuplas mediante alguna clave candidata.

---

## 9. Segunda forma normal — 2FN

Una relación está en **2FN** si:

1. está en 1FN;
2. cada atributo no primo depende funcionalmente de forma completa de **todas** las claves candidatas.

La 2FN elimina dependencias parciales de atributos no primos respecto de claves candidatas compuestas.

### 9.1. Cuándo puede existir el problema

Si todas las claves candidatas son simples, la relación cumple automáticamente 2FN. No existe una parte propia no vacía de una clave de un solo atributo.

### 9.2. Ejemplo

```text
LINEA_PEDIDO(
  id_pedido,
  id_producto,
  fecha_pedido,
  nombre_producto,
  cantidad,
  precio_pactado
)

PK(id_pedido, id_producto)
```

Dependencias:

$$
id\_pedido \rightarrow fecha\_pedido
$$

$$
id\_producto \rightarrow nombre\_producto
$$

$$
\{id\_pedido,id\_producto\} \rightarrow cantidad,precio\_pactado
$$

`fecha_pedido` y `nombre_producto` dependen solo de partes de la clave compuesta. No cumple 2FN.

### 9.3. Descomposición

```text
PEDIDO(id_pedido PK, fecha_pedido)

PRODUCTO(id_producto PK, nombre_producto)

LINEA_PEDIDO(
  id_pedido PK FK -> PEDIDO,
  id_producto PK FK -> PRODUCTO,
  cantidad,
  precio_pactado
)
```

`precio_pactado` permanece en `LINEA_PEDIDO`: puede depender de la combinación concreta de pedido y producto, aunque exista un precio actual en `PRODUCTO`.

> [!IMPORTANT]
> La definición rigurosa usa todas las claves candidatas, no únicamente la PK elegida.

---

## 10. Tercera forma normal — 3FN

Una relación está en **3FN** si, para toda DF no trivial $X \rightarrow A$, se cumple al menos una condición:

1. $X$ es una superclave, o
2. $A$ es un atributo primo.

Esta definición contempla todas las claves candidatas y casos que una explicación informal de «dependencias transitivas» puede pasar por alto.

### 10.1. Interpretación práctica

En muchos esquemas sencillos, 3FN evita que un atributo no primo dependa de otro atributo no primo en lugar de depender directamente de una clave.

### 10.2. Ejemplo

```text
EDICION(
  id_edicion PK,
  id_curso,
  nombre_curso,
  fecha_inicio
)
```

Dependencias:

$$
id\_edicion \rightarrow id\_curso, fecha\_inicio
$$

$$
id\_curso \rightarrow nombre\_curso
$$

`id_curso` no es superclave de `EDICION` y `nombre_curso` no es primo. La segunda DF incumple 3FN.

### 10.3. Descomposición

```text
CURSO(id_curso PK, nombre_curso)

EDICION(
  id_edicion PK,
  id_curso FK -> CURSO,
  fecha_inicio
)
```

Ahora el nombre del curso se almacena una vez y cada edición lo referencia.

### 10.4. Eslogan con límites

Como ayuda mnemotécnica:

> Cada atributo no clave debe depender de la clave, de toda la clave y de nada más que la clave.

- «de la clave» recuerda la dependencia;
- «de toda la clave» recuerda 2FN;
- «de nada más» recuerda 3FN.

Es útil, pero no sustituye las definiciones formales cuando existen varias claves candidatas o atributos primos.

---

## 11. Forma normal de Boyce-Codd — BCNF

Una relación está en **BCNF** si, para toda DF no trivial:

$$
X \rightarrow Y
$$

$X$ es una superclave.

BCNF es más estricta que 3FN. Toda relación en BCNF está en 3FN, pero una relación en 3FN puede no estar en BCNF.

### 11.1. Ejemplo de 3FN que no cumple BCNF

```text
ASIGNACION(ALUMNO, ASIGNATURA, PROFESOR)
```

Reglas:

- cada profesor imparte una única asignatura;
- para cada alumno y asignatura se asigna un profesor.

Dependencias:

$$
PROFESOR \rightarrow ASIGNATURA
$$

$$
\{ALUMNO,ASIGNATURA\} \rightarrow PROFESOR
$$

Claves candidatas:

- `{ALUMNO, ASIGNATURA}`;
- `{ALUMNO, PROFESOR}`.

La relación está en 3FN porque `ASIGNATURA`, a la derecha de la DF problemática, es atributo primo. No está en BCNF porque `PROFESOR` no es superclave.

### 11.2. Descomposición a BCNF

```text
PROFESOR_ASIGNATURA(PROFESOR PK, ASIGNATURA)

ALUMNO_PROFESOR(
  ALUMNO PK,
  PROFESOR PK FK -> PROFESOR_ASIGNATURA
)
```

La información original puede reconstruirse combinando ambas relaciones.

### 11.3. 3FN frente a BCNF

BCNF elimina más redundancia por dependencias funcionales. Sin embargo, alguna descomposición a BCNF puede no conservar todas las dependencias de forma local. En ciertos diseños se prefiere 3FN si garantiza unión sin pérdida y conservación de dependencias.

Por eso BCNF no es un «parche que siempre se aplica» después de 3FN: exige analizar compromisos.

---

## 12. Descomposición correcta

Separar columnas no basta. Una descomposición debe evaluarse con dos propiedades.

### 12.1. Unión sin pérdida

Una descomposición tiene **unión sin pérdida** si, al combinar sus relaciones mediante las columnas comunes, recuperamos exactamente las tuplas válidas de la relación original:

- no se pierde información;
- no aparecen tuplas espurias.

Para una descomposición binaria de $R$ en $R_1$ y $R_2$, es sin pérdida respecto de $F$ si se cumple al menos una de estas condiciones:

$$
(R_1 \cap R_2) \rightarrow R_1
$$

o

$$
(R_1 \cap R_2) \rightarrow R_2
$$

### 12.2. Ejemplo sin pérdida

Descomponemos:

```text
EDICION(id_edicion, id_curso, nombre_curso)
```

en:

```text
EDICION(id_edicion, id_curso)
CURSO(id_curso, nombre_curso)
```

La intersección es `{id_curso}` y `id_curso -> nombre_curso`, por lo que determina completamente `CURSO`. La unión es sin pérdida.

### 12.3. Tuplas espurias

Supongamos:

```text
R(ALUMNO, MODULO, DOCENTE)
```

con filas `(Ana, BD, Sara)` y `(Luis, PROG, Sara)`.

Si proyectamos sin justificación a:

```text
R1(ALUMNO, DOCENTE)
R2(MODULO, DOCENTE)
```

al combinarlas por docente aparecen también `(Ana, PROG, Sara)` y `(Luis, BD, Sara)`, que no estaban en la relación original. La descomposición tiene pérdida de información semántica, aunque no haya desaparecido ninguna celda.

### 12.4. Conservación de dependencias

Una descomposición **conserva dependencias** si las dependencias originales pueden comprobarse aplicando restricciones en las relaciones resultantes, sin tener que combinarlas.

La conservación facilita garantizar integridad. Es deseable, aunque en ocasiones entra en tensión con alcanzar BCNF.

---

## 13. Proceso recomendado hasta 3FN/BCNF

1. Escribir la relación universal o relación que se desea analizar.
2. Recoger las dependencias a partir de reglas del dominio.
3. Calcular una cobertura mínima si el conjunto es complejo.
4. Obtener todas las claves candidatas.
5. Marcar atributos primos y no primos.
6. Comprobar 1FN.
7. Eliminar incumplimientos de 2FN.
8. Eliminar incumplimientos de 3FN.
9. Comprobar BCNF y valorar sus descomposiciones.
10. Verificar unión sin pérdida.
11. Verificar conservación de dependencias.
12. Declarar PK, AK, FK, nulabilidad y restricciones.
13. Probar inserciones, modificaciones y borrados.

> [!TIP]
> En cada separación escribe la dependencia que la justifica. Si no puedes explicar qué regla estás aislando, probablemente estés aplicando una receta sin entenderla.

---

## 14. Ejemplo completo

Partimos de:

```text
INSCRIPCION(
  id_alumno,
  nombre_alumno,
  id_edicion,
  id_curso,
  nombre_curso,
  convocatoria,
  nota
)
```

Reglas:

- un identificador de alumno determina su nombre;
- una edición corresponde a un curso;
- un curso determina su nombre;
- un alumno puede presentarse en varias convocatorias de una edición;
- la nota depende de alumno, edición y convocatoria.

Dependencias:

$$
id\_alumno \rightarrow nombre\_alumno
$$

$$
id\_edicion \rightarrow id\_curso
$$

$$
id\_curso \rightarrow nombre\_curso
$$

$$
\{id\_alumno,id\_edicion,convocatoria\} \rightarrow nota
$$

Clave candidata:

```text
(id_alumno, id_edicion, convocatoria)
```

### 14.1. 1FN

Suponemos valores únicos y atómicos. Cumple 1FN.

### 14.2. 2FN

Incumplimientos:

- `id_alumno -> nombre_alumno`;
- `id_edicion -> id_curso` y, por transitividad, `nombre_curso`.

Ambos determinantes son partes propias de la clave compuesta.

Descomposición inicial:

```text
ALUMNO(id_alumno PK, nombre_alumno)

EDICION_TMP(id_edicion PK, id_curso, nombre_curso)

INSCRIPCION(
  id_alumno PK FK -> ALUMNO,
  id_edicion PK FK -> EDICION_TMP,
  convocatoria PK,
  nota
)
```

### 14.3. 3FN

En `EDICION_TMP`:

$$
id\_edicion \rightarrow id\_curso
$$

$$
id\_curso \rightarrow nombre\_curso
$$

La segunda DF incumple 3FN porque `id_curso` no es superclave de `EDICION_TMP` y `nombre_curso` no es primo.

Resultado:

```text
ALUMNO(id_alumno PK, nombre_alumno)

CURSO(id_curso PK, nombre_curso)

EDICION(
  id_edicion PK,
  id_curso FK -> CURSO NOT NULL
)

INSCRIPCION(
  id_alumno PK FK -> ALUMNO,
  id_edicion PK FK -> EDICION,
  convocatoria PK,
  nota
)
```

### 14.4. Verificación

- Cada hecho descriptivo se almacena una vez.
- Las combinaciones permiten reconstruir la información original.
- Las DFs se controlan en relaciones individuales.
- Una edición puede registrarse sin matrícula.
- Borrar la última inscripción no elimina el alumno, curso ni edición.

---

## 15. Cuarta forma normal — 4FN

La 4FN trata **dependencias multivaluadas** independientes.

Una dependencia multivaluada se escribe:

$$
X \twoheadrightarrow Y
$$

Indica que, para un valor de $X$, el conjunto de valores de $Y$ es independiente de los restantes atributos de la relación.

> [!WARNING]
> Una dependencia multivaluada no es una dependencia funcional. `PUESTO ->> TIPO_HAMBURGUESA` no significa que cada puesto determine un único tipo.

### 15.1. Ejemplo

Un puesto ofrece varios tipos de hamburguesa y reparte en varias zonas. Ambas listas son independientes.

```text
OFERTA(PUESTO, TIPO_HAMBURGUESA, ZONA_REPARTO)
```

| puesto | tipo_hamburguesa | zona_reparto |
|---|---|---|
| Norte | carne | Arrecife |
| Norte | carne | Tías |
| Norte | vegetal | Arrecife |
| Norte | vegetal | Tías |

Dependencias:

$$
PUESTO \twoheadrightarrow TIPO\_HAMBURGUESA
$$

$$
PUESTO \twoheadrightarrow ZONA\_REPARTO
$$

La combinación de ambas listas produce redundancia.

### 15.2. Definición de 4FN

Una relación está en 4FN si, para toda dependencia multivaluada no trivial $X \twoheadrightarrow Y$, $X$ es superclave.

### 15.3. Descomposición

```text
PUESTO_TIPO(PUESTO PK, TIPO_HAMBURGUESA PK)
PUESTO_ZONA(PUESTO PK, ZONA_REPARTO PK)
```

Solo es correcta si tipos y zonas son independientes. Si una hamburguesa concreta se reparte únicamente en ciertas zonas, el hecho ternario tiene significado y no debe eliminarse.

---

## 16. Quinta forma normal — 5FN

La 5FN o **forma normal de proyección-unión** trata dependencias de unión que no se explican únicamente mediante dependencias funcionales o multivaluadas.

Una relación está en 5FN cuando toda dependencia de unión no trivial está implicada por sus claves candidatas.

Suele aparecer en relaciones con tres o más participantes donde:

- el hecho ternario puede reconstruirse exactamente desde varias proyecciones;
- la reconstrucción no genera tuplas espurias;
- mantener el hecho completo produciría redundancia.

Ejemplo clásico orientativo:

```text
SUMINISTRO(PROVEEDOR, PIEZA, PROYECTO)
```

Podría descomponerse en:

```text
PROVEEDOR_PIEZA(PROVEEDOR, PIEZA)
PROVEEDOR_PROYECTO(PROVEEDOR, PROYECTO)
PIEZA_PROYECTO(PIEZA, PROYECTO)
```

solo si una regla del negocio garantiza que las tres asociaciones binarias implican exactamente el suministro ternario. Sin esa regla, la unión generaría combinaciones falsas.

La 5FN se presenta como ampliación: lo esencial para este nivel es comprender la dependencia de unión y el riesgo de tuplas espurias.

---

## 17. Resumen de formas normales

| Forma | Condición esencial | Problema principal |
|---|---|---|
| 1FN | Un valor del dominio por atributo y tupla; sin grupos repetitivos | Listas y estructuras repetidas en celdas |
| 2FN | 1FN y todo atributo no primo depende completamente de cada clave candidata | Dependencias parciales |
| 3FN | Para cada DF no trivial, determinante superclave o dependiente primo | Dependencias problemáticas entre atributos no clave |
| BCNF | Todo determinante de una DF no trivial es superclave | Determinantes que no son claves |
| 4FN | Todo determinante de una DMV no trivial es superclave | Multivalores independientes |
| 5FN | Toda dependencia de unión no trivial está implicada por claves | Redundancia de proyección-unión |

La secuencia es acumulativa:

```text
5FN ⇒ 4FN ⇒ BCNF ⇒ 3FN ⇒ 2FN ⇒ 1FN
```

---

## 18. Desnormalización responsable

Desnormalizar es introducir redundancia de forma deliberada para satisfacer una necesidad medible.

Puede considerarse cuando:

- una consulta crítica realiza combinaciones muy costosas;
- existe una carga analítica principalmente de lectura;
- se usan vistas materializadas o datos precalculados;
- se conserva una fuente de verdad normalizada;
- el sistema puede sincronizar y verificar las copias.

Antes de desnormalizar:

1. medir con datos y carga representativos;
2. revisar consultas, índices y planes de ejecución;
3. identificar el dato canónico;
4. definir cómo se actualiza la copia;
5. planificar detección y reparación de inconsistencias;
6. documentar el motivo y el coste.

> «Más rápido en una consulta de prueba» no basta para justificar duplicar información en producción.

---

## 19. Errores frecuentes

### Normalizar observando solo los datos actuales

Las DFs proceden de reglas permanentes del dominio.

### Comprobar solo la clave primaria

2FN y 3FN requieren conocer todas las claves candidatas.

### Confundir atributo primo con atributo de la PK

Es primo si pertenece a alguna candidata.

### Afirmar que 1FN elimina toda redundancia

1FN elimina grupos repetitivos; todavía puede haber dependencias parciales y transitivas.

### Separar cada dependencia sin verificar la unión

Una mala descomposición puede generar tuplas espurias.

### Creer que 3FN y BCNF son equivalentes

3FN permite una excepción cuando el dependiente es primo.

### Llamar funcional a una dependencia multivaluada

`X -> Y` y `X ->> Y` expresan reglas diferentes.

### Descomponer siempre una relación ternaria

La separación solo es correcta si una dependencia de unión del dominio lo garantiza.

### Normalizar por rendimiento

La normalización mejora integridad y mantenibilidad; su efecto sobre cada consulta debe medirse.

### Perder restricciones al crear un `id`

Una clave artificial no elimina las candidatas naturales. Deben conservarse como `UNIQUE` cuando la regla siga vigente.

---

## 20. Lista de comprobación

### Reglas y dependencias

- [ ] Las DFs proceden de reglas del dominio.
- [ ] Cada DF tiene determinante y dependiente bien definidos.
- [ ] Se han eliminado atributos extráneos y DFs redundantes cuando procede.
- [ ] Se han calculado todas las claves candidatas.
- [ ] Se han marcado atributos primos y no primos.

### Formas normales

- [ ] No existen listas ni grupos repetitivos.
- [ ] Los atributos no primos dependen de las claves candidatas completas.
- [ ] Cada DF cumple la condición formal de 3FN.
- [ ] Se han revisado determinantes que no sean superclaves para BCNF.
- [ ] Se han estudiado multivalores independientes, si existen.

### Descomposición

- [ ] Cada separación está justificada por una dependencia.
- [ ] La descomposición tiene unión sin pérdida.
- [ ] Se ha comprobado la conservación de dependencias.
- [ ] No aparecen tuplas espurias al recomponer.
- [ ] Se han conservado PK, AK, FK y reglas de nulabilidad.
- [ ] Los casos de inserción, actualización y borrado funcionan correctamente.

---

## 21. Actividades de autoevaluación

1. Identifica tres anomalías en una relación no normalizada.
2. Distingue una repetición necesaria de una redundancia perjudicial.
3. Propón DFs para una biblioteca y justifícalas mediante reglas.
4. Calcula el cierre de varios conjuntos de atributos.
5. Obtén todas las claves candidatas de una relación dada.
6. Distingue atributos primos y no primos.
7. Obtén una cobertura mínima.
8. Corrige una tabla con listas para llevarla a 1FN.
9. Detecta dependencias parciales y descompón a 2FN.
10. Detecta incumplimientos formales de 3FN.
11. Construye un ejemplo en 3FN que no esté en BCNF.
12. Comprueba si una descomposición binaria tiene unión sin pérdida.
13. Construye una descomposición que genere tuplas espurias.
14. Explica la diferencia entre conservar datos y conservar dependencias.
15. Identifica una dependencia multivaluada y descompón a 4FN.
16. Explica por qué una relación ternaria no siempre se puede descomponer a 5FN.
17. Propón un caso razonable de desnormalización y sus controles.

---

## 22. Glosario

| Término | Definición breve |
|---|---|
| Anomalía | Problema de inserción, actualización o borrado provocado por un diseño inadecuado. |
| Atributo primo | Atributo que pertenece a alguna clave candidata. |
| BCNF | Forma normal en la que todo determinante no trivial es superclave. |
| Cierre | Atributos que pueden derivarse desde un conjunto mediante unas DFs. |
| Cobertura mínima | Conjunto equivalente de DFs sin atributos ni dependencias redundantes. |
| Dependencia funcional | Regla por la que un conjunto de atributos determina otro. |
| Dependencia multivaluada | Regla que expresa conjuntos de valores independientes asociados a un determinante. |
| Dependencia parcial | DF respecto de una parte propia de una clave candidata compuesta. |
| Dependencia transitiva | Dependencia deducida a través de un conjunto intermedio. |
| Desnormalización | Introducción deliberada y controlada de redundancia. |
| Determinante | Parte izquierda de una dependencia. |
| Forma normal | Condición formal que limita determinados tipos de redundancia. |
| Pérdida | Imposibilidad de reconstruir exactamente la relación original. |
| Tupla espuria | Tupla falsa que aparece al recomponer una mala descomposición. |
| Unión sin pérdida | Propiedad que permite reconstruir exactamente la relación original. |

---

## Referencias

- [Real Decreto 405/2023: currículo básico actualizado del módulo Bases de Datos (0484)](https://www.boe.es/diario_boe/txt.php?id=BOE-A-2023-13221).
- Codd, E. F. (1971). *Further Normalization of the Data Base Relational Model*.
- Fagin, R. (1977). *Multivalued Dependencies and a New Normal Form for Relational Databases*. ACM Transactions on Database Systems, 2(3), 262-278.

> Las formas normales se aplican a relaciones junto con sus dependencias y claves. El nombre de las columnas o una pequeña muestra de filas no bastan para demostrar que un esquema está normalizado.
