---
title: Azure Application Insights für ASP.NET Core | Microsoft-Dokumentation
description: Überwachen Sie Webanwendungen auf Verfügbarkeit, Leistung und Auslastung.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/03/2018
ms.author: mbullwin
ms.openlocfilehash: 6635906b6aae418fa3754f1152fae3e0d8903ffc
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989764"
---
# <a name="application-insights-for-aspnet-core"></a>Application Insights für ASP.NET Core

Azure Application Insights ermöglicht eine eingehende Überwachung Ihrer Webanwendung bis hin zur Codeebene. Mit dieser Lösung können Sie ganz einfach die Verfügbarkeit, Leistung und Nutzung Ihrer Webanwendung überwachen. Sie können auch Fehler in Ihrer Anwendung schnell erkennen und diagnostizieren, ohne darauf warten zu müssen, dass diese Fehler von Benutzern gemeldet werden.

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie in Visual Studio eine ASP.NET Core-Beispielanwendung für [Razor-Seiten](https://docs.microsoft.com/aspnet/core/mvc/razor-pages/?tabs=visual-studio) erstellen und mit der Überwachung unter Verwendung von Azure Application Insights beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- .NET Core 2.0.0 SDK oder eine höhere Version
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) (Version 15.7.3 oder höher) mit der Workload für ASP.NET und Webentwicklung 

## <a name="create-an-aspnet-core-project-in-visual-studio"></a>Erstellen eines ASP.NET Core-Projekts in Visual Studio

1. Klicken Sie mit der rechten Maustaste auf **Visual Studio 2017**, und starten Sie das Programm als Administrator.
2. Klicken Sie auf **Datei** > **Neu** > **Projekt** (STRG+UMSCHALT+N).

   ![Screenshot: „Datei“ > „Neu“ > „Projekt“ in Visual Studio – Menü](./media/app-insights-asp-net-core/001-new-project.png)

3. Erweitern Sie **Visual C#**, und klicken Sie auf **.NET Core** > **ASP.NET Core-Webanwendung**. Geben Sie unter **Name** > einen Namen und unter **Projektmappenname** einen Projektmappennamen ein, und aktivieren Sie das Kontrollkästchen **Neues Git-Repository erstellen**.

   ![Screenshot: „Datei“ > „Neu“ > „Projekt“ in Visual Studio – Assistent](./media/app-insights-asp-net-core/002-asp-net-core-web-application.png)

4. Klicken Sie auf **.NET Core** > **ASP.NET Core 2.0** > **Webanwendung** > **OK**.

    ![Screenshot: „Datei“ > „Neu“ > „Projekt“ in Visual Studio – Auswahlmenü](./media/app-insights-asp-net-core/003-web-application.png)

## <a name="application-insights-search"></a>Application Insights-Suche

Standardmäßig können Sie in Visual Studio 2015 Update 2 oder höher bei einem auf ASP.NET Core 2+ basierenden Projekt die [Application Insights-Suche](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio) nutzen, noch bevor Sie Application Insights explizit zu Ihrem Projekt hinzugefügt haben.

So testen Sie diese Funktionalität:

1. Führen Sie Ihre App aus, indem Sie auf IIS Express klicken: ![Screenshot: IIS Express-Symbol in Visual Studio](./media/app-insights-asp-net-core/004-iis-express.png)

2. Wählen Sie **Ansicht** > **Weitere Fenster** > **Application Insights-Suche** aus.

   ![Screenshot: Visual Studio-Diagnosetools](./media/app-insights-asp-net-core/005-view-other-windows-search.png)

3. Die Telemetriedaten der Debugsitzung stehen derzeit nur für lokale Analysen zur Verfügung. Zum vollständigen Aktivieren von Application Insights wählen Sie oben rechts die Option **Telemetriebereitschaft** aus, oder führen Sie die Schritte im folgenden Abschnitt aus.

   ![Screenshot: Visual Studio-Fenster „Application Insights-Suche“](./media/app-insights-asp-net-core/006-search.png)

> [!NOTE]
> Weitere Informationen dazu, wie in Visual Studio Funktionen wie [Application Insights-Suche](app-insights-visual-studio.md) und [CodeLens](app-insights-visual-studio-codelens.md) lokal eingeblendet werden, bevor Sie Application Insights zu Ihrem ASP.NET Core-Projekt hinzugefügt haben, finden Sie in der Erläuterung am [Ende dieses Artikels](#Application-Insights-search-continued).

## <a name="add-application-insights-telemetry"></a>Hinzufügen von Application Insights-Telemetrie

1. Klicken Sie auf **Projekt** > **Application Insights-Telemetrie hinzufügen...**. (Sie können auch mit der rechten Maustaste auf **Verbundene Dienste** klicken und „Verbundenen Dienst hinzufügen“ auswählen.)

    ![Screenshot: „Datei“ > „Neu“ > „Projekt“ in Visual Studio – Auswahlmenü](./media/app-insights-asp-net-core/007-project-add-telemetry.png)

2. Wählen Sie **Erste Schritte** aus. (Je nach verwendeter Version von Visual Studio kann der Text geringfügig abweichen. Bei einigen älteren Versionen wird die Schaltfläche **Kostenlos starten** angezeigt.)

    ![Screenshot: „Datei“ > „Neu“ > „Projekt“ in Visual Studio – Auswahlmenü](./media/app-insights-asp-net-core/008-get-started.png)

3. Wählen Sie ein entsprechendes **Abonnement** > **Ressource** > **Registrieren** aus.

## <a name="changes-made-to-your-project"></a>Änderungen an Ihrem Projekt

Der durch Application Insights entstehende Mehraufwand ist überschaubar. Die Änderungen, die durch Hinzufügen von Application Insights-Telemetrie an Ihrem Projekt vorgenommen werden, können Sie wie folgt überprüfen:

Klicken Sie auf **Ansicht** > **Team Explorer** (STRG+\, STRG+M) > **Projekt** > **Änderungen**.

- Insgesamt wurden vier Änderungen vorgenommen:

  ![Screenshot: Dateien, die durch Hinzufügen von Application Insights geändert wurden](./media/app-insights-asp-net-core/009-changes.png)

- Eine neue Datei wird erstellt:

   _ConnectedService.json_

```json
{
  "ProviderId": "Microsoft.ApplicationInsights.ConnectedService.ConnectedServiceProvider",
  "Version": "8.12.10405.1",
  "GettingStartedDocument": {
    "Uri": "https://go.microsoft.com/fwlink/?LinkID=798432"
  }
}
```

- Drei Dateien werden geändert: (zusätzliche Kommentare werden zum Hervorheben von Änderungen hinzugefügt)

  _appsettings.json_

```json
{
  "Logging": {
    "IncludeScopes": false,
    "LogLevel": {
      "Default": "Warning"
    }
  },
// Changes to file post adding Application Insights Telemetry:
  "ApplicationInsights": {
    "InstrumentationKey": "10101010-1010-1010-1010-101010101010"
  }
}
//
```

  _ContosoDotNetCore.csproj_

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
 <!--Changes to file post adding Application Insights Telemetry:-->
    <ApplicationInsightsResourceId>/subscriptions/2546c5a9-fa20-4de1-9f4a-62818b14b8aa/resourcegroups/Default-ApplicationInsights-EastUS/providers/microsoft.insights/components/DotNetCore</ApplicationInsightsResourceId>
    <ApplicationInsightsAnnotationResourceId>/subscriptions/2546c5a9-fa20-4de1-9f4a-62818b14b8aa/resourcegroups/Default-ApplicationInsights-EastUS/providers/microsoft.insights/components/DotNetCore</ApplicationInsightsAnnotationResourceId>
<!---->
  </PropertyGroup>
  <ItemGroup>
 <!--Changes to file post adding Application Insights Telemetry:-->
    <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.1.1" />
<!---->
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.8" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.4" />
  </ItemGroup>
<!--Changes to file post adding Application Insights Telemetry:-->
  <ItemGroup>
    <WCFMetadata Include="Connected Services" />
  </ItemGroup>
<!---->
</Project>
```

   _Program.cs_

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;

namespace DotNetCore
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
// Change to file post adding Application Insights Telemetry:
                .UseApplicationInsights()
//
                .UseStartup<Startup>()
                .Build();
    }
}
```

## <a name="synthetic-transactions-with-powershell"></a>Synthetische Transaktionen mit PowerShell

So automatisieren Sie Anforderungen für Ihre App mit synthetischen Transaktionen:

1. Führen Sie Ihre App aus, indem Sie auf IIS Express klicken: ![Screenshot: IIS Express-Symbol in Visual Studio](./media/app-insights-asp-net-core/004-iis-express.png)

2. Kopieren Sie die URL aus der Adressleiste des Browsers. Das Format ist `http://localhost:{random port number}`.

   ![Screenshot: URL in der Browseradressleiste](./media/app-insights-asp-net-core/0013-copy-url.png)

3. Führen Sie die folgende PowerShell-Schleife aus, um 100 synthetische Transaktionen für Ihre Test-App zu erstellen. Ändern Sie die Portnummer nach **localhost:** gemäß der URL, die Sie im vorherigen Schritt kopiert haben.

   ```PowerShell
   for ($i = 0 ; $i -lt 100; $i++)
   {
    Invoke-WebRequest -uri http://localhost:50984/
   }
   ```

## <a name="open-application-insights-portal"></a>Öffnen des Application Insights-Portals

Starten Sie nach dem Ausführen der PowerShell-Schleife aus dem vorherigen Abschnitt Application Insights, um die Transaktionen anzuzeigen und sich zu vergewissern, dass Daten gesammelt werden. 

Klicken Sie im Visual Studio-Menü auf **Projekt** > **Application Insights** > **Application Insights-Portal öffnen**

   ![Screenshot: Application Insights-Übersicht](./media/app-insights-asp-net-core/010-portal.png)

> [!NOTE]
> Im obigen Beispielscreenshot ist zu sehen, dass für **Live Stream**, **Ladezeit der Seitenansicht** und **Fehlerhafte Anforderungen** derzeit keine Daten gesammelt werden. Im nächsten Abschnitt wird das Hinzufügen dieser Elemente Schritt für Schritt erläutert. Wenn Sie bereits Daten für **Live Stream** und **Ladezeit der Seitenansicht** sammeln, führen Sie nur die Schritte für **Fehlerhafte Anforderungen** aus.

## <a name="collect-failed-requests-live-stream--page-view-load-time"></a>Sammeln von Daten für „Fehlerhafte Anforderungen“, „Live Stream“ und „Ladezeit der Seitenansicht“

### <a name="failed-requests"></a>Anforderungsfehler

Genau genommen werden Daten für **Fehlerhafte Anforderungen** gesammelt, es sind jedoch noch keine aufgetreten. Zur Beschleunigung des Prozesses kann dem vorhandenen Projekt eine benutzerdefinierte Ausnahme hinzugefügt werden, um eine echte Ausnahme zu simulieren. Falls Ihre App noch in Visual Studio ausgeführt wird, **beenden Sie das Debuggen** (UMSCHALT+F5), bevor Sie den Vorgang fortsetzen.

1. Erweitern Sie im **Projektmappen-Explorer** den Knoten **Seiten** > **About.cshtml**, und öffnen Sie **About.cshtml.cs**.

   ![Screenshot: Projektmappen-Explorer in Visual Studio](./media/app-insights-asp-net-core/011-about.png)

2. Fügen Sie unter ``Message=`` eine Ausnahme hinzu, und speichern Sie die Änderung in der Datei.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc.RazorPages;

    namespace DotNetCore.Pages
    {
        public class AboutModel : PageModel
        {
            public string Message { get; set; }

            public void OnGet()
            {
                Message = "Your application description page.";
                throw new Exception("Test Exception");
            }
        }
    }
    ```

### <a name="live-stream"></a>Live Stream

Führen Sie ein Update auf die NuGet-Pakete **Microsoft.ApplicationInsights.AspNetCore 2.2.0** durch, um mit ASP.NET Core auf die Live Stream-Funktionen von Application Insights zugreifen zu können.

Klicken Sie in Visual Studio auf **Projekt** > **NuGet-Pakete verwalten** > **Microsoft.ApplicationInsights.AspNetCore** > Version **2.2.0** > **Aktualisieren**.

  ![Screenshot: NuGet-Paket-Manager](./media/app-insights-asp-net-core/012-nuget-update.png)

Es werden mehrere Bestätigungsaufforderungen angezeigt. Lesen und akzeptieren Sie diese, wenn Sie mit den Änderungen einverstanden sind.

### <a name="page-view-load-time"></a>Ladezeit der Seitenansicht

1. Navigieren Sie in Visual Studio zu **Projektmappen-Explorer** > **Seiten**. Zwei Dateien müssen geändert werden: _Layout.cshtml_ und _ViewImports.cshtml_.

2. Fügen Sie in __ViewImports.cshtml_ Folgendes hinzu:

   ```csharp
   @using Microsoft.ApplicationInsights.AspNetCore
   @inject JavaScriptSnippet snippet
   ```

3. Fügen Sie in **_Layout.cshtml** die folgende Zeile vor dem Tag ``</head>`` (und auch vor jedem anderen Skript) hinzu.

    ```csharp
    @Html.Raw(snippet.FullScript)
    ```

### <a name="test-failed-requests-page-view-load-time-live-stream"></a>Testen von „Fehlerhafte Anforderungen“, „Ladezeit der Seitenansicht“ und „Live Stream“

So testen und vergewissern Sie sich, dass alles funktioniert:

1. Führen Sie Ihre App aus, indem Sie auf IIS Express klicken: ![Screenshot: IIS Express-Symbol in Visual Studio](./media/app-insights-asp-net-core/0012-iis-express.png)

2. Navigieren Sie zur Seite **Info**, um die Testausnahme auszulösen. (Bei Verwendung des Debugmodus müssen Sie in Visual Studio auf **Weiter** klicken, damit die Ausnahme in Application Insights angezeigt wird.)

3. Führen Sie das simulierte PowerShell-Transaktionsskript von weiter oben erneut aus. (Gegebenenfalls muss die Portnummer im Skript angepasst werden.)

4. Falls die Application Insights-Übersicht nicht mehr geöffnet ist, klicken Sie im Menü von Visual Studio auf **Projekt** > **Application Insights** > **Application Insights-Portal öffnen**. 

   > [!TIP]
   > Sollte Ihr neuer Datenverkehr noch nicht angezeigt werden, überprüfen Sie den **Zeitbereich**, und klicken Sie auf **Aktualisieren**.

   ![Screenshot: Übersichtsfenster](./media/app-insights-asp-net-core/0019-overview-updated.png)

5. Klicken Sie auf „Live Stream“.

   ![Screenshot: Live Metrics Stream](./media/app-insights-asp-net-core/0020-live-metrics-stream.png)

   (Wenn Ihr PowerShell-Skript noch ausgeführt wird, sollten Livemetriken angezeigt werden. Wurde das Skript bereits beendet, starten Sie es erneut, während Live Stream geöffnet ist.)

## <a name="app-insights-sdk-comparison"></a>Vergleich für das App Insights SDK

Die Application Insights-Produktgruppe hat sich sehr darum bemüht, Featureübereinstimmung zwischen dem [vollständigen .NET Framework SDK](https://github.com/Microsoft/ApplicationInsights-dotnet) und dem .NET Core SDK zu erreichen. Mit der Version 2.2.0 des [ASP.NET Core SDKs](https://github.com/Microsoft/ApplicationInsights-aspnetcore) für Application Insights wurde die Featurelücke größtenteils geschlossen.

Weitere Informationen zu den Unterschieden und Kompromissen zwischen .NET und .NET Core finden Sie [hier](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).

   | ASK-Vergleich | ASP.NET        | ASP.NET Core 2.1.0    | ASP.NET Core 2.2.0 |
  |:-- | :-------------: |:------------------------:|:----------------------:|
   | **Livemetriken**      | **+** |**-** | **+** |
   | **Servertelemetriekanal** | **+** |**-** | **+**|
   |**Adaptive Stichprobenerstellung**| **+** | **-** | **+**|
   | **SQL-Abhängigkeitsaufrufe**     | **+** |**-** | **+**|
   | **Leistungsindikatoren*** | **+** | **-**| **-**|

_Leistungsindikatoren_ bezieht sich in diesem Kontext auf [serverseitige Leistungsindikatoren](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) wie Prozessor, Arbeitsspeicher und Datenträgernutzung.

## <a name="open-source-sdk"></a>Open Source SDK
[Lesen und Hinzufügen von Code](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)

## <a name="application-insights-search-continued"></a>Application Insights-Suche (Fortsetzung)

Zum besseren Verständnis der Funktionsweise der Application Insights-Suche in Visual Studio für ein ASP.NET Core 2-Projekt, selbst wenn noch keine explizite Installation der Application Insights NuGet-Pakete stattgefunden hat, kann es hilfreich sein, die Debugausgabe zu überprüfen.

Wenn Sie die Ausgabe nach dem Wort _insight_ durchsuchen, werden Ergebnisse hervorgehoben, die Folgendem ähneln:

```DebugOuput
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.aspnetcore.applicationinsights.hostingstartup\2.0.3\lib\netcoreapp2.0\Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll'.
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.applicationinsights.aspnetcore\2.1.1\lib\netstandard1.6\Microsoft.ApplicationInsights.AspNetCore.dll'.

Application Insights Telemetry (unconfigured): {"name":"Microsoft.ApplicationInsights.Dev.Message","time":"2018-06-03T17:32:38.2796801Z","tags":{"ai.location.ip":"127.0.0.1","ai.operation.name":"DEBUG /","ai.internal.sdkVersion":"aspnet5c:2.1.1","ai.application.ver":"1.0.0.0","ai.cloud.roleInstance":"CONTOSO-SERVER","ai.operation.id":"de85878e-4618b05bad11b5a6","ai.internal.nodeName":"CONTOSO-SERVER","ai.operation.parentId":"|de85878e-4618b05bad11b5a6."},"data":{"baseType":"MessageData","baseData":{"ver":2,"message":"Request starting HTTP/1.1 DEBUG http://localhost:53022/  0","severityLevel":"Information","properties":{"AspNetCoreEnvironment":"Development","Protocol":"HTTP/1.1","CategoryName":"Microsoft.AspNetCore.Hosting.Internal.WebHost","Host":"localhost:53022","Path":"/","Scheme":"http","ContentLength":"0","DeveloperMode":"true","Method":"DEBUG"}}}}
```

Zwei Assemblys werden von CoreCLR geladen: 

- _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_
- _Microsoft.ApplicationInsights.AspNetCore.dll_.

Das _unconfigured_ in jeder Instanz der Application Insights-Telemetrie gibt an, dass diese Anwendung nicht mit einem ikey verknüpft ist, sodass die während der Ausführung Ihrer App generierten Daten nicht an Azure gesendet werden und nur für die lokale Suche und Analyse verfügbar sind.

Dies ist teilweise dadurch möglich, dass das NuGet-Paket _Microsoft.AspNetCore.All_ als eine Abhängigkeit [_Microsoft.ASPNetCoreApplicationInsights.HostingStartup_](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.applicationinsights.hostingstartup.applicationinsightshostingstartup?view=aspnetcore-2.1) verwendet.

![Screenshot: NuGet-Abhängigkeitsdiagramm für „Microsoft.AspNETCore.all“](./media/app-insights-asp-net-core/013-dependency.png)

Wenn Sie außerhalb von Visual Studio ein ASP.NET Core-Projekt in VSCode oder einem anderen Editor bearbeiten, werden diese Assemblys nicht automatisch während des Debugvorgangs geladen, sofern Sie Application Insights nicht explizit zu Ihrem Projekt hinzugefügt haben.

In Visual Studio jedoch erfolgt dieses Einblenden lokaler Application Insights-Features aus externen Assemblys durch Verwendung der [IHostingStartup-Schnittstelle](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup?view=aspnetcore-2.1), über die Application Insights während des Debugvorgangs dynamisch hinzufügt wird.

Erfahren Sie mehr über das Erweitern einer App über eine [externe Assembly in ASP.NET Core mit IHostingStartup](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/platform-specific-configuration?view=aspnetcore-2.1). 

### <a name="how-to-disable-application-insights-in-visual-studio-net-core-projects"></a>Deaktivieren von Application Insights in Visual Studio .NET Core-Projekten

Zwar kann das automatische Einblenden der Suchfunktion von Application Insights für einige hilfreich sein, doch ist die Anzeige der generierten Debugtelemetrie, wenn dies nicht erwartet wurde, möglicherweise verwirrend.

Wenn es für Sie ausreicht, einfach die Telemetriegenerierung zu deaktivieren, können Sie den folgenden Codeblock zur „Configure“-Methode Ihrer Datei _Startup.cs_ hinzufügen:

```csharp
  var configuration = app.ApplicationServices.GetService<Microsoft.ApplicationInsights.Extensibility.TelemetryConfiguration>();
            configuration.DisableTelemetry = true;
            if (env.IsDevelopment())
```

_Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_ und _Microsoft.ApplicationInsights.AspNetCore.dll_ werden weiterhin von CoreCLR geladen, haben jedoch keine Auswirkung.

Wenn Sie Application Insights in Ihrem Visual Studio .NET Core-Projekt vollständig deaktivieren möchten, ist die bevorzugte Methode das Auswählen von **Extras** > **Optionen** > **Projekte und Projektmappen** > **Webprojekte**. Aktivieren Sie dann das entsprechende Kontrollkästchen, um Application Insights lokal für ASP.NET Core-Webprojekte zu deaktivieren. Diese Funktionalität wurde in Visual Studio 15.6 hinzugefügt.

![Screenshot: Bildschirm „Websprojekte“ im Fenster „Optionen“ von Visual Studio](./media/app-insights-asp-net-core/014-disable.png)

Wenn Sie eine frühere Version von Visual Studio verwenden und alle Assemblys, die über IHostingStartup geladen werden, vollständig entfernen möchten, können Sie

`.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")`

zu _Program.cs_ hinzufügen:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;

namespace DotNetCore
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
                .UseStartup<Startup>()
                .Build();
    }
}
```

Sie können aber auch ``"ASPNETCORE_preventHostingStartup": "True"`` zu Umgebungsvariablen in _launchSettings.json_ hinzufügen.

Das Problem bei der Verwendung dieser Methoden ist, dass nicht nur Application Insights deaktiviert wird, sondern alles in Visual Studio, das die IHostingStartup-Einblendungsfunktionalität nutzt.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="next-steps"></a>Nächste Schritte
* [Untersuchen Sie Benutzerabläufe](app-insights-usage-flows.md), um die Benutzernavigation in Ihrer App nachzuvollziehen.
* [Konfigurieren Sie die Momentaufnahmensammlung](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger#configure-snapshot-collection-for-aspnet-core-20-applications), um den Status des Quellcodes und der Variablen zu dem Zeitpunkt anzuzeigen, zu dem eine Ausnahme ausgelöst wurde.
* [Verwenden Sie die API](app-insights-api-custom-events-metrics.md) , um Ihre eigenen Ereignisse und Metriken für eine detailliertere Ansicht der Leistung und Nutzung Ihrer App zu senden.
* [Availability tests](app-insights-monitor-web-app-availability.md) wird Ihre App fortwährend an Standorten auf der ganzen Welt überprüft.
