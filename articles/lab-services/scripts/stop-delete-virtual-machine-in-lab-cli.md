---
title: 'Azure CLI-Skriptbeispiel: Beenden und Löschen eines virtuellen Computers in einem benutzerdefinierten Lab | Microsoft-Dokumentation'
description: Dieses Azure CLI-Skript beendet und löscht einen virtuellen Computer in einem benutzerdefinierten Lab.
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
ms.openlocfilehash: 0da99bb5e029f4d1c6be74d09ad11c6017121a7c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="use-azure-cli-to-stop-and-delete-a-virtual-machine-in-a-custom-lab"></a>Verwenden von Azure CLI zum Beenden und Löschen eines virtuellen Computers in einem benutzerdefinierten Lab

Dieses Azure CLI-Skript beendet und löscht einen virtuellen Computer in einem benutzerdefinierten Lab. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/devtest-lab/stop-delete-virtual-machine-in-lab/stop-delete-virtual-machine-in-lab.sh "Stop and delete a VM in a custom lab")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle:

| Get-Help | Notizen |
|---|---|
| [az lab vm stop](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-stop) | Beendet einen virtuellen Computer in einem benutzerdefinierten Lab. Dieser Vorgang kann eine Weile dauern. |
| [az lab vm delete](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-delete) | Löscht einen virtuellen Computer in einem benutzerdefinierten Lab. Dieser Vorgang kann eine Weile dauern. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Weitere PowerShell-Skriptbeispiele für Azure Lab Services finden Sie in den [Azure Lab Services – Beispiele für die Befehlszeilenschnittstelle](../samples-cli.md).