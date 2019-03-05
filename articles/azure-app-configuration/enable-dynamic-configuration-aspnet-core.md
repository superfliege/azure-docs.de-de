---
title: 'Tutorial: Verwenden der dynamischen Konfiguration von Azure App Configuration in einer ASP.NET Core-App | Microsoft-Dokumentation'
description: In diesem Tutorial wird beschrieben, wie Sie die Konfigurationsdaten für ASP.NET Core-Apps dynamisch aktualisieren.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: na
ms.topic: tutorial
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 44ae922b182874eef378d4868fb278c3c76252db
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2019
ms.locfileid: "56884413"
---
# <a name="tutorial-use-dynamic-configuration-in-an-aspnet-core-app"></a>Tutorial: Verwenden der dynamischen Konfiguration in einer ASP.NET Core-App

ASP.NET Core verfügt über ein austauschbares Konfigurationssystem, das Konfigurationsdaten aus vielen verschiedenen Quellen lesen und Änderungen während der Ausführung verarbeiten kann, ohne dass eine Anwendung neu gestartet werden muss. Es ist Unterstützung für das Binden von Konfigurationseinstellungen an stark typisierte .NET-Klassen und das Einfügen in Ihren Code mit den unterschiedlichen `IOptions<T>`-Mustern vorhanden. Eines dieser Muster (`IOptionsSnapshot<T>`) ermöglicht das automatische erneute Laden der Anwendungskonfiguration, wenn sich die zugrunde liegenden Daten ändern. Sie können `IOptionsSnapshot<T>` in Controller in Ihrer Anwendung einfügen, um auf die aktuellste Konfiguration zuzugreifen, die in Azure App Configuration gespeichert ist. Darüber hinaus können Sie die ASP.NET Core-Clientbibliothek von App Configuration einrichten, um alle Änderungen eines App-Konfigurationsspeichers fortlaufend zu überwachen und abzurufen, indem Sie in selbst definierten regelmäßigen Abständen Abfragen durchführen.

In diesem Tutorial wird veranschaulicht, wie Sie dynamische Konfigurationsupdates in Ihrem Code implementieren können. Dies baut auf der Web-App auf, die in den Schnellstartanleitungen vorgestellt wurde. Arbeiten Sie zunächst [Erstellen einer ASP.NET Core-App mit App Configuration](./quickstart-aspnet-core-app.md) durch, bevor Sie fortfahren.

Sie können einen beliebigen Code-Editor nutzen, um die Schritte dieser Schnellstartanleitung auszuführen. [Visual Studio Code](https://code.visualstudio.com/) ist aber eine hervorragende Option, die auf Windows-, macOS- und Linux-Plattformen verfügbar ist.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten Ihrer Anwendung für die Aktualisierung der Konfiguration als Reaktion auf Änderungen in einem App-Konfigurationsspeicher
> * Einfügen der aktuellen Konfiguration in die Controller Ihrer Anwendung

## <a name="prerequisites"></a>Voraussetzungen

Installieren Sie das [.NET Core SDK](https://dotnet.microsoft.com/download), um diese Schnellstartanleitung durcharbeiten zu können.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="reload-data-from-app-configuration"></a>Erneutes Laden von Daten aus App Configuration

1. Öffnen Sie die Datei *Program.cs*, und aktualisieren Sie die `CreateWebHostBuilder`-Methode, indem Sie die `config.AddAzureAppConfiguration()`-Methode hinzufügen.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(o => o.Connect(settings["ConnectionStrings:AppConfig"])
                    .Watch("TestApp:Settings:BackgroundColor", TimeSpan.FromSeconds(1))
                    .Watch("TestApp:Settings:FontColor", TimeSpan.FromSeconds(1))
                    .Watch("TestApp:Settings:Message", TimeSpan.FromSeconds(1)));
            })
            .UseStartup<Startup>();
    ```

    Der zweite Parameter in der `.Watch`-Methode ist das Abrufintervall, mit dem die ASP.NET-Clientbibliothek einen App-Konfigurationsspeicher abfragt, um zu ermitteln, ob für die Konfigurationseinstellung eine Änderung vorgenommen wurde.

2. Fügen Sie die Datei *Settings.cs* hinzu, mit der eine neue `Settings`-Klasse definiert und implementiert wird.

    ```csharp
    namespace TestAppConfig
    {
        public class Settings
        {
            public string BackgroundColor { get; set; }
            public long FontSize { get; set; }
            public string FontColor { get; set; }
            public string Message { get; set; }
        }
    }
    ```

3. Öffnen Sie die Datei *Startup.cs*, und aktualisieren Sie die `ConfigureServices`-Methode, um die Konfigurationsdaten an die `Settings`-Klasse zu binden.

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.Configure<Settings>(Configuration.GetSection("TestApp:Settings"));

        services.Configure<CookiePolicyOptions>(options =>
        {
            options.CheckConsentNeeded = context => true;
            options.MinimumSameSitePolicy = SameSiteMode.None;
        });

        services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
    }
    ```

## <a name="use-the-latest-configuration-data"></a>Verwenden der aktuellsten Konfigurationsdaten

1. Öffnen Sie die Datei *HomeController.cs* im Verzeichnis *Controllers*, aktualisieren Sie die `HomeController`-Klasse, um `Settings` per Abhängigkeitsinjektion zu erhalten, und nutzen Sie die darin enthaltenen Werte.

    ```csharp
    public class HomeController : Controller
    {
        private readonly Settings _settings;
        public HomeController(IOptionsSnapshot<Settings> settings)
        {
            _settings = settings.Value;
        }

        public IActionResult Index()
        {
            ViewData["BackgroundColor"] = _settings.BackgroundColor;
            ViewData["FontSize"] = _settings.FontSize;
            ViewData["FontColor"] = _settings.FontColor;
            ViewData["Message"] = _settings.Message;

            return View();
        }
    }
    ```

2. Öffnen Sie die Datei *Index.cshtml* im Verzeichnis *Views* > *Home*, und ersetzen Sie dessen Inhalt durch Folgendes:

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <style>
        body {
            background-color: @ViewData["BackgroundColor"]
        }
        h1 {
            color: @ViewData["FontColor"];
            font-size: @ViewData["FontSize"];
        }
    </style>
    <head>
        <title>Index View</title>
    </head>
    <body>
        <h1>@ViewData["Message"]</h1>
    </body>
    </html>
    ```

## <a name="build-and-run-the-app-locally"></a>Lokales Erstellen und Ausführen der App

1. Führen Sie den folgenden Befehl in der Befehlsshell aus, um die App per .NET Core-CLI zu erstellen:

        dotnet build

2. Führen Sie nach dem erfolgreichen Abschluss des Buildvorgangs den folgenden Befehl aus, um die Web-App lokal auszuführen:

        dotnet run

3. Starten Sie ein Browserfenster, und navigieren Sie zu `http://localhost:5000`. Dies ist die Standard-URL für die lokal gehostete Web-App.

    ![Schnellstartanleitung: Lokales Starten der App](./media/quickstarts/aspnet-core-app-launch-local-before.png)

4. Melden Sie sich am [Azure-Portal](https://aka.ms/azconfig/portal) an, und klicken Sie auf **Alle Ressourcen** und dann auf die Instanz des App-Konfigurationsspeichers, den Sie in der Schnellstartanleitung erstellt haben.

5. Klicken Sie auf **Schlüssel/Wert-Explorer**, und aktualisieren Sie die Werte der folgenden Schlüssel:

    | Schlüssel | Wert |
    |---|---|
    | TestAppSettings:BackgroundColor | blue |
    | TestAppSettings:FontColor | lightGray |
    | TestAppSettings:Message | Daten aus Azure App Configuration – jetzt mit Liveupdates! |

6. Aktualisieren Sie die Browserseite, um die neuen Konfigurationseinstellungen anzuzeigen.

    ![Schnellstart: Lokale Aktualisierung der App](./media/quickstarts/aspnet-core-app-launch-local-after.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine verwaltete Azure-Dienstidentität hinzugefügt, um den Zugriff auf App Configuration zu optimieren und die Verwaltung der Anmeldeinformationen für Ihre App zu verbessern. Fahren Sie mit den Azure CLI-Beispielen fort, um weitere Informationen zu App Configuration zu erhalten.

> [!div class="nextstepaction"]
> [CLI-Beispiele](./cli-samples.md)
