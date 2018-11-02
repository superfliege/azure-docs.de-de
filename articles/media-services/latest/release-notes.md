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
ms.date: 10/22/2018
ms.author: juliako
ms.openlocfilehash: db68f979239a5783338d99360209ae231a75c936
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945034"
---
# <a name="azure-media-services-v3-release-notes"></a>Versionshinweise zu Azure Media Services v3 

Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

* Neueste Versionen
* Bekannte Probleme
* Fehlerbehebungen
* Veraltete Funktionen
* Pläne für Änderungen

## <a name="october-2018---ga"></a>Oktober 2018 – Allgemeine Verfügbarkeit (GA)

In diesem Abschnitt werden die Oktober-Updates für Azure Media Services (AMS) beschrieben.

### <a name="rest-v3-ga-release"></a>REST v3-GA-Release

Das [REST v3-GA-Release](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01) umfasst mehr APIs für den Livebetrieb, Manifestfilter für die Konto-/Ressourcenebene und DRM-Support.

#### <a name="azure-resource-management"></a>Azure-Ressourcenverwaltung 

Die Unterstützung der Azure-Ressourcenverwaltung ermöglicht eine einheitliche Verwaltung und Vorgangs-API (jetzt alles an einem Ort).

Ab dieser Version können Sie Resource Manager-Vorlagen verwenden, um Liveereignisse zu erstellen.

#### <a name="improvement-of-asset-operations"></a>Verbesserung von Medienobjektvorgängen 

Die folgenden Verbesserungen wurden eingeführt:

- Erfassen von HTTP(S)-URLs oder Azure Blob Storage-SAS-URLs
- Angeben von eigenen Containernamen für Medienobjekte 
- Einfachere Unterstützung der Ausgabe zur Erstellung von benutzerdefinierten Workflows mit Azure Functions

#### <a name="new-transform-object"></a>Neues Transform-Objekt

Mit dem neuen **Transform**-Objekt wird das Codierungsmodell vereinfacht. Es ist mit dem neuen Objekt einfach, Resource Manager-Vorlagen und -Voreinstellungen für die Codierung zu erstellen und gemeinsam zu nutzen. 

#### <a name="azure-active-directory-authentication-and-rbac"></a>Azure Active Directory-Authentifizierung und rollenbasierte Zugriffssteuerung

Mit der Azure AD-Authentifizierung und rollenbasierten Zugriffssteuerung (RBAC) werden sichere Transformationen, Liveereignisse, Richtlinien für Inhaltsschlüssel oder Medienobjekte nach Rolle oder Benutzer in Azure AD ermöglicht.

#### <a name="client-sdks"></a>Client-SDKs  

In Media Services v3 unterstützte Sprachen: .NET Core, Java, Node.js, Ruby, TypeScript, Python, Go.

#### <a name="live-encoding-updates"></a>Updates der Livecodierung

Die folgenden Updates für die Livecodierung wurden eingeführt:

- Neuer Modus mit geringer Latenz für den Livebetrieb (10 Sekunden für den End-to-End-Vorgang).
- Verbesserte RTMP-Unterstützung (höhere Stabilität und bessere Unterstützung für Quellcodierer).
- Sichere RTMPS-Erfassung.

    Bei der Erstellung eines Liveereignisses erhalten Sie jetzt vier Erfassungs-URLs. Die vier Erfassungs-URLs sind nahezu identisch und verfügen über das gleiche Streamingtoken (AppId). Nur der Portnummernteil unterscheidet sich. Zwei der URLs dienen als primäre URL und Backup-URL für RTMPS. 
- Unterstützung der 24-Stunden-Transcodierung. 
- Verbesserte Unterstützung für Werbeeinblendungen in RTMP über SCTE35.

#### <a name="improved-event-grid-support"></a>Verbesserte Event Grid-Unterstützung

Folgende Verbesserungen der Event Grid-Unterstützung wurden eingeführt:

- Azure Event Grid-Integration, um die Entwicklung mit Logic Apps und Azure Functions zu vereinfachen. 
- Abonnieren von Ereignissen für Codierung, Livekanäle und mehr.

### <a name="cmaf-support"></a>CMAF-Unterstützung

CMAF- und „cbcs“-Verschlüsselungsunterstützung für Apple HLS- (iOS 11+) und MPEG-DASH-Player, die CMAF unterstützen.

### <a name="video-indexer"></a>Video Indexer

Das Video Indexer-GA-Release wurde im August angekündigt. Neue Informationen zu derzeit unterstützten Funktionen finden Sie unter [Was ist Video Indexer?](../../cognitive-services/video-indexer/video-indexer-overview.md?toc=/azure/media-services/video-indexer/toc.json&bc=/azure/media-services/video-indexer/breadcrumb/toc.json). 

### <a name="plans-for-changes"></a>Pläne für Änderungen

#### <a name="azure-cli-20"></a>Azure CLI 2.0
 
Das Azure CLI 2.0-Modul mit Vorgängen für alle Features (z.B. Livebetrieb, Richtlinien für Inhaltsschlüssel, Konto-/Ressourcenfilter, Streamingrichtlinien) ist in Kürze verfügbar. 

### <a name="known-issues"></a>Bekannte Probleme

Nur Kunden, die die Vorschauversion der API für Ressourcen- oder Kontofilter verwendet haben, sind vom folgenden Problem betroffen.

Wenn Sie zwischen dem 28.09. und dem 12.10. mit der Media Services v3 CLI oder den zugehörigen APIs Ressourcen- oder Kontofilter erstellt haben, müssen Sie aufgrund eines Versionskonflikts alle Ressourcen- und Kontofilter entfernen und neu erstellen. 

## <a name="may-2018---preview"></a>Mai 2018 – Vorschau

### <a name="net-sdk"></a>.NET SDK

Das .NET SDK enthält die folgenden Features:

* **Transformationen** und **Aufträge** zum Codieren oder Analysieren von Medieninhalten. Beispiele finden Sie in den Tutorials zum [Streamen von Dateien](stream-files-tutorial-with-api.md) und [Analysieren](analyze-videos-tutorial-with-api.md).
* **StreamingLocators** zum Veröffentlichen und Streamen von Inhalten auf Endbenutzergeräten
* **StreamingPolicies** und **ContentKeyPolicies** zum Konfigurieren der Schlüsselübermittlung und des Inhaltsschutzes (DRM) bei der Inhaltsbereitstellung
* **LiveEvents** und **LiveOutputs** zum Konfigurieren der Erfassung und Archivierung von Livestreaminginhalten
* **Medienobjekte** zum Speichern und Veröffentlichen von Medieninhalten in Azure Storage 
* **StreamingEndpoints** zum Konfigurieren und Skalieren der dynamischen Paketerstellung, der Verschlüsselung und des Streamings von Live- und On-Demand-Medieninhalten

### <a name="known-issues"></a>Bekannte Probleme

* Wenn Sie einen Auftrag übermitteln, können Sie angeben, dass Ihr Quellvideo mithilfe von HTTPS, URLs, SAS-URLs oder Pfaden zu Dateien in Azure Blob Storage erfasst wird. AMS v3 unterstützt derzeit keine segmentierte Transfercodierung über HTTPS-URLs.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Übersicht](media-services-overview.md)
