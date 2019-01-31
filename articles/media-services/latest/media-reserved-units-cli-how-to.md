---
title: Verwenden der CLI zum Skalieren reservierter Einheiten für Medien – Azure | Microsoft-Dokumentation
description: In diesem Thema wird gezeigt, wie Sie die CLI zum Skalieren der Medienverarbeitung mit Azure Media Services verwenden.
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
ms.date: 01/24/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: b40ab6bcc2f718eda85ff64d69a6689e12d60ab8
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2019
ms.locfileid: "55094834"
---
# <a name="scaling-media-processing"></a>Skalieren der Medienverarbeitung

Mit Azure Media Services können Sie die Medienverarbeitung in Ihrem Konto skalieren, indem Sie reservierte Einheiten für Medien (Media Reserved Units, MRUs) verwalten. Eine ausführliche Übersicht finden Sie unter [Scaling media processing](../previous/media-services-scale-media-processing-overview.md) (Skalieren der Medienverarbeitung). 

In diesem Artikel wird veranschaulicht, wie Sie [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref) zum Skalieren von MRUs verwenden.

> [!NOTE]
> Für die Audioanalyse- und Videoanalyseaufträge, die von Media Services v3 oder Video Indexer ausgelöst werden, wird dringend empfohlen, Ihr Konto mit zehn S3-MRUs bereitzustellen. <br/>Erstellen Sie im [Azure-Portal](https://portal.azure.com/) ein Supportticket, falls Sie mehr als zehn S3-MRUs benötigen.

## <a name="prerequisites"></a>Voraussetzungen 

[Erstellen Sie ein Media Services-Konto.](create-account-cli-how-to.md)

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="scale-media-reserved-units-with-cli"></a>Skalieren von reservierten Einheiten für Medien mit der CLI

Führen Sie den Befehl `mru` aus.

Mit dem folgenden Befehl [az ams account mru](https://docs.microsoft.com/cli/azure/ams/account/mru?view=azure-cli-latest) werden reservierte Einheiten für Medien im Konto „amsaccount“ festgelegt, indem die Parameter **count** und **type** verwendet werden.

```azurecli
az account set mru -n amsaccount -g amsResourceGroup --count 10 --type S3
```

## <a name="billing"></a>Abrechnung

Ihnen werden die Gebühren basierend auf Anzahl, Typ und Zeitraum der Bereitstellung von MRUs unter Ihrem Konto berechnet. Die Gebühren fallen unabhängig davon an, ob Sie Aufträge ausführen oder nicht. Eine ausführliche Erläuterung finden Sie im Abschnitt mit den häufig gestellten Fragen auf der Seite [Media Services – Preise](https://azure.microsoft.com/pricing/details/media-services/).   

## <a name="next-step"></a>Nächster Schritt

[Analysieren von Videos](analyze-videos-tutorial-with-api.md) 

## <a name="see-also"></a>Weitere Informationen

[Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
