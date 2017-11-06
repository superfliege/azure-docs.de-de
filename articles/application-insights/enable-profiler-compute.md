---
title: "Aktivieren von Application Insights Profiler für Azure Compute-Anwendungen | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie den Application Insights Profiler für eine Anwendung einrichten, die in Azure Compute ausgeführt wird."
services: application-insights
documentationcenter: 
author: ramach-msft
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: ramach
ms.openlocfilehash: 66ea24cfe9dd03ed62c06daa76ee043886ad7bcc
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2017
---
# <a name="enable-application-insights-profiler-for-azure-vms-service-fabric-and-cloud-services"></a>Aktivieren von Application Insights Profiler für Azure-VMs, Service Fabric und Cloud Services

Dieser Artikel veranschaulicht, wie Sie Azure Application Insights Profiler für eine ASP.NET-Anwendung aktivieren, die von einer Azure Compute-Ressource gehostet wird. 

Die Beispiele in diesem Artikel umfassen die Unterstützung für Azure Virtual Machines, VM-Skalierungsgruppen, Azure Service Fabric und Azure Cloud Services. Die Beispiele basieren auf Vorlagen, die das [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview)-Bereitstellungsmodell unterstützen.  


## <a name="overview"></a>Übersicht

Die folgende Abbildung zeigt das Zusammenwirken von Application Insights Profiler mit Azure-Ressourcen. In der Abbildung dient ein virtueller Azure-Computer als Beispiel.

  ![Übersicht](./media/enable-profiler-compute/overview.png)

Um den Profiler vollständig zu aktivieren, müssen Sie die Konfiguration an drei Stellen ändern:

* Im Bereich der Application Insights-Instanz im Azure-Portal.
* Im Quellcode der Anwendung (z.B. einer ASP.NET-Webanwendung).
* Im Quellcode der Bereitstellungsdefinition für die Umgebung (z.B. einer JSON-Datei einer VM-Bereitstellungsvorlage).


## <a name="set-up-the-application-insights-instance"></a>Einrichten der Application Insights-Instanz

Erstellen Sie im Azure-Portal die Application Insights-Instanz oder navigieren Sie zur bereits verwendeten Instanz. Beachten Sie den Instanzinstrumentierungsschlüssel. Sie werden den Instrumentierungsschlüssel in anderen Konfigurationsschritten verwenden.

  ![Speicherort des Instrumentierungsschlüssels](./media/enable-profiler-compute/CopyAIKey.png)

Diese Instanz sollte mit Ihrer Anwendung identisch sein. Sie ist zum Senden von Telemetriedaten bei jeder Anforderung konfiguriert.
Profiler-Ergebnisse werden in dieser Instanz auch zur Verfügung gestellt.  

Führen Sie im Azure-Portal die in [Aktivieren des Profilers](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler) beschriebenen Schritte zum Einrichten der Application Insights-Instanz für den Profiler aus. Sie müssen keine Web-Apps für das Beispiel in diesem Artikel verknüpfen. Stellen Sie nur sicher, dass der Profiler im Portal aktiviert ist.


## <a name="set-up-the-application-source-code"></a>Einrichten des Quellcodes der Anwendung

Richten Sie Ihre Anwendung so ein, dass sie bei jedem `Request`-Vorgang Telemetriedaten an eine Application Insights-Instanz sendet:  

1. Fügen Sie das [Application Insights SDK](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-overview#get-started) zu Ihrem Anwendungsprojekt hinzu. Stellen Sie sicher, dass die folgenden Versionen des NuGet-Pakets verwendet werden:  
  - Für ASP.NET-Anwendungen: [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) Version 2.3.0 oder höher.
  - Für ASP.NET Core-Anwendungen: [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/) 2.1.0 oder höher.
  - Für andere .NET- und .NET Core-Anwendungen (z.B. zustandsloser Service Fabric-Dienst oder eine Cloud Services-Workerrolle): [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) oder [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 oder höher.  

2. Wenn Ihre Anwendung *keine* ASP.NET- oder ASP.NET Core-Anwendung ist (z.B. Cloud Services-Workerrolle oder zustandslose Service Fabric-APIs), ist das folgende zusätzliche Instrumentierungssetup erforderlich:  

  1. Fügen Sie den folgenden Code zu einem frühen Zeitpunkt der Anwendungslebensdauer hinzu:  

    ```csharp
    using Microsoft.ApplicationInsights.Extensibility;
    ...
    // Replace with your own Application Insights instrumentation key.
    TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
    ```
  Weitere Informationen zu dieser globalen Konfiguration des Instrumentierungsschlüssels finden Sie unter [Verwenden von Service Fabric mit Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  2. Schließen Sie zu instrumentierende Codepassagen in eine `StartOperation<RequestTelemetry>` **USING**-Anweisung ein, wie im folgenden Beispiel:

    ```csharp
    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.DataContracts;
    ...
    var client = new TelemetryClient();
    ...
    using (var operation = client.StartOperation<RequestTelemetry>("Insert_Your_Custom_Event_Unique_Name"))
    {
      // ... Code I want to profile.
    }
    ```

  Das Aufrufen von `StartOperation<RequestTelemetry>` innerhalb eines anderen `StartOperation<RequestTelemetry>`-Bereichs wird nicht unterstützt. Sie können stattdessen `StartOperation<DependencyTelemetry>` im geschachtelten Bereich verwenden. Beispiel:  

    ```csharp
    using (var getDetailsOperation = client.StartOperation<RequestTelemetry>("GetProductDetails"))
    {
      try
      {
        ProductDetail details = new ProductDetail() { Id = productId };
        getDetailsOperation.Telemetry.Properties["ProductId"] = productId.ToString();

        // By using DependencyTelemetry, 'GetProductPrice' is correctly linked as part of the 'GetProductDetails' request.
        using (var getPriceOperation = client.StartOperation<DependencyTelemetry>("GetProductPrice"))
        {
            double price = await _priceDataBase.GetAsync(productId);
            if (IsTooCheap(price))
            {
                throw new PriceTooLowException(productId);
            }
            details.Price = price;
        }

        // Similarly, note how 'GetProductReviews' doesn't establish another RequestTelemetry.
        using (var getReviewsOperation = client.StartOperation<DependencyTelemetry>("GetProductReviews"))
        {
            details.Reviews = await _reviewDataBase.GetAsync(productId);
        }

        getDetailsOperation.Telemetry.Success = true;
        return details;
      }
      catch(Exception ex)
      {
        getDetailsOperation.Telemetry.Success = false;

        // This exception gets linked to the 'GetProductDetails' request telemetry.
        client.TrackException(ex);
        throw;
      }
    }
    ```


## <a name="set-up-the-environment-deployment-definition"></a>Einrichten der Definition für die Umgebungsbereitstellung

Die Umgebung, in der der Profiler und Ihre Anwendung ausgeführt werden, kann ein virtueller Computer, eine VM-Skalierungsgruppe, ein Service Fabric-Cluster oder eine Cloud Services-Instanz sein.  

### <a name="virtual-machines-virtual-machine-scale-sets-or-service-fabric"></a>Virtuelle Computer, VM-Skalierungsgruppen oder Service Fabric

Vollständige Beispiele:  
  * [Virtueller Computer](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
  * [VM-Skalierungsgruppe](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
  * [Service Fabric-Cluster](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json)

1. Es ist ausreichend zu bestätigen, dass das bereitgestellte Betriebssystem `Windows Server 2012 R2` oder höher ist, um sicherzustellen, dass [.NET Framework 4.6.1](https://docs.microsoft.com/en-us/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) oder höher verwendet wird.

2. Suchen Sie die Erweiterung für die [Azure-Diagnose](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/azure-diagnostics) in der Bereitstellungsvorlagendatei, und fügen Sie den folgenden `SinksConfig`-Abschnitt als untergeordnetes Element von `WadCfg` hinzu. Ersetzen Sie den `ApplicationInsightsProfiler`-Eigenschaftswert durch Ihren eigenen Application Insights-Instrumentierungsschlüssel:  
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

  Informationen zum Hinzufügen der Diagnoseerweiterung zu Ihrer Bereitstellungsvorlage finden Sie unter [Verwenden von Überwachung und Diagnose bei einer Windows-VM und Azure Resource Manager-Vorlagen](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


### <a name="cloud-services"></a>Cloud Services

1. Um sicherzustellen, dass [.NET Framework 4.6.1](https://docs.microsoft.com/en-us/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) oder höher verwendet wird, ist es ausreichend, zu bestätigen, dass ServiceConfiguration.\*.cscfg-Dateien einen `osFamily`-Wert von **„5“** oder höher haben.

2. Suchen Sie die [Azure-Diagnose](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/azure-diagnostics)-Datei „diagnostics.wadcfgx“ für Ihre Anwendungsrolle:  
  ![Speicherort der Konfigurationsdatei für die Diagnose](./media/enable-profiler-compute/cloudservice-solutionexplorer.png)  
  Wenn Sie die Datei nicht finden können, finden Sie Informationen zum Aktivieren der Diagnoseerweiterung in Ihrem Cloud Services-Projekt unter [Konfigurieren der Diagnose für Azure Cloud Services und Virtual Machines](https://docs.microsoft.com/en-us/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them).

3. Fügen Sie den folgenden `SinksConfig`-Abschnitt als untergeordnetes Element von `WadCfg` hinzu:  
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

> [!NOTE]  
> Wenn die Datei `diagnostics.wadcfgx` auch eine andere Senke vom Typ `ApplicationInsights` enthält, müssen alle drei der folgenden Instrumentierungsschlüssel übereinstimmen:  
>  * Der von Ihrer Anwendung verwendete Instrumentierungsschlüssel.  
>  * Der von der `ApplicationInsights`-Senke verwendete Instrumentierungsschlüssel.  
>  * Der von der `ApplicationInsightsProfiler`-Senke verwendete Instrumentierungsschlüssel.  
>
> Sie finden den von der `ApplicationInsights`-Senke verwendeten tatsächlichen Instrumentierungsschlüsselwert in den ServiceConfiguration.\*.cscfg-Dateien.  
> Nach der Freigabe des Visual Studio 15.5 Azure SDKs müssen nur noch die von der Anwendung und der `ApplicationInsightsProfiler`-Senke verwendeten Instrumentierungsschlüssel übereinstimmen.


## <a name="environment-deployment-and-runtime-configurations"></a>Umgebungsbereitstellung und Laufzeitkonfigurationen

1. Stellen Sie die geänderte Definition für die Umgebungsbereitstellung bereit.  

  In der Regel ist eine vollständige Vorlagenbereitstellung oder eine Cloud Services-Veröffentlichung über PowerShell-Cmdlets oder Visual Studio einbezogen, um die Änderungen anzuwenden.  

  Nachfolgend finden Sie einen alternativen Ansatz für vorhandene VMs, der nur die Azure-Diagnose-Erweiterung betrifft:  
  ```powershell
  $ConfigFilePath = [IO.Path]::GetTempFileName()
  # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
  (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
  # Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
  # if your original diagnostics configuration had the storageAccountName property in the protectedSettings section
  # (which is not downloadable). Make sure to pass the same original value you had in this cmdlet call.
  Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
  ```

2. Wenn die betroffene Anwendung über [IIS](https://www.microsoft.com/web/platform/server.aspx) ausgeführt wird, aktivieren Sie das Windows-Feature `IIS Http Tracing`:  
  
  1. Richten Sie den Remotezugriff auf die Umgebung ein, und verwenden Sie dann das Fenster [Hinzufügen von Windows-Features]( https://docs.microsoft.com/en-us/iis/configuration/system.webserver/tracing/), oder führen Sie den folgenden Befehl in PowerShell (als Administrator) aus:  
    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
  2. Wenn das Einrichten des Remotezugriffs ein Problem darstellt, können Sie folgenden Befehl über die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/en-us/cli/azure/get-started-with-azure-cli) ausführen:  
    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```


## <a name="enable-the-profiler-on-on-premises-servers"></a>Aktivieren von Profiler auf lokalen Servern

Das Aktivieren des Profilers auf einem lokalen Server ist auch als Ausführen von Application Insights Profiler im eigenständigen Modus bekannt (es ist nicht an Änderungen der Azure-Diagnose-Erweiterung gebunden). 

Es ist nicht geplant, den Profiler offiziell für lokale Server zu unterstützen. Wenn Sie daran interessiert sind, mit diesem Szenario zu experimentieren, können Sie den [Unterstützungscode herunterladen](https://github.com/ramach-msft/AIProfiler-Standalone). Wir sind *nicht* für die Beibehaltung dieses Codes oder die Reaktion auf Anforderungen zu Problemen und Featuranforderungen hinsichtlich des Codes verantwortlich.

## <a name="next-steps"></a>Nächste Schritte

- Generieren Sie Datenverkehr zu Ihrer Anwendung (starten Sie z.B. einen [Verfügbarkeitstest](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-monitor-web-app-availability)). Warten Sie dann 10 bis 15 Minuten, bis das Senden der Ablaufverfolgungen an die Application Insights-Instanz beginnt.
- Weitere Informationen finden Sie unter [Profiler-Ablaufverfolgungen](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler) im Azure-Portal.
- Hilfe bei der Problembehandlung von Profiler-Problemen finden Sie unter [Profiler-Problembehandlung](app-insights-profiler.md#troubleshooting).
- Weitere Informationen zum Profiler finden Sie unter [Application Insights Profiler](app-insights-profiler.md).
