# Tema 1. Almacenamiento de la información

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

## 2. Ficheros como sistema de almacenamiento

Un **fichero** es una secuencia de bytes identificada normalmente por un nombre y gestionada por un sistema de archivos. Su interpretación depende del formato y del programa que lo procesa.

### 2.1. Formatos habituales

- **Texto plano:** contenido legible como texto, sin una estructura obligatoria.
- **CSV:** datos tabulares sencillos separados por delimitadores.
- **JSON:** documentos con objetos, arrays y valores; común en API y configuración.
- **XML:** documentos jerárquicos con etiquetas y esquemas opcionales.
- **Binario:** formato específico de una aplicación o estándar, normalmente más compacto o eficiente.

La extensión orienta, pero no garantiza el formato real del contenido.

### 2.2. Formas de acceso

- **Secuencial:** se recorren los datos en orden. Es sencillo y resulta adecuado para procesamiento por lotes.
- **Directo o aleatorio:** se accede a una posición sin leer todo lo anterior.
- **Indexado:** una estructura auxiliar relaciona una clave con la ubicación del dato y acelera las búsquedas.

### 2.3. Operaciones básicas

Sobre un fichero se realizan operaciones como crear, abrir, leer, escribir, anexar, renombrar, copiar, cerrar y eliminar. Sobre los datos pueden realizarse altas, consultas, modificaciones y bajas.

### 2.4. Ventajas y limitaciones

Los ficheros son apropiados para configuración, intercambio de datos, registros de eventos, documentos o conjuntos pequeños con reglas sencillas. No deben presentarse como una tecnología superada: siguen siendo una pieza básica de los sistemas actuales.

Cuando cada aplicación mantiene sus propios ficheros aparecen problemas frecuentes:

- duplicación e inconsistencia de datos;
- fuerte dependencia entre formato y código;
- validaciones repetidas en distintas aplicaciones;
- dificultad para consultar información relacionada;
- control de concurrencia y permisos implementado de forma artesanal;
- copias de seguridad y recuperación poco coordinadas.

## 3. Bases de datos y SGBD

Una **base de datos** es una colección organizada de datos relacionados, diseñada para ser almacenada, consultada y actualizada. Un **sistema gestor de bases de datos (SGBD)** es el software que permite definir, manipular, proteger y administrar esos datos.

Conviene no confundir:

- **base de datos:** los datos, su estructura y sus restricciones;
- **SGBD:** el software que los gestiona;
- **aplicación:** el programa que utiliza el SGBD para resolver una necesidad del usuario.

### 3.1. Qué aporta un SGBD

- definición de estructuras, tipos y restricciones;
- consultas y modificación de datos;
- control de concurrencia y transacciones;
- autenticación, roles y permisos;
- catálogo o metadatos;
- registro de actividad y diagnóstico;
- copias de seguridad y recuperación;
- mecanismos de replicación, alta disponibilidad y optimización.

El SGBD reduce trabajo en las aplicaciones, pero añade administración, consumo de recursos y complejidad. Para un fichero de configuración pequeño, introducir un servidor de bases de datos puede ser innecesario.

### 3.2. Componentes conceptuales

- **Motor de almacenamiento:** organiza páginas, ficheros, índices y acceso físico.
- **Procesador de consultas:** interpreta y planifica las operaciones solicitadas.
- **Gestor de transacciones:** coordina concurrencia, confirmación y recuperación.
- **Catálogo de datos:** conserva metadatos sobre objetos, restricciones y usuarios.
- **Subsistema de seguridad:** aplica autenticación, autorización y auditoría.
- **Herramientas de administración:** permiten supervisar, copiar, restaurar y configurar.

El **administrador de bases de datos (DBA)** es un rol humano, no un componente de software. Define políticas, controla permisos, vigila el rendimiento y planifica la continuidad del servicio.

### 3.3. Niveles de abstracción

La arquitectura de tres esquemas distingue:

1. **Nivel interno:** cómo se almacenan físicamente los datos.
2. **Nivel conceptual:** estructura lógica global, relaciones y restricciones.
3. **Nivel externo:** vistas específicas para usuarios o aplicaciones.

La **independencia física** permite cambiar aspectos del almacenamiento sin rediseñar el esquema lógico. La **independencia lógica** permite modificar el esquema conceptual reduciendo el impacto sobre determinadas vistas y aplicaciones. Ninguna de las dos implica que todo cambio sea automáticamente compatible.

## 4. Clasificación por modelo de datos

Un **modelo de datos** define conceptos para representar datos, relaciones, restricciones y operaciones.

| Modelo | Organización | Uso orientativo |
|---|---|---|
| Relacional | Tablas relacionadas, claves y restricciones | Gestión transaccional y consultas estructuradas |
| Documental | Documentos, normalmente similares a JSON | Datos semiestructurados y esquemas flexibles |
| Clave-valor | Pares clave-valor | Caché, sesiones y acceso muy rápido por clave |
| Columnas anchas | Filas con familias de columnas distribuidas | Grandes volúmenes y escrituras distribuidas |
| Grafos | Nodos, relaciones y propiedades | Redes, rutas, fraude y recomendaciones |
| Series temporales | Valores asociados al tiempo | Métricas, sensores y observabilidad |
| Vectorial | Vectores y búsqueda por similitud | Recuperación semántica y sistemas con IA |

Los modelos **jerárquico** y **en red** tienen importancia histórica y todavía existen en sistemas heredados. Para tomar decisiones actuales resulta más útil compararlos brevemente y dedicar mayor atención a los modelos utilizados en aplicaciones contemporáneas.

Un producto puede admitir varios modelos. Por eso, «modelo», «producto» y «arquitectura de despliegue» no son categorías equivalentes.

## 5. Clasificación de los SGBD

Los SGBD se pueden clasificar con varios criterios simultáneos:

- por modelo: relacional, documental, grafos, clave-valor, etc.;
- por despliegue: embebido, servidor, clúster o servicio gestionado;
- por ubicación: local, centralizado, distribuido o en la nube;
- por licencia y operación: software libre, propietario o servicio comercial;
- por carga: transaccional, analítica o híbrida.

Ejemplos para investigar, sin convertir la lista en un ranking: SQLite, PostgreSQL, MariaDB, MySQL, SQL Server, Oracle Database, MongoDB, Redis, Apache Cassandra y Neo4j.

**SQLite** es un ejemplo de motor embebido: la aplicación accede a una base de datos almacenada normalmente en un fichero, sin un servidor independiente. Esto demuestra que «fichero» y «base de datos» no siempre son alternativas excluyentes.

## 6. Ubicación y distribución de la información

### 6.1. Centralizada

La base de datos lógica se gestiona principalmente desde un único sistema o emplazamiento. Simplifica la administración y la consistencia, pero puede crear un punto de fallo o un cuello de botella si no se aplican medidas de disponibilidad.

### 6.2. Distribuida

Una base de datos distribuida almacena y procesa datos en varios nodos conectados, pero procura ofrecer una visión coordinada del conjunto.

Puede mejorar:

- disponibilidad y tolerancia a fallos;
- proximidad de los datos a sus usuarios;
- escalabilidad horizontal;
- reparto de carga.

También aumenta la complejidad de coordinación, consistencia, latencia, observabilidad, seguridad y recuperación.

### 6.3. Replicación y fragmentación

No son lo mismo:

- **Replicación:** conserva copias de los mismos datos en varios nodos.
- **Fragmentación o particionamiento:** divide los datos y ubica cada parte en uno o varios nodos.

Tipos de fragmentación:

- **Horizontal:** reparte filas. Ejemplo: pedidos por región.
- **Vertical:** reparte columnas, manteniendo una clave que permita recomponer la información.
- **Mixta:** combina fragmentación horizontal y vertical.

Una fragmentación correcta debe permitir reconstruir la información necesaria, evitar pérdidas y escoger fragmentos útiles para las consultas. La clave de partición debe elegirse según los patrones de acceso; una mala elección produce nodos sobrecargados y consultas costosas.

### 6.4. Nube y servicios gestionados

«En la nube» describe una forma de desplegar y operar recursos, no un modelo de datos. Un servicio gestionado puede ocuparse de actualizaciones, copias, supervisión y parte de la alta disponibilidad. La organización sigue siendo responsable de configurar accesos, clasificación de datos, retención, costes y cumplimiento normativo.

## 7. Cargas transaccionales y analíticas

- **OLTP:** muchas operaciones breves sobre datos actuales; por ejemplo, registrar pedidos o pagos.
- **OLAP:** consultas complejas y agregaciones sobre grandes conjuntos históricos para apoyar decisiones.

Un **almacén de datos** integra información histórica, normalmente procedente de varias fuentes, para análisis. Un **lago de datos** conserva grandes volúmenes de datos en formatos diversos. Estas arquitecturas no sustituyen automáticamente a la base de datos operacional: responden a necesidades diferentes.

## 8. Big Data, analítica e inteligencia de negocio

**Big Data** describe escenarios en los que el volumen, la velocidad, la variedad u otras características de los datos exigen técnicas de almacenamiento y procesamiento distintas de las habituales. No significa simplemente «muchos datos» ni obliga a usar una tecnología concreta.

La **analítica de datos** utiliza datos para describir, explicar, predecir o recomendar. La **inteligencia de negocio (BI)** reúne procesos y herramientas que convierten datos en indicadores, informes y cuadros de mando para apoyar decisiones.

Flujo simplificado:

1. fuentes operacionales;
2. extracción o ingestión;
3. limpieza y transformación;
4. almacenamiento analítico;
5. modelo semántico e indicadores;
6. informes, visualizaciones y decisiones.

La calidad, el contexto, la trazabilidad y la protección de los datos son tan importantes como la herramienta utilizada.

## 9. Protección de datos desde el diseño

En España, el marco general incluye el **Reglamento (UE) 2016/679 (RGPD)** y la **Ley Orgánica 3/2018 (LOPDGDD)**. Que un dato pueda almacenarse técnicamente no significa que pueda tratarse legalmente.

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

**Seudonimizar** sustituye identificadores por referencias separadas, pero los datos siguen siendo personales si es posible volver a asociarlos. **Anonimizar** pretende impedir de forma irreversible la identificación; borrar el nombre no suele ser suficiente.

En prácticas educativas se usarán datos ficticios o conjuntos expresamente autorizados. No se copiarán datos reales de alumnado, clientes o terceras personas a repositorios públicos.

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

## 11. Resumen

- Los ficheros siguen siendo útiles, aunque gestionar datos relacionados y concurrentes directamente sobre ellos puede resultar complejo.
- Un SGBD aporta estructura, consultas, integridad, concurrencia, seguridad y recuperación.
- Las bases de datos se clasifican por modelo, ubicación, despliegue y tipo de carga.
- Distribuir datos puede aportar disponibilidad y escalabilidad, a cambio de mayor complejidad.
- Replicación y fragmentación resuelven problemas diferentes.
- Big Data, analítica y BI están relacionados, pero no son sinónimos.
- La protección de datos debe incorporarse desde el diseño.

## 12. Preguntas de repaso

1. ¿Cuándo elegirías un fichero JSON en lugar de un SGBD?
2. ¿Qué diferencia hay entre una base de datos y un SGBD?
3. ¿Qué funciones debería dejar una aplicación en manos del SGBD?
4. ¿Por qué SQLite cuestiona una comparación rígida entre ficheros y bases de datos?
5. ¿Qué modelos serían razonables para pedidos, sesiones web, rutas y métricas?
6. ¿Qué diferencia hay entre distribución, replicación y fragmentación?
7. ¿Qué problema puede causar una mala clave de partición?
8. ¿Por qué un servicio en la nube no elimina la responsabilidad de la organización?
9. ¿En qué se diferencian OLTP, OLAP, Big Data y BI?
10. ¿Qué principios de protección de datos aplicarías a una base de datos escolar?