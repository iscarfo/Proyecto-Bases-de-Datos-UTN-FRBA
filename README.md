# Trabajo Práctico – Bases de Datos  
## Migración de Datos y Modelo de Business Intelligence

## Descripción General

Este repositorio contiene el desarrollo del **Trabajo Práctico de la materia Bases de Datos** de la **Universidad Tecnológica Nacional – Facultad Regional Buenos Aires (UTN FRBA)**.

El proyecto consistió en la **migración de una tabla maestra altamente desnormalizada**, con gran volumen de datos, hacia un **modelo transaccional normalizado**, y posteriormente en el diseño e implementación de un **modelo de Business Intelligence (BI)** orientado al análisis y explotación de la información.

El trabajo abarca tanto aspectos técnicos de **ETL**, integridad referencial y calidad de datos, como decisiones de **modelado dimensional** y optimización de consultas analíticas.

---

## Objetivos del Proyecto

- Migrar los datos desde `gd_esquema.Maestra` a un esquema transaccional normalizado.
- Garantizar **unicidad, consistencia e integridad referencial** durante el proceso de migración.
- Diseñar un **modelo BI dimensional** optimizado para consultas de negocio.
- Implementar procedimientos almacenados reutilizables y mantenibles.
- Construir vistas de negocio que respondan a los indicadores solicitados por el enunciado.

---

## Modelo Transaccional

### Estrategia de Migración

La migración se realizó mediante **procedimientos almacenados específicos por tabla**, respetando un orden de ejecución definido por las dependencias de claves foráneas.

Principales criterios aplicados:

#### Manejo de Duplicados y Unicidad
- Uso de `SELECT DISTINCT` para evitar inserciones repetidas desde la tabla maestra.
- Validaciones mediante `NOT EXISTS` para prevenir duplicados en tablas destino.
- Aplicación de `ROW_NUMBER()` en escenarios puntuales donde un mismo identificador lógico podía repetirse con distintas variantes.

#### Manejo de Claves Primarias y Valores Nulos
- Exclusión de registros sin identificadores válidos cuando los datos asociados carecían de sentido lógico.
- Generación o reutilización controlada de claves primarias según el caso (por ejemplo, envíos).

#### Mapeo y Consolidación de Datos
- Uso intensivo de `JOINs` para traducir datos desnormalizados en claves foráneas válidas.
- Consolidación de encabezados (Pedidos, Facturas) para evitar duplicación provocada por registros de detalle.

#### Materiales y Tipos
- Migración jerárquica de tipos y subtipos de materiales.
- Manejo explícito de campos nulos en criterios de unicidad para evitar inconsistencias.

#### Orden de Ejecución
El proceso respeta una secuencia estricta para asegurar integridad referencial, migrando primero entidades base (ubicaciones, clientes, sucursales, proveedores) y luego entidades dependientes (pedidos, facturas, detalles, envíos).

---

## Modelo de Business Intelligence (BI)

### Enfoque General

El modelo BI se diseñó bajo un **esquema dimensional**, separando claramente **dimensiones** y **hechos**, con el objetivo de facilitar el análisis de métricas y mejorar el rendimiento de las consultas.

---

### Dimensiones

- **Dimensión Tiempo:**  
  Granularidad diaria, con atributos pre-calculados (año, mes, cuatrimestre) para optimizar agregaciones.

- **Dimensión Ubicación:**  
  Consolidación de provincia y localidad en una única dimensión reutilizable.

- **Dimensión Sucursal:**  
  Información específica de cada sucursal, relacionada con la dimensión de ubicación.

- **Dimensiones Categóricas:**  
  Rango etario y turno, con categorías predefinidas e inclusión de valores especiales para datos incompletos.

---

### Hechos

Se implementaron múltiples tablas de hechos, cada una representando un proceso de negocio independiente:

- **Hechos Ventas**
- **Hechos Compras**
- **Hechos Envíos**

Principales decisiones:
- Pre-agregación de métricas durante la migración para reducir volumen y mejorar performance.
- Uso de flags calculados (por ejemplo, cumplimiento de envíos) para simplificar consultas analíticas.

---

### Proceso de Migración BI

La carga del modelo BI se realiza mediante procedimientos almacenados:

- Migración de dimensiones.
- Migración de hechos.
- Procedimiento maestro que orquesta la ejecución completa.

Este enfoque permite reejecutar migraciones de forma controlada y facilita futuras actualizaciones de datos.

---

## Vistas de Negocio

Las vistas se diseñaron como una **capa de abstracción**, ocultando la complejidad del modelo y exponiendo directamente los indicadores solicitados.

Ejemplos:
- Rankings de modelos más vendidos mediante funciones analíticas.
- Reportes de ganancias que contemplan períodos sin actividad, evitando pérdida de información.

---

## Tecnologías Utilizadas

- **Base de Datos:** SQL Server
- **Lenguaje:** T-SQL
- **Técnicas:**  
  - Stored Procedures  
  - Funciones analíticas  
  - Modelado relacional y dimensional  
- **Herramientas:** SQL Server Management Studio (SSMS)

---

## Equipo de Trabajo

**Grupo 83 – LOS_QUE_SABEN_SABEN**

- Ignacio Scarfo  
- Lucas Zheng  
- Manuel Di Bucci  
- Santiago Nicolás Torres Franco  

---
