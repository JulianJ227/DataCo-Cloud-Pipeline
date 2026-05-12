# Caso 2: Pipeline de Datos en la Nube - DataCo

## 1. Ficha General
* **Integrantes:** Julian, Mateo Sanchez, Nataly Rivera Agudelo, Yesica Carolina Restrepo Acosta, Yuli Tatiana Marin Rondón
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
| 4.0 | 6/5/26 | Nataly Rivera| Creación diagrama c3|
| 5.0 | 7/5/26 | Yuli Marin | Creación de objetivos, requerimientos y Arquitectura de solución|
| 6.0 | 7/5/26 | Julian | Servicios implementados|


## 4. Contexto del Proyecto
DataCo es una empresa colombiana de distribución con operaciones en 12 departamentos del país. Actualmente presenta problemas de fragmentación de datos debido a que la información se encuentra distribuida en cuatro sistemas aislados: SAP, Oracle, GPS y Salesforce.

### Problemas identificados a resolver:
* **Reportes manuales:** El equipo tarda hasta 5 días habiles en consolidar información en Excel.
* **Inconsistencia:** Datos de clientes y productos no coinciden entre sistemas.
* **Rezago:** Decisiones tomadas con datos de hasta 72 horas de antigüedad.
*  **Trazabilidad inesistente:** no hay forma de correlacionar una factura de SAP con su entrega real en GPS.
*   **Escalabilidad nula:** servidor antiguo.

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


# 6. Requerimientos de la Arquitectura

## 6.1 Requerimientos Funcionales

| ID | Requerimiento Funcional | Solución Implementada | Restricciones Consideradas |
|---|---|---|---|
| RF-01 | El sistema debe permitir la ingesta automática de datos desde múltiples fuentes (SAP, Oracle, GPS y Salesforce). | Se implementó Azure Data Factory para automatizar la extracción e ingesta de archivos CSV, JSON y datos comerciales hacia el Data Lake. | SAP no dispone de API REST y la integración debe realizarse mediante archivos exportados. |
| RF-02 | El sistema debe almacenar datos crudos y procesados en capas separadas. | Se configuró Azure Data Lake Storage Gen2 con zonas `raw` y `curated`. | Se priorizó una solución escalable y de bajo costo utilizando el tier LRS Standard. |
| RF-03 | El sistema debe limpiar y transformar los datos antes de su análisis. | Azure Databricks ejecuta notebooks para eliminar duplicados, corregir formatos de fecha y estandarizar información. | El equipo tiene conocimientos básicos de Python, por lo que se utilizaron transformaciones simples y automatizadas. |
| RF-04 | El sistema debe consolidar la información en un almacén analítico centralizado. | Los datos transformados se cargan en Azure SQL Database mediante procesos batch. | Se utilizó el Free Tier de Azure SQL para ajustarse al presupuesto del proyecto. |
| RF-05 | El sistema debe permitir la visualización automática de reportes y dashboards. | Power BI se conectó directamente a Azure SQL Database para generar reportes analíticos. | La empresa ya posee licencias Power BI Desktop y no puede asumir costos adicionales. |
| RF-06 | El sistema debe ejecutar el pipeline automáticamente cada 4 horas. | Azure Data Factory programa y orquesta las ejecuciones del pipeline ETL. | Se buscó reducir el rezago de información de hasta 72 horas identificado en el caso. |
| RF-07 | El sistema debe mantener trazabilidad de las transformaciones realizadas sobre los datos. | Databricks registra las transformaciones y Azure SQL almacena datos procesados para auditoría. | Se debe cumplir con políticas internas de gobierno y control de datos. |
| RF-08 | El sistema debe continuar procesando información aunque una fuente falle. | Azure Data Factory implementa pipelines independientes y reintentos automáticos. | El proyecto exige tolerancia a fallos parciales durante la ejecución. |

---

## 6.2 Requerimientos No Funcionales

| ID | Requerimiento No Funcional | Solución Implementada | Restricciones Consideradas |
|---|---|---|---|
| RNF-01 | El sistema debe soportar grandes volúmenes de datos. | Azure Data Lake y Databricks permiten procesamiento distribuido y almacenamiento escalable. | La arquitectura debe soportar hasta 5 millones de registros por ejecución. |
| RNF-02 | El sistema debe mantener un costo reducido durante la fase piloto. | Se utilizaron servicios gratuitos o Free Tier de Azure y Databricks Community Edition. | El presupuesto mensual no debe superar los 80 USD. |
| RNF-03 | El sistema debe garantizar disponibilidad periódica de la información. | Los pipelines se ejecutan automáticamente cada 4 horas mediante Azure Data Factory. | La gerencia requiere información actualizada para toma de decisiones. |
| RNF-04 | El sistema debe garantizar seguridad y control de acceso sobre los datos. | Azure SQL Database implementa autenticación y control de acceso por roles. | Los datos contienen información sensible de clientes y precios. |
| RNF-05 | El sistema debe garantizar calidad de datos superior al 98%. | Databricks aplica validaciones, eliminación de duplicados y normalización de datos. | El caso exige mejorar la confiabilidad de los reportes ejecutivos. |
| RNF-06 | El sistema debe ser mantenible y fácil de administrar. | La solución se dividió en servicios independientes y notebooks modulares. | El equipo posee experiencia limitada en Spark y administración avanzada. |
| RNF-07 | El sistema debe permitir consultas analíticas eficientes. | Azure SQL Database optimiza el almacenamiento relacional para Power BI. | Se requiere análisis rápido de ventas, clientes y regiones. |
| RNF-08 | El sistema debe ser escalable para futuras integraciones. | La arquitectura cloud permite agregar nuevas fuentes y procesos sin rediseñar el pipeline. | DataCo maneja múltiples líneas de negocio y crecimiento continuo. |


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

## ADR-01 · Azure Data Factory sobre Azure Logic Apps para la orquestación

## Contexto. 
DataCo necesita orquestar la ingesta desde cuatro fuentes heterogéneas (SAP por SFTP, Oracle on-premise, archivos GPS y API de Salesforce) hacia el Data Lake, con frecuencia de cada 4 horas, manejo de reintentos y dependencias entre etapas. El equipo de datos tiene solo 2 analistas con SQL y Python básico. El presupuesto es máximo $80 USD/mes y el pipeline debe ser tolerante a fallos parciales (que una fuente falle no debe detener a las otras).

## Alternativas evaluadas.

Azure Logic Apps: orientado a flujos de integración tipo "if-this-then-that". Tiene conectores para Salesforce y SFTP, interfaz visual amigable. Ventaja: curva de aprendizaje muy baja para un equipo no-developer. Desventaja: mal optimizado para mover volúmenes altos de datos (5M registros), facturación por acción ejecutada se dispara en cargas masivas, integración débil con Databricks (no tiene actividad nativa para invocar notebooks Spark).
Azure Data Factory: servicio nativo de Azure para movimiento y orquestación de datos. Ventajas: actividad de copia optimizada para volumen, integración nativa con Databricks (Notebook Activity), self-hosted Integration Runtime para alcanzar Oracle on-premise, tier gratuito que cubre el caso, manejo declarativo de dependencias y reintentos. Desventaja: más complejidad inicial que Logic Apps, su modelo de pipelines requiere comprensión de actividades y triggers.

## Decisión. 
Se adopta Azure Data Factory como orquestador único. La razón principal es que el caso involucra mover volumen significativo de datos hacia un lago y orquestar Databricks; Logic Apps no es la herramienta adecuada para ese patrón aunque sea más amigable. Adicionalmente, ADF permite definir cuatro pipelines independientes (uno por fuente) que pueden ejecutarse en paralelo y fallar de forma aislada — encajando con el requisito de tolerancia a fallos parciales.

## Consecuencias.

Positivas: Tier gratuito cubre el caso piloto sin costo, integración nativa con Databricks elimina código pegamento, monitoreo visual de ejecuciones cubre la auditoría requerida.
Trade-offs: el equipo debe invertir tiempo en aprender el modelo de ADF (se mitiga con la documentación oficial). El self-hosted IR para Oracle requiere instalar un agente en la red de DataCo, lo cual añade un componente operativo a mantener.


## ADR-02 · Azure Databricks Community Edition sobre Azure Synapse Analytics

## Contexto. 
El pipeline requiere transformaciones distribuidas: limpieza, deduplicación, estandarización de códigos cliente/producto entre SAP y CRM, y enriquecimiento cruzado de facturas con datos GPS. Volumen objetivo de hasta 5M registros por ejecución. Restricciones críticas: presupuesto mensual de $80 USD, equipo sin experiencia en Spark ni en administración de clústeres distribuidos.

## Alternativas evaluadas.

Azure Synapse Analytics: plataforma analítica integrada con SQL pools dedicados, Spark pools y orquestación. Ventajas: experiencia unificada, escalabilidad masiva, buen rendimiento para warehouses muy grandes. Desventajas: incluso en serverless el costo mensual estimado supera con holgura los $80 USD si se ejecuta cada 4h, requiere administración de pools, su ecosistema es más complejo para un equipo principiante.
Azure Databricks (workspace pago): mejor experiencia desarrollo Spark, escalado automático, integración con ADF. Costo de DBU + VMs supera presupuesto piloto.
Azure Databricks Community Edition: edición gratuita con clúster único de hasta 15 GB de RAM en Spark, notebooks colaborativos, sin tarjeta de crédito. Desventaja: sin SLA, sin integración nativa con Azure AD, los notebooks se ejecutan en infraestructura propia de Databricks (no en la suscripción Azure de DataCo) — la integración con ADF se hace vía REST API, no con la actividad nativa.

## Decisión. 
Se adopta Azure Databricks Community Edition para la fase piloto. El argumento decisivo es presupuesto: es la única alternativa que entrega Spark sin costo. La capacidad de 15 GB de RAM cubre holgadamente el dataset de prueba (1.000 registros pedidos por la rúbrica) y soporta el volumen objetivo si se procesan archivos por particiones diarias.

## Consecuencias.

Positivas: costo cero en procesamiento, los analistas pueden aprender Spark y PySpark en un entorno de bajo riesgo, los notebooks son portables a un workspace pago de Databricks cuando DataCo escale.
Trade-offs: sin SLA — riesgo aceptable en piloto, no en producción real. La invocación desde ADF es por API REST y no nativa. Al pasar a producción será necesario migrar a un workspace pago o a Synapse, y este ADR debe revisarse. Se asume explícitamente que esta decisión es válida solo durante el piloto.


## ADR-03 · Azure Data Lake Storage Gen2 sobre Blob Storage estándar

## Contexto. 
El pipeline necesita una zona raw para datos crudos en su formato original (CSV/JSON desde SAP, Oracle, GPS, Salesforce) y una zona curated con datos transformados en Parquet, organizados por fuente y fecha. Los datos contienen información sensible de precios y márgenes, y deben tener acceso restringido por roles. El presupuesto es ajustado.

## Alternativas evaluadas.

Azure Blob Storage estándar: almacenamiento de objetos genérico. Ventajas: costo por GB ligeramente menor, simple. Desventajas: namespace plano (no hay carpetas reales, solo prefijos), permisos limitados a nivel de contenedor (no por carpeta), peor rendimiento de Spark al listar grandes volúmenes de archivos.
Azure Data Lake Storage Gen2: construido sobre Blob Storage pero con namespace jerárquico habilitado (HNS). Ventajas: estructura real de carpetas, ACLs POSIX por carpeta y archivo, optimizado para motores analíticos como Spark, soporte nativo de formato Parquet y particiones, mismo precio base que Blob con un pequeño recargo por operaciones de metadatos. Desventajas: ligero recargo de costo y un poco más de complejidad inicial.

## Decisión. 
Se adopta Azure Data Lake Storage Gen2. La justificación combina tres factores: el namespace jerárquico es necesario para organizar raw/<fuente>/<fecha>/ y curated/<entidad>/, las ACLs por carpeta resuelven el requisito de acceso restringido a datos sensibles de precios sin necesidad de cuentas separadas, y la práctica estándar del sector para arquitecturas tipo medallion exige ADLS Gen2 + Parquet.

## Consecuencias.

Positivas: organización limpia por fuente y fecha, permisos finos sin proliferar contenedores, lecturas Spark más rápidas, alineado con la arquitectura de referencia de Microsoft que cita el enunciado.
Trade-offs: costo marginalmente superior a Blob estándar (despreciable a escala piloto), cualquier herramienta o script que asuma namespace plano debe ajustarse.

## ADR-04 · Azure SQL Database sobre Azure Cosmos DB para el almacén analítico

## Contexto. 
El almacén final debe servir un modelo dimensional consolidado (hechos de ventas, dimensiones de cliente, producto, ruta) consultable desde Power BI Desktop. Las consultas son típicamente analíticas: agregaciones por periodo, región y producto. El equipo de DataCo conoce SQL bien pero no tiene experiencia con bases NoSQL. Power BI ya está licenciado y se conecta nativamente a SQL Server. Presupuesto: $80 USD/mes total para todo el stack.


## Alternativas evaluadas.

Azure Cosmos DB: base NoSQL multi-modelo, latencia muy baja, escalado global. Ventajas: excelente para cargas operacionales con altísimo throughput. Desventajas: costo por RU/s difícil de mantener bajo $80 USD para cargas analíticas, modelo de consultas distinto al SQL clásico (los analistas tendrían que aprender), conexión con Power BI menos directa, optimizado para perfiles de uso transaccional, no analítico.
Azure SQL Database (Free tier): SQL Server gestionado con 32 GB de almacenamiento y 100.000 vCore-segundos/mes gratis. Ventajas: el equipo ya domina SQL, conector nativo en Power BI, soporta vistas, índices columnares y roles a nivel de objeto, encaja en presupuesto cero durante piloto. Desventajas: el free tier tiene cuota de cómputo limitada que en cierres de mes podría ser ajustada, escala vertical limitada frente a opciones masivas como Synapse.

## Decisión. 
Se adopta Azure SQL Database en su free tier. Tres razones convergen: el equipo no necesita aprender un paradigma nuevo, Power BI Desktop se conecta nativamente con un conector probado, y el costo en piloto es cero. Cosmos DB sería una elección equivocada para un caso analítico con perfil de consulta de BI tradicional.

## Consecuencias.

Positivas: sin costo en piloto, índices columnares aceleran las consultas analíticas de Power BI, los roles SQL cubren el requisito de acceso restringido a datos sensibles, los analistas son productivos desde el primer día.
Trade-offs: en cierres de mes la cuota gratuita de cómputo puede saturarse — se debe monitorear y eventualmente escalar a tier pago. La escala máxima de Azure SQL es menor a la de Synapse; si DataCo crece a decenas de millones de registros/día este ADR debe revisarse.


## ADR-05 · Power BI Desktop sobre Azure Analysis Services

## Contexto. 
La capa de visualización debe entregar dashboards de ventas, inventario y logística actualizados automáticamente cada 4 horas. Power BI Desktop ya está licenciado en los equipos de los analistas (gratuito). El presupuesto no permite herramientas adicionales de visualización. Los analistas conocen Power BI Desktop.

## Alternativas evaluadas.

Azure Analysis Services (AAS): servicio de modelado tabular en memoria, escalable, ideal cuando varios consumidores comparten un modelo semántico complejo. Ventajas: rendimiento superior con modelos grandes, modelo centralizado reusable. Desventajas: tier más bajo cuesta varias decenas de USD al mes, queda fuera del presupuesto $80 USD considerando todo el stack, requiere administración adicional, redundante para un caso piloto con un único modelo y pocos consumidores.
Power BI Desktop con publicación local (.pbix): dashboards diseñados localmente, conexión directa o programada a Azure SQL, distribución del archivo dentro del equipo. Ventajas: costo cero (ya licenciado), se conecta nativamente a Azure SQL Database con el conector SQL Server, soporta refresh programado vía gateway si se publica al servicio, los analistas ya lo manejan. Desventajas: el refresh automático sin gateway es limitado, gestión de versiones del .pbix manual, escalabilidad de usuarios concurrentes inferior a un servicio centralizado.

## Decisión. 
Se adopta Power BI Desktop como herramienta de visualización del piloto. La restricción de presupuesto y el hecho de que ya está licenciado hacen de cualquier alternativa una decisión injustificable en esta fase.

## Consecuencias.

Positivas: costo adicional cero, productividad inmediata del equipo, conexión nativa a Azure SQL, cumple el requisito de "dashboard sin intervención manual" si se publica al Power BI Service con refresh programado.
Trade-offs: la administración del modelo semántico es por archivo, no centralizada — adecuado para un piloto pero no para una organización con muchos creadores. Para escalar habrá que evaluar Power BI Premium o AAS, y este ADR debe revisarse cuando aumente el número de consumidores o la complejidad del modelo.

## 10.Implementación del Pipeline

## 11.Evidencias

## 12.Conclusiones

## 13.Referencias
