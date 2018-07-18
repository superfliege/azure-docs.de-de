---
title: Includedatei
description: Includedatei
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/17/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 3009948c72b0cad6ba603a3ccbe35db8d93fe2c7
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664632"
---
## <a name="storage"></a>Speicher

|  |  |
|---------|---------|
| [Zulässige SKUs für Speicherkonten und virtuelle Computer](../articles/azure-policy/scripts/allowed-skus-storage.md) | Erfordert, dass Speicherkonten und virtuelle Computer genehmigte SKUs verwenden. Verwendet integrierte Richtlinien, um die Verwendung von genehmigten SKUs zu gewährleisten. Sie geben ein Array von genehmigten SKUs für virtuelle Computer und ein Array von genehmigten SKUs für Speicherkonten an. |
| [Zulässige Speicherkonten-SKUs](../articles/azure-policy/scripts/allowed-stor-acct-skus.md) | Erfordert, dass Speicherkonten eine genehmigte SKU verwenden. Sie geben ein Array von zulässigen SKUs an. |
| [Verweigern des kalten Zugriffstierings für Speicherkonten](../articles/azure-policy/scripts/deny-cool-access-tiering.md) | Untersagt die Verwendung des kalten Zugriffstierings für Blob-Speicherkonten.  |
| [Gewährleisten eines HTTPS-Datenverkehrs ausschließlich für Speicherkonten](../articles/azure-policy/scripts/ensure-https-stor-acct.md) | Erfordert, dass Speicherkonten HTTPS-Datenverkehr verwenden.  |
| [Gewährleisten der Verschlüsselung von Speicherdateien](../articles/azure-policy/scripts/ensure-store-file-enc.md) | Erfordert, dass die Dateiverschlüsselung für Speicherkonten aktiviert ist.  |
| [Verlangen der Speicherkontenverschlüsselung](../articles/azure-policy/scripts/req-store-acct-enc.md) | Erfordert das Verwenden der Blobverschlüsselung für das Speicherkonto.  |