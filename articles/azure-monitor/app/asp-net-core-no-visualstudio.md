---
title: Azure Application Insights für ASP.NET Core ohne Visual Studio | Microsoft-Dokumentation
description: Überwachen Sie ASP.NET Core-Webanwendungen auf Verfügbarkeit, Leistung und Auslastung.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: cithomas
ms.openlocfilehash: 8243523887ec9861459b2d196126237cf89bad97
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59287543"
---
# <a name="application-insights-for-aspnet-core-applications"></a>Application Insights für ASP.NET Core-Anwendungen

In diesem Artikel werden die Schritte zum Aktivieren von Application Insights für eine [ASP.NET Core](https://docs.microsoft.com/aspnet/core/?view=aspnetcore-2.2)-Anwendung ohne Verwendung der IDE von Visual Studio beschrieben. Wenn Sie die IDE von Visual Studio installiert haben, hilft Ihnen [dieses Dokument](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) mit der spezifischen Anleitung für Visual Studio weiter. Wenn Sie die Anleitung in diesem Artikel befolgen, beginnt Application Insights damit, Anforderungen, Abhängigkeiten, Ausnahmen, Leistungsindikatoren, Heartbeats und Protokolle für Ihre ASP.NET Core-Anwendung zu erfassen. Als Beispielanwendung wird eine [MVC-Anwendung](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/?view=aspnetcore-2.2) mit einer Ausrichtung auf `netcoreapp2.2` genutzt, aber die hier beschriebene Anleitung gilt für alle ASP.NET Core-Anwendungen. Auf etwaige Ausnahmen wird jeweils hingewiesen.

## <a name="supported-scenarios"></a>Unterstützte Szenarien

Mit dem Application Insights SDK (Software Development Kit) für ASP.NET Core können Ihre Anwendungen unabhängig davon überwacht werden, wo bzw. wie sie ausgeführt werden. Wenn Ihre Anwendung ausgeführt wird und über eine Netzwerkverbindung mit dem Application Insights-Dienst verfügt, wird erwartet, dass Telemetriedaten erfasst werden. Diese Unterstützung gilt u. a. für alle Betriebssysteme (Windows, Linux, Mac), Hostingmethoden (In-Process/Out-of-Process), Bereitstellungsmethoden (Frameworkabhängig/Eigenständig), Webserver (IIS, Kestrel), Plattformen (Azure-Web-Apps, Azure-VM, Docker, AKS usw.) oder IDEs (Visual Studio, VS Code, Befehlszeile).

## <a name="prerequisites"></a>Voraussetzungen

- Eine funktionierende ASP.NET Core-Anwendung. Befolgen Sie [diese Anleitung](https://docs.microsoft.com/aspnet/core/getting-started/), um bei Bedarf eine ASP.NET Core-Anwendung zu erstellen.
- Einen gültigen Application Insights-Instrumentierungsschlüssel, der zum Senden von Telemetriedaten an den Application Insights-Dienst benötigt wird. Befolgen Sie [diese Anleitung](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource), um bei Bedarf eine neue Application Insights-Ressource zu erstellen und einen Instrumentierungsschlüssel zu beschaffen.

## <a name="enabling-application-insights-server-side-telemetry"></a>Aktivieren der serverseitigen Telemetrie für Application Insights

1. Installieren Sie das Application Insights SDK für ASP.NET Core. Hierbei handelt es sich um ein reguläres NuGet-Paket. Es wird empfohlen, immer die neueste stabile Version zu verwenden. Einige der in diesem Artikel beschriebenen Funktionen sind in früheren Versionen unter Umständen nicht verfügbar. Die vollständigen Versionshinweise für das SDK finden Sie [hier](https://github.com/Microsoft/ApplicationInsights-aspnetcore/releases). 

Im Folgenden sind die Änderungen angegeben, die der CSPROJ-Datei Ihres Projekts hinzugefügt werden müssen.

```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.6.1" />
    </ItemGroup>
```

2. Fügen Sie `services.AddApplicationInsightsTelemetry();` der `ConfigureServices()`-Methode in Ihrer `Startup`-Klasse hinzu. Unten ist das vollständige Beispiel angegeben.

```csharp
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        // The following line enables Application Insights telemetry collection.
        services.AddApplicationInsightsTelemetry();

        // code adding other services for your application
        services.AddMvc();
    }
```

3. Richten Sie den Instrumentierungsschlüssel ein.

   Es ist zwar möglich, einen Instrumentierungsschlüssel als Argument für `services.AddApplicationInsightsTelemetry("putinstrumentationkeyhere");` bereitzustellen, aber es wird empfohlen, den Instrumentierungsschlüssel in der Konfiguration anzugeben. Im Folgenden wird veranschaulicht, wie Sie in `appsettings.json` einen Instrumentierungsschlüssel angeben. Stellen Sie sicher, dass `appsettings.json` während der Veröffentlichung in den Stammordner der Anwendung kopiert wird.

```json
    {
      "ApplicationInsights": {
        "InstrumentationKey": "putinstrumentationkeyhere"
      },
      "Logging": {
        "LogLevel": {
          "Default": "Warning"
        }
      }
    }
```

    Alternately, the instrumentation key can also be specified in either of the following environment variables.
    APPINSIGHTS_INSTRUMENTATIONKEY
    ApplicationInsights:InstrumentationKey

    Example:
    `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`

`APPINSIGHTS_INSTRUMENTATIONKEY` wird normalerweise verwendet, um den Instrumentierungsschlüssel für Anwendungen anzugeben, die in Azure-Web-Apps bereitgestellt werden.

> [!NOTE]
> Ein Instrumentierungsschlüssel, der im Code angegeben ist, erhält Vorrang vor der Umgebungsvariablen `APPINSIGHTS_INSTRUMENTATIONKEY`, die wiederum Vorrang vor anderen Optionen hat.

4. Führen Sie Ihre Anwendung aus, und senden Sie Anforderungen an die Anwendung. Die Telemetriedaten sollten nun an Application Insights fließen. Die folgenden Telemetriedaten werden vom Application Insights SDK automatisch erfasst.

    1. Anforderungen: Eingehende Webanforderungen für Ihre Anwendung.
    1. Abhängigkeiten
        1. HTTP/HTTPS-Aufrufe per `HttpClient`
        1. SQL-Aufrufe per `SqlClient`
        1. Azure-Speicheraufrufe per [Azure Storage-Client](https://www.nuget.org/packages/WindowsAzure.Storage/)*
        1. [EventHub Client SDK](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) Version 1.1.0 und höher
        1. [ServiceBus Client SDK](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus) Version 3.0.0 und höher

             *Azure Cosmos DB wird nur dann automatisch nachverfolgt, wenn HTTP/HTTPS verwendet wird. Der TCP-Modus wird von Application Insights nicht erfasst.


    1. [Leistungsindikatoren](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/)
        1. Für die Unterstützung von Leistungsindikatoren in ASP.NET gelten die folgenden Beschränkungen:
            1. SDK Version 2.4.1 und höher erfasst Leistungsindikatoren, wenn die Anwendung per Azure-Web-App (Windows) ausgeführt wird.
            1. Mit SDK Version 2.7.0-beta3 und höher werden Leistungsindikatoren erfasst, wenn die Anwendung unter Windows ausgeführt wird und auf `NETSTANDARD2.0` oder höher ausgerichtet ist.
            1. Für Anwendungen, die auf das vollständige .NET Framework abzielen, werden Leistungsindikatoren in allen Versionen des SDK unterstützt.

            Dieses Dokument wird aktualisiert, wenn unter Linux die Unterstützung von Leistungsindikatoren hinzugefügt wird.
    1. [Livemetriken](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream)
    1. `ILogger` Protokolle mit dem Schweregrad `Warning` oder höher werden von SDK Version 2.7.0-beta3 oder höher automatisch erfasst. Weitere Informationen finden Sie [hier](https://docs.microsoft.com/azure/azure-monitor/app/ilogger).

Es kann einige Minuten dauern, bis Telemetriedaten im Portal angezeigt werden. Um schnell zu überprüfen, ob alles funktioniert, sind [Livemetriken](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream) am besten geeignet, während Anforderungen an der ausgeführten Anwendung vorgenommen werden.

## <a name="send-ilogger-logs-to-application-insights"></a>Senden von ILogger-Protokollen an Application Insights

Application Insights unterstützt das Erfassen von Protokollen, die über ILogger gesendet werden. Die vollständige Dokumentation finden Sie [hier](https://docs.microsoft.com/azure/azure-monitor/app/ilogger).

## <a name="enable-client-side-telemetry-for-web-applications"></a>Aktivieren der clientseitigen Telemetrie für Webanwendungen

Die obigen Schritte sind ausreichend, um mit dem Erfassen der serverseitigen Telemetriedaten zu beginnen. Wenn Ihre Anwendung über clientseitige Komponenten verfügt, sollten Sie die unten angegebenen Schritte ausführen, um dafür mit dem Erfassen von [Telemetriedaten zur Verwendung](https://docs.microsoft.com/azure/azure-monitor/app/usage-overview) zu beginnen.

1. Fügen Sie in „_ViewImports.cshtml“ Folgendes ein:

```cshtml
    @inject Microsoft.ApplicationInsights.AspNetCore.JavaScriptSnippet JavaScriptSnippet
```

2. Fügen Sie in „_Layout.cshtml“ am Ende des Abschnitts `<head>` „HtmlHelper“ ein (vor allen anderen Skripts). Alle benutzerdefinierten JavaScript-Telemetriedaten, die Sie über die Seite melden möchten, sollten nach diesem Codeausschnitt eingefügt werden:

```cshtml
    @Html.Raw(JavaScriptSnippet.FullScript)
    </head>
```

Ändern Sie die Dateinamen gemäß Ihrer Anwendung. Die oben verwendeten Namen stammen aus einer Standardvorlage für die MVC-Anwendung.

## <a name="configuring-application-insights-sdk"></a>Konfigurieren des Application Insights SDK

Das Application Insights SDK für ASP.NET Core kann angepasst werden, um die Standardkonfiguration zu ändern. Benutzer des Application Insights ASP.NET SDK sind ggf. mit der Konfiguration per `ApplicationInsights.config` oder per Änderung von `TelemetryConfiguration.Active` vertraut. Für ASP.NET Core wird die Konfiguration auf andere Art durchgeführt. Das ASP.NET Core SDK wird der Anwendung hinzugefügt, indem der in ASP.NET Core integrierte [DependencyInjection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection)-Mechanismus verwendet wird. Für die Konfiguration wird ebenfalls DependencyInjection verwendet. Fast alle Konfigurationsänderungen werden in der `ConfigureServices()`-Methode Ihrer `Startup.cs`-Klasse durchgeführt, falls nichts anderes angegeben ist. Die Abschnitte unten enthalten weitere Informationen hierzu.

> [!NOTE]
> Es ist wichtig zu beachten, dass eine Modifizierung der Konfiguration durch das Ändern von `TelemetryConfiguration.Active` in ASP.NET Core-Anwendungen nicht empfohlen wird.

### <a name="configuring-using-applicationinsightsserviceoptions"></a>Konfigurieren mit ApplicationInsightsServiceOptions

Es ist möglich, einige allgemeine Einstellungen zu ändern, indem `ApplicationInsightsServiceOptions` an `services.AddApplicationInsightsTelemetry();` übergeben wird. Ein entsprechendes Beispiel ist nachfolgend dargestellt.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions aiOptions
                    = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions();
        // Disables adaptive sampling.
        aiOptions.EnableAdaptiveSampling = false;

        // Disables QuickPulse (Live Metrics stream).
        aiOptions.EnableQuickPulseMetricStream = false;
        services.AddApplicationInsightsTelemetry(aiOptions);
    }
```

Die genaue Liste mit den konfigurierbaren Einstellungen in `ApplicationInsightsServiceOptions` finden Sie [hier](https://github.com/Microsoft/ApplicationInsights-aspnetcore/blob/f0b8631e482d25982eb52092103b34e3ff6e5fef/src/Microsoft.ApplicationInsights.AspNetCore/Extensions/ApplicationInsightsServiceOptions.cs).

### <a name="sampling"></a>Stichproben

Das Application Insights SDK für ASP.NET Core unterstützt sowohl die FixedRate- als auch die adaptive Stichprobenerstellung. Die adaptive Stichprobenerstellung ist standardmäßig aktiviert. Informationen zum Konfigurieren der Stichprobenerstellung für ASP.NET Core-Anwendungen finden Sie in [diesem Dokument](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications).

### <a name="adding-telemetryinitializers"></a>Hinzufügen von TelemetryInitializers

Fügen Sie das `TelemetryInitializer`-Element wie unten gezeigt in den DependencyInjection-Container ein, wenn Sie ein Element dieser Art hinzufügen möchten. `TelemetryInitializer`-Elemente, die dem DependencyInjection-Container hinzugefügt werden, werden vom SDK automatisch ausgewählt.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();
    }
```

### <a name="removing-telemetryinitializers"></a>Entfernen von TelemetryInitializer-Elementen

Verwenden Sie zum Entfernen aller oder bestimmter TelemetryInitializer-Elemente, die standardmäßig vorhanden sind, den folgenden Beispielcode **nach** dem Aufruf von `AddApplicationInsightsTelemetry()`.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // Remove a specific built-in TelemetryInitializer
        var tiToRemove = services.FirstOrDefault<ServiceDescriptor>
                         (t => t.ImplementationType == typeof(AspNetCoreEnvironmentTelemetryInitializer));
        if (tiToRemove != null)
        {
            services.Remove(tiToRemove);
        }

        // Remove all initializers
        // This requires importing namespace using Microsoft.Extensions.DependencyInjection.Extensions;
        services.RemoveAll(typeof(ITelemetryInitializer));
    }
```

### <a name="adding-telemetryprocessors"></a>Hinzufügen von TelemetryProcessor-Elementen

Benutzerdefinierte Telemetrieprozessoren können `TelemetryConfiguration` hinzugefügt werden, indem die Erweiterungsmethode `AddApplicationInsightsTelemetryProcessor` in `IServiceCollection` verwendet wird. Verwenden Sie das folgende Beispiel.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // ...
        services.AddApplicationInsightsTelemetry();
        services.AddApplicationInsightsTelemetryProcessor<MyFirstCustomTelemetryProcessor>();

        // If you have more processors:
        services.AddApplicationInsightsTelemetryProcessor<MySecondCustomTelemetryProcessor>();
    }
```

### <a name="configuring-or-removing-default-telemetrymodules"></a>Konfigurieren oder Entfernen von TelemetryModule-Standardelementen

Die folgenden Module für die automatische Erfassung sind standardmäßig aktiviert und für die automatische Erfassung der Telemetriedaten verantwortlich. Sie können deaktiviert und konfiguriert werden, um das Standardverhalten zu ändern.

1. `RequestTrackingTelemetryModule`
1. `DependencyTrackingTelemetryModule`
1. `PerformanceCollectorModule`
1. `QuickPulseTelemetryModule`
1. `AppServicesHeartbeatTelemetryModule`
1. `AzureInstanceMetadataTelemetryModule`

Verwenden Sie zum Konfigurieren von `TelemetryModule`-Standardelementen die Erweiterungsmethode `ConfigureTelemetryModule<T>` in `IServiceCollection`. Dies ist im Beispiel unten dargestellt.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // The following configures DependencyTrackingTelemetryModule.
        // Similarly, any other default modules can be configured.
        services.ConfigureTelemetryModule<DependencyTrackingTelemetryModule>((module, o) =>
                        {
                            module.EnableW3CHeadersInjection = true;
                        });

        // The following removes PerformanceCollectorModule to disable perf-counter collection.
        // Similarly, any other default modules can be removed.
        var performanceCounterService = services.FirstOrDefault<ServiceDescriptor>(t => t.ImplementationType == typeof(PerformanceCollectorModule));
        if (performanceCounterService != null)
        {
         services.Remove(performanceCounterService);
        }
    }
```

### <a name="configuring-telemetry-channel"></a>Konfigurieren des Telemetriekanals

Der verwendete Standardkanal ist `ServerTelemetryChannel`. Er kann mithilfe des unten angegebenen Beispiels außer Kraft gesetzt werden.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // use the following to replace the default channel with InMemoryChannel.
        // this can also be applied to ServerTelemetryChannel as well.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetry();
    }
```

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

*1. Ich möchte zusätzlich zu den automatisch erfassten Telemetriedaten einige zusätzliche Telemetriedaten nachverfolgen. Wie kann ich hierbei vorgehen?*

* Beschaffen Sie eine Instanz von `TelemetryClient`, indem Sie die Konstruktorinjektion nutzen und die erforderliche `TrackXXX()`-Methode aufrufen. Es wird nicht empfohlen, in der ASP.NET Core-Anwendung neue `TelemetryClient`-Instanzen zu erstellen, da eine Singletoninstanz von `TelemetryClient` bereits im DI-Container registriert ist, über den `TelemetryConfiguration` mit der restlichen Telemetrie gemeinsam genutzt wird. Die Erstellung einer neuen `TelemetryClient`-Instanz wird nur empfohlen, wenn dafür gegenüber der restlichen Telemetrie eine separate Konfiguration vorhanden sein muss. Im folgenden Beispiel wird veranschaulicht, wie Sie zusätzliche Telemetrie über einen Controller nachverfolgen.

```csharp
public class HomeController : Controller
{
    private TelemetryClient telemetry;

    // use constructor injection to get TelemetryClient instance
    public HomeController(TelemetryClient telemetry)
    {
        this.telemetry = telemetry;
    }

    public IActionResult Index()
    {
        // call required TrackXXX method.
        this.telemetry.TrackEvent("HomePageRequested");
        return View();
    }
```

 Eine Beschreibung des Meldens von benutzerdefinierten Daten in Application Insights finden Sie unter [Application Insights-API für benutzerdefinierte Ereignisse und Metriken](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics/).

*2. Für einige Visual Studio-Vorlagen wurde die Erweiterungsmethode „UseApplicationInsights()“ in IWebHostBuilder verwendet, um Application Insights zu aktivieren. Ist diese Verwendung noch gültig?*

* Die Aktivierung von Application Insights mit dieser Methode ist noch gültig und wird beim Visual Studio-Onboarding und auch für die Azure-Web-App-Erweiterungen verwendet. Es wird aber empfohlen, `services.AddApplicationInsightsTelemery()` zu nutzen, da bei diesem Ansatz Überladungen zum Steuern der Konfiguration eingesetzt werden können. Beide Methoden bewirken intern dasselbe. Wenn keine benutzerdefinierte Konfiguration angewendet wird, ist also das Aufrufen beider Methoden in Ordnung.

*3. Ich stelle meine ASP.NET Core-Anwendung in Azure-Web-Apps bereit. Sollte ich trotzdem die Application Insights-Erweiterung über Web-Apps aktivieren?*

* Wenn das SDK wie in diesem Artikel gezeigt zur Buildzeit installiert wird, ist es nicht erforderlich, die Application Insights-Erweiterung über das Web-Apps-Portal zu aktivieren. Auch bei installierter Erweiterung werden keine Schritte ausgeführt, wenn erkannt wird, dass das SDK der Anwendung bereits hinzugefügt wurde. Durch das Aktivieren von Application Insights über die Erweiterung sparen Sie sich das Installieren und Aktualisieren des SDK für Ihre Anwendung. Die Aktivierung von Application Insights gemäß diesem Artikel bietet aus den unten genannten Gründen mehr Flexibilität.
    1. Application Insights-Telemetrie funktioniert weiterhin unter:
        1. allen Betriebssystemen (Windows, Linux, Mac).
        1. allen Veröffentlichungsmodi (Eigenständig oder Frameworkabhängig).
        1. allen Zielframeworks, z. B. vollständigem .NET Framework.
        1. allen Hostingoptionen (Azure-Web-App, VMs, Linux, Container, AKS, außerhalb von Azure).
    1. Die Telemetriedaten können beim Debuggen über Visual Studio lokal angezeigt werden.
    1. Ermöglicht das Nachverfolgen von zusätzlichen benutzerdefinierten Telemetriedaten per `TrackXXX()`-API.
    1. Ermöglicht die vollständige Steuerung der Konfiguration.

*4. Kann ich die Application Insights-Überwachung mit Tools wie Statusmonitor aktivieren?*

*  Nein. [Statusmonitor](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now) und die neue Ersatzanwendung [IISConfigurator](https://github.com/Microsoft/ApplicationInsights-Announcements/issues/21) unterstützen derzeit nur ASP.NET. Das Dokument wird aktualisiert, wenn die Unterstützung für die ASP.NET Core-Anwendung verfügbar ist.

*5. Ich verwende eine ASP.NET Core 2.0-Anwendung. Ist Application Insights hierfür nicht automatisch aktiviert, ohne dass ich etwas tun muss?*

* `Microsoft.AspNetCore.All` Im Metapaket 2.0 war das Application Insights SDK (Version 2.1.0) enthalten. Wenn Sie die Anwendung unter dem Visual Studio-Debugger ausführen, wird Application Insights von Visual Studio aktiviert, und die Telemetriedaten werden lokal in der IDE angezeigt. Telemetriedaten wurden nicht an den Application Insights-Dienst gesendet, sofern nicht explizit ein Instrumentierungsschlüssel angegeben wurde. Wir empfehlen Ihnen, die Anleitung in diesem Artikel zu befolgen, um Application Insights zu aktivieren (auch für 2.0-Apps).

*6. Ich führe meine Anwendung unter Linux aus. Werden alle Features auch unter Linux unterstützt?*

* Ja. Die Featureunterstützung für das SDK ist auf allen Plattformen gleich. Es gelten lediglich die folgenden Ausnahmen:
    1. Leistungsindikatoren werden außerhalb von Windows noch nicht unterstützt. Dieses Dokument wird aktualisiert, wenn Linux-Unterstützung hinzugefügt wird.
    1. Auch wenn `ServerTelemetryChannel` standardmäßig aktiviert ist, wird über den Kanal bei der Anwendungsausführung außerhalb von Windows nicht automatisch ein lokaler Speicherordner erstellt, um die Telemetriedaten bei Netzwerkproblemen vorübergehend zu speichern. Diese Einschränkung bewirkt, dass Telemetriedaten verloren gehen, wenn vorübergehende Netzwerk- oder Serverprobleme auftreten. Benutzer können dieses Problem umgehen, indem sie einen lokalen Ordner für den Kanal konfigurieren. Dies ist unten dargestellt.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // The following will configure channel to use the given folder to temporarily
        // store telemetry items during network or application insights server issues.
        // User should ensure that the given folder already exists,
        // and that application has read/write permissions.
        services.AddSingleton(typeof(ITelemetryChannel),
                                new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
        services.AddApplicationInsightsTelemetry();
    }
```
