# Jujutsu Kaisen: la academia de hechicería

## Objetivo

Diseñar un modelo E/R extendido utilizando la notación de Chen a partir del siguiente enunciado. Las reglas de este ejercicio son ficticias y están adaptadas para la actividad.

## Enunciado

La academia de hechicería necesita una base de datos para registrar a sus integrantes y organizar sus misiones.

De cada integrante se guarda un código único, su nombre y su fecha de incorporación. Un integrante puede ser estudiante, docente o no desempeñar todavía ninguno de esos dos papeles. También es posible que una misma persona sea estudiante y docente a la vez.

De los estudiantes se registra su curso actual y su nivel de energía maldita. De los docentes se guarda su especialidad y sus años de experiencia. Los estudiantes y los docentes conservan el código que tienen como integrantes de la academia.

La academia organiza misiones, de las que almacena un código único, una descripción, una fecha y un nivel de peligro.

Cada misión tiene exactamente un docente responsable. Un docente puede encargarse de varias misiones o no tener ninguna asignada.

En cada misión participa al menos un estudiante. Un estudiante puede participar en varias misiones o no haber participado todavía en ninguna. De la participación de cada estudiante en cada misión se registra el papel asignado y una evaluación, que puede estar pendiente. Un estudiante participa una sola vez en una misma misión.

Solo pueden participar estudiantes cuyo nivel de energía maldita sea igual o superior al nivel de peligro de la misión. Ambos niveles se expresan mediante números enteros del 1 al 5.

## Trabajo que debes realizar

Antes de comenzar, marca en el enunciado las posibles entidades y relaciones. Después, desarrolla el modelo siguiendo los pasos trabajados en clase:

1. **Proponer las frases que describan el problema.** Divide el enunciado en reglas sencillas.
2. **Generar los modelos de cada frase.** Dibuja los modelos parciales y distingue las relaciones ordinarias de las conexiones de herencia.
3. **Realizar un estudio de cardinalidad.** Justifica los mínimos y máximos de las relaciones. Determina también las restricciones de completitud y solapamiento de la especialización.
4. **Generar el modelo completo E/R.** Integra los modelos parciales utilizando la notación de Chen extendida.
5. **Colocar los atributos a cada entidad e interrelación y señalar los identificadores.** Subraya los identificadores y ten en cuenta la herencia.
6. **Revisar el modelo y documentar las restricciones adicionales.** Comprueba que el diagrama cumple el enunciado y escribe las reglas que no queden expresadas completamente en él.

## Criterios de representación

- Utiliza rectángulos para las entidades, rombos para las relaciones y óvalos para los atributos.
- Representa la especialización con la convención trabajada en clase e indica su completitud y solapamiento.
- Utiliza líneas simples en las relaciones ordinarias e indica la participación obligatoria mediante las cardinalidades.
- Coloca las cardinalidades siguiendo el criterio de clase: junto a una entidad se indica cuántas ocurrencias de ella corresponden a una de la entidad situada al otro lado de la relación.
- Justifica tus decisiones. Si necesitas hacer una suposición, escríbela expresamente.
