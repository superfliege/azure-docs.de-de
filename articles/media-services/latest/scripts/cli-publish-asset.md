---
title: 'Azure CLI-Skriptbeispiel: Veröffentlichen eines Medienobjekts | Microsoft-Dokumentation'
description: Verwenden Sie das Azure CLI-Skript zum Veröffentlichen eines Medienobjekts.
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
ms.openlocfilehash: 9b23fba88f8a6f6d7ecd211f8ad9c137cb92789d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38723084"
---
# <a name="cli-example-publish-an-asset"></a>CLI-Beispiel: Veröffentlichen eines Medienobjekts

Das Azure CLI-Skript in diesem Artikel zeigt, wie ein Streaminglocator erstellt wird und Streaming-URLs zurückerhalten werden. 

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens die Azure CLI-Version 2.0.20 verwenden. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/publish-asset/Publish-Asset.sh "Publish an asset")]

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele finden Sie unter [Azure CLI-Beispiele](../cli-samples.md).
