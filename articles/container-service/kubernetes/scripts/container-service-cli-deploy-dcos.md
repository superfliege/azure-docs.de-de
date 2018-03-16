---
title: 'Azure CLI-Skriptbeispiel: Erstellen eines ACS-DC/OS-Clusters | Microsoft Dokumentation'
description: 'Azure CLI-Skriptbeispiel: Erstellen eines ACS-DC/OS-Clusters'
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Kubernetes, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2017
ms.author: nepeters
ms.openlocfilehash: 1d08dc6b1b39a6908ce5851b5f57756701fc39eb
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="create-an-azure-container-service-dcos-cluster"></a>Erstellen eines Azure Container Service-DC/OS-Clusters

In diesem Beispiel wird ein Azure Container Service-Cluster erstellt, der DCOS ausführt.

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

```azurecli
az group create --name myResourceGroup --location eastus

az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen der Bereitstellung. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az acs create](https://docs.microsoft.com/cli/azure/acs#az_acs_create) | Erstellt einen ACS-Cluster. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Weitere CLI-Skriptbeispiele von Azure Container Service finden Sie in der [Dokumentation zu Azure Container Service](../cli-samples.md).