---
title: Bereitstellen einer App aus einer privaten Registrierung in Azure Service Fabric Mesh | Microsoft-Dokumentation
description: Weitere Informationen zum Bereitstellen einer App, die eine private Containerregistrierung in Service Fabric Mesh mit Azure CLI verwendet
services: service-fabric-mesh
documentationcenter: .net
author: rwike77
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/16/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: af92d3c6ea881d00ec687a5560bf4db35aa431c5
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39089485"
---
# <a name="deploy-a-service-fabric-mesh-app-from-a-private-container-image-registry"></a>Bereitstellen einer Service Fabric Mesh-App aus einer privaten Containerimageregistrierung

In diesem Artikel erfahren Sie, wie Sie eine Azure Service Fabric Mesh-App bereitstellen, die eine private Containerimageregistrierung verwendet.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="set-up-service-fabric-mesh-cli"></a>Einrichten der Service Fabric Mesh-CLI 
Sie können Azure Cloud Shell oder eine lokale Installation der Azure CLI für diese Aufgabe verwenden. Installieren Sie das CLI-Erweiterungsmodul von Azure Service Fabric Mesh, indem Sie die folgenden [Anweisungen](service-fabric-mesh-howto-setup-cli.md) befolgen.

### <a name="install-docker"></a>Installieren von Docker

Installieren Sie Docker, um die von Service Fabric Mesh verwendeten Service Fabric-Containeranwendungen zu unterstützen.

### <a name="windows-10"></a>Windows 10

Laden Sie die neueste Version von [Docker Community Edition für Windows][download-docker] herunter, und installieren Sie diese. 

Wählen Sie bei der Installation **Windows-Container anstelle von Linux-Containern verwenden** aus, falls Sie dazu aufgefordert werden. Anschließend müssen Sie sich abmelden und dann erneut anmelden. Wenn Sie Hyper-V zuvor nicht aktiviert haben, werden Sie nach der erneuten Anmeldung möglicherweise aufgefordert, Hyper-V zu aktivieren. Aktivieren Sie Hyper-V und starten Sie den Computer dann neu.

Nach dem Neustart fordert Docker Sie auf, die Funktion **Container** zu aktivieren. Aktivieren Sie sie und starten Sie den Computer neu.

### <a name="windows-server-2016"></a>Windows Server 2016

Verwenden Sie die folgenden PowerShell-Befehle, um Docker zu installieren. Weitere Informationen finden Sie in der [Docker Enterprise Edition für Windows Server][download-docker-server].

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

Starten Sie den Computer neu.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich in Azure an und legen Sie das aktive Abonnement fest:

```azurecli-interactive
az login
az account set --subscription "<subscriptionName>"
```

## <a name="create-a-container-registry-and-push-an-image-to-it"></a>Erstellen einer Containerregistrierung und Pushen von Images in eine Containerregistrierung

Erstellen Sie eine Azure Container Registry mithilfe der Anleitung [Erstellen einer privaten Docker-Registrierung in Azure mit der Azure CLI](../container-registry/container-registry-get-started-azure-cli.md). Führen Sie die Schritte bis zum Punkt [Anmelden bei ACR](../container-registry/container-registry-get-started-azure-cli.md#log-in-to-acr) aus. 

### <a name="push-image-to-acr"></a>Übertragen eines Images an ACR mithilfe von Push

Um ein Image mithilfe von Push an Ihre Azure Container Registry-Instanz übertragen zu können, benötigen Sie zunächst ein Image. Wenn Sie über keine lokalen Containerimages verfügen, führen Sie den folgenden Befehl aus, um ein vorhandenes Image aus dem Docker-Hub abzurufen.

```bash
docker pull seabreeze/azure-mesh-helloworld:1.1-alpine
```

Bevor Sie ein Image mithilfe von Push in Ihre Registrierung übertragen können, müssen Sie es mit dem vollqualifizierten Namen Ihres ACR-Anmeldeservers markieren. Führen Sie den folgenden Befehl aus, um den vollständigen Anmeldeservernamen der ACR-Instanz abzurufen.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Markieren Sie das Image mithilfe des Befehls [docker tag][docker-tag]. Ersetzen Sie `<acrLoginServer>` durch den Anmeldeservernamen Ihrer ACR-Instanz.

```bash
docker tag seabreeze/azure-mesh-helloworld:1.1-alpine <acrLoginServer>/azure-mesh-helloworld:1.1-alpine
```
### <a name="list-container-images"></a>Auflisten von Containerimages

Das folgende Beispiel listet die Repositorys in einer Registrierung auf:

```azurecli
az acr repository list --name <acrName> --output table
```

Ausgabe:

```bash
Result
----------------
azure-mesh-helloworld
```

Das folgende Beispiel listet die Tags des Repositorys **azure-mesh-helloworld** auf.

```azurecli
az acr repository show-tags --name <acrName> --repository azure-mesh-helloworld --output table
```

Ausgabe:

```bash
Result
--------
1.1-alpine
```
Die vorhergehende Ausgabe bestätigt das Vorhandensein von `azure-mesh-helloworld:1.1-alpine` in der privaten Containerregistrierung.

## <a name="retrieve-credentials-for-the-registry"></a>Abrufen von Anmeldeinformationen für die Registrierung

Um eine Containerinstanz aus der erstellten Registrierung bereitzustellen, geben Sie beim Bereitstellen die Anmeldeinformationen an. Aktivieren Sie den Administratorbenutzer in Ihrer Registrierung mit dem folgenden Befehl:

```azurecli-interactive
az acr update --name <acrName> --admin-enabled true
```

Rufen Sie Registrierungsservernamen, Benutzernamen und Kennwort mit den folgenden Befehlen ab:

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
az acr credential show --name <acrName> --query username
az acr credential show --name <acrName> --query "passwords[0].value"
```

Die von vorherigen Befehlen gelieferten Werte werden im folgenden Befehl als `<acrLoginServer>`, `<acrUserName>` und `<acrPassword>` bezeichnet.


## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Erstellen Sie die Anwendung und die zugehörigen Ressourcen mit dem folgenden Befehl, und geben Sie die Anmeldeinformationen aus dem vorherigen Schritt ein.

Der `registry-password`-Parameter in der Vorlage ist ein `securestring`. Er wird nicht im Bereitstellungsstatus und den `az mesh service show`-Befehlen angezeigt. Stellen Sie sicher, dass er im folgenden Befehl ordnungsgemäß angegeben ist.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/helloworld/mesh_rp.private_registry.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}, \"registry-server\": {\"value\": \"<acrLoginServer>\"}, \"registry-username\": {\"value\": \"<acrUserName>\"}, \"registry-password\": {\"value\": \"<acrPassword>\"}}" 
```

In wenigen Minuten sollte der Befehl zurückgeben werden mit:

`helloWorldPrivateRegistryApp has been deployed successfully on helloWorldPrivateRegistryNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Öffnen der Anwendung
Nachdem die Anwendung erfolgreich bereitgestellt wurde, rufen Sie die öffentliche IP-Adresse für den Dienstendpunkt ab, und öffnen Sie sie in einem Browser. Eine Webseite mit dem Service Fabric Mesh-Logo wird angezeigt.

Der Bereitstellungsbefehl gibt die öffentliche IP-Adresse des Dienstendpunkts zurück. Optional können Sie auch die Netzwerkressource abfragen, um die öffentliche IP-Adresse des Dienstendpunkts zu finden. 
 
Der Netzwerkressourcenname für diese Anwendung ist `helloWorldPrivateRegistryNetwork`. Informationen dazu können Sie mithilfe des folgenden Befehls abrufen. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name helloWorldPrivateRegistryNetwork
```

## <a name="delete-the-resources"></a>Löschen der Ressourcen

Um die begrenzten Ressourcen für das Vorschauprogramm einzusparen, löschen Sie die Ressourcen häufig. Um zu diesem Beispiel gehörende Ressourcen zu löschen, löschen Sie die Ressourcengruppe, in der sie bereitgestellt wurden.

```azurecli-interactive
az group delete --resource-group myResourceGroup 
```

## <a name="next-steps"></a>Nächste Schritte
- Sehen Sie sich die Hallo Welt-Beispielanwendung in [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/helloworld) an.
- Weitere Informationen zum Service Fabric-Ressourcenmodell finden Sie unter [Service Fabric Mesh-Ressourcenmodell](service-fabric-mesh-service-fabric-resources.md).
- In der [Übersicht über Azure Service Fabric Mesh](service-fabric-mesh-overview.md) erfahren Sie mehr über Service Fabric Mesh.

[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/