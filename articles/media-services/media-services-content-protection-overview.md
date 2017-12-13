---
title: "Schützen Ihrer Inhalte mit Azure Media Services | Microsoft-Dokumentation"
description: "In diesem Artikel finden Sie eine Übersicht über die Content Protection mit Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 81bc00e1-dcda-4d69-b9ab-8768b793422b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 6475a865b9d1b263bd7cc68c99acdb5f6959531e
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2017
---
# <a name="protecting-content-overview"></a>Schützen von Inhalten – Übersicht
Microsoft Azure Media Services ermöglicht die Sicherung Ihrer Medien ab dem Zeitpunkt, an dem sie Ihren Computer durch Speicherung, Verarbeitung und Übermittlung verlassen. Mit Media Services können Sie zu übermittelnde Live- und On-Demand-Inhalte dynamisch mit Advanced Encryption Standard (AES-128) oder unter Verwendung eines der anderen drei DRM-Systeme (Microsoft PlayReady, Google Widevine und Apple FairPlay) verschlüsseln. Media Services bietet auch einen Dienst für die Übermittlung von AES-Schlüsseln und DRM-Lizenzen (PlayReady, Widevine und FairPlay) an autorisierte Clients. 

Die folgende Abbildung veranschaulicht den Workflow zum Schutz der Inhalte von Azure Media Services. 

![Schützen mit PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

In diesem Artikel werden relevante Konzepte und Begriffe für Content Protection mit AMS behandelt. Der Artikel enthält auch Links zu Artikeln, in denen es um den Schutz von Inhalten geht. 

## <a name="dynamic-encryption"></a>Dynamische Verschlüsselung
Mit Azure Media Services können Sie zu übermittelnde Inhalte dynamisch mit unverschlüsseltem AES-Schlüssel oder mit DRM-Verschlüsselung (Microsoft PlayReady, Google Widevine und Apple FairPlay) verschlüsseln. Zur Zeit können Sie die folgenden Streamingformate verschlüsseln: HLS, MPEG DASH und Smooth Streaming. Die Verschlüsselung für progressive Downloads wird nicht unterstützt. Jedes Verschlüsselungsverfahren unterstützt die folgenden Streamingprotokolle:
- AES: MPEG-DASH, Smooth Streaming und HLS
- PlayReady: MPEG-DASH, Smooth Streaming und HLS
- Widevine: MPEG-DASH
- FairPlay: HLS

Wenn Sie ein Medienobjekt verschlüsseln möchten, müssen Sie dem Medienobjekt einen Inhaltsschlüssel für die Verschlüsselung zuordnen und außerdem eine Autorisierungsrichtlinie für den Schlüssel konfigurieren. Inhaltsschlüssel können angegeben oder von Media Services automatisch generiert werden.

Außerdem müssen Sie die Übermittlungsrichtlinie des Medienobjekts konfigurieren. Wenn Sie ein speicherverschlüsseltes Medienobjekt streamen möchten, geben Sie durch Konfigurieren der Übermittlungsrichtlinie an, wie die Übermittlung erfolgen soll.

Wenn ein Player einen Datenstrom anfordert, verwendet Media Services den angegebenen Schlüssel, um den Inhalt dynamisch mit einem unverschlüsselten AES-Schlüssel oder mit DRM-Verschlüsselung zu verschlüsseln. Um den Stream zu entschlüsseln, fordert der Player den Schlüssel vom AMS-Schlüsselübermittlungsdienst an. Um zu entscheiden, ob der Benutzer berechtigt ist, den Schlüssel zu erhalten, wertet der Dienst die Autorisierungsrichtlinien aus, die Sie für den Schlüssel angegeben haben.

## <a name="aes-128-clear-key-vs-drm"></a>Unverschlüsselter Schlüssel mit AES-128 im Vergleich zu DRM
Kunden fragen sich oft, ob sie die AES-Verschlüsselung oder ein DRM-System verwenden sollten. Der Hauptunterschied zwischen der AES-Verschlüsselung und den DRM-Systemen besteht darin, dass bei der AES-Verschlüsselung der Inhaltsschlüssel unverschlüsselt an den Client übertragen wird. So kann der Schlüssel zur Verschlüsselung des Inhalts bei einer Netzwerkablaufverfolgung auf dem Client unverschlüsselt angezeigt werden. Der unverschlüsselte AES-128-Schlüssel eignet sich für Anwendungsfälle, in denen der Betrachter vertrauenswürdig ist (z. B. bei der Verschlüsselung von Unternehmensvideos, die innerhalb eines Unternehmens verteilt werden, um von Mitarbeitern wiedergegeben zu werden).

PlayReady, Widevine und FairPlay bieten alle eine höhere Verschlüsselungsstufe als die Verschlüsselung mit dem unverschlüsselten AES-128-Schlüssel. Der Inhaltsschlüssel wird in einem verschlüsselten Format übertragen. Zusätzlich wird die Entschlüsselung in einer sicheren Umgebung auf Betriebssystemebene durchgeführt, wo es für einen böswilligen Benutzer wesentlich schwieriger ist, Angriffe auszuführen. DRM wird für Anwendungsfälle empfohlen, in denen der Betrachter möglicherweise nicht vertrauenswürdig ist und Sie ein Höchstmaß an Sicherheit erfordern.

## <a name="storage-encryption"></a>Speicherverschlüsselung
Sie können die Speicherverschlüsselung verwenden, um Ihre unverschlüsselten Inhalte lokal mithilfe der AES-256-Bit-Verschlüsselung zu verschlüsseln und sie dann in Azure Storage hochzuladen, wo sie verschlüsselt im Ruhezustand gespeichert werden. Medienobjekte, die durch Speicherverschlüsselung geschützt sind, werden automatisch entschlüsselt, vor der Codierung in einem verschlüsselten Dateisystem platziert und optional vor dem Hochladen als neues Ausgabemedienobjekt erneut verschlüsselt. Der primäre Anwendungsfall für Storage Encryption ist, wenn Sie Ihre qualitativ hochwertigen Eingabemediendateien mit starker Verschlüsselung beim Speichern im Ruhezustand auf dem Datenträger sichern möchten.

Um ein speicherverschlüsseltes Medienobjekt zu übermitteln, müssen Sie die Übermittlungsrichtlinie des Medienobjekts konfigurieren und Media Services so mitteilen, wie die Inhalte bereitgestellt werden sollen. Bevor das Medienobjekt gestreamt werden kann, entschlüsselt und streamt der Streamingserver den Inhalt mithilfe der angegebenen Übermittlungsrichtlinie (z. B. AES, Common Encryption oder unverschlüsselt).

## <a name="types-of-encryption"></a>Arten der Verschlüsselung
PlayReady und Widevine nutzen Common Encryption (AES CTR-Modus). FairPlay nutzt die Verschlüsselung im AES-CBC-Modus. Die Verschlüsselung mit unverschlüsseltem AES-128-Schlüssel verwendet die Umschlagverschlüsselung.

## <a name="licenses-and-keys-delivery-service"></a>Dienst für die Übermittlung von Lizenzen und Schlüsseln
Media Services bietet einen Schlüsselübermittlungsdienst zum Übermitteln von DRM-Lizenzen (PlayReady, Widevine und FairPlay) und unverschlüsselten AES-Schlüsseln an autorisierte Clients. Zum Konfigurieren von Autorisierungs- und Authentifizierungsrichtlinien für Ihre Lizenzen und Schlüssel können Sie das [Azure-Portal](media-services-portal-protect-content.md), die REST-API oder das Media Services SDK für .NET verwenden.

## <a name="control-content-access"></a>Steuern des Zugriffs auf Inhalte
Sie können steuern, wer Zugriff auf Ihre Inhalte hat, indem Sie die Autorisierungsrichtlinie für Inhaltsschlüssel konfigurieren. Die Autorisierungsrichtlinie für Inhaltsschlüssel unterstützt entweder eine offene oder eine Tokenbeschränkung.

### <a name="open-authorization"></a>Offene Autorisierung
Bei einer offenen Autorisierungsrichtlinie wird der Inhaltsschlüssel an jeden Client gesendet (keine Einschränkung).

### <a name="token-authorization"></a>Tokenautorisierung
Bei einer Autorisierungsrichtlinie mit Tokenbeschränkung wird der Inhaltsschlüssel nur an einen Client gesendet, der ein gültiges JWT (JSON Web Token) oder SWT (Simple Web Token) in der Schlüssel-/Lizenzanforderung präsentiert. Dieses Token muss von einem Sicherheitstokendienst (Secure Token Service, STS) ausgestellt werden. Sie können Microsoft Active Directory als Sicherheitstokendienst verwenden oder einen benutzerdefinierten Sicherheitstokendienst bereitstellen. Der STS muss für die Erstellung eines mit dem angegebenen Schlüssel signierten Tokens und die Ausstellungsansprüche konfiguriert sein, die Sie in der Konfiguration der Tokeneinschränkung angegeben haben. Der Schlüsselübermittlungsdienst von Media Services gibt den angeforderten Schlüssel bzw. die Lizenz an den Client zurück, wenn das Token gültig ist und die Ansprüche im Token mit den für den Schlüssel bzw. die Lizenz konfigurierten Ansprüchen übereinstimmen.

Bei der Konfiguration der Richtlinie mit Token-Einschränkung müssen die Parameter PrimaryVerificationKey, Issuer und Audience angegeben werden. PrimaryVerificationKey enthält den Schlüssel, mit dem das Token signiert wurde, und Issuer ist der STS (Secure Token Service), von dem das Token ausgestellt wurde. „Audience“ (manchmal auch „Scope“) beschreibt den Verwendungszweck des Tokens oder die Ressource, auf die durch das Token Zugriff gewährt wird. Der Schlüsselübermittlungsdienst von Media Services überprüft, ob die Werte im Token mit den Werten in der Vorlage übereinstimmen.

## <a name="streaming-urls"></a>Streaming-URLs
Falls Ihr Asset mit mehreren DRM-Lösungen verschlüsselt wurde, empfiehlt es sich, in der Streaming-URL ein Verschlüsselungstag zu verwenden: (format='m3u8-aapl', encryption='xxx').

Es gelten die folgenden Bedingungen:
* Es kann höchstens ein Verschlüsselungstyp angegeben werden.
* Der Verschlüsselungstyp muss nicht in der URL angegeben werden, wenn auf das Medienobjekt nur eine einzelne Verschlüsselung angewendet wurde.
* Beim Verschlüsselungstyp wird die Groß-/Kleinschreibung nicht beachtet.
* Folgende Verschlüsselungstypen können angegeben werden:  
  * **cenc**: Für PlayReady oder Widevine (Common Encryption)
  * **cbcs-aapl**: Für FairPlay (AES-CBC-Verschlüsselung)
  * **cbc**: Für AES-Umschlagverschlüsselung (Envelope Encryption)

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln werden die nächsten Schritte beschrieben, um mit Content Protection zu beginnen:
* [Schutz durch Speicherverschlüsselung](media-services-rest-storage-encryption.md)
* [Schutz durch AES-Verschlüsselung](media-services-protect-with-aes128.md)
* [Schutz mit PlayReady und/oder Widevine](media-services-protect-with-playready-widevine.md)
* [Schutz mit FairPlay](media-services-protect-hls-with-FairPlay.md)

## <a name="related-links"></a>Verwandte Links
[Azure Media Services PlayReady license delivery pricing explained ("Erläuterung der Preisgestaltung bei der Azure Media Services PlayReady-Lizenzübermittlung", in englischer Sprache)](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[How to debug for AES encrypted stream in Azure Media Services ("Debuggen AES-verschlüsselter Streams in Azure Media Services", in englischer Sprache)](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[JWT-Tokenauthentifizierung](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integrieren einer Azure Media Services-OWIN MVC-basierten App in Azure Active Directory und Einschränken der Übermittlung von Inhaltsschlüsseln auf Grundlage von JWT-Ansprüchen](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/)

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
