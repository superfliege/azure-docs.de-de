---
title: Tutorial für das Integrieren mit einer Continuous Integration- und Continuous Delivery-Pipeline unter Verwendung von Azure App Configuration | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie eine Konfigurationsdatei unter Verwendung von Daten in Azure App Configuration während Continuous Integration und Continuous Delivery generieren.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: tutorial
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 7db796b33bab941f038afab1b80127aded50b54a
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "60000016"
---
# <a name="integrate-with-a-cicd-pipeline"></a>Integrieren in eine CI/CD-Pipeline

Sie können die Resilienz Ihrer Anwendung für den unwahrscheinlichen Fall erhöhen, dass Azure App Configuration nicht erreichbar ist. Fassen Sie hierzu die aktuellen Konfigurationsdaten in einer Datei zusammen, die gemeinsam mit der Anwendung bereitgestellt und beim Start lokal geladen wird. Dadurch wird sichergestellt, dass Ihre Anwendung mindestens über Standardeinstellungen verfügt. Diese Werte werden durch neuere Änderungen in einem App-Konfigurationsspeicher überschrieben (sofern verfügbar).

Mithilfe der Azure App Configuration-Funktion [Exportieren](./howto-import-export-data.md#export-data) können Sie das Abrufen aktueller Konfigurationsdaten als einzelne Datei automatisieren. Betten Sie diese Datei anschließend in einen Build- oder Bereitstellungsschritt in Ihre CI/CD-Pipeline (Continuous Integration/Continuous Deployment) ein.

Das folgende Beispiel zeigt, wie Sie App Configuration-Daten als Buildschritt für die in den Schnellstartanleitungen eingeführte Web-App integrieren. Durchlaufen Sie zuerst die Schnellstartanleitung zum [Erstellen einer ASP.NET Core-App mit Azure App Configuration](./quickstart-aspnet-core-app.md), bevor Sie fortfahren.

Die Schritte in dieser Schnellstartanleitung können mit einem beliebigen Code-Editor ausgeführt werden. [Visual Studio Code](https://code.visualstudio.com/) ist eine hervorragende Option, die auf Windows-, macOS- und Linux-Plattformen verfügbar ist.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie die App lokal erstellen möchten, laden Sie die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) herunter, und installieren Sie sie (sofern noch nicht geschehen).

Für einen Cloud-Build (etwa mit Azure DevOps) muss die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) auf Ihrem Buildsystem installiert sein.

## <a name="export-an-app-configuration-store"></a>Exportieren eines App-Konfigurationsspeichers

1. Öffnen Sie Ihre*CSPROJ-Datei*, und fügen Sie das folgende Skript hinzu:

    ```xml
    <Target Name="Export file" AfterTargets="Build">
        <Message Text="Export the configurations to a temp file. " />
        <Exec WorkingDirectory="$(MSBuildProjectDirectory)" Condition="$(ConnectionString) != ''" Command="az appconfig kv export -f $(OutDir)\azureappconfig.json --format json --separator : --connection-string $(ConnectionString)" />
    </Target>
    ```

    Fügen Sie die Ihrem App-Konfigurationsspeicher zugeordnete Verbindungszeichenfolge (*ConnectionString*) als Umgebungsvariable hinzu.

2. Öffnen Sie „Program.cs“, und aktualisieren Sie die Methode `CreateWebHostBuilder` für die Verwendung der exportierten JSON-Datei durch Aufrufen der Methode `config.AddJsonFile()`.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var directory = System.IO.Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location);
                var settings = config.Build();

                config.AddJsonFile(Path.Combine(directory, "azureappconfig.json"));
                config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
            })
            .UseStartup<Startup>();
    ```

## <a name="build-and-run-the-app-locally"></a>Lokales Erstellen und Ausführen der App

1. Legen Sie eine Umgebungsvariable mit dem Namen **ConnectionString** fest, und geben Sie dafür den Zugriffsschlüssel für Ihren App-Konfigurationsspeicher an. Führen Sie bei Verwendung einer Windows-Eingabeaufforderung den folgenden Befehl aus, und starten Sie die Eingabeaufforderung neu, damit die Änderung wirksam wird:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Führen Sie bei Verwendung von Windows PowerShell den folgenden Befehl aus:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    Führen Sie bei Verwendung von macOS oder Linux den folgenden Befehl aus:

        export ConnectionString='connection-string-of-your-app-configuration-store'

2. Führen Sie den folgenden Befehl in der Befehlsshell aus, um die App mithilfe der .NET Core-CLI zu erstellen:

        dotnet build

3. Führen Sie nach erfolgreicher Erstellung den folgenden Befehl aus, um die Web-App lokal auszuführen:

        dotnet run

4. Öffnen Sie ein Browserfenster, und navigieren Sie zu `http://localhost:5000`. Dies ist die Standard-URL für die lokal gehostete Web-App.

    ![Schnellstartanleitung: Lokales Starten der App](./media/quickstarts/aspnet-core-app-launch-local.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie in einer Bereitstellungspipeline zu verwendende Azure App Configuration-Daten exportiert. Fahren Sie mit den Azure CLI-Beispielen fort, um mehr über die Verwendung von App Configuration zu erfahren.

> [!div class="nextstepaction"]
> [Integration der verwalteten Identität](./howto-integrate-azure-managed-service-identity.md)
