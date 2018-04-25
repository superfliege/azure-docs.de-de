---
title: Azure PowerShell-Skriptbeispiel – Erstellen eines benutzerdefinierten Themas | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiel – Erstellen eines benutzerdefinierten Themas
services: event-grid
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/05/2018
ms.author: tomfitz
ms.openlocfilehash: 2948f1eabe93d092d905965429ff73eaa6c443cd
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="create-event-grid-custom-topic-with-powershell"></a>Erstellen eines benutzerdefinierten Event Grid-Themas mit PowerShell

Dieses Skript erstellt ein benutzerdefiniertes Event Grid-Thema.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/event-grid/create-custom-topic/create-custom-topic.ps1 "Create custom topic")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet den folgenden Befehl zum Erstellen des benutzerdefinierten Themas. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzureRmEventGridTopic](https://docs.microsoft.com/powershell/module/azurerm.eventgrid/new-azurermeventgridtopic) | Erstellt ein benutzerdefiniertes Event Grid-Thema. |

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](../overview.md).
* Weitere Informationen zu PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/azure/get-started-azureps).
