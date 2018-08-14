---
title: Bereitstellen einer App in Azure Service Fabric Mesh mithilfe einer Vorlage | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine .NET Core-Anwendung über eine Vorlage mithilfe der Azure-Befehlszeilenschnittstelle in Service Fabric Mesh bereitstellen.
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
ms.date: 07/12/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: 356e8019f9a4a64cb1c1c113d0f44990aa4e0f95
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2018
ms.locfileid: "40038250"
---
# <a name="deploy-a-service-fabric-mesh-application-to-service-fabric-mesh-using-a-template"></a>Bereitstellen einer Service Fabric Mesh-Anwendung in Service Fabric Mesh mithilfe einer Vorlage
In diesem Artikel wird beschrieben, wie Sie eine .NET Core-Anwendung über eine Vorlage in Service Fabric Mesh bereitstellen. Am Ende verfügen Sie über eine Abstimmungsanwendung mit einem ASP.NET Core-Web-Front-End, mit der Abstimmungsergebnisse im Cluster in einem Back-End-Dienst gespeichert werden. Das Front-End verwendet DNS zum Auflösen der Adresse des Back-End-Dienstes.

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

## <a name="deploy-the-application"></a>Bereitstellen der Anwendung
Erstellen Sie Ihre Anwendung in der Ressourcengruppe mit dem Befehl `deployment create`.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/voting/mesh_rp.windows.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
```

Der vorherige Befehl stellt eine Windows-Anwendung mit einer [mesh_rp.windows.json-Vorlage](https://sfmeshsamples.blob.core.windows.net/templates/voting/mesh_rp.windows.json) bereit. Wenn Sie eine Linux-Anwendung bereitstellen möchten, verwenden Sie die [mesh_rp.linux.json-Vorlage](https://sfmeshsamples.blob.core.windows.net/templates/voting/mesh_rp.linux.json). Windows-Containerimages sind größer als Linux-Containerimages und benötigen ggf. mehr Zeit für die Bereitstellung.

In wenigen Minuten sollte der Befehl zurückgeben werden mit:

`VotingApp has been deployed successfully on VotingAppNetwork with public ip address <IP address>` 

## <a name="open-the-application"></a>Öffnen der Anwendung
Nachdem die Anwendung erfolgreich bereitgestellt wurde, rufen Sie die öffentliche IP-Adresse für den Dienstendpunkt ab, und öffnen Sie sie in einem Browser. Die folgende Webseite wird angezeigt. 

![Voting-Anwendung](./media/service-fabric-mesh-howto-deploy-app-template/VotingApplication.png)

Sie können jetzt Abstimmungsoptionen zur Anwendung hinzufügen und dafür abstimmen, oder Sie können die Abstimmungsoptionen löschen.

Der Bereitstellungsbefehl gibt die öffentliche IP-Adresse des Dienstendpunkts zurück. Optional können Sie auch die Netzwerkressource abfragen, um die öffentliche IP-Adresse des Dienstendpunkts zu finden. 

Der Netzwerkname für die Ressource für diese Anwendung ist `VotingAppNetwork`. Informationen dazu können Sie mithilfe des folgenden Befehls abrufen. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name VotingAppNetwork
```

## <a name="check-the-application-details"></a>Überprüfen der Anwendungsdetails
Sie können den Status der Anwendung mit dem Befehl `app show` überprüfen. Der Anwendungsname für die bereitgestellte Anwendung ist „VotingApp“. Sie können deren Details abrufen. 

```azurecli-interactive
az mesh app show --resource-group myResourceGroup --name VotingApp
```

## <a name="list-the-deployed-applications"></a>Liste der bereitgestellten Anwendungen
Sie können den Befehl „app list“ verwenden, um eine Liste der Anwendungen abzurufen, die Sie in Ihrem Abonnement bereitgestellt haben. 

```azurecli-interactive
az mesh app list -o table
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie die Anwendung und die zugehörigen Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, die sie enthält. 

```azurecli-interactive
az group delete --resource-group myResourceGroup  
``` 

## <a name="next-steps"></a>Nächste Schritte
- Schauen Sie sich die Beispielanwendung für die Abstimmung in [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp) an.
- Lesen Sie die [Übersicht über Service Fabric Mesh](service-fabric-mesh-overview.md), um mehr zu Service Fabric Mesh zu erfahren.


