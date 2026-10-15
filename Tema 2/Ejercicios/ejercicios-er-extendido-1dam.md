# Ejercicios de modelo E/R extendido

Para cada ejercicio, sigue los siete pasos indicados y documenta el proceso en el `README.md` del repositorio del módulo. Crea los diagramas en **draw.io (diagrams.net)**, guarda y añade al README las imágenes exportadas de cada fase.

## Pasos que debes seguir en cada ejercicio

Sigue estos pasos en el orden indicado y documenta todo el proceso en el `README.md` de tu repositorio del módulo:

1. **Marcar en el enunciado las posibles entidades y las relaciones.**
2. **Proponer las frases que describan el problema.**
3. **Generar los modelos de cada frase.** Crea en draw.io los diagramas individuales que representen las frases del problema.
4. **Identificación de entidades y analizar su fortaleza.** Indica qué entidades son fuertes y cuáles, si las hubiera, son débiles. Justifica cómo se identifican.
5. **Realizar un estudio de cardinalidad.** Analiza y justifica los mínimos y máximos de cada relación en ambos sentidos.
6. **Generar el modelo completo E/R.** Integra las entidades y relaciones de los modelos parciales.
7. **Colocar los atributos a cada entidad e interrelación.** Añade los atributos, subraya los identificadores y aplica las convenciones de E/R extendido necesarias: especializaciones, completitud, solapamiento y agregación cuando corresponda.

En los diagramas usa la notación de Chen trabajada en clase: rectángulos para entidades, rombos para relaciones y óvalos para atributos. Representa las especializaciones con un círculo, indicando `d` o `o` y `total` o `parcial`. En las relaciones ordinarias, coloca las cardinalidades como pares **(mínimo,máximo)** junto a los extremos correctos.
---

# Ejercicio 1. Star Wars: flota de la Alianza Rebelde

## Enunciado

La Alianza Rebelde necesita organizar la información de los vehículos de su flota, las bases donde se encuentran y las misiones para las que están asignados.

De cada **vehículo** se guarda un código único, un nombre de identificación, el año de fabricación y su estado operativo. Todo vehículo de la flota es **terrestre** o **aéreo**. Ningún vehículo pertenece a ambos tipos y no hay vehículos que queden fuera de esas dos categorías.

Los vehículos terrestres pueden ser **deslizadores** o **caminantes**. Un vehículo terrestre puede no pertenecer a ninguno de esos dos tipos, por ejemplo, si es un transporte terrestre de otra clase. Si pertenece a uno, no puede pertenecer al otro. De los deslizadores interesa la velocidad máxima y el número de ocupantes; de los caminantes, el número de patas y la capacidad de transporte. De los vehículos aéreos se registra la altitud máxima y el tipo de propulsión. El código del vehículo identifica al mismo vehículo con independencia de su clasificación.

De cada **base** se guarda un código único, un nombre y el planeta donde está situada. Todo vehículo de la flota tiene asignada una única base. Una base puede no tener vehículos o tener varios.

De cada **misión** se registra un código único, un nombre, un objetivo y una fecha de inicio. Una misión debe contar con uno o varios vehículos, mientras que un vehículo puede no estar asignado a ninguna misión o estar asignado como máximo a una misión activa. Para cada vehículo asignado se guarda la función que desempeña dentro de esa misión. El sistema representa la planificación actual y no conserva el historial de asignaciones ya finalizadas.

No es necesario registrar pilotos, pasajeros, armamento ni daños sufridos durante las misiones.

## Aplicación de los pasos

Al construir los modelos parciales y el modelo completo, presta atención a los dos niveles de especialización y a sus restricciones independientes. Analiza la fortaleza de BASE, MISIÓN y VEHÍCULO, además de cualquier otra entidad que identifiques. Incluye en el README las decisiones de cardinalidad y las reglas que no queden expresadas por completo mediante el diagrama.

Comprueba durante la revisión si el modelo permite representar:

- un vehículo que no sea ni aéreo ni terrestre;
- un vehículo terrestre que no sea deslizador ni caminante;
- un vehículo que sea a la vez deslizador y caminante;
- una base sin vehículos;
- una misión sin vehículos asignados;
- un vehículo asignado simultáneamente a dos misiones activas.
---

# Ejercicio 2. Stranger Things: investigaciones en Hawkins

## Enunciado

El grupo de investigación de Hawkins quiere organizar las operaciones que realiza para investigar sucesos extraños. Para simplificar, el sistema registra agentes, investigaciones y la asignación de cada agente a una investigación.

De cada **agente** se guarda un código único, nombre, fecha de incorporación y especialidad. De cada **investigación** se registra un código único, nombre en clave, descripción, fecha de inicio y estado.

Un agente puede no estar asignado a ninguna investigación o participar en varias. Toda investigación debe contar con al menos dos agentes. El grupo registra qué agentes participan en cada investigación. De cada participación concreta se guarda la función del agente en esa investigación, el turno asignado y la fecha de incorporación al equipo. En este sistema se registra únicamente la composición actual de cada equipo; no se conserva el historial de participaciones anteriores.

Cada participación concreta debe estar supervisada por exactamente un agente. Un agente puede supervisar ninguna, una o muchas participaciones. La supervisión corresponde a la participación de un agente en una investigación concreta: no basta con registrar que alguien supervisa al agente en general ni que supervisa toda la investigación. Un agente no puede supervisar su propia participación en esa misma investigación.

Representa la supervisión mediante **agregación**: el conjunto formado por AGENTE, la relación de participación y INVESTIGACIÓN puede intervenir en la relación SUPERVISA. El contorno de agregación no es una entidad nueva.

No es necesario guardar pruebas recogidas, ubicaciones visitadas, criaturas, parentescos ni cronologías de los sucesos.

## Aplicación de los pasos

Al construir los modelos parciales y el modelo completo, presta especial atención a los atributos de la participación y a la agregación. El contorno debe incluir AGENTE, el rombo de participación e INVESTIGACIÓN; SUPERVISA conecta el agente supervisor con esa unidad agregada. Analiza la fortaleza de las entidades identificadas e incluye en el README las cardinalidades y las reglas que no queden expresadas por completo mediante el diagrama.

Comprueba durante la revisión si el modelo permite representar:

- un agente que todavía no participa en investigaciones;
- una investigación con un solo agente;
- una investigación con varios agentes;
- una participación sin supervisor;
- un agente que supervisa varias participaciones;
- un agente que intenta supervisar su propia participación.
---

# Ejercicio 3. Jujutsu Kaisen: academia de hechicería

## Enunciado

La academia de hechicería necesita una base de datos para registrar a sus integrantes y organizar sus misiones. Las reglas de este ejercicio son ficticias y están adaptadas para la actividad.

De cada **integrante** se guarda un código único, su nombre y su fecha de incorporación. Un integrante puede ser **estudiante**, **docente** o no desempeñar todavía ninguno de esos dos papeles. También es posible que una misma persona sea estudiante y docente a la vez.

De los estudiantes se registra su curso actual y su nivel de energía maldita. De los docentes se guarda su especialidad y sus años de experiencia. Los estudiantes y los docentes conservan el código que tienen como integrantes de la academia.

La academia organiza **misiones**, de las que almacena un código único, una descripción, una fecha y un nivel de peligro. Cada misión tiene exactamente un docente responsable. Un docente puede encargarse de varias misiones o no tener ninguna asignada.

En cada misión participa al menos un estudiante. Un estudiante puede participar en varias misiones o no haber participado todavía en ninguna. De la participación de cada estudiante en cada misión se registra el papel asignado y una evaluación, que puede estar pendiente. Un estudiante participa una sola vez en una misma misión.

Solo pueden participar estudiantes cuyo nivel de energía maldita sea igual o superior al nivel de peligro de la misión. Ambos niveles se expresan mediante números enteros del 1 al 5.

## Aplicación de los pasos

Al construir los modelos parciales y el modelo completo, presta especial atención a la especialización de INTEGRANTE y a los atributos de la participación entre ESTUDIANTE y MISIÓN. Analiza la fortaleza de las entidades identificadas e incluye en el README las cardinalidades y las reglas que no queden expresadas por completo mediante el diagrama, incluida la comparación entre energía maldita y nivel de peligro.

Comprueba durante la revisión si el modelo permite representar:

- un integrante que todavía no es estudiante ni docente;
- una persona que desempeña ambos papeles;
- una misión sin docente responsable;
- una misión sin estudiantes;
- dos participaciones del mismo estudiante en la misma misión;
- una participación en una misión cuyo nivel de peligro supera la energía maldita del estudiante.
---

## Entrega en el repositorio

Realiza todo el trabajo en un archivo **`README.md`** dentro del repositorio del módulo. Organízalo en apartados, siguiendo los siete pasos en el orden indicado para cada ejercicio.

En el README incluye:

- El análisis y las frases que describen el problema.
- Las decisiones sobre la fortaleza de las entidades.
- El estudio de cardinalidades con su justificación.
- Las capturas o exportaciones en PNG de los **diagramas individuales** generados para las frases.
- La imagen del **modelo completo E/R** integrado.
- La imagen del **diagrama final**, con atributos, identificadores y las convenciones de E/R extendido que correspondan.
- Las explicaciones necesarias para entender cada imagen y las reglas que el diagrama no exprese por sí solo.

Guarda también las imágenes utilizadas en el README. Inserta las imágenes en el documento mediante rutas relativas para que se visualicen al abrir el repositorio. Repite esta estructura para los tres ejercicios: Star Wars, Stranger Things y Jujutsu Kaisen.
