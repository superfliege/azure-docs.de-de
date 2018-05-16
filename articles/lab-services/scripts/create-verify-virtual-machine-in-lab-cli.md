---
title: 'Azure CLI-Skriptbeispiel: Erstellen und Überprüfen eines virtuellen Computers in einem benutzerdefinierten Lab | Microsoft-Dokumentation'
description: Dieses Azure CLI-Skript erstellt einen virtuellen Computer in einem benutzerdefinierten Lab, und überprüft, ob der Computer verfügbar ist.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2018
ms.author: spelluru
ms.custom: mvc
ms.openlocfilehash: bd564cda7b4d5c2158b8499b48b8faa68309b461
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="use-azure-cli-to-create-and-verify-availability-of-a-virtual-machine-in-a-custom-lab"></a>Verwenden von Azure CLI zum Erstellen eines virtuellen Computers in einem benutzerdefinierten Lab und zum Überprüfen seiner Verfügbarkeit

Dieses Azure CLI-Skript erstellt einen virtuellen Computer in einem benutzerdefinierten Lab. Der virtuelle Computer basiert auf einem Marketplace-Image mit SSH-Authentifizierung. Das Skript überprüft dann, ob der virtuelle Computer für die Verwendung verfügbar ist. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/devtest-lab/create-verify-virtual-machine-in-lab/create-verify-virtual-machine-in-lab.sh "Create and verify availability of a VM")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle:

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az lab vm create ](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-create) | Erstellt einen virtuellen Computer in einem benutzerdefinierten Lab. |
| [az lab vm show](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-show) | Zeigt den Status des virtuellen Computers in einem benutzerdefinierten Lab an. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Weitere PowerShell-Skriptbeispiele für Azure Lab Services finden Sie in den [Azure Lab Services – Beispiele für die Befehlszeilenschnittstelle](../samples-cli.md).