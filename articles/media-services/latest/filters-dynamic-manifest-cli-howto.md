---
title: Verwenden der CLI zum Erstellen von Filtern mit Azure Media Services | Microsoft-Dokumentation
description: In diesem Thema wird gezeigt, wie Sie mithilfe der CLI Filter in Media Services erstellen können.
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
ms.date: 11/26/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 2ba3de32f4ec3b9f6faf1d5a51da9c1c91e4a2e4
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2019
ms.locfileid: "55099305"
---
# <a name="creating-filters-with-cli"></a>Erstellen von Filtern mit der CLI 

Bei der Inhaltsbereitstellung für Ihre Kunden (Streaming von Liveereignissen oder Video on Demand) benötigen Ihre Kunden möglicherweise mehr Flexibilität als in der Manifestdatei für die Standardmediendatei beschrieben. Azure Media Services ermöglicht es Ihnen, Kontofilter und Medienobjektfilter für Ihre Inhalte zu definieren. Weitere Informationen finden Sie unter [Filter und dynamische Manifeste](filters-dynamic-manifest-overview.md).

In diesem Thema werden das Konfigurieren eines Filters für ein Video on Demand-Medienobjekt und die Verwendung der Befehlszeilenschnittstelle für Media Services v3 zum Erstellen von [Kontofiltern](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) und [Filtern für Medienobjekte](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest) gezeigt. 

## <a name="prerequisites"></a>Voraussetzungen 

- [Erstellen Sie ein Media Services-Konto.](create-account-cli-how-to.md) Merken Sie sich den Namen der Ressourcengruppe und den Namen des Media Services-Kontos. 
- Lesen Sie [Filter und dynamische Manifeste](filters-dynamic-manifest-overview.md).

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="define-a-filter"></a>Definieren eines Filters 

Das folgende Beispiel definiert die Titelauswahlbedingungen, die dem endgültigen Manifest hinzugefügt werden. Dieser Filter bezieht alle EC-3-Audiotitel und alle Videotitel mit Bitraten im Bereich von 0–1000000 ein.

In REST definierte Filter beziehen das Wrapper-JSON-Objekt „Properties“ ein.  

```json
[
    {
        "trackSelections": [
            {
                "property": "Type",
                "value": "Audio",
                "operation": "Equal"
            },
            {
                "property": "FourCC",
                "value": "EC-3",
                "operation": "NotEqual"
            }
        ]
    },
    {
        "trackSelections": [
            {
                "property": "Type",
                "value": "Video",
                "operation": "Equal"
            },
            {
                "property": "Bitrate",
                "value": "0-1000000",
                "operation": "Equal"
            }
        ]
    }
]
```

## <a name="create-account-filters"></a>Erstellen von Kontofiltern

Der folgende [az ams account-filter](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest)-Befehl erstellt einen Kontofilter mit [zuvor definierten](#define-a-filter) Titelfilter-Auswahloptionen. 

Mit diesem Befehl können Sie einen optionalen `--tracks`-Parameter übergeben, der JSON-Code für die ausgewählten Spuren enthält.  Verwenden Sie „@{file}“, um den JSON-Code aus einer Datei zu laden. Wenn Sie die Azure CLI lokal verwenden, geben Sie den gesamten Dateipfad an:

```azurecli
az ams account-filter create -a amsAccount -g resourceGroup -n filterName --tracks @tracks.json
```

Siehe auch [JSON-Beispiele für Filter](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create_an_account_filter).

## <a name="create-asset-filters"></a>Erstellen eines Medienobjektfilters

Der folgende [az ams asset-filter](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest)-Befehl erstellt einen Medienobjektfilter mit [zuvor definierten](#define-a-filter) Titelfilter-Auswahloptionen. 

```azurecli
az ams asset-filter create -a amsAccount -g resourceGroup -n filterName --asset-name assetName --tracks @tracks.json
```

Siehe auch [JSON-Beispiele für Filter](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create_an_asset_filter).

## <a name="next-step"></a>Nächster Schritt

[Streamen von Videos](stream-files-tutorial-with-api.md) 

## <a name="see-also"></a>Weitere Informationen

[Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
