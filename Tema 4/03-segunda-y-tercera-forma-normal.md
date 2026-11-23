# 3. Segunda y tercera forma normal

Las formas normales se comprueban en orden:

```text
1FN → 2FN → 3FN
```

No tiene sentido afirmar que una relación está en 3FN si antes no cumple 1FN y 2FN.

## 3.1. Segunda forma normal (2FN)

Una relación está en **2FN** cuando:

1. está en 1FN;
2. todo atributo no primo depende completamente de cada clave candidata.

Un atributo es **primo** si forma parte de alguna clave candidata.

En el nivel de esta unidad aplicaremos la idea práctica: no debe haber atributos no clave que dependan solo de una parte de una clave compuesta.

> Si todas las claves candidatas son simples, no puede existir una dependencia parcial y la relación que está en 1FN también está en 2FN.

### Ejemplo: matrículas

```text
MATRICULA_INICIAL(
  id_alumno,
  id_curso,
  nombre_alumno,
  nombre_curso,
  fecha_matricula
)
```

Clave primaria: `(id_alumno, id_curso)`.

Dependencias:

```text
id_alumno → nombre_alumno
id_curso → nombre_curso
(id_alumno, id_curso) → fecha_matricula
```

`nombre_alumno` depende solo de `id_alumno` y `nombre_curso` solo de `id_curso`. Son dependencias parciales.

### Descomposición en 2FN

```text
ALUMNO(id_alumno PK, nombre_alumno)

CURSO(id_curso PK, nombre_curso)

MATRICULA(
  id_alumno PK FK → ALUMNO,
  id_curso  PK FK → CURSO,
  fecha_matricula
)
```

En `MATRICULA`, `fecha_matricula` depende de la clave compuesta completa.

## 3.2. Tercera forma normal (3FN)

Una relación está en **3FN** cuando:

1. está en 2FN;
2. ningún atributo no primo depende transitivamente de una clave candidata a través de otro atributo no primo.

### Ejemplo: alumnado y localidades

```text
ALUMNO_INICIAL(
  id_alumno,
  nombre_alumno,
  id_localidad,
  nombre_localidad,
  provincia
)
```

Dependencias:

```text
id_alumno → nombre_alumno, id_localidad
id_localidad → nombre_localidad, provincia
```

La clave es `id_alumno`, pero `nombre_localidad` y `provincia` no describen directamente al alumno: describen a la localidad.

Existe una cadena:

```text
id_alumno → id_localidad → nombre_localidad, provincia
```

### Descomposición en 3FN

```text
LOCALIDAD(
  id_localidad PK,
  nombre_localidad,
  provincia
)

ALUMNO(
  id_alumno PK,
  nombre_alumno,
  id_localidad FK → LOCALIDAD
)
```

Los datos de cada localidad se almacenan una sola vez.

## 3.3. La frase para recordar

De forma didáctica, los atributos no clave deben depender de:

> «la clave, toda la clave y nada más que la clave».

- **la clave**: la fila debe ser identificable;
- **toda la clave**: no debe haber dependencias parciales;
- **nada más que la clave**: no debe haber dependencias transitivas entre atributos no clave.

Esta frase ayuda a razonar, pero no sustituye a la definición formal cuando existen varias claves candidatas.

## 3.4. Comparación

| Forma normal | Problema principal | Pregunta útil |
| --- | --- | --- |
| 1FN | Listas o grupos repetitivos | ¿Cada celda contiene un solo valor? |
| 2FN | Dependencia parcial | ¿Algún atributo depende solo de parte de una clave compuesta? |
| 3FN | Dependencia transitiva | ¿Algún atributo no clave describe a otro atributo no clave? |

## 3.5. No descomponer por intuición

No debemos crear una tabla `PROFESOR` solo porque exista una columna con ese nombre. La separación se justifica si conocemos alguna regla como:

```text
id_profesor → nombre_profesor, departamento
```

Si solo guardamos el nombre del profesor como un texto asociado a un curso y no conocemos más dependencias, no podemos inventarlas.

## 3.6. Una buena descomposición

Además de alcanzar la forma normal buscada, una descomposición debe permitir:

- reconstruir la información original mediante combinaciones naturales, sin generar filas falsas;
- aplicar las reglas del negocio mediante claves y restricciones razonables;
- evitar que desaparezcan datos o significados relevantes.

En los ejercicios de esta unidad justificaremos cada tabla indicando la dependencia que la origina.
