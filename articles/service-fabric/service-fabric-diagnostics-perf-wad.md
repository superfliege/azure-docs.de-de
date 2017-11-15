---
title: "Azure Service Fabric: Leistungsüberwachung mit der Microsoft Azure-Diagnoseerweiterung | Microsoft-Dokumentation"
description: "Verwenden Sie die Microsoft Azure-Diagnose, um Leistungsindikatoren für Ihre Azure Service Fabric-Cluster zu erfassen."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/05/2017
ms.author: dekapur
ms.openlocfilehash: f71c483eba201eae91c15b84a2ed42fcb68ae575
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2017
---
# <a name="performance-monitoring-with-windows-azure-diagnostics-extension"></a>Leistungsüberwachung mit der Microsoft Azure-Diagnoseerweiterung

In diesem Artikel erfahren Sie, wie Sie die Erfassung von Leistungsindikatoren über die Microsoft Azure-Diagnoseerweiterung für Windows-Cluster einrichten. Für Linux-Cluster muss der [OMS-Agent](service-fabric-diagnostics-oms-agent.md) eingerichtet werden, um Leistungsindikatoren für die Knoten zu erfassen. 

 > [!NOTE]
> Die Microsoft Azure-Diagnoseerweiterung muss in Ihrem Cluster bereitgestellt sein, damit diese Schritte funktionieren. Falls sie noch nicht eingerichtet ist, wechseln Sie zu [Ereignisaggregation und -sammlung mit der Windows Azure-Diagnose](service-fabric-diagnostics-event-aggregation-wad.md), und führen Sie die Schritte unter *Bereitstellen der Diagnoseerweiterung* aus.

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

3. Fügen Sie `PerformanceCounterConfiguration` (wurde im vorherigen Schritt deklariert) die Leistungsindikatoren hinzu, die erfasst werden sollen. Jeder zu erfassende Leistungsindikator wird mit `counterSpecifier`, `sampleRate`, `unit`, `annotation` sowie mit allen relevanten `sinks` definiert. Hier sehen Sie ein Beispiel eines Leistungsindikators für die *Gesamtprozessorzeit* (gibt an, wie lang die CPU mit Verarbeitungsvorgängen beschäftigt war):

    ```json
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
            }
        ]
    }
    ```

    Die Samplingrate für den Leistungsindikator kann gemäß Ihren Anforderungen angepasst werden. Das Format lautet `PT<time><unit>`. Wenn der Leistungsindikator also beispielsweise sekündlich erfasst werden soll, legen Sie Folgendes fest: `"sampleRate": "PT15S"`.

    Analog dazu können Sie mehrere weitere Leistungsindikatoren erfassen, indem Sie diese der Liste in `PerformanceCounterConfiguration` hinzufügen. Mithilfe von `*` können Sie zwar Gruppen von Leistungsindikatoren mit ähnlichem Namen angeben, um Leistungsindikatoren jedoch über eine Senke (an Application Insights) senden zu können, müssen sie einzeln deklariert werden. 

4. Nach dem Hinzufügen der zu erfassenden Leistungsindikatoren müssen Sie Ihre Clusterressource upgraden, damit die Änderungen in Ihrem Cluster ausgeführten Cluster übernommen werden. Speichern Sie Ihre Änderungen an `template.json`, und öffnen Sie PowerShell. Das Upgrade des Clusters kann mithilfe von `New-AzureRmResourceGroupDeployment` durchgeführt werden. Für den Aufruf müssen Sie den Namen der Ressourcengruppe, die aktualisierte Vorlagendatei und die Parameterdatei angeben. Durch den Aufruf wird Resource Manager angewiesen, entsprechende Änderungen an den von Ihnen aktualisierten Ressourcen vorzunehmen. Vergewissern Sie sich, dass Sie bei Ihrem Konto angemeldet sind und sich im richtigen Abonnement befinden, und führen Sie dann das Upgrade mithilfe des folgenden Befehls durch:

    ```sh
    New-AzureRmResourceGroupDeployment -ResourceGroupName <ResourceGroup> -TemplateFile <PathToTemplateFile> -TemplateParameterFile <PathToParametersFile> -Verbose
    ```

5. Nach Abschluss der Upgradebereitstellung (dauert zwischen 15 und 45 Minuten) sollte die Microsoft Azure-Diagnoseerweiterung Leistungsindikatoren erfassen und an eine Tabelle im Speicherkonto senden (gemäß Deklaration in `WadCfg`).

## <a name="next-steps"></a>Nächste Schritte
* Zeigen Sie Ihre Leistungsindikatoren in Application Insights an, indem Sie die Application Insights-Senke zu `WadCfg` hinzufügen. Informationen zum Konfigurieren der Application Insights-Senke finden Sie unter [Ereignisanalyse und Visualisierung mit Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) im Abschnitt *Hinzufügen der AI-Senke zur Resource Manager-Vorlage*.
* Erfassen Sie weitere Leistungsindikatoren für Ihren Cluster. Eine Liste mit Leistungsindikatoren, die Sie erfassen sollten, finden Sie unter [Leistungsmetriken](service-fabric-diagnostics-event-generation-perf.md).
* Unter [Verwenden von Überwachung und Diagnose bei einer Windows-VM und Azure Resource Manager-Vorlagen](../virtual-machines/windows/extensions-diagnostics-template.md) erfahren Sie, wie Sie `WadCfg` noch weiter anpassen können und beispielsweise zusätzliche Speicherkonten für die Übermittlung von Diagnosedaten konfigurieren.