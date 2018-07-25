---
title: Speichern des Status durch Einbinden eines auf Azure Files basierenden Volume in einem Container in der Service Fabric Mesh-Anwendung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Status durch Einbinden eines auf Azure Files basierenden Volume in einem Container in der Service Fabric Mesh-Anwendung mithilfe der Azure-Befehlszeilenschnittstelle speichern.
services: service-fabric-mesh
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/26/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: 94a4e17e6285893520a2f6482b32a69b1229e2fa
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090631"
---
# <a name="store-state-by-mounting-azure-files-based-volume-in-service-fabric-mesh-application"></a>Speichern des Status durch Einbinden eines auf Azure Files basierenden Volume in der Service Fabric Mesh-Anwendung

In diesem Artikel zeigt, wie Sie den Status in Azure Files speichern, indem Sie ein Volume in einem Container einer Service Fabric Mesh-Anwendung einbinden. In diesem Beispiel verfügt die Zähleranwendung über einen ASP.NET Core-Dienst mit einer Webseite, die Zählerwerte in einem Browser anzeigt. 

Der `counterService` liest regelmäßig einen Zählerwert aus einer Datei aus, erhöht diesen, und schreibt ihn wieder in die Datei. Die Datei wird in einem Ordner gespeichert, der auf dem von der Azure Files-Freigabe unterstützten Volume eingebunden ist. 

## <a name="set-up-service-fabric-mesh-cli"></a>Einrichten der Service Fabric Mesh-CLI 
Sie können Azure Cloud Shell oder eine lokale Installation der Azure CLI für diese Aufgabe verwenden. Installieren Sie das CLI-Erweiterungsmodul von Azure Service Fabric Mesh, indem Sie die folgenden [Anweisungen](service-fabric-mesh-howto-setup-cli.md) befolgen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure
Melden Sie sich bei Azure an, und legen Sie Ihr Abonnement fest.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-file-share"></a>Erstellen einer Dateifreigabe 
Erstellen Sie eine Azure-Dateifreigabe anhand dieser [Anweisungen](/azure/storage/files/storage-how-to-create-file-share). Der Name des Speicherkontos, der Speicherkontoschlüssel und der Dateinamen für die Freigabe werden in den folgenden Anweisungen als `<storageAccountName>`, `<storageAccountKey>` und `<fileShareName>` bezeichnet.

## <a name="create-resource-group"></a>Ressourcengruppe erstellen
Erstellen Sie eine Ressourcengruppe, in der die Anwendung bereitgestellt werden soll. Sie können eine vorhandene Ressourcengruppe verwenden und diesen Schritt überspringen. 

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Erstellen Sie die Anwendung und die zugehörigen Ressourcen mit dem folgenden Befehl und geben Sie die Werte für `storageAccountName`, `storageAccountKey` und `fileShareName` aus dem vorherigen Schritt ein.

Der `storageAccountKey`-Parameter in der Vorlage ist ein `securestring`. Er wird nicht im Bereitstellungsstatus und den `az mesh service show`-Befehlen angezeigt. Stellen Sie sicher, dass er im folgenden Befehl ordnungsgemäß angegeben ist.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/counter/mesh_rp.linux.json  --parameters "{\"location\": {\"value\": \"eastus\"}, \"fileShareName\": {\"value\": \"<fileShareName>\"}, \"storageAccountName\": {\"value\": \"<storageAccountName>\"}, \"storageAccountKey\": {\"value\": \"<storageAccountKey>\"}}"
```

Der vorherige Befehl stellt eine Linux-Anwendung mit der [mesh_rp.linux.json-Vorlage](https://sfmeshsamples.blob.core.windows.net/templates/counter/mesh_rp.linux.json) bereit. Wenn Sie eine Windows-Anwendung bereitstellen möchten, verwenden Sie die [mesh_rp.windows.json-Vorlage](https://sfmeshsamples.blob.core.windows.net/templates/counter/mesh_rp.windows.json). Windows-Containerimages sind größer als Linux-Containerimages und benötigen ggf. mehr Zeit für die Bereitstellung.

In wenigen Minuten sollte der Befehl zurückgeben werden mit:

`counterApp has been deployed successfully on counterAppNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Öffnen der Anwendung
Nachdem die Anwendung erfolgreich bereitgestellt wurde, rufen Sie die öffentliche IP-Adresse für den Dienstendpunkt ab, und öffnen Sie sie in einem Browser. Es wird eine Webseite mit dem Zählerwert angezeigt, der jede Sekunde aktualisiert wird.

Der Bereitstellungsbefehl gibt die öffentliche IP-Adresse des Dienstendpunkts zurück. Optional können Sie auch die Netzwerkressource abfragen, um die öffentliche IP-Adresse des Dienstendpunkts zu finden. 
 
Der Netzwerkname für die Ressource für diese Anwendung ist `counterAppNetwork`. Informationen dazu können Sie mithilfe des folgenden Befehls abrufen. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name counterAppNetwork
```

## <a name="verify-that-the-application-is-able-to-use-the-volume"></a>Sicherstellen, dass die Anwendung das Volume verwenden kann
Die Anwendung erstellt eine Datei mit der Bezeichnung `counter.txt` in der Dateifreigabe im `counter/counterService`-Ordner. Der Inhalt dieser Datei ist der Zählerwert, der auf der Webseite angezeigt wird.

Die Datei kann mit einem beliebigen Tool heruntergeladen werden, mit dem eine Azure Files-Dateifreigabe durchsucht werden kann. Ein Beispiel für ein solches Tool ist der [Microsoft Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/).

## <a name="delete-the-resources"></a>Löschen der Ressourcen

Um die begrenzten Ressourcen für das Vorschauprogramm zu schonen, löschen Sie die Ressourcen häufig. Um zu diesem Beispiel gehörende Ressourcen zu löschen, löschen Sie die Ressourcengruppe, in der sie bereitgestellt wurden.

```azurecli-interactive
az group delete --resource-group myResourceGroup 
```

## <a name="next-steps"></a>Nächste Schritte

- Schauen Sie sich die Azure Files-Volume-Beispielanwendung in [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter) an.
- Weitere Informationen zum Service Fabric-Ressourcenmodell finden Sie unter [Service Fabric Mesh-Ressourcenmodell](service-fabric-mesh-service-fabric-resources.md).
- Lesen Sie die [Übersicht über Service Fabric Mesh](service-fabric-mesh-overview.md), um mehr zu Service Fabric Mesh zu erfahren.
