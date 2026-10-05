# Modelo entidad/relación. Viveros
Alba Hidalgo Martín
Juhi Kamal Chatani Mansukhani
Administración y Diseño de Bases de Datos
Universidad de La Laguna


## Entidades

### Producto
Los productos que vende Tajinaste S.A.

Atributos:
`id_producto`: Identificador del producto (clave primaria).
`nombre`
`tipo`
`precio`

Ejemplo: 01, Rosa, Flores, 1 euro

### Vivero

Viveros perteneciente a Tajinaste S.A.

Atributos:
`id_vivero`: Identificador del vivero (clave primaria).
`nombre` 
`latitud`
`longitud`

Ejemplo: 01, Vivero Palmar, 40.10, 4.1

### Zona

Zonas en las que se divide un vivero. Dependiente al vivero (entidad débil)

Atributos:
`id_zona` Identificador de la zona dentro del vivero.
`nombre` Nombre o tipo de zona.
`latitud` 
`longitud`

Ejemplo: 04, Exposición, 29.19, -10.2

### Empleado

Empleados de Tajinaste S.A.

Atributos
`id_empleado` Identificador del empleado (clave primaria)
`nombre` 
`apellidos`
`telefono` 

Ejemplo: 03, Eva, Martínez Afonso, 29381920

### Cliente

Clientes de Tajinaste S.A.

Atributos
`dni_cliente` Identificador del cliente (clave primaria).
`nombre` 
`email` 

Ejemplo: 13832923N, Pablo, pablo@gmail.com

### Cliente_plus

Es una especialización de `Cliente` que representa a los clientes que pertenecen al programa de fidelización Tajinaste Plus. Posee los atributos heredados de Cliente

Atributos
`fecha_ingreso` Fecha en la que el cliente se incorporó a  Tajinaste Plus
`bonificacion_acumulada` Bonificación acumulada por el cliente en función de sus compras

Ejemplo: 13/08/2020, 25 euros

### Pedido

Pedido realizado por un cliente.

Atributos:
`num_pedido` Identificador del pedido (clave primaria).
`fecha` Fecha en la que se realizó el pedido.
`importe_total`

Ejemplo: 193283B, 19/09/2026, 7 euros


## Relaciones

### Compuesto_por

Relaciona cada Vivero con las Zonas que lo forman.

- Un Vivero está compuesto por una o varias Zonas `(1,N)`.
- Una Zona pertenece a un único Vivero `(1,1)`.

Teniendo en cuenta la entidad débil zona de que no existe sin el vivero

### Almacena

Los Productos con las Zonas donde se encuentran disponibles.

Atributo:
`stock` Cantidad disponible de un producto en una determinada zona.
Ejemplo: 50

Un producto puede estar disponible en ninguna, una o varias zonas (0,N), y una zona puede contener ninguno, uno o varios productos (0,N).

### Trabaja_en

Relaciona los Empleados con las Zonas en las que trabajan.

Atributos:
`fecha_inicio` Fecha en la que comienza la asignación del empleado a la zona.
`fecha_fin` Fecha en la que finaliza la asignación. Puede ser nulo si continúa trabajando allí.
`productividad` Productividad registrada para el empleado durante esa asignación. 
`puesto`: Tarea que realizó durante la asignación
Ejemplo: 01/12/25, NULL, 80%, podaje

Un empleado puede tener cero, una o varias asignaciones a lo largo del tiempo (0,N). Una zona puede tener cero, uno o varios empleados (0,N).
Se tiene en cuenta que la cardinalidad de (0,N) para los empleados en zonas es para implementar el historial de asignaciones. En un mismo tiempo no se puede tener más de una zona asignada a la vez.


### Incluye

Relaciona los Pedidos con los Productos que contienen.

Atributos:
`cantidad` Número de unidades del producto incluidas en el pedido.
`precio_unitario` Precio del producto en el momento de realizar el pedido.
Ejemplo: 2, 12 euros

Un pedido incluye uno o varios productos (1,N), mientras que un producto puede aparecer en cero o muchos pedidos (0,N).

### Realiza

Relaciona los Cliente_plus con los Pedidos que realizan.

Un Cliente_plus puede realizar cero o muchos pedidos (0,N) y cada pedido pertenece a un único Cliente_plus (1,1).

### Gestiona

Relaciona los Empleados con los Pedidos que gestionan para realizar seguimiento y valorar los objetivos de venta.


Un empleado puede gestionar cero o muchos pedidos (0,N), mientras que cada pedido tiene un único empleado responsable (1,1).



## Especialización Cliente - Cliente_plus

- Un Cliente puede pertenecer al programa Tajinaste Plus, pero no es obligatorio (parcial).
- Un Cliente_plus es también un Cliente.
- Los atributos `fecha_ingreso` y `bonificacion_acumulada` son específicos de los clientes Plus.

La especialización es **parcial**:

No se distingue entre exclusiva y solapada ya que sólo hay 1 subtipo


## Restricciones semánticas

1. Un empleado no puede tener dos destinos simultáneamente. Los períodos de asignación de un mismo empleado no pueden solaparse.

