---
title: "Azure CLI-Beispielskript – Erstellen einer Definition für die verwaltete Anwendung | Microsoft-Dokumentation"
description: "Azure CLI-Beispielskript – Erstellen einer Definition für die verwaltete Anwendung"
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
ms.openlocfilehash: 430cadf0cc609ab3473b14115b2956553a677a26
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="create-a-managed-application-definition-with-azure-cli"></a>Erstellen einer Definition für die verwaltete Anwendung mit der Azure CLI

Mit diesem Skript wird eine Definition für die verwaltete Anwendung in einem Dienstkatalog veröffentlicht. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/managed-applications/create-definition/create-definition.sh "Create definition")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet den folgenden Befehl zum Erstellen der Definition für die verwaltete Anwendung. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az managedapp definition create](https://docs.microsoft.com/cli/azure/managedapp/definition#az_managedapp_definition_create) | Erstellen Sie eine Definition für die verwaltete Anwendung. Stellen Sie das Paket bereit, das die erforderlichen Dateien enthält. |


## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](../overview.md).
* Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).
