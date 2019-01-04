---
title: 'Schützen Ihrer Inhalte mit Media Services: Azure | Microsoft-Dokumentation'
description: In diesem Artikel finden Sie eine Übersicht über die Content Protection mit Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: cb7e867ea4304cf8b8741eac183e60d325c752c7
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53141848"
---
# <a name="content-protection-overview"></a>Übersicht über den Inhaltsschutz

Mit Azure Media Services können Sie Ihre Medien ab dem Zeitpunkt, an dem sie Ihren Computer verlassen, während des gesamten Prozesses der Speicherung, Verarbeitung und Übermittlung sichern. Mit Media Services können Sie Ihre zu übermittelnden Live- und On-Demand-Inhalte dynamisch mit Advanced Encryption Standard (AES-128) oder einem der drei wichtigsten DRM-Systeme verschlüsseln: Microsoft PlayReady, Google Widevine und Apple FairPlay. Media Services bietet auch einen Dienst für die Übermittlung von AES-Schlüsseln und DRM-Lizenzen (PlayReady, Widevine und FairPlay) an autorisierte Clients. 

Die folgende Abbildung veranschaulicht den Media Services-Workflow zum Schutz von Inhalten: 

![Inhalt schützen](./media/content-protection/content-protection.png)

&#42; *Die dynamische Verschlüsselung unterstützt AES-128 mit unverschlüsseltem Schlüssel, CBCS und CENC. Details finden Sie [hier](#streaming-protocols-and-encryption-types) in der Unterstützungsmatrix.*

Dieser Artikel erläutert die relevanten Konzepte und Begriffe für den Inhaltsschutz mit Media Services. Außerdem enthält er den Abschnitt [Häufig gestellte Fragen](#faq) und Links zu Artikeln, in denen es um den Schutz von Inhalten geht. 

## <a name="main-components-of-the-content-protection-system"></a>Hauptkomponenten des Inhaltsschutzsystems

Für einen erfolgreichen Entwurf Ihres Inhaltsschutzsystems oder Ihrer Inhaltsschutzanwendung müssen Sie mit dem vollständigen Umfang und Aufwand vertraut sein. Die folgende Aufstellung bietet eine Übersicht über drei Komponenten, die Sie implementieren müssen. 

1. Azure Media Services-Code
  
  * Lizenzvorlagen für PlayReady, Widevine und/oder FairPlay. Mit den Vorlagen können Sie die Rechte und Berechtigungen für die einzelnen verwendeten DRMs konfigurieren.
  * Autorisierung zur Lizenzbereitstellung, die die Logik der Autorisierungsprüfung basierend auf Ansprüchen in JWT angibt
  * Inhaltsschlüssel, Streamingprotokolle und entsprechende angewandte DRMs, die die DRM-Verschlüsselung definieren

  > [!NOTE]
  > Sie können jedes Medienobjekt mit mehreren Verschlüsselungstypen (AES-128, PlayReady, Widevine, FairPlay) verschlüsseln. Informationen zu sinnvollen Kombinationen finden Sie unter [Streamingprotokolle und Verschlüsselungstypen](#streaming-protocols-and-encryption-types).
  
  Im folgenden Artikel sind die Schritte zum Verschlüsseln von Inhalten mit AES und/oder DRM beschrieben: 
  
  * [Schutz durch AES-Verschlüsselung](protect-with-aes128.md)
  * [Schutz mit DRM](protect-with-drm.md)

2. Player mit AES- oder DRM-Client. Eine Videoplayer-App basierend auf einem Player-SDK (nativ oder browserbasiert) muss die folgenden Anforderungen erfüllen:
  * Das Player-SDK unterstützt die erforderlichen DRM-Clients.
  * Das Player-SDK unterstützt die erforderlichen Streamingprotokolle: Smooth, DASH und/oder HLS.
  * Das Player-SDK muss die Übergabe eines JWT-Token in die Lizenzerwerbsanforderung verarbeiten können.
  
    Sie können mithilfe der [Azure Media Player-API](http://amp.azure.net/libs/amp/latest/docs/) einen Player erstellen. Verwenden Sie die [ProtectionInfo-API von Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/), um anzugeben, welche DRM-Technologie auf unterschiedlichen DRM-Plattformen verwendet werden soll.

    Zum Testen von mit AES oder CENC (Widevine und/oder PlayReady) verschlüsselten Inhalten können Sie [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) verwenden. Stellen Sie sicher, dass Sie auf „Erweiterte Optionen“ klicken und Ihre Verschlüsselungsoptionen angeben.

    Wenn Sie mit FairPlay verschlüsselte Inhalte testen möchten, verwenden Sie [diesen Testplayer](https://aka.ms/amtest). Der Player unterstützt Widevine-, PlayReady- und FairPlay-DRMs sowie die AES-128-Verschlüsselung mit unverschlüsselten Schlüsseln. Sie müssen den richtigen Browser zum Testen der verschiedenen DRMs auswählen: Chrome/Opera/Firefox für Widevine, MS Edge/IE11 für PlayReady, Safari unter macOS für FairPlay.

3. Sicherheitstokendienst (STS), der JSON Web Token (JWT) als Zugriffstoken für den Zugriff auf die Back-End-Ressource ausgibt. Sie können die AMS-Lizenzbereitstellungsdienste als Back-End-Ressource verwenden. Ein Sicherheitstokendienst muss Folgendes definieren:

  * Aussteller und Zielgruppe (oder Gültigkeitsbereich)
  * Ansprüche, die von Geschäftsanforderungen für den Inhaltsschutz abhängen
  * Symmetrische oder asymmetrische Überprüfung für die Signaturüberprüfung
  * Unterstützung des Schlüsselrollovers (sofern erforderlich)

    Sie können [dieses STS-Tool](https://openidconnectweb.azurewebsites.net/DRMTool/Jwt) verwenden, um den Sicherheitstokendienst zu testen. Das Tool unterstützt alle drei Typen von Verifizierungsschlüssel: symmetrisch, asymmetrisch oder AAD mit Schlüsselrollover. 

> [!NOTE]
> Es wird dringend empfohlen, jede Komponente umfassend einzurichten und vollständig zu testen (siehe Beschreibung weiter oben), bevor Sie mit der nächsten Komponente fortfahren. Verwenden Sie zum Testen Ihres Inhaltsschutzsystems die in der Aufstellung oben angegebenen Tools.  

## <a name="streaming-protocols-and-encryption-types"></a>Streamingprotokolle und Verschlüsselungstypen

Mit Media Services können Sie Ihre zu übermittelnden Inhalte dynamisch mit unverschlüsselten AES-Schlüsseln oder per DRM-Verschlüsselung über Microsoft PlayReady, Google Widevine oder FairPlay verschlüsseln. Zurzeit können Sie die Streamingformate HTTP Live Streaming (HLS), MPEG DASH und Smooth Streaming verschlüsseln. Jedes Protokoll unterstützt die folgenden Verschlüsselungsmethoden:

|Protokoll|Containerformat|Verschlüsselungsschema|
|---|---|---|---|
|MPEG-DASH|Alle|AES|
||CSF(fmp4) |CENC (Widevine + PlayReady) |
||CMAF(fmp4)|CENC (Widevine + PlayReady)|
|HLS|Alle|AES|
||MPG2-TS |CBCS (FairPlay) |
||MPG2-TS |CENC (PlayReady) |
||CMAF(fmp4) |CENC (PlayReady) |
|Smooth Streaming|fMP4|AES|
||fMP4 | CENC (PlayReady) |

## <a name="dynamic-encryption"></a>Dynamische Verschlüsselung

In Media Services v3 ist StreamingLocator ein Inhaltsschlüssel zugeordnet (siehe [dieses Beispiel](protect-with-aes128.md)). Bei Verwendung des Media Services-Schlüsselübermittlungsdiensts sollten Sie den Inhaltsschlüssel automatisch generieren. Sie sollten den Inhaltsschlüssel selbst generieren, wenn Sie Ihren eigenen Schlüsselbereitstellungsdienst verwenden oder wenn Sie ein Hochverfügbarkeitsszenario verwenden, für das Sie denselben Inhaltsschlüssel in zwei Rechenzentren benötigen.

Wenn ein Player einen Stream anfordert, verwendet Media Services den angegebenen Schlüssel, um den Inhalt dynamisch mit einem unverschlüsselten AES-Schlüssel oder per DRM-Verschlüsselung zu verschlüsseln. Um den Stream zu entschlüsseln, fordert der Player den Schlüssel vom Media Services-Schlüsselübermittlungsdienst oder dem von Ihnen angegebenen Schlüsselübermittlungsdienst an. Um zu entscheiden, ob der Benutzer berechtigt ist, den Schlüssel zu erhalten, wertet der Dienst die Richtlinie für Inhaltsschlüssel aus, die Sie für den Schlüssel angegeben haben.

## <a name="aes-128-clear-key-vs-drm"></a>Unverschlüsselter AES-128-Schlüssel im Vergleich zu DRM

Kunden fragen sich oft, ob sie die AES-Verschlüsselung oder ein DRM-System verwenden sollten. Der Hauptunterschied zwischen den beiden Systemen besteht darin, dass bei der AES-Verschlüsselung der Inhaltsschlüssel in unverschlüsseltem Format (als Klartext) an den Client übertragen wird. So kann der Schlüssel zur Verschlüsselung des Inhalts in einer Netzwerkablaufverfolgung auf dem Client als unverschlüsselter Text angezeigt werden. Die Verschlüsselung mit einem unverschlüsselten AES-128-Schlüssel eignet sich für Anwendungsfälle, in denen der Betrachter vertrauenswürdig ist (z.B. bei Unternehmensvideos, die innerhalb eines Unternehmens verteilt und von Mitarbeitern angesehen werden).

PlayReady, Widevine und FairPlay bieten alle eine höhere Verschlüsselungsstufe als die Verschlüsselung mit dem unverschlüsselten AES-128-Schlüssel. Der Inhaltsschlüssel wird in einem verschlüsselten Format übertragen. Zusätzlich erfolgt die Entschlüsselung in einer sicheren Umgebung auf Betriebssystemebene, wo ein Angriff durch einen böswilligen Benutzer schwieriger auszuführen ist. DRM wird für Anwendungsfälle empfohlen, in denen der Betrachter möglicherweise nicht vertrauenswürdig ist und Sie ein Höchstmaß an Sicherheit benötigen.

## <a name="storage-side-encryption"></a>Speicherseitige Verschlüsselung

Zum Schutz Ihrer im Ruhezustand befindlichen Ressourcen sollten die Ressourcen durch die speicherseitige Verschlüsselung verschlüsselt werden. Die folgende Tabelle zeigt, wie die speicherseitige Verschlüsselung in Media Services v3 funktioniert:

|Verschlüsselungsoption|BESCHREIBUNG|Media Services v3|
|---|---|---|---|
|Media Services-Speicherverschlüsselung| AES-256-Verschlüsselung, Schlüssel von Media Services verwaltet|Nicht unterstützt<sup>(1)</sup>|
|[Speicherdienstverschlüsselung für ruhende Daten](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Durch Azure Storage angebotene serverseitige Verschlüsselung – Schlüssel wird von Azure oder vom Kunden verwaltet|Unterstützt|
|[Clientseitige Storage-Verschlüsselung](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Durch Azure Storage angebotene clientseitige Verschlüsselung – Schlüssel wird vom Kunden in Key Vault verwaltet|Nicht unterstützt|

<sup>1</sup> In Media Services v3 wird Speicherverschlüsselung (AES-256-Verschlüsselung) nur für die Abwärtskompatibilität unterstützt, wenn Ihre Ressourcen mit Media Services v2 erstellt wurden. Dies bedeutet, dass v3 mit vorhandenen speicherverschlüsselten Ressourcen funktioniert, jedoch nicht die Erstellung neuer zulässt.

## <a name="licenses-and-keys-delivery-service"></a>Dienst für die Übermittlung von Lizenzen und Schlüsseln

Media Services bietet einen Schlüsselübermittlungsdienst zum Übermitteln von DRM-Lizenzen (PlayReady, Widevine und FairPlay) und unverschlüsselten AES-Schlüsseln an autorisierte Clients. Zum Konfigurieren von Autorisierungs- und Authentifizierungsrichtlinien für Ihre Lizenzen und Schlüssel können Sie die REST-API oder eine Media Services-Clientbibliothek verwenden.

## <a name="control-content-access"></a>Steuern des Zugriffs auf Inhalte

Sie können steuern, wer Zugriff auf Ihre Inhalte hat, indem Sie die Richtlinie für Inhaltsschlüssel konfigurieren. Media Services unterstützt mehrere Möglichkeiten zur Authentifizierung von Benutzern, die Schlüssel anfordern. Sie müssen die Richtlinie für Inhaltsschlüssel konfigurieren. Der Client (Player) muss die Richtlinie erfüllen, bevor der Schlüssel an den Client übermittelt werden kann. Die Richtlinie für Inhaltsschlüssel kann eine **offene** oder eine **Tokeneinschränkung** enthalten. 

Bei einer Richtlinie mit Tokeneinschränkung wird der Inhaltsschlüssel nur an einen Client gesendet, der in der Schlüssel-/Lizenzanforderung ein gültiges JWT (JSON Web Token) oder SWT (Simple Web Token) präsentiert. Dieses Token muss von einem Sicherheitstokendienst ausgestellt werden. Sie können Azure Active Directory als Sicherheitstokendienst verwenden oder einen benutzerdefinierten Sicherheitstokendienst bereitstellen. Der STS muss für die Erstellung eines mit dem angegebenen Schlüssel signierten Tokens und die Ausstellungsansprüche konfiguriert sein, die Sie in der Konfiguration der Tokeneinschränkung angegeben haben. Der Schlüsselübermittlungsdienst von Media Services gibt den angeforderten Schlüssel bzw. die Lizenz an den Client zurück, wenn das Token gültig ist und die Ansprüche im Token mit den für den Schlüssel bzw. die Lizenz konfigurierten Ansprüchen übereinstimmen.

Bei der Konfiguration der Richtlinie mit Tokeneinschränkung müssen die Parameter für den primären Verifizierungsschlüssel (primary verification key), den Aussteller (issuer) und die Zielgruppe (audience) angegeben werden. Der primäre Verifizierungsschlüssel enthält den Schlüssel, mit dem das Token signiert wurde. Der Aussteller ist der Sicherheitstokendienst, der das Token ausstellt. „Audience“ (manchmal auch „Scope“) beschreibt den Verwendungszweck des Tokens oder die Ressource, auf die durch das Token Zugriff gewährt wird. Der Schlüsselübermittlungsdienst von Media Services überprüft, ob die Werte im Token mit den Werten in der Vorlage übereinstimmen.

## <a name="a-idfaqfrequently-asked-questions"></a><a id="faq"/>Häufig gestellte Fragen

### <a name="question"></a>Frage

Wie kann ich ein Multi-DRM-System (PlayReady, Widevine und FairPlay) mit Azure Media Services (AMS) v3 implementieren und auch die AMS-Lizenz/den AMS-Schlüsselbereitstellungsdienst nutzen?

### <a name="answer"></a>Antwort

Ein End-to-End-Szenario finden Sie in [diesem Codebeispiel](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs). 

Das Beispiel veranschaulicht die folgenden Schritte:

1. Erstellen und konfigurieren Sie ContentKeyPolicies.

  Das Beispiel enthält Funktionen, die [PlayReady](playready-license-template-overview.md)-, [Widevine](widevine-license-template-overview.md)- und [FairPlay](fairplay-license-overview.md)-Lizenzen konfigurieren.

    ```
    ContentKeyPolicyPlayReadyConfiguration playReadyConfig = ConfigurePlayReadyLicenseTemplate();
    ContentKeyPolicyWidevineConfiguration widevineConfig = ConfigureWidevineLicenseTempate();
    ContentKeyPolicyFairPlayConfiguration fairPlayConfig = ConfigureFairPlayPolicyOptions();
    ```

2. Erstellen Sie einen StreamingLocator, der so konfiguriert ist, dass er ein verschlüsseltes Objekt streamen kann. 

  Sie können z. B. „StreamingLocator.StreamingPolicyName“ auf die Richtlinie „Predefined_MultiDrmCencStreaming“ festlegen. Diese Richtlinie gibt an, dass zwei Inhaltsschlüssel (Umschlag und CENC) generiert und für den Locator festgelegt werden sollen. Daher werden die Umschlag-, PlayReady- und Widevine-Verschlüsselungen angewendet (der Schlüssel wird dem Client basierend auf den konfigurierten DRM-Lizenzen bereitgestellt). Wenn Sie den Stream auch mit CBCS (FairPlay) verschlüsseln möchten, verwenden Sie „Predefined_MultiDrmStreaming“.

3. Erstellen Sie einen Testtoken.

  Die **GetTokenAsync**-Methode zeigt, wie Sie einen Testtoken erstellen.
  
4. Erstellen Sie die Streaming-URL.

  Die **GetDASHStreamingUrlAsync**-Methode zeigt, wie Sie die Streaming-URL erstellen. In diesem Fall streamt die URL den **DASH**-Inhalt.

### <a name="question"></a>Frage

Wie und wo kann ich JWT-Token abrufen, um damit dann eine Lizenz oder einen Schlüssel anzufordern?

### <a name="answer"></a>Antwort

1. Für die Produktion benötigen Sie einen Sicherheitstokendienst (STS) (Webdienst), der bei einer HTTPS-Anforderung JWT-Token ausgibt. Zum Testen können Sie den in der Methode **GetTokenAsync** angegebenen Code verwenden, der in [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs) definiert ist.
2. Player müssen nach der Authentifizierung eines Benutzers beim STS ein solches Token anfordern und dieses als Wert des Token zuweisen. Sie können die [Azure Media Player-API](https://amp.azure.net/libs/amp/latest/docs/) verwenden.

* Ein Beispiel für die Ausführung des STS mit einem symmetrischen und einem asymmetrischen Schlüssel finden Sie unter [http://aka.ms/jwt](https://aka.ms/jwt). 
* Ein Beispiel für einen Player, der auf dem Azure Media Player basiert und ein solches JWT-Token verwendet, finden Sie unter [http://aka.ms/amtest](https://aka.ms/amtest) (erweitern Sie den Link „player_settings“, um die Tokeneingabe anzusehen).

### <a name="question"></a>Frage

Wie autorisiere ich Anforderungen zum Streamen von Videos mit der AES-Verschlüsselung?

### <a name="answer"></a>Antwort

Der richtige Ansatz ist die Nutzung von STS:

Fügen Sie in STS je nach Benutzerprofil unterschiedliche Ansprüche hinzu (z. B. „Premiumbenutzer“, „Standardbenutzer“, „Benutzer der kostenlosen Testversion). Bei unterschiedlichen Ansprüchen in einem JWT kann der Benutzer unterschiedliche Inhalte sehen. Selbstverständlich hat ContentKeyPolicyRestriction für unterschiedliche Inhalte/Objekte die entsprechenden RequiredClaims.

Verwenden Sie die Azure Media Services-APIs für die Konfiguration von Lizenzen/Schlüsselbereitstellungen und die Verschlüsselung Ihrer Objekte (siehe [dieses Beispiel](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs).

## <a name="next-steps"></a>Nächste Schritte

Schauen Sie sich die folgenden Artikel an:

  * [Schutz durch AES-Verschlüsselung](protect-with-aes128.md)
  * [Schutz mit DRM](protect-with-drm.md)

Zusätzliche Informationen finden Sie unter [Entwickeln eines Systems mit Inhaltsschutz mit Multi-DRM und Zugriffssteuerung](design-multi-drm-system-with-access-control.md).


