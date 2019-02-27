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
ms.date: 02/12/2019
ms.author: juliako
ms.openlocfilehash: d49d056ab84b60389df8bcaf1c75d6224633863d
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2019
ms.locfileid: "56337857"
---
# <a name="cloud-upload-and-storage"></a>Clouduploads und Cloudspeicherung

Um mit dem Verwalten, Verschlüsseln, Codieren, Analysieren und Streamen von Medieninhalten in Azure beginnen zu können, müssen Sie ein Media Services-Konto erstellen. Wenn Sie ein Media Services-Konto erstellen, müssen Sie den Namen einer Azure Storage-Kontoressource angeben. Das angegebene Speicherkonto wird an Ihr Media Services-Konto angefügt. 

Das Media Services-Konto und alle zugeordneten Speicherkonten müssen im gleichen Azure-Abonnement enthalten sein. Zur Vermeidung zusätzlicher Wartezeiten und Kosten für ausgehenden Datenverkehr wird dringend empfohlen, Speicherkonten zu verwenden, die sich am selben Ort wie das Media Services-Konto befinden.

Sie müssen über ein **primäres** Speicherkonto verfügen. Darüber hinaus können Sie beliebig viele **sekundäre** Speicherkonten an Ihr Media Services-Konto anfügen. Media Services unterstützt Konten des Typs **Allgemein v2** (GPv2) oder **Allgemein v1** (GPv1). 

>[!NOTE]
> Reine Blobkonten sind als **primäre** Konten nicht zulässig. 

Wir empfehlen die Verwendung von GPv2, damit Sie zwischen „heißen“ und „kalten“ Speicherebenen wählen können. Weitere Informationen zu Speicherkonten finden Sie in der [Übersicht über Azure Storage-Konten](../../storage/common/storage-account-overview.md). 

## <a name="assets-in-a-storage-account"></a>Medienobjekte in einem Speicherkonto

In Media Services v3 verwenden Sie Azure Storage-APIs zum Hochladen von Dateien.

> [!Note]
> Sie können den Inhalt von Blobcontainern, die mit dem Media Services SDK generiert wurden, nur mithilfe von Media Service-APIs ändern.

Informationen zur Verwendung von Speicher-APIs mit Media Services zum Hochladen Ihrer Eingabedateien finden Sie unter [Erstellen einer Auftragseingabe aus einer lokalen Datei](job-input-from-local-file-how-to.md). 
 
## <a name="next-steps"></a>Nächste Schritte

Informationen dazu, wie Sie ein Speicherkonto an Ihr Media Services-Konto anfügen, finden Sie unter [Erstellen eines Kontos](create-account-cli-quickstart.md).
