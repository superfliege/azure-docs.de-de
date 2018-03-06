---
title: "Azure Application Insights für ASP.NET Core | Microsoft-Dokumentation"
description: "Überwachen Sie Webanwendungen auf Verfügbarkeit, Leistung und Auslastung."
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: mbullwin
ms.openlocfilehash: e9fb3e68db66449d9ca3b43e6974910cb9477e62
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2018
---
# <a name="application-insights-for-aspnet-core"></a>Application Insights für ASP.NET Core

Azure Application Insights ermöglicht eine eingehende Überwachung Ihrer Webanwendung bis hin zur Codeebene. Mit dieser Lösung können Sie ganz einfach die Verfügbarkeit, Leistung und Nutzung Ihrer Webanwendung überwachen. Sie können auch Fehler in Ihrer Anwendung schnell erkennen und diagnostizieren, ohne darauf warten zu müssen, dass diese Fehler von Benutzern gemeldet werden.

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie in Visual Studio eine ASP.NET Core-Beispielanwendung für [Razor-Seiten](https://docs.microsoft.com/aspnet/core/mvc/razor-pages/?tabs=visual-studio) erstellen und mit der Überwachung unter Verwendung von Azure Application Insights beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- .NET Core 2.0.0 SDK oder eine höhere Version
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) (Version 15.3 oder höher) mit der Workload für ASP.NET und Webentwicklung

## <a name="create-an-aspnet-core-project-in-visual-studio"></a>Erstellen eines ASP.NET Core-Projekts in Visual Studio

1. Klicken Sie mit der rechten Maustaste auf **Visual Studio 2017**, und starten Sie das Programm als Administrator.
2. Klicken Sie auf **Datei** > **Neu** > **Projekt** (STRG+UMSCHALT+N).

   ![Screenshot: „Datei“ > „Neu“ > „Projekt“ in Visual Studio – Menü](./media/app-insights-asp-net-core/0001-file-new-project.png)

3. Erweitern Sie **Visual C#**, und klicken Sie auf **.NET Core** > **ASP.NET Core-Webanwendung**. Geben Sie unter **Name** > einen Namen und unter **Projektmappenname** einen Projektmappennamen ein, und aktivieren Sie das Kontrollkästchen **Neues Git-Repository erstellen**.

   ![Screenshot: „Datei“ > „Neu“ > „Projekt“ in Visual Studio – Assistent](./media/app-insights-asp-net-core/0002-new-project-web-application.png)

4. Klicken Sie auf **.NET Core** > **ASP.NET Core 2.0** > **Webanwendung** > **OK**.

    ![Screenshot: „Datei“ > „Neu“ > „Projekt“ in Visual Studio – Auswahlmenü](./media/app-insights-asp-net-core/0003-dot-net-core.png)

## <a name="add-application-insights-telemetry"></a>Hinzufügen von Application Insights-Telemetrie

1. Klicken Sie auf **Projekt** > **Application Insights-Telemetrie hinzufügen...**. (Alternative: Klicken Sie mit der rechten Maustaste auf **Verbundene Dienste**, und klicken Sie anschließend auf „Verbundenen Dienst hinzufügen“.)

    ![Screenshot: „Datei“ > „Neu“ > „Projekt“ in Visual Studio – Auswahlmenü](./media/app-insights-asp-net-core/0004-add-application-insights-telemetry.png)

2. Wählen Sie die Option **Kostenlos starten**.

    ![Screenshot: „Datei“ > „Neu“ > „Projekt“ in Visual Studio – Auswahlmenü](./media/app-insights-asp-net-core/0005-start-free.png)

3. Wählen Sie eine geeignete Option für **Abonnement** > **Ressource** aus, geben Sie an, ob Daten gesammelt werden dürfen, die über 1 GB/Monat hinausgehen, und klicken Sie anschließend auf **Registrieren**.

    ![Screenshot: „Datei“ > „Neu“ > „Projekt“ in Visual Studio – Auswahlmenü](./media/app-insights-asp-net-core/0006-register.png)

## <a name="changes-made-to-your-project"></a>Änderungen an Ihrem Projekt

Der durch Application Insights entstehende Mehraufwand ist äußerst überschaubar. Die Änderungen, die durch Hinzufügen von Application Insights-Telemetrie an Ihrem Projekt vorgenommen werden, können Sie wie folgt überprüfen:

Klicken Sie auf **Ansicht** > **Team Explorer** (STRG+\, STRG+M) > **Projekt** > **Änderungen**.

- Insgesamt wurden vier Änderungen vorgenommen:

  ![Screenshot: Dateien, die durch Hinzufügen von Application Insights geändert wurden](./media/app-insights-asp-net-core/0007-changes.png)

- Eine neue Datei wird erstellt:

   **ConnectedService.json**

  ![Screenshot: Dateien, die durch Hinzufügen von Application Insights geändert wurden](./media/app-insights-asp-net-core/0008-connectedservice-json.png)

- Drei Dateien werden geändert:

  **appsettings.json**

   ![Screenshot: Dateien, die durch Hinzufügen von Application Insights geändert wurden](./media/app-insights-asp-net-core/0009-appsettings-json.png)

  **ContosoDotNetCore.csproj**

   ![Screenshot: Dateien, die durch Hinzufügen von Application Insights geändert wurden](./media/app-insights-asp-net-core/0010-contoso-netcore-csproj.png)

   **Program.cs**

   ![Screenshot: Dateien, die durch Hinzufügen von Application Insights geändert wurden](./media/app-insights-asp-net-core/0011-program-cs.png)

## <a name="synthetic-transactions-with-powershell"></a>Synthetische Transaktionen mit PowerShell

Sie können Ihre App starten und manuell auf verschiedene Links klicken, um Datenverkehr zu Testzwecken zu generieren. Häufig ist es jedoch hilfreich, eine einfache synthetische Transaktion in PowerShell zu erstellen.

1. Führen Sie Ihre App aus, indem Sie auf IIS Express klicken: ![Screenshot: IIS Express-Symbol in Visual Studio](./media/app-insights-asp-net-core/0012-iis-express.png)

2. Kopieren Sie die URL aus der Adressleiste des Browsers. Die URL hat folgendes Format: http://localhost:{beliebige Portnummer}

   ![Screenshot: URL in der Browseradressleiste](./media/app-insights-asp-net-core/0013-copy-url.png)

3. Führen Sie die folgende PowerShell-Schleife aus, um 100 synthetische Transaktionen für Ihre Test-App zu erstellen. Ändern Sie die Portnummer nach **localhost:** gemäß der URL, die Sie im vorherigen Schritt kopiert haben.

   ```PS
   for ($i = 0 ; $i -lt 100; $i++)
   {
    Invoke-WebRequest -uri http://localhost:50984/
   }
   ```

## <a name="open-application-insights-portal"></a>Öffnen des Application Insights-Portals

Starten Sie nach dem Ausführen der PowerShell-Schleife aus dem vorherigen Abschnitt Application Insights, um die Transaktionen anzuzeigen und sich zu vergewissern, dass Daten gesammelt werden. 

Klicken Sie im Visual Studio-Menü auf **Projekt** > **Application Insights** > **Application Insights-Portal öffnen**

   ![Screenshot: Application Insights-Übersicht](./media/app-insights-asp-net-core/0014-portal-01.png)

> [!NOTE]
> Im obigen Beispielscreenshot ist zu sehen, dass für **Live Stream**, **Ladezeit der Seitenansicht** und **Fehlerhafte Anforderungen** derzeit keine Daten gesammelt werden. Im nächsten Abschnitt wird das Hinzufügen dieser Elemente Schritt für Schritt erläutert. Wenn Sie bereits Daten für **Live Stream** und **Ladezeit der Seitenansicht** sammeln, führen Sie nur die Schritte für **Fehlerhafte Anforderungen** aus.

## <a name="collect-failed-requests-live-stream--page-view-load-time"></a>Sammeln von Daten für „Fehlerhafte Anforderungen“, „Live Stream“ und „Ladezeit der Seitenansicht“

### <a name="failed-requests"></a>Fehlerhafte Anforderungen

Genau genommen werden Daten für **Fehlerhafte Anforderungen** gesammelt, es sind jedoch noch keine aufgetreten. Zur Beschleunigung des Prozesses kann dem vorhandenen Projekt eine benutzerdefinierte Ausnahme hinzugefügt werden, um eine echte Ausnahme zu simulieren. Falls Ihre App noch in Visual Studio ausgeführt wird, **beenden Sie das Debuggen** (UMSCHALT+F5), bevor Sie den Vorgang fortsetzen.

1. Erweitern Sie im **Projektmappen-Explorer** den Knoten **Seiten** > **About.cshtml**, und öffnen Sie **About.cshtml.cs**.

   ![Screenshot: Projektmappen-Explorer in Visual Studio](./media/app-insights-asp-net-core/0015-solution-explorer-about.png)

2. Fügen Sie unter ``Message=`` eine Ausnahme hinzu, und speichern Sie die Änderung in der Datei.

   ```C#
   throw new Exception("Test Exception");
   ```

   ![Screenshot: Ausnahmecode](./media/app-insights-asp-net-core/000016-exception.png)

### <a name="live-stream"></a>Live Stream

Führen Sie ein Update auf die NuGet-Pakete **Microsoft.ApplicationInsights.AspNetCore 2.2.0** durch, um mit ASP.NET Core auf die Live Stream-Funktionen von Application Insights zugreifen zu können.

Klicken Sie in Visual Studio auf **Projekt** > **NuGet-Pakete verwalten** > **Microsoft.ApplicationInsights.AspNetCore** > Version **2.2.0** > **Aktualisieren**.

  ![Screenshot: NuGet-Paket-Manager](./media/app-insights-asp-net-core/0017-update-nuget.png)

Lesen Sie sich die angezeigten Bestätigungsaufforderungen durch, und stimmen Sie ihnen zu, wenn Sie mit den Änderungen einverstanden sind.

### <a name="page-view-load-time"></a>Ladezeit der Seitenansicht

1. Navigieren Sie in Visual Studio zu **Projektmappen-Explorer** > **Seiten**. Zwei Dateien müssen geändert werden: **_Layout.cshtml** und **_ViewImports.cshtml**

2. Fügen Sie in **_ViewImports.cshtml** Folgendes hinzu:

   ```C#
   @using Microsoft.ApplicationInsights.AspNetCore
   @inject JavaScriptSnippet snippet
   ```
     ![Screenshot: Codeänderung in „_ViewImports.cshtml“](./media/app-insights-asp-net-core/00018-view-imports.png)

3. Fügen Sie in **Layout.cshtml** die folgende Zeile vor dem Tag ``</head>`` (und vor jedem anderen Skript) hinzu.

    ```C#
    @Html.Raw(snippet.FullScript)
    ```
    ![Screenshot: Codeänderung in „layout.cshtml“](./media/app-insights-asp-net-core/0018-layout-cshtml.png)

### <a name="test-failed-requests-page-view-load-time-live-stream"></a>Testen von „Fehlerhafte Anforderungen“, „Ladezeit der Seitenansicht“ und „Live Stream“

Nachdem Sie die obigen Schritte ausgeführt haben, können Sie testen, ob alles funktioniert.

1. Führen Sie Ihre App aus, indem Sie auf IIS Express klicken: ![Screenshot: IIS Express-Symbol in Visual Studio](./media/app-insights-asp-net-core/0012-iis-express.png)

2. Navigieren Sie zur Seite **Info**, um die Testausnahme auszulösen. (Bei Verwendung des Debugmodus müssen Sie in Visual Studio auf **Weiter** klicken, damit die Ausnahme von Application Insights registriert wird.)

3. Führen Sie das simulierte PowerShell-Transaktionsskript von weiter oben erneut aus. (Gegebenenfalls muss die Portnummer im Skript angepasst werden.)

4. Falls die Application Insights-Übersicht nicht mehr geöffnet ist, klicken Sie im Menü von Visual Studio auf **Projekt** > **Application Insights** > **Application Insights-Portal öffnen**. 

   > [!TIP]
   > Sollte Ihr neuer Datenverkehr noch nicht angezeigt werden, überprüfen Sie den **Zeitbereich**, und klicken Sie auf **Aktualisieren**.

   ![Screenshot: Übersichtsfenster](./media/app-insights-asp-net-core/0019-overview-updated.png)

5. Klicken Sie auf „Live Stream“.

   ![Screenshot: Live Metrics Stream](./media/app-insights-asp-net-core/0020-live-metrics-stream.png)

   (Wenn Ihr PowerShell-Skript noch ausgeführt wird, sollten Livemetriken angezeigt werden. Ist das Skript bereits beendet, starten Sie es erneut, während Live Stream geöffnet ist.)

## <a name="app-insights-sdk-comparison"></a>Vergleich für das App Insights SDK

Die Application Insights-Produktgruppe hat sich sehr darum bemüht, eine möglichst hohe Featureübereinstimmung zwischen dem [vollständigen .NET Framework SDK](https://github.com/Microsoft/ApplicationInsights-dotnet) und dem .NET Core SDK zu erreichen. Mit der Version 2.2.0 des [ASP.NET Core SDKs](https://github.com/Microsoft/ApplicationInsights-aspnetcore) für Application Insights wurde die Featurelücke größtenteils geschlossen.

Weitere Informationen zu den Unterschieden und Kompromissen zwischen .NET und .NET Core finden Sie [hier](https://docs.microsoft.com/en-us/dotnet/standard/choosing-core-framework-server).

   | ASK-Vergleich | ASP.NET        | ASP.NET Core 2.1.0    | ASP.NET Core 2.2.0 |
  |:-- | :-------------: |:------------------------:|:----------------------:|
   | **Livemetriken**      | **+** |**-** | **+** |
   | **Servertelemetriekanal** | **+** |**-** | **+**|
   |**Adaptive Stichprobenerstellung**| **+** | **-** | **+**|
   | **SQL-Abhängigkeitsaufrufe**     | **+** |**-** | **+**|
   | **Leistungsindikatoren*** | **+** | **-**| **-**|

_Leistungsindikatoren_ bezieht sich in diesem Kontext auf [serverseitige Leistungsindikatoren](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-performance-counters) wie Prozessor, Arbeitsspeicher und Datenträgernutzung.

## <a name="open-source-sdk"></a>Open Source SDK
[Lesen und Hinzufügen von Code](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="next-steps"></a>Nächste Schritte
* [Untersuchen Sie Benutzerabläufe](app-insights-usage-flows.md), um die Benutzernavigation in Ihrer App nachzuvollziehen.
* [Verwenden Sie die API](app-insights-api-custom-events-metrics.md) , um Ihre eigenen Ereignisse und Metriken für eine detailliertere Ansicht der Leistung und Nutzung Ihrer App zu senden.
* [Availability tests](app-insights-monitor-web-app-availability.md) wird Ihre App fortwährend an Standorten auf der ganzen Welt überprüft.