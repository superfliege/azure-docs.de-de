---
title: Azure Application Insights für ASP.NET Core | Microsoft-Dokumentation
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
ms.date: 06/03/2018
ms.author: mbullwin
ms.openlocfilehash: ae0d3658d9ae8534b1596fa7363495926cd0dfe7
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58520750"
---
# <a name="application-insights-for-aspnet-core"></a>Application Insights für ASP.NET Core

Azure Application Insights ermöglicht eine eingehende Überwachung Ihrer Webanwendung bis hin zur Codeebene. Mit dieser Lösung können Sie ganz einfach die Verfügbarkeit, Leistung und Nutzung Ihrer Webanwendung überwachen. Sie können auch Fehler in Ihrer Anwendung schnell erkennen und diagnostizieren, ohne darauf warten zu müssen, dass diese Fehler von Benutzern gemeldet werden.

Dieser Artikel führt Sie durch die Schritte zum Erstellen einer ASP.NET Core-Beispielanwendung für [Razor Pages](https://docs.microsoft.com/aspnet/core/mvc/razor-pages/?tabs=visual-studio) in Visual Studio. Sie erfahren auch, wie Sie mit der Überwachung mit Application Insights beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- .NET Core 2.0.0 SDK oder höher
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) (Version 15.7.3 oder höher) mit der Workload für ASP.NET und Webentwicklung

## <a name="create-an-aspnet-core-project-in-visual-studio"></a>Erstellen eines ASP.NET Core-Projekts in Visual Studio

1. Klicken Sie mit der rechten Maustaste auf **Visual Studio 2017**, und wählen Sie dann **Als Administrator ausführen**.
2. Wählen Sie **Datei** > **Neu** > **Projekt** aus (STRG+UMSCHALT+N).

   ![Screenshot des Visual Studio-Menüs für ein neues Projekt](./media/asp-net-core/001-new-project.png)

3. Erweitern Sie **Visual C#**. Wählen Sie **.NET Core** > **ASP.NET Core-Webanwendung** aus. Geben Sie einen Projektnamen und einen Projektmappennamen ein, und wählen Sie dann **Neues Git-Repository erstellen**.

   ![Screenshot des Visual Studio-Assistenten für ein neues Projekt](./media/asp-net-core/002-asp-net-core-web-application.png)

4. Wählen Sie **.NET Core** > **ASP.NET Core 2.0**-**Webanwendung** > **OK** aus.

    ![Screenshot der Auswahl einer Vorlage für ein neues Projekt in Visual Studio](./media/asp-net-core/003-web-application.png)

## <a name="application-insights-search"></a>Application Insights-Suche

In Visual Studio 2015 Update 2 oder höher können Sie bei einem auf ASP.NET Core 2+ basierenden Projekt die [Application Insights-Suche](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio) nutzen, noch bevor Sie Application Insights explizit Ihrem Projekt hinzugefügt haben.

So testen Sie diese Funktionalität:

1. Führen Sie Ihre App aus. Wählen Sie zum Ausführen Ihrer App das Symbol **IIS Express** aus (![Screenshot des Visual Studio IIS Express-Symbols](./media/asp-net-core/004-iis-express.png)).

2. Wählen Sie **Ansicht** > **Weitere Fenster** > **Application Insights-Suche** aus.

   ![Screenshot der Visual Studio-Diagnosetoolsauswahl](./media/asp-net-core/005-view-other-windows-search.png)

3. Derzeit stehen die Telemetriedaten der Debugsitzung nur für lokale Analysen zur Verfügung. Zum vollständigen Aktivieren von Application Insights wählen Sie oben rechts die Option **Telemetriebereitschaft** aus, oder führen Sie die Schritte im folgenden Abschnitt aus.

   ![Screenshot: Visual Studio-Fenster „Application Insights-Suche“](./media/asp-net-core/006-search.png)

> [!NOTE]
> Weitere Informationen zum lokalen Einblenden von Funktionen wie [Application Insights-Suche](../../azure-monitor/app/visual-studio.md) und [CodeLens](../../azure-monitor/app/visual-studio-codelens.md) in Visual Studio, bevor Sie Application Insights zu Ihrem ASP.NET Core-Projekt hinzufügen, finden Sie unter [Application Insights-Suche (Fortsetzung)](#application-insights-search-continued).

## <a name="add-application-insights-telemetry"></a>Hinzufügen von Application Insights-Telemetrie

1. Wählen Sie **Projekt** > **Application Insights-Telemetrie hinzufügen** aus. (Sie können auch mit der rechten Maustaste auf **Verbundene Dienste** klicken und **Verbundenen Dienst hinzufügen** auswählen.)

    ![Screenshot des Auswahlmenüs für ein neues Projekt in Visual Studio](./media/asp-net-core/007-project-add-telemetry.png)

2. Wählen Sie **Erste Schritte** aus. (Je nach verwendeter Version von Visual Studio kann der Text geringfügig abweichen. Bei einigen älteren Versionen wird stattdessen die Schaltfläche **Kostenlos starten** angezeigt.)

    ![Screenshot der Schaltfläche „Erste Schritte“ in Application Insights](./media/asp-net-core/008-get-started.png)

3. Wählen Sie Ihr Abonnement und dann **Ressource** > **Registrieren** aus.

## <a name="changes-made-to-your-project"></a>Änderungen an Ihrem Projekt

Der durch Application Insights entstehende Mehraufwand ist überschaubar. Die Änderungen, die durch Hinzufügen von Application Insights-Telemetrie an Ihrem Projekt vorgenommen werden, können Sie wie folgt überprüfen:

Klicken Sie auf **Ansicht** > **Team Explorer** (STRG+\, STRG+M) > **Projekt** > **Änderungen**.

- Vier Änderungen werden insgesamt angezeigt:

  ![Screenshot: Dateien, die durch Hinzufügen von Application Insights geändert wurden](./media/asp-net-core/009-changes.png)

- Eine neue Datei wird erstellt:

  - _ConnectedService.json_

    ```json
    {
     "ProviderId": "Microsoft.ApplicationInsights.ConnectedService.ConnectedServiceProvider",
     "Version": "8.12.10405.1",
     "GettingStartedDocument": {
       "Uri": "https://go.microsoft.com/fwlink/?LinkID=798432"
     }
    }
    ```

- Drei Dateien werden geändert (zusätzliche Kommentare werden zum Hervorheben von Änderungen hinzugefügt):

  - _appsettings.json_:

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

  - _ContosoDotNetCore.csproj_:

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

  -  _Program.cs_:

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

## <a name="send-ilogger-logs-to-application-insights"></a>Senden von ILogger-Protokollen an Application Insights

Application Insights unterstützt das Erfassen von Protokollen, die über ILogger gesendet werden. Zum Einrichten der Protokollierung sehen Sie sich die Codebeispiele [hier](https://docs.microsoft.com/azure/azure-monitor/app/ilogger) an.

## <a name="synthetic-transactions-with-powershell"></a>Synthetische Transaktionen mit PowerShell

So automatisieren Sie Anforderungen an Ihre App mit synthetischen Transaktionen:

1. Wählen Sie zum Ausführen Ihrer App das ![Screenshot: IIS Express-Symbol in Visual Studio](./media/asp-net-core/004-iis-express.png) Symbol aus.

2. Kopieren Sie die URL aus der Adressleiste des Browsers. Das Format der URL ist `http://localhost:<port number>`.

   ![Screenshot der URL in der Browseradressleiste](./media/asp-net-core/0013-copy-url.png)

3. Führen Sie die folgende PowerShell-Schleife aus, um 100 synthetische Transaktionen mit Ihrer Test-App zu erstellen. Ändern Sie die Portnummer nach `localhost:` gemäß der URL, die Sie im vorherigen Schritt kopiert haben. Beispiel: 

   ```powershell
   for ($i = 0 ; $i -lt 100; $i++)
   {
    Invoke-WebRequest -uri http://localhost:50984/
   }
   ```

## <a name="open-the-application-insights-portal"></a>Öffnen des Application Insights-Portals

Starten Sie nach dem Ausführen der PowerShell-Befehle aus dem vorherigen Abschnitt Application Insights, um die Transaktionen anzuzeigen und sich zu vergewissern, dass Daten gesammelt werden. 

Wählen Sie im Visual Studio-Menü **Projekt** > **Application Insights** > **Application Insights-Portal öffnen** aus.

   ![Screenshot: Application Insights-Übersicht](./media/asp-net-core/010-portal.png)

> [!NOTE]
> Im obigen Beispielscreenshot ist zu sehen, dass für **Live Stream**, **Ladezeit der Seitenansicht** und **Fehlerhafte Anforderungen** keine Daten gesammelt werden. Der nächste Abschnitt führt Sie durch die Schritte, mit denen Sie diese hinzufügen können. Wenn Sie bereits Daten für **Live Stream** und **Ladezeit der Seitenansicht** sammeln, führen Sie nur die Schritte für **Fehlerhafte Anforderungen** aus.

## <a name="collect-failed-requests-live-stream-and-page-view-load-time"></a>Sammeln von Daten für „Fehlerhafte Anforderungen“, „Live Stream“ und „Ladezeit der Seitenansicht“

### <a name="failed-requests"></a>Anforderungsfehler

Genau genommen werden Daten für fehlerhafte Anforderungen gesammelt, es sind jedoch noch keine fehlerhaften Anforderungen aufgetreten. Zur Beschleunigung des Prozesses können Sie dem vorhandenen Projekt eine benutzerdefinierte Ausnahme hinzufügen, um eine echte Ausnahme zu simulieren. Falls Ihre App noch in Visual Studio ausgeführt wird, wählen Sie **Debuggen beenden** aus (UMSCHALT+F5), bevor Sie den Vorgang fortsetzen.

1. Erweitern Sie im **Projektmappen-Explorer** den Knoten **Seiten** > **About.cshtml**, und öffnen Sie *About.cshtml.cs*.

   ![Screenshot: Projektmappen-Explorer in Visual Studio](./media/asp-net-core/011-about.png)

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

Führen Sie ein Update der Microsoft.ApplicationInsights.AspNetCore 2.2.0-NuGet-Pakete durch, um mit ASP.NET Core auf die Live Stream-Funktionen von Application Insights zugreifen zu können.

Wählen Sie in Visual Studio **Projekt** > **NuGet-Pakete verwalten** > **Microsoft.ApplicationInsights.AspNetCore** > Version **2.2.0** > **Aktualisieren** aus.

  ![Screenshot: NuGet-Paket-Manager](./media/asp-net-core/012-nuget-update.png)

Es werden mehrere Bestätigungsaufforderungen angezeigt. Lesen und akzeptieren Sie diese, wenn Sie mit den Änderungen einverstanden sind.

### <a name="page-view-load-time"></a>Ladezeit der Seitenansicht

1. Wechseln Sie in Visual Studio zu **Projektmappen-Explorer** > **Seiten**. Sie müssen zwei Dateien ändern: *Layout.cshtml* und *ViewImports.cshtml*.

2. Fügen Sie in *ViewImports.cshtml* diesen Code hinzu:

   ```csharp
   @using Microsoft.ApplicationInsights.AspNetCore
   @inject JavaScriptSnippet snippet
   ```

3. Fügen Sie in *Layout.cshtml* den folgenden Code vor dem ``</head>``-Tag und jedem anderen Skript hinzu:

    ```csharp
    @Html.Raw(snippet.FullScript)
    ```

### <a name="test-failed-requests-page-view-load-time-and-live-stream"></a>Testen von „Fehlerhafte Anforderungen“, „Ladezeit der Seitenansicht“ und „Live Stream“

So testen Sie und vergewissern sich, dass alles funktioniert:

1. Führen Sie Ihre App aus. Wählen Sie zum Ausführen Ihrer App das ![Screenshot: IIS Express-Symbol in Visual Studio](./media/asp-net-core/004-iis-express.png) Symbol aus.

2. Gehen Sie zur Seite **Info**, um die Testausnahme auszulösen. (Wählen Sie im Debugmodus in Visual Studio **Weiter** aus, damit die Ausnahme in Application Insights angezeigt wird.)

3. Führen Sie das simulierte PowerShell-Transaktionsskript, das Sie zuvor verwendet haben, erneut aus. (Möglicherweise müssen Sie die Portnummer im Skript anpassen.)

4. Falls die **Übersicht**-Seite in Application Insights noch nicht geöffnet ist, wählen Sie im Menü von Visual Studio **Projekt** > **Application Insights** > **Application Insights-Portal öffnen** aus. 

   > [!TIP]
   > Wenn Ihr neuer Datenverkehr nicht angezeigt wird, überprüfen Sie den Wert für **Zeitbereich**, und wählen Sie dann **Aktualisieren** aus.

   ![Screenshot des Übersichtsfensters](./media/asp-net-core/0019-overview-updated.png)

5. Wählen Sie **Live Stream** aus.

   ![Screenshot: Live Metrics Stream](./media/asp-net-core/0020-live-metrics-stream.png)

   (Wenn das PowerShell-Skript noch ausgeführt wird, sollten Livemetriken angezeigt werden. Wenn Ihr PowerShell-Skript nicht mehr ausgeführt wird, starten Sie es erneut, während Live Stream geöffnet ist.)

## <a name="application-insights-sdk-comparison"></a>Application Insights SDK im Vergleich

Die Application Insights-Produktgruppe hat intensiv daran gearbeitet, Featureübereinstimmung zwischen dem [vollständigen .NET Framework SDK](https://github.com/Microsoft/ApplicationInsights-dotnet) und dem .NET Core SDK zu erreichen. Das Release 2.2.0 des [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore) für Application Insights schließt die Featurelücke größtenteils.

In der folgenden Tabelle werden mehrere Unterschiede und Kompromisse zwischen [.NET und .NET Core](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) beschrieben:

   | SDK-Vergleich | ASP.NET        | ASP.NET Core 2.1.0    | ASP.NET Core 2.2.0 |
  |:-- | :-------------: |:------------------------:|:----------------------:|
   | **Livemetriken**      | **+** |**-** | **+** |
   | **Servertelemetriekanal** | **+** |**-** | **+**|
   |**Adaptive Stichprobenerstellung**| **+** | **-** | **+**|
   | **SQL-Abhängigkeitsaufrufe**     | **+** |**-** | **+**|
   | **Leistungsindikatoren*** | **+** | **-**| **-**|

„Leistungsindikatoren“ bezieht sich in diesem Kontext auf [serverseitige Leistungsindikatoren](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) wie Prozessor, Arbeitsspeicher und Datenträgernutzung.

## <a name="open-source-sdk"></a>Open Source SDK
[Lesen und Hinzufügen von Code](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates).

## <a name="application-insights-search-continued"></a>Application Insights-Suche (Fortsetzung)

In diesem Abschnitt wird erläutert, wie die Application Insights-Suche in Visual Studio für ein ASP.NET Core 2-Projekt funktioniert. Es funktioniert auch dann auf diese Weise, wenn Sie die Application Insights-NuGet-Pakete nicht explizit installiert haben. Es kann auch hilfreich sein, die Debugausgabe zu untersuchen.

Wenn Sie die Ausgabe nach dem Wort _insight_ durchsuchen, werden Ergebnisse hervorgehoben, die Folgendem ähneln:

```DebugOutput
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.aspnetcore.applicationinsights.hostingstartup\2.0.3\lib\netcoreapp2.0\Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll'.
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.applicationinsights.aspnetcore\2.1.1\lib\netstandard1.6\Microsoft.ApplicationInsights.AspNetCore.dll'.

Application Insights Telemetry (unconfigured): {"name":"Microsoft.ApplicationInsights.Dev.Message","time":"2018-06-03T17:32:38.2796801Z","tags":{"ai.location.ip":"127.0.0.1","ai.operation.name":"DEBUG /","ai.internal.sdkVersion":"aspnet5c:2.1.1","ai.application.ver":"1.0.0.0","ai.cloud.roleInstance":"CONTOSO-SERVER","ai.operation.id":"de85878e-4618b05bad11b5a6","ai.internal.nodeName":"CONTOSO-SERVER","ai.operation.parentId":"|de85878e-4618b05bad11b5a6."},"data":{"baseType":"MessageData","baseData":{"ver":2,"message":"Request starting HTTP/1.1 DEBUG http://localhost:53022/  0","severityLevel":"Information","properties":{"AspNetCoreEnvironment":"Development","Protocol":"HTTP/1.1","CategoryName":"Microsoft.AspNetCore.Hosting.Internal.WebHost","Host":"localhost:53022","Path":"/","Scheme":"http","ContentLength":"0","DeveloperMode":"true","Method":"DEBUG"}}}}
```

In der Ausgabe lädt CoreCLR zwei Assemblys: 

- _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_
- _Microsoft.ApplicationInsights.AspNetCore.dll_.

Der _unconfigured_ Verweis in jeder Instanz von Application Insights-Telemetrie gibt an, dass dieser Anwendung kein „iKey“ (Instrumentierungsschlüssel) zugeordnet ist. Die Daten, die während der Ausführung Ihrer App generiert werden, werden nicht an Azure gesendet. Die Daten sind nur für die lokale Suche und Analyse verfügbar.

Die Funktionalität ist in diesem Teil möglich, weil das NuGet-Paket _Microsoft.AspNetCore.All_ als Abhängigkeit [_Microsoft.ASPNetCoreApplicationInsights.HostingStartup_](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.applicationinsights.hostingstartup.applicationinsightshostingstartup?view=aspnetcore-2.1) verwendet.

![Screenshot des NuGet-Abhängigkeitsdiagramms für „Microsoft.AspNETCore.all“](./media/asp-net-core/013-dependency.png)

Wenn Sie außerhalb von Visual Studio ein ASP.NET Core-Projekt in VSCode oder einem anderen Editor bearbeiten, werden diese Assemblys nicht automatisch während des Debugvorgangs geladen, sofern Sie Application Insights nicht explizit Ihrem Projekt hinzugefügt haben.

In Visual Studio jedoch erfolgt dieses Einblenden lokaler Application Insights-Features aus externen Assemblys durch Verwendung der [IHostingStartup-Schnittstelle](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup?view=aspnetcore-2.1). Die Schnittstelle fügt Application Insights während des Debugvorgangs dynamisch hinzu.

Erfahren Sie mehr über das Erweitern einer App über eine [externe Assembly in ASP.NET Core mit IHostingStartup](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/platform-specific-configuration?view=aspnetcore-2.1). 

### <a name="disable-application-insights-in-visual-studio-net-core-projects"></a>Deaktivieren von Application Insights in Visual Studio .NET Core-Projekten

Zwar kann das automatische Einblenden der Suchfunktion von Application Insights hilfreich sein, doch ist die Anzeige der generierten Debugtelemetrie, sofern unerwartet, möglicherweise verwirrend.

Wenn es für Sie ausreicht, einfach die Telemetriegenerierung zu deaktivieren, können Sie den folgenden Codeblock der **configure**-Methode Ihrer Datei _Startup.cs_ hinzufügen:

```csharp
  var configuration = app.ApplicationServices.GetService<Microsoft.ApplicationInsights.Extensibility.TelemetryConfiguration>();
            configuration.DisableTelemetry = true;
            if (env.IsDevelopment())
```

_Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_ und _Microsoft.ApplicationInsights.AspNetCore.dll_ werden weiterhin von CoreCLR geladen, haben jedoch keine Auswirkung.

Wenn Sie Application Insights in Ihrem Visual Studio .NET Core-Projekt vollständig deaktivieren möchten, ist die bevorzugte Methode das Auswählen von **Extras** > **Optionen** > **Projekte und Projektmappen** > **Webprojekte**. Aktivieren Sie das Kontrollkästchen **Lokales Application Insights für ASP.NET Core-Webprojekte deaktivieren**. Diese Funktionalität wurde in Visual Studio 15.6 hinzugefügt.

![Screenshot: Bildschirm „Websprojekte“ im Fenster „Optionen“ von Visual Studio](./media/asp-net-core/014-disable.png)

Wenn Sie eine frühere Version von Visual Studio verwenden und alle Assemblys, die über *IHostingStartup* geladen werden, vollständig entfernen möchten, können Sie zwischen zwei Optionen wählen:

* Fügen Sie `.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")` zu _Program.cs_ hinzu:

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

* Fügen Sie ``"ASPNETCORE_preventHostingStartup": "True"`` den _launchSettings.json_-Umgebungsvariablen  hinzu.

Bei der Verwendung dieser Methoden besteht das Problem, dass sie nicht nur Application Insights deaktivieren. Die Methoden deaktivieren auch alles in Visual Studio, was die Einblendungsfunktionalität von *IHostingStartup* verwendet hat.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="next-steps"></a>Nächste Schritte
* [Untersuchen Sie Benutzerabläufe](../../azure-monitor/app/usage-flows.md), um die Benutzernavigation in Ihrer App nachzuvollziehen.
* [Konfigurieren Sie die Momentaufnahmensammlung](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger), um den Status des Quellcodes und der Variablen zu dem Zeitpunkt anzuzeigen, zu dem eine Ausnahme ausgelöst wurde.
* [Verwenden Sie die API](../../azure-monitor/app/api-custom-events-metrics.md) , um Ihre eigenen Ereignisse und Metriken für eine detailliertere Ansicht der Leistung und Nutzung Ihrer App zu senden.
* Überprüfen Sie Ihre App mit [Verfügbarkeitstests](../../azure-monitor/app/monitor-web-app-availability.md) fortwährend von jedem Ort der Welt aus.
