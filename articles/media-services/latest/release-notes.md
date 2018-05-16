---
title: 'Azure Media Services v3: Versionshinweise | Microsoft-Dokumentation'
description: Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Azure Media Services v3.
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
ms.openlocfilehash: fc6c5ba6cd97c261dd44eade33bf21e8d1b74bf0
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
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

Bekanntes Problem:

Stellen Sie beim Übermitteln eines Auftrags mit einer HTTPS-URL (JobInputHttp), die auf den Quellinhalt verweist, sicher, dass der HTTP-Server die HEAD-Anforderung unterstützt. Andernfalls wird der Auftrag abgelehnt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Übersicht](media-services-overview.md)
