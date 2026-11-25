# Solucionario — Transformación del modelo E/R al modelo relacional

**BAE · 1.º DAM**

Este documento contiene la resolución de los seis ejercicios. Utilícenlo para comprobar sus resultados y entender el procedimiento, no solo para consultar el modelo final.

## Procedimiento de resolución

1. **Localiza las entidades** del diagrama.
2. **Crea una tabla por cada entidad** e incluye sus atributos.
3. **Marca la clave primaria (PK)** de cada tabla.
4. **Busca las relaciones 1:N** y coloca las FKs en el lado N.
5. **Busca las relaciones N:M** y crea las tablas intermedias correspondientes.
6. **Revisa las relaciones 1:1** y decide dónde colocar una FK única.
7. **Comprueba los casos especiales**: entidades débiles, reflexivas, jerarquías y ternarias.
8. **Revisa los resultados:** ninguna FK debe hacer referencia a un registro inexistente; determina cuáles pueden admitir `NULL` y qué valores deben ser únicos.

> **Criterio de los apuntes:** en una relación 1:N, cuando la cardinalidad junto al lado 1 es (0,1), se representa la relación mediante una tabla intermedia cuya PK es la clave del lado N. Así se evita almacenar una FK nula en la tabla del lado N. Cuando es (1,1), se incorpora la FK directamente al lado N. Este criterio se aplica en los ejercicios 1 y 4.

## Notación

- **PK**: clave primaria.
- **FK**: clave foránea.
- **NOT NULL**: valor obligatorio.
- **NULL**: valor opcional.
- **UQ (UNIQUE)**: valor o combinación de valores únicos.

En los diagramas, las tablas de entidades aparecen en verde y las tablas procedentes de relaciones en amarillo. Las PK se subrayan; las líneas conectan las FK con las PK referenciadas.

Las cardinalidades se interpretan según la notación de los diagramas originales. Una clave primaria compuesta está formada por varios atributos.

---

## Ejercicio 1. Empresa de transportes

### 1. Localizar las entidades

CAMIONERO, CAMION, PAQUETE y PROVINCIA.

### 2. Crear las tablas e incluir sus atributos

| Tabla | Atributos |
|---|---|
| CAMIONERO | dni, nombre, poblacion, tfno, direccion, salario |
| CAMION | matricula, potencia, modelo, tipo |
| PAQUETE | codigo, descripcion, destinatario, direccion |
| PROVINCIA | codigo, nombre |

### 3. Marcar las claves primarias

- **CAMIONERO:** `dni` (PK).
- **CAMION:** `matricula` (PK).
- **PAQUETE:** `codigo` (PK).
- **PROVINCIA:** `codigo` (PK).


### 4. Relaciones 1:N

- **DISTRIBUYE:** un camionero distribuye uno o varios paquetes. Un paquete puede tener cero o un camionero. Como la cardinalidad junto a CAMIONERO es (0,1), se crea DISTRIBUYE siguiendo los apuntes. Su PK es `codigo_paquete` (también FK) y `dni_camionero` es una FK obligatoria. Un paquete sin camionero no tendrá fila en DISTRIBUYE.
- **DESTINADO:** una provincia recibe cero o varios paquetes. Cada paquete tiene una única provincia de destino. Se añade `codigo_provincia` a PAQUETE como FK obligatoria.

### 5. Relaciones N:M

**CONDUCE:** un camionero puede conducir varios camiones y un camión puede ser conducido por varios camioneros. Se crea CONDUCE con las dos FKs, que forman su PK compuesta.

### 6. Relaciones 1:1

No existen.

### 7. Casos especiales

No existen entidades débiles, relaciones reflexivas, jerarquías ni ternarias.

### 8. Revisar los resultados: modelo relacional y restricciones

```text
CAMIONERO(dni, nombre, poblacion, tfno, direccion, salario)
    PK: dni

CAMION(matricula, potencia, modelo, tipo)
    PK: matricula

PROVINCIA(codigo, nombre)
    PK: codigo

PAQUETE(codigo, descripcion, destinatario, direccion,
        codigo_provincia)
    PK: codigo
    FK: codigo_provincia -> PROVINCIA(codigo)

DISTRIBUYE(codigo_paquete, dni_camionero)
    PK: codigo_paquete
    FK: codigo_paquete -> PAQUETE(codigo)
    FK: dni_camionero -> CAMIONERO(dni)

CONDUCE(dni_camionero, matricula_camion)
    PK: (dni_camionero, matricula_camion)
    FK: dni_camionero -> CAMIONERO(dni)
    FK: matricula_camion -> CAMION(matricula)
```

| Atributo | Restricción |
|---|---|
| DISTRIBUYE.codigo_paquete | PK, FK, NOT NULL y único |
| DISTRIBUYE.dni_camionero | FK, NOT NULL; puede repetirse |
| PAQUETE.codigo_provincia | NOT NULL |
| CONDUCE.dni_camionero | NOT NULL |
| CONDUCE.matricula_camion | NOT NULL |
| Pareja de claves de CONDUCE | Única |

#### Representación gráfica del modelo relacional

![Modelo relacional del ejercicio 1](imagenes/ejercicio_1.png)

---

## Ejercicio 2. Clientes, productos y proveedores

### 1. Localizar las entidades

CLIENTE, PRODUCTO y PROVEEDOR.

### 2. Crear las tablas e incluir sus atributos

| Tabla | Atributos |
|---|---|
| CLIENTE | dni, nombre, apellidos, fecha_nac, tfno |
| PRODUCTO | codigo, nombre, precio |
| PROVEEDOR | nif, nombre, direccion |

### 3. Marcar las claves primarias

- **CLIENTE:** `dni` (PK).
- **PRODUCTO:** `codigo` (PK).
- **PROVEEDOR:** `nif` (PK).


### 4. Relaciones 1:N

**SUMINISTRA:** un proveedor suministra uno o varios productos y cada producto tiene un único proveedor. Se añade `nif_proveedor` a PRODUCTO como FK obligatoria.

### 5. Relaciones N:M

**COMPRA:** un cliente puede comprar cero o varios productos y un producto puede ser comprado por cero o varios clientes. Se crea COMPRA con las dos FKs como PK compuesta. La relación no tiene atributos propios.

### 6. Relaciones 1:1

No existen.

### 7. Casos especiales

No existen.

### 8. Revisar los resultados: modelo relacional y restricciones

```text
CLIENTE(dni, nombre, apellidos, fecha_nac, tfno)
    PK: dni

PROVEEDOR(nif, nombre, direccion)
    PK: nif

PRODUCTO(codigo, nombre, precio, nif_proveedor)
    PK: codigo
    FK: nif_proveedor -> PROVEEDOR(nif)

COMPRA(dni_cliente, codigo_producto)
    PK: (dni_cliente, codigo_producto)
    FK: dni_cliente -> CLIENTE(dni)
    FK: codigo_producto -> PRODUCTO(codigo)
```

| Atributo | Restricción |
|---|---|
| PRODUCTO.nif_proveedor | NOT NULL |
| COMPRA.dni_cliente | NOT NULL |
| COMPRA.codigo_producto | NOT NULL |
| Pareja de claves de COMPRA | Única |

Una pareja cliente-producto solo puede registrarse una vez; el modelo no contempla un historial de compras.

#### Representación gráfica del modelo relacional

![Modelo relacional del ejercicio 2](imagenes/ejercicio_2.png)

---

## Ejercicio 3. Centro educativo

### 1. Localizar las entidades

PROFESOR, ALUMNO y MODULO.

### 2. Crear las tablas e incluir sus atributos

| Tabla | Atributos |
|---|---|
| PROFESOR | dni, nombre, direccion, tfno |
| ALUMNO | expediente, nombre, apellidos, fecha_nac |
| MODULO | codigo, nombre |

### 3. Marcar las claves primarias

- **PROFESOR:** `dni` (PK).
- **ALUMNO:** `expediente` (PK).
- **MODULO:** `codigo` (PK).


### 4. Relaciones 1:N

**IMPARTE:** un profesor imparte uno o varios módulos y cada módulo es impartido por un único profesor. Se añade `dni_profesor` a MODULO como FK obligatoria.

### 5. Relaciones N:M

**CURSA:** cada alumno cursa uno o varios módulos y cada módulo es cursado por uno o varios alumnos. Se crea CURSA con `expediente_alumno` y `codigo_modulo` como FKs y PK compuesta.

### 6. Relaciones 1:1

No existen.

### 7. Casos especiales

**ES DELEGADO:** relación reflexiva 1:N. Un alumno puede ser delegado de cero o varios alumnos y cada alumno tiene un delegado. Se añade `expediente_delegado` a ALUMNO como FK que referencia a la propia tabla. No admite NULL según la cardinalidad indicada, ni debe ser UNIQUE, pues varios alumnos pueden compartir delegado. Si se permite que el delegado se represente a sí mismo, la FK puede apuntar al propio registro.

### 8. Revisar los resultados: modelo relacional y restricciones

```text
PROFESOR(dni, nombre, direccion, tfno)
    PK: dni

ALUMNO(expediente, nombre, apellidos, fecha_nac, expediente_delegado)
    PK: expediente
    FK: expediente_delegado -> ALUMNO(expediente)

MODULO(codigo, nombre, dni_profesor)
    PK: codigo
    FK: dni_profesor -> PROFESOR(dni)

CURSA(expediente_alumno, codigo_modulo)
    PK: (expediente_alumno, codigo_modulo)
    FK: expediente_alumno -> ALUMNO(expediente)
    FK: codigo_modulo -> MODULO(codigo)
```

| Atributo | Restricción |
|---|---|
| ALUMNO.expediente_delegado | NOT NULL |
| MODULO.dni_profesor | NOT NULL |
| CURSA.expediente_alumno | NOT NULL |
| CURSA.codigo_modulo | NOT NULL |
| Pareja de claves de CURSA | Única |

Si se quisiera permitir alumnos sin delegado, habría que modificar la cardinalidad y admitir NULL.

#### Representación gráfica del modelo relacional

![Modelo relacional del ejercicio 3](imagenes/ejercicio_3.png)

---

## Ejercicio 4. Concesionario de coches

### 1. Localizar las entidades

CLIENTE, COCHE y REVISION.

### 2. Crear las tablas e incluir sus atributos

| Tabla | Atributos |
|---|---|
| CLIENTE | nif, nombre, direccion, ciudad, tfno |
| COCHE | matricula, marca, modelo, color, precio |
| REVISION | codigo, filtro, aceite, frenos |

### 3. Marcar las claves primarias

- **CLIENTE:** `nif` (PK).
- **COCHE:** `matricula` (PK).
- **REVISION:** `codigo` (PK).


### 4. Relaciones 1:N

- **COMPRA:** un cliente compra uno o varios coches. Un coche puede tener cero o un comprador. Como la cardinalidad junto a CLIENTE es (0,1), se crea COMPRA siguiendo los apuntes. Su PK es `matricula_coche` (también FK) y `nif_cliente` es una FK obligatoria. Un coche sin vender no tendrá fila en COMPRA.
- **PASA:** un coche puede pasar cero o varias revisiones y cada revisión corresponde a un único coche. Se añade `matricula_coche` a REVISION como FK obligatoria.

### 5. Relaciones N:M

No existen.

### 6. Relaciones 1:1

No existen.

### 7. Casos especiales

No existen. REVISION no es una entidad débil, pues dispone de su propia PK.

### 8. Revisar los resultados: modelo relacional y restricciones

```text
CLIENTE(nif, nombre, direccion, ciudad, tfno)
    PK: nif

COCHE(matricula, marca, modelo, color, precio)
    PK: matricula

COMPRA(matricula_coche, nif_cliente)
    PK: matricula_coche
    FK: matricula_coche -> COCHE(matricula)
    FK: nif_cliente -> CLIENTE(nif)

REVISION(codigo, filtro, aceite, frenos, matricula_coche)
    PK: codigo
    FK: matricula_coche -> COCHE(matricula)
```

| Atributo | Restricción |
|---|---|
| COMPRA.matricula_coche | PK, FK, NOT NULL y único |
| COMPRA.nif_cliente | FK, NOT NULL; puede repetirse |
| REVISION.matricula_coche | NOT NULL |

La matrícula de COMPRA es única por ser PK: un coche no puede tener varios compradores. El NIF de COMPRA y la matrícula de REVISION pueden repetirse.

#### Representación gráfica del modelo relacional

![Modelo relacional del ejercicio 4](imagenes/ejercicio_4.png)

---

## Ejercicio 5. Hospital

### 1. Localizar las entidades

MEDICO, INGRESO y PACIENTE.

### 2. Crear las tablas e incluir sus atributos

| Tabla | Atributos |
|---|---|
| MEDICO | codigo, nombre, apellidos |
| INGRESO | codigo, habitacion |
| PACIENTE | codigo, nombre, apellidos |

### 3. Marcar las claves primarias

- **MEDICO:** `codigo` (PK).
- **INGRESO:** `codigo` (PK).
- **PACIENTE:** `codigo` (PK).


### 4. Relaciones 1:N

- **ATIENDE:** un médico atiende cero o varios ingresos y cada ingreso es atendido por un único médico. Se añade `codigo_medico` a INGRESO como FK obligatoria.
- **REALIZA:** un paciente realiza uno o varios ingresos y cada ingreso corresponde a un único paciente. Se añade `codigo_paciente` a INGRESO como FK obligatoria. El atributo `fecha` se incorpora también a INGRESO, la tabla del lado N.

### 5. Relaciones N:M

No existen.

### 6. Relaciones 1:1

No existen.

### 7. Casos especiales

No existen. INGRESO tiene una PK propia y no es una entidad débil.

### 8. Revisar los resultados: modelo relacional y restricciones

```text
MEDICO(codigo, nombre, apellidos)
    PK: codigo

PACIENTE(codigo, nombre, apellidos)
    PK: codigo

INGRESO(codigo, habitacion, fecha, codigo_medico, codigo_paciente)
    PK: codigo
    FK: codigo_medico -> MEDICO(codigo)
    FK: codigo_paciente -> PACIENTE(codigo)
```

| Atributo | Restricción |
|---|---|
| INGRESO.codigo_medico | NOT NULL |
| INGRESO.codigo_paciente | NOT NULL |
| INGRESO.fecha | NOT NULL, si se exige registrar la fecha |

Ambas FKs pueden repetirse: un médico puede atender varios ingresos y un paciente puede ingresar varias veces.

#### Representación gráfica del modelo relacional

![Modelo relacional del ejercicio 5](imagenes/ejercicio_5.png)

---

## Ejercicio 6. Gestión comercial

### 1. Localizar las entidades

PRODUCTO, CLIENTE y PROVEEDOR.

### 2. Crear las tablas e incluir sus atributos

| Tabla | Atributos |
|---|---|
| PRODUCTO | codigo, descripcion, existencias, precio |
| CLIENTE | codigo, nombre, apellidos, direccion, tfno |
| PROVEEDOR | codigo, nombre, apellidos, direccion, tfno |

### 3. Marcar las claves primarias

- **PRODUCTO:** `codigo` (PK).
- **CLIENTE:** `codigo` (PK).
- **PROVEEDOR:** `codigo` (PK).


### 4. Relaciones 1:N

No existen.

### 5. Relaciones N:M

- **COMPRA:** un cliente compra cero o varios productos y un producto puede ser comprado por cero o varios clientes. Se crea COMPRA con `codigo_cliente`, `codigo_producto` y `fecha_compra`, atributo propio de la relación. La PK de la transformación directa es la pareja de FKs.
- **SUMINISTRA:** un proveedor suministra uno o varios productos y un producto es suministrado por uno o varios proveedores. Se crea SUMINISTRA con las dos FKs como PK compuesta.

### 6. Relaciones 1:1

No existen.

### 7. Casos especiales

No existen entidades débiles, relaciones reflexivas, jerarquías ni ternarias. Hay que conservar el atributo propio de COMPRA.

### 8. Revisar los resultados: modelo relacional y restricciones

```text
PRODUCTO(codigo, descripcion, existencias, precio)
    PK: codigo

CLIENTE(codigo, nombre, apellidos, direccion, tfno)
    PK: codigo

PROVEEDOR(codigo, nombre, apellidos, direccion, tfno)
    PK: codigo

COMPRA(codigo_cliente, codigo_producto, fecha_compra)
    PK: (codigo_cliente, codigo_producto)
    FK: codigo_cliente -> CLIENTE(codigo)
    FK: codigo_producto -> PRODUCTO(codigo)

SUMINISTRA(codigo_proveedor, codigo_producto)
    PK: (codigo_proveedor, codigo_producto)
    FK: codigo_proveedor -> PROVEEDOR(codigo)
    FK: codigo_producto -> PRODUCTO(codigo)
```

| Atributo | Restricción |
|---|---|
| COMPRA.codigo_cliente | NOT NULL |
| COMPRA.codigo_producto | NOT NULL |
| COMPRA.fecha_compra | NOT NULL, si se exige registrar la fecha |
| SUMINISTRA.codigo_proveedor | NOT NULL |
| SUMINISTRA.codigo_producto | NOT NULL |
| Pareja de claves de COMPRA | Única |
| Pareja de claves de SUMINISTRA | Única |

#### Representación gráfica del modelo relacional

![Modelo relacional del ejercicio 6](imagenes/ejercicio_6.png)

### Consideración adicional: compras repetidas

La transformación directa no permite que un cliente compre el mismo producto en fechas diferentes.

| codigo_cliente | codigo_producto | fecha_compra |
|---|---|---|
| C01 | P01 | 20/11/2026 |
| C01 | P01 | 25/11/2026 |

El segundo registro produciría un error de clave duplicada. Para almacenar compras en fechas diferentes, se podría ampliar la PK:

```text
COMPRA(codigo_cliente, codigo_producto, fecha_compra)
    PK: (codigo_cliente, codigo_producto, fecha_compra)
    FK: codigo_cliente -> CLIENTE(codigo)
    FK: codigo_producto -> PRODUCTO(codigo)
```

Esto tampoco permite dos compras del mismo producto por el mismo cliente en la misma fecha. Para admitirlas, se podría introducir un identificador propio:

```text
COMPRA(id_compra, codigo_cliente, codigo_producto, fecha_compra)
    PK: id_compra
    FK: codigo_cliente -> CLIENTE(codigo)
    FK: codigo_producto -> PRODUCTO(codigo)
```

Ambas alternativas son ampliaciones del modelo original.

---

> **Importante:** las restricciones PK, FK, UNIQUE y NOT NULL no garantizan por sí solas todas las cardinalidades mínimas. Por ejemplo, una FK en PRODUCTO garantiza que cada producto tenga un proveedor válido, pero no que todos los proveedores suministren al menos un producto. Para ello se necesitan controles adicionales, como disparadores o controles transaccionales.

Los diagramas no especifican la obligatoriedad de todos los atributos ordinarios. Las restricciones NULL y NOT NULL del solucionario se deducen principalmente de las claves y las cardinalidades.
