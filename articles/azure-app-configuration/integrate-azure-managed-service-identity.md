---
title: Tutorial zur Integration mit verwalteten Azure-Identitäten | Microsoft-Dokumentation
description: In diesem Tutorial wird beschrieben, wie Sie verwaltete Azure-Identitäten verwenden, um die Authentifizierung für Azure App Configuration durchzuführen und Zugriff darauf zu erhalten.
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
ms.openlocfilehash: 874522b6b4ca3739e0736d4f70f76bb82cad25f9
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2019
ms.locfileid: "56957350"
---
# <a name="tutorial-integrate-with-azure-managed-identities"></a>Tutorial: Integrieren mit verwalteten Azure-Identitäten

Mit [verwalteten Identitäten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) von Azure Active Directory wird die Verwaltung von Geheimnissen für Ihre Cloudanwendung vereinfacht. Mit einer verwalteten Identität können Sie Ihren Code für die Nutzung des Dienstprinzipals einrichten, der für den Azure-Computedienst erstellt wurde, auf dem die Ausführung erfolgt. Hierbei werden also keine separaten Anmeldeinformationen verwendet, die in Azure Key Vault oder einer lokalen Verbindungszeichenfolge gespeichert sind. Azure App Configuration und die zugehörigen .NET Core-, .NET- und Java Spring-Clientbibliotheken verfügen über integrierte MSI-Unterstützung. Die Verwendung von MSI ist zwar nicht obligatorisch, aber hierbei entfällt das Zugriffstoken mit Geheimnissen. Ihrem Code muss lediglich der Dienstendpunkt für einen App-Konfigurationsspeicher bekannt sein, damit darauf zugegriffen werden kann. Sie können diese URL direkt in Ihren Code einbetten, ohne fürchten zu müssen, dass Geheimnisse offengelegt werden.

In diesem Tutorial wird veranschaulicht, wie Sie MSI für den Zugriff auf App Configuration nutzen können. Dies baut auf der Web-App auf, die in den Schnellstartanleitungen vorgestellt wurde. Arbeiten Sie zunächst [Erstellen einer ASP.NET Core-App mit App Configuration](./quickstart-aspnet-core-app.md) durch, bevor Sie fortfahren.

Sie können einen beliebigen Code-Editor nutzen, um die Schritte in diesem Tutorial auszuführen. [Visual Studio Code](https://code.visualstudio.com/) ist aber eine hervorragende Option, die auf Windows-, macOS- und Linux-Plattformen verfügbar ist.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Gewähren des Zugriffs auf App Configuration für eine verwaltete Identität
> * Konfigurieren Ihrer App für die Verwendung einer verwalteten Identität bei der Verbindungsherstellung mit App Configuration

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* [.NET Core SDK](https://www.microsoft.com/net/download/windows)
* [Konfigurierte Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="add-a-managed-identity"></a>Hinzufügen einer verwalteten Identität

Um eine verwaltete Entität im Portal einzurichten, erstellen Sie wie gewohnt zuerst eine Anwendung und aktivieren dann das Feature.

1. Erstellen Sie wie gewohnt im [Azure-Portal](https://aka.ms/azconfig/portal) eine App. Navigieren Sie im Portal zu dieser App.

2. Scrollen Sie im linken Navigationsbereich nach unten zur Gruppe **Einstellungen**, und wählen Sie **Identität**.

3. Ändern Sie auf der Registerkarte **Systemseitig zugewiesen** den **Status** in **Ein**, und klicken Sie auf **Speichern**.

    ![Festlegen der verwalteten Identität in App Service](./media/set-managed-identity-app-service.png)

## <a name="grant-access-to-app-configuration"></a>Gewähren des Zugriffs auf App Configuration

1. Klicken Sie im [Azure-Portal](https://aka.ms/azconfig/portal) auf **Alle Ressourcen** und dann auf den App-Konfigurationsspeicher, den Sie anhand der Schnellstartanleitung erstellt haben.

2. Wählen Sie die Option **Zugriffssteuerung (IAM)**.

3. Klicken Sie auf der Registerkarte **Zugriff überprüfen** im Kartenelement **Rollenzuweisung hinzufügen** auf **Hinzufügen**.

4. Legen Sie **Rolle** auf *Mitwirkender* und **Zugriff zuweisen zu** auf *App Service* (unter *Systemseitig zugewiesene verwaltete Identität*) fest.

5. Legen Sie **Abonnement** auf Ihr Azure-Abonnement fest, und wählen Sie die App Service-Ressource für Ihre App aus.

6. Klicken Sie auf **Speichern**.

    ![Hinzufügen einer verwalteten Identität](./media/add-managed-identity.png)

## <a name="use-a-managed-identity"></a>Verwenden einer verwalteten Identität

1. Öffnen Sie *appsettings.json*, fügen Sie Folgendes hinzu, und ersetzen Sie *<service_endpoint>* (mit den Klammern) durch die URL für Ihren App-Konfigurationsspeicher:

    ```json
    "AppConfig": {
        "Endpoint": "<service_endpoint>"
    }
    ```

2. Öffnen Sie die Datei *Program.cs*, und aktualisieren Sie die `CreateWebHostBuilder`-Methode, indem Sie die `config.AddAzureAppConfiguration()`-Methode ersetzen.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(o => o.ConnectWithManagedIdentity(settings["AppConfig:Endpoint"]));
            })
            .UseStartup<Startup>();
    ```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-from-local-git"></a>Bereitstellen über lokales Git

Die einfachste Möglichkeit zum Aktivieren einer lokalen Git-Bereitstellung für Ihre App mit dem Kudu-Buildserver ist die Verwendung von Cloud Shell.

### <a name="configure-a-deployment-user"></a>Konfigurieren eines Bereitstellungsbenutzers

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="enable-local-git-with-kudu"></a>Aktivieren von lokalem Git mit Kudu

Zum Aktivieren einer lokalen Git-Bereitstellung für Ihre App mit dem Kudu-Buildserver führen Sie [`az webapp deployment source config-local-git`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) in Cloud Shell aus.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group <group_name>
```

Wenn Sie stattdessen eine Git-fähige App erstellen möchten, führen Sie [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) in Cloud Shell mit dem Parameter `--deployment-local-git` aus.

```azurecli-interactive
az webapp create --name <app_name> --resource-group <group_name> --plan <plan_name> --deployment-local-git
```

Die Ausgabe des Befehls `az webapp create` sollte ungefähr wie folgt aussehen:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="deploy-your-project"></a>Bereitstellen des Projekts

Kehren Sie zum _lokalen Terminalfenster_ zurück, und fügen Sie Ihrem lokalen Git-Repository einen Azure-Remotespeicherort hinzu. Ersetzen Sie _\<Url>_ durch die URL des Git-Remoterepositorys, die Sie beim [Aktivieren von Git für die App](#enable-local-git-with-kudu) erhalten haben.

```bash
git remote add azure <url>
```

Führen Sie einen Pushvorgang zum Azure-Remotespeicherort durch, um Ihre App mit dem folgenden Befehl bereitzustellen. Stellen Sie sicher, dass Sie bei der entsprechenden Aufforderung das Kennwort eingeben, das Sie unter [Konfigurieren eines Bereitstellungsbenutzers](#configure-a-deployment-user) erstellt haben, und nicht das Kennwort für die Anmeldung am Azure-Portal.

```bash
git push azure master
```

Die Ausgabe enthält u.U. laufzeitspezifische Automatisierungen wie MSBuild für ASP.NET, `npm install` für Node.js und `pip install` für Python.

### <a name="browse-to-the-azure-web-app"></a>Navigieren Sie zur Azure-Web-App.

Navigieren Sie in einem Browser zu Ihrer Web-App, um sicherzustellen, dass der Inhalt bereitgestellt wird.

```bash
http://<app_name>.azurewebsites.net
```

![In App Service ausgeführte App](../app-service/media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png)

## <a name="use-managed-identity-in-other-languages"></a>Verwenden einer verwalteten Identität in anderen Sprachen

App Configuration-Anbieter für .NET Framework und Java Spring verfügen auch über integrierte Unterstützung für verwaltete Identitäten. In diesen Fällen verwenden Sie beim Konfigurieren eines Anbieters anstelle der vollständigen Verbindungszeichenfolge einfach den URL-Endpunkt Ihres App-Konfigurationsspeichers. Für die in der Schnellstartanleitung erstellte .NET Framework-Konsolen-App geben Sie beispielsweise in der Datei *App.config* die folgenden Einstellungen an:

    ```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>

    <configBuilders>
        <builders>
            <add name="MyConfigStore" mode="Greedy" endpoint="${Endpoint}" type="Microsoft.Configuration.ConfigurationBuilders.AzureAppConfigurationBuilder, Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration" />
            <add name="Environment" mode="Greedy" type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Environment" />
        </builders>
    </configBuilders>

    <appSettings configBuilders="Environment,MyConfigStore">
        <add key="AppName" value="Console App Demo" />
        <add key="Endpoint" value ="Set via an environment variable - for example, dev, test, staging, or production endpoint." />
    </appSettings>
    ```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine verwaltete Azure-Dienstidentität hinzugefügt, um den Zugriff auf App Configuration zu optimieren und die Verwaltung der Anmeldeinformationen für Ihre App zu verbessern. Fahren Sie mit den Azure CLI-Beispielen fort, um weitere Informationen zu App Configuration zu erhalten.

> [!div class="nextstepaction"]
> [CLI-Beispiele](./cli-samples.md)
