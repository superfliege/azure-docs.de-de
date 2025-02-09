---
title: Verwenden eines hochverfügbaren Service Fabric Reliable Disk-Volumes in einer Azure Service Fabric Mesh-Anwendung | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie durch Einbinden eines auf Service Fabric Reliable Disk basierenden Volumes im Container mit der Azure CLI den Status in der Azure Service Fabric Mesh-Anwendung speichern.
services: service-fabric-mesh
documentationcenter: .net
author: ashishnegi
manager: raunakpandya
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/03/2018
ms.author: asnegi
ms.custom: mvc, devcenter
ms.openlocfilehash: 9f760e7e693334475fb61ba9e5d44df019e78604
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66147477"
---
# <a name="mount-highly-available-service-fabric-reliable-disk-based-volume-in-a-service-fabric-mesh-application"></a>Bereitstellen eines auf Service Fabric Reliable Disk basierenden Volumes in einer Azure Service Fabric Mesh-Anwendung 
Die gängige Methode, den Zustand mit Container-Apps persistent zu speichern, ist die Verwendung von Remotespeicher wie Azure File Storage oder Datenbanken wie Azure Cosmos DB. Dies führt für den Remotespeicher zu erheblicher Netzwerklatenz durch Lese- und Schreibzugriffe.

In diesem Artikel wird gezeigt, wie Sie den Zustand in einem hochverfügbaren Service Fabric Reliable Disk-Volume speichern, indem Sie das Volume in einem Container einer Service Fabric Mesh-Anwendung einbinden.
Service Fabric Reliable Disk stellt Volumes für lokale Lesezugriffe zur Verfügung und repliziert Schreibzugriffe innerhalb des Service Fabric-Clusters für Hochverfügbarkeit. Auf diese Weise entfallen Netzwerkaufrufe für Lesezugriffe, und die Netzwerklatenz für Schreibzugriffe wird verringert. Wenn der Container neu gestartet oder auf einen anderen Knoten verschoben wird, „sieht“ die neue Containerinstanz das gleiche Volume wie die ältere Instanz. Daher ist sie effizient und hochverfügbar.

In diesem Beispiel verfügt die Zähleranwendung über einen ASP.NET Core-Dienst mit einer Webseite, die Zählerwerte in einem Browser anzeigt.

`counterService` liest regelmäßig einen Zählerwert aus einer Datei aus, erhöht diesen und schreibt ihn wieder in die Datei. Die Datei wird in einem Ordner gespeichert, der auf dem durch Service Fabric Reliable Disk unterstützten Volume eingebunden ist.

## <a name="prerequisites"></a>Voraussetzungen

Sie können Azure Cloud Shell oder eine lokale Installation der Azure CLI für diese Aufgabe verwenden. Wenn Sie die Azure CLI für diesen Artikel verwenden möchten, muss `az --version` mindestens `azure-cli (2.0.43)` zurückgeben.  Installieren (oder aktualisieren) Sie das CLI-Erweiterungsmodul von Azure Service Fabric Mesh, indem Sie [diese Anweisungen](service-fabric-mesh-howto-setup-cli.md) befolgen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich in Azure an, und legen Sie Ihr Abonnement fest.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie eine Ressourcengruppe, in der die Anwendung bereitgestellt werden soll. Der folgende Befehl erstellt eine Ressourcengruppe namens `myResourceGroup` an einem Standort im Osten der USA. Wenn Sie den Namen der Ressourcengruppe im folgenden Befehl ändern, denken Sie daran, ihn auch in allen folgenden Befehlen zu ändern.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Der folgende Befehl stellt eine Linux-Anwendung mit der Vorlage [counter.sfreliablevolume.linux.json](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/counter/counter.sfreliablevolume.linux.json) bereit. Zum Bereitstellen einer Windows-Anwendung verwenden Sie die Vorlage [counter.sfreliablevolume.windows.json](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/counter/counter.sfreliablevolume.windows.json). Beachten Sie, dass die Bereitstellung größerer Containerimages länger dauern kann.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://raw.githubusercontent.com/Azure-Samples/service-fabric-mesh/master/templates/counter/counter.sfreliablevolume.linux.json
```

Sie können mit dem Befehl auch den Zustand der Bereitstellung anzeigen.

```azurecli-interactive
az group deployment show --name counter.sfreliablevolume.linux --resource-group myResourceGroup
```

Beachten Sie den Namen der Gatewayressource, die den Ressourcentyp `Microsoft.ServiceFabricMesh/gateways` aufweist. Dieser wird beim Abrufen der öffentlichen IP-Adresse der App verwendet.

## <a name="open-the-application"></a>Öffnen der Anwendung

Sobald die Anwendung erfolgreich bereitgestellt wurde, rufen Sie die IP-Adresse der Gatewayressource für die App ab. Verwenden Sie den Gatewaynamen, der im Abschnitt oben genannt wurde.
```azurecli-interactive
az mesh gateway show --resource-group myResourceGroup --name counterGateway
```

Die Ausgabe sollte über eine Eigenschaft `ipAddress` verfügen, die die öffentliche IP-Adresse für den Dienstendpunkt darstellt. Öffnen Sie diese in einem Browser. Es wird eine Webseite mit dem Zählerwert angezeigt, der jede Sekunde aktualisiert wird.

## <a name="verify-that-the-application-is-able-to-use-the-volume"></a>Sicherstellen, dass die Anwendung das Volume verwenden kann

Die Anwendung erstellt eine Datei namens `counter.txt` im Volume im Ordner `counter/counterService`. Der Inhalt dieser Datei ist der Zählerwert, der auf der Webseite angezeigt wird.

## <a name="delete-the-resources"></a>Löschen der Ressourcen

Löschen Sie immer wieder die Ressourcen in Azure, die Sie nicht mehr verwenden. Wenn Sie die Ressourcen für dieses Beispiel löschen möchten, entfernen Sie mit dem folgenden Befehl die Ressourcengruppe, in der sie bereitgestellt wurden. (Dadurch werden alle mit der Ressourcengruppe verknüpften Ressourcen gelöscht.)

```azurecli-interactive
az group delete --resource-group myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

- Anzeigen der Service Fabric Reliable Volume-Beispielanwendung auf [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter).
- Weitere Informationen zum Service Fabric-Ressourcenmodell finden Sie unter [Service Fabric Mesh-Ressourcenmodell](service-fabric-mesh-service-fabric-resources.md).
- In der [Übersicht über Azure Service Fabric Mesh](service-fabric-mesh-overview.md) erfahren Sie mehr über Service Fabric Mesh.
