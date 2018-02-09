---
title: "Azure-Diagnoseprotokolle – unterstützte Dienste, Schemas und Kategorien | Microsoft-Dokumentation"
description: "Erläuterung der unterstützten Dienste und Ereignisschemas für Azure-Diagnose-Protokolle."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: fe8887df-b0e6-46f8-b2c0-11994d28e44f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/24/2018
ms.author: johnkem
ms.openlocfilehash: 3e2b5305b969c96e6b14122af03da9249373094a
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2018
---
# <a name="supported-services-schemas-and-categories-for-azure-diagnostic-logs"></a>Unterstützte Dienste, Schemas und Kategorien für Azure-Diagnoseprotokolle

[Azure-Ressourcendiagnoseprotokolle](monitoring-overview-of-diagnostic-logs.md) sind von Ihren Azure-Ressourcen ausgegebene Protokolle, die den Betrieb der jeweiligen Ressource beschreiben. Diese Protokolle sind ressourcentypspezifisch. In diesem Artikel beschreiben wir den Satz von unterstützten Diensten und Ereignisschemas für Ereignisse, der vom jeweiligen Dienst ausgegeben wird. Dieser Artikel enthält auch eine vollständige Liste der pro Ressourcentyp verfügbaren Protokollkategorien.

## <a name="supported-services-and-schemas-for-resource-diagnostic-logs"></a>Unterstützte Dienste und Schemas für Ressourcendiagnoseprotokolle
Das Schema für Diagnoseprotokolle für Ressourcen variiert abhängig von der Ressource und der Protokollkategorie.   

| Dienst | Schema und Dokumente |
| --- | --- |
| Analysis Services | Schema nicht verfügbar. |
| API Management | [API Management-Diagnoseprotokolle](../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| Anwendungsgateways |[Diagnoseprotokollierung für Application Gateway](../application-gateway/application-gateway-diagnostics.md) |
| Azure-Automatisierung |[Protokollanalysen für Azure Automation](../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Diagnoseprotokolle für Azure Batch](../batch/batch-diagnostics.md) |
| Customer Insights | Schema nicht verfügbar. |
| Content Delivery Network | Schema nicht verfügbar. |
| CosmosDB | [Azure Cosmos DB-Protokollierung](../cosmos-db/logging.md) |
| Data Lake Analytics |[Zugreifen auf Diagnoseprotokolle für Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake-Speicher |[Zugreifen auf Diagnoseprotokolle für Azure Data Lake Store](../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Event Hubs |[Azure Event Hubs-Diagnoseprotokolle](../event-hubs/event-hubs-diagnostic-logs.md) |
| IoT Hub | [IoT Hub-Vorgänge](../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| Schlüsseltresor |[Azure-Schlüsseltresor-Protokollierung](../key-vault/key-vault-logging.md) |
| Lastenausgleichsmodul |[Protokollanalysen für den Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Benutzerdefiniertes Logic Apps-B2B-Nachverfolgungsschema](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Netzwerksicherheitsgruppen |[Protokollanalysen für Netzwerksicherheitsgruppen (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md) |
| DDoS-Schutz | Schema nicht verfügbar. |
| Recovery Services | [Datenmodell für Azure Backup](../backup/backup-azure-reports-data-model.md)|
| Suchen, |[Aktivieren und Verwenden von „Datenverkehrsanalyse durchsuchen“](../search/search-traffic-analytics.md) |
| Server Management | Schema nicht verfügbar. |
| SERVICE BUS |[Azure Service Bus Diagnoseprotokolle](../service-bus-messaging/service-bus-diagnostic-logs.md) |
| SQL-Datenbank | [Azure SQL-Datenbank-Diagnoseprotokollierung](../sql-database/sql-database-metrics-diag-logging.md) |
| Stream Analytics |[Auftragsdiagnoseprotokolle](../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Virtuelle Netzwerke | Schema nicht verfügbar. |

## <a name="supported-log-categories-per-resource-type"></a>Unterstützte Protokollkategorien pro Ressourcentyp
|Ressourcentyp|Category (Kategorie)|Anzeigename der Kategorie|
|---|---|---|
|Microsoft.AnalysisServices/servers|Motor|Motor|
|Microsoft.AnalysisServices/servers|Dienst|Dienst|
|Microsoft.ApiManagement/service|GatewayLogs|Protokolle im Zusammenhang mit dem ApiManagement-Gateway|
|Microsoft.Automation/automationAccounts|JobLogs|Auftragsprotokolle|
|Microsoft.Automation/automationAccounts|JobStreams|Auftragsdatenströme|
|Microsoft.Automation/automationAccounts|DscNodeStatus|DSC-Knotenstatus|
|Microsoft.Batch/batchAccounts|ServiceLog|Dienstprotokolle|
|Microsoft.Cdn/profiles/endpoints|CoreAnalytics|Ruft die Metriken des Endpunkts ab, z.B. Bandbreite, ausgehenden Datenverkehr usw.|
|Microsoft.CustomerInsights/hubs|AuditEvents|AuditEvents|
|Microsoft.DataFactory/factories|ActivityRuns|Pipeline-Aktivitätsausführungsprotokoll|
|Microsoft.DataFactory/factories|PipelineRuns|Pipelineausführungsprotokoll|
|Microsoft.DataFactory/factories|TriggerRuns|Triggerausführungsprotokoll|
|Microsoft.DataLakeAnalytics/accounts|Audit|Überwachungsprotokolle|
|Microsoft.DataLakeAnalytics/accounts|Requests|Anforderungsprotokolle|
|Microsoft.DataLakeStore/accounts|Audit|Überwachungsprotokolle|
|Microsoft.DataLakeStore/accounts|Requests|Anforderungsprotokolle|
|Microsoft.Devices/IotHubs|Verbindungen|Verbindungen|
|Microsoft.Devices/IotHubs|DeviceTelemetry|Gerätetelemetrie|
|Microsoft.Devices/IotHubs|C2DCommands|C2D-Befehle|
|Microsoft.Devices/IotHubs|DeviceIdentityOperations|Geräteidentitätsvorgänge|
|Microsoft.Devices/IotHubs|FileUploadOperations|Dateiuploadvorgänge|
|Microsoft.Devices/IotHubs|Routen|Routen|
|Microsoft.Devices/IotHubs|D2CTwinOperations|D2CTwinOperations|
|Microsoft.Devices/IotHubs|C2DTwinOperations|C2D-Zwillingsvorgänge|
|Microsoft.Devices/IotHubs|TwinQueries|Zwillingsabfragen|
|Microsoft.Devices/IotHubs|JobsOperations|Auftragsvorgänge|
|Microsoft.Devices/IotHubs|DirectMethods|Direkte Methoden|
|Microsoft.Devices/provisioningServices|DeviceOperations|Gerätevorgänge|
|Microsoft.Devices/provisioningServices|ServiceOperations|Dienstoperationen|
|Microsoft.DocumentDB/databaseAccounts|DataPlaneRequests|DataPlaneRequests|
|Microsoft.DocumentDB/databaseAccounts|MongoRequests|MongoRequests|
|Microsoft.EventHub/namespaces|ArchiveLogs|Archivprotokolle|
|Microsoft.EventHub/namespaces|OperationalLogs|Betriebsprotokolle|
|Microsoft.EventHub/namespaces|AutoScaleLogs|Protokolle zur automatischen Skalierung|
|Microsoft.KeyVault/vaults|AuditEvent|Überwachungsprotokolle|
|Microsoft.Logic/workflows|WorkflowRuntime|Diagnoseereignisse zur Workflowlaufzeit|
|Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|Integrationskonto –Nachverfolgen von Ereignissen|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Ereignis der Netzwerksicherheitsgruppe|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Regelzähler der Netzwerksicherheitsgruppe|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupFlowEvent|Regelflussereignis der Netzwerksicherheitsgruppe|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Load Balancer-Warnereignisse|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Integritätsstatus der Load Balancer-Stichprobe|
|Microsoft.Network/publicIPAddresses|DDoSProtectionNotifications|DDoS-Schutz-Benachrichtigungen|
|Microsoft.Network/virtualNetworks|VMProtectionAlerts|VM-Schutz-Warnungen|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Application Gateway-Zugriffsprotokoll|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Application Gateway-Leistungsprotokoll|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Application Gateway-Firewallprotokoll|
|Microsoft.Network/virtualNetworkGateways|GatewayDiagnosticLog|Gatewaydiagnoseprotokolle|
|Microsoft.Network/virtualNetworkGateways|TunnelDiagnosticLog|Tunneldiagnoseprotokolle|
|Microsoft.Network/virtualNetworkGateways|RouteDiagnosticLog|Routendiagnoseprotokolle|
|Microsoft.Network/trafficManagerProfiles|ProbeHealthStatusEvents|Traffic Manager-Testintegritätsergebnisse (Ereignis)|
|Microsoft.Network/expressRouteCircuits|GWMCountersTable|Tabelle der GWM-Leistungsindikatoren|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Azure Backup-Berichtsdaten|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryJobs|Azure Site Recovery-Aufträge|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryEvents|Azure Site Recovery-Ereignisse|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Replizierte Azure Site Recovery-Elemente|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationStats|Azure Site Recovery-Replikationsstatistiken|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryRecoveryPoints|Azure Site Recovery-Wiederherstellungspunkte|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationDataUploadRate|Uploadrate für Azure Site Recovery-Replikationsdaten|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryProtectedDiskDataChurn|Datenänderungen auf mit Azure Site Recovery geschützten Datenträgern|
|Microsoft.Search/searchServices|OperationLogs|Vorgangsprotokolle|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Betriebsprotokolle|
|Microsoft.Sql/servers/databases|QueryStoreRuntimeStatistics|Laufzeitstatistik des Abfragespeichers|
|Microsoft.Sql/servers/databases|QueryStoreWaitStatistics|Wartestatistik des Abfragespeichers|
|Microsoft.Sql/servers/databases|Errors|Errors|
|Microsoft.Sql/servers/databases|DatabaseWaitStatistics|Wartestatistik der Datenbank|
|Microsoft.Sql/servers/databases|Zeitlimits|Zeitlimits|
|Microsoft.Sql/servers/databases|Blöcke|Blöcke|
|Microsoft.Sql/servers/databases|SQLInsights|SQL-Informationen|
|Microsoft.Sql/servers/databases|Audit|Überwachungsprotokolle|
|Microsoft.StreamAnalytics/streamingjobs|Ausführung|Ausführung|
|Microsoft.StreamAnalytics/streamingjobs|Erstellen|Erstellen|

## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zu Diagnoseprotokollen](monitoring-overview-of-diagnostic-logs.md)
* [Streamen von Diagnoseprotokollen für Ressourcen an **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* Ändern der Diagnoseeinstellungen für Ressourcen mithilfe der Azure Monitor-REST-API ([Service Diagnostic Settings](https://msdn.microsoft.com/library/azure/dn931931.aspx) (Diagnoseeinstellungen für Dienste))
* [Analysieren von Protokollen aus Azure Storage mit Log Analytics](../log-analytics/log-analytics-azure-storage.md)
