---
title: Speicherkonten in Azure Media Services | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Media Services-Speicherkonten verwendet werden.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/15/2019
ms.author: juliako
ms.openlocfilehash: f5fc1385bff92851db81cd4ed397d66cb8a832f2
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2019
ms.locfileid: "54350930"
---
# <a name="storage-accounts"></a>Speicherkonten

Wenn Sie ein Media Services-Konto erstellen, müssen Sie den Namen einer Azure Storage-Kontoressource angeben. Das angegebene Speicherkonto wird an Ihr Media Services-Konto angefügt. Das Media Services-Konto und das ihm zugeordnete Speicherkonto müssen sich im selben Datencenter und in derselben Ressourcengruppe befinden.

Sie müssen über ein **primäres** Speicherkonto verfügen. Darüber hinaus können Sie beliebig viele **sekundäre** Speicherkonten an Ihr Media Services-Konto anfügen. Media Services unterstützt Konten des Typs **Allgemein v2** (GPv2) oder **Allgemein v1** (GPv1). 

>[!NOTE]
> Reine Blobkonten sind als **primäre** Konten nicht zulässig. 

Wir empfehlen die Verwendung von GPv2, damit Sie zwischen „heißen“ und „kalten“ Speicherebenen wählen können. Weitere Informationen zu Speicherkonten finden Sie in der [Übersicht über Azure Storage-Konten](../../storage/common/storage-account-overview.md). 

## <a name="assets-in-a-storage-account"></a>Medienobjekte in einem Speicherkonto

In Media Services v3 verwenden Sie Azure Storage-APIs zum Hochladen von Dateien. Informationen zur Verwendung von Speicher-APIs mit Media Services zum Hochladen Ihrer Eingabedateien finden Sie unter [Erstellen einer Auftragseingabe aus einer lokalen Datei](job-input-from-local-file-how-to.md). 

> [!Note]
> Sie können den Inhalt von Blobcontainern, die mit dem Media Services SDK generiert wurden, nur mithilfe von Media Service-APIs ändern.

## <a name="next-steps"></a>Nächste Schritte

Informationen dazu, wie Sie ein Speicherkonto an Ihr Media Services-Konto anfügen, finden Sie unter [Erstellen eines Kontos](create-account-cli-quickstart.md).
