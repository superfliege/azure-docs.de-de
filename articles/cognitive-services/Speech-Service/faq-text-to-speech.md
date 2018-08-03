---
title: Häufig gestellte Fragen zum Spracherkennungsdienst in Azure | Microsoft-Dokumentation
description: Hier erhalten Sie Antworten auf die am häufigsten gestellten Fragen zur Spracherkennung.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: 4a29435c0ace79fc3a5d3a5a42a0e91bdbc8da5e
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37082823"
---
# <a name="text-to-speech-frequently-asked-questions"></a>Häufig gestellte Fragen zur Sprachsynthese

Wenn Sie in diesen FAQs keine Antwort auf Ihre Frage finden, können Sie die Fragen unter [Stack Overflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) und [UserVoice](https://cognitive.uservoice.com/) an die Custom Speech Service-Community stellen.

## <a name="general"></a>Allgemein

**Frage**: Worin besteht der Unterschied zwischen standardmäßigen und benutzerdefinierten Stimmmodellen?

**Antwort**: Die standardmäßigen Stimmmodelle (auch als Voicefonts bekannt) wurden mit Daten von Microsoft trainiert und sind bereits in der Cloud verfügbar. Benutzerdefinierte Stimmmodelle ermöglichen es Benutzern, entweder ein vorhandenes durchschnittliches Modell zu verwenden und Klangfarbe und Sprachausdruck an den Stil der Sprecherstimme anzupassen oder ein vollständig neues Modell basierend auf vom Benutzer vorbereiteten Daten zu trainieren. Heute möchten immer mehr Kunden eine einmalige Stimme mit hohem Wiedererkennungswert für ihre Bots. Hierfür eignet sich die Plattform zum Erstellen benutzerdefinierter Stimmen ideal.

**Frage**: Wo fange ich an, wenn ich ein Stimmmodell verwenden möchte?

**Antwort**: Mehr als 80 Standardsprachmodelle in über 45 Sprachen sind über HTTP-Anforderungen verfügbar. Zunächst benötigen Sie einen [Abonnementschlüssel](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/get-started). Wenn Sie REST-Aufrufe an die vorab bereitgestellten Stimmmodelle senden möchten, lesen Sie [diese Details](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-apis#text-to-speech).

**Frage**: Wenn ich ein benutzerdefiniertes Stimmmodell verwenden möchte, ist die API die gleiche wie für die Standardstimmen?

**Antwort**: Wenn Sie das benutzerdefinierte Stimmmodell erstellt und bereitgestellt haben, erhalten Sie einen eindeutigen Endpunkt für Ihr Modell. Diesen Endpunkt müssen Sie in Ihren HTTP-Anforderungen angeben, damit diese Stimme für Sprachausgaben in Ihren Apps verwendet wird. Die gleiche Funktionalität, die über die REST-API für den Sprachsynthesedienst verfügbar ist, steht auch für Ihren benutzerdefinierten Endpunkt zur Verfügung. Weitere Informationen finden Sie unter [Erstellen und Verwenden eines benutzerdefinierten Endpunkts](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/how-to-customize-voice-font#create-and-use-a-custom-endpoint).

**Frage**: Muss ich beim Erstellen von benutzerdefinierten Stimmmodellen die Trainingsdaten selbst vorbereiten?

**Antwort**: Ja, Sie müssen die Trainingsdaten selbst erstellen. Zum Erstellen eines benutzerdefinierten Stimmmodells ist eine Sammlung von Sprachdaten erforderlich. Diese Sammlung besteht aus einer Reihe von Audiodateien mit Sprachaufnahmen sowie einer Textdatei mit der Transkription jeder Audiodatei. Die resultierende digitale Stimme hängt in hohem Maß von der Qualität der Trainingsdaten ab. Die Erstellung einer guten TTS-Stimme setzt voraus, dass die Aufnahmen in einem ruhigen Raum mit einem hochwertigen Standmikrofon erfolgen. Eine gleichmäßige Lautstärke, Sprechgeschwindigkeit und Tonhöhe sowie eine ausdrucksstarke und doch konsistente Prosodie sind bei der Erstellung einer angenehmen digitalen Stimme entscheidend. Es wird dringend empfohlen, die Stimmen in einem Tonstudio aufzuzeichnen.
Zurzeit bieten wir keine Unterstützung für Onlineaufnahmen oder Empfehlungen für Tonstudios. Informationen zu den Anforderungen an das Format finden Sie unter [Vorbereiten von Aufnahmen und Transkripten](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/how-to-customize-voice-font#prepare-recordings-and-transcripts).
 
**Frage**: Welche Skripts sollte ich zur Aufzeichnung der Sprachdaten für das benutzerdefinierte Stimmtraining verwenden? 

**Antwort**: Es gibt keinerlei Einschränkungen hinsichtlich der für Aufnahmen verwendeten Skripts. Sie können eigene Skripts für die Sprachaufnahmen verwenden. Stellen Sie nur sicher, dass eine ausreichende phonetische Abdeckung in den Sprachdaten gegeben ist. Um eine benutzerdefinierte Stimme zu trainieren, beginnen Sie mit einer geringen Menge an Sprachdaten – ungefähr 50 verschiedene Sätze (3-5 Minuten Spracheingabe). Je mehr Daten Sie bereitstellen, desto natürlicher klingt die Stimme. Sie können einen vollständigen Voicefont trainieren, wenn Sie Aufzeichnungen mit mehr als 2000 Sätzen bereitstellen (ca. 3-4 Stunden Spracheingabe). Um einen vollständigen Voicefont in sehr hoher Qualität zu erhalten, müssen Sie Aufzeichnungen von mehr als 6000 Sätzen erstellen (ca. 8-10 Stunden Spracheingabe).  
Wir bieten weitere Dienste, um Sie bei der Erstellung von Skripts für die Aufzeichnung zu unterstützen. Um diese Dienste anzufragen, wenden Sie sich an den [Custom Voice-Kundendienst](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation).

**Frage**: Was geschieht, wenn ich eine höhere Parallelität als den Standardwert oder den im Portal angebotenen Wert benötige?

**Antwort**: Sie können Ihr Modell in Schritten von 20 gleichzeitigen Anforderungen hochskalieren. Um eine höhere Skalierung anzufragen, wenden Sie sich an den [Custom Voice-Kundendienst](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation).

**Frage**: Kann ich mein Modell herunterladen und lokal ausführen?

**Antwort**: Modelle können nicht heruntergeladen und lokal ausgeführt werden.

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung](troubleshooting.md)
* [Anmerkungen zu dieser Version](releasenotes.md)
