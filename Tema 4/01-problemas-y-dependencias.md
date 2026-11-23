# 1. Problemas de diseño y dependencias funcionales

## 1.1. ¿Qué es normalizar?

Normalizar es revisar la estructura de una base de datos relacional para que cada hecho se almacene en el lugar adecuado y, siempre que sea posible, una sola vez.

Normalizar persigue tres objetivos:

1. reducir la repetición innecesaria de datos;
2. evitar incoherencias durante las operaciones de inserción, actualización y borrado;
3. facilitar el mantenimiento y la evolución de la base de datos.

Una base de datos normalizada no tiene por qué poseer el mayor número posible de tablas. Tiene que tener las tablas que se justifican mediante las reglas del negocio.

## 1.2. Redundancia

Existe redundancia cuando el mismo hecho se almacena repetidamente.

Considera esta relación:

### `MATRICULA_INICIAL`

| id_alumno | nombre_alumno | id_curso | nombre_curso | aula |
| ---: | --- | ---: | --- | --- |
| 1 | Ana | 10 | Bases de Datos | A12 |
| 1 | Ana | 20 | Programación | B05 |
| 2 | Luis | 10 | Bases de Datos | A12 |

El nombre de Ana se repite por cada curso en el que está matriculada. También se repiten el nombre y el aula de cada curso.

La repetición consume espacio, pero ese no es su principal problema. El verdadero riesgo es que las copias de un mismo hecho terminen conteniendo valores diferentes.

## 1.3. Anomalías

### Anomalía de actualización

Si el curso `10` cambia al aula `A14`, hay que modificar todas sus filas. Si una queda sin actualizar, la base de datos afirmará que el mismo curso se imparte en dos aulas.

### Anomalía de inserción

No podemos registrar un curso nuevo mientras no tenga alumnado matriculado si la clave de la tabla necesita tanto `id_alumno` como `id_curso`.

### Anomalía de borrado

Si eliminamos la última matrícula del curso `20`, también desaparecen su nombre y su aula. Se pierde información del curso al borrar una matrícula.

> Las anomalías son síntomas de que una tabla mezcla hechos de naturaleza diferente.

## 1.4. Dependencias funcionales

Una **dependencia funcional** expresa que el valor de uno o varios atributos determina un único valor de otro atributo.

Se representa así:

```text
X → Y
```

Se lee: «X determina funcionalmente a Y».

Por ejemplo:

```text
id_alumno → nombre_alumno
id_curso → nombre_curso, aula
(id_alumno, id_curso) → fecha_matricula
```

Si dos filas tienen el mismo `id_alumno`, deben tener el mismo `nombre_alumno`. Sin embargo, el nombre no tiene por qué determinar al identificador: puede haber varias personas llamadas Ana.

### El determinante

El atributo o conjunto de atributos situado a la izquierda de la flecha se denomina **determinante**.

En `id_curso → aula`, el determinante es `id_curso`.

### Las dependencias proceden del negocio

No basta con observar una muestra de datos. Que en cinco filas cada docente aparezca en una sola asignatura no demuestra que siempre vaya a ser así.

Antes de escribir una dependencia, debemos formular la regla:

> «En este centro, ¿cada docente puede impartir una sola asignatura o varias?»

La respuesta cambia el diseño.

## 1.5. Claves y dependencias

Una **superclave** es un conjunto de atributos que identifica de forma única cada fila.

Una **clave candidata** es una superclave mínima: si eliminamos uno de sus atributos, deja de identificar la fila.

Entre las claves candidatas se elige una **clave primaria**. Las demás continúan siendo claves alternativas y también deben respetarse mediante restricciones `UNIQUE` cuando corresponda.

### Ejemplo

En una matrícula, una posible clave es:

```text
(id_alumno, id_curso)
```

Esto supone la regla «un alumno solo puede matricularse una vez en un curso». Si pudiera repetir el mismo curso en años diferentes, la clave tendría que incluir, por ejemplo, `curso_academico`.

## 1.6. Tipos de dependencia

### Dependencia completa

Un atributo depende de **toda** una clave compuesta y no de una parte.

```text
(id_factura, cod_producto) → cantidad
```

La cantidad pertenece a una línea concreta: no la determina la factura por sí sola ni el producto por sí solo.

### Dependencia parcial

Un atributo depende solo de una parte de una clave compuesta.

```text
(id_alumno, id_curso)  clave
id_alumno → nombre_alumno
```

`nombre_alumno` depende únicamente de `id_alumno`. Esta dependencia provoca un problema de segunda forma normal.

### Dependencia transitiva

Un atributo no clave depende de otro atributo no clave, que a su vez depende de la clave.

```text
id_alumno → id_localidad
id_localidad → nombre_localidad
```

Por tanto, `nombre_localidad` depende indirectamente de `id_alumno`. Esta situación puede provocar un problema de tercera forma normal.

## 1.7. Antes de normalizar

Para cada relación:

1. identifica qué representa una fila;
2. escribe las reglas del negocio;
3. determina las claves candidatas;
4. anota las dependencias funcionales;
5. comprueba las formas normales en orden.

> Si las dependencias están mal, la descomposición también lo estará.
