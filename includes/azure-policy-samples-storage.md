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
ms.openlocfilehash: b967558828c42331b3702fe90ce842fd1c0032bd
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47003817"
---
## <a name="storage"></a>Speicher

|  |  |
|---------|---------|
| [Zulässige SKUs für Speicherkonten und virtuelle Computer](../articles/governance/policy/samples/allowed-skus-storage.md) | Erfordert, dass Speicherkonten und virtuelle Computer genehmigte SKUs verwenden. Verwendet integrierte Richtlinien, um die Verwendung von genehmigten SKUs zu gewährleisten. Sie geben ein Array von genehmigten SKUs für virtuelle Computer und ein Array von genehmigten SKUs für Speicherkonten an. |
| [Zulässige Speicherkonten-SKUs](../articles/governance/policy/samples/allowed-stor-acct-skus.md) | Erfordert, dass Speicherkonten eine genehmigte SKU verwenden. Sie geben ein Array von zulässigen SKUs an. |
| [Verweigern des kalten Zugriffstierings für Speicherkonten](../articles/governance/policy/samples/deny-cool-access-tiering.md) | Untersagt die Verwendung des kalten Zugriffstierings für Blob-Speicherkonten.  |
| [Gewährleisten eines HTTPS-Datenverkehrs ausschließlich für Speicherkonten](../articles/governance/policy/samples/ensure-https-stor-acct.md) | Erfordert, dass Speicherkonten HTTPS-Datenverkehr verwenden.  |
| [Gewährleisten der Verschlüsselung von Speicherdateien](../articles/governance/policy/samples/ensure-store-file-enc.md) | Erfordert, dass die Dateiverschlüsselung für Speicherkonten aktiviert ist.  |
| [Verlangen der Speicherkontenverschlüsselung](../articles/governance/policy/samples/req-store-acct-enc.md) | Erfordert das Verwenden der Blobverschlüsselung für das Speicherkonto.  |