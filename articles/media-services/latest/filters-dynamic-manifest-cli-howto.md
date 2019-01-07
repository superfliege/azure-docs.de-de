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
ms.openlocfilehash: a16024ad5d8b9d2355b579b9b508ef0de91f2ccd
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53133858"
---
# <a name="creating-filters-with-cli"></a>Erstellen von Filtern mit der CLI 

Bei der Inhaltsbereitstellung für Ihre Kunden (Streaming von Liveereignissen oder Video on Demand) benötigen Ihre Kunden möglicherweise mehr Flexibilität als in der Manifestdatei für die Standardmediendatei beschrieben. Azure Media Services ermöglicht es Ihnen, Kontofilter und Medienobjektfilter für Ihre Inhalte zu definieren. Weitere Informationen finden Sie unter [Filter und dynamische Manifeste](filters-dynamic-manifest-overview.md).

In diesem Thema werden das Konfigurieren eines Filters für ein Video on Demand-Medienobjekt und die Verwendung der Befehlszeilenschnittstelle für Media Services v3 zum Erstellen von [Kontofiltern](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) und [Filtern für Medienobjekte](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest) gezeigt. 

## <a name="prerequisites"></a>Voraussetzungen 

- Installieren und verwenden Sie die CLI lokal. Dieser Artikel erfordert mindestens Version 2.0 der Azure CLI. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 

    Derzeit funktionieren nicht alle Befehle der [CLI von Media Services v3](https://aka.ms/ams-v3-cli-ref) in Azure Cloud Shell. Es wird empfohlen, die CLI lokal zu verwenden.
- [Erstellen Sie ein Media Services-Konto.](create-account-cli-how-to.md) Merken Sie sich den Namen der Ressourcengruppe und den Namen des Media Services-Kontos. 
- Lesen Sie [Filter und dynamische Manifeste](filters-dynamic-manifest-overview.md).

## <a name="define-a-filter"></a>Definieren eines Filters 

Das folgende Beispiel definiert die Titelauswahlbedingungen, die dem endgültigen Manifest hinzugefügt werden. Dieser Filter bezieht alle englischen EC-3-Audiotitel und alle Videotitel mit Bitraten im Bereich von 0-1.000.000 ein.

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
                "property": "Language",
                "value": "en",
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

```azurecli
az ams account-filter create -a amsAccount -g resourceGroup -n filterName --tracks @C:\tracks.json
```

Siehe auch [JSON-Beispiele für Filter](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create_an_account_filter).

## <a name="create-asset-filters"></a>Erstellen eines Medienobjektfilters

Der folgende [az ams asset-filter](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest)-Befehl erstellt einen Medienobjektfilter mit [zuvor definierten](#define-a-filter) Titelfilter-Auswahloptionen. 

```azurecli
az ams asset-filter create -a amsAccount -g resourceGroup -n filterName --asset-name assetName --tracks @C:\tracks.json
```

Siehe auch [JSON-Beispiele für Filter](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create_an_asset_filter).

## <a name="next-step"></a>Nächster Schritt

[Streamen von Videos](stream-files-tutorial-with-api.md) 

## <a name="see-also"></a>Weitere Informationen

[Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
