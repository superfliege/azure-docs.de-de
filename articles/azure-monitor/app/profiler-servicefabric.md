---
title: Erstellen von Profilen für Azure Service Fabric-Liveanwendungen mit Application Insights | Microsoft-Dokumentation
description: Aktivieren des Profilers für eine Service Fabric-Anwendung
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 7d47310217c06689a6088aa5c908223b41aadbfa
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54435480"
---
# <a name="profile-live-azure-service-fabric-applications-with-application-insights"></a>Erstellen von Profilen für Azure Service Fabric-Liveanwendungen mit Application Insights

Application Insights Profiler kann auch für diese Dienste bereitgestellt werden:
* [Azure App Service](profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Dokumentation zu virtuellen Computern](profiler-vm.md?toc=/azure/azure-monitor/toc.json)

## <a name="set-up-the-environment-deployment-definition"></a>Einrichten der Definition für die Umgebungsbereitstellung

Application Insights Profiler ist in Azure-Diagnose enthalten. Sie können die Azure-Diagnoseerweiterung mithilfe einer Azure Resource Manager-Vorlage für Ihren Service Fabric-Cluster installieren. Rufen Sie eine [Vorlage zur Installation von Azure-Diagnose auf einem Service Fabric-Cluster](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json) ab.

Zum Einrichten Ihrer Umgebung führen Sie folgende Schritte aus:

1. Zum Sicherstellen, dass [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) oder höher verwendet wird, ist es ausreichend, zu bestätigen, dass das bereitgestellte Betriebssystem `Windows Server 2012 R2` oder höher ist.

1. Suchen Sie nach der [Azure-Diagnoseerweiterung](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) in der Bereitstellungsvorlagendatei.

1. Fügen Sie den folgenden `SinksConfig`-Abschnitt als untergeordnetes Element von `WadCfg` hinzu. Ersetzen Sie den `ApplicationInsightsProfiler`-Eigenschaftswert durch Ihren eigenen Application Insights-Instrumentierungsschlüssel:  

      ```json
      "SinksConfig": {
        "Sink": [
          {
            "name": "MyApplicationInsightsProfilerSink",
            "ApplicationInsightsProfiler": "00000000-0000-0000-0000-000000000000"
          }
        ]
      }
      ```

      Informationen zum Hinzufügen der Diagnoseerweiterung zu Ihrer Bereitstellungsvorlage finden Sie unter [Verwenden von Überwachung und Diagnose bei einer Windows-VM und Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

1. Stellen Sie Ihren Service Fabric-Cluster über Ihre Azure Resource Manager-Vorlage bereit.  
  Wenn Ihre Einstellungen richtig sind, wird Application Insights Profiler installiert und aktiviert, sobald die Azure-Diagnoseerweiterung installiert wird. 

1. Fügen Sie Application Insights Ihrer Service Fabric-Anwendung hinzu.  
  Damit Profiler Profile für Ihre Anforderungen erfasst, muss Ihre Anwendung Anforderungsdaten an Application Insights senden. Weitere Informationen finden Sie auf der Seite zum [Application Insights SDK für Service Fabric-Projekte](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).

1. Stellen Sie Ihre Anwendung erneut bereit.

> [TIPP] Für virtuelle Computer gibt es folgende Alternative zu den zuvor genannten JSON-basierten Schritten: Navigieren Sie im Azure-Portal zu **Virtuelle Computer** > **Diagnoseeinstellungen** > **Senken**. **Aktivieren Sie das Senden von Diagnosedaten an Application Insights**, und wählen Sie dann entweder ein Application Insights-Konto oder einen bestimmten iKey aus.

## <a name="next-steps"></a>Nächste Schritte

* Generieren Sie Datenverkehr zu Ihrer Anwendung (starten Sie z.B. einen [Verfügbarkeitstest](monitor-web-app-availability.md)). Warten Sie dann 10 bis 15 Minuten, bis das Senden der Ablaufverfolgungen an die Application Insights-Instanz beginnt.
* Weitere Informationen finden Sie unter [Profiler-Ablaufverfolgungen](profiler-overview.md?toc=/azure/azure-monitor/toc.json) im Azure-Portal.
* Hilfe bei der Behandlung von Profiler-Problemen finden Sie unter [Problembehandlung für den Profiler](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).
