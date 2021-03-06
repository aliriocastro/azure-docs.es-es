---
title: Qué supervisa Azure Monitor
description: Referencia de todos los servicios y otros recursos supervisados por Azure Monitor.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/17/2020
ms.openlocfilehash: 3cd330e9c4ceba2feeb7a74cafe9f094fd03d690
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77669121"
---
# <a name="what-is-monitored-by-azure-monitor"></a>¿Qué supervisa Azure Monitor?
En este artículo se describen las distintas aplicaciones y servicios que se supervisan mediante Azure Monitor. 

## <a name="insights-and-core-solutions"></a>Conclusiones y soluciones principales
Las conclusiones y soluciones principales se consideran parte de Azure Monitor y siguen los acuerdos de nivel de servicio y soporte técnico de Azure. Se admiten en todas las regiones de Azure donde Azure Monitor está disponible.

### <a name="insights"></a>Información detallada

Insights proporciona una experiencia de supervisión personalizada para determinadas aplicaciones y servicios. Recopilan y analizan tanto los registros como las métricas.

| Conclusión | Descripción |
|:---|:---|
| [Application Insights](app/app-insights-overview.md) | Servicio de administración del rendimiento de aplicaciones (APM) extensible para supervisar una aplicación web activa en cualquier plataforma. |
| [Azure Monitor para contenedores](insights/container-insights-overview.md) | Supervisa el rendimiento de las cargas de trabajo de contenedor implementadas en Azure Container Instances o en clústeres de Kubernetes administrados hospedados en Azure Kubernetes Service (AKS). |
| [Azure Monitor para Cosmos DB (versión preliminar)](insights/cosmosdb-insights-overview.md) | Proporciona una vista del rendimiento general, los errores, la capacidad y el estado operativo de todos los recursos de Azure Cosmos DB en una experiencia interactiva unificada. |
| [Azure Monitor para Networks (versión preliminar)](insights/network-insights-overview.md) | Proporciona una vista completa del estado y las métricas de todos los recursos de red. La capacidad de búsqueda avanzada ayuda a identificar dependencias de recursos, lo que habilita escenarios como la identificación de recursos que hospedan el sitio web con una simple búsqueda del nombre del sitio web. |
[Azure Monitor para grupos de recursos (versión preliminar)](insights/resource-group-insights.md) |  Clasifica y diagnostica cualquier problema que encuentren sus recursos individuales, a la vez que ofrece un contexto en cuanto al estado y el rendimiento del grupo de recursos como un todo. |
| [Azure Monitor para Storage (versión preliminar)](insights/storage-insights-overview.md) | Proporciona una supervisión completa de las cuentas de Azure Storage al ofrecer una vista unificada del rendimiento, la capacidad y la disponibilidad de los servicios de Azure Storage. |
| [Azure Monitor para VM (versión preliminar)](insights/container-insights-overview.md) | Supervisa las máquinas virtuales (VM) y los conjuntos de escalado de máquinas virtuales de Azure. El servicio analiza el rendimiento y el estado de las VM Windows y Linux, y supervisa sus procesos y dependencias en otros recursos y procesos externos. |

### <a name="core-solutions"></a>Soluciones principales

Las soluciones se basan en consultas de registros y vistas personalizadas para una aplicación o servicio en particular. Recopilan y analizan solo los registros y están cayendo en desuso a favor de las conclusiones.

| Solución | Descripción |
|:---|:---|
| [Estado del agente](insights/solution-agenthealth.md) | Analice el estado y la configuración de los agentes de Log Analytics. |
| [Administración de alertas](platform/alert-management-solution.md) | Analice las alertas recopiladas de System Center Operations Manager, Nagios o Zabbix. |
| [Mapa de servicio](insights/service-map.md) | Detecta automáticamente los componentes de la aplicación en sistemas Windows y Linux, y asigna la comunicación entre servicios. |



## <a name="azure-services"></a>Servicios de Azure
En la tabla siguiente se enumeran los servicios de Azure y los datos que estos recopilan en Azure Monitor. 

- Métricas: El servicio recopila automáticamente métricas en Métricas de Azure Monitor. 
- Registros: El servicio admite la configuración de diagnóstico que puede recopilar registros y métricas de la plataforma para Registros de Azure Monitor.
- Insight: Hay una conclusión disponible para el servicio, que proporciona una experiencia de supervisión personalizada para el servicio.

| Servicio | Métricas | Registros | Conclusión | Notas |
|:---|:---|:---|:---|:---|
|Active Directory | Sin | Sí | [Sí](../active-directory/reports-monitoring/howto-use-azure-monitor-workbooks.md) |  |
|Active Directory B2C | Sin | Sin | Sin |  |
|Active Directory Domain Services | Sin | Sí | Sin |  |
|Registro de actividades | Sin | Sí | Sin | |
|Protección contra amenazas avanzada | Sin | Sin | Sin |  |
|Advisor | Sin | Sin | Sin |  |
|AI Builder | Sin | Sin | Sin |  |
|Analysis Services | Sí | Sí | Sin |  |
|API for FHIR | Sin | Sin | Sin |  |
|API Management | Sí | Sí | Sin |  |
|App Service | Sí | Sí | Sin |  |
|AppConfig | Sin | Sin | Sin |  |
|Application Gateway | Sí | Sí | Sin |  |
|Servicio de atestación | Sin | Sin | Sin |  |
|Automation | Sí | Sí | Sin |  |
|Azure Service Manager (RDFE) | Sin | Sin | Sin |  |
|Copia de seguridad | Sin | Sí | Sin |  |
|Bastion | Sin | Sin | Sin |  |
|Batch | Sí | Sí | Sin |  |
|Batch AI | Sin | Sin | Sin |  |
|Blockchain Service | Sin | Sí | Sin |  |
|Planos técnicos | Sin | Sin | Sin |  |
|Servicio de bots | Sin | Sin | Sin |  |
|Cloud Services | Sí | Sí | Sin | Agente necesario para supervisar los flujos de trabajo y el sistema operativo invitado.  |
|Cloud Shell | Sin | Sin | Sin |  |
|Cognitive Services | Sí | Sí | Sin |  |
|Azure Container Instances | Sí | Sin | Sin |  |
|Container Registry | Sí | Sí | Sin |  |
|Content Delivery Network (CDN) | Sin | Sí | Sin |  |
|Cosmos DB | Sí | Sí | [Sí](insights/cosmosdb-insights-overview.md) |  |
|Administración de costos | Sin | Sin | Sin |  |
|Data Box | Sin | Sin | Sin |  |
|Data Catalog Gen2 | Sin | Sin | Sin |  |
|Data Explorer | Sí | Sí | Sin |  |
|Data Factory | Sí | Sí | Sin |  |
|Data Factory v2 | Sin | Sí | Sin |  |
|Recurso compartido de datos | Sin | Sin | Sin |  |
|Database for MariaDB | Sí | Sí | Sin |  |
|Database for MySQL | Sí | Sí | Sin |  |
|Database for PostgreSQL | Sí | Sí | Sin |  |
|Database Migration Service | Sin | Sin | Sin |  |
|Databricks | Sin | Sí | Sin |  |
|Protección contra DDOS | Sí | Sí | Sin |  |
|DevOps | Sin | Sin | Sin |  |
|DNS | Sí | Sin | Sin |  |
|Nombres de dominio | Sin | Sin | Sin |  |
|DPS | Sin | Sin | Sin |  |
|Dynamics 365 Customer Engagement | Sin | Sin | Sin |  |
|Dynamics 365 Finance y Operations | Sin | Sin | Sin |  |
|Event Grid | Sí | Sin | Sin |  |
|Event Hubs | Sí | Sí | Sin |  |
|ExpressRoute | Sí | Sí | Sin |  |
|Firewall | Sí | Sí | Sin |  |
|Front Door | Sí | Sí | Sin |  |
|Functions | Sí | Sí | Sin |  |
|HDInsight | Sin | Sí | Sin |  |
|HPC Cache | Sin | Sin | Sin |  |
|Information Protection | Sin | Sí | Sin |  |
|Intune | Sin | Sí | Sin |  |
|IoT Central | Sin | Sin | Sin |  |
|IoT Hub | Sí | Sí | Sin |  |
|Key Vault | Sí | Sí | Sin |  |
|Kubernetes Service (AKS) | Sin | Sin | [Sí](insights/container-insights-overview.md)  |  |
|Load Balancer | Sí | Sí | Sin |  |
|Logic Apps | Sí | Sí | Sin |  |
|Machine Learning Service | Sin | Sin | Sin |  |
|Aplicaciones administradas  | Sin | Sin | Sin |  |
|Mapas  | Sin | Sin | Sin |  |
|Media Services | Sí | Sí | Sin |  |
|Microsoft Flow | Sin | Sin | Sin |  |
|Escritorio administrado de Microsoft | Sin | Sin | Sin |  |
|Microsoft PowerApps | Sin | Sin | Sin |  |
|Microsoft Social Engagement | Sin | Sin | Sin |  |
|Microsoft Stream | Sí | Sí | Sin |  |
|Migrar | Sin | Sin | Sin |  |
|Multi-Factor Authentication | Sin | Sí | Sin |  |
|Network Watcher | Sí | Sí | Sin |  |
|Notification Hubs | Sí | Sin | Sin |  |
|Open Datasets | Sin | Sin | Sin |  |
|Directiva | Sin | Sin | Sin |  |
|Power BI | Sí | Sí | Sin |  |
|Power BI Embedded | Sin | Sin | Sin |  |
|Private Link | Sin | Sin | Sin |  |
|Plataforma de comunicación de Project Spool | Sin | Sin | Sin |  |
|Red Hat OpenShift | Sin | Sin | Sin |  |
|Redis Cache | Sí | Sí | Sin |  |
|Gráfico de recursos | Sin | Sin | Sin |  |
|Resource Manager | Sin | Sin | Sin |  |
|Búsqueda minorista: por Bing | Sin | Sin | Sin |  |
|Search | Sí | Sí | Sin |  |
|Azure Service Bus | Sí | Sí | Sin |  |
|Service Fabric | Sin | Sí | Sin | Agente necesario para supervisar los flujos de trabajo y el sistema operativo invitado.  |
|Portal de suscripción | Sin | Sin | Sin |  |
|Site Recovery | Sin | Sí | Sin |  |
|Servicio Spring Cloud | Sin | Sin | Sin |  |
|SQL Data Warehouse | Sí | Sí | Sin |  |
|SQL Database | Sí | Sí | Sin |  |
|SQL Server Stretch Database | Sí | Sí | Sin |  |
|Pila | Sin | Sin | Sin |  |
|Storage | Sí | Sin | [Sí](insights/storage-insights-overview.md) |  |
|Almacenamiento caché | Sin | Sin | Sin |  |
|Servicios de sincronización de almacenamiento | Sin | Sin | Sin |  |
|Stream Analytics | Sí | Sí | Sin |  |
|Time Series Insights | Sí | Sí | Sin |  |
|TINA | Sin | Sin | Sin |  |
|Traffic Manager | Sí | Sí | Sin |  |
|Impresión universal | Sin | Sin | Sin |  |
|Virtual Machine Scale Sets | Sin | Sí | [Sí](insights/vminsights-overview.md) | Agente necesario para supervisar los flujos de trabajo y el sistema operativo invitado. |
|Virtual Machines | Sí | Sí | [Sí](insights/vminsights-overview.md) | Agente necesario para supervisar los flujos de trabajo y el sistema operativo invitado. |
|Virtual Network | Sí | Sí | [Sí](insights/network-insights-overview.md) |  |
|Virtual Network: registros de flujo de NSG | Sin | Sí | Sin |  |
|VPN Gateway | Sí | Sí | Sin |  |
|Windows Virtual Desktop | Sin | Sin | Sin |  |


## <a name="product-integrations"></a>Integraciones de productos
Los servicios y las soluciones de la tabla siguiente almacenan sus datos en un área de trabajo de Log Analytics para que se puedan analizar con otros datos de registro recopilados por Azure Monitor.

| Producto o servicio | Descripción |
|:---|:---|
| [Azure Automation](/azure/automation/) | Administre las actualizaciones del sistema operativo y realice un seguimiento de los cambios en equipos Windows y Linux. Consulte [Change Tracking](../automation/change-tracking.md) y [Update Management](../automation/automation-update-management.md). |
| [Azure Information Protection ](https://docs.microsoft.com/azure/information-protection/) | Clasifique y, opcionalmente, proteja documentos y correos electrónicos. Consulte [Informes centrales para Azure Information Protection](https://docs.microsoft.com/azure/information-protection/reports-aip#configure-a-log-analytics-workspace-for-the-reports). |
| [Azure Security Center](/azure/security-center/) | Recopile y analice eventos de seguridad y complete análisis de amenazas. Consulte [Recopilación de datos en Azure Security Center](/azure/security-center/security-center-enable-data-collection). |
| [Azure Sentinel](/azure/sentinel/) | Se conecta a orígenes diferentes, incluido Office 365 y Amazon Web Services CloudTrail. Consulte [Conexión con orígenes de datos](/azure/sentinel/connect-data-sources). |
| [Análisis de Key Vault](insights/azure-key-vault.md) | Analice registros de AuditEvent de Azure Key Vault. |
| [Microsoft Intune](https://docs.microsoft.com/intune/) | Cree una configuración de diagnóstico para enviar registros a Azure Monitor. Consulte [Envío de datos de registro al almacenamiento, a Event Hubs o a Log Analytics en Intune (versión preliminar)](https://docs.microsoft.com/intune/fundamentals/review-logs-using-azure-monitor).  |
| Red  | [Network Performance Monitor](insights/network-performance-monitor.md): Supervisa la conectividad de red y el rendimiento de los puntos de conexión de servicios y aplicaciones.<br>[Azure Application Gateway](insights/azure-networking-analytics.md#azure-application-gateway-analytics-solution-in-azure-monitor): Analice registros y métricas de Azure Application Gateway.<br>[Análisis de tráfico](/azure/network-watcher/traffic-analytics): Analiza los registros de flujo del grupo de seguridad de red (NSG) de Network Watcher para proporcionar conclusiones sobre el flujo de tráfico en la nube de Azure. |
| [Office 365](insights/solution-office-365.md) | Supervise su entorno de Office 365. Versión actualizada con incorporación mejorada disponible a través de Azure Sentinel. |
| [SQL Analytics](insights/azure-sql.md) | Supervise el rendimiento de bases de datos, grupos elásticos e instancias administradas de Azure SQL a escala y entre varias suscripciones. |
| [Surface Hub](insights/surface-hubs.md) | Realice un seguimiento del estado y el uso de los dispositivos Surface Hub. |
| [System Center Operations Manager](https://docs.microsoft.com/system-center/scom) | Recopile datos de agentes de Operations Manager mediante la conexión de su grupo de administración a Azure Monitor. Consulte [Conexión de Operations Manager con Azure Monitor](platform/om-agents.md).<br> Evalúe el riesgo y el estado del grupo de administración de System Center Operations Manager con la solución [Operations Manager Assessment](insights/scom-assessment.md). |
| [Salas de Microsoft Teams](https://docs.microsoft.com/microsoftteams/room-systems/azure-monitor-deploy) | Administración integrada de un extremo a otro de los dispositivos de Salas de Microsoft Teams. |
| [Visual Studio App Center](https://docs.microsoft.com/appcenter/) | Compile, pruebe y distribuya aplicaciones y, a continuación, supervise su estado y uso. Consulte [Comience a analizar la aplicación móvil con App Center y Application Insights](learn/mobile-center-quickstart.md). |
| Windows | [Windows Update Compliance](https://docs.microsoft.com/windows/deployment/update/update-compliance-get-started): Evalúe las actualizaciones del escritorio de Windows.<br>[Análisis de escritorio](https://docs.microsoft.com/configmgr/desktop-analytics/overview): Se integra en Configuration Manager para proporcionar conclusiones e inteligencia para tomar decisiones más informadas sobre la preparación de las actualizaciones de los clientes Windows. |



## <a name="other-solutions"></a>Otras soluciones
Hay otras soluciones disponibles para supervisar diferentes aplicaciones y servicios, pero el desarrollo activo se ha detenido y puede que no estén disponibles en todas las regiones. Están cubiertas por el acuerdo de nivel de servicio de ingesta de datos de Azure Log Analytics.

| Solución | Descripción |
|:---|:---|
| [Comprobación de mantenimiento de Active Directory](insights/ad-assessment.md) | Evalúe el riesgo y el estado de los entornos de Active Directory. |
| [Estado de replicación de Active Directory](insights/ad-replication-status.md) | Supervisa periódicamente el entorno de Active Directory para comprobar si existen errores de replicación. |
| [Activity Log Analytics](platform/activity-log-view.md#activity-logs-analytics-monitoring-solution) | Analice las entradas de Activity Log mediante consultas y vistas de registros predefinidas. |
| [DNS Analytics (versión preliminar)](insights/dns-analytics.md) | Recopila, analiza y correlaciona los registros analíticos y de auditoría de Windows DNS y otros datos relacionados a partir de los servidores DNS. |
| [Cloud Foundry](../cloudfoundry/cloudfoundry-oms-nozzle.md) | Recopile, vea y analice las métricas de rendimiento y estado de sistema de Cloud Foundry entre varias implementaciones. |
| [Contenedores](insights/containers.md) | Vea y administre hosts de contenedor de Docker y Windows. |
| [Evaluaciones a petición](https://docs.microsoft.com/services-hub/health/getting_started_with_on_demand_assessments) | Evalúe y optimice la disponibilidad, la seguridad y el rendimiento de los entornos de tecnología de Microsoft locales, híbridos y en la nube. |
| [Comprobación de mantenimiento de SQL](insights/sql-assessment.md) | Evalúe el riesgo y el estado de los entornos de SQL Server.  |
| [Datos de conexión](insights/wire-data.md) | Datos consolidados de rendimiento y de red recopilados de equipos conectados a Windows y Linux con el agente de Log Analytics. |


## <a name="third-party-integration"></a>Integración de terceros

| Solución | Descripción |
|:---|:---|
| [ITSM](platform/itsmc-overview.md) | El Conector de Administración de servicios de TI (ITSMC) le permite conectar Azure y un producto o servicio de Administración de servicios de TI (ITSM) compatibles.  |


## <a name="resources-outside-of-azure"></a>Recursos fuera de Azure
Azure Monitor puede recopilar datos de recursos fuera de Azure mediante los métodos que se enumeran en la tabla siguiente.

| Resource | Método |
|:---|:---|
| APLICACIONES | Supervise las aplicaciones web fuera de Azure mediante Application Insights. Vea [¿Qué es Application Insights?](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview). |
| Máquinas virtuales | Use el agente de Log Analytics para recopilar datos del sistema operativo invitado de máquinas virtuales en otros entornos en la nube o locales. Consulte [Recopilación de datos de registro con el agente de Log Analytics](platform/log-analytics-agent.md). |
| Cliente de API de REST | Hay API independientes disponibles para escribir datos en Registros y Métricas de Azure Monitor desde cualquier cliente de API de REST. Consulte [Envío de datos de registro a Azure Monitor con HTTP Data Collector API (versión preliminar pública)](platform/data-collector-api.md) para Registros y [Envío de métricas personalizadas de un recurso de Azure al almacén de métricas de Azure Monitor mediante la API REST](platform/metrics-store-custom-rest-api.md) para Métricas. |



## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre la [plataforma de datos de Azure Monitor, que almacena los registros y las métricas recopilados por conclusiones y soluciones](platform/data-platform.md).
- Complete un [tutorial sobre la supervisión de un recurso de Azure](learn/tutorial-resource-logs.md).
- Complete un [tutorial sobre la escritura de una consulta de registro para analizar datos en Registros de Azure Monitor](learn/tutorial-resource-logs.md).
- Complete un [tutorial sobre la creación de un gráfico de métricas para analizar datos en Métricas de Azure Monitor](learn/tutorial-metrics-explorer.md).

 
