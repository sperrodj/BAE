# Unidad 4. Normalización 🧹

> Diseñar una base de datos no consiste solo en crear tablas: consiste en decidir **qué dato pertenece a cada tabla**.

La normalización es un proceso de revisión del modelo relacional que ayuda a reducir datos repetidos y a evitar incoherencias al insertar, modificar o eliminar información.

## ¿Qué aprenderemos?

Al finalizar la unidad serás capaz de:

- detectar redundancias y anomalías en una tabla;
- expresar reglas del negocio mediante dependencias funcionales;
- distinguir dependencias completas, parciales y transitivas;
- comprobar si una relación cumple la primera, segunda y tercera forma normal;
- descomponer una relación conservando correctamente la información;
- justificar las decisiones tomadas durante la normalización;
- reconocer cuándo puede ser útil la forma normal de Boyce-Codd;
- comprender, como ampliación, qué problemas resuelven la cuarta y la quinta forma normal.

## Itinerario de la unidad

| Archivo | Contenido |
| --- | --- |
| [01. Problemas y dependencias](01-problemas-y-dependencias.md) | Redundancia, anomalías, claves y dependencias funcionales |
| [02. Primera forma normal](02-primera-forma-normal.md) | Atomicidad y grupos repetitivos |
| [03. Segunda y tercera forma normal](03-segunda-y-tercera-forma-normal.md) | Dependencias parciales y transitivas |
| [04. Ejemplo completo](04-ejemplo-completo-facturas.md) | Normalización guiada de una factura hasta 3FN |
| [05. BCNF y formas avanzadas](05-bcnf-y-formas-avanzadas.md) | BCNF y aproximación a 4FN y 5FN |
| [06. Actividades](06-actividades.md) | Ejercicios graduados y retos |
| [Chuleta de normalización](chuleta-normalizacion.md) | Resumen del procedimiento y errores frecuentes |

## Temporalización orientativa

La unidad dispone de **10 sesiones de 55 minutos**.

| Sesión | Trabajo principal |
| ---: | --- |
| 1 | El problema de la redundancia y las anomalías |
| 2 | Dependencias funcionales, determinantes y claves |
| 3 | Primera forma normal |
| 4 | Segunda forma normal |
| 5 | Tercera forma normal |
| 6 | Ejemplo completo guiado: facturas |
| 7 | Práctica guiada de normalización |
| 8 | BCNF y presentación breve de 4FN y 5FN |
| 9 | Actividades integradoras y corrección razonada |
| 10 | Repaso, autoevaluación y actividad evaluable |

## Idea fundamental

La normalización no se hace mirando únicamente los valores que aparecen hoy en una tabla. Se hace a partir de las **reglas que siempre deben cumplirse** en el sistema.

> Una tabla con pocos datos puede parecer correcta y estar mal diseñada. Antes de dividirla, pregunta siempre: **¿de qué depende realmente cada atributo?**

## Alcance de la unidad

El núcleo de la unidad es la normalización hasta **tercera forma normal (3FN)**. La **BCNF** se trabajará como profundización. La **4FN y la 5FN** se presentan para reconocer problemas más avanzados, sin exigir el mismo nivel de dominio.

---

💡 **Consejo:** no memorices descomposiciones. Escribe primero las dependencias funcionales y deja que estas te indiquen qué tablas necesitas.
