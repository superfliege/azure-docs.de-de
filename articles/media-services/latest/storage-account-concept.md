---
title: Clouduploads und Cloudspeicherung mit Azure Media Services | Microsoft-Dokumentation
description: Dieser Artikel beschreibt Konzepte für Clouduploads und Cloudspeicherung.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/21/2019
ms.author: juliako
ms.openlocfilehash: cda029dd11e8cb4cb07e9fce7eef95d6d4d78d7e
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960213"
---
# <a name="cloud-upload-and-storage"></a>Clouduploads und Cloudspeicherung

Um mit dem Verwalten, Verschlüsseln, Codieren, Analysieren und Streamen von Medieninhalten in Azure beginnen zu können, müssen Sie ein Media Services-Konto erstellen. Wenn Sie ein Media Services-Konto erstellen, müssen Sie den Namen einer Azure Storage-Kontoressource angeben. Das angegebene Speicherkonto wird an Ihr Media Services-Konto angefügt. 

Das Media Services-Konto und alle zugeordneten Speicherkonten müssen im gleichen Azure-Abonnement enthalten sein. Zur Vermeidung zusätzlicher Wartezeiten und Kosten für ausgehenden Datenverkehr wird dringend empfohlen, Speicherkonten zu verwenden, die sich am selben Ort wie das Media Services-Konto befinden.

Sie müssen über ein **primäres** Speicherkonto verfügen. Darüber hinaus können Sie beliebig viele **sekundäre** Speicherkonten an Ihr Media Services-Konto anfügen. Media Services unterstützt Konten des Typs **Allgemein v2** (GPv2) oder **Allgemein v1** (GPv1). 

>[!NOTE]
> Reine Blobkonten sind als **primäre** Konten nicht zulässig. 

Wir empfehlen die Verwendung von GPv2, damit Sie zwischen „heißen“ und „kalten“ Speicherebenen wählen können. Weitere Informationen zu Speicherkonten finden Sie in der [Übersicht über Azure Storage-Konten](../../storage/common/storage-account-overview.md). 

Es gibt verschiedene SKUs, die Sie für Ihr Speicherkonto auswählen können. Weitere Informationen finden Sie unter [Storage accounts](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest) (Speicherkonten). Falls Sie mit Speicherkonten experimentieren möchten, verwenden Sie `--sku Standard_LRS`. Wenn Sie eine SKU für die Produktion auswählen, sollten Sie jedoch die Verwendung von `--sku Standard_RAGRS` erwägen, da diese Option geografische Replikation zum Gewährleisten der Geschäftskontinuität bietet. 

## <a name="assets-in-a-storage-account"></a>Medienobjekte in einem Speicherkonto

In Media Services v3 verwenden Sie Azure Storage-APIs zum Hochladen von Dateien in Medienobjekte. Weitere Informationen finden Sie unter [Konzept von Medienobjekten](assets-concept.md).

> [!Note]
> Sie können den Inhalt von Blobcontainern, die mit dem Media Services SDK generiert wurden, nur mithilfe von Media Service-APIs ändern.
 
## <a name="next-steps"></a>Nächste Schritte

Informationen dazu, wie Sie ein Speicherkonto an Ihr Media Services-Konto anfügen, finden Sie unter [Erstellen eines Kontos](create-account-cli-quickstart.md).
