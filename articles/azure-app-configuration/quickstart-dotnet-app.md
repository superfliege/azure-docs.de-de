---
title: Schnellstart für Azure App Configuration mit .NET Framework | Microsoft-Dokumentation
description: Enthält eine Schnellstartanleitung für die Verwendung von Azure App Configuration mit .NET Framework-Apps.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: .NET
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: b5e41b1f9ee982b8ff8c86232f715d5dab705cd6
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2019
ms.locfileid: "56962161"
---
# <a name="quickstart-create-a-net-framework-app-with-azure-app-configuration"></a>Schnellstart: Erstellen einer .NET Framework-App mit Azure App Configuration

Azure App Configuration ist ein verwalteter Konfigurationsdienst in Azure. Hiermit können Sie Ihre gesamten Anwendungseinstellungen leicht an einem zentralen Ort speichern und verwalten, der von Ihrem Code getrennt ist. In dieser Schnellstartanleitung wird veranschaulicht, wie Sie den Dienst in eine .NET Framework-basierte Windows-Desktop-Konsolen-App einbinden.

![Schnellstart – Lokale Durchführung](./media/quickstarts/dotnet-fx-app-run.png)

## <a name="prerequisites"></a>Voraussetzungen

Installieren Sie zum Durcharbeiten dieser Schnellstartanleitung [Visual Studio 2017](https://visualstudio.microsoft.com/vs) und [.NET Framework 4.7.1](https://dotnet.microsoft.com/download) oder höher, falls Sie dies noch nicht getan haben.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Erstellen eines App-Konfigurationsspeichers

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

## <a name="create-a-net-console-app"></a>Erstellen einer .NET-Konsolen-App

1. Starten Sie Visual Studio, und wählen Sie **Datei** > **Neu** > **Projekt**.

2. Wählen Sie im Dialogfeld **Neues Projekt** die Option **Installiert**, erweitern Sie **Visual C#** > **Windows Desktop**, und wählen Sie **Konsolen-App (.NET Framework)**. Geben Sie einen **Namen** für Ihr Projekt ein, wählen Sie die Option **.NET Framework 4.7.1** oder höher, und klicken Sie auf **OK**.

## <a name="connect-to-app-configuration-store"></a>Herstellen einer Verbindung mit dem App-Konfigurationsspeicher

1. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Pakete verwalten...**. Suchen Sie über die Registerkarte **Durchsuchen** nach den folgenden NuGet-Paketen, und fügen Sie sie Ihrem Projekt hinzu. (Aktivieren Sie das Kontrollkästchen **Vorabversion einbeziehen**, falls Sie sie nicht finden können.)
    ```
    Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration 1.0.0 preview or later
    Microsoft.Configuration.ConfigurationBuilders.Environment 2.0.0 preview or later
    ```

2. Aktualisieren Sie die Datei *App.config* Ihres Projekts wie folgt:

    ```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>

    <configBuilders>
        <builders>
            <add name="MyConfigStore" mode="Greedy" connectionString="${ConnectionString}" type="Microsoft.Configuration.ConfigurationBuilders.AzureAppConfigurationBuilder, Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration" />
            <add name="Environment" mode="Greedy" type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Environment" />
        </builders>
    </configBuilders>

    <appSettings configBuilders="Environment,MyConfigStore">
        <add key="AppName" value="Console App Demo" />
        <add key="ConnectionString" value ="Set via an environment variable - for example, dev, test, staging, or production connection string." />
    </appSettings>
    ```

   Beachten Sie Folgendes: Da die Verbindungszeichenfolge Ihres App-Konfigurationsspeichers aus der Umgebungsvariablen `ConnectionString` ausgelesen wird, ist es wichtig, den Konfigurationsbuilder `Environment` im Abschnitt `appSettings` vor dem `MyConfigStore`-Objekt in der `configBuilders`-Eigenschaft hinzuzufügen.

3. Öffnen Sie die Datei *Program.cs*, und aktualisieren Sie die `Main`-Methode für die Verwendung von App Configuration, indem Sie `ConfigurationManager` aufrufen.

    ```csharp
    static void Main(string[] args)
    {
        string message = ConfigurationManager.AppSettings["TestApp:Settings:Message"];

        Console.WriteLine(message);
    }
    ```

## <a name="build-and-run-the-app-locally"></a>Lokales Erstellen und Ausführen der App

1. Legen Sie eine Umgebungsvariable mit dem Namen **ConnectionString** auf die Verbindungszeichenfolge Ihres App-Konfigurationsspeichers fest. Führen Sie bei Verwendung der Windows-Eingabeaufforderung den folgenden Befehl aus:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Führen Sie bei Verwendung von Windows PowerShell den folgenden Befehl aus:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

2. Starten Sie Visual Studio neu, und warten Sie, bis die Änderung wirksam wird. Drücken Sie anschließend auf der Tastatur **STRG+F5**, um die Konsolen-App zu erstellen und auszuführen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen neuen App-Konfigurationsspeicher erstellt und mit einer .NET Framework-Konsolen-App verwendet. Weitere Informationen zur Verwendung von App Configuration finden Sie im nächsten Tutorial, in dem es um die Authentifizierung geht.

> [!div class="nextstepaction"]
> [Verwaltete Identitäten für Azure-Ressourcenintegration](./integrate-azure-managed-service-identity.md)
