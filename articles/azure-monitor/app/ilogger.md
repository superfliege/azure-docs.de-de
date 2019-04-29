---
title: Untersuchen von .NET-Ablaufverfolgungsprotokollen in Azure Application Insights mit ILogger
description: Beispiele für die Verwendung des Azure Application Insights-ILogger-Anbieters mit ASP.NET Core- und Konsolenanwendungen.
services: application-insights
author: cijothomas
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 02/19/2019
ms.reviewer: mbullwin
ms.author: cithomas
ms.openlocfilehash: 615eaa3df7cabad72ac321978eb01d93a7bfa988
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2019
ms.locfileid: "59608284"
---
# <a name="applicationinsightsloggerprovider-for-net-core-ilogger-logs"></a>ApplicationInsightsLoggerProvider für .NET Core-ILogger-Protokolle

ASP.NET Core unterstützt eine Protokollierungs-API, die mit verschiedenen Arten von integrierten und Drittanbieter-Protokollierungsfunktionen funktioniert. Die Protokollierung erfolgt durch Aufrufen von Log() oder einer Variante davon für `ILogger`-Instanzen. Dieser Artikel zeigt, wie Sie `ApplicationInsightsLoggerProvider` verwenden, um `ILogger`-Protokolle sowohl in Konsolen- als auch ASP.NET Core-Anwendungen zu erfassen. Dieser Artikel beschreibt auch, wie `ApplicationInsightsLoggerProvider` in andere Application Insights-Telemetrie integriert wird.
Weitere Informationen zur Protokollierung in ASP.NET Core finden Sie in [diesem Artikel](https://docs.microsoft.com/aspnet/core/fundamentals/logging).

## <a name="aspnet-core-applications"></a>ASP.NET Core-Anwendungen

Ab [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) Version 2.7.0-beta3 ist `ApplicationInsightsLoggerProvider` standardmäßig aktiviert, wenn die normale Application Insights-Überwachung mit einer der Standardmethoden aktiviert wird – durch Aufruf der Erweiterungsmethode `UseApplicationInsights` für IWebHostBuilder oder der Erweiterungsmethode `AddApplicationInsightsTelemetry` für IServiceCollection. `ILogger`-Protokolle, die von `ApplicationInsightsLoggerProvider` erfasst wurden, unterliegen derselben Konfiguration wie alle anderen gesammelten Telemetriedaten, z.B.: sie weisen die gleiche Gruppe von `TelemetryInitializer` und `TelemetryProcessor` auf, verwenden denselben `TelemetryChannel` und werden auf die gleiche Weise wie alle andere Telemetriedaten korreliert und Stichproben dafür erstellt.  Wenn Sie diese oder eine höhere Version des SDK verwenden, ist zum Erfassen von `ILogger`-Protokollen keine Aktion erforderlich.

Standardmäßig werden nur `ILogger`-Protokolle für `Warning` oder höher (aus allen Kategorien) an Application Insights gesendet. Dieses Verhalten kann geändert werden, indem Sie Filter wie [hier](#control-logging-level) gezeigt anwenden. Außerdem sind zusätzliche Schritte erforderlich, wenn `ILogger`-Protokolle aus `Program.cs` oder `Startup.cs` wie [hier](#capturing-ilogger-logs-from-startupcs-programcs-in-aspnet-core-applications) gezeigt erfasst werden sollen.

Wenn Sie eine frühere Version des Microsoft.ApplicationInsights.AspNet SDK verwenden oder nur ApplicationInsightsLoggerProvider ohne jede andere Application Insights-Überwachung verwenden möchten, führen Sie die folgenden Schritte aus.

1. Installieren Sie das NuGet-Paket.

```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.Extensions.Logging.ApplicationInsights" Version="2.9.1" />  
    </ItemGroup>
```

2. Ändern Sie `Program.cs` wie nachfolgend gezeigt.

```csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureLogging(
            builder =>
            {
                // Providing an instrumentation key here is required if you are using
                // standalone package Microsoft.Extensions.Logging.ApplicationInsights
                // or if you want to capture logs from early in the application startup
                // pipeline from Startup.cs or Program.cs itself.
                builder.AddApplicationInsights("ikey");

                // Optional: Apply filters to control what logs are sent to Application Insights.
                // The following configures LogLevel Information or above to be sent to
                // Application Insights for all categories.
                builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                                 ("", LogLevel.Information);
            }
        );
}
```

Mit dem oben gezeigten Code wird `ApplicationInsightsLoggerProvider` konfiguriert. Es folgt ein Beispiel für die Klasse „Controller“, bei dem `ILogger` zum Senden von Protokollen verwendet wird, die von Application Insights erfasst werden.

```csharp
public class ValuesController : ControllerBase
{
    private readonly `ILogger` _logger;

    public ValuesController(ILogger<ValuesController> logger)
    {
        _logger = logger;
    }

    // GET api/values
    [HttpGet]
    public ActionResult<IEnumerable<string>> Get()
    {
        // All the following logs will be picked up by Application Insights.
        // and all of them will have ("MyKey", "MyValue") in Properties.
        using (_logger.BeginScope(new Dictionary<string, object> { { "MyKey", "MyValue" } }))
            {
                _logger.LogWarning("An example of a Warning trace..");
                _logger.LogError("An example of an Error level message");
            }
        return new string[] { "value1", "value2" };
    }
}
```

### <a name="capturing-ilogger-logs-from-startupcs-programcs-in-aspnet-core-applications"></a>Erfassen von ILogger-Protokollen aus „Startup.cs“ und „Program.cs“ in ASP.Net Core-Anwendungen

Mit dem neuen ApplicationInsightsLoggerProvider können Protokolle zu einem frühen Zeitpunkt in der Startpipeline der Anwendung erfasst werden. Obwohl ApplicationInsightsLoggerProvider in Application Insights (ab Version 2.7.0-beta3) automatisch aktiviert ist, wird erst später in der Pipeline ein Instrumentierungsschlüssel eingerichtet, sodass nur Protokolle der Klasse „Controller“ oder anderer Klassen erfasst werden. Um alle Protokolle beginnend mit `Program.cs` und `Startup.cs` selbst zu erfassen, müssen Sie ApplicationInsightsLoggerProvider mit einen Instrumentierungsschlüssel explizit aktivieren. Zudem ist zu beachten, dass `TelemetryConfiguration` nicht vollständig eingerichtet ist, wenn etwas aus `Program.cs` oder `Startup.cs` selbst protokolliert wird. Daher verwenden diese Protokolle eine absolute Minimalkonfiguration mit InMemoryChannel, ohne Sampling und ohne standardmäßige Telemetrieinitialisierer oder -prozessoren.

Es folgen Beispiele für `Program.cs` und `Startup.cs` unter Verwendung dieser Funktion.

#### <a name="example-programcs"></a>Beispiel für „Program.cs“

```csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        var host = CreateWebHostBuilder(args).Build();
        var logger = host.Services.GetRequiredService<ILogger<Program>>();
        // This will be picked up by AI
        logger.LogInformation("From Program. Running the host now..");
        host.Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureLogging(
        builder =>
            {
            // providing an instrumentation key here is required if you are using
            // standalone package Microsoft.Extensions.Logging.ApplicationInsights
            // or if you want to capture logs from early in the application startup 
            // pipeline from Startup.cs or Program.cs itself.
            builder.AddApplicationInsights("ikey");

            // Adding the filter below to ensure logs of all severity from Program.cs
            // is sent to ApplicationInsights.
            // Replace YourAppName with the namespace of your application's Program.cs
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             ("YourAppName.Program", LogLevel.Trace);
            // Adding the filter below to ensure logs of all severity from Startup.cs
            // is sent to ApplicationInsights.
            // Replace YourAppName with the namespace of your application's Startup.cs
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             ("YourAppName.Startup", LogLevel.Trace);
            }
        );
}
```

#### <a name="example-startupcs"></a>Beispiel für „Startup.cs“

```csharp
public class Startup
{
    private readonly `ILogger` _logger;

    public Startup(IConfiguration configuration, ILogger<Startup> logger)
    {
        Configuration = configuration;
        _logger = logger;
    }

    public IConfiguration Configuration { get; }

    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // The following will be picked up by Application Insights.
        _logger.LogInformation("Logging from ConfigureServices.");
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            // The following will be picked up by Application Insights.
            _logger.LogInformation("Configuring for Development environment");
            app.UseDeveloperExceptionPage();
        }
        else
        {
            // The following will be picked up by Application Insights.
            _logger.LogInformation("Configuring for Production environment");
        }

        app.UseMvc();
    }
}
```

## <a name="migrating-from-old-applicationinsightsloggerprovider"></a>Migrieren vom alten ApplicationInsightsLoggerProvider

Frühere Versionen des Microsoft.ApplicationInsights.AspNet SDK als 2.7.0-beta2 unterstützten einen Protokollierungsanbieter, der nun veraltet ist. Dieser Anbieter wurde mit der Erweiterungsmethode `AddApplicationInsights()` von `ILoggerFactory` aktiviert. Dieser Anbieter ist nun veraltet, und Benutzern wird empfohlen, zum neuen Anbieter zu migrieren. Die Migration umfasst zwei Schritte.

1. Entfernen des Aufrufs von ILoggerFactory.AddApplicationInsights() aus der `Startup.Configure()`-Methode, um eine doppelte Protokollierung zu vermeiden.
2. Erneutes Anwenden aller Filterregeln im Code, da sie vom neuen Anbieter nicht berücksichtigt werden. Überladungen von ILoggerFactory.AddApplicationInsights() erforderten mindestens LogLevel- oder Filterfunktionen. Beim neuen Anbieter ist das Filtern Teil des Protokollierungsframeworks und wird nicht vom Application Insights-Anbieter durchgeführt. Deshalb sollten alle per `ILoggerFactory.AddApplicationInsights()`-Überladungen bereitgestellte Filter entfernt werden, und Filterregeln sollten gemäß [dieser](#control-logging-level) Anweisungen bereitgestellt werden. Wenn Sie `appsettings.json` zum Filtern der Protokollierung verwenden, funktioniert dies auch beim neuen Anbieter, da beide den gleichen Anbieteralias **ApplicationInsights** verwenden.

Zwar kann der alte Anbieter weiterhin verwendet werden (er ist jetzt veraltet und wird erst beim Wechsel zur Hauptversion 3.xx entfernt), doch wird das Migrieren zum neuen Anbieter aus den folgenden Gründen dringend empfohlen.

1. Beim vorherigen Anbieter wurden [Bereiche](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-scopes) nicht unterstützt. Beim neuen Anbieter werden Eigenschaften aus dem Bereich den gesammelten Telemetriedaten automatisch als benutzerdefinierte Eigenschaften hinzugefügt.
2. Protokolle können jetzt zu einem viel früheren Zeitpunkt in der Startpipeline der Anwendung erfasst werden, z.B.: Protokolle können jetzt aus den Klassen „Program“ und „Startup“ erfasst werden.
3. Beim neuen Anbieter erfolgt die Filterung auf der Frameworkebene. Das Filtern von Protokollen für den Application Insights-Anbieter kann auf genau die gleiche Weise wie für andere Anbieter durchgeführt werden, einschließlich integrierte Anbieter wie Console, Debug usw. Es können auch dieselben Filter für mehrere Anbieter angewendet werden.
4. Die [empfohlene](https://github.com/aspnet/Announcements/issues/255) Methode in ASP.NET Core (2.0 oder höher) zum Aktivieren von Protokollierungsanbietern ist die Verwendung von Erweiterungsmethoden für ILoggingBuilder in `Program.cs` selbst.

> [!Note]
> Der neue Anbieter ist für Anwendungen verfügbar, die auf `NETSTANDARD2.0` oder höher ausgelegt sind. Wenn Ihre Anwendung auf ältere .NET Core-Versionen wie .NET Core 1.1 oder das .NET Framework ausgelegt ist, verwenden Sie weiterhin den alten Anbieter.

## <a name="console-application"></a>Konsolenanwendung

Der folgende Code zeigt ein Beispiel für eine Konsolenanwendung, die so konfiguriert wurde, dass `ILogger`-Ablaufverfolgungen an Application Insights gesendet werden.

Installierte Pakete:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.Extensions.DependencyInjection" Version="2.1.0" />  
  <PackageReference Include="Microsoft.Extensions.Logging.ApplicationInsights" Version="2.9.1" />
  <PackageReference Include="Microsoft.Extensions.Logging.Console" Version="2.1.0" />
</ItemGroup>
```

```csharp
class Program
{
    static void Main(string[] args)
    {
        // Create DI container.
        IServiceCollection services = new ServiceCollection();

        // Channel is explicitly configured to do flush on it later.
        var channel = new InMemoryChannel();
        services.Configure<TelemetryConfiguration>(
            (config) =>
            {
                config.TelemetryChannel = channel;
            }
        );

        // Add the logging pipelines to use. We are using Application Insights only here.
        services.AddLogging(builder =>
        {
            // Optional: Apply filters to configure LogLevel Trace or above is sent to
            // Application Insights for all categories.
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             ("", LogLevel.Trace);
            builder.AddApplicationInsights("--YourAIKeyHere--");
        });

        // Build ServiceProvider.
        IServiceProvider serviceProvider = services.BuildServiceProvider();

        ILogger<Program> logger = serviceProvider.GetRequiredService<ILogger<Program>>();

        // Begin a new scope. This is optional.
        using (logger.BeginScope(new Dictionary<string, object> { { "Method", nameof(Main) } }))
        {
            logger.LogInformation("Logger is working"); // this will be captured by Application Insights.
        }

        // Explicitly call Flush() followed by sleep is required in Console Apps.
        // This is to ensure that even if application terminates, telemetry is sent to the back-end.
        channel.Flush();
        Thread.Sleep(1000);
    }
}
```

Im oben gezeigten Beispiel wird das eigenständige Paket `Microsoft.Extensions.Logging.ApplicationInsights` verwendet. Standardmäßig verwendet diese Konfiguration für `TelemetryConfiguration` das absolute Minimum zum Senden von Daten an Application Insights. Absolutes Minimum bedeutet, dass als Kanal `InMemoryChannel` verwendet wird, ohne Sampling und ohne Standard-TelemetryInitializers. Dieses Verhalten kann für eine Konsolenanwendung wie im folgenden Beispiel gezeigt außer Kraft gesetzt werden.

Installieren Sie dieses zusätzliche Paket:

```xml
<PackageReference Include="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" Version="2.9.1" />
```

Im folgenden Abschnitt wird gezeigt, wie die Standard-`TelemetryConfiguration` mithilfe der Methode `services.Configure<TelemetryConfiguration>()` außer Kraft gesetzt werden kann. In diesem Beispiel werden `ServerTelemetryChannel` und Sampling eingerichtet und ein benutzerdefinierter `ITelemetryInitializer` zur `TelemetryConfiguration` hinzugefügt.

```csharp
    // Create DI container.
    IServiceCollection services = new ServiceCollection();
    var serverChannel = new ServerTelemetryChannel();
    services.Configure<TelemetryConfiguration>(
        (config) =>
        {
            config.TelemetryChannel = serverChannel;
            config.TelemetryInitializers.Add(new MyTelemetryInitalizer());
            config.DefaultTelemetrySink.TelemetryProcessorChainBuilder.UseSampling(5);
            serverChannel.Initialize(config);
        }
    );

    // Add the logging pipelines to use. We are adding Application Insights only.
    services.AddLogging(loggingBuilder =>
    {
        loggingBuilder.AddApplicationInsights();
    });

    ........
    ........

    // Explicitly call Flush() followed by sleep is required in Console Apps.
    // This is to ensure that even if application terminates, telemetry is sent to the back-end.
    serverChannel.Flush();
    Thread.Sleep(1000);
```

## <a name="control-logging-level"></a>Steuern des Protokolliergrads

Die `ILogger`-Infrastruktur in ASP.Net Core verfügt über einen integrierten Mechanismus zum Anwenden der [Filterung](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-filtering) von Protokollen. Dadurch können Benutzer steuern, welche Protokolle an die einzelnen registrierten Anbieter gesendet werden, einschließlich Application Insights-Anbieter. Diese Filterung kann entweder in der Konfiguration (normalerweise mit der Datei `appsettings.json`) oder im Code erfolgen. Diese Funktion wird durch das Framework bereitgestellt und ist nicht für den Application Insights-Anbieter spezifisch.

Beispiele für das Anwenden der Filterregeln auf ApplicationInsightsLoggerProvider sind weiter unten angegeben.

### <a name="create-filter-rules-in-configuration-with-appsettingsjson"></a>Erstellen von Filterregeln in der Konfiguration mit „appsettings.json“

Für ApplicationInsightsLoggerProvider lautet der Anbieteralias `ApplicationInsights`. Der unten gezeigte Abschnitt in `appsettings.json` konfiguriert, dass Protokolle für `Warning` und höher aus allen Kategorien sowie `Error` und höher aus Kategorien beginnend mit „Microsoft“ an `ApplicationInsightsLoggerProvider` gesendet werden.

```json
{
  "Logging": {
    "ApplicationInsights": {
      "LogLevel": {
        "Default": "Warning",
        "Microsoft": "Error"
      }
    },
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="create-filter-rules-in-code"></a>Erstellen von Filterregeln im Code

Der unten gezeigte Codeausschnitt konfiguriert, dass Protokolle für `Warning` und höher aus allen Kategorien sowie `Error` und höher aus Kategorien beginnend mit „Microsoft“ an `ApplicationInsightsLoggerProvider` gesendet werden. Diese Konfiguration entspricht der oben gezeigten Konfiguration in `appsettings.json`.

```csharp
    WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .ConfigureLogging(logging =>
      logging.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                        ("", LogLevel.Warning)
             .AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                        ("Microsoft", LogLevel.Error);
```

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

*1. Was ist der alte und was ist der neue ApplicationInsightsLoggerProvider?*

* [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) mit integriertem ApplicationInsightsLoggerProvider (Microsoft.ApplicationInsights.AspNetCore.Logging.ApplicationInsightsLoggerProvider), der mithilfe der ILoggerFactory-Erweiterungsmethoden aktiviert wurde. Dieser Anbieter ist ab Version 2.7.0-beta2 als veraltet markiert und wird beim Wechsel zur nächsten Hauptversion vollständig entfernt. [Dieses](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) Paket selbst ist nicht veraltet und wird zum Aktivieren der Überwachung von Anforderungen, Abhängigkeiten usw. benötigt.

* Die empfohlene Alternative ist das neue eigenständige Paket [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights), das einen verbesserten ApplicationInsightsLoggerProvider (Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider) und Erweiterungsmethoden in ILoggerBuilder für dessen Aktivierung enthält.

* [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) Version 2.7.0-beta3 und höher ist vom oben genannten Paket abhängig und aktiviert die `ILogger`-Erfassung automatisch.

*2. Einige `ILogger`-Protokolle werden in Application Insights zweimal angezeigt?*

* Diese Duplizierung ist möglich, wenn Sie die ältere (jetzt veraltete) Version von `ApplicationInsightsLoggerProvider` durch Aufruf von `AddApplicationInsights` für `ILoggerFactory` aktiviert haben. Überprüfen Sie, ob Ihre `Configure`-Methode Folgendes enthält, und entfernen sie den Eintrag.

   ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
        loggerFactory.AddApplicationInsights(app.ApplicationServices, LogLevel.Warning);
        // ..other code.
    }
   ```

* Wenn beim Debuggen aus Visual Studio eine doppelte Protokollierung auftritt, ändern Sie den zum Aktivieren von Application Insights verwendeten Code wie folgt, indem Sie `EnableDebugLogger` auf „false“ festlegen. Dieses Duplizierungsproblem und dessen Behebung trifft nur beim Debuggen der Anwendung zu.

   ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        ApplicationInsightsServiceOptions options = new ApplicationInsightsServiceOptions();
        options.EnableDebugLogger = false;
        services.AddApplicationInsightsTelemetry(options);
        // ..other code.
    }
   ```

*3. Ich habe ein Update auf [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) Version 2.7.0-beta3 durchgeführt und stelle jetzt fest, dass Protokolle aus `ILogger` automatisch erfasst werden. Wie kann ich dieses Feature vollständig deaktivieren?*

* Allgemeine Informationen zum Filtern von Protokollen finden Sie in [diesem](../../azure-monitor/app/ilogger.md#control-logging-level) Abschnitt. Zum Deaktivieren von ApplicationInsightsLoggerProvider verwenden Sie `LogLevel.None`.

  Im Code

    ```csharp
        builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                          ("", LogLevel.None);
    ```

  In der Konfiguration

    ```json
    {
      "Logging": {
        "ApplicationInsights": {
          "LogLevel": {
            "Default": "None"
          }
    }
    ```

*4. Warum weisen einige `ILogger`-Protokolle nicht dieselben Eigenschaften wie andere Protokolle auf?*

* Application Insights erfasst und sendet `ILogger`-Protokolle anhand derselben `TelemetryConfiguration`, die auch für alle anderen Telemetriedaten verwendet wird. Es gibt eine Ausnahme von dieser Regel. Die Standard-`TelemetryConfiguration` ist nicht vollständig eingerichtet, wenn die Protokollierung aus `Program.cs` oder `Startup.cs` selbst erfolgt, daher weisen Protokolle aus diesen Quellen nicht die Standardkonfiguration auf und führen somit auch nicht alle `TelemetryInitializer` und `TelemetryProcessor` aus.

*5. Ich verwende das eigenständige Paket „Microsoft.Extensions.Logging.ApplicationInsights“ und möchte einige zusätzliche benutzerdefinierte Telemetriedaten manuell protokollieren. Wie sollte ich dazu vorgehen?*

* Bei Verwendung des eigenständigen Pakets wird `TelemetryClient` in den DI-Container nicht eingefügt. Deshalb wird erwartet, dass die Benutzer eine neue Instanz von `TelemetryClient` erstellen und dazu die gleiche Konfiguration wie der Protokollanbieter verwenden (siehe das folgende Beispiel). So wird sichergestellt, dass dieselbe Konfiguration für alle benutzerdefinierten Telemetriedaten sowie für die von ILogger erfassten Telemetriedaten verwendet wird.

```csharp
public class MyController : ApiController
{
   // This telemtryclient can be used to track additional telemetry using TrackXXX() api.
   private readonly TelemetryClient _telemetryClient;
   private readonly ILogger _logger;

   public MyController(IOptions<TelemetryConfiguration> options, ILogger<MyController> logger)
   {
        _telemetryClient = new TelemetryClient(options.Value);
        _logger = logger;
   }  
}
```

> [!NOTE]
> Bitte beachten Sie: Wenn das Paket „Microsoft.ApplicationInsights.AspNetCore“ zum Aktivieren von Application Insights verwendet wird, sollte das vorstehende Beispiel geändert werden, damit `TelemetryClient` direkt in den Konstruktor abgerufen wird. Ein vollständiges Beispiel finden Sie [hier](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core-no-visualstudio#frequently-asked-questions).


*6. Welcher Typ von Application Insights-Telemetriedaten wird aus `ILogger`-Protokollen erzeugt, oder wo kann ich `ILogger`-Protokolle in Application Insights anzeigen?*

* ApplicationInsightsLoggerProvider erfasst `ILogger`-Protokolle und erstellt daraus `TraceTelemetry`. Wenn ein Ausnahmeobjekt an die Log()-Methode in ILogger übergeben wird, wird anstelle einer `TraceTelemetry` eine `ExceptionTelemetry` erstellt. Diese Telemetrieelemente sind an denselben Orten wie jede andere `TraceTelemetry` oder `ExceptionTelemetry` für Application Insights zu finden, einschließlich Portal, Analysen oder lokalem Visual Studio-Debugger.
Wenn Sie immer `TraceTelemetry` senden möchten, verwenden Sie den Codeausschnitt ```builder.AddApplicationInsights((opt) => opt.TrackExceptionsAsExceptionTelemetry = false);```.

*7. Ich habe das SDK nicht installiert und verwende die Azure-Web-App-Erweiterung, um Application Insights für meine ASP.Net Core-Anwendungen zu aktivieren. Wie verwende ich den neuen Anbieter?*

* Die Application Insights-Erweiterung in Azure-Web-App verwendet den alten-Anbieter. Filterregeln können in `appsettings.json` für Ihre Anwendung geändert werden. Wenn Sie den neuen Anbieter nutzen möchten, verwenden Sie die Instrumentierung zur Buildzeit, indem Sie NuGet-Abhängigkeit für das SDK anwenden. Dieses Dokument wird aktualisiert, wenn die Erweiterung zur Verwendung des neuen Anbieters wechselt.

*8. Ich verwende das eigenständige Paket Microsoft.Extensions.Logging.ApplicationInsights und aktiviere den Application Insights-Anbieter durch Aufruf von builder.AddApplicationInsights("ikey"). Gibt es eine Möglichkeit, den Instrumentierungsschlüssel aus der Konfiguration abzurufen?*


* Ändern Sie `Program.cs` und `appsettings.json` wie unten gezeigt.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .ConfigureLogging((hostingContext, logging) =>
            {
                // hostingContext.HostingEnvironment can be used to determine environments as well.
                var appInsightKey = hostingContext.Configuration["myikeyfromconfig"];
                logging.AddApplicationInsights(appInsightKey);
            });
}
```

Entsprechender Abschnitt aus `appsettings.json`

```json
{
  "myikeyfromconfig": "putrealikeyhere"
}
```

Der oben gezeigte Code ist nur bei Verwendung des eigenständigen Protokollierungsanbieters erforderlich. Bei der normalen Application Insights-Überwachung wird der Instrumentierungsschlüssel automatisch aus dem Konfigurationspfad `ApplicationInsights:Instrumentationkey` geladen, und `appsettings.json` sollte wie nachfolgend gezeigt aussehen.

```json
{
  "ApplicationInsights":
    {
        "Instrumentationkey":"putrealikeyhere"
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen:

* [Protokollierung in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging)
* [.NET-Ablaufverfolgungsprotokolle in Application Insights](../../azure-monitor/app/asp-net-trace-logs.md)
