---
title: Migrieren von Azure Media Services V2 zu V3 | Microsoft-Dokumentation
description: Dieser Artikel beschreibt Änderungen, die in Azure Media Services V3 eingeführt wurden, und zeigt Unterschiede zwischen den beiden Versionen.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, streamen, übertragen, live, offline
ms.service: media-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 11/05/2018
ms.author: juliako
ms.openlocfilehash: 2f5c0ef63ba150fdad4aea1a0c65269611d56815
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51247686"
---
# <a name="migration-guidance-for-moving-from-media-services-v2-to-v3"></a>Hinweise zur Migration von Media Services v2 zu v3

Dieser Artikel beschreibt Änderungen, die in Azure Media Services V3 eingeführt wurden, zeigt Unterschiede zwischen den beiden Versionen und enthält die Migrationsanleitung.

Wenn Sie derzeit über einen Videodienst verfügen, der auf Basis von der [älteren APIs von Media Services v2](../previous/media-services-overview.md) entwickelt wurden, überprüfen Sie vor der Migration zu den v3-APIs die folgenden Richtlinien und Überlegungen. Die v3-API bietet zahlreiche Vorteile und neue Features, die die Entwicklungsumgebung und Funktionen von Media Services verbessern. Wie im Abschnitt [Bekannte Probleme](#known-issues) dieses Artikels erläutert, gibt es auch einige Einschränkungen aufgrund von Änderungen zwischen den API-Versionen. Diese Seite wird laufend aktualisiert, da das Media Services-Team die v3-APIs weiterhin verbessert und die Diskrepanz zwischen den Versionen behebt. 

## <a name="benefits-of-media-services-v3"></a>Vorteile von Media Services v3

### <a name="api-is-more-approachable"></a>Besser zugängliche API

*  v3 basiert auf einer vereinheitlichten API-Oberfläche, die sowohl Verwaltungs- als auch Betriebsfunktionen auf der Basis von Azure Resource Manager bereitstellt. Azure Resource Manager-Vorlagen können zum Erstellen und Bereitstellen von Transformationen, Streamingendpunkten, LiveEvents und mehr verwendet werden.
* [Open API (oder auch Swagger)](https://aka.ms/ams-v3-rest-sdk)-Spezifikationsdokument.
    Macht das Schema für alle Dienstkomponenten verfügbar, einschließlich dateibasierter Codierung.
* SDKs verfügbar für [.NET](https://aka.ms/ams-v3-dotnet-ref), .NET Core, [Node.js](https://aka.ms/ams-v3-nodejs-ref), [Python](https://aka.ms/ams-v3-python-ref), [Java](https://aka.ms/ams-v3-java-ref), [Go](https://aka.ms/ams-v3-go-ref) und Ruby.
* [Azure CLI](https://aka.ms/ams-v3-cli-ref)-Integration für einfache Skriptunterstützung.

### <a name="new-features"></a>Neue Funktionen

* Für dateibasierte Auftragsverarbeitung können Sie als Eingabe eine HTTP(S)-URL verwenden.
    Sie müssen noch keine Inhalte in Azure gespeichert haben und müssen auch keine Objekte erstellen.
* Führt das Konzept von [Transformationen](transforms-jobs-concept.md) für dateibasierte Auftragsverarbeitung ein. Eine Transformation kann verwendet werden, um wiederverwendbare Konfigurationen und Azure Resource Manager-Vorlagen zu erstellen, und um Verarbeitungseinstellungen zwischen mehreren Kunden oder Mandanten zu isolieren.
* Ein Medienobjekt kann [mehrere StreamingLocators](streaming-locators-concept.md) mit verschiedenen Einstellungen für die dynamische Paketerstellung und dynamische Verschlüsselung haben.
* Der [Inhaltsschutz](content-key-policy-concept.md) unterstützt Features mit mehreren Schlüsseln.
* Sie können Liveereignisse streamen, die bis zu 24 Stunden lang sind.
* Neue Unterstützung für Livestreaming mit niedriger Latenz für LiveEvents.
* Die LiveEvent-Vorschau unterstützt die dynamische Paketerstellung und dynamische Verschlüsselung. Dadurch wird in der Vorschau der Inhaltsschutz sowie auch die Paketerstellung für DASH und HLS ermöglicht.
* LiveOutput ist einfacher zu verwenden als die Programmentität in den v2-APIs. 
* Sie verfügen über rollenbasierte Zugriffssteuerung (RBAC) für Ihre Entitäten. 

## <a name="changes-from-v2"></a>Änderungen von V2

* Für Ressourcen, die mit v3 erstellt wurden, unterstützt Media Services nur die serverseitige Speicherverschlüsselung, siehe [Azure Storage Service Encryption für ruhende Daten](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).
    * Sie können v3-APIs mit Ressourcen verwenden, die mit v2-APIs erstellt wurden, sofern für diese [Speicherverschlüsselung](../previous/media-services-rest-storage-encryption.md) (AES-256) von Media Services bereitgestellt wurde.
    * Mit der älteren AES-256-[Speicherverschlüsselung](../previous/media-services-rest-storage-encryption.md) können Sie mithilfe von v3-APIs keine neuen Medienobjekte erstellen.
* Die v3-SDKs sind jetzt vom Storage-SDK entkoppelt, wodurch sich eine bessere Kontrolle über das zu verwendende Storage-SDK ergibt und Versionsprobleme vermieden werden. 
* In den v3-APIs werden alle Codierungsbitraten in Bits pro Sekunde angegeben. Dies unterscheidet sich von den Voreinstellungen von v2 Media Encoder Standard. Beispielsweise würde die Bitrate in v2 mit 128 (Kbit/s), in v3 jedoch mit 12.8000 (Bits/Sekunde) angegeben. 
* Entities AssetFiles, AccessPolicies und IngestManifests sind in v3 nicht vorhanden.
* ContentKeys ist jetzt keine Entität mehr, sondern eine Eigenschaft von StreamingLocator.
* Die Event Grid-Unterstützung ersetzt NotificationEndpoints.
* Die folgenden Entitäten wurden umbenannt:
    * JobOutput ersetzt Aufgabe und ist jetzt Teil eines Auftrags.
    * StreamingLocator ersetzt Locator.
    * LiveEvent ersetzt Kanal.
        
        Die LiveEvents-Abrechnung basiert auf Livekanal-Verbrauchseinheiten. Weitere Informationen finden Sie in der [Livestreaming-Übersicht](live-streaming-overview.md#billing) unter [Preise](https://azure.microsoft.com/pricing/details/media-services/).
    * LiveOutput ersetzt Programm.
* LiveOutputs müssen nicht explizit gestartet werden, sie werden bei der Erstellung gestartet und beim Löschen beendet. In den v2-APIs haben Programme anders funktioniert, sie mussten nach der Erstellung gestartet werden.

## <a name="feature-gaps-with-respect-to-v2-apis"></a>Featurelücken in Bezug auf v2-APIs

Die v3-API weist in Bezug auf die v2-API die folgenden Featurelücken auf. Am Schließen der Lücken wird derzeit gearbeitet.

* Der Zugriff auf den [Premium-Encoder](../previous/media-services-premium-workflow-encoder-formats.md) und die älteren [Media Analytics-Prozessoren](../previous/media-services-analytics-overview.md) (Azure Media Services Indexer 2 Preview, Face Redactor usw.) ist über v3 nicht möglich.

    Kunden, die eine Migration von der Media Indexer 1- oder 2-Vorschauversion durchführen möchten, können sofort die AudioAnalyzer-Voreinstellung in der v3-API verwenden.  Diese neue Voreinstellung enthält mehr Funktionen als die ältere Media Indexer 1- oder 2-Version. 

* Viele der erweiterten Features von Media Encoder Standard in v2-APIs sind derzeit in v3 nicht verfügbar, z.B.:
    * Ausschneiden (für On-Demand- und Live-Szenarien)
    * Zusammenfügen von Medienobjekten
    * Überlagerungen
    * Zuschneiden
    * Miniaturbild-Sprites
* LiveEvents mit Transcodierung unterstützen derzeit keine Slate-Einfügungen beim Streamen, benutzerdefinierte Voreinstellungen oder Werbemarkereinfügungen über API-Aufruf. 

> [!NOTE]
> Erstellen Sie ein Lesezeichen für diesen Artikel, und überprüfen Sie ihn regelmäßig auf Aktualisierungen.

## <a name="code-differences"></a>Codeunterschiede

Die folgende Tabelle zeigt die Codeunterschiede zwischen v2 und v3 für gängige Szenarien.

|Szenario|V2-API|V3-API|
|---|---|---|
|Erstellen eines Medienobjekts und Hochladen einer Datei |[Beispiel für v2-.NET](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L113)|[Beispiel für v3-.NET](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L169)|
|Übermitteln eines Auftrags|[Beispiel für v2-.NET](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L146)|[Beispiel für v3-.NET](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L298)<br/><br/>Zeigt, wie zuerst eine Transformation erstellt und dann ein Auftrag übermittelt wird.|
|Veröffentlichen eines Medienobjekts mit AES-Verschlüsselung |1. Erstellen von ContentKeyAuthorizationPolicyOption<br/>2. Erstellen von ContentKeyAuthorizationPolicy<br/>3. Erstellen von AssetDeliveryPolicy<br/>4. Erstellen eines Medienobjekts und Hochladen von Inhalten ODER Übermitteln eines Auftrags und Verwenden des Ausgabemedienobjekts<br/>5. Zuordnen von AssetDeliveryPolicy zu Medienobjekt<br/>6. Erstellen von ContentKey<br/>7. Zuordnen von ContentKey zu einem Medienobjekt<br/>8. Erstellen von AccessPolicy<br/>9. Erstellen von Locator<br/><br/>[Beispiel für v2-.NET](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L64)|1. Erstellen einer Richtlinie für Inhaltsschlüssel<br/>2. Erstellen eines Medienobjekts<br/>3. Hochladen von Inhalten oder Verwenden von Medienobjekt als JobOutput<br/>4. Erstellen von StreamingLocator<br/><br/>[Beispiel für v3-.NET](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs#L105)|

## <a name="known-issues"></a>Bekannte Probleme

* Derzeit können Sie das Azure-Portal nicht für die Verwaltung von v3-Ressourcen verwenden. Verwenden Sie die [REST-API](https://aka.ms/ams-v3-rest-sdk), CLI oder eines der unterstützten SDKs.
* Derzeit können reservierte Einheiten für Medien nur mithilfe der Media Services v2-API verwaltet werden. Weitere Informationen finden Sie unter [Übersicht über das Skalieren der Medienverarbeitung](../previous/media-services-scale-media-processing-overview.md).
* Mit der v3-API erstellte Media Services-Entitäten können von der v2-API nicht verwaltet werden.  
* Es wird nicht empfohlen, mit v2-APIs erstellte Entitäten über die v3-APIs zu verwalten. Es folgen Beispiele für die Unterschiede, aufgrund deren die Entitäten in zwei Versionen nicht kompatibel sind:   
    * In v2 erstellte Aufträge und Aufgaben werden in v3 nicht angezeigt, da sie keiner Transformation zugeordnet sind. Es wird empfohlen, zu v3-Transformationen und -Aufträge zu wechseln. Während des Wechsels müssen die aktiven v2-Aufträge für einen relativ kurzen Zeitraum überwacht werden.
    * Mit v2 erstellte Kanäle und Programme (die LiveEvents und LiveOutputs in v3 zugeordnet sind) können nicht mehr mit v3 verwaltet werden. Es wird empfohlen, zu einem günstigen Zeitpunkt (beim Beenden des Kanals) zu v3-LiveEvents und LiveOutputs zu wechseln.
    
        Derzeit können Sie kontinuierlich ausgeführte Kanäle nicht migrieren.  
> [!NOTE]
> Erstellen Sie ein Lesezeichen für diesen Artikel, und überprüfen Sie ihn regelmäßig auf Aktualisierungen.

## <a name="next-steps"></a>Nächste Schritte

Um zu erfahren, wie einfach es ist, mit der Codierung und dem Streaming von Videodateien zu beginnen, lesen Sie [Streamen von Dateien](stream-files-dotnet-quickstart.md). 

