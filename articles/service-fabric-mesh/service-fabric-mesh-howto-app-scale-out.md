---
title: Skalieren von Diensten in einer Azure Service Fabric Mesh-Anwendung | Microsoft-Dokumentation
description: Weitere Informationen zur unabhängigen Dienstskalierungen in einer Anwendung, die in Service Fabric Mesh mit Azure CLI ausgeführt wird
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
ms.openlocfilehash: a4260fd808643971036ad87c01bd2fdec299ccc6
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39089742"
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
Erstellen Sie eine Ressourcengruppe, in der die Anwendung bereitgestellt werden soll. Sie können eine vorhandene Ressourcengruppe verwenden und diesen Schritt überspringen. 

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-application-with-one-worker-service"></a>Bereitstellen der Anwendung mit einem Workerdienst
Erstellen Sie Ihre Anwendung in der Ressourcengruppe mit dem Befehl `deployment create`.

```azurecli-interactive
az mesh deployment create --resource-group <resourceGroupName> --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
  
```
Der vorherige Befehl stellt eine Linux-Anwendung mit der [mesh_rp.base.linux.json-Vorlage](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json) bereit. Wenn Sie eine Windows-Anwendung bereitstellen möchten, verwenden Sie die [mesh_rp.base.windows.json-Vorlage](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.windows.json). Windows-Containerimages sind größer als Linux-Containerimages und benötigen ggf. mehr Zeit für die Bereitstellung.

In wenigen Minuten sollte der Befehl zurückgeben werden mit:

`visualObjectsApp has been deployed successfully on visualObjectsNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Öffnen der Anwendung
Nachdem die Anwendung erfolgreich bereitgestellt wurde, rufen Sie die öffentliche IP-Adresse für den Dienstendpunkt ab, und öffnen Sie sie in einem Browser. Es wird eine Webseite mit einem sich bewegenden Dreieck angezeigt.

Der Bereitstellungsbefehl gibt die öffentliche IP-Adresse des Dienstendpunkts zurück. Optional können Sie auch die Netzwerkressource abfragen, um die öffentliche IP-Adresse des Dienstendpunkts zu finden. 
 
Der Netzwerkressourcenname für diese Anwendung ist `visualObjectsNetwork`. Informationen dazu können Sie mithilfe des folgenden Befehls abrufen. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name visualObjectsNetwork
```

## <a name="scale-worker-service"></a>Skalieren des Diensts `worker`

Skalieren Sie den Dienst `worker` mit dem folgenden Befehl auf drei Instanzen. 

```azurecli-interactive
az mesh deployment create --resource-group <resourceGroupName> --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
  
```
Der vorherige Befehl stellt eine Linux-Anwendung mit der [mesh_rp.scaleout.linux.json-Vorlage](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json) bereit. Wenn Sie eine Windows-Anwendung bereitstellen möchten, verwenden Sie die [mesh_rp.scaleout.windows.json-Vorlage](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.windows.json). Windows-Containerimages sind größer als Linux-Containerimages und benötigen ggf. mehr Zeit für die Bereitstellung.

Nachdem die Anwendung erfolgreich bereitgestellt wurde, sollte der Browser eine Webseite mit drei sich bewegenden Dreiecken anzeigen.

## <a name="delete-the-resources"></a>Löschen der Ressourcen

Um die begrenzten Ressourcen für das Vorschauprogramm einzusparen, löschen Sie die Ressourcen häufig. Um zu diesem Beispiel gehörende Ressourcen zu löschen, löschen Sie die Ressourcengruppe, in der sie bereitgestellt wurden.

```azurecli-interactive
az group delete --resource-group myResourceGroup 
```

## <a name="next-steps"></a>Nächste Schritte
- Sehen Sie sich die Visual Objects-Beispielanwendung in [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/visualobjects) an.
- Weitere Informationen zum Service Fabric-Ressourcenmodell finden Sie unter [Service Fabric Mesh-Ressourcenmodell](service-fabric-mesh-service-fabric-resources.md).
- In der [Übersicht über Azure Service Fabric Mesh](service-fabric-mesh-overview.md) erfahren Sie mehr über Service Fabric Mesh.