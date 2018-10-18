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
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: eae9947b7dcbd6f2c52fb0d4abc65375aed7766e
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378787"
---
# <a name="cli-example-create-an-azure-event-grid-subscription"></a>CLI-Beispiel: Erstellen eines Azure Event Grid-Abonnements 

Das Azure CLI-Skript in diesem Artikel zeigt, wie auf Kontoebene ein Event Grid-Abonnement für Änderungen des Auftragsstatus erstellt wird.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens die Azure CLI-Version 2.0.20 verwenden. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-event-grid/Create-EventGrid.sh "Create an EventGrid subscription")]

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele finden Sie unter [Azure CLI-Beispiele](../cli-samples.md).
