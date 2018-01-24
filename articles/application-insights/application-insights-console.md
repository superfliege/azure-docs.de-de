---
title: "Azure Application Insights für Konsolenanwendungen | Microsoft-Dokumentation"
description: "Überwachen Sie Webanwendungen auf Verfügbarkeit, Leistung und Auslastung."
services: application-insights
documentationcenter: .net
author: lmolkova
manager: bfung
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: lmolkova
ms.openlocfilehash: 1b6bef88d729a2febfca2bd236a5382d2c11bd69
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2018
---
# <a name="application-insights-for-net-console-applications"></a>Application Insights für .NET-Konsolenanwendungen
Mit [Application Insights](app-insights-overview.md) können Sie Ihre Webanwendung auf Verfügbarkeit, Leistung und Nutzung überwachen.

Sie benötigen ein Abonnement für [Microsoft Azure](http://azure.com). Melden Sie sich mit einem Microsoft-Konto an, das Sie möglicherweise für Windows, Xbox Live oder andere Microsoft-Clouddienste verwenden. Falls Ihr Team über ein Unternehmensabonnement für Azure verfügt, können Sie den Besitzer bitten, Sie über Ihr Microsoft-Konto hinzuzufügen.

## <a name="getting-started"></a>Erste Schritte

* Erstellen Sie im [Azure-Portal](https://portal.azure.com) [eine Application Insights-Ressource](app-insights-create-new-resource.md). Wählen Sie als Anwendungstyp "ASP.NET-App" aus.
* Erstellen Sie eine Kopie des Instrumentierungsschlüssels. Diesen finden Sie in der Dropdownliste „Essentials“ der neuen Ressource, die Sie erstellt haben. 
* Installieren Sie das neueste [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights)-Paket.
* Legen Sie den Instrumentierungsschlüssel im Code fest, bevor Sie Telemetriedaten nachverfolgen (oder legen Sie die Umgebungsvariable APPINSIGHTS_INSTRUMENTATIONKEY fest). Anschließend sollten Sie Telemetriedaten manuell nachverfolgen und im Azure-Portal anzeigen können.

```C#
TelemetryConfiguration.Active.InstrumentationKey = " *your key* ";
var telemetryClient = new TelemetryClient();
telemetryClient.TrackTrace("Hello World!");
```

* Installieren Sie die neueste Version des Pakets [Microsoft.ApplicationInsights.DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector). Es verfolgt automatisch HTTP-, SQL- und einige andere externe Abhängigkeitsaufrufe.

Sie können Application Insights im Code initialisieren und konfigurieren oder dazu die Datei `ApplicationInsights.config` verwenden. Stellen Sie sicher, dass die Initialisierung so früh wie möglich erfolgt.

### <a name="using-config-file"></a>Verwenden der Konfigurationsdatei

Standardmäßig sucht das Application Insights SDK im Arbeitsverzeichnis nach der Datei `ApplicationInsights.config`, wenn `TelemetryConfiguration` erstellt wird.

```C#
TelemetryConfiguration config = TelemetryConfiguration.Active; // Read ApplicationInsights.config file if present
```

Sie können den Pfad zu der Konfigurationsdatei aber auch angeben.

```C#
TelemetryConfiguration configuration = TelemetryConfiguration.CreateFromConfiguration("ApplicationInsights.config");
```

Weitere Informationen finden Sie in der [Referenz zu Konfigurationsdateien](app-insights-configuration-with-applicationinsights-config.md).

Ein vollständiges Beispiel der Konfigurationsdatei erhalten Sie, wenn Sie die neueste Version des Pakets [Microsoft.ApplicationInsights.WindowsServer](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer) installieren. Dies ist die **minimale** Konfiguration für die Abhängigkeitssammlung, die dem Codebeispiel entspricht.

```XML
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <TelemetryInitializers>
    <Add Type="Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer, Microsoft.AI.DependencyCollector"/>
  </TelemetryInitializers>
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule, Microsoft.AI.DependencyCollector">
      <ExcludeComponentCorrelationHttpHeadersOnDomains>
        <Add>core.windows.net</Add>
        <Add>core.chinacloudapi.cn</Add>
        <Add>core.cloudapi.de</Add>
        <Add>core.usgovcloudapi.net</Add>
        <Add>localhost</Add>
        <Add>127.0.0.1</Add>
      </ExcludeComponentCorrelationHttpHeadersOnDomains>
      <IncludeDiagnosticSourceActivities>
        <Add>Microsoft.Azure.ServiceBus</Add>
        <Add>Microsoft.Azure.EventHubs</Add>
      </IncludeDiagnosticSourceActivities>
    </Add>
  </TelemetryModules>
  <TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel, Microsoft.AI.ServerTelemetryChannel"/>
</ApplicationInsights>

```

### <a name="configuring-telemetry-collection-from-code"></a>Konfigurieren der Telemetrieerfassung im Code

* Erstellen und konfigurieren Sie während des Starts der Anwendung die `DependencyTrackingTelemetryModule`-Instanz – sie muss ein Singleton sein und für die Lebensdauer der Anwendung beibehalten werden.

```C#
var module = new DependencyTrackingTelemetryModule();

// prevent Correlation Id to be sent to certain endpoints. You may add other domains as needed.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
//...

// enable known dependency tracking, note that in future versions, we will extend this list. 
// please check default settings in https://github.com/Microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/DependencyCollector/NuGet/ApplicationInsights.config.install.xdt#L20
module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.ServiceBus");
module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.EventHubs");
//....

// initialize the module
module.Initialize(configuration);
```

* Hinzufügen häufiger Telemetrieinitialisierer

```C#
// stamps telemetry with correlation identifiers
TelemetryConfiguration.Active.TelemetryInitializers.Add(new OperationCorrelationTelemetryInitializer());

// ensures proper DependencyTelemetry.Type is set for Azure RESTful API calls
TelemetryConfiguration.Active.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());
```

* Für .NET Framework-Windows-Apps können Sie außerdem das Leistungsindikator-Collectormodul installieren und initialisieren. Eine Beschreibung finden Sie [hier](http://apmtips.com/blog/2017/02/13/enable-application-insights-live-metrics-from-code/).

#### <a name="full-example"></a>Vollständiges Beispiel

```C#
static void Main(string[] args)
{
    TelemetryConfiguration configuration = TelemetryConfiguration.Active;

    configuration.InstrumentationKey = "removed";
    configuration.TelemetryInitializers.Add(new OperationCorrelationTelemetryInitializer());
    configuration.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());

    var telemetryClient = new TelemetryClient();
    using (IntitializeDependencyTracking(configuration))
    {
        telemetryClient.TrackTrace("Hello World!");

        using (var httpClient = new HttpClient())
        {
            // Http dependency is automatically tracked!
            httpClient.GetAsync("https://microsoft.com").Wait();
        }
    }

    // run app...

    // when application stops or you are done with dependency tracking, do not forget to dispose the module
    dependencyTrackingModule.Dispose();

    telemetryClient.Flush();
}

static DependencyTrackingTelemetryModule IntitializeDependencyTracking(TelemetryConfiguration configuration)
{
    // prevent Correlation Id to be sent to certain endpoints. You may add other domains as needed.
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.chinacloudapi.cn");
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.cloudapi.de");    
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.usgovcloudapi.net");
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("localhost");
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("127.0.0.1");

    // enable known dependency tracking, note that in future versions, we will extend this list. 
    // please check default settings in https://github.com/Microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/DependencyCollector/NuGet/ApplicationInsights.config.install.xdt#L20
    module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.ServiceBus");
    module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.EventHubs");

    // initialize the module
    module.Initialize(configuration);

    return module;
}
```

## <a name="next-steps"></a>Nächste Schritte
* [Überwachen Sie Abhängigkeiten](app-insights-asp-net-dependencies.md), um zu überprüfen, ob REST-, SQL- oder andere externe Ressourcen die Leistung beeinträchtigen.
* [Verwenden Sie die API](app-insights-api-custom-events-metrics.md) , um Ihre eigenen Ereignisse und Metriken für eine detailliertere Ansicht der Leistung und Nutzung Ihrer App zu senden.
