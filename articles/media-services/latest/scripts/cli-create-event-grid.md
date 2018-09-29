---
title: 'Azure CLI-Skriptbeispiel: Erstellen eines Azure Event Grid-Abonnements | Microsoft-Dokumentation'
description: Das Azure CLI-Skript in diesem Thema zeigt, wie auf Kontoebene ein Event Grid-Abonnement für Änderungen des Auftragsstatus erstellt wird.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/11/2018
ms.author: juliako
ms.openlocfilehash: 5293ac55fdb13bba85996f5ed81034d4ebeeff12
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47093019"
---
# <a name="cli-example-create-an-azure-event-grid-subscription"></a>CLI-Beispiel: Erstellen eines Azure Event Grid-Abonnements 

Das Azure CLI-Skript in diesem Artikel zeigt, wie auf Kontoebene ein Event Grid-Abonnement für Änderungen des Auftragsstatus erstellt wird.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens die Azure CLI-Version 2.0.20 verwenden. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-event-grid/Create-EventGrid.sh "Create an EventGrid subscription")]

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele finden Sie unter [Azure CLI-Beispiele](../cli-samples.md).
