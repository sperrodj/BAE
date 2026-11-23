# Tarea evaluable: Diseño y normalización de una base de datos

## Descripción de la actividad

En esta actividad deberán diseñar y normalizar una pequeña base de datos a partir de una situación real.

Cada estudiante deberá seleccionar **uno de los siguientes contextos**:

- Sistema de reservas.
- Tienda o comercio.
- Torneo o competición.
- Clínica veterinaria.
- Plataforma educativa.

También se podrá utilizar otro contexto, siempre que haya sido aprobado previamente por la profesora.

El objetivo es partir de una única relación con problemas de diseño y transformarla, de manera razonada, en un modelo relacional que cumpla la **tercera forma normal (3FN)**.

## Trabajo que se debe realizar

### 1. Descripción del supuesto

Expliquen brevemente:

- en qué consiste el sistema elegido;
- qué información necesita almacenar;
- qué operaciones debería permitir;
- qué representa cada fila de la relación inicial.

La descripción deberá tener una extensión aproximada de entre **150 y 250 palabras**.

### 2. Relación inicial

Diseñen una única relación que mezcle información perteneciente, al menos, a **tres hechos o conceptos diferentes**.

Por ejemplo, una tabla podría mezclar información sobre clientes, reservas y servicios.

Deberán indicar:

- nombre de la relación;
- atributos que contiene;
- significado de cada atributo;
- posible clave de la relación.

La relación inicial deberá contener suficientes atributos para que existan problemas de redundancia y sea necesario aplicar el proceso de normalización.

> [!IMPORTANT]
> No se debe presentar desde el principio una base de datos ya normalizada.

### 3. Datos de ejemplo

Incluyan, como mínimo, **cinco filas de ejemplo**.

Los datos deberán permitir observar alguno de los siguientes problemas:

- repetición de información;
- grupos de valores o datos no atómicos;
- dependencias parciales;
- dependencias transitivas;
- posibles anomalías de inserción, actualización o eliminación.

Los datos utilizados pueden ser ficticios, pero deberán resultar coherentes con el supuesto planteado.

### 4. Reglas del negocio

Redacten las reglas que determinan cómo se relacionan los datos.

Por ejemplo:

- cada reserva pertenece a un único cliente;
- un cliente puede realizar varias reservas;
- cada servicio se identifica mediante un código;
- una reserva puede incluir varios servicios;
- un servicio puede aparecer en distintas reservas.

Las reglas deberán ser claras y no podrán contradecir los datos de ejemplo ni el esquema final.

### 5. Claves y dependencias funcionales

Identifiquen:

- las claves candidatas de la relación inicial;
- la clave primaria seleccionada;
- los atributos primos y no primos;
- las dependencias funcionales existentes;
- las dependencias parciales y transitivas, cuando existan.

Las dependencias funcionales deberán expresarse mediante la notación estudiada:

```text
atributo_determinante → atributo_dependiente
```

Cuando el determinante esté compuesto por varios atributos:

```text
(atributo_1, atributo_2) → atributo_dependiente
```

Cada dependencia deberá acompañarse de una breve justificación basada en las reglas del negocio.

### 6. Proceso de normalización

Normalicen la relación inicial explicando cada paso.

#### Primera forma normal — 1FN

Indiquen:

- si la relación inicial cumple 1FN;
- qué atributos incumplen esta forma normal, si los hubiera;
- qué cambios son necesarios para conseguir valores atómicos y eliminar grupos repetitivos.

#### Segunda forma normal — 2FN

Indiquen:

- cuál es la clave de la relación;
- si existen dependencias parciales;
- qué relaciones se crean para eliminarlas;
- qué dependencia funcional justifica cada nueva relación.

#### Tercera forma normal — 3FN

Indiquen:

- si existen dependencias transitivas;
- qué atributos no clave dependen de otros atributos no clave;
- qué nuevas relaciones se crean;
- por qué el esquema resultante cumple 3FN.

> [!WARNING]
> No será suficiente con mostrar únicamente el resultado final. Se debe explicar y justificar cada transformación.

### 7. Esquema relacional final

Presenten todas las relaciones obtenidas utilizando una notación similar a la siguiente:

```text
CLIENTE(
  id_cliente PK,
  nombre,
  correo UNIQUE
)

RESERVA(
  id_reserva PK,
  fecha,
  id_cliente FK → CLIENTE
)

SERVICIO(
  id_servicio PK,
  nombre,
  precio
)

DETALLE_RESERVA(
  id_reserva PK FK → RESERVA,
  id_servicio PK FK → SERVICIO,
  cantidad
)
```

En cada relación deberán identificarse:

- claves primarias (`PK`);
- claves ajenas (`FK`);
- claves compuestas;
- restricciones `UNIQUE`, cuando sean necesarias;
- atributos que pueden admitir valores nulos, si procede.

También deberán incluir un **diagrama del modelo relacional final**. Se podrá elaborar con una herramienta digital o a mano, siempre que el resultado sea legible.

### 8. Anomalías evitadas

Expliquen, como mínimo, **dos anomalías** presentes en la relación inicial y cómo el diseño normalizado permite evitarlas.

Pueden analizar:

- anomalía de inserción;
- anomalía de actualización;
- anomalía de eliminación.

La explicación deberá estar relacionada directamente con los datos y las relaciones del supuesto desarrollado.

## Formato de entrega

La actividad se entregará mediante un **repositorio de GitHub**.

El repositorio deberá contener la siguiente estructura mínima:

```text
normalizacion/
├── README.md
└── imagenes/
    └── modelo-relacional.png
```

El archivo `README.md` deberá contener todo el desarrollo de la actividad siguiendo el orden establecido en este enunciado.

Si el diagrama se realiza a mano, deberán escanearlo o fotografiarlo con suficiente calidad y guardarlo en la carpeta `imagenes`. La imagen deberá aparecer enlazada o insertada dentro del `README.md`.

La entrega consistirá en proporcionar el **enlace al repositorio de GitHub**.

## Criterios de calificación

| Aspecto evaluado | Puntuación |
| --- | ---: |
| Descripción del supuesto y coherencia de los datos | 1 punto |
| Reglas del negocio | 1 punto |
| Identificación de claves y dependencias funcionales | 2 puntos |
| Aplicación y justificación de 1FN | 1 punto |
| Aplicación y justificación de 2FN | 1,5 puntos |
| Aplicación y justificación de 3FN | 1,5 puntos |
| Esquema relacional final y diagrama | 1 punto |
| Explicación de las anomalías evitadas | 0,5 puntos |
| Presentación, organización y uso adecuado de Markdown | 0,5 puntos |
| **Total** | **10 puntos** |

## Condiciones de realización

- La actividad tendrá carácter **individual**.
- Todas las decisiones de diseño deberán estar justificadas.
- No se valorará positivamente crear un gran número de tablas sin una razón basada en las dependencias funcionales.
- Las claves y relaciones deberán ser coherentes con las reglas del negocio.
- El repositorio deberá ser accesible durante el periodo de corrección.
- El contenido presentado deberá ser de elaboración propia.
- Si se utilizan recursos externos, deberán indicarse en un apartado de referencias.

> [!NOTE]
> Se valorará especialmente la capacidad para explicar el proceso seguido. Una solución correctamente razonada tendrá más valor que una descomposición extensa presentada sin justificación.
