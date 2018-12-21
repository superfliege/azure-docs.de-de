---
title: Azure PowerShell-Skriptbeispiel – Abonnieren eines benutzerdefinierten Themas | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiel – Abonnieren eines benutzerdefinierten Themas
services: event-grid
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/10/2018
ms.author: tomfitz
ms.openlocfilehash: cddf60c3e9356b9c18ab60adbea8eb5af768ad90
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269249"
---
# <a name="subscribe-to-events-for-a-custom-topic-with-powershell"></a>Abonnieren von Ereignissen für ein benutzerdefiniertes Thema mit PowerShell

Dieses Skript erstellt ein Event Grid-Abonnement für die Ereignisse für ein benutzerdefiniertes Thema.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Für das Vorschaubeispielskript ist das Event Grid-Modul erforderlich. Führen Sie zum Installieren `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery` aus.

## <a name="sample-script---stable"></a>Beispielskript: stabil

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-custom-topic/subscribe-to-custom-topic.ps1 "Subscribe to custom topic")]

## <a name="sample-script---preview-module"></a>Beispielskript: Vorschaumodul

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-custom-topic-preview/subscribe-to-custom-topic-preview.ps1 "Subscribe to custom topic")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet den folgenden Befehl, um das Ereignisabonnement zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzureRmEventGridSubscription](https://docs.microsoft.com/powershell/module/azurerm.eventgrid/new-azurermeventgridsubscription) | Erstellen Sie ein Event Grid-Abonnement. |

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](../overview.md).
* Weitere Informationen zu PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/azure/get-started-azureps).
