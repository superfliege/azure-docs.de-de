---
title: Azure Cloud Services-Definitionsschema (CSDEF-Datei) | Microsoft-Dokumentation
ms.custom: 
ms.date: 04/14/2015
ms.prod: azure
ms.reviewer: 
ms.service: cloud-services
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: reference
ms.assetid: b7735dbf-8e91-4d1b-89f7-2f17e9302469
caps.latest.revision: "42"
author: thraka
ms.author: adegeo
manager: timlt
ms.openlocfilehash: b833fdc06e4193c1b478028733c336feb6d8b9ba
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cloud-services-definition-schema-csdef-file"></a>Azure Cloud Services-Definitionsschema (.CSDEF-Datei)
Die Dienstdefinitionsdatei definiert das Dienstmodell für eine Anwendung. Die Datei enthält die Definitionen für die Rollen, die für einen Clouddienst verfügbar sind, gibt die Anbieterendpunkte an und legt Konfigurationseinstellungen für den Dienst fest. Konfigurationseinstellungswerte werden in der Dienstkonfigurationsdatei festgelegt, wie im [Clouddienst-Konfigurationsschema (klassisch)](http://msdn.microsoft.com/library/b1ae68cd-cc95-48cb-a4a4-da91dc708a35) beschrieben.

Standardmäßig wird die Konfigurationsschemadatei der Azure-Diagnose im `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas`-Verzeichnis installiert. Ersetzen Sie `<version>` durch die installierte Version des [Azure SDK](http://www.windowsazure.com/develop/downloads/).

Die Standarderweiterung für die Dienstdefinitionsdatei lautet .csdef.

## <a name="basic-service-definition-schema"></a>Basisdienstdefinitionsschema
Die Dienstdefinitionsdatei muss ein `ServiceDefinition`-Element enthalten. Die Dienstdefinition muss mindestens ein Rollenelement enthalten (`WebRole` oder `WorkerRole`). Sie kann bis zu 25 Rollen enthalten, die in einer Definition definiert sind. Rollentypen können miteinander kombiniert werden. Zudem enthält die Dienstdefinition das optionale Element `NetworkTrafficRules`, mit dem eingeschränkt werden kann, welche Rollen mit angegebenen internen Endpunkten kommunizieren können. Die Dienstdefinition enthält auch das optionale Element `LoadBalancerProbes`, das vom Kunden definierte Integritätstests von Endpunkten enthält.

Das Standardformat der Dienstdefinitionsdatei sieht wie folgt aus.

```xml
<ServiceDefinition name="<service-name>" topologyChangeDiscovery="<change-type>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" upgradeDomainCount="<number-of-upgrade-domains>" schemaVersion="<version>">
  
  <LoadBalancerProbes>
         …
  </LoadBalancerProbes>
  
  <WebRole …>
         …
  </WebRole>
  
  <WorkerRole …>
         …
  </WorkerRole>
  
  <NetworkTrafficRules>
         …
  </NetworkTrafficRules>

</ServiceDefinition>
```

## <a name="schema-definitions"></a>Schemadefinitionen
In den folgenden Themen wird das Schema beschrieben:

- [LoadBalancerProbe Schema](schema-csdef-loadbalancerprobe.md) (LoadBalancerProbe-Schema)
- [WebRole Schema](schema-csdef-webrole.md) (WebRole-Schema)
- [WorkerRole Schema](schema-csdef-workerrole.md) (WorkerRole-Schema)
- [NetworkTrafficRules Schema](schema-csdef-networktrafficrules.md) (NetworkTrafficRules-Schema)

##  <a name="ServiceDefinition"></a> Element ServiceDefinition
Das Element `ServiceDefinition` ist das Element der obersten Ebene der Dienstdefinitionsdatei.

In der folgenden Tabelle werden die Attribute des Elements `ServiceDefinition` beschrieben.

| Attribut               | Beschreibung |
| ----------------------- | ----------- |
| Name                    |Erforderlich. Der Name des Diensts. Der Name muss innerhalb des Dienstkontos eindeutig sein.|
| topologyChangeDiscovery | Optional. Gibt die Änderungsbenachrichtigung zur Art der Topologie an. Mögliche Werte:<br /><br /> -   `Blast`: Sendet das Update so schnell wie möglich an alle Rolleninstanzen. Wenn Sie die Option auswählen, sollte die Rolle das Topologieupdate verarbeiten können, ohne neu gestartet werden zu müssen.<br />-   `UpgradeDomainWalk`: Sendet das Update sequenziell an alle Rolleninstanzen, nachdem die vorherige Instanz das Update erfolgreich akzeptiert hat.|
| Schemaversion           | Optional. Gibt die Version des Dienstdefinitionsschemas an. Die Schemaversion ermöglicht Visual Studio, die richtigen SDK-Tools für die Schemaüberprüfung auszuwählen, wenn mehrere Versionen des SDK nebeneinander installiert sind.|
| upgradeDomainCount      | Optional. Gibt die Anzahl von Upgradedomänen an, über die die Rollen in diesem Dienst zugeordnet werden. Rolleninstanzen werden bei der Bereitstellung des Diensts einer Upgradedomäne zugeordnet. Weitere Informationen finden Sie unter [Aktualisieren einer Clouddienstrolle oder Bereitstellung](cloud-services-how-to-manage-portal.md#how-to-update-a-cloud-service-role-or-deployment).<br /><br /> Sie können bis zu 20 Upgradedomänen angeben. Bei fehlender Angabe wird der Standardwert in Höhe von 5 Upgradedomänen verwendet.|