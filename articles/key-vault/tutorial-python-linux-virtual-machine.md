---
title: 'Tutorial: Verwenden von Azure Key Vault mit einem virtuellen Azure-Computer in Python | Microsoft-Dokumentation'
description: In diesem Tutorial konfigurieren Sie eine Python-Anwendung zum Lesen eines Geheimnisses aus einem Schlüsseltresor.
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
ms.openlocfilehash: 1e364003093d5e37a75830386cafe855b0bdcad2
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54467400"
---
# <a name="tutorial-use-azure-key-vault-with-an-azure-virtual-machine-in-python"></a>Tutorial: Tutorial: Verwenden von Azure Key Vault mit einem virtuellen Azure-Computer in Python

Azure Key Vault unterstützt Sie beim Schutz von Geheimnissen wie den API-Schlüsseln und Datenbank-Verbindungszeichenfolgen, die für den Zugriff auf Ihre Anwendungen, Dienste und IT-Ressourcen benötigt werden.

In diesem Tutorial führen Sie die Schritte aus, um eine Azure-Webanwendung zu erhalten, die unter Verwendung von verwalteten Identitäten für Azure-Ressourcen Informationen aus Azure Key Vault liest. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines Schlüsseltresors
> * Speichern eines Geheimnisses im Schlüsseltresor
> * Erstellen Sie einen virtuellen Azure-Computer.
> * Aktivieren Sie eine [verwaltete Identität](../active-directory/managed-identities-azure-resources/overview.md) für den virtuellen Computer.
> * Erteilen der erforderlichen Berechtigungen zum Lesen von Daten aus dem Schlüsseltresor für die Konsolenanwendung
> * Abrufen eines Geheimnisses aus dem Schlüsseltresor

Lesen Sie die Informationen zu [grundlegenden Konzepten zu Key Vault](key-vault-whatis.md#basic-concepts), bevor Sie fortfahren.

## <a name="prerequisites"></a>Voraussetzungen
Für alle Plattformen wird Folgendes benötigt:

* Git ([Download](https://git-scm.com/downloads))
* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
* [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), Version 2.0.4 oder höher. Diese ist für Windows, Mac und Linux verfügbar.

### <a name="managed-service-identity-and-how-it-works"></a>Verwaltete Dienstidentität und ihre Funktionsweise
In diesem Tutorial wird die verwaltete Dienstidentität (MSI) verwendet.

Azure Key Vault dient zum sicheren Speichern von Anmeldeinformationen, damit sich diese nicht in Ihrem Code befinden. Um diese abzurufen, müssen Sie sich bei Key Vault authentifizieren. Und für die Authentifizierung bei Key Vault benötigen Sie wiederum Anmeldeinformationen. Dies ist ein klassisches Bootstrap-Problem. MSI stellt über Azure und Azure Active Directory (Azure AD) eine „Bootstrap-Identität“ bereit, die die ersten Schritte erheblich vereinfacht.

Wenn Sie MSI für einen Azure-Dienst aktivieren (beispielsweise Virtual Machines, App Service oder Functions), erstellt Azure einen [Dienstprinzipal](key-vault-whatis.md#basic-concepts) für die Instanz des Diensts in Azure AD. Azure fügt die Anmeldeinformationen für den Dienstprinzipal in der Instanz des Diensts ein. 

![MSI](media/MSI.png)

Als Nächstes ruft Ihr Code einen lokalen, für die Azure-Ressource verfügbaren Metadatendienst auf, um ein Zugriffstoken zu erhalten.
Ihr Code verwendet das vom lokalen MSI-Endpunkt erhaltene Zugriffstoken für die Authentifizierung bei einem Azure Key Vault-Dienst. 

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

* Name des Schlüsseltresors: Der Name muss eine Zeichenfolge aus 3 bis 24 Zeichen sein und darf nur die Zeichen 0–9, a–z, A–Z und Bindestrich (-) enthalten.
* Ressourcengruppenname
* Standort: **USA, Westen**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
An diesem Punkt ist nur Ihr Azure-Konto zum Ausführen von Vorgängen für den neuen Tresor autorisiert.

## <a name="add-a-secret-to-the-key-vault"></a>Hinzufügen eines Geheimnisses zum Schlüsseltresor

Wir fügen ein Geheimnis hinzu, um die Vorgehensweise zu veranschaulichen. Sie können eine SQL-Verbindungszeichenfolge oder beliebige andere Informationen speichern, die sicher aufbewahrt, aber für Ihre Anwendung verfügbar gemacht werden müssen.

Geben Sie die folgenden Befehle ein, um im Schlüsseltresor ein Geheimnis namens *AppSecret* zu erstellen. Dieses Geheimnis speichert den Wert **MySecret**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

Erstellen Sie mit dem Befehl [az vm create](/cli/azure/vm) eine VM.

Im folgenden Beispiel wird eine VM mit dem Namen *myVM* erstellt und ein Benutzerkonto mit dem Namen *azureuser* hinzugefügt. Der Parameter `--generate-ssh-keys` generiert automatisch einen SSH-Schlüssel und legt ihn am Speicherort für den Standardschlüssel ab (*~/.ssh*). Um stattdessen einen bestimmten Satz mit Schlüsseln zu verwenden, können Sie die Option `--ssh-key-value` nutzen.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

Das Erstellen des virtuellen Computers und der unterstützenden Ressourcen dauert einige Minuten. In der folgenden Beispielausgabe wird angezeigt, dass die Erstellung des virtuellen Computers erfolgreich war:

```
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-00-00-00-00-00",
  "powerState": "VM running",
  "privateIpAddress": "XX.XX.XX.XX",
  "publicIpAddress": "XX.XX.XXX.XXX",
  "resourceGroup": "myResourceGroup"
}
```

Beachten Sie Ihren eigenen `publicIpAddress`-Wert in der Ausgabe Ihres virtuellen Computers. Diese Adresse wird in den nächsten Schritten verwendet, um auf den virtuellen Computer zuzugreifen.

## <a name="assign-an-identity-to-the-virtual-machine"></a>Zuweisen einer Identität zum virtuellen Computer
In diesem Schritt erstellen wir eine vom System zugewiesene Identität für den virtuellen Computer. Führen Sie in der Azure CLI den folgenden Befehl aus:

```
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Die Ausgabe des Befehls lautet: Beachten Sie den Wert von **systemAssignedIdentity**. 

```
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="give-the-virtual-machine-identity-permission-to-the-key-vault"></a>Dem Schlüsseltresor Berechtigungen für die Identität des virtuellen Computers erteilen
Jetzt können wir dem Schlüsseltresor die Identitätsberechtigung erteilen. Führen Sie den folgenden Befehl aus:

```
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-in-to-the-virtual-machine"></a>Melden Sie sich beim virtuellen Computer an.

Melden Sie sich beim virtuellen Computer an, indem Sie [dieses Tutorial](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon) durchführen.

## <a name="create-and-run-the-sample-python-app"></a>Erstellen und Ausführen der Python-Beispiel-App

Die folgende Beispieldatei heißt *Sample.py*. Die Datei verwendet die [Anforderungsbibliothek](https://pypi.org/project/requests/2.7.0/), um HTTP-GET-Aufrufe auszuführen.

## <a name="edit-samplepy"></a>Bearbeiten der Datei „Sample.py“
Nachdem Sie „Sample.py“ erstellt haben, öffnen Sie die Datei, und kopieren Sie den folgenden Code. Der Code ist ein zweistufiger Prozess: 
1. Rufen Sie ein Token vom lokalen MSI-Endpunkt auf dem virtuellen Computer ab. Der Endpunkt ruft dann ein Token aus Azure Active Directory ab.
2. Übergeben Sie das Token an den Schlüsseltresor, und rufen Sie Ihr Geheimnis ab. 

```
    # importing the requests library 
    import requests 

    # Step 1: Fetch an access token from an MSI-enabled Azure resource      
    # Note that the resource here is https://vault.azure.net for the public cloud, and api-version is 2018-02-01
    MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
    r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"}) 
      
    # Extracting data in JSON format 
    # This request gets an access token from Azure Active Directory by using the local MSI endpoint
    data = r.json() 
    
    # Step 2: Pass the access token received from the previous HTTP GET call to the key vault
    KeyVaultURL = "https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01"
    kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})
    
    print(kvSecret.json()["value"])
```

Durch die Ausführung des folgenden Befehls sollte der Wert des Geheimnisses angezeigt werden: 

```
python Sample.py
```

Der Code oben veranschaulicht, wie Sie mit Azure Key Vault Vorgänge auf einem virtuellen Windows-Computer ausführen. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Azure Key Vault-REST-API](https://docs.microsoft.com/rest/api/keyvault/)
