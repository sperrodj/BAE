# Resumen. Relaciones y cardinalidades en el modelo E/R

Una relación expresa una asociación relevante entre ocurrencias de entidades. Para comprenderla debemos distinguir varias cuestiones: cuántos participantes intervienen, cuántas ocurrencias pueden asociarse, si la participación es obligatoria y si el vínculo tiene atributos propios.

## Índice

- [1. Cómo analizar una relación](#analizar)
- [2. Relación uno a uno](#uno-uno)
- [3. Relación uno a muchos](#uno-muchos)
- [4. Relación muchos a muchos](#muchos-muchos)
- [5. Atributos de las relaciones](#atributos)
- [6. Relaciones recursivas y roles](#recursivas)
- [7. Relaciones ternarias](#ternarias)
- [8. Relaciones identificadoras](#identificadoras)
- [9. Especialización y generalización](#herencia)
- [10. Identificadores compuestos](#compuestos)
- [11. Resumen y errores frecuentes](#resumen)

<a id="analizar"></a>
## 1. Cómo analizar una relación

No todas las clasificaciones responden a la misma pregunta:

| Aspecto | Pregunta | Posibilidades |
|---|---|---|
| Participantes | ¿Qué entidades o roles intervienen? | Recursiva, binaria, ternaria… |
| Cardinalidad máxima | ¿Cuántas ocurrencias admite cada extremo? | `1:1`, `1:N`, `N:M` |
| Participación mínima | ¿Es obligatorio participar? | Opcional o total |
| Atributos | ¿Hay datos que describen el vínculo? | Con atributos o sin ellos |
| Identificación | ¿El vínculo contribuye a identificar una entidad dependiente? | Identificadora o no identificadora |

Una relación puede ser, por ejemplo, binaria, `N:M`, opcional en ambos extremos y tener atributos. No son categorías excluyentes.

### 1.1. Las dos preguntas fundamentales

Para una relación entre A y B:

1. Para **una A**, ¿cuántas B puede o debe haber?
2. Para **una B**, ¿cuántas A puede o debe haber?

En cada respuesta indicamos mínimo y máximo:

| Pareja | Significado |
|---|---|
| `(0,1)` | Ninguna o una |
| `(1,1)` | Exactamente una |
| `(0,N)` | Ninguna, una o muchas |
| `(1,N)` | Una o muchas |

`N` representa un máximo de «muchas» sin precisar una cifra en este esquema; el mínimo uno no exige al menos dos. Si existe un límite concreto, como cinco préstamos activos, hay que anotarlo expresamente.

Cuando el enunciado no aclara un mínimo, formulamos una pregunta o documentamos una suposición. «Puede tener varios» no permite deducir por sí solo que deba tener alguno.

<a id="uno-uno"></a>
## 2. Relación uno a uno: 1:1

En ambos sentidos, una ocurrencia puede relacionarse **como máximo con una** del otro extremo. Eso no determina si la participación es obligatoria.

### Ejemplo: empleado y tarjeta de acceso

Para este ejemplo, cada empleado puede tener una única tarjeta activa. Cada tarjeta puede estar sin asignar o asignada a un único empleado. No se conserva aquí el historial de asignaciones.

| Partimos de… | Contamos… | Cardinalidad |
|---|---|---|
| Un EMPLEADO | Sus TARJETAS activas | `(0,1)` |
| Una TARJETA | Sus EMPLEADOS asignados | `(0,1)` |

La relación es `1:1` y opcional en ambos sentidos.

Si la empresa exigiera que todo empleado tuviera tarjeta, la primera pareja cambiaría a `(1,1)`, pero la relación seguiría siendo `1:1` por sus máximos.

> Los ejemplos de oficina o pasaporte necesitan reglas adicionales: una oficina puede compartirse y no debemos asumir sin más un único documento por persona. La cardinalidad se deduce de las condiciones del ejercicio.

<a id="uno-muchos"></a>
## 3. Relación uno a muchos: 1:N

Una ocurrencia de A puede asociarse a varias de B, mientras que una de B se asocia como máximo a una de A.

### Ejemplo: proveedor y producto

Cada producto tiene exactamente un proveedor asignado. Un proveedor puede suministrar varios productos y puede registrarse antes de suministrar alguno.

| Partimos de… | Contamos… | Cardinalidad |
|---|---|---|
| Un PROVEEDOR | Sus PRODUCTOS | `(0,N)` |
| Un PRODUCTO | Sus PROVEEDORES | `(1,1)` |

La relación es `1:N` desde PROVEEDOR hacia PRODUCTO. Leída al revés se expresa como `N:1`; es la misma asociación vista desde el otro lado.

La existencia de un producto sin proveedor se rechaza por el mínimo uno. Un proveedor sin productos se admite por el mínimo cero.

<a id="muchos-muchos"></a>
## 4. Relación muchos a muchos: N:M

Una ocurrencia de cada extremo puede asociarse a varias del otro.

### Ejemplo: alumno y módulo

Cada alumno está matriculado en uno o varios módulos. Un módulo admite varios alumnos y puede existir antes de recibir matrículas.

| Partimos de… | Contamos… | Cardinalidad |
|---|---|---|
| Un ALUMNO | Sus MODULOS | `(1,N)` |
| Un MODULO | Sus ALUMNOS | `(0,N)` |

Esta relación es `N:M`, obligatoria para ALUMNO y opcional para MODULO.

Una relación `N:M` es válida en el modelo conceptual. No hay que eliminarla por anticipar su implementación. Al transformarla al modelo relacional, normalmente se representará mediante una tabla de asociación.

La relación simple indica qué parejas están vinculadas. Si necesitamos distinguir matrículas repetidas en distintos periodos, habrá que incorporar el periodo o representar cada matrícula con identidad propia.

<a id="atributos"></a>
## 5. Atributos de las relaciones

Un atributo pertenece a una relación si describe **la asociación concreta**, no a un participante de forma aislada. Puede haber atributos tanto en relaciones `1:N` como `N:M`.

### 5.1. Ejemplo en una relación 1:N

Una empresa asigna a cada empleado como máximo a un departamento actual. Un departamento puede tener varios empleados. La relación ASIGNACION incluye `fecha_inicio`, que indica cuándo comenzó esa asignación.

| Dato | Qué describe |
|---|---|
| Nombre del empleado | EMPLEADO |
| Nombre del departamento | DEPARTAMENTO |
| Fecha de inicio de la asignación actual | El vínculo ASIGNACION |

No necesitamos una «tabla intermedia» para explicar este modelo conceptual. La forma de almacenar el atributo se decidirá al transformar el modelo.

Si se pidiera conservar todos los cambios de departamento, habría que representar asignaciones históricas y distinguirlas entre sí.

### 5.2. Ejemplo en una relación N:M

Un alumno se matricula en una edición de un curso. De esa inscripción se guardan la fecha y el estado. Suponemos una sola inscripción por pareja alumno-edición.

| Alumno | Edición | Fecha de matrícula | Estado |
|---|---|---|---|
| Ana | BD-octubre | 2026-09-20 | Confirmada |
| Ana | Redes-noviembre | 2026-10-15 | Pendiente |
| Bruno | BD-octubre | 2026-09-22 | Confirmada |

La fecha depende de la inscripción concreta. No sería correcto guardar una única `fecha_matricula` en ALUMNO, porque Ana tiene dos inscripciones con fechas diferentes.

En Chen se pueden conectar los atributos al rombo de la relación. También puede representarse MATRICULA como entidad asociativa si interesa tratarla como un concepto propio. No basta con decir «tiene atributos, por tanto siempre debe convertirse en entidad».

### 5.3. No confundir atributos de una operación con atributos de su relación

Si VENTA es una entidad con identificador propio, `fecha` y `total` pueden ser atributos de VENTA. El hecho de que CLIENTE realice VENTA mediante una relación `1:N` no convierte automáticamente esos datos en atributos de REALIZA.

La pregunta es siempre: **¿qué hecho describe este valor?**

<a id="recursivas"></a>
## 6. Relaciones recursivas y roles

Una relación es **recursiva** cuando un mismo tipo de entidad participa en distintos papeles. En la clasificación introductoria se habla también de relación unaria por intervenir un único tipo de entidad.

### Ejemplo: empleados que supervisan a otros empleados

| Supervisor | Supervisado |
|---|---|
| Laura | Ana |
| Laura | Bruno |
| Ana | Carla |

Los roles son **supervisor** y **supervisado**, pero ambos pertenecen a EMPLEADO. Ana participa en ambos papeles respecto a personas diferentes.

Suponemos que cada empleado tiene cero o un supervisor directo y puede supervisar a cero o muchos empleados.

| Partimos de… | Contamos… | Cardinalidad |
|---|---|---|
| Un empleado como supervisor | Empleados supervisados | `(0,N)` |
| Un empleado como supervisado | Supervisores directos | `(0,1)` |

La relación es recursiva y de máximos `1:N`. Lo recursivo no determina su cardinalidad: también podrían existir relaciones recursivas `1:1` o `N:M` con otras reglas.

Hay que documentar condiciones adicionales, como que una persona no pueda supervisarse a sí misma. Si la jerarquía no admite ciclos, también debe indicarse.

<a id="ternarias"></a>
## 7. Relaciones ternarias

Una relación ternaria vincula simultáneamente tres participantes.

**Ejemplo:** un proveedor suministra un producto para un proyecto. El hecho completo necesita los tres datos:

| Proveedor | Producto | Proyecto |
|---|---|---|
| P1 | Teclado | Aula Norte |
| P1 | Ratón | Aula Sur |
| P2 | Teclado | Aula Sur |

Si conservamos solo las parejas proveedor-producto, proveedor-proyecto y producto-proyecto, aparecerán las tres parejas de la combinación «P1, Teclado, Aula Sur». Sin embargo, esa combinación no figura en el conjunto original.

Por eso no se debe sustituir una relación ternaria automáticamente por tres binarias: podrían admitirse asociaciones que no existían.

Para estudiar los máximos en una ternaria preguntamos por el tercer participante **manteniendo fija una combinación de los otros dos**. Por ejemplo: para un producto y un proyecto concretos, ¿puede haber uno o varios proveedores? La notación utilizada debe dejar clara esa interpretación.

<a id="identificadoras"></a>
## 8. Relaciones identificadoras

Una relación identificadora conecta una entidad débil con la entidad propietaria que necesita para identificarse.

### Ejemplo: hotel y habitación

Suponemos que las habitaciones se numeran dentro de cada hotel y no tienen otro identificador propio global.

| Hotel | Número de habitación | Identificación completa |
|---|---|---|
| H01 | 101 | `(H01, 101)` |
| H02 | 101 | `(H02, 101)` |

El número 101 no basta para identificar la habitación. Hace falta el hotel. El número es un **identificador parcial**.

Cada habitación pertenece obligatoriamente a un hotel. El mínimo de habitaciones por hotel dependerá de si permitimos registrar un hotel antes de introducir sus habitaciones.

Una relación obligatoria no siempre es identificadora. Un pedido con código propio puede necesitar un cliente para existir y seguir teniendo identificación independiente.

<a id="herencia"></a>
## 9. Especialización y generalización

La **especialización** y la **generalización** son construcciones del modelo E/R extendido. Expresan una relación de «es un tipo de», diferente de asociaciones como COMPRA o IMPARTE.

- En la especialización partimos de un concepto general y distinguimos subclases.
- En la generalización partimos de conceptos semejantes y reunimos sus propiedades comunes en una superclase.

**Ejemplo:** PERSONA puede especializarse en ALUMNO y DOCENTE. Una ocurrencia de ALUMNO también es una PERSONA y hereda su identificador y propiedades comunes.

| Restricción | Opciones | Pregunta del ejemplo |
|---|---|---|
| Completitud | Total o parcial | ¿Toda persona debe ser alumno o docente? |
| Solapamiento | Disjunta o solapada | ¿Una persona puede ser ambas cosas? |

Si admitimos personas sin ninguno de esos papeles y otras que sean alumnado y profesorado a la vez, la especialización será **parcial y solapada**.

No se describe adecuadamente como una relación ordinaria `1:1`: hay inclusión de conjuntos y herencia, además de las restricciones anteriores.

<a id="compuestos"></a>
## 10. Identificadores compuestos

Un **identificador compuesto** utiliza varios atributos necesarios conjuntamente. No es un tipo de relación.

Por ejemplo, la pareja «hotel y número de habitación» puede identificar una habitación en el supuesto del apartado 8. La composición del identificador es una cuestión diferente de la cardinalidad de HOTEL–HABITACION.

Al transformar una relación `N:M` al modelo relacional puede utilizarse una clave compuesta para su tabla de asociación. Esa es una decisión posterior: no existe una categoría conceptual llamada «relación de clave compuesta».

<a id="resumen"></a>
## 11. Resumen y errores frecuentes

| Error | Corrección |
|---|---|
| «1:1 significa que ambas entidades deben participar» | Solo indica máximos; faltan los mínimos |
| «Un autor siempre se relaciona con libros mediante 1:N» | Depende de si se admite coautoría |
| «Una relación 1:N con atributos necesita una tabla intermedia» | Se están mezclando diseño conceptual e implementación |
| «Todas las relaciones N:M deben desaparecer del E/R» | Son válidas en el modelo conceptual |
| «Recursiva significa muchos a muchos» | Recursividad y cardinalidad son aspectos diferentes |
| «La herencia es otra relación 1:1» | Expresa especialización e inclusión de ocurrencias |
| «Clave compuesta es un tipo de relación» | Describe la composición de un identificador |
| «Las cardinalidades expresan todas las reglas» | Hay restricciones temporales y semánticas que deben escribirse aparte |

Para estudiar cualquier asociación, escribe primero las frases en ambos sentidos, identifica qué cuenta cada pregunta y determina mínimos y máximos. Después comprueba casos habituales, casos límite y situaciones que deberían rechazarse.
