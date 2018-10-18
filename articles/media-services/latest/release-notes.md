---
title: 'Azure Media Services v3: Versionshinweise | Microsoft-Dokumentation'
description: Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 09/25/2018
ms.author: juliako
ms.openlocfilehash: ed2550c1df4645933fb968c54ee536995c810136
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47219328"
---
# <a name="azure-media-services-v3-preview-release-notes"></a>Versionshinweise zu Azure Media Services v3 (Vorschauversion) 

Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

* Neueste Versionen
* Bekannte Probleme
* Fehlerbehebungen
* Veraltete Funktionen
* Pläne für Änderungen

## <a name="may-07-2018"></a>07. Mai 2018

### <a name="net-sdk"></a>.NET SDK

Das .NET SDK enthält die folgenden Features:

1. **Transformationen** und **Aufträge** zum Codieren oder Analysieren von Medieninhalten. Beispiele finden Sie in den Tutorials zum [Streamen von Dateien](stream-files-tutorial-with-api.md) und [Analysieren](analyze-videos-tutorial-with-api.md).
2. **StreamingLocators** zum Veröffentlichen und Streamen von Inhalten auf Endbenutzergeräten
3. **StreamingPolicies** und **ContentKeyPolicies** zum Konfigurieren der Schlüsselübermittlung und des Inhaltsschutzes (DRM) bei der Inhaltsbereitstellung
4. **LiveEvents** und **LiveOutputs** zum Konfigurieren der Erfassung und Archivierung von Livestreaminginhalten
5. **Medienobjekte** zum Speichern und Veröffentlichen von Medieninhalten in Azure Storage 
6. **StreamingEndpoints** zum Konfigurieren und Skalieren der dynamischen Paketerstellung, der Verschlüsselung und des Streamings von Live- und On-Demand-Medieninhalten

### <a name="known-issues"></a>Bekannte Probleme

* Wenn Sie einen Auftrag übermitteln, können Sie angeben, dass Ihr Quellvideo mithilfe von HTTPS, URLs, SAS-URLs oder Pfaden zu Dateien in Azure Blob Storage erfasst wird. AMS v3 unterstützt derzeit keine segmentierte Transfercodierung über HTTPS-URLs.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Übersicht](media-services-overview.md)
