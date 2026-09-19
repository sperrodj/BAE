# Tema 1. Almacenamiento de la información

Las aplicaciones necesitan conservar información para poder recuperarla y utilizarla más adelante. Una tienda guarda productos y pedidos; un centro educativo, matrículas y calificaciones; una aplicación de música, canciones y listas de reproducción. En este tema estudiaremos cómo se organiza esa información y qué herramientas permiten gestionarla.

## Índice

- [1. Del dato a la información persistente](#apartado-1)
- [2. Ficheros como sistema de almacenamiento](#apartado-2)
- [3. Bases de datos y SGBD](#apartado-3)
- [4. Clasificación por modelo de datos](#apartado-4)
- [5. Clasificación de los SGBD](#apartado-5)
- [6. Ubicación y distribución de la información](#apartado-6)
- [7. Cargas transaccionales y analíticas](#apartado-7)
- [8. Big Data, analítica e inteligencia de negocio](#apartado-8)
- [9. Protección de datos desde el diseño](#apartado-9)
- [10. Cómo elegir una solución](#apartado-10)
- [11. Resumen](#apartado-11)

<a id="apartado-1"></a>
## 1. Del dato a la información persistente

Un **dato** es una representación de un hecho o valor. Se convierte en **información** cuando se interpreta en un contexto. El valor `18`, por ejemplo, no significa lo mismo si representa una edad, una temperatura o el número de unidades disponibles.

La **persistencia** permite conservar los datos cuando termina un programa o se apaga el equipo. Un sistema de almacenamiento debe responder, entre otras, a estas necesidades:

- capacidad y crecimiento;
- velocidad de lectura y escritura;
- acceso simultáneo;
- integridad y consistencia;
- disponibilidad y recuperación;
- seguridad y privacidad;
- coste y facilidad de mantenimiento.

No existe una tecnología óptima para todos los casos. La elección depende del problema.

### 1.1. Datos, información y metadatos

| Concepto | Significado | Ejemplo |
|---|---|---|
| Dato | Valor que representa un hecho | `18` |
| Información | Dato interpretado en un contexto | Quedan 18 unidades de un producto |
| Metadato | Descripción de otros datos | El campo `stock` contiene una cantidad entera |

Los metadatos ayudan a interpretar y gestionar los datos: indican nombres, tipos, formatos o reglas. Conocer únicamente un valor no basta si desconocemos qué representa.

### 1.2. Almacenamiento físico y lógico

El **almacenamiento físico** se refiere a los dispositivos donde se conservan los datos, como SSD, discos duros o cintas. El **almacenamiento lógico** se refiere a cómo se organizan y se accede a ellos: ficheros, registros, índices o estructuras de bases de datos.

> **Ejemplo:** un CSV y una base de datos pueden estar en el mismo SSD. Comparten soporte físico, pero su organización y los mecanismos utilizados para consultarlos son diferentes.

La RAM convencional es volátil: pierde su contenido al interrumpirse la alimentación. Guardar datos de forma persistente permite recuperarlos después, pero no evita que puedan borrarse o que el dispositivo falle. Por eso, persistencia y copia de seguridad son conceptos diferentes.

<a id="apartado-2"></a>
## 2. Ficheros como sistema de almacenamiento

Un **fichero** es una secuencia de bytes identificada normalmente por un nombre y gestionada por un sistema de archivos. Su interpretación depende del formato y del programa que lo procesa.

### 2.1. Formatos habituales

- **Texto plano:** contenido legible como texto, sin una estructura obligatoria.
- **CSV:** datos tabulares sencillos separados por delimitadores.
- **JSON:** documentos con objetos, arrays y valores; común en API y configuración.
- **XML:** documentos jerárquicos con etiquetas y esquemas opcionales.
- **Binario:** formato específico de una aplicación o estándar, interpretado mediante software compatible; puede ser más compacto o eficiente según su diseño.

La extensión orienta, pero no garantiza el formato real del contenido. Renombrar `productos.csv` como `productos.json` no transforma los datos.

**El mismo producto en distintos formatos:**

CSV representa los datos como filas y campos. En este ejemplo, la primera línea contiene sus nombres:

```csv
id,nombre,precio,stock
1,Teclado,25.50,18
2,Raton,12.00,30
```

JSON representa objetos mediante propiedades y valores:

```json
{
  "id": 1,
  "nombre": "Teclado",
  "precio": 25.50,
  "stock": 18
}
```

XML utiliza elementos y atributos:

```xml
<producto id="1">
  <nombre>Teclado</nombre>
  <precio>25.50</precio>
  <stock>18</stock>
</producto>
```

No es necesario memorizar estas sintaxis en este tema. Lo importante es observar que podemos representar los mismos datos de distintas formas. Un archivo JSON no constituye por sí solo un sistema gestor documental.

Aunque todos los ficheros se almacenan como bits, llamamos **formato binario** a uno que no está diseñado para leerse directamente como texto. Una imagen o el archivo de una base de datos requieren herramientas que entiendan su estructura.

### 2.2. Formas de acceso

- **Secuencial:** se recorren los datos en orden. Es sencillo y resulta adecuado para procesamiento por lotes.
- **Directo o aleatorio:** se accede a una posición sin leer todo lo anterior.
- **Indexado:** una estructura auxiliar relaciona una clave con la ubicación del dato y acelera las búsquedas.

| Acceso | Ejemplo | Aspecto que hay que considerar |
|---|---|---|
| Secuencial | Recorrer todos los movimientos de un día | Para llegar a un registro puede ser necesario leer los anteriores |
| Directo | Leer un registro cuya posición se conoce | Necesitamos poder determinar su ubicación |
| Indexado | Buscar un producto mediante un índice de identificadores | El índice ocupa espacio y debe actualizarse |

Un índice se parece al índice de un libro: ayuda a localizar contenido sin recorrer todas las páginas. Sin embargo, no acelera cualquier búsqueda ni elimina el coste de acceder al dato.

### 2.3. Operaciones básicas

Sobre un fichero se realizan operaciones como crear, abrir, leer, escribir, anexar, renombrar, copiar, cerrar y eliminar. Sobre los datos pueden realizarse altas, consultas, modificaciones y bajas.

Las operaciones sobre los datos suelen resumirse con las siglas **CRUD**:

| Operación | Significado | Ejemplo |
|---|---|---|
| Create | Alta | Añadir un producto |
| Read | Consulta | Consultar su precio |
| Update | Modificación | Cambiar las unidades disponibles |
| Delete | Baja | Eliminar un registro de prueba |

Eliminar un registro no significa eliminar todo el fichero que lo contiene.

### 2.4. Ventajas y limitaciones

Los ficheros son apropiados para configuración, intercambio de datos, registros de eventos, documentos o conjuntos pequeños con reglas sencillas. No deben presentarse como una tecnología superada: siguen siendo una pieza básica de los sistemas actuales.

Cuando cada aplicación mantiene sus propios ficheros aparecen problemas frecuentes:

- duplicación e inconsistencia de datos;
- fuerte dependencia entre formato y código;
- validaciones repetidas en distintas aplicaciones;
- dificultad para consultar información relacionada;
- control de concurrencia y permisos implementado de forma artesanal;
- copias de seguridad y recuperación poco coordinadas.

> **Ejemplo de inconsistencia:** el departamento de ventas guarda un teléfono de un cliente y el de facturación guarda otro distinto como teléfono actual. Si cada departamento actualiza su archivo por separado, deja de estar claro cuál es el dato correcto.

La **redundancia** es la repetición de datos. La **inconsistencia** aparece cuando las representaciones de un mismo dato se contradicen. No toda redundancia es incorrecta: algunas copias son intencionadas, pero deben mantenerse de forma controlada.

<a id="apartado-3"></a>
## 3. Bases de datos y SGBD

Una **base de datos** es una colección organizada de datos relacionados, diseñada para ser almacenada, consultada y actualizada. Un **sistema gestor de bases de datos (SGBD)** es el software que permite definir, manipular, proteger y administrar esos datos.

Conviene no confundir:

- **base de datos:** los datos, su estructura y sus restricciones;
- **SGBD:** el software que los gestiona;
- **aplicación:** el programa que utiliza el SGBD para resolver una necesidad del usuario.

| Elemento | Ejemplo en una tienda |
|---|---|
| Base de datos | Productos, clientes y pedidos almacenados |
| SGBD | Software que recibe y ejecuta las operaciones sobre esos datos |
| Aplicación | Pantallas donde el personal consulta y registra pedidos |

**SQL** es un lenguaje utilizado para definir y manipular datos en muchos gestores, especialmente relacionales. No es el nombre de una base de datos ni de un producto concreto.

### 3.1. Qué aporta un SGBD

- definición de estructuras, tipos y restricciones;
- consultas y modificación de datos;
- control de concurrencia y transacciones;
- autenticación, roles y permisos;
- catálogo o metadatos;
- registro de actividad y diagnóstico;
- copias de seguridad y recuperación;
- mecanismos de replicación, alta disponibilidad y optimización.

Estas capacidades dependen del producto y de su configuración. El gestor no conoce por sí solo las reglas de una empresa: hay que establecer qué datos son válidos y qué operaciones están permitidas.

Dos funciones especialmente importantes son las **transacciones** y el **control de concurrencia**:

- Una transacción agrupa operaciones que deben completarse como una unidad. Si no se pueden completar, se deshacen sus efectos.
- El control de concurrencia coordina operaciones simultáneas o solapadas para evitar interferencias indebidas.

> **Ejemplo:** al realizar una transferencia, se descuenta dinero de una cuenta y se añade a otra. No sería correcto confirmar solo uno de esos cambios. Una transacción permite tratarlos conjuntamente.
>
> Si dos personas intentan comprar la última unidad de un producto, también hacen falta reglas y coordinación para impedir venderla dos veces.

El SGBD reduce trabajo en las aplicaciones, pero añade administración, consumo de recursos y complejidad. Para un fichero de configuración pequeño, introducir un servidor de bases de datos puede ser innecesario.

### 3.2. Componentes conceptuales

- **Motor de almacenamiento:** organiza páginas, ficheros, índices y acceso físico.
- **Procesador de consultas:** interpreta y planifica las operaciones solicitadas.
- **Gestor de transacciones:** coordina concurrencia, confirmación y recuperación.
- **Catálogo de datos:** conserva metadatos sobre objetos, restricciones y usuarios.
- **Subsistema de seguridad:** aplica autenticación, autorización y auditoría.
- **Herramientas de administración:** permiten supervisar, copiar, restaurar y configurar.

El procesador de consultas puede incorporar un **optimizador**, que decide cómo ejecutar una petición de manera eficiente. La gestión de memoria intermedia conserva temporalmente páginas de datos para reducir accesos al almacenamiento. Los mecanismos de recuperación ayudan a volver a un estado válido tras determinados fallos.

| Petición o necesidad | Elemento que interviene principalmente |
|---|---|
| Localizar productos por precio | Procesador de consultas y optimizador |
| Leer los registros del almacenamiento | Motor de almacenamiento |
| Confirmar todas las operaciones de una compra | Gestor de transacciones |
| Consultar la definición de un campo | Catálogo o diccionario de datos |
| Comprobar si se permite una modificación | Subsistema de seguridad |
| Preparar una copia y comprobar su restauración | Herramientas de administración y recuperación |

Los componentes colaboran: una misma operación suele utilizar varios de ellos.

El **administrador de bases de datos (DBA)** es un rol humano, no un componente de software. Define políticas, controla permisos, vigila el rendimiento y planifica la continuidad del servicio.

### 3.3. Niveles de abstracción

La arquitectura de tres esquemas distingue:

1. **Nivel interno:** cómo se almacenan físicamente los datos.
2. **Nivel conceptual:** estructura lógica global, relaciones y restricciones.
3. **Nivel externo:** vistas específicas para usuarios o aplicaciones.

La **independencia física** permite cambiar aspectos del almacenamiento sin rediseñar el esquema lógico. La **independencia lógica** permite modificar el esquema conceptual reduciendo el impacto sobre determinadas vistas y aplicaciones. Ninguna de las dos implica que todo cambio sea automáticamente compatible.

> **Ejemplo de abstracción:** el personal de ventas consulta productos y pedidos, mientras que el de administración accede a información de facturación. El esquema global organiza todos esos datos; el nivel interno determina cómo se almacenan.

Cambiar un índice es un ejemplo de modificación física que normalmente no obliga a cambiar las consultas de la aplicación. Renombrar un campo, en cambio, puede afectar al código que utiliza su nombre anterior.

<a id="apartado-4"></a>
## 4. Clasificación por modelo de datos

Un **modelo de datos** define conceptos para representar datos, relaciones, restricciones y operaciones.

| Modelo | Organización | Uso orientativo |
|---|---|---|
| Relacional | Tablas relacionadas, claves y restricciones | Gestión transaccional y consultas estructuradas |
| Documental | Documentos, normalmente similares a JSON | Datos semiestructurados y esquemas flexibles |
| Clave-valor | Pares clave-valor | Caché, sesiones y acceso muy rápido por clave |
| Columnas anchas | Filas con familias de columnas distribuidas | Grandes volúmenes y escrituras distribuidas |
| Grafos | Nodos, relaciones y propiedades | Redes, rutas, fraude y recomendaciones |
| Orientado a objetos | Objetos con identidad y estructuras complejas | Persistencia de objetos de aplicaciones |

También existen modelos con importancia histórica:

- **Jerárquico:** organiza registros en un árbol; cada registro hijo tiene un único padre dentro de esa estructura.
- **En red:** permite conectar registros mediante varios enlaces y caminos de navegación. No significa simplemente que la base de datos sea accesible por Internet.

### 4.1. Modelo relacional

Organiza los datos en tablas con filas y columnas. Las filas representan registros y las columnas, características de esos registros. Los datos de distintas tablas pueden relacionarse mediante valores identificadores.

| id_producto | nombre | precio |
|---|---|---|
| 1 | Teclado | 25,50 |
| 2 | Ratón | 12,00 |

Esta tabla permite reconocer la organización relacional. El diseño detallado de tablas y relaciones se estudiará en otros temas.

### 4.2. Modelos no relacionales

La denominación **NoSQL** reúne diferentes familias, como documentos, clave-valor, columnas anchas y grafos. No describe un único modelo ni significa que los datos carezcan de estructura.

- En un modelo documental, la ficha de un producto puede contener listas de características.
- En uno clave-valor, el identificador de una sesión permite recuperar su contenido.
- En un grafo, las personas pueden representarse como nodos y sus vínculos como relaciones.

Los modelos flexibles también requieren decisiones sobre organización, validación y acceso. NoSQL no es automáticamente más rápido ni más adecuado para cualquier volumen de datos. [Documentación de MongoDB sobre modelado](https://www.mongodb.com/docs/manual/data-modeling/).

### 4.3. Datos especializados y sistemas multimodelo

Algunos sistemas se especializan en **series temporales**, como medidas de sensores asociadas a instantes, o en **búsqueda vectorial**, que compara representaciones numéricas para encontrar elementos similares. Estas capacidades también pueden incorporarse a gestores de otros modelos.

Un producto puede admitir varios modelos. Por eso, «modelo», «producto» y «arquitectura de despliegue» no son categorías equivalentes.

<a id="apartado-5"></a>
## 5. Clasificación de los SGBD

Los SGBD se pueden clasificar mediante varios criterios simultáneos:

| Criterio | Posibilidades | Qué describe |
|---|---|---|
| Modelo | Relacional, documental, grafos, clave-valor, multimodelo… | Cómo representa los datos |
| Despliegue | Embebido o cliente-servidor | Si se integra en la aplicación o funciona como servidor independiente |
| Acceso | Uso individual o multiusuario, según capacidades | Cómo admite y coordina usuarios concurrentes |
| Distribución | Centralizado o distribuido | Cómo coordina los nodos de datos |
| Licencia | Libre, propietaria u otras condiciones específicas | Derechos de uso, modificación y distribución |
| Administración | Autoadministrado o servicio gestionado | Quién asume las tareas operativas |
| Carga | Transaccional, analítica o híbrida | Tipo principal de operaciones |

El alojamiento en la nube puede combinarse con distintas opciones de modelo, distribución y administración. Un servicio comercial también puede estar basado en software libre.

Ejemplos para investigar, sin convertir la lista en un ranking: SQLite, PostgreSQL, MariaDB, MySQL, SQL Server, Oracle Database, MongoDB, Redis, Apache Cassandra y Neo4j.

**SQLite** es un ejemplo de motor embebido: la aplicación accede a una base de datos almacenada normalmente en un fichero, sin un servidor independiente. Esto demuestra que «fichero» y «base de datos» no siempre son alternativas excluyentes. Puede utilizar archivos auxiliares durante su funcionamiento. [Descripción oficial de SQLite](https://www.sqlite.org/about.html).

> **Ejemplo de clasificación:** un gestor relacional instalado como servidor y mantenido por la propia empresa sería relacional, cliente-servidor y autoadministrado. Si varios empleados lo utilizan, también debemos considerar sus capacidades multiusuario.

No todas las prestaciones se ofrecen del mismo modo: SQLite no incorpora el sistema de cuentas y roles propio de un gestor servidor. La aplicación y los permisos del archivo participan en el control del acceso.

<a id="apartado-6"></a>
## 6. Ubicación y distribución de la información

Una base **local** se encuentra en el equipo que la utiliza. Puede funcionar sin depender de una conexión a un servidor remoto. Cuando varias aplicaciones comparten datos, conviene distinguir cómo se distribuye su gestión.

### 6.1. Centralizada

La base de datos lógica se gestiona principalmente desde un único sistema o emplazamiento. Simplifica la administración y la consistencia, pero puede crear un punto de fallo o un cuello de botella si no se aplican medidas de disponibilidad.

> **Ejemplo:** veinte equipos de una oficina consultan un mismo servidor de bases de datos. Hay varios clientes, pero la gestión principal sigue centralizada.

### 6.2. Distribuida

Una base de datos distribuida almacena y procesa datos en varios nodos conectados, pero procura ofrecer una visión coordinada del conjunto.

Puede mejorar:

- disponibilidad y tolerancia a fallos;
- proximidad de los datos a sus usuarios;
- escalabilidad horizontal;
- reparto de carga.

También aumenta la complejidad de coordinación, consistencia, latencia, observabilidad, seguridad y recuperación.

| Situación | Posible utilidad de la distribución | Dificultad asociada |
|---|---|---|
| Usuarios en distintas regiones | Acercar datos a quienes los utilizan | Sincronizar cambios |
| Crecimiento de la carga | Repartir trabajo entre nodos | Evitar nodos sobrecargados |
| Fallo de un equipo | Mantener ciertas operaciones en otros nodos | Configurar redundancia y recuperación |
| Informe de todas las sedes | Consultar el conjunto de la organización | Reunir información por la red |

La mejora no es automática: depende del diseño. Una desconexión entre nodos puede impedir ciertas operaciones. Tener varios archivos independientes en distintas sedes no basta para hablar de una base distribuida coordinada.

### 6.3. Replicación y fragmentación

No son lo mismo:

- **Replicación:** conserva copias de los mismos datos en varios nodos.
- **Fragmentación o particionamiento:** divide los datos en partes. La asignación determina en qué nodo se almacena cada parte; también puede haber particiones dentro de un mismo sistema.

> **Ejemplo de replicación:** dos nodos conservan copias del catálogo de productos. Si la sincronización no es inmediata, uno puede mostrar temporalmente un precio anterior. Una réplica no sustituye una copia de seguridad: un borrado accidental también puede replicarse.

Tipos de fragmentación:

- **Horizontal:** reparte filas. Ejemplo: pedidos por región.
- **Vertical:** reparte columnas, manteniendo una clave que permita recomponer la información.
- **Mixta:** combina fragmentación horizontal y vertical.

Una fragmentación correcta debe permitir reconstruir la información necesaria, evitar pérdidas y escoger fragmentos útiles para las consultas. La clave de partición debe elegirse según los patrones de acceso; una mala elección produce nodos sobrecargados y consultas costosas.

**Ejemplo de fragmentación horizontal:**

| id_pedido | sede | importe |
|---|---|---|
| 101 | Norte | 50 |
| 102 | Sur | 30 |
| 103 | Norte | 80 |
| 104 | Sur | 20 |

Al dividir por sede, un fragmento contiene los pedidos 101 y 103; el otro, 102 y 104. Para recuperar el conjunto se reúnen las filas de ambos.

**Ejemplo de fragmentación vertical:**

| Datos originales | Fragmento A | Fragmento B |
|---|---|---|
| `id_cliente`, `nombre`, `email` | `id_cliente`, `nombre` | `id_cliente`, `email` |

El identificador compartido permite volver a asociar el nombre y el correo de cada cliente. Separar columnas no garantiza por sí solo la confidencialidad.

**Ejemplo de fragmentación mixta:** primero se dividen los clientes por sede y después se separan sus datos de contacto del resto de campos, conservando el identificador para reconstruir los registros.

### Condiciones y políticas de fragmentación

- **Completitud:** todos los datos originales deben quedar representados.
- **Reconstrucción:** debe ser posible recuperar la información original a partir de los fragmentos.
- **Disyunción cuando corresponda:** una división horizontal disjunta evita que una misma fila se asigne a varios fragmentos. En la vertical se repite intencionadamente el identificador necesario para reconstruir.

La política debe adaptarse al uso. Dividir pedidos por fecha facilita consultas por periodos, pero puede concentrar las escrituras en el fragmento más reciente. Dividir por sede favorece consultas locales, aunque una sede grande puede acumular casi todo el trabajo.

### 6.4. Nube y servicios gestionados

«En la nube» describe una forma de desplegar y operar recursos, no un modelo de datos. Un servicio gestionado puede ocuparse de actualizaciones, copias, supervisión y parte de la alta disponibilidad. La organización sigue siendo responsable de configurar accesos, clasificación de datos, retención, costes y cumplimiento normativo.

> **Ejemplo:** una empresa puede contratar una base relacional gestionada en la nube. Sigue siendo relacional y su distribución dependerá de la arquitectura contratada. «En la nube» no significa necesariamente «distribuida».

<a id="apartado-7"></a>
## 7. Cargas transaccionales y analíticas

- **OLTP:** muchas operaciones breves sobre datos actuales; por ejemplo, registrar pedidos o pagos.
- **OLAP:** consultas complejas y agregaciones sobre grandes conjuntos históricos para apoyar decisiones.

| Aspecto | OLTP | OLAP |
|---|---|---|
| Finalidad | Gestionar la actividad cotidiana | Analizar tendencias y apoyar decisiones |
| Operación típica | Registrar una venta | Comparar ventas anuales por región |
| Datos consultados | Los necesarios para una operación concreta | Conjuntos amplios e históricos |

Una aplicación pequeña puede realizar ambas clases de operaciones sobre una misma base de datos. La separación de sistemas se plantea cuando las necesidades lo justifican.

Un **almacén de datos** integra información histórica, normalmente procedente de varias fuentes, para análisis. Un **lago de datos** conserva grandes volúmenes de datos en formatos diversos. Estas arquitecturas no sustituyen automáticamente a la base de datos operacional: responden a necesidades diferentes.

<a id="apartado-8"></a>
## 8. Big Data, analítica e inteligencia de negocio

**Big Data** describe escenarios en los que el volumen, la velocidad, la variedad u otras características de los datos exigen técnicas de almacenamiento y procesamiento distintas de las habituales. No significa simplemente «muchos datos» ni obliga a usar una tecnología concreta.

| Característica | Qué representa | Ejemplo |
|---|---|---|
| Volumen | Cantidad de datos | Historial masivo de eventos |
| Velocidad | Ritmo de llegada y procesamiento | Lecturas continuas de sensores |
| Variedad | Diversidad de formatos | Texto, imágenes y registros |

También se suelen mencionar la **veracidad**, relacionada con la fiabilidad, y el **valor**, referido a la utilidad obtenida. No existe una cifra universal que determine por sí sola si un conjunto pertenece a Big Data.

La **analítica de datos** utiliza datos para describir, explicar, predecir o recomendar. La **inteligencia de negocio (BI)** reúne procesos y herramientas que convierten datos en indicadores, informes y cuadros de mando para apoyar decisiones.

Flujo simplificado:

1. fuentes operacionales;
2. extracción o ingestión;
3. limpieza y transformación;
4. almacenamiento analítico;
5. modelo semántico e indicadores;
6. informes, visualizaciones y decisiones.

> **Ejemplo de BI:** una tienda reúne sus ventas mensuales, corrige registros duplicados y calcula qué productos se venden más. Con ese informe decide qué existencias reponer.

Hacer BI no exige tener Big Data: una pequeña empresa puede obtener indicadores útiles a partir de unos pocos miles de registros. Del mismo modo, almacenar enormes cantidades de datos no garantiza convertirlos en conocimiento útil.

La calidad, el contexto, la trazabilidad y la protección de los datos son tan importantes como la herramienta utilizada. Un producto con pocas ventas puede tener poca demanda, pero también puede haber estado agotado. El indicador necesita contexto.

<a id="apartado-9"></a>
## 9. Protección de datos desde el diseño

En España, el marco general incluye el **Reglamento (UE) 2016/679 (RGPD)** y la **Ley Orgánica 3/2018 (LOPDGDD)**. Que un dato pueda almacenarse técnicamente no significa que pueda tratarse legalmente. [RGPD](https://eur-lex.europa.eu/eli/reg/2016/679/oj?locale=es), [LOPDGDD](https://www.boe.es/buscar/act.php?id=BOE-A-2018-16673).

Un **dato personal** permite identificar a una persona directa o indirectamente. El RGPD establece el marco europeo y la LOPDGDD adapta el ordenamiento español y garantiza derechos digitales. La **AEPD** es la Agencia Española de Protección de Datos, una autoridad de control, no una ley.

Al diseñar una solución deben plantearse, como mínimo:

- qué datos personales se recogen y con qué finalidad;
- cuál es la base jurídica del tratamiento;
- quién puede acceder y con qué permisos;
- cuánto tiempo deben conservarse;
- cómo se garantizan exactitud, confidencialidad, integridad y disponibilidad;
- cómo se atenderán los derechos de las personas;
- qué ocurrirá ante una brecha de seguridad.

Principios especialmente relacionados con bases de datos:

- licitud, lealtad y transparencia;
- limitación de la finalidad;
- minimización de datos;
- exactitud;
- limitación del plazo de conservación;
- integridad y confidencialidad;
- responsabilidad proactiva.

**Seudonimizar** sustituye identificadores por referencias separadas, pero los datos siguen siendo personales si es posible volver a asociarlos. Para considerar datos **anónimos**, la persona no debe resultar identificable teniendo en cuenta los medios razonablemente utilizables; borrar el nombre no suele ser suficiente.

> **Ejemplo:** cambiar el nombre de una persona por `cliente_27` no anonimiza el historial si conservamos una tabla que relaciona ese código con su identidad.

Entre los derechos se encuentran acceso, rectificación, supresión, oposición, limitación del tratamiento y portabilidad, con los requisitos y límites aplicables a cada caso. Corregir un correo erróneo es un ejemplo de rectificación. [AEPD: ejercicio de derechos](https://www.aepd.es/derechos-y-deberes/ejerce-tus-derechos).

En prácticas educativas se usarán datos ficticios o conjuntos expresamente autorizados. No se copiarán datos reales de alumnado, clientes o terceras personas a repositorios públicos.

<a id="apartado-10"></a>
## 10. Cómo elegir una solución

Antes de nombrar un producto, se deben analizar:

1. estructura y relaciones de los datos;
2. consultas y operaciones dominantes;
3. volumen, crecimiento y retención;
4. concurrencia, latencia y disponibilidad;
5. consistencia y transacciones necesarias;
6. distribución geográfica;
7. seguridad y obligaciones legales;
8. competencias del equipo y ecosistema;
9. costes de implantación, operación y salida;
10. copias, recuperación, migración y observabilidad.

La respuesta profesional no es «usar siempre SQL» ni «usar siempre NoSQL», sino justificar la alternativa que satisface mejor los requisitos y explicar sus compromisos.

> **Ejemplo de elección:** para una aplicación local con datos relacionados, un gestor embebido puede simplificar el despliegue. Si muchos usuarios necesitan modificar información compartida, habrá que valorar una arquitectura de servidor y su control de concurrencia. Para intercambiar un listado con otra aplicación, un CSV puede seguir siendo adecuado.

<a id="apartado-11"></a>
## 11. Resumen

- Los ficheros siguen siendo útiles, aunque gestionar datos relacionados y concurrentes directamente sobre ellos puede resultar complejo.
- Un SGBD aporta estructura, consultas, integridad, concurrencia, seguridad y recuperación.
- Las bases de datos se clasifican por modelo, ubicación, despliegue y tipo de carga.
- Distribuir datos puede aportar disponibilidad y escalabilidad, a cambio de mayor complejidad.
- Replicación y fragmentación resuelven problemas diferentes.
- Big Data, analítica y BI están relacionados, pero no son sinónimos.
- La protección de datos debe incorporarse desde el diseño.


## Fuentes de consulta

- [SQLite: descripción del motor y su funcionamiento embebido](https://www.sqlite.org/about.html).
- [MongoDB: organización de documentos](https://www.mongodb.com/docs/manual/data-modeling/).
- [Unión Europea: Reglamento General de Protección de Datos](https://eur-lex.europa.eu/eli/reg/2016/679/oj?locale=es).
- [BOE: Ley Orgánica 3/2018](https://www.boe.es/buscar/act.php?id=BOE-A-2018-16673).
- [AEPD: ejercicio de derechos](https://www.aepd.es/derechos-y-deberes/ejerce-tus-derechos).
