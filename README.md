# Caso 2: Pipeline de Datos en la Nube - DataCo

## 1. Ficha General
* **Integrantes:** [Nombre 1], [Nombre 2], [Nombre 3], Yesica Carolina Restrepo Acosta, Yuli Tatiana Marin Rondón
* **Institución:** Tecnológico de Antioquia - Institución Universitaria
* **Curso:** Computación en la Nube 2026-1
* **Profesor:** Julian David Florez Sanchez

## Índice
* Ficha General
* Matriz de control de cambios
* Contexto del proyecto
* Objetivos
* Requerimientos y Restricciones
* Arquitectura de Solución
* Modelo C4
* ADRs
* Implementación del Pipeline
* Evidencias
* Conclusiones
* Referencias

## 3. Matriz de control de cambios
| Versión | Fecha | Responsable | Descripcion |
|---|---|---|---|
| 1.0 | 28/4/26 | Mateo sanchez  | Creación estructura inicial README |
| 2.0 | 28/4/26 | Mateo sanchez| Creación diagrama c1|
| 2.0 | 5/5/26 | Yesica Restrepo | Creación diagrama c2|
| 4.0 | 6/5/26 | Nataly | Creación diagrama c3|
| 5.0 | 7/5/26 | Yuli Marin | Creación de objetivos, requerimientos y Arquitectura de solución|
| 6.0 | 7/5/26 | Julian | Servicios implementados|


## 4. Contexto del Proyecto
DataCo es una empresa colombiana de distribución con operaciones en 12 departamentos del país. Actualmente presenta problemas de fragmentación de datos debido a que la información se encuentra distribuida en cuatro sistemas aislados: SAP, Oracle, GPS y Salesforce.

### Problemas identificados a resolver:
* **Reportes manuales:** El equipo tarda hasta 5 días en consolidar información en Excel.
* **Inconsistencia:** Datos de clientes y productos no coinciden entre sistemas.
* **Rezago:** Decisiones tomadas con datos de hasta 72 horas de antigüedad.

Para solucionar estas problemáticas, se propone implementar un pipeline de datos en Microsoft Azure utilizando Azure Data Factory, Data Lake Storage Gen2, Databricks, Azure SQL Database y Power BI, con el fin de automatizar el procesamiento de datos y mejorar la disponibilidad y calidad de la información.

## 5. Objetivos
**5.1 Objetivo General**

Diseñar e implementar un pipeline de datos en Azure para centralizar, transformar y analizar la información de DataCo mediante servicios cloud escalables.

**5.2 Objetivos Específicos**
* Integrar múltiples fuentes de datos.
* Automatizar procesos ETL.
* Mejorar calidad de datos.
* Generar dashboards automáticos.
* Reducir tiempos de procesamiento.


## 6. Requerimientos


## 7. Arquitectura de Solución
La solución propuesta para DataCo consiste en un pipeline de datos en la nube basado en servicios de Microsoft Azure, diseñado para automatizar la ingesta, transformación, almacenamiento y visualización de la información empresarial.

La arquitectura implementada permite integrar datos provenientes de múltiples sistemas fuente, mejorar la calidad de la información y reducir los tiempos de generación de reportes.

## 7.1 Servicios implementados

| Servicio | Función |
|---|---|
| Azure Data Factory | Orquestación e ingesta automática de datos |
| Data Lake Storage Gen2 | Almacenamiento de datos raw y curated |
| Azure Databricks | Limpieza y transformación de datos |
| Azure SQL Database | Almacén relacional final |
| Power BI | Visualización y análisis de datos |

## 7.2 Flujo de Datos
* Los archivos CSV son cargados en la zona raw del Data Lake.
* Azure Data Factory detecta e ingesta los archivos.
* Azure Databricks ejecuta procesos de limpieza y transformación.
* Los datos procesados son almacenados en Azure SQL Database.
* Power BI consume la información para generar dashboards automáticos.

## 7.3 Beneficios
* Automatización del procesamiento de datos.
* Reducción de procesos manuales en Excel.
* Mejora en la calidad de datos.
* Escalabilidad en la nube.
* Actualización periódica de reportes.
* Centralización de la información empresarial.



## 8. Modelo C4
En esta sección se detalla la arquitectura de la solución utilizando el modelo C4.

### 8.1 Nivel 1: Diagrama de Contexto
El diagrama de contexto representa la interacción entre el sistema principal de DataCo, los actores del negocio y las fuentes externas de información. El sistema centraliza los datos provenientes de SAP, Oracle, GPS y Salesforce para disponibilizarlos mediante dashboards y reportes en Power BI. Los principales usuarios del sistema son el Analista BI, el Gerente Comercial y el Auditor, quienes utilizan la información para análisis, toma de decisiones y validación de trazabilidad y calidad de datos.

![Diagrama de Contexto](./assets/C1-DataCo.drawio.png)

### 8.2 Nivel 2: Diagrama de Contenedores
El diagrama de contenedores muestra los principales servicios Azure utilizados en la arquitectura del pipeline de datos y el flujo de información entre ellos. Azure Data Factory realiza la orquestación e ingesta de datos desde los sistemas fuente hacia Azure Data Lake Storage Gen2, donde los archivos son almacenados en capas RAW y CURATED. Posteriormente, Azure Databricks ejecuta los procesos de limpieza, validación y transformación de los datos utilizando Apache Spark. Los datos procesados son cargados en Azure SQL Database para su análisis y finalmente consumidos por Power BI mediante dashboards y reportes actualizados automáticamente cada 4 horas.

![Diagrama de Contexto](./assets/C2-DataCo.drawio.png)

### 8.3 Nivel 3: Diagrama de Componentes
El diagrama de componentes detalla la estructura interna de Azure Databricks y los notebooks encargados del procesamiento de datos. Cada componente cumple una función específica dentro del pipeline ETL: ingestión de datos desde la capa RAW, limpieza y validación de inventarios, enriquecimiento de entregas y carga final al Data Warehouse en Azure SQL Database. Además, el modelo incluye servicios de soporte como Delta Lake, Spark Cluster y Job Scheduler para garantizar procesamiento distribuido, control de versiones y automatización de ejecuciones.

![Diagrama de Contexto](./assets/C3-DataCo.drawio.png)



## 9.ADRs

## 10.Implementación del Pipeline

## 11.Evidencias

## 12.Conclusiones

## 13.Referencias
