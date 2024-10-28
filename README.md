# Envios Database

El esquema de la base de datos sistemagestion está diseñado para gestionar de manera eficiente la información logística de una empresa de transporte y distribución. Este sistema permite el seguimiento integral de paquetes, la gestión de clientes y conductores, así como la administración de rutas y sucursales.

## Creación de la Base de Datos


```sql


CREATE DATABASE sistemagestion;
USE sistemagestion;

CREATE TABLE logistica_auxiliares(
id int NOT NULL,
nombre VARCHAR(100),
telefono int,
CONSTRAINT PK_LOGISTICA_AUXILIARES_Id PRIMARY KEY (Id)
);
 CREATE TABLE logistica_pais(
id int NOT NULL,
nombre VARCHAR(50),
CONSTRAINT PK_LOGISTICA_PAIS_Id PRIMARY KEY (Id)
);

CREATE TABLE logistica_ciudades(
id int NOT NULL,
nombre VARCHAR(100),
idpais_fk int (11),
CONSTRAINT PK_LOGISTICACIUDADES_Id PRIMARY KEY (Id),
CONSTRAINT FK_LOGISTICACIUDADES_PAIS_Id FOREIGN KEY (idpais_fk ) REFERENCES  logistica_pais(id)
);

CREATE TABLE logistica_clientes(
id int NOT NULL AUTO_INCREMENT,
nombre VARCHAR(50),
email VARCHAR(100),
direccion VARCHAR(200),
CONSTRAINT PK_LOGISTICA_CLIENTES_Id PRIMARY KEY (Id)
);

CREATE TABLE logistica_conductores(
id int NOT NULL,
nombre VARCHAR(100),
CONSTRAINT PK_LOGISTICA_CONDUCTORES_Id PRIMARY KEY (Id)
);
CREATE TABLE logistica_paquetes(
 id int NOT NULL ,
numero_seguimiento int,
peso DECIMAL (10,2),
dimensiones VARCHAR (50),
contenido TEXT,
valor_declarado DECIMAL (10,2),
tipo_servicio VARCHAR(50),
estado  VARCHAR(50),
tipo_envio VARCHAR(20),
CONSTRAINT PK_LOGISTICAPAQUETES_Id PRIMARY KEY (Id)
);
CREATE TABLE logistica_sucursales(
id int NOT NULL ,
nombre VARCHAR(100),
direccion VARCHAR(200),
idciudad_fk int (11),
CONSTRAINT PK_LOGISTICASUCURSALES_Id PRIMARY KEY (Id),
CONSTRAINT FK_SucursalesCiudad_Id FOREIGN KEY (idciudad_fk ) REFERENCES logistica_ciudades(id)
);
CREATE TABLE logistica_seguimiento(
id int NOT NULL,
ubicacion VARCHAR(200),
fecha_hora TIMESTAMP,
estado VARCHAR(50),
idpaquete_fk int (11),
CONSTRAINT PK_LOGISTICASEGUIMIENTO_Id PRIMARY KEY (Id),
CONSTRAINT FK_SeguimientoPaquete_Id FOREIGN KEY (idpaquete_fk ) REFERENCES logistica_paquetes(id)
);
CREATE TABLE logistica_rutas (
 id int NOT NULL ,
descripcion VARCHAR(200),
idsucursal_fk int (11),
CONSTRAINT PK_LOGISTICARUTAS_Id PRIMARY KEY (Id),
CONSTRAINT FK_LOGISTICARutas_Id FOREIGN KEY (idsucursal_fk ) REFERENCES logistica_sucursales(id)
);
CREATE TABLE logistica_envios(
id int NOT NULL AUTO_INCREMENT,
idcliente_fk int (11),
idpaquete_fk int (11),
fecha_envio TIMESTAMP,
destino VARCHAR(200),
idrutas_fk int (11),
idsucursal_fk int(11),
idciudad_fk int(11),
estado VARCHAR(50),
CONSTRAINT PK_LOGISTICAENVIOS_Id PRIMARY KEY (Id),
CONSTRAINT FK_LOGISTICAENVIOSClientes_Id FOREIGN KEY (idcliente_fk ) REFERENCES logistica_clientes(id),
CONSTRAINT FK_LOGISTICAENVIOSPaquetes_Id FOREIGN KEY (idpaquete_fk ) REFERENCES logistica_paquetes(id),
CONSTRAINT FK_LOGISTICAENVIOSRutas_Id FOREIGN KEY (idrutas_fk ) REFERENCES logistica_rutas(id),
CONSTRAINT FK_LOGISTICAENVIOSucursales_Id FOREIGN KEY (idsucursal_fk ) REFERENCES logistica_sucursales(id),
CONSTRAINT FK_EnviosCiudad_Id FOREIGN KEY (idciudad_fk ) REFERENCES logistica_ciudades(id)
);
CREATE TABLE logistica_vehiculos(
id int NOT NULL,
placa VARCHAR(20),
marca VARCHAR(50),
modelo VARCHAR(50),
capacidad_carga DECIMAL(10,2),
idsucursal_fk int (11),
CONSTRAINT PK_LOGISTICAVEHICULOS_Id PRIMARY KEY (Id),
CONSTRAINT FK_VEHICULO_SUCURSAL_Id FOREIGN KEY (idsucursal_fk ) REFERENCES logistica_sucursales(id)
);

CREATE TABLE logistica_conductores_vias(
 id int NOT NULL,
idconductor_fk int (11),
idruta_fk int (11),
idvehiculo_fk int (11),
idsucursales_fk int(11),
CONSTRAINT PK_LOGISTICACONDUCTORESVIAS_Id PRIMARY KEY (Id),
CONSTRAINT FK_LOGISTICACONDUCTORES_Id FOREIGN KEY (idconductor_fk ) REFERENCES logistica_conductores(id),
CONSTRAINT FK_LOGISTICACONDUCTORES_RUTA_Id FOREIGN KEY (idruta_fk ) REFERENCES logistica_rutas(id),
CONSTRAINT FK_LOGISTICACONDUCTORES_VEHICULO_Id FOREIGN KEY (idvehiculo_fk ) REFERENCES logistica_vehiculos(id),
CONSTRAINT FK_LOGISTICACONDUCTORES_SUCURSALES_Id FOREIGN KEY (idsucursales_fk ) REFERENCES logistica_sucursales(id)
);
CREATE TABLE logistica_rutas_auxiliares(
id int NOT NULL ,
idrutas_fk int(11),
idauxiliares_fk int (11),
CONSTRAINT PK_LOGISTICARUTASAUXILIARES_Id PRIMARY KEY (Id),
CONSTRAINT FK_LOGISTICAAuxiliareSRutas_Id FOREIGN KEY (idauxiliares_fk ) REFERENCES logistica_auxiliares(id),
CONSTRAINT FK_LOGISTICARutasAuxiliares_Id FOREIGN KEY (idrutas_fk ) REFERENCES logistica_rutas(id)
);

CREATE TABLE logistica_telefono_clientes(
id int NOT NULL,
numero int,
idcliente_fk int (11),
CONSTRAINT PK_TELEFONO_CLIENTES_Id PRIMARY KEY (Id),
CONSTRAINT FK_TELEFONOCLIENTES_Id FOREIGN KEY (idcliente_fk ) REFERENCES logistica_clientes(id)
);

CREATE TABLE logistica_telefonos_conductores(
id int NOT NULL,
numero int,
idconductor_fk int (11),
CONSTRAINT PK_TELEFONO_CONDUCTORES_Id PRIMARY KEY (Id),
CONSTRAINT FK_TELEFONOCONDUCTORES_Id FOREIGN KEY (idconductor_fk ) REFERENCES logistica_conductores(id)
);

```
# Agregar Información 

## Insertar información
```sql
INSERT INTO logistica_auxiliares (id, nombre, telefono) VALUES
(1, 'Juan Pérez', 30012345),
(2, 'María Gómez', 310987);

INSERT INTO logistica_pais (id, nombre) VALUES
(1, 'Colombia'),
(2, 'México');
INSERT INTO logistica_ciudades (id, nombre, idpais_fk) VALUES
(1, 'Bogotá', 1),
(2, 'Medellín', 1),
(3, 'Ciudad de México', 2),
(4, 'Guadalajara', 2);

INSERT INTO logistica_clientes (id, nombre, email, direccion) VALUES
(1, 'Carlos Rodríguez', 'carlos@gmail.com', 'Calle 123, Bogotá'),
(2, 'Ana López', 'ana@gmail.com', 'Avenida 456, Medellín'),
(3, 'Luis Fernández', 'luis@gmail.com', 'Calle 789, Ciudad de México');

INSERT INTO logistica_conductores (id, nombre) VALUES
(1, 'Andrés Torres'),
(2, 'Sofía Martínez'),
(3,' Danna Guerrero' );


INSERT INTO logistica_paquetes (id, numero_seguimiento, peso, dimensiones, contenido, valor_declarado, tipo_servicio, estado, tipo_envio) VALUES
(1, 123456, 1.5, '30x20x10', 'Ropa', 50000, 'Express', 'En tránsito', 'Nacional'),
(2, 789012, 2.0, '50x40x30', 'Electrodoméstico', 150000, 'Normal', 'Entregado', 'Internacional');

INSERT INTO logistica_sucursales (id, nombre, direccion, idciudad_fk) VALUES
(1, 'Sucursal Centro', 'Carrera 10 # 20-30', 1),
(2, 'Sucursal Norte', 'Avenida 15 # 30-40', 2),
(3, 'Sucursal Sur', 'Avenida Principal 100', 3);

INSERT INTO logistica_vehiculos (id, placa, marca, modelo, capacidad_carga, idsucursal_fk) VALUES
(1, 'ABC123', 'Toyota', 'Hilux', 1000, 1),
(2, 'XYZ456', 'Chevrolet', 'Tracker', 800, 2);

INSERT INTO logistica_rutas (id, descripcion, idsucursal_fk) VALUES
(1, 'Ruta 1: Centro a Norte', 1),
(2, 'Ruta 2: Norte a Sur', 2),
(3, 'Ruta 3: Sur a Centro', 3);

INSERT INTO logistica_seguimiento (id, ubicacion, fecha_hora, estado, idpaquete_fk) VALUES
(1, 'Centro de distribución', '2024-10-01 10:00:00', 'En tránsito', 1),
(2, 'Sucursal Norte', '2024-10-02 12:30:00', 'Entregado', 2);

INSERT INTO logistica_conductores_vias (id, idconductor_fk, idruta_fk, idvehiculo_fk, idsucursales_fk) VALUES
(1, 1, 1, 1, 1),
(2, 2, 2, 2, 2);

INSERT INTO logistica_rutas_auxiliares (id, idrutas_fk, idauxiliares_fk) VALUES
(1, 1, 1),
(2, 2, 2);

INSERT INTO logistica_telefono_clientes (id, numero, idcliente_fk) VALUES
(1, 3007654, 1),
(2, 3106543, 2);
INSERT INTO logistica_telefonos_conductores (id, numero, idconductor_fk) VALUES
(1, 3209876, 1),
(2, 3211234, 2);

INSERT INTO logistica_envios (idcliente_fk, idpaquete_fk, fecha_envio, destino, idrutas_fk, idsucursal_fk, idciudad_fk, estado)
VALUES 
(1, 1, NOW(), 'Ciudad de México', 1, 1, 3, 'En tránsito'),
(2, 2, NOW(), 'Guadalajara', 2, 2, 4, 'En tránsito');


INSERT INTO logistica_rutas_auxiliares (id, idrutas_fk, idauxiliares_fk)
VALUES (3, 1, 2); 


INSERT INTO logistica_seguimiento (id, ubicacion, fecha_hora, estado, idpaquete_fk)
VALUES (3, 'Centro de Distribución - Zona Norte', NOW(), 'En Tránsito', 1);

```
## Consultas
```sql

SELECT e.id AS id, e.fecha_envio, p.numero_seguimiento, p.peso, p.dimensiones, p.estado
FROM logistica_envios e
JOIN logistica_clientes c ON e.idcliente_fk = c.id
JOIN logistica_paquetes p ON e.idpaquete_fk = p.id
WHERE c.id = 1;


UPDATE logistica_paquetes
SET estado = 'Entregado'
WHERE id = 1;


SELECT ubicacion, estado, fecha_hora
FROM logistica_seguimiento
WHERE idpaquete_fk = 1 
ORDER BY fecha_hora DESC
LIMIT 1;

SELECT 
    env.id AS envio_id,
    cli.nombre AS cliente_nombre,
    cli.email AS cliente_email,
    cli.direccion AS cliente_direccion,
    pqt.numero_seguimiento AS paquete_numero_seguimiento,
    pqt.peso AS paquete_peso,
    pqt.dimensiones AS paquete_dimensiones,
    pqt.contenido AS paquete_contenido,
    pqt.valor_declarado AS paquete_valor_declarado,
    pqt.tipo_servicio AS paquete_tipo_servicio,
    pqt.estado AS paquete_estado,
    rts.descripcion AS ruta_descripcion,
    cnd.nombre AS conductor_nombre,
    suc.nombre AS sucursal_nombre,
    suc.direccion AS sucursal_direccion
FROM 
    logistica_envios env
JOIN 
    logistica_clientes cli ON env.idcliente_fk = cli.id
JOIN 
    logistica_paquetes pqt ON env.idpaquete_fk = pqt.id
JOIN 
    logistica_rutas rts ON env.idrutas_fk = rts.id
JOIN 
    logistica_conductores cnd ON cnd.id = env.idcliente_fk
JOIN 
    logistica_sucursales suc ON env.idsucursal_fk = suc.id;

SELECT 
    env.id AS envio_id,
    pqt.numero_seguimiento AS paquete_numero_seguimiento,
    pqt.peso AS paquete_peso,
    pqt.dimensiones AS paquete_dimensiones,
    pqt.contenido AS paquete_contenido,
    pqt.estado AS paquete_estado,
    seg.fecha_hora AS seguimiento_fecha,
    seg.ubicacion AS seguimiento_ubicacion,
    seg.estado AS seguimiento_estado
FROM 
    logistica_envios env
JOIN 
    logistica_paquetes pqt ON env.idpaquete_fk = pqt.id
JOIN 
    logistica_seguimiento seg ON seg.idpaquete_fk = pqt.id
WHERE 
    env.idcliente_fk = 1;

SELECT 
    cnd.nombre AS conductor_nombre,
    rts.descripcion AS ruta_descripcion,
    veh.placa AS vehiculo_placa,
    veh.marca AS vehiculo_marca,
    veh.modelo AS vehiculo_modelo,
    suc.nombre AS sucursal_nombre,
    suc.direccion AS sucursal_direccion
FROM 
    logistica_conductores_vias cv
JOIN 
    logistica_conductores cnd ON cv.idconductor_fk = cnd.id
JOIN 
    logistica_rutas rts ON cv.idruta_fk = rts.id
JOIN 
    logistica_vehiculos veh ON cv.idvehiculo_fk = veh.id
JOIN 
    logistica_sucursales suc ON cv.idsucursales_fk = suc.id;


SELECT 
    rts.descripcion AS ruta_descripcion,
    aux.nombre AS auxiliar_nombre,
    aux.telefono AS auxiliar_telefono
FROM 
    logistica_rutas_auxiliares ra
JOIN 
    logistica_rutas rts ON ra.idrutas_fk = rts.id
JOIN 
    logistica_auxiliares aux ON ra.idauxiliares_fk = aux.id;

SELECT 
    suc.nombre AS sucursal_nombre,
    pqt.estado AS paquete_estado,
    COUNT(pqt.id) AS cantidad_paquetes
FROM 
    logistica_paquetes pqt
JOIN 
    logistica_envios env ON pqt.id = env.idpaquete_fk
JOIN 
    logistica_sucursales suc ON env.idsucursal_fk = suc.id
GROUP BY 
    suc.nombre, pqt.estado
ORDER BY 
    suc.nombre, pqt.estado;

SELECT 
    pqt.id AS paquete_id,
    pqt.numero_seguimiento AS paquete_numero_seguimiento,
    pqt.peso AS paquete_peso,
    pqt.dimensiones AS paquete_dimensiones,
    pqt.contenido AS paquete_contenido,
    pqt.valor_declarado AS paquete_valor_declarado,
    pqt.tipo_servicio AS paquete_tipo_servicio,
    pqt.estado AS paquete_estado,
    seg.fecha_hora AS seguimiento_fecha,
    seg.ubicacion AS seguimiento_ubicacion,
    seg.estado AS seguimiento_estado
FROM 
    logistica_paquetes pqt
JOIN 
    logistica_seguimiento seg ON seg.idpaquete_fk = pqt.id
WHERE 
    pqt.id = 1
ORDER BY 
    seg.fecha_hora;

SELECT *
FROM logistica_paquetes p
JOIN logistica_envios e ON p.id = e.idpaquete_fk
WHERE e.fecha_envio BETWEEN '2024-10-01' AND '2024-10-31';

SELECT *
FROM logistica_paquetes
WHERE estado IN ('En tránsito', 'Entregado');

SELECT *
FROM logistica_paquetes
WHERE estado NOT IN ('Recibido', 'Retenido en aduana');

SELECT DISTINCT c.*
FROM logistica_clientes c
JOIN logistica_envios e ON c.id = e.idcliente_fk
WHERE e.fecha_envio BETWEEN '2024-10-01' AND '2024-10-31';

SELECT *
FROM logistica_conductores
WHERE id NOT IN (SELECT idconductor_fk FROM logistica_conductores_vias WHERE idruta_fk IN (1, 2));  

SELECT *
FROM logistica_paquetes
WHERE valor_declarado BETWEEN 10000 AND 100000;

SELECT a.*
FROM logistica_auxiliares a
JOIN logistica_rutas_auxiliares ra ON a.id = ra.idauxiliares_fk
WHERE ra.idrutas_fk IN (1, 2);

SELECT e.* 
FROM logistica_envios e
JOIN logistica_ciudades c ON e.idciudad_fk = c.id
WHERE c.nombre NOT IN ('Bogotá', 'Medellín');

SELECT s.*
FROM logistica_seguimiento s
WHERE s.fecha_hora BETWEEN '2024-01-01' AND '2024-12-31';

SELECT *
FROM logistica_clientes
WHERE id IN (
    SELECT idcliente_fk
    FROM logistica_envios e
    JOIN logistica_paquetes p ON e.idpaquete_fk = p.id
    WHERE p.tipo_envio IN ('Nacional', 'Internacional')
);

```
