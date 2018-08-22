---
title: Skalieren von Diensten in einer Azure Service Fabric Mesh-Anwendung | Microsoft-Dokumentation
description: Weitere Informationen zur unabhängigen Dienstskalierungen in einer Anwendung, die in Service Fabric Mesh mit Azure CLI ausgeführt wird
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
ms.date: 08/08/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: e68bcd135c33c7fd83908b8fed0fd098a698fd36
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2018
ms.locfileid: "40038274"
---
# <a name="scale-services-within-an-application-running-on-service-fabric-mesh"></a>Skalieren von Diensten in einer Service Fabric Mesh-Anwendung

In diesem Artikel erfahren Sie, wie Sie Microservices unabhängig voneinander in einer Anwendung skalieren können. In diesem Beispiel besteht die Visual Objects-Anwendung aus zwei Microservices: `web` und `worker`.

Der Dienst `web` ist eine ASP.NET Core-Anwendung mit einer Webseite, die Dreiecke im Browser anzeigt. Der Browser zeigt für jede Instanz des Diensts `worker` ein Dreieck an.

Der Dienst `worker` verschiebt das Dreieck in einem vordefinierten Intervall und sendet die Position des Dreiecks an den Dienst `web`. Er verwendet DNS, um die Adresse von Dienst `web` aufzulösen.

## <a name="set-up-service-fabric-mesh-cli"></a>Einrichten der Service Fabric Mesh-CLI

Sie können Azure Cloud Shell oder eine lokale Installation der Azure CLI für diese Aufgabe verwenden. Installieren Sie das CLI-Erweiterungsmodul von Azure Service Fabric Mesh, indem Sie die folgenden [Anweisungen](service-fabric-mesh-howto-setup-cli.md) befolgen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich in Azure an, und legen Sie Ihr Abonnement fest.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie eine Ressourcengruppe, in der die Anwendung bereitgestellt werden soll.

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-application-with-one-worker-service"></a>Bereitstellen der Anwendung mit einem Workerdienst

Erstellen Sie Ihre Anwendung in der Ressourcengruppe mit dem Befehl `deployment create`.

Das folgende Beispiel stellt eine Linux-Anwendung mit der Vorlage [mesh_rp.base.linux.json](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json) bereit. Zum Bereitstellen einer Windows-Anwendung verwenden Sie die Vorlage [mesh_rp.base.windows.json](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.windows.json). Windows-Containerimages sind größer als Linux-Containerimages und benötigen ggf. mehr Zeit für die Bereitstellung.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
```

In wenigen Minuten sollte der Befehl Folgendes zurückgeben:

`visualObjectsApp has been deployed successfully on visualObjectsNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Öffnen der Anwendung

Der Bereitstellungsbefehl gibt die öffentliche IP-Adresse des Dienstendpunkts zurück. Nachdem die Anwendung erfolgreich bereitgestellt wurde, rufen Sie die öffentliche IP-Adresse für den Dienstendpunkt ab, und öffnen Sie sie in einem Browser. Eine Webseite mit einem sich bewegenden Dreieck wird angezeigt.

Der Name der Netzwerkressource für diese Anwendung lautet `visualObjectsNetwork`. Mit dem folgenden Befehl können Sie Informationen zur App anzeigen, etwa ihre Beschreibung, den Standort und die Ressourcengruppe:

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name visualObjectsNetwork
```

## <a name="scale-worker-service"></a>Skalieren des Diensts `worker`

Skalieren Sie den Dienst `worker` mit dem folgenden Befehl auf drei Instanzen. Das folgende Beispiel stellt eine Linux-Anwendung mit der Vorlage [mesh_rp.scaleout.linux.json](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json) bereit. Wenn Sie eine Windows-Anwendung bereitstellen möchten, verwenden Sie die Vorlage [mesh_rp.scaleout.windows.json](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.windows.json). Beachten Sie, dass die Bereitstellung größerer Containerimages länger dauern kann.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
  
```

Nachdem die Anwendung erfolgreich bereitgestellt wurde, sollte der Browser eine Webseite mit drei sich bewegenden Dreiecken anzeigen.

## <a name="delete-the-resources"></a>Löschen der Ressourcen

Löschen Sie immer wieder die Ressourcen in Azure, die Sie nicht mehr verwenden. Wenn Sie die Ressourcen für dieses Beispiel löschen möchten, entfernen Sie mit dem folgenden Befehl die Ressourcengruppe, in der sie bereitgestellt wurden. (Dadurch werden alle mit der Ressourcengruppe verknüpften Ressourcen gelöscht.)

```azurecli-interactive
az group delete --resource-group myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die Visual Objects-Beispielanwendung in [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/visualobjects) an.
- Weitere Informationen zum Service Fabric-Ressourcenmodell finden Sie unter [Service Fabric Mesh-Ressourcenmodell](service-fabric-mesh-service-fabric-resources.md).
- In der [Übersicht über Azure Service Fabric Mesh](service-fabric-mesh-overview.md) erfahren Sie mehr über Service Fabric Mesh.