---
title: 'Azure CLI-Skriptbeispiel: Erstellen eines Azure Media Services-Medienobjekts | Microsoft-Dokumentation'
description: Das Azure CLI-Skript in diesem Thema zeigt, wie ein Media Services-Medienobjekt erstellt wird, in das Inhalt hochgeladen werden kann.
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
ms.date: 11/11/2018
ms.author: juliako
ms.openlocfilehash: 76d2adf3c408e174081c3106e07a87f751b4a179
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51612660"
---
# <a name="cli-example-create-an-asset"></a>CLI-Beispiel: Erstellen eines Medienobjekts

Das Azure CLI-Skript in diesem Artikel zeigt, wie ein Media Services-Medienobjekt erstellt wird, in das Inhalt hochgeladen werden kann.

## <a name="prerequisites"></a>Voraussetzungen 

- Installieren und verwenden Sie die Befehlszeilenschnittelle lokal. Dieser Artikel erfordert mindestens die Azure CLI-Version 2.0. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 

    Derzeit funktionieren nicht alle Befehle der [Befehlszeilenschnittstelle von Media Services v3](https://aka.ms/ams-v3-cli-ref) in Azure Cloud Shell. Es wird empfohlen, die Befehlszeilenschnittstelle lokal zu verwenden.

- [Erstellen Sie ein Media Services-Konto.](../create-account-cli-how-to.md)

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-asset/Create-Asset.sh "Create an asset")]

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele finden Sie unter [Azure CLI-Beispiele](../cli-samples.md).
