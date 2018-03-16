---
title: 'Azure CLI-Skriptbeispiel: Skalieren eines ACS-Clusters | Microsoft Dokumentation'
description: 'Azure CLI-Skriptbeispiel: Skalieren eines ACS-Clusters'
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
ms.openlocfilehash: 7d3997f28dd07d5eb31fc811fbb7ffa1f8d2c7c4
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="scale-an-azure-container-service-cluster"></a>Skalieren eines Azure Container Service-Clusters

In diesem Beispiel wird Azure Container Service skaliert. 

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

```azurecli
az acs scale --resource-group myResourceGroup --name myK8SCluster --new-agent-count 5
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
| [az acs scale](/cli/azure/acs#az_acs_scale) | Skaliert einen ACS-Cluster. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Weitere CLI-Skriptbeispiele von Azure Container Service finden Sie in der [Dokumentation zu Azure Container Service](../cli-samples.md).

