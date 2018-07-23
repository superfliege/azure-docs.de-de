---
title: 'Tutorial: Konfigurieren einer Azure-Webanwendung zum Lesen eines Geheimnisses aus Key Vault | Microsoft-Dokumentation'
description: 'Tutorial: Konfigurieren einer ASP.NET Core-Anwendung zum Lesen eines Geheimnisses aus Key Vault'
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: identity
ms.topic: tutorial
ms.date: 05/17/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 747a0fc7f66edbae8d4a99eeaf0ea45f844d6465
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125938"
---
# <a name="tutorial-configure-an-azure-web-application-to-read-a-secret-from-key-vault"></a>Tutorial: Konfigurieren einer Azure-Webanwendung zum Lesen eines Geheimnisses aus Key Vault

In diesem Tutorial wird ausführlich erläutert, mit Sie eine Azure-Webanwendung zum Lesen von Informationen aus Key Vault unter Verwendung von verwalteten Dienstidentitäten konfigurieren. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen einer Key Vault-Instanz
> * Speichern eines Geheimnisses in Key Vault
> * Erstellen einer Azure-Webanwendung
> * Aktivieren von verwalteten Dienstidentitäten
> * Erteilen der erforderlichen Berechtigungen zum Lesen von Daten aus Key Vault für die Anwendung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu.

Geben Sie Folgendes ein, um sich über die Befehlszeilenschnittstelle bei Azure anzumelden:

```azurecli
az login
```

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *ContosoResourceGroup* am Standort *eastus*.

```azurecli
# To list locations: az account list-locations --output table
az group create --name "ContosoResourceGroup" --location "East US"
```

Die eben erstellte Ressourcengruppe wird im gesamten Tutorial verwendet.

## <a name="create-an-azure-key-vault"></a>Erstellen einer Azure Key Vault-Instanz

Als Nächstes erstellen Sie eine Key Vault-Instanz in der Ressourcengruppe aus dem vorherigen Schritt. In diesem Tutorial lautet der Name für die Key Vault-Instanz zwar „ContosoKeyVault“, Sie müssen jedoch einen eindeutigen Namen verwenden. Geben Sie die folgenden Informationen ein:

* Der Tresorname lautet **ContosoKeyVault**.
* Der Ressourcengruppenname lautet **ContosoResourceGroup**.
* Als Standort wird **USA, Osten** verwendet.

```azurecli
az keyvault create --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --location "East US"
```

Die Ausgabe dieses Befehls zeigt Eigenschaften der neu erstellten Key Vault-Instanz. Beachten Sie die beiden folgenden Eigenschaften:

* **Tresorname**: In diesem Beispiel ist dies **ContosoKeyVault**. Sie verwenden den Namen Ihrer Key Vault-Instanz für alle Key Vault-Befehle.
* **Tresor-URI**: In diesem Beispiel ist dies „https://<YourKeyVaultName>.vault.azure.net/“. Anwendungen, die Ihren Tresor über die zugehörige REST-API nutzen, müssen diesen URI verwenden.

>[!IMPORTANT]
> Wenn ein Fehler mit dem Hinweis angezeigt wird, dass der Parameter „vault_name“ dem Muster „^[a-zA-Z0-9-]{3,24}$“ entsprechen muss, war der Parameter „-name“ nicht eindeutig oder entsprach keiner Zeichenfolge aus 3 bis 24 alphanumerischen Zeichen.

An diesem Punkt ist nur Ihr Azure-Konto zum Ausführen von Vorgängen für den neuen Tresor autorisiert.

## <a name="add-a-secret-to-key-vault"></a>Hinzufügen eines Geheimnisses zu Key Vault

Wir fügen ein Geheimnis hinzu, um die Vorgehensweise zu veranschaulichen. Sie können eine SQL-Verbindungszeichenfolge oder beliebige andere Informationen speichern, die sicher aufbewahrt, aber für Ihre Anwendung verfügbar gemacht werden müssen. In diesem Tutorial heißt das Kennwort **AppSecret**, und darin wird der Wert **MySecret** gespeichert.

Geben Sie die folgenden Befehle ein, um in Key Vault ein Geheimnis namens **AppSecret** mit dem Wert **Pa$$MySecret** zu erstellen:

```azurecli
az keyvault secret set --vault-name "ContosoKeyVault" --name "AppSecret" --value "MySecret"
```

Geben Sie Folgendes ein, um den Wert im Geheimnis als Nur-Text anzuzeigen:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "ContosoKeyVault"
```

Mit diesem Befehl werden die Geheimnisinformationen einschließlich des URI angezeigt. Nach der Ausführung dieser Schritte sollten Sie einen URI zu einem Geheimnis in einer Azure Key Vault-Instanz besitzen. Notieren Sie sich diese Informationen. Sie benötigen sie in einem späteren Schritt.

## <a name="create-a-web-app"></a>Erstellen einer Web-App

In diesem Abschnitt erstellen Sie eine ASP.NET MVC-Anwendung und stellen sie in Azure als Web-App bereit. Weitere Informationen zu Azure-Web-Apps finden Sie unter [Web-Apps – Übersicht](../app-service/app-service-web-overview.md).

1. Erstellen Sie in Visual Studio ein Projekt durch Auswählen von **Datei > Neu > Projekt**. 

2. Wählen Sie im Dialogfeld **Neues Projekt** die Optionen **Visual C# > Web > ASP.NET Core-Webanwendung** aus.

3. Nennen Sie die Anwendung **WebKeyVault**, und klicken Sie dann auf **OK**.
   >[!IMPORTANT]
   > Sie müssen der App den Namen „WebKeyVault“ geben, damit der von Ihnen kopierte und eingefügte Code dem Namespace entspricht. Wenn Sie der Site einen anderen Namen geben, müssen Sie den Code anpassen, sodass er dem Sitenamen entspricht.

    ![Dialogfeld "Neues ASP.NET-Projekt"](media/tutorial-web-application-keyvault/aspnet-dialog.png)

4. Sie können jede Art von ASP.NET Core-Web-App für Azure bereitstellen. Wählen Sie in diesem Tutorial die Vorlage **Webanwendung** aus, und vergewissern Sie sich, dass die Authentifizierung auf **Keine Authentifizierung** festgelegt ist.

    ![ASP.NET-Dialogfeld „Keine Authentifizierung“](media/tutorial-web-application-keyvault/aspnet-noauth.png)

5. Klicken Sie auf **OK**.

6. Sobald das ASP.NET Core-Projekt erstellt wurde, wird die ASP.NET Core-Startseite mit zahlreichen Links zu Ressourcen für die ersten Schritte angezeigt.

7. Wählen Sie im Menü **Debuggen > Starten ohne Debugging** aus, um die Web-App lokal auszuführen.

## <a name="modify-the-web-app"></a>Ändern der Web-App

Zwei NuGet-Pakete müssen für Ihre Webanwendung installiert sein. Führen Sie zum Installieren die folgenden Schritte aus:

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Websitenamen.
2. Klicken Sie auf **NuGet-Pakete für Projektmappe verwalten...**.
3. Aktivieren Sie das Kontrollkästchen neben dem Suchfeld: **Vorabversion einbeziehen**
4. Suchen Sie nach den beiden unten aufgeführten NuGet-Paketen, und lassen Sie zu, dass sie zu Ihrer Projektmappe hinzugefügt werden:

    * [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication): Dieses Paket vereinfacht das Abrufen von Zugriffstoken für Szenarien mit Dienst-zu-Azure-Authentifizierung. 
    * [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) enthält Methoden für die Interaktion mit Key Vault.

5. Öffnen Sie `Program.cs` mithilfe des Projektmappen-Explorers, und ersetzen Sie den Inhalt der Datei „Program.cs“ durch den folgenden Code. Ersetzen Sie ```<YourKeyVaultName>``` durch den Namen Ihres Schlüsseltresors:

    ```csharp
    
    using Microsoft.AspNetCore;
    using Microsoft.AspNetCore.Hosting;
    using Microsoft.Azure.KeyVault;
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Configuration.AzureKeyVault;
    
    namespace WebKeyVault
    {
       public class Program
       {
           public static void Main(string[] args)
           {
               BuildWebHost(args).Run();
           }

           public static IWebHost BuildWebHost(string[] args) =>
           WebHost.CreateDefaultBuilder(args)
               .ConfigureAppConfiguration((ctx, builder) =>
               {
                   var keyVaultEndpoint = GetKeyVaultEndpoint();
                   if (!string.IsNullOrEmpty(keyVaultEndpoint))
                   {
                       var azureServiceTokenProvider = new AzureServiceTokenProvider();
                       var keyVaultClient = new KeyVaultClient(
                           new KeyVaultClient.AuthenticationCallback(
                               azureServiceTokenProvider.KeyVaultTokenCallback));
                       builder.AddAzureKeyVault(
                           keyVaultEndpoint, keyVaultClient, new DefaultKeyVaultSecretManager());
                   }
               }
            ).UseStartup<Startup>()
             .Build();

           private static string GetKeyVaultEndpoint() => "https://<YourKeyVaultName>.vault.azure.net";
         }
    }
    ```

6. Navigieren Sie mithilfe des Projektmappen-Explorers zum Abschnitt **Seiten**, und öffnen Sie `About.cshtml`. Ersetzen Sie den Inhalt der Datei **About.cshtml.cs** durch den folgenden Code:

    ```csharp
    
    using Microsoft.AspNetCore.Mvc.RazorPages;
    using Microsoft.Extensions.Configuration;
    
    namespace WebKeyVault.Pages
    {
        public class AboutModel : PageModel
        {
            public AboutModel(IConfiguration configuration)
            {
                _configuration = configuration;
            }
    
            private readonly IConfiguration _configuration = null;
            public string Message { get; set; }
    
            public void OnGet()
            {
                Message = "My key val = " +  _configuration["AppSecret"];
            }
        }
    }
    
    ```

7. Klicken Sie im Hauptmenü auf **Debuggen** > **Ohne Debuggen starten**. Navigieren Sie im angezeigten Browser zur Seite **Info**. Der Wert für „AppSecret“ wird angezeigt.

>[!IMPORTANT]
> Wenn Sie eine Fehlermeldung des Typs „HTTP-Fehler 502.5 – Prozessfehler“ erhalten,
> > überprüfen Sie den Namen des in `Program.cs` angegebenen Schlüsseltresors.

## <a name="publish-the-web-application-to-azure"></a>Veröffentlichen der Webanwendung in Azure

1. Klicken Sie über dem Editor auf **WebKeyVault**.
2. Klicken Sie auf **Veröffentlichen** und dann auf **Starten**.
3. Erstellen Sie eine neue Instanz von **App Service**, und klicken Sie auf **Veröffentlichen**.
4. Klicken Sie auf **Erstellen**.

>[!IMPORTANT]
> Ein Browserfenster wird geöffnet, und die Meldung „502.5 – Prozessfehler“ wird angezeigt. Dies entspricht dem erwarteten Verhalten. Sie müssen der Anwendung Identitätsberechtigungen zum Lesen von Geheimnissen aus Key Vault gewähren.

## <a name="enable-managed-service-identity"></a>Aktivieren der verwalteten Dienstidentität

Azure Key Vault bietet eine Möglichkeit zum sicheren Speichern von Anmeldeinformationen und anderen Schlüsseln und Geheimnissen. Um diese abrufen zu können, muss sich Ihr Code jedoch bei Key Vault authentifizieren. Mithilfe von MSI (Managed Service Identity, verwaltete Dienstidentität) kann dieses Problem leichter gelöst werden, indem für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory (Azure AD) bereitgestellt wird. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung einschließlich von Key Vault unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein.

1. Kehren Sie zur Azure-Befehlszeilenschnittstelle zurück.
2. Führen Sie den Befehl „assign-identity“ aus, um die Identität für diese Anwendung zu erstellen:

```azurecli
az webapp identity assign --name "WebKeyVault" --resource-group "ContosoResourcegroup"
```

>[!NOTE]
>Dieser Befehl entspricht dem Aufrufen des Portals und dem Festlegen von **Verwaltete Dienstidentität** auf **Ein** in den Webanwendungseigenschaften.

## <a name="grant-rights-to-the-application-identity"></a>Gewähren von Berechtigungen für die Anwendungsidentität

Navigieren Sie im Azure-Portal zu den Key Vault-Zugriffsrichtlinien, und erteilen Sie sich selbst Zugriff vom Typ „Verwaltung von Geheimnissen“ auf die Key Vault-Instanz. Dadurch können Sie die Anwendung auf Ihrem lokalen Entwicklungscomputer ausführen.

1. Suchen Sie im Azure-Portal im Dialogfeld **Ressourcen suchen** nach Ihrer Key Vault-Instanz.
2. Klicken Sie auf **Zugriffsrichtlinien**.
3. Klicken Sie auf **Neue hinzufügen** und anschließend im Abschnitt **Berechtigungen für Geheimnis** auf **Abrufen** und **Auflisten**.
4. Klicken Sie auf **Prinzipal auswählen**, und fügen Sie die Anwendungsidentität hinzu. Sie hat den gleichen Namen wie die Anwendung.
5. Klicken Sie auf **OK**.

Ihr Konto in Azure und die Anwendungsidentität verfügen nun über Berechtigungen zum Lesen von Informationen aus Key Vault. Wenn Sie die Seite aktualisieren, sollte die Zielseite angezeigt werden. Wenn Sie auf **Info** klicken, wird der in Key Vault gespeicherte Wert angezeigt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Führen Sie zum Löschen einer Ressourcengruppe und all ihrer Ressourcen den Befehl **az group delete** aus.

  ```azurecli
  az group delete -n "ContosoResourceGroup"
  ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Entwicklerhandbuch zu Azure-Schlüsseltresor](key-vault-developers-guide.md)
