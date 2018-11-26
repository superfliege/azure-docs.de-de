---
title: 'Tutorial: Verwenden von Azure Key Vault mit einer Azure-Linux-VM in Python | Microsoft-Dokumentation'
description: 'Tutorial: Konfigurieren einer ASP.NET Core-Anwendung zum Lesen eines Geheimnisses aus Key Vault'
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: key-vault
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: 863f3a8c18eab5b42b5f1377ca0f91f9740c06e7
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52265969"
---
# <a name="tutorial-how-to-use-azure-key-vault-with-azure-linux-virtual-machine-in-python"></a>Tutorial: Verwenden von Azure Key Vault mit einer Azure-Linux-VM in Python

Azure Key Vault unterstützt Sie beim Schutz von Geheimnissen wie API-Schlüsseln und Datenbank-Verbindungszeichenfolgen, die für den Zugriff auf Ihre Anwendungen, Dienste und IT-Ressourcen benötigt werden.

In diesem Tutorial führen Sie die erforderlichen Schritte aus, um eine Azure-Webanwendung zu erhalten und Informationen unter Verwendung von verwalteten Identitäten für Azure-Ressourcen aus Azure Key Vault zu lesen. Dieses Tutorial basiert auf [Azure-Web-Apps](../app-service/app-service-web-overview.md). In diesem Artikel wird Folgendes vermittelt:

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

Als Nächstes ruft Ihr Code einen lokalen, für die Azure-Ressource verfügbaren Metadatendienst auf, auf ein Zugriffstoken zu erhalten.
Ihr Code verwendet das vom lokalen MSI-Endpunkt (MSI_ENDPOINT) erhaltene Zugriffstoken für die Authentifizierung bei einem Azure Key Vault-Dienst. 

## <a name="log-in-to-azure"></a>Anmelden an Azure

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

* Name des Schlüsseltresors: Der Name muss zwischen drei und 24 Zeichen umfassen und darf nur folgende Zeichen enthalten: 0-9, a-z, A-Z und -.
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

## <a name="give-virtual-machine-identity-permission-to-key-vault"></a>Erteilen von Berechtigungen für die VM-Identität für Key Vault
Nun können Sie der oben erstellten Identität Berechtigungen für Key Vault erteilen. Führen Sie dazu den folgenden Befehl aus:

```
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="login-to-the-virtual-machine"></a>Anmelden beim virtuellen Computer

Sie können [dieses Tutorial](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon) absolvieren.

## <a name="create-and-run-sample-python-app"></a>Erstellen und Ausführen der Python-Beispiel-App

Im Folgenden wird eine Datei „Sample.py“ verwendet, bei der es sich lediglich um eine Beispieldatei handelt. Die Datei verwendet die [Anforderungsbibliothek](http://docs.python-requests.org/master/), um HTTP-GET-Aufrufe auszuführen.

## <a name="edit-samplepy"></a>Bearbeiten der Datei „Sample.py“
Nachdem Sie die Datei „Sample.py“ erstellt haben, öffnen Sie sie, und kopieren Sie den folgenden Code.

```
The below is a 2 step process. 
1. Fetch a token from the local MSI endpoint on the VM which in turn fetches a token from Azure Active Directory
2. Pass the token to Key Vault and fetch your secret 
```
    # importing the requests library 
    import requests 

    # Step 1: Fetch an access token from a Managed Identity enabled azure resource      
    # Note that the resource here is https://vault.azure.net for public cloud and api-version is 2018-02-01
    MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
    r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"}) 
      
    # extracting data in json format 
    # This request gets a access_token from Azure Active Directory using the local MSI endpoint
    data = r.json() 
    
    # Step 2: Pass the access_token received from previous HTTP GET call to Key Vault
    KeyVaultURL = "https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01"
    kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})
    
    print(kvSecret.json()["value"])
```

By running you should see the secret value 
```
Python-Datei „Sample.py“
```

The above code shows you how to do operations with Azure Key Vault in an Azure Windows Virtual Machine. 

## Next steps

> [!div class="nextstepaction"]
> [Azure Key Vault REST API](https://docs.microsoft.com/rest/api/keyvault/)
