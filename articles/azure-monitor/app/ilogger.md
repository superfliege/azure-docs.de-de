---
title: Untersuchen von .NET-Ablaufverfolgungsprotokollen in Azure Application Insights mit ILogger
description: Beispiele für die Verwendung der Azure Application Insights-ILogger-Implementierung mit ASP.NET Core- und Konsolenanwendungen.
services: application-insights
author: cijothomas
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 02/19/2019
ms.reviewer: mbullwin
ms.author: cithomas
ms.openlocfilehash: 14c9a453913a9a8080c840df3f23751487b86c34
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58006150"
---
# <a name="ilogger"></a>ILogger

ASP.NET Core unterstützt eine Protokollierungs-API, die mit einer Vielzahl von integrierten und Drittanbieter-Protokollierungsfunktionen funktioniert. Dieser Artikel zeigt den Umgang mit der Protokollierung mithilfe der Application Insights-ILogger-Implementierung sowohl in Konsolen- als auch in ASP.NET Core-Anwendungen. Weitere Informationen zur ILogger-basierten Protokollierung finden Sie [in diesem Artikel](https://docs.microsoft.com/aspnet/core/fundamentals/logging).

## <a name="console-application"></a>Konsolenanwendung

Im Folgenden sehen Sie ein Beispiel für eine Konsolenanwendung, die so konfiguriert wurde, dass ILogger-Ablaufverfolgungen an Application Insights gesendet werden.

Installierte Pakete:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.Extensions.DependencyInjection" Version="2.1.0" />
  <PackageReference Include="Microsoft.Extensions.Logging" Version="2.1.0" />
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
            
        // Add the logging pipelines to use. We are using Application Insights only here.
        services.AddLogging(loggingBuilder =>
        {
        // Optional: Apply filters to configure LogLevel Trace or above is sent to ApplicationInsights for all
        // categories.
        loggingBuilder.AddFilter<ApplicationInsightsLoggerProvider>("", LogLevel.Trace);
            loggingBuilder.AddApplicationInsights("--YourAIKeyHere--");                
        });

        // Build ServiceProvider.
        IServiceProvider serviceProvider = services.BuildServiceProvider();

        ILogger<Program> logger = serviceProvider.GetRequiredService<ILogger<Program>>();

        // Begin a new scope. This is optional. Epecially in case of AspNetCore request info is already
        // present in scope.
        using (logger.BeginScope(new Dictionary<string, object> { { "Method", nameof(Main) } }))
        {
            logger.LogInformation("Logger is working"); // this will be captured by Application Insights.
        }
    }
}
```

## <a name="aspnet-core-application"></a>ASP.NET Core-Anwendung

Im Folgenden sehen Sie ein Beispiel für eine ASP.NET Core-Anwendung, die so konfiguriert wurde, dass ILogger-Ablaufverfolgungen an Application Insights gesendet werden. Mit diesem Beispiel können Sie ILogger-Ablaufverfolgungen aus „Program.cs“, „Startup.cs“ oder einer anderen Controller-/Anwendungslogik senden.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var host = BuildWebHost(args);
        var logger = host.Services.GetRequiredService<ILogger<Program>>();
        logger.LogInformation("From Program. Running the host now.."); // This will be picked up up by AI
        host.Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()                
        .ConfigureLogging(logging =>
        {                
        logging.AddApplicationInsights("ikeyhere");
                
        // Optional: Apply filters to configure LogLevel Trace or above is sent to
        // ApplicationInsights for all categories.
            logging.AddFilter<ApplicationInsightsLoggerProvider>("", LogLevel.Trace);
                
            // Additional filtering For category starting in "Microsoft",
        // only Warning or above will be sent to Application Insights.
        logging.AddFilter<ApplicationInsightsLoggerProvider>("Microsoft", LogLevel.Warning);
        })
        .Build();
}
```

```csharp
public class Startup
{
    private readonly ILogger _logger;

    public Startup(IConfiguration configuration, ILogger<Startup> logger)
    {
        Configuration = configuration;
        _logger = logger;
    }

    public IConfiguration Configuration { get; }

    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
    
    // The following be picked up up by Application Insights.
        _logger.LogInformation("From ConfigureServices. Services.AddMVC invoked"); 
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
        // The following be picked up up by Application Insights.   
            _logger.LogInformation("Configuring for Development environment");
            app.UseDeveloperExceptionPage();
        }
        else
        {
            // The following be picked up up by Application Insights.
            _logger.LogInformation("Configuring for Production environment");
        }

        app.UseMvc();
    }
}
```

```csharp
public class ValuesController : ControllerBase
{
    private readonly ILogger _logger;

    public ValuesController(ILogger<ValuesController> logger)
    {
        _logger = logger;
    }

    // GET api/values
    [HttpGet]
    public ActionResult<IEnumerable<string>> Get()
    {
        // All the following logs will be picked upby Application Insights.
    // and all have ("MyKey", "MyValue") in Properties.
    using (_logger.BeginScope(new Dictionary<string, object> { { "MyKey", "MyValue" } }))
        {           
        _logger.LogInformation("An example of a Information trace..");
        _logger.LogWarning("An example of a Warning trace..");
        _logger.LogTrace("An example of a Trace level message");
        }

        return new string[] { "value1", "value2" };
    }
}
```

In den beiden oben aufgeführten Beispielen wird das eigenständige Paket „Microsoft.Extensions.Logging.ApplicationInsights“ eingesetzt. Standardmäßig verwendet diese Konfiguration für `TelemetryConfiguration` das absolute Minimum zum Senden von Daten an Application Insights. Absolutes Minimum bedeutet, dass als Kanal `InMemoryChannel` verwendet wird, ohne Sampling und ohne Standard-TelemetryInitializers. Dieses Verhalten kann für eine Konsolenanwendung wie im folgenden Beispiel gezeigt außer Kraft gesetzt werden.

Installieren Sie dieses zusätzliche Paket:

```xml
<PackageReference Include="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" Version="2.9.1" />
```

Im folgenden Abschnitt wird gezeigt, wie die Standard-`TelemetryConfiguration` außer Kraft gesetzt werden kann. In diesem Beispiel werden `ServerTelemetryChannel`, Sampling und ein benutzerdefinierter `ITelemetryInitializer` konfiguriert.

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

    // Add the logging pipelines to use. We are adding ApplicationInsights only.
    services.AddLogging(loggingBuilder =>
    {
        loggingBuilder.AddApplicationInsights();
    });
```

Der oben beschriebene Ansatz kann zwar in einer ASP.NET Core-Anwendung verwendet werden, aber ein gängigerer Ansatz wäre, die reguläre Anwendungsüberwachung (Anforderungen, Abhängigkeiten usw.) wie unten gezeigt mit der ILogger-Erfassung zu kombinieren.

Installieren Sie dieses zusätzliche Paket:

```xml
<PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.6.1" />
```

Fügen Sie der `ConfigureServices`-Methode Folgendes hinzu. Dieser Code ermöglicht die reguläre Anwendungsüberwachung mit der Standardkonfiguration (ServerTelemetryChannel, LiveMetrics, Anforderungen/Abhängigkeiten, Korrelation usw.).

```csharp
services.AddApplicationInsightsTelemetry("ikeyhere");
```

In diesem Beispiel entspricht die von `ApplicationInsightsLoggerProvider` verwendete Konfiguration der Konfiguration, die von der regulären Anwendungsüberwachung verwendet wird. Aus diesem Grund wird für beide `ILogger`-Ablaufverfolgungen und andere Telemetriedaten (Anforderungen, Abhängigkeiten usw.) derselbe Satz von `TelemetryInitializers`, `TelemetryProcessors` und `TelemetryChannel` ausgeführt. Sie werden korreliert, und Stichproben werden auf dieselbe Weise entnommen bzw. nicht entnommen.

Es gibt bei diesem Verhalten jedoch eine Ausnahme. Die Standard-`TelemetryConfiguration` ist nicht vollständig eingerichtet, wenn die Protokollierung aus `Program.cs` oder `Startup.cs` selbst erfolgt, daher weisen diese Protokolle nicht die Standardkonfiguration auf. Die Konfiguration wird jedoch für alle übrigen Protokolle (Protokolle aus Controllern, Modellen usw.) verwendet.

## <a name="control-logging-level"></a>Steuern des Protokolliergrads

Abgesehen vom Filtern der Protokolle für Code – wie in den oben genannten Beispielen – ist es auch möglich, durch Ändern von `appsettings.json` den Protokolliergrad für erfasste Application Insights-Daten zu steuern. Die [Dokumentation mit den Grundlagen der ASP.NET-Protokollierung](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-filtering) zeigt, wie sich dies erreichen lässt. Für Application Insights speziell lautet der Name des Anbieteralias `ApplicationInsights` – wie im unten stehenden Beispiel gezeigt, das `ApplicationInsights` so konfiguriert, dass für alle Kategorien nur Protokolle mit dem Schweregrad `Warning` und höher erfasst werden.

```json
{
  "Logging": {
    "ApplicationInsights": {
      "LogLevel": {
        "Default": "Warning"
      }
    },
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen:

- [ILogger-basierte Protokollierung](https://docs.microsoft.com/aspnet/core/fundamentals/logging)
- [.NET-Ablaufverfolgungsprotokolle](../../azure-monitor/app/asp-net-trace-logs.md)
