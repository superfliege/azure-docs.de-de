---
title: Integrieren einer externen Überwachungslösung mit Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Stack mit einer externen Überwachungslösung in Ihr Rechenzentrum integrieren können.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 856738a7-1510-442a-88a8-d316c67c757c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 4188d114aa86086821b2c640d7f2d98a78bcbf4e
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="integrate-external-monitoring-solution-with-azure-stack"></a>Integrieren einer externen Überwachungslösung mit Azure Stack

Für die externe Überwachung der Azure Stack-Infrastruktur müssen Sie die Azure Stack-Software, die physischen Computer und die physischen Netzwerkswitches überwachen. Jeder dieser Bereiche bietet eine Methode zum Abrufen von Integritäts- und Warnungsinformationen:

- Die Azure Stack-Software bietet eine REST-basierte API zum Abrufen der Integrität und von Warnungen. (Durch den Einsatz von softwaredefinierten Technologien wie direkte Speicherplätze sind Speicherintegrität und Warnungen Teil der Softwareüberwachung.)
- Physische Computer können Integritäts- und Warnungsinformationen über die Baseboard-Verwaltungscontroller (Baseboard Management Controller, BMCs) zur Verfügung stellen.
- Physische Netzwerkgeräte können Integritäts- und Warnungsinformationen über das SNMP-Protokoll zur Verfügung stellen.

Jede Azure Stack-Lösung wird mit einem Hardwarelebenszyklushost ausgeliefert. Auf diesem Host wird die Überwachungssoftware des OEM-Hardwareanbieters (Originalgerätehersteller) für die physischen Server und Netzwerkgeräte ausgeführt. Bei Bedarf können Sie diese Überwachungslösungen umgehen und direkt in bestehende Überwachungslösungen in Ihrem Rechenzentrum integrieren.

> [!IMPORTANT]
> Die von Ihnen verwendete externe Überwachungslösung darf keine Agenten verwenden. Sie können keine Agenten von Drittanbietern in Azure Stack-Komponenten installieren.

Das folgende Diagramm zeigt den Datenverkehrsfluss zwischen einem integrierten Azure Stack-System, dem Hardwarelebenszyklushost, einer externen Überwachungslösung und einem externen Ticketausstellungs-/Datensammlungssystem.

![Das Diagramm zeigt den Datenverkehr zwischen Azure Stack, der Überwachungs- und der Ticketausstellungslösung.](media/azure-stack-integrate-monitor/MonitoringIntegration.png)  

In diesem Artikel wird beschrieben, wie Sie Azure Stack in externe Überwachungslösungen wie System Center Operations Manager und Nagios integrieren können. Außerdem wird beschrieben, wie Sie mithilfe von PowerShell oder über REST-API-Aufrufe programmgesteuert mit Warnungen arbeiten können.

## <a name="integrate-with-operations-manager"></a>Integrieren in Operations Manager

Sie können Operations Manager für die externe Überwachung von Azure Stack verwenden. Mit dem System Center-Management Pack für Microsoft Azure Stack können Sie mehrere Azure Stack-Implementierungen mit einer einzigen Operations Manager-Instanz überwachen. Das Management Pack verwendet die REST-APIs des Integritätsressourcenanbieters und des Updateressourcenanbieters, um mit Azure Stack zu kommunizieren. Wenn Sie planen, die auf dem Hardwarelebenszyklushost ausgeführte OEM-Überwachungssoftware zu umgehen, können Sie Vendor Management Packs zur Überwachung physischer Server installieren. Sie können die Operations Manager-Netzwerkgeräteerkennung auch zur Überwachung von Netzwerkswitches verwenden.

Das Management Pack für Azure Stack bietet die folgenden Funktionen:

- Sie können mehrere Azure Stack-Bereitstellungen verwalten.
- Azure Active Directory (Azure AD) und Active Directory-Verbunddienste (AD FS) werden unterstützt.
- Sie können Warnungen abrufen und schließen.
- Es gibt jeweils ein Dashboard für Integrität und Kapazität.
- Umfasst die Erkennung des automatischen Wartungsmodus, wenn Patch und Update (P&U) ausgeführt wird.
- Umfasst Aufgaben zum Erzwingen von Updates für die Bereitstellung und die Region.
- Sie können benutzerdefinierte Informationen zu einer Region hinzufügen.
- Unterstützt die Benachrichtigung und Berichterstellung.

Sie können das System Center Management Pack für Microsoft Azure Stack und das zugehörige Benutzerhandbuch [hier](https://www.microsoft.com/en-us/download/details.aspx?id=55184) oder direkt über Operations Manager herunterladen.

Für eine Ticketausstellungslösung können Sie Operations Manager in System Center Service Manager integrieren. Der integrierte Produktconnektor ermöglicht eine bidirektionale Kommunikation, die es Ihnen ermöglicht, eine Warnung in Azure Stack und Operations Manager zu schließen, nachdem Sie eine Serviceanfrage in Service Manager aufgelöst haben.

Das folgende Diagramm zeigt die Integration von Azure Stack in eine bestehende System Center-Bereitstellung. Sie können Service Manager mit System Center Orchestrator oder Service Management Automation (SMA) weiter automatisieren, um Vorgänge in Azure Stack auszuführen.

![Das Diagramm zeigt die Integration in OM, Service Manager und SMA.](media/azure-stack-integrate-monitor/SystemCenterIntegration.png)

## <a name="integrate-with-nagios"></a>Integrieren in Nagios

Zusammen mit den Cloudbase-Partnerlösungen wurde ein Plug-In für die Nagios-Überwachung entwickelt, das unter der freien Softwarelizenz – MIT (Massachusetts Institute of Technology) – verfügbar ist.

Das Plug-In ist in Python geschrieben und nutzt die REST-API des Integritätsressourcenanbieters. Es bietet grundlegende Funktionen zum Abrufen und Schließen von Warnungen in Azure Stack. Ähnlich wie beim System Center Management Pack können Sie damit mehrere Azure Stack-Implementierungen hinzufügen und Benachrichtigungen versenden.

Das Plug-In funktioniert mit Nagios Enterprise und Nagios Core. Sie können es [hier](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)herunterladen. Die Downloadwebsite enthält auch Installations- und Konfigurationsdetails.

### <a name="plugin-parameters"></a>Plug-In-Parameter

Konfigurieren Sie die Plug-In-Datei „Azurestack_plugin.py“ mit den folgenden Parametern:

| Parameter | BESCHREIBUNG | Beispiel |
|---------|---------|---------|
| *ARM_Endpunkt* | Azure Resource Manager-Endpunkt (Administrator) |https://adminmanagement.local.azurestack.external |
| *API_Endpunkt* | Azure Resource Manager-Endpunkt (Administrator)  | https://adminmanagement.local.azurestack.external |
| *Mandanten_ID* | Admin-Abonnement-ID | Abruf über das Administratorportal oder PowerShell |
| *Benutzername* | Benutzername für Operatorabonnement | operator@myazuredirectory.onmicrosoft.com |
| *Benutzerkennwort* | Kennwort für Operatorabonnement | mypassword |
| *Client_ID* | Client | 0a7bdc5c-7b57-40be-9939-d4c5fc7cd417* |
| *Region* |  Name der Azure Stack-Region | local |
|  |  |

*Die bereitgestellte PowerShell-GUID ist universell. Sie können sie für jede Bereitstellung verwenden.

## <a name="use-powershell-to-monitor-health-and-alerts"></a>Verwenden von PowerShell zum Überwachen von Integrität und Warnungen

Wenn Sie weder Operations Manager, Nagios noch eine auf Nagios basierende Lösung verwenden, können Sie mit PowerShell eine breite Palette von Überwachungslösungen für die Integration in Azure Stack ermöglichen.
 
1. Um PowerShell zu verwenden, stellen Sie sicher, dass [PowerShell für eine Azure Stack-Operatorumgebung installiert und konfiguriert ist](azure-stack-powershell-configure-quickstart.md). Installieren Sie PowerShell auf einem lokalen Computer, der den Resource Manager-Endpunkt (Administrator) erreichen kann (https://adminmanagement.[Region].[Externer_FQDN]).

2. Führen Sie die folgenden Befehle aus, um sich mit der Azure Stack-Umgebung als Azure Stack-Operator zu verbinden:

   ```PowerShell
   Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint https://adminmanagement.[Region].[External_FQDN]

   Add-AzureRmAccount -EnvironmentName "AzureStackAdmin"
   ```
3. Wechseln Sie zum Verzeichnis, in dem Sie die [Azure Stack-Tools](https://github.com/Azure/AzureStack-Tools) als Teil der PowerShell-Installation installiert haben, z. B. „c:\azurestack-tools-master“. Wechseln Sie dann in das Infrastrukturverzeichnis, und führen Sie den folgenden Befehl aus, um das Infrastrukturmodul zu importieren:

   ```PowerShell
   Import-Module .\AzureStack.Infra.psm1
    ```
4. Verwenden Sie Befehle wie die folgenden Beispiele, um mit Warnungen zu arbeiten:
   ```PowerShell
   #Retrieve all alerts
   Get-AzsAlert -location [Region]

   #Filter for active alerts
   $Active=Get-AzsAlert -location [Region] | Where {$_.State -eq "active"}
   $Active

   #Close alert
   Close-AzsAlert -location [Region] -AlertID "ID"

   #Retrieve resource provider health
   Get-AzsResourceProviderHealths -location [Region]

   #Retrieve infrastructure role instance health
   Get-AzsInfrastructureRoleHealths -location [Region]
   ```

## <a name="use-the-rest-api-to-monitor-health-and-alerts"></a>Verwenden der REST-API zum Überwachen von Integrität und Warnungen

Sie können REST-API-Aufrufe verwenden, um Warnungen zu erhalten, Warnungen zu schließen und die Integrität von Ressourcenanbietern abzurufen.

### <a name="get-alert"></a>Abrufen von Warnungen

**Anforderung**

Die Anforderung erhält alle aktiven und geschlossenen Warnungen für das Standardanbieterabonnement. Es gibt keinen Anforderungstext.


|Methode  |Anforderungs-URI  |
|---------|---------|
|GET     |   https://{armendpoint}/subscriptions/{subId}/resourceGroups/system.{RegionName}/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/{RegionName}/Alerts?api-version=2016-05-01"      |
|     |         |

**Argumente**

|Argument  |BESCHREIBUNG  |
|---------|---------|
|armendpoint     |  Der Azure Resource Management-Endpunkt Ihrer Azure Stack-Umgebung im Format https://adminmanagement.{RegionName}.{External-FQDN}. Wenn der externe FQDN z.B. *azurestack.external* und der Regionsname *local* ist, dann ist der Resource Manager-Endpunkt https://adminmanagement.local.azurestack.external.       |
|subid     |   Abonnement-ID des Benutzers, von dem der Aufruf ausgeht Mit dieser API können Sie nur mit einem Benutzer abfragen, der über die Berechtigung für das Standardanbieterabonnement verfügt.      |
|RegionName     |    Der Name der Region der Azure Stack-Bereitstellung.     |
|api-version     |  Die Version des Protokolls, dass für diese Anforderung verwendet wird. Sie müssen 2016-05-01 verwenden.      |
|     |         |

**Antwort**

```http
GET https://adminmanagement.local.azurestack.external/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/Alerts?api-version=2016-05-01 HTTP/1.1
```

```json
{
"value":[
{"id":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/alerts/71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"name":"71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"type":"Microsoft.InfrastructureInsights.Admin/regionHealths/alerts",
"location":"local",
"tags":{},
"properties":
{
"closedTimestamp":"",
"createdTimestamp":"2017-08-10T20:13:57.4398842Z",
"description":[{"text":"The infrastructure role (Updates) is experiencing issues.",
"type":"Text"}],
"faultId":"ServiceFabric:/UpdateResourceProvider/fabric:/AzurestackUpdateResourceProvider",
"alertId":"71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"faultTypeId":"ServiceFabricApplicationUnhealthy",
"lastUpdatedTimestamp":"2017-08-10T20:18:58.1584868Z",
"alertProperties":
{
"healthState":"Warning",
"name":"Updates",
"fabricName":"fabric:/AzurestackUpdateResourceProvider",
"description":null,
"serviceType":"UpdateResourceProvider"},
"remediation":[{"text":"1. Navigate to the (Updates) and restart the role. 2. If after closing the alert the issue persists, please contact support.",
"type":"Text"}],
"resourceRegistrationId":null,
"resourceProviderRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"serviceRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"severity":"Warning",
"state":"Active",
"title":"Infrastructure role is unhealthy",
"impactedResourceId":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.Fabric.Admin/fabricLocations/local/infraRoles/UpdateResourceProvider",
"impactedResourceDisplayName":"UpdateResourceProvider",
"closedByUserAlias":null
}
},

…
```

**Antwortdetails**


|  Argument  |BESCHREIBUNG  |
|---------|---------|
|*id*     |      Eindeutige ID der Warnung.   |
|*name*     |     Interner Name der Warnung.   |
|*type*     |     Ressourcendefinition.    |
|*location*     |       Name der Region.     |
|*Tags*     |   Ressourcentags.     |
|*closedtimestamp*    |  UTC-Zeit, wann die Warnung geschlossen wurde.    |
|*createdtimestamp*     |     UTC-Zeit, wann die Warnung erstellt wurde.   |
|*description*     |    Beschreibung der Warnung.     |
|*faultid*     | Betroffene Komponente.        |
|*alertid*     |  Eindeutige ID der Warnung.       |
|*faulttypeid*     |  Eindeutiger Typ der fehlerhaften Komponente.       |
|*lastupdatedtimestamp*     |   UTC-Zeit, wann die Warnungsinformationen zuletzt aktualisiert wurden.    |
|*healthstate*     | Allgemeiner Integritätsstatus.        |
|*name*     |   Name der bestimmten Warnung.      |
|*fabricname*     |    Registrierter Fabric-Name der fehlerhaften Komponente.   |
|*description*     |  Beschreibung der registrierten Fabric-Komponente.   |
|*servicetype*     |   Typ des registrierten Fabric-Diensts.   |
|*remediation*     |   Empfohlene Schritte zur Wiederherstellung.    |
|*type*     |   Warnungstyp.    |
|*resourceRegistrationid*    |     ID der betroffenen registrierten Ressource.    |
|*resourceProviderRegistrationID*   |    ID des registrierten Ressourcenanbieters der betroffenen Komponente.  |
|*serviceregistrationid*     |    ID des registrierten Diensts.   |
|*severity*     |     Schweregrad der Warnung.  |
|*state*     |    Zustand der Warnung.   |
|*title*     |    Titel der Warnung.   |
|*impactedresourceid*     |     ID der betroffenen Ressource.    |
|*ImpactedresourceDisplayName*     |     Name der betroffenen Ressource.  |
|*closedByUserAlias*     |   Benutzer, der die Warnung geschlossen hat.      |

### <a name="close-alert"></a>Schließen von Warnungen

**Anforderung**

Die Anforderung schließt eine Warnung über ihre eindeutige ID.

|Methode    |Anforderungs-URI  |
|---------|---------|
|PUT     |   https://{armendpoint}/subscriptions/{subId}/resourceGroups/system.{RegionName}/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/{RegionName}/Alerts/alertid?api-version=2016-05-01"    |

**Argumente**


|Argument  |BESCHREIBUNG  |
|---------|---------|
|*armendpunkt*     |   Resource Management-Endpunkt Ihrer Azure Stack-Umgebung im Format https://adminmanagement.{RegionName}.{External-FQDN}. Wenn der externe FQDN z.B. *azurestack.external* und der Regionsname *local* ist, dann ist der Resource Manager-Endpunkt https://adminmanagement.local.azurestack.external.      |
|*subid*     |    Abonnement-ID des Benutzers, von dem der Aufruf ausgeht Mit dieser API können Sie nur mit einem Benutzer abfragen, der über die Berechtigung für das Standardanbieterabonnement verfügt.     |
|*RegionName*     |   Der Name der Region der Azure Stack-Bereitstellung.      |
|*api-version*     |    Die Version des Protokolls, dass für diese Anforderung verwendet wird. Sie müssen 2016-05-01 verwenden.     |
|*alertid*     |    Eindeutige ID der Warnung.     |

**Text**

```json

{
"value":[
{"id":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/alerts/71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"name":"71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"type":"Microsoft.InfrastructureInsights.Admin/regionHealths/alerts",
"location":"local",
"tags":{},
"properties":
{
"closedTimestamp":"2017-08-10T20:18:58.1584868Z",
"createdTimestamp":"2017-08-10T20:13:57.4398842Z",
"description":[{"text":"The infrastructure role (Updates) is experiencing issues.",
"type":"Text"}],
"faultId":"ServiceFabric:/UpdateResourceProvider/fabric:/AzurestackUpdateResourceProvider",
"alertId":"71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"faultTypeId":"ServiceFabricApplicationUnhealthy",
"lastUpdatedTimestamp":"2017-08-10T20:18:58.1584868Z",
"alertProperties":
{
"healthState":"Warning",
"name":"Updates",
"fabricName":"fabric:/AzurestackUpdateResourceProvider",
"description":null,
"serviceType":"UpdateResourceProvider"},
"remediation":[{"text":"1. Navigate to the (Updates) and restart the role. 2. If after closing the alert the issue persists, please contact support.",
"type":"Text"}],
"resourceRegistrationId":null,
"resourceProviderRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"serviceRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"severity":"Warning",
"state":"Closed",
"title":"Infrastructure role is unhealthy",
"impactedResourceId":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.Fabric.Admin/fabricLocations/local/infraRoles/UpdateResourceProvider",
"impactedResourceDisplayName":"UpdateResourceProvider",
"closedByUserAlias":null
}
},
```
**Antwort**

```http
PUT https://adminmanagement.local.azurestack.external//subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/alerts/71dbd379-1d1d-42e2-8439-6190cc7aa80b?api-version=2016-05-01 HTTP/1.1
```

```json
{
"value":[
{"id":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/alerts/71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"name":"71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"type":"Microsoft.InfrastructureInsights.Admin/regionHealths/alerts",
"location":"local",
"tags":{},
"properties":
{
"closedTimestamp":"",
"createdTimestamp":"2017-08-10T20:13:57.4398842Z",
"description":[{"text":"The infrastructure role (Updates) is experiencing issues.",
"type":"Text"}],
"faultId":"ServiceFabric:/UpdateResourceProvider/fabric:/AzurestackUpdateResourceProvider",
"alertId":"71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"faultTypeId":"ServiceFabricApplicationUnhealthy",
"lastUpdatedTimestamp":"2017-08-10T20:18:58.1584868Z",
"alertProperties":
{
"healthState":"Warning",
"name":"Updates",
"fabricName":"fabric:/AzurestackUpdateResourceProvider",
"description":null,
"serviceType":"UpdateResourceProvider"},
"remediation":[{"text":"1. Navigate to the (Updates) and restart the role. 2. If after closing the alert the issue persists, please contact support.",
"type":"Text"}],
"resourceRegistrationId":null,
"resourceProviderRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"serviceRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"severity":"Warning",
"state":"Closed",
"title":"Infrastructure role is unhealthy",
"impactedResourceId":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.Fabric.Admin/fabricLocations/local/infraRoles/UpdateResourceProvider",
"impactedResourceDisplayName":"UpdateResourceProvider",
"closedByUserAlias":null
}
},
```

**Antwortdetails**


|  Argument  |BESCHREIBUNG  |
|---------|---------|
|*id*     |      Eindeutige ID der Warnung.   |
|*name*     |     Interner Name der Warnung.   |
|*type*     |     Ressourcendefinition.    |
|*location*     |       Name der Region.     |
|*Tags*     |   Ressourcentags.     |
|*closedtimestamp*    |  UTC-Zeit, wann die Warnung geschlossen wurde.    |
|*createdtimestamp*     |     UTC-Zeit, wann die Warnung erstellt wurde.   |
|*Beschreibung*     |    Beschreibung der Warnung.     |
|*faultid*     | Betroffene Komponente.        |
|*alertid*     |  Eindeutige ID der Warnung.       |
|*faulttypeid*     |  Eindeutiger Typ der fehlerhaften Komponente.       |
|*lastupdatedtimestamp*     |   UTC-Zeit, wann die Warnungsinformationen zuletzt aktualisiert wurden.    |
|*healthstate*     | Allgemeiner Integritätsstatus.        |
|*name*     |   Name der bestimmten Warnung.      |
|*fabricname*     |    Registrierter Fabric-Name der fehlerhaften Komponente.   |
|*description*     |  Beschreibung der registrierten Fabric-Komponente.   |
|*servicetype*     |   Typ des registrierten Fabric-Diensts.   |
|*remediation*     |   Empfohlene Schritte zur Wiederherstellung.    |
|*type*     |   Warnungstyp.    |
|*resourceRegistrationid*    |     ID der betroffenen registrierten Ressource.    |
|*resourceProviderRegistrationID*   |    ID des registrierten Ressourcenanbieters der betroffenen Komponente.  |
|*serviceregistrationid*     |    ID des registrierten Diensts.   |
|*severity*     |     Schweregrad der Warnung.  |
|*state*     |    Zustand der Warnung.   |
|*title*     |    Titel der Warnung.   |
|*impactedresourceid*     |     ID der betroffenen Ressource.    |
|*ImpactedresourceDisplayName*     |     Name der betroffenen Ressource.  |
|*closedByUserAlias*     |   Benutzer, der die Warnung geschlossen hat.      |

### <a name="get-resource-provider-health"></a>Abrufen der Integrität des Ressourcenanbieters

**Anforderung**

Die Anforderung erhält den Integritätsstatus für alle registrierten Ressourcenanbieter.


|Methode  |Anforderungs-URI  |
|---------|---------|
|GET    |   https://{armendpoint}/subscriptions/{subId}/resourceGroups/system.{RegionName}/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/{RegionName}/serviceHealths?api-version=2016-05-01"   |


**Argumente**


|Argumente  |BESCHREIBUNG  |
|---------|---------|
|*armendpunkt*     |    Der Resource Management-Endpunkt Ihrer Azure Stack-Umgebung im Format https://adminmanagement.{RegionName}.{External-FQDN}. Wenn der externe FQDN z.B. „azurestack.external“ und der Regionsname „local“ ist, dann ist der Resource Manager-Endpunkt https://adminmanagement.local.azurestack.external.     |
|*subid*     |     Abonnement-ID des Benutzers, von dem der Aufruf ausgeht Mit dieser API können Sie nur mit einem Benutzer abfragen, der über die Berechtigung für das Standardanbieterabonnement verfügt.    |
|*RegionName*     |     Der Name der Region der Azure Stack-Bereitstellung.    |
|*api-version*     |   Die Version des Protokolls, dass für diese Anforderung verwendet wird. Sie müssen 2016-05-01 verwenden.      |


**Antwort**

```http
GET https://adminmanagement.local.azurestack.external/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/serviceHealths?api-version=2016-05-01
```

```json
{
"value":[
{
"id":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/serviceHealths/03ccf38f-f6b1-4540-9dc8-ec7b6389ecca",
"name":"03ccf38f-f6b1-4540-9dc8ec7b6389ecca",
"type":"Microsoft.InfrastructureInsights.Admin/regionHealths/serviceHealths",
"location":"local",
"tags":{},
"properties":{
"registrationId":"03ccf38f-f6b1-4540-9dc8-ec7b6389ecca",
"displayName":"Key Vault",
"namespace":"Microsoft.KeyVault.Admin",
"routePrefix":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.KeyVault.Admin/locations/local",
"serviceLocation":"local",
"infraURI":"/subscriptions/4aa97de3-6b83-4582-86e1-65a5e4d1295b/resourceGroups/system.local/providers/Microsoft.KeyVault.Admin/locations/local/infraRoles/Key Vault",
"alertSummary":{"criticalAlertCount":0,"warningAlertCount":0},
"healthState":"Healthy"
}
}

…
```
**Antwortdetails**


|Argument  |BESCHREIBUNG  |
|---------|---------|
|*Id*     |   Eindeutige ID der Warnung.      |
|*name*     |  Interner Name der Warnung.       |
|*type*     |  Ressourcendefinition.       |
|*location*     |  Name der Region.       |
|*Tags*     |     Ressourcentags.    |
|*registrationId*     |   Eindeutige Registrierung für den Ressourcenanbieter.      |
|*displayName*     |Anzeigename des Ressourcenanbieters.        |
|*namespace*     |   Vom Ressourcenanbieter implementierter API-Namespace.       |
|*routePrefix*     |    URI für die Interaktion mit dem Ressourcenanbieter.     |
|*serviceLocation*     |   Region, mit der dieser Ressourcenanbieter registriert ist.      |
|*infraURI*     |   URI des Ressourcenanbieters, der als Infrastrukturrolle aufgeführt ist.      |
|*alertSummary*     |   Zusammenfassung der kritischen und allgemeinen Warnungen, die dem Ressourcenanbieter zugeordnet sind.      |
|*healthState*     |    Integritätsstatus des Ressourcenanbieters.     |


### <a name="get-resource-health"></a>Abrufen der Ressourcenintegrität

Die Anforderung ruft den Integritätsstatus für einen bestimmten registrierten Ressourcenanbieter ab.

**Anforderung**

|Methode  |Anforderungs-URI  |
|---------|---------|
|GET     |     https://{armendpoint}/subscriptions/{subId}/resourceGroups/system.{RegionName}/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/{RegionName}/serviceHealths/{RegistrationID}/resourceHealths?api-version=2016-05-01"    |

**Argumente**

|Argumente  |BESCHREIBUNG  |
|---------|---------|
|*armendpunkt*     |    Der Resource Management-Endpunkt Ihrer Azure Stack-Umgebung im Format https://adminmanagement.{RegionName}.{External-FQDN}. Wenn der externe FQDN z.B. „azurestack.external“ und der Regionsname „local“ ist, dann ist der Resource Manager-Endpunkt https://adminmanagement.local.azurestack.external.     |
|*subid*     |Abonnement-ID des Benutzers, von dem der Aufruf ausgeht Mit dieser API können Sie nur mit einem Benutzer abfragen, der über die Berechtigung für das Standardanbieterabonnement verfügt.         |
|*RegionName*     |  Der Name der Region der Azure Stack-Bereitstellung.       |
|*api-version*     |  Die Version des Protokolls, dass für diese Anforderung verwendet wird. Sie müssen 2016-05-01 verwenden.       |
|*RegistrationID* |Registrierungs-ID für einen bestimmten Ressourcenanbieter. |

**Antwort**

```http
GET https://adminmanagement.local.azurestack.external/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/serviceHealths/03ccf38f-f6b1-4540-9dc8-ec7b6389ecca /resourceHealths?api-version=2016-05-01 HTTP/1.1
```

```json
{
"value":
[
{"id":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/serviceHealths/472aaaa6-3f63-43fa-a489-4fd9094e235f/resourceHealths/028c3916-ab86-4e7f-b5c2-0468e607915c",
"name":"028c3916-ab86-4e7f-b5c2-0468e607915c",
"type":"Microsoft.InfrastructureInsights.Admin/regionHealths/serviceHealths/resourceHealths",
"location":"local",
"tags":{},
"properties":
{"registrationId":"028c3916-ab86-4e7f-b5c2 0468e607915c","namespace":"Microsoft.Fabric.Admin","routePrefix":"/subscriptions/4aa97de3-6b83-4582-86e1 65a5e4d1295b/resourceGroups/system.local/providers/Microsoft.Fabric.Admin/fabricLocations/local",
"resourceType":"infraRoles",
"resourceName":"Privileged endpoint",
"usageMetrics":[],
"resourceLocation":"local",
"resourceURI":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.Fabric.Admin/fabricLocations/local/infraRoles/Privileged endpoint",
"rpRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"alertSummary":{"criticalAlertCount":0,"warningAlertCount":0},"healthState":"Unknown"
}
}
…
```

**Antwortdetails**

|Argument  |BESCHREIBUNG  |
|---------|---------|
|*Id*     |   Eindeutige ID der Warnung.      |
|*name*     |  Interner Name der Warnung.       |
|*type*     |  Ressourcendefinition.       |
|*location*     |  Name der Region.       |
|*Tags*     |     Ressourcentags.    |
|*registrationId*     |   Eindeutige Registrierung für den Ressourcenanbieter.      |
|*resourceType*     |Ressourcentyp.        |
|*resourceName*     |   Name der Ressource.   |
|*usageMetrics*     |    Nutzungsmetrik für Ressource.     |
|*resourceLocation*     |   Name der Region, sofern bereitgestellt.      |
|*resourceURI*     |   URI für die Ressource.   |
|*alertSummary*     |   Zusammenfassung von kritischen und allgemeinen Warnmeldungen sowie Integritätsstatus.     |

## <a name="learn-more"></a>Weitere Informationen

Informationen zur integrierten Integritätsüberwachung finden Sie unter [Überwachen von Integrität und Warnungen in Azure Stack](azure-stack-monitor-health.md).


## <a name="next-steps"></a>Nächste Schritte

[Integrieren von Azure Stack-Sicherheitsfunktionen in Datencenter-Sicherheitslösungen](azure-stack-integrate-security.md)
