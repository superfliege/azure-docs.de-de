---
title: 'Tutorial: Verwenden von Azure Key Vault mit einem virtuellen Azure-Windows-Computer in .NET – Azure Key Vault | Microsoft-Dokumentation'
description: 'Tutorial: Konfigurieren einer ASP.NET Core-Anwendung zum Lesen eines Geheimnisses aus Key Vault'
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: key-vault
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: a19da45d849facc8fe7ed18d95862ab9e79eaace
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2019
ms.locfileid: "55744380"
---
# <a name="tutorial-how-to-use-azure-key-vault-with-azure-windows-virtual-machine-in-net"></a>Tutorial: Verwenden von Azure Key Vault mit einer Azure-Windows-VM in .NET

Azure Key Vault unterstützt Sie beim Schutz von Geheimnissen wie API-Schlüsseln und Datenbank-Verbindungszeichenfolgen, die für den Zugriff auf Ihre Anwendungen, Dienste und IT-Ressourcen benötigt werden.

In diesem Tutorial führen Sie die erforderlichen Schritte aus, um eine Konsolenanwendung zu erhalten und Informationen unter Verwendung von verwalteten Identitäten für Azure-Ressourcen aus Azure Key Vault zu lesen. In diesem Artikel wird Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen eines Schlüsseltresors
> * Speichern eines Geheimnisses im Schlüsseltresor
> * Abrufen eines Geheimnisses aus dem Schlüsseltresor
> * Erstellen eines virtuellen Azure-Computers
> * Aktivieren einer [verwalteten Identität](../active-directory/managed-identities-azure-resources/overview.md) für den virtuellen Computer
> * Erteilen der erforderlichen Berechtigungen zum Lesen von Daten aus dem Schlüsseltresor für die Konsolenanwendung
> * Abrufen von Geheimnissen aus Key Vault

Lesen Sie die Informationen zu [grundlegenden Konzepten](key-vault-whatis.md#basic-concepts), bevor Sie fortfahren.

## <a name="prerequisites"></a>Voraussetzungen
* Alle Plattformen:
  * Git ([Download](https://git-scm.com/downloads))
  * Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
  * [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), Version 2.0.4 oder höher. Diese Version ist für Windows, Mac und Linux verfügbar.

In diesem Tutorial wird die verwaltete Dienstidentität verwendet.

## <a name="what-is-managed-service-identity-and-how-does-it-work"></a>Was ist eine verwaltete Dienstidentität, und wie funktioniert sie?

Gehen wir zunächst auf das Konzept der verwalteten Dienstidentität (Managed Service Identity, MSI) ein. Azure Key Vault kann Anmeldeinformationen sicher speichern. Dadurch befinden sie sich nicht in Ihrem Code. Zum Abrufen der Anmeldeinformationen ist jedoch eine Authentifizierung bei Azure Key Vault erforderlich. Und für die Authentifizierung bei Key Vault sind Anmeldeinformationen erforderlich. Ein klassisches Bootstrap-Problem. Dank Azure und Azure AD bietet MSI eine „Bootstrap-Identität“, die die ersten Schritte erheblich vereinfacht.

Das funktioniert so: Wenn Sie MSI für einen Azure-Dienst wie etwa Virtual Machines, App Service oder Functions aktivieren, erstellt Azure einen [Dienstprinzipal](key-vault-whatis.md#basic-concepts) für die Instanz des Diensts in Azure Active Directory und fügt die Anmeldeinformationen für den Dienstprinzipal in die Instanz des Diensts ein. 

![MSI](media/MSI.png)

Als Nächstes ruft Ihr Code einen lokalen, für die Azure-Ressource verfügbaren Metadatendienst auf, um ein Zugriffstoken zu erhalten.
Ihr Code verwendet das vom lokalen MSI-Endpunkt (MSI_ENDPOINT) erhaltene Zugriffstoken für die Authentifizierung bei einem Azure Key Vault-Dienst. 

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Geben Sie Folgendes ein, um sich mithilfe der Azure-Befehlszeilenschnittstelle bei Azure anzumelden:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Wählen Sie einen Namen für die Ressourcengruppe aus, und fügen Sie den Platzhalter ein.
Im folgenden Beispiel wird eine Ressourcengruppe am Standort „USA, Westen“ erstellt:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Die eben erstellte Ressourcengruppe wird im gesamten Artikel verwendet.

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors

Als Nächstes erstellen Sie einen Schlüsseltresor in der Ressourcengruppe aus dem vorherigen Schritt. Geben Sie die folgenden Informationen ein:

* Name des Schlüsseltresors: Der Name muss eine Zeichenfolge aus 3 bis 24 Zeichen sein und darf nur die Zeichen 0-9, a-z, A-Z und Bindestrich (-) enthalten.
* Ressourcengruppenname
* Standort: **USA, Westen**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
An diesem Punkt ist nur Ihr Azure-Konto zum Ausführen von Vorgängen für den neuen Tresor autorisiert.

## <a name="add-a-secret-to-the-key-vault"></a>Hinzufügen eines Geheimnisses zum Schlüsseltresor

Wir fügen ein Geheimnis hinzu, um die Vorgehensweise zu veranschaulichen. Sie können eine SQL-Verbindungszeichenfolge oder beliebige andere Informationen speichern, die sicher aufbewahrt, aber für Ihre Anwendung verfügbar gemacht werden müssen.

Geben Sie die folgenden Befehle ein, um im Schlüsseltresor ein Geheimnis namens **AppSecret** zu erstellen. Dieses Geheimnis speichert den Wert **MySecret**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers
Informationen zum Erstellen eines virtuellen Windows-Computers finden Sie unter folgenden Links:

[Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-cli) 

[PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell)

[Portal](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

## <a name="assign-identity-to-virtual-machine"></a>Zuweisen einer Identität zum virtuellen Computer
In diesem Schritt erstellen wir mithilfe des folgenden Befehls in der Azure CLI eine vom System zugewiesene ID für den virtuellen Computer.

```
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Beachten Sie das unten gezeigte systemAssignedIdentity-Element. Die Ausgabe des obigen Befehls lautet etwa wie folgt: 

```
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="give-vm-identity-permission-to-key-vault"></a>Erteilen von Berechtigungen für die VM-Identität für Key Vault
Nun können Sie der oben erstellten Identität Berechtigungen für Key Vault erteilen. Führen Sie dazu den folgenden Befehl aus:

```
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="sign-in-to-the-virtual-machine"></a>Melden Sie sich beim virtuellen Computer an.

Sie können [dieses Tutorial](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon) absolvieren.

## <a name="install-net-core"></a>.NET Core installieren

Sie können .NET Core anhand der Schritte in [diesem Artikel](https://www.microsoft.com/net/download) installieren.

## <a name="create-and-run-sample-dot-net-app"></a>Erstellen und Ausführen der .NET-Beispiel-App

Öffnen Sie eine Eingabeaufforderung.

Durch Ausführung der folgenden Befehle sollte in der Konsole „Hallo Welt“ ausgegeben werden.

```
dotnet new console -o helloworldapp
cd helloworldapp
dotnet run
```

## <a name="edit-console-app"></a>Bearbeiten der Konsolen-App
Öffnen Sie die Datei „Program.cs“, und fügen Sie die folgenden Pakete hinzu:
```
using System;
using System.IO;
using System.Net;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
```
Ändern Sie anschließend die Klassendatei, sodass sie den unten angegebenen Code enthält. Dieser Prozess besteht aus zwei Schritten.

1. Rufen Sie ein Token vom lokalen MSI-Endpunkt auf dem virtuellen Computer ab. Daraufhin wird wiederum ein Token aus Azure Active Directory abgerufen.
2. Übergeben Sie das Token an Key Vault, und rufen Sie Ihr Geheimnis ab. 

```
 class Program
    {
        static void Main(string[] args)
        {
            // Step 1: Get a token from local (URI) Managed Service Identity endpoint which in turn fetches it from Azure Active Directory
            var token = GetToken();

            // Step 2: Fetch the secret value from Key Vault
            System.Console.WriteLine(FetchSecretValueFromKeyVault(token));
        }

        static string GetToken()
        {
            WebRequest request = WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net");
            request.Headers.Add("Metadata", "true");
            WebResponse response = request.GetResponse();
            return ParseWebResponse(response, "access_token");
        }

        static string FetchSecretValueFromKeyVault(string token)
        {
            WebRequest kvRequest = WebRequest.Create("https://<YourVaultName>.vault.azure.net/secrets/<YourSecretName>?api-version=2016-10-01");
            kvRequest.Headers.Add("Authorization", "Bearer "+  token);
            WebResponse kvResponse = kvRequest.GetResponse();
            return ParseWebResponse(kvResponse, "value");
        }

        private static string ParseWebResponse(WebResponse response, string tokenName)
        {
            string token = String.Empty;
            using (Stream stream = response.GetResponseStream())
            {
                StreamReader reader = new StreamReader(stream, Encoding.UTF8);
                String responseString = reader.ReadToEnd();

                JObject joResponse = JObject.Parse(responseString);    
                JValue ojObject = (JValue)joResponse[tokenName];             
                token = ojObject.Value.ToString();
            }
            return token;
        }
    }
```


Der Code oben veranschaulicht, wie Sie mit Azure Key Vault Vorgänge auf einem virtuellen Azure-Windows-Computer ausführen. 



## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Azure Key Vault-REST-API](https://docs.microsoft.com/rest/api/keyvault/)
