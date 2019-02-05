---
title: 'Azure CLI-Skriptbeispiel: Erstellen eines Azure Event Grid-Abonnements | Microsoft-Dokumentation'
description: Das Azure CLI-Skript in diesem Thema zeigt, wie auf Kontoebene ein Event Grid-Abonnement für Änderungen des Auftragsstatus erstellt wird.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/25/2019
ms.author: juliako
ms.openlocfilehash: 2fcacd559db0ab143ffa7ed49bbd546785765154
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2019
ms.locfileid: "55098904"
---
# <a name="cli-example-create-an-azure-event-grid-subscription"></a>CLI-Beispiel: Erstellen eines Azure Event Grid-Abonnements 

Das Azure CLI-Skript in diesem Artikel zeigt, wie auf Kontoebene ein Event Grid-Abonnement für Änderungen des Auftragsstatus erstellt wird.

## <a name="prerequisites"></a>Voraussetzungen 

[Erstellen Sie ein Media Services-Konto.](../create-account-cli-how-to.md)

[!INCLUDE [media-services-cli-instructions.md](../../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-event-grid/Create-EventGrid.sh "Create an EventGrid subscription")]

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele finden Sie unter [Azure CLI-Beispiele](../cli-samples.md).
