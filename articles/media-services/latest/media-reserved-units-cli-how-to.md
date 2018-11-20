---
title: Skalieren von reservierten Einheiten für Medien – Azure | Microsoft-Dokumentation
description: Dieses Thema bietet eine Übersicht über die Skalierung der Medienverarbeitung mithilfe von Azure Media Services.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/11/2018
ms.author: juliako
ms.openlocfilehash: db1915f23c33b5cc0d504f8fcc21b9533228247f
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634395"
---
# <a name="scaling-media-processing"></a>Skalieren der Medienverarbeitung

Mit Azure Media Services können Sie die Medienverarbeitung in Ihrem Konto skalieren, indem Sie reservierte Einheiten für Medien (Media Reserved Units, MRUs) verwalten. Eine ausführliche Übersicht finden Sie unter [Scaling media processing](../previous/media-services-scale-media-processing-overview.md) (Skalieren der Medienverarbeitung). In diesem Artikel wird veranschaulicht, wie Sie [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref) zum Skalieren von MRUs verwenden.

> [!IMPORTANT]
> Prüfen Sie die in [diesem Abschnitt](#considerations) beschriebenen Überlegungen.  
> 
>

## <a name="prerequisites"></a>Voraussetzungen 

- Installieren und verwenden Sie die Befehlszeilenschnittelle lokal. Dieser Artikel erfordert mindestens Version 2.0 der Azure CLI. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 

    Derzeit funktionieren nicht alle Befehle der [Befehlszeilenschnittstelle von Media Services v3](https://aka.ms/ams-v3-cli-ref) in Azure Cloud Shell. Es wird empfohlen, die Befehlszeilenschnittstelle lokal zu verwenden.

- [Erstellen Sie ein Media Services-Konto.](create-account-cli-how-to.md)

## <a name="scale-media-reserved-units-with-cli"></a>Skalieren von reservierten Einheiten für Medien mit der CLI

Mit dem folgenden Befehl [az ams account mru](https://docs.microsoft.com/cli/azure/ams/account/mru?view=azure-cli-latest) werden reservierte Einheiten für Medien im Konto „amsaccount“ festgelegt, indem die Parameter **count** und **type** verwendet werden.

```azurecli
az account set mru -n amsaccount -g amsResourceGroup --count 10 --type S3
```

## <a name="considerations"></a>Überlegungen

- Für die Audioanalyse- und Videoanalyseaufträge, die von Media Services v3 oder Video Indexer ausgelöst werden, wird dringend empfohlen, Ihr Konto mit zehn S3-MRUs bereitzustellen.
- Erstellen Sie im [Azure-Portal](https://portal.azure.com/) ein Supportticket, falls Sie mehr als zehn S3-MRUs benötigen.

## <a name="billing"></a>Abrechnung

Ihnen werden die Gebühren basierend auf Anzahl, Typ und Zeitraum der Bereitstellung von MRUs unter Ihrem Konto berechnet. Die Gebühren fallen unabhängig davon an, ob Sie Aufträge ausführen oder nicht. Eine ausführliche Erläuterung finden Sie im Abschnitt mit den häufig gestellten Fragen auf der Seite [Media Services – Preise](https://azure.microsoft.com/pricing/details/media-services/).   

## <a name="next-step"></a>Nächster Schritt

[Analysieren von Videos](analyze-videos-tutorial-with-api.md) 

## <a name="see-also"></a>Weitere Informationen

[Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
