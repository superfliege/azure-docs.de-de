---
title: 'Azure CLI-Skriptbeispiel: Starten eines virtuellen Computers in einem benutzerdefinierten Lab | Microsoft-Dokumentation'
description: Dieses Azure CLI-Skript startet einen virtuellen Computer in einem benutzerdefinierten Lab.
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
ms.openlocfilehash: c14328904f29f8160a1ccc1130fe6d2cfa629822
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="use-azure-cli-to-start-a-virtual-machine-in-a-custom-lab"></a>Verwenden von Azure CLI zum Starten eines virtuellen Computers in einem benutzerdefinierten Lab

Dieses Azure CLI-Skript startet einen virtuellen Computer in einem benutzerdefinierten Lab. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/devtest-lab/start-connect-virtual-machine-in-lab/start-connect-virtual-machine-in-lab.sh "Start a VM")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle:

| Get-Help | Notizen |
|---|---|
| [az lab vm start ](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-start) | Startet einen virtuellen Computer in einem benutzerdefinierten Lab. Dieser Vorgang kann eine Weile dauern. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Weitere PowerShell-Skriptbeispiele für Azure Lab Services finden Sie in den [Azure Lab Services – Beispiele für die Befehlszeilenschnittstelle](../samples-cli.md).