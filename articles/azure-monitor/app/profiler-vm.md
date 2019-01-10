---
title: Profilerstellung für Web-Apps in einer Azure-VM mit Application Insights Profiler | Microsoft-Dokumentation
description: Profilerstellung für Web-Apps in einer Azure-VM mit Application Insights Profiler
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
ms.openlocfilehash: e8f80e7d19a961c22b4e1e88556ac165d2558034
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54081682"
---
# <a name="profile-web-apps-running-on-an-azure-virtual-machine-or-virtual-machine-scale-set-with-application-insights-profiler"></a>Profilerstellung von Web-Apps, die auf einem virtuellen Azure-Computer oder in einer VM-Skalierungsgruppe mit Application Insights Profiler ausgeführt werden
Application Insights Profiler kann auch für diese Dienste bereitgestellt werden:
* [Azure App Service](../../azure-monitor/app/profiler.md?toc=/azure/azure-monitor/toc.json)
* [Cloud Services](profiler-cloudservice.md ?toc=/azure/azure-monitor/toc.json)
* [Service Fabric](profiler-vm.md?toc=/azure/azure-monitor/toc.json)

## <a name="deploy-profiler-on-a-virtual-machine-or-scale-set"></a>Bereitstellen von Profiler auf einem virtuellen Computer oder in einer Skalierungsgruppe
Diese Seite führt Sie durch die Schritte, die zum Ausführen von Application Insights Profiler auf einem virtuellen Azure-Computer oder in einer Azure-VM-Skalierungsgruppe erforderlich sind. Application Insights Profiler wird mit der Erweiterung „Microsoft Azure-Diagnose“ für VMs installiert. Die Erweiterung muss für die Ausführung des Profilers konfiguriert und das App Insights SDK in Ihre Anwendung integriert werden.

1. Fügen Sie Ihrer [ASP.NET-Anwendung](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) oder Ihrer regulären [.NET-Anwendung](https://docs.microsoft.com/azure/application-insights/app-insights-windows-services?toc=/azure/azure-monitor/toc.json) das Application Insights SDK hinzu. Sie müssen Anforderungstelemetriedaten an Application Insights senden, damit Profile für Ihre Anforderungen angezeigt werden.
1. Installieren Sie die Erweiterung „Microsoft Azure-Diagnose“ auf Ihrem virtuellen Computer. Vollständige Beispiele für Resource Manager-Vorlagen finden Sie hier:  
    * [Virtueller Computer](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
    * [VM-Skalierungsgruppe](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
    
    Der Schlüsselteil ist die Senke des Application Insights-Profilers (ApplicationInsightsProfilerSink) in der WAD-Konfiguration (WadCfg). Fügen Sie diesem Abschnitt eine weitere Senke hinzu, um WAD anzuweisen, dem Profiler das Senden von Daten an Ihre iKey-Instanz zu ermöglichen.
    ```json
      "SinksConfig": {
        "Sink": [
          {
            "name": "ApplicationInsightsSink",
            "ApplicationInsights": "85f73556-b1ba-46de-9534-606e08c6120f"
          },
          {
            "name": "MyApplicationInsightsProfilerSink",
            "ApplicationInsightsProfiler": "85f73556-b1ba-46de-9534-606e08c6120f"
          }
        ]
      },
    ```

1. Stellen Sie die geänderte Definition für die Umgebungsbereitstellung bereit.  

   In der Regel wird eine vollständige Vorlagenbereitstellung oder eine clouddienstbasierte Veröffentlichung über PowerShell-Cmdlets oder Visual Studio verwendet, um die Änderungen anzuwenden.  

   Die folgenden PowerShell-Befehle sind eine Alternative für vorhandene virtuelle Computer und betreffen nur die Azure-Diagnose-Erweiterung. Sie müssen lediglich die Profiler-Senke wie oben erläutert der Konfiguration hinzufügen, die durch den Befehl „Get-AzureRmVMDiagnosticsExtension“ zurückgegeben wird. Übergeben Sie anschließend die aktualisierte Konfiguration an den Befehl „Set-AzureRmVMDiagnosticsExcension“.

    ```powershell
    $ConfigFilePath = [IO.Path]::GetTempFileName()
    # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
    (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
    # Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
    # If your original diagnostics configuration had the storageAccountName property in the protectedSettings section (which is not downloadable), be sure to pass the same original value you had in this cmdlet call.
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
    ```

1. Wenn die betroffene Anwendung über [IIS](https://www.microsoft.com/web/downloads/platform.aspx) ausgeführt wird, aktivieren Sie das Windows-Feature `IIS Http Tracing`.

   a. Richten Sie den Remotezugriff auf die Umgebung ein, und verwenden Sie dann das Fenster [Hinzufügen von Windows-Features]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/), oder führen Sie den folgenden Befehl in PowerShell (als Administrator) aus:  

    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
   b. Wenn das Einrichten des Remotezugriffs ein Problem darstellt, können Sie folgenden Befehl über die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) ausführen:  

    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```

1. Stellen Sie Ihre Anwendung bereit.

## <a name="can-profiler-run-on-on-premises-servers"></a>Können Profiler auf lokalen Servern ausgeführt werden?
Es ist nicht geplant, Application Insights Profiler auf lokalen Servern zu unterstützen.

## <a name="next-steps"></a>Nächste Schritte

- Generieren Sie Datenverkehr zu Ihrer Anwendung (starten Sie z.B. einen [Verfügbarkeitstest](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Warten Sie dann 10 bis 15 Minuten, bis das Senden der Ablaufverfolgungen an die Application Insights-Instanz beginnt.
- Weitere Informationen finden Sie unter [Profiler-Ablaufverfolgungen](https://docs.microsoft.com/azure/application-insights/app-insights-profiler-overview?toc=/azure/azure-monitor/toc.json) im Azure-Portal.
- Hilfe bei der Problembehandlung von Profiler-Problemen finden Sie unter [Profiler-Problembehandlung](profiler-troubleshooting.md ?toc=/azure/azure-monitor/toc.json).
