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



## 4. Contexto del Proyecto
DataCo es una empresa colombiana de distribución con operaciones en 12 departamentos del país. Actualmente presenta problemas de fragmentación de datos debido a que la información se encuentra distribuida en cuatro sistemas aislados: SAP, Oracle, GPS y Salesforce.

Esta situación genera procesos manuales de consolidación en Excel, inconsistencias entre registros, retrasos en la actualización de información y dificultades para realizar análisis integrados del negocio. Además, los reportes pueden tardar entre 3 y 5 días en generarse y existen problemas de trazabilidad en las entregas.

Para solucionar estas problemáticas, se propone implementar un pipeline de datos en Microsoft Azure utilizando Azure Data Factory, Data Lake Storage Gen2, Databricks, Azure SQL Database y Power BI, con el fin de automatizar el procesamiento de datos y mejorar la disponibilidad y calidad de la información.

## 5. Objetivos
**5.1 Objetivo General**

Diseñar e implementar un pipeline de datos en Azure para centralizar, transformar y analizar la información de DataCo mediante servicios cloud escalables.

**5.2 Objetivos Específicos**
Integrar múltiples fuentes de datos.
Automatizar procesos ETL.
Mejorar calidad de datos.
Generar dashboards automáticos.
Reducir tiempos de procesamiento.










### Problemas identificados a resolver:
* [cite_start]**Reportes manuales:** El equipo tarda hasta 5 días en consolidar información en Excel[cite: 38, 39].
* [cite_start]**Inconsistencia:** Datos de clientes y productos no coinciden entre sistemas[cite: 41].
* [cite_start]**Rezago:** Decisiones tomadas con datos de hasta 72 horas de antigüedad[cite: 40].# DataCo-Cloud-Pipeline

## 3. Modelo C4
En esta sección se detalla la arquitectura de la solución utilizando el modelo C4.

### 3.1 Nivel 1: Diagrama de Contexto
El siguiente diagrama muestra cómo el Sistema de Datos de DataCo interactúa con los usuarios y los sistemas fuente existentes.

![Diagrama de Contexto](./assets/C1-DataCo.drawio.png)

**Elementos del Sistema:**
* **Sistema de Datos DataCo:** Solución centralizada encargada de la ingesta, transformación y carga de datos.
* **Sistemas Fuente:** SAP (Ventas), Oracle (Inventario), Salesforce (CRM) y GPS (Logística).
* **Usuarios:** El Gerente Comercial y el Analista de BI consumen los datos procesados, mientras que el Auditor supervisa la trazabilidad.
