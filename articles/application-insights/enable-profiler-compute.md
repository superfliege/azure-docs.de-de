---
title: "Aktivieren von Application Insights Profiler für Azure Compute-Anwendungen | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie den Profiler für eine Anwendung einrichten, die in Azure Compute ausgeführt wird."
services: application-insights
documentationcenter: 
author: ramach
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: ramach
ms.openlocfilehash: b39b3b234112647ebeb531262d3b3876aee0b63b
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2017
---
# <a name="enable-application-insights-profiler-on-azure-virtual-machines-service-fabric-and-cloud-services"></a>Aktivieren von Application Insights Profiler für Azure Virtual Machines, Service Fabric und Cloud Services

Diese exemplarische Vorgehensweise veranschaulicht, wie Sie Azure Application Insights Profiler für eine ASP.NET-Anwendung aktivieren, die von einer Azure Compute-Ressource gehostet wird.  
Die Beispiele umfassen die Unterstützung für Azure Virtual Machines, Azure-VM-Skalierungsgruppen, Azure Service Fabric und Azure Cloud Services.  
Die Beispiele basieren auf Vorlagen, die das [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview)-Bereitstellungsmodell unterstützen.  


## <a name="overview"></a>Übersicht

Das folgende Diagramm veranschaulicht die Funktionsweise von Application Insights Profiler für Azure-Ressourcen. Als Beispiel dient ein virtueller Azure-Computer.

![Übersicht](./media/enable-profiler-compute/overview.png)


Die Konfiguration muss an drei Stellen erfolgen, um den Profiler vollständig zu aktivieren:

1. Portalblatt der Application Insights-Instanz
2. Quellcode der Anwendung (z. B. eine ASP.NET-Webanwendung)
3. Quellcode der Bereitstellungsdefinition für die Umgebung (z. B. eine JSON-Datei einer Bereitstellungsvorlage für virtuelle Computer).


## <a name="configure-the-application-insights-instance"></a>Konfigurieren der Application Insights-Instanz

Erstellen oder besuchen Sie das Blatt „Azure-Portal“ für die zu verwendende Application Insights-Instanz, und notieren Sie ihren Instrumentierungsschlüssel. Dieser ist für weitere Konfigurationsschritte hilfreich:

  ![Speicherort des Schlüssels](./media/enable-profiler-compute/CopyAIKey.png)

Diese Instanz sollte mit der identisch sein, mit der Ihre Anwendung konfiguriert wurde, um bei den einzelnen Anforderungen Telemetriedaten an sie zu senden.
Darüber hinaus werden Profiler-Ergebnisse in dieser Instanz zur Verfügung gestellt.  

Während Sie sich weiterhin im Azure-Portal befinden, befolgen Sie die Schritte von [Aktivieren des Profilers](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler), um die Konfiguration der AI-Instanz für Profiler abzuschließen. Es ist nicht erforderlich, Web-Apps im Rahmen dieser exemplarischen Vorgehensweise zu verknüpfen. Achten Sie einfach darauf, dass Profiler im Portal aktiviert ist.


## <a name="configure-the-application-source-code"></a>Konfigurieren des Quellcodes der Anwendung

Ihre Anwendung sollte so konfiguriert werden, dass sie bei jedem `Request`-Vorgang Telemetriedaten an eine Application Insights-Instanz sendet.  

1. Fügen Sie das [Application Insights SDK](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-overview#get-started) zu Ihrem Anwendungsprojekt hinzu. Stellen Sie sicher, dass die folgenden Versionen des NuGet-Pakets verwendet werden:  
  - Für ASP.NET-Anwendungen: [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) von Version 2.3.0 oder höher.
  - Für ASP.NET Core-Anwendungen: [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/) 2.1.0 oder höher.
  - Für andere .NET- und .NET Core-Anwendungen (z. B. zustandsloser Service Fabric-Dienst, Clouddienst-Workerrolle): [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) oder [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 oder höher.  

2. Wenn Ihre Anwendung *keine* ASP.NET- oder ASP.NET Core-Anwendung ist (z. B. Cloud Services-Workerrollen, zustandslose Service Fabric-APIs), ist das folgende zusätzliche Instrumentierungssetup erforderlich:  

  2.1. Fügen Sie den folgenden Code zu einem frühen Zeitpunkt der Anwendungslebensdauer hinzu.  

  ```csharp
  using Microsoft.ApplicationInsights.Extensibility;
  ...
  // Replace with your own Application Insights instrumentation key.
  TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
  ```
  Weitere Informationen zu dieser globalen Konfiguration des Instrumentierungsschlüssels finden Sie unter [Verwenden von Service Fabric mit Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  2.2. Schließen Sie zu instrumentierende Codepassagen in eine `StartOperation<RequestTelemetry>` `using`-Anweisung ein, wie im folgenden Beispiel:

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

> [!NOTE]  
> Das Aufrufen von `StartOperation<RequestTelemetry>` innerhalb eines anderen `StartOperation<RequestTelemetry>`-Bereichs wird nicht unterstützt. Sie können stattdessen `StartOperation<DependencyTelemetry>` im geschachtelten Bereich verwenden. Beispiel:  

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


## <a name="configure-the-environment-deployment-definition"></a>Konfigurieren der Definition für die Umgebungsbereitstellung

Die Umgebung, in der Application Insights Profiler und Ihre Anwendung ausgeführt werden, kann ein virtueller Computer, eine VM-Skalierungsgruppe, ein Service Fabric-Cluster oder ein Clouddienst sein.  

### <a name="virtual-machine-virtual-machine-scale-sets-or-service-fabric"></a>Virtueller Computer, VM-Skalierungsgruppe oder Service Fabric

Vollständige Beispiele:  
  * [Virtueller Computer](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
  * [VM-Skalierungsgruppe](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
  * [Service Fabric-Cluster](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json)

1. Es ist ausreichend zu bestätigen, dass das bereitgestellte Betriebssystem `Windows Server 2012 R2` oder höher ist, um sicherzustellen, dass [.NET Framework 4.6.1](https://docs.microsoft.com/en-us/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) oder höher verwendet wird.

2. Suchen Sie die Erweiterung für die [Azure-Diagnose](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/azure-diagnostics) in der Bereitstellungsvorlagendatei, und fügen Sie den folgenden `SinksConfig`-Abschnitt als untergeordnetes Element von `WadCfg` hinzu. Ersetzen Sie dann den `ApplicationInsightsProfiler`-Eigenschaftswert durch Ihren eigenen AI-Instrumentierungsschlüssel:  
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

Informationen zum Hinzufügen der Azure-Diagnoseerweiterung zu Ihrer Bereitstellungsvorlage finden Sie in den Beispielen und in [diesem Leitfaden](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


### <a name="cloud-services"></a>Cloud Services

1. Es ist ausreichend zu bestätigen, dass `ServiceConfiguration.*.cscfg`-Dateien für `osFamily` den Wert `"5"` oder höher enthalten, um sicherzustellen, dass [.NET Framework 4.6.1](https://docs.microsoft.com/en-us/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) oder höher verwendet wird.

2. Suchen Sie die `diagnostics.wadcfgx`-Datei von [Azure-Diagnose](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/azure-diagnostics) für Ihre Anwendungsrolle:  
![Speicherort der Konfigurationsdatei für die Diagnose](./media/enable-profiler-compute/cloudservice-solutionexplorer.png)  
Wenn Sie die Datei nicht finden können, finden Sie die Informationen zum Aktivieren der Diagnoseerweiterung in Ihrem Azure Cloud Services-Projekt in [diesem Leitfaden](https://docs.microsoft.com/en-us/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them).

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
>  * Der von Ihrer Anwendung verwendete Instrumentierungsschlüssel  
>  * Der von der `ApplicationInsights`-Senke verwendete Instrumentierungsschlüssel  
>  * Der von der `ApplicationInsightsProfiler`-Senke verwendete Instrumentierungsschlüssel  
>
>Der tatsächlich von der `ApplicationInsights`-Senke verwendete Instrumentierungsschlüssel befindet sich in den `ServiceConfiguration.*.cscfg`-Dateien.  
>Nach der Freigabe des Visual Studio 15.5 Azure SDKs müssen nur noch die von der Anwendung und der `ApplicationInsightsProfiler`-Senke verwendeten Instrumentierungsschlüssel übereinstimmen.


## <a name="environment-deployment-and-runtime-configurations"></a>Umgebungsbereitstellung und Laufzeitkonfigurationen

1. Stellen Sie die geänderte Definition für die Umgebungsbereitstellung bereit.  
In der Regel ist eine vollständige Vorlagenbereitstellung oder eine Cloud Services-Veröffentlichung über PowerShell-Cmdlets oder Visual Studio einbezogen, um die Änderungen anzuwenden.  
Nachfolgend finden Sie einen alternativen Ansatz für vorhandene `Virtual Machines`, der sich nur mit seiner Diagnoseerweiterung befasst:  
```powershell
$ConfigFilePath = [IO.Path]::GetTempFileName()
# After exporting the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
(Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
# Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
# if your original diagnostics configuration had the storageAccountName property in the protectedSettings section
# (which is not downloadable). Make sure to pass the same original value you had in this cmdlet call.
Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
```

2. Wenn die betroffene Anwendung über [IIS](https://www.microsoft.com/web/platform/server.aspx) ausgeführt wird, muss das Windows-Feature `IIS Http Tracing` aktiviert werden:  
  Richten Sie den Remotezugriff auf die Umgebung ein, und verwenden Sie dann das Fenster [Hinzufügen von Windows-Features]( https://docs.microsoft.com/en-us/iis/configuration/system.webserver/tracing/), oder führen Sie den folgenden Befehl in PowerShell (als Administrator) aus:  
  ```powershell
  Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
  ```  
  Wenn das Einrichten des Remotezugriffs ein Problem darstellt, können Sie Folgendes über die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/en-us/cli/azure/get-started-with-azure-cli) ausführen:  
  ```powershell
  az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
  ```


## <a name="how-to-enable-profiler-on-on-premises-servers"></a>Aktivieren von Profiler auf lokalen Servern

Auch als die Ausführung von Application Insights Profiler im eigenständigen Modus bekannt (nicht an Änderungen der Diagnoseerweiterung gebunden).  
Es ist nicht geplant, Application Insights Profiler für lokale Server zu unterstützen.
Wenn Sie daran interessiert sind, mit diesem Szenario zu experimentieren, können Sie den Unterstützungscode [hier](https://github.com/ramach-msft/AIProfiler-Standalone) herunterladen.  
Wir sind *nicht* für die Beibehaltung dieses Codes oder die Reaktion auf Anforderungen zu Problemen oder Features verantwortlich.


## <a name="next-steps"></a>Nächste Schritte

- Generieren Sie Datenverkehr für Ihre Anwendung (starten Sie z. B. einen [Verfügbarkeitstest](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-monitor-web-app-availability)) und gestatten Sie anschließend 10 bis 15 Minuten, bevor Ablaufverfolgungen an die Application Insights-Instanz gesendet werden.

- Weitere Informationen finden Sie unter [Profiler-Ablaufverfolgungen](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler) im Azure-Portal.

- Hilfe bei der Problembehandlung von Profiler-Problemen finden Sie unter [Profiler-Problembehandlung](app-insights-profiler.md#troubleshooting).

- Weitere Informationen zum Profiler finden Sie unter [Application Insights Profiler](app-insights-profiler.md).
