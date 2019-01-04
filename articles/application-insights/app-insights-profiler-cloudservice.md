---
title: Erstellen von Profilen für Azure Cloud Services-Liveinstanzen mit Application Insights | Microsoft-Dokumentation
description: Aktivieren von Application Insights Profiler für Azure Cloud Services.
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
ms.openlocfilehash: 28e21180fde50d19154830694cd4959795ae9d5c
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/27/2018
ms.locfileid: "53789380"
---
# <a name="profile-live-azure-cloud-services-with-application-insights"></a>Erstellen von Profilen für Azure Cloud Services-Liveinstanzen mit Application Insights

Application Insights Profiler kann auch für diese Dienste bereitgestellt werden:
* [Azure App Service](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric-Anwendungen](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Virtuelle Computer](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler wird mit der Erweiterung „Microsoft Azure-Diagnose“ installiert. Sie müssen die Microsoft Azure-Diagnose nur so konfigurieren, dass Profiler installiert wird und Profile an Ihre Application Insights-Ressource sendet.

## <a name="enable-profiler-for-your-azure-cloud-service"></a>Aktivieren von Profiler für Ihren Azure-Clouddienst
1. Stellen Sie sicher, dass [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) oder höher verwendet wird.  Dazu ist es ausreichend zu bestätigen, dass die *ServiceConfiguration.\*.cscfg*-Dateien einen `osFamily`-Wert von „5“ oder höher haben.
1. Fügen Sie dem Clouddienst das [Application Insights SDK](../azure-monitor/app/cloudservices.md?toc=/azure/azure-monitor/toc.json) hinzu.
1. Verfolgen Sie Anforderungen mit Application Insights:

    Für ASP.NET-Webrollen kann Application Insights die Anforderungen automatisch verfolgen.

    Für Workerrollen müssen Sie [Code hinzufügen, um Anforderungen nachzuverfolgen](app-insights-profiler-trackrequests.md?toc=/azure/azure-monitor/toc.json).

    

1. Konfigurieren Sie die Erweiterung „Microsoft Azure-Diagnose“, um Profiler zu aktivieren.



    1. Suchen Sie wie hier gezeigt die [Azure-Diagnose](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics)-Datei *diagnostics.wadcfgx* für Ihre Anwendungsrolle:  

       ![Speicherort der Konfigurationsdatei für die Diagnose](./media/enable-profiler-compute/cloudservice-solutionexplorer.png)  

        Wenn Sie die Datei nicht finden können, finden Sie Informationen zum Aktivieren der Diagnoseerweiterung in Ihrem Azure Cloud Services-Projekt unter [Konfigurieren der Diagnose für Azure Cloud Services und Virtual Machines](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them).

    1. Fügen Sie den folgenden `SinksConfig`-Abschnitt als untergeordnetes Element von `WadCfg` hinzu:  

      ```xml
      <WadCfg>
        <DiagnosticMonitorConfiguration>...</DiagnosticMonitorConfiguration>
        <SinksConfig>
          <Sink name="MyApplicationInsightsProfiler">
            <!-- Replace with your own Application Insights instrumentation key. -->
            <ApplicationInsightsProfiler>00000000-0000-0000-0000-000000000000</ApplicationInsightsProfiler>
          </Sink>
        </SinksConfig>
      </WadCfg>
      ```

    >   **HINWEIS:** Wenn die Datei diagnostics.wadcfgx* auch eine andere Senke vom Typ `ApplicationInsights` enthält, müssen alle drei der folgenden Instrumentierungsschlüssel übereinstimmen:  
    >  * Der von Ihrer Anwendung verwendete Schlüssel  
    >  * Der von der `ApplicationInsights`-Senke verwendete Schlüssel  
    >  * Der von der `ApplicationInsightsProfiler`-Senke verwendete Schlüssel  
    >
    > Sie finden den von der `ApplicationInsights`-Senke verwendeten tatsächlichen Instrumentierungsschlüsselwert in den *ServiceConfiguration.\*.cscfg*-Dateien.  
    > Nach dem Visual Studio 15.5 Azure SDK-Release müssen nur noch die von der Anwendung und der `ApplicationInsightsProfiler`-Senke verwendeten Instrumentierungsschlüssel übereinstimmen.
1. Stellen Sie Ihren Dienst mit der neuen Diagnosekonfiguration bereit, und Application Insights Profiler wird für die Ausführung in Ihrem Dienst konfiguriert.
 
## <a name="next-steps"></a>Nächste Schritte

- Generieren Sie Datenverkehr zu Ihrer Anwendung (starten Sie z.B. einen [Verfügbarkeitstest](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Warten Sie dann 10 bis 15 Minuten, bis das Senden der Ablaufverfolgungen an die Application Insights-Instanz beginnt.
- Weitere Informationen finden Sie unter [Profiler-Ablaufverfolgungen](https://docs.microsoft.com/azure/application-insights/app-insights-profiler-overview?toc=/azure/azure-monitor/toc.json) im Azure-Portal.
- Hilfe bei der Problembehandlung von Profiler-Problemen finden Sie unter [Profiler-Problembehandlung](app-insights-profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).