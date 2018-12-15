---
title: Azure PowerShell-Skriptbeispiel – Erstellen eines Log Analytics-Arbeitsbereichs | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiel – Erstellen eines Log Analytics-Arbeitsbereichs
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: log-analytics
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2017
ms.author: magoedte
ms.openlocfilehash: 5ad04c52da4709a7097ff7915d7af7404d6725eb
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53110352"
---
# <a name="create-a-log-analytics-workspace-with-powershell"></a>Erstellen eines Log Analytics-Arbeitsbereichs mit PowerShell

Mithilfe dieses Skripts verfügen Sie rasch über einen einsatzbereiten Azure Log Analytics-Arbeitsbereich, den Sie benötigen, wenn Sie damit beginnen möchten, Daten zu erfassen, zu analysieren und zu bearbeiten.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/log-analytics/log-analytics-create-new-resource/log-analytics-create-new-resource.ps1 "Create new Log Analytics workspace")]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript wird mithilfe der folgenden Befehle ein neuer Log Analytics-Arbeitsbereich in Ihrem Abonnement erstellt. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [Get-AzureRmOperationalInsightsWorkspace](/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightsworkspace) | Ruft Informationen zu einem vorhandenen Arbeitsbereich ab. |
| [New-AzureRmOperationalInsightsWorkspace](/powershell/module/azurerm.operationalinsights/new-azurermoperationalinsightsworkspace) | Erstellt einen Arbeitsbereich in der angegebenen Ressourcengruppe und am angegebenen Ort. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

