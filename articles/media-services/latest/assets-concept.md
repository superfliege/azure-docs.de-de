---
title: Medienobjekte in Azure Media Services | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, was Medienobjekte sind und wie sie von Azure Media Services verwendet werden.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 928d88d51503350abe7df847ce58ff066b987c8e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="assets"></a>Medienobjekte

Ein **Medienobjekt** enthält digitale Dateien (z.B. Video, Audio, Bilder, Sammlungen von Miniaturansichten, Textspuren und Untertiteldateien) sowie die Metadaten zu diesen Dateien. Nachdem die digitalen Dateien in ein Medienobjekt geladen wurden, können Sie in den Codierungs- und Streaming-Workflows der Media Services verwendet werden.

Ein Medienobjekt ist einem Blobcontainer im [Azure Storage-Konto](storage-account-concept.md) zugeordnet, und die im Medienobjekt enthaltenen Dateien werden als Blockblobs in diesem Container gespeichert. Sie können über die Storage SDK-Clients mit den Medienobjektdateien in den Containern interagieren.

Azure Media Services unterstützt Blobebenen, wenn das Konto einen Speicher vom Typ „Allgemein v2“ (GPv2) verwendet. In GPv2-Speichern können Sie Dateien auf die kalte Speicherebene verschieben. Die kalte Speicherebene eignet sich für die Archivierung von Mezzaninedateien (Zwischendateien), wenn diese nicht mehr benötigt werden (z.B nach der Codierung).

In Media Services v3 kann die Auftragseingabe von Medienobjekten oder HTTP- bzw. HTTPS- URLs erstellt werden. Informationen dazu, wie Sie ein Medienobjekt erstellen, das als Eingabe für Ihren Auftrag verwendet werden kann, finden Sie unter [Erstellen einer Auftragseingabe aus einer lokalen Datei](job-input-from-local-file-how-to.md).

Weitere Informationen finden Sie auch unter [Speicherkonten in Media Services](storage-account-concept.md) und [Transformationen und Aufträge](transform-concept.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Streamen einer Datei](stream-files-dotnet-quickstart.md)
