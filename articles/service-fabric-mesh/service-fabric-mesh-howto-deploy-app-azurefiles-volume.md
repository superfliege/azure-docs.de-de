---
title: Speichern des Status in einer Azure Service Fabric Mesh-Anwendung durch Einbinden eines auf Azure Files basierenden Volumes im Container | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle und durch Einbinden eines auf Azure Files basierenden Volumes im Container den Status in der Azure Service Fabric Mesh-Anwendung speichern.
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
ms.date: 08/09/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: cb5b421c1bcfe888d65335f3ab7f67bed80eec34
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51614258"
---
# <a name="store-state-in-an-azure-service-fabric-mesh-application-by-mounting-an-azure-files-based-volume-inside-the-container"></a>Speichern des Status in einer Azure Service Fabric Mesh-Anwendung durch Einbinden eines auf Azure Files basierenden Volumes im Container

In diesem Artikel zeigt, wie Sie den Status in Azure Files speichern, indem Sie ein Volume in einem Container einer Service Fabric Mesh-Anwendung einbinden. In diesem Beispiel verfügt die Zähleranwendung über einen ASP.NET Core-Dienst mit einer Webseite, die Zählerwerte in einem Browser anzeigt. 

`counterService` liest regelmäßig einen Zählerwert aus einer Datei aus, erhöht diesen und schreibt ihn wieder in die Datei. Die Datei wird in einem Ordner gespeichert, der auf dem von der Azure Files-Freigabe unterstützten Volume eingebunden ist.

## <a name="prerequisites"></a>Voraussetzungen

Sie können Azure Cloud Shell oder eine lokale Installation der Azure CLI für diese Aufgabe verwenden. Wenn Sie die Azure CLI für diesen Artikel verwenden möchten, muss `az --version` mindestens `azure-cli (2.0.43)` zurückgeben.  Installieren (oder aktualisieren) Sie das CLI-Erweiterungsmodul von Azure Service Fabric Mesh, indem Sie [diese Anweisungen](service-fabric-mesh-howto-setup-cli.md) befolgen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich in Azure an, und legen Sie Ihr Abonnement fest.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-a-file-share"></a>Erstellen einer Dateifreigabe

Erstellen Sie eine Azure-Dateifreigabe anhand dieser [Anweisungen](/azure/storage/files/storage-how-to-create-file-share). Der Name des Speicherkontos, der Speicherkontoschlüssel und der Dateifreigabename werden in den folgenden Anweisungen als `<storageAccountName>`, `<storageAccountKey>` und `<fileShareName>` bezeichnet. Diese Werte sind in Ihrem Azure-Portal verfügbar:
* <storageAccountName> Unter **Speicherkonten** ist der Name des Speicherkontos angegeben, das Sie beim Erstellen der Dateifreigabe verwendet haben.
* <storageAccountKey> Wählen Sie unter **Speicherkonten** Ihr Speicherkonto aus, klicken Sie auf **Zugriffsschlüssel**, und verwenden Sie den Wert unter **key1**.
* <fileShareName> Wählen Sie unter **Speicherkonten** Ihr Speicherkonto aus, und klicken Sie dann auf **Dateien**. Als Name muss der Name der Dateifreigabe verwendet werden, die Sie gerade erstellt haben.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie eine Ressourcengruppe, in der die Anwendung bereitgestellt werden soll. Der folgende Befehl erstellt eine Ressourcengruppe namens `myResourceGroup` an einem Standort im Osten der USA.

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Erstellen Sie die Anwendung und die zugehörigen Ressourcen mit dem folgenden Befehl, und geben Sie die Werte für `storageAccountName`, `storageAccountKey` und `fileShareName` aus dem vorherigen Schritt [Erstellen einer Dateifreigabe](#create-a-file-share) ein.

Der `storageAccountKey`-Parameter in der Vorlage ist eine sichere Zeichenfolge. Er wird nicht im Bereitstellungsstatus und den `az mesh service show`-Befehlen angezeigt. Stellen Sie sicher, dass er im folgenden Befehl ordnungsgemäß angegeben ist.

Der folgende Befehl stellt eine Linux-Anwendung mit der Vorlage [counter.azurefilesvolume.linux.json](https://sfmeshsamples.blob.core.windows.net/templates/counter/counter.azurefilesvolume.linux.json) bereit. Zum Bereitstellen einer Windows-Anwendung verwenden Sie die Vorlage [counter.azurefilesvolume.windows.json](https://sfmeshsamples.blob.core.windows.net/templates/counter/counter.azurefilesvolume.windows.json). Beachten Sie, dass die Bereitstellung größerer Containerimages länger dauern kann.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/counter/counter.azurefilesvolume.linux.json  --parameters "{\"location\": {\"value\": \"eastus\"}, \"fileShareName\": {\"value\": \"<fileShareName>\"}, \"storageAccountName\": {\"value\": \"<storageAccountName>\"}, \"storageAccountKey\": {\"value\": \"<storageAccountKey>\"}}"
```

In wenigen Minuten sollte der Befehl Folgendes zurückgeben: `counterApp has been deployed successfully on counterAppNetwork with public ip address <IP Address>`

## <a name="open-the-application"></a>Öffnen der Anwendung

Der Bereitstellungsbefehl gibt die öffentliche IP-Adresse des Dienstendpunkts zurück. Nachdem die Anwendung erfolgreich bereitgestellt wurde, rufen Sie die öffentliche IP-Adresse für den Dienstendpunkt ab, und öffnen Sie sie in einem Browser. Es wird eine Webseite mit dem Zählerwert angezeigt, der jede Sekunde aktualisiert wird.

Der Name der Netzwerkressource für diese Anwendung lautet `counterAppNetwork`. Mit dem folgenden Befehl können Sie Informationen zur App anzeigen, etwa ihre Beschreibung, den Standort und die Ressourcengruppe:

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name counterAppNetwork
```

## <a name="verify-that-the-application-is-able-to-use-the-volume"></a>Sicherstellen, dass die Anwendung das Volume verwenden kann

Die Anwendung erstellt eine Datei mit der Bezeichnung `counter.txt` in der Dateifreigabe im `counter/counterService`-Ordner. Der Inhalt dieser Datei ist der Zählerwert, der auf der Webseite angezeigt wird.

Die Datei kann mit einem beliebigen Tool heruntergeladen werden, mit dem eine Azure Files-Dateifreigabe durchsucht werden kann, etwa dem [Microsoft Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/).

## <a name="delete-the-resources"></a>Löschen der Ressourcen

Löschen Sie immer wieder die Ressourcen in Azure, die Sie nicht mehr verwenden. Wenn Sie die Ressourcen für dieses Beispiel löschen möchten, entfernen Sie mit dem folgenden Befehl die Ressourcengruppe, in der sie bereitgestellt wurden. (Dadurch werden alle mit der Ressourcengruppe verknüpften Ressourcen gelöscht.)

```azurecli-interactive
az group delete --resource-group myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

- Schauen Sie sich die Azure Files-Volume-Beispielanwendung in [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter) an.
- Weitere Informationen zum Service Fabric-Ressourcenmodell finden Sie unter [Service Fabric Mesh-Ressourcenmodell](service-fabric-mesh-service-fabric-resources.md).
- In der [Übersicht über Azure Service Fabric Mesh](service-fabric-mesh-overview.md) erfahren Sie mehr über Service Fabric Mesh.