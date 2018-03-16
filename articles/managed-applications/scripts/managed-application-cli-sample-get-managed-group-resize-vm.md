---
title: "Azure CLI-Skriptbeispiel – Abrufen einer verwalteten Ressourcengruppe und Ändern der Größe von virtuellen Computern | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Abrufen einer verwalteten Ressourcengruppe und Ändern der Größe von virtuellen Computern"
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/25/2017
ms.author: tomfitz
ms.openlocfilehash: 9c4032191738703b71319c05bce4f6bbbc61b44b
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-azure-cli"></a>Abrufen von Ressourcen in einer verwalteten Ressourcengruppe und Ändern der Größe von virtuellen Computern mit der Azure CLI

Dieses Skript ruft Ressourcen aus einer verwalteten Ressourcengruppe ab und ändert die Größe der virtuellen Computer in dieser Ressourcengruppe.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/managed-applications/get-application/get-application.sh "Get application")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Bereitstellen der verwalteten Anwendung. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az managedapp list](https://docs.microsoft.com/cli/azure/managedapp#az_managedapp_list) | Auflisten der verwalteten Anwendungen. Geben Sie Abfragewerte an, um die Ergebnisse einzugrenzen. |
| [az resource list](https://docs.microsoft.com/cli/azure/resource#az_resource_list) | Auflisten der Ressourcen. Geben Sie eine Ressourcengruppe und Abfragewerte an, um die Ergebnisse einzugrenzen. |
| [az vm resize](https://docs.microsoft.com/cli/azure/vm#az_vm_resize) | Aktualisieren der Größe eines virtuellen Computers |


## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](../overview.md).
* Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).
