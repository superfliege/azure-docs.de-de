---
title: 'Azure CLI-Beispiele: Verwenden eines benutzerdefinierten VM-Images | Microsoft-Dokumentation'
description: Azure CLI-Beispiele
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 637a2e48ca95b5d1a456f9a48f7c8e9008b1d14d
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2019
ms.locfileid: "55697741"
---
# <a name="create-a-virtual-machine-scale-set-from-a-custom-vm-image-with-the-azure-cli"></a>Erstellen einer VM-Skalierungsgruppe auf der Grundlage eines benutzerdefinierten VM-Images mithilfe der Azure CLI
Dieses Skript erstellt eine VM-Skalierungsgruppe mit einem benutzerdefinierten VM-Image als Quelle für die VM-Instanzen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript
[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine-scale-sets/use-custom-vm-image/use-custom-vm-image.sh "Create a virtual machine scale set with a custom VM image")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, die Skalierungsgruppe und alle dazugehörigen Ressourcen zu entfernen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts
In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine VM-Skalierungsgruppe und alle dazugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/ad/group) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az vmss create](/cli/azure/vmss) | Erstellt die VM-Skalierungsgruppe und verbindet sie mit dem virtuellen Netzwerk, dem Subnetz und der Netzwerksicherheitsgruppe. Des Weiteren wird ein Lastenausgleich erstellt, um Datenverkehr auf mehrere VM-Instanzen zu verteilen. Dieser Befehl gibt auch das zu verwendende VM-Image und die Administratoranmeldeinformationen an.  |
| [az group delete](/cli/azure/ad/group) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Weitere Azure CLI-Skriptbeispiele für VM-Skalierungsgruppen finden Sie in der [Dokumentation zu Azure-VM-Skalierungsgruppen](../cli-samples.md).
