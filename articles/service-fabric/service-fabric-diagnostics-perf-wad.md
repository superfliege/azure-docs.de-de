---
title: 'Azure Service Fabric: Leistungsüberwachung mit der Microsoft Azure-Diagnoseerweiterung | Microsoft-Dokumentation'
description: Verwenden Sie die Microsoft Azure-Diagnose, um Leistungsindikatoren für Ihre Azure Service Fabric-Cluster zu erfassen.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/26/2018
ms.author: srrengar
ms.openlocfilehash: f99206fe673f69c78bf130026207ed58344ccea5
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39324424"
---
# <a name="performance-monitoring-with-the-windows-azure-diagnostics-extension"></a>Leistungsüberwachung mit der Microsoft Azure-Diagnoseerweiterung

In diesem Artikel erfahren Sie, wie Sie die Erfassung von Leistungsindikatoren über die Microsoft Azure-Diagnoseerweiterung für Windows-Cluster einrichten. Für Linux-Cluster muss der [OMS-Agent](service-fabric-diagnostics-oms-agent.md) eingerichtet werden, um Leistungsindikatoren für die Knoten zu erfassen. 

 > [!NOTE]
> Die Microsoft Azure-Diagnoseerweiterung muss in Ihrem Cluster bereitgestellt sein, damit diese Schritte funktionieren. Falls sie nicht eingerichtet ist, helfen Ihnen die Informationen unter [Ereignisaggregation und -sammlung mit der Microsoft Azure-Diagnose](service-fabric-diagnostics-event-aggregation-wad.md) weiter.  

## <a name="collect-performance-counters-via-the-wadcfg"></a>Erfassen von Leistungsindikatoren über WadCfg

Um Leistungsindikatoren über die Microsoft Azure-Diagnoseerweiterung erfassen zu können, müssen Sie die Konfiguration in der Resource Manager-Vorlage Ihres Clusters entsprechend ändern. Führen Sie die folgenden Schritte aus, um Ihrer Vorlage einen zu erfassenden Leistungsindikator hinzuzufügen und ein Upgrade der Resource Manager-Ressource durchzuführen.

1. Suchen Sie in der Vorlage Ihres Clusters die Konfiguration der Microsoft Azure-Diagnoseerweiterung (`WadCfg`). Hier werden zu erfassende Leistungsindikatoren unter `DiagnosticMonitorConfiguration` hinzugefügt.

2. Fügen Sie `DiagnosticMonitorConfiguration` den folgenden Abschnitt hinzu, um in Ihrer Konfiguration die Erfassung von Leistungsindikatoren einzurichten: 

    ```json
    "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": []
    }
    ```

    `scheduledTransferPeriod` definiert, wie häufig die Werte der zu erfassenden Leistungsindikatoren an Ihre Azure-Speichertabelle sowie an ggf. konfigurierte Senken übertragen werden sollen. 

3. Fügen Sie `PerformanceCounterConfiguration` (wurde im vorherigen Schritt deklariert) die Leistungsindikatoren hinzu, die erfasst werden sollen. Jeder zu erfassende Leistungsindikator wird mit `counterSpecifier`, `sampleRate`, `unit`, `annotation` sowie mit allen relevanten `sinks` definiert.

Hier ist ein Beispiel für eine Konfiguration mit dem Leistungsindikator für *Total Processor Time* (Prozessorzeit gesamt; Dauer der Nutzung der CPU für Verarbeitungsvorgänge) und *Service Fabric Actor Method Invocations per Second* (Service Fabric Actor-Methode – Aufrufe pro Sekunde; einer der benutzerdefinierten Service Fabric-Leistungsindikatoren) angegeben. Eine vollständige Liste mit benutzerdefinierten Service Fabric-Leistungsindikatoren finden Sie in den Abschnitten zu [Reliable Actors-Leistungsindikatoren](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters) und [Reliable Services-Leistungsindikatoren](service-fabric-reliable-serviceremoting-diagnostics.md#list-of-performance-counters).

 ```json
 "WadCfg": {
         "DiagnosticMonitorConfiguration": {
           "overallQuotaInMB": "50000",
           "EtwProviders": {
             "EtwEventSourceProviderConfiguration": [
               {
                 "provider": "Microsoft-ServiceFabric-Actors",
                 "scheduledTransferKeywordFilter": "1",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricReliableActorEventTable"
                 }
               },
               {
                 "provider": "Microsoft-ServiceFabric-Services",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricReliableServiceEventTable"
                 }
               }
             ],
             "EtwManifestProviderConfiguration": [
               {
                 "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                 "scheduledTransferLogLevelFilter": "Information",
                 "scheduledTransferKeywordFilter": "4611686018427387904",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricSystemEventTable"
                 }
               }
             ]
           },
           "PerformanceCounters": {
                 "scheduledTransferPeriod": "PT1M",
                 "PerformanceCounterConfiguration": [
                     {
                         "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                         "sampleRate": "PT1M",
                         "unit": "Percent",
                         "annotation": [
                         ],
                         "sinks": ""
                     },
                     {
                         "counterSpecifier": "\\Service Fabric Actor Method(*)\\Invocations/Sec",
                         "sampleRate": "PT1M",
                     }
                 ]
             }
         }
       },
  ```

 Die Samplingrate für den Leistungsindikator kann gemäß Ihren Anforderungen angepasst werden. Das Format lautet `PT<time><unit>`. Wenn der Leistungsindikator also beispielsweise sekündlich erfasst werden soll, legen Sie Folgendes fest: `"sampleRate": "PT15S"`.

 >[!NOTE]
 >Mithilfe von `*` können Sie zwar Gruppen von Leistungsindikatoren mit ähnlichem Namen angeben, um Leistungsindikatoren jedoch über eine Senke (an Application Insights) senden zu können, müssen sie einzeln deklariert werden. 

4. Nach dem Hinzufügen der zu erfassenden Leistungsindikatoren müssen Sie Ihre Clusterressource upgraden, damit die Änderungen in Ihrem ausgeführten Cluster übernommen werden. Speichern Sie Ihre Änderungen an `template.json`, und öffnen Sie PowerShell. Das Upgrade des Clusters kann mithilfe von `New-AzureRmResourceGroupDeployment` durchgeführt werden. Für den Aufruf müssen Sie den Namen der Ressourcengruppe, die aktualisierte Vorlagendatei und die Parameterdatei angeben. Durch den Aufruf wird Resource Manager angewiesen, entsprechende Änderungen an den von Ihnen aktualisierten Ressourcen vorzunehmen. Vergewissern Sie sich, dass Sie bei Ihrem Konto angemeldet sind und sich im richtigen Abonnement befinden, und führen Sie dann das Upgrade mithilfe des folgenden Befehls durch:

    ```sh
    New-AzureRmResourceGroupDeployment -ResourceGroupName <ResourceGroup> -TemplateFile <PathToTemplateFile> -TemplateParameterFile <PathToParametersFile> -Verbose
    ```

5. Nachdem der Rollout des Upgrades abgeschlossen ist (Dauer: 15 bis 45 Minuten), sollten die Leistungsindikatoren per Microsoft Azure-Diagnose erfasst und an die Tabelle „WADPerformanceCountersTable“ in dem Speicherkonto gesendet werden, das dem Cluster zugeordnet ist. Sie können die Leistungsindikatoren in Application Insights anzeigen, indem Sie [die AI-Senke der Resource Manager-Vorlage hinzufügen](service-fabric-diagnostics-event-analysis-appinsights.md#add-the-ai-sink-to-the-resource-manager-template).

## <a name="next-steps"></a>Nächste Schritte
* Erfassen Sie weitere Leistungsindikatoren für Ihren Cluster. Eine Liste mit Leistungsindikatoren, die Sie erfassen sollten, finden Sie unter [Leistungsmetriken](service-fabric-diagnostics-event-generation-perf.md).
* Unter [Verwenden von Überwachung und Diagnose bei einer Windows-VM und Azure Resource Manager-Vorlagen](../virtual-machines/windows/extensions-diagnostics-template.md) erfahren Sie, wie Sie `WadCfg` noch weiter anpassen können und beispielsweise zusätzliche Speicherkonten für die Übermittlung von Diagnosedaten konfigurieren.
