---
title: Migrieren von der Bing-Spracheingabe zum Spracherkennungsdienst
titleSuffix: Azure Cognitive Services
description: Lernen Sie die Unterschiede zwischen Bing-Spracheingabe und dem Spracherkennungsdienst aus Entwicklersicht kennen und migrieren Sie Ihre Anwendung, um den Spracherkennungsdienst zu nutzen.
services: cognitive-services
author: wsturman
ms.service: cognitive-services
ms.component: Speech
ms.topic: article
ms.date: 10/01/2018
ms.author: gracez
ms.openlocfilehash: e72cf547ac911b22a03cae6032351c8c0f22de8e
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2018
ms.locfileid: "48884848"
---
# <a name="migrate-from-bing-speech-to-the-speech-service"></a>Migrieren von der Bing-Spracheingabe zum Spracherkennungsdienst

Verwenden Sie diesen Artikel, um Ihre Anwendungen von der Bing-Spracheingabe-API zum Spracherkennungsdienst zu migrieren.

Dieser Artikel beschreibt die Unterschiede zwischen den Bing-Spracheingabe-APIs und dem Spracherkennungsdienst und schlägt Strategien für die Migration Ihrer Anwendungen vor. Ihr Abonnementschlüssel für die Bing-Spracheingabe-API wird vom Spracherkennungsdienst nicht akzeptiert. Sie benötigen ein neues Abonnement für den Spracherkennungsdienst.

Mit einem einzelnen Abonnementschlüssel für den Speech-Dienst wird Zugriff auf die folgenden Features gewährt. Da die Abrechnung jeweils separat erfolgt, werden Ihnen nur die genutzten Features berechnet.

* [Spracherkennung](speech-to-text.md)
* [Benutzerdefinierte Spracherkennung](https://cris.ai)
* [Sprachsynthese](text-to-speech.md)
* [Benutzerdefinierte Stimmen für Sprachsynthese](how-to-customize-voice-font.md)
* [Sprachübersetzung](speech-translation.md) (ohne [Textübersetzung](../translator/translator-info-overview.md))

Das [Sprach-SDK](speech-sdk.md) ist ein funktionaler Ersatz für die Clientbibliotheken der Bing-Spracheingabe. Es verwendet aber eine andere API.

## <a name="comparison-of-features"></a>Vergleich der Features

Der Spracherkennungsdienst ist der Bing-Spracheingabe hinsichtlich Features, Plattform und Programmiersprache weitgehend ähnlich, weist aber die folgenden Unterschiede auf.

Feature | Bing-Spracheingabe | Spracherkennungsdienst | Details
-|-|-|-
C++ SDK | :heavy_minus_sign: | :heavy_check_mark: | Der Spracherkennungsdienst unterstützt Windows und Linux
Java-SDK | :heavy_check_mark: | :heavy_check_mark: | Der Spracherkennungsdienst unterstützt Android- und Spracherkennungsgeräte
C# SDK | :heavy_check_mark: | :heavy_check_mark: | Der Spracherkennungsdienst unterstützt Windows 10, UWP und .NET Standard 2.0
Kontinuierliche Spracherkennung | 10 Minuten | Unbegrenzt (mit SDK) | Die WebSockets-Protokolle der Bing-Spracheingabe als auch des Spracherkennungsdiensts unterstützen bis zu 10 Minuten pro Aufruf. Das Spracherkennungs-SDK verbindet sich jedoch bei einem Timeout oder wenn die Verbindung getrennt wird automatisch erneut.
Teil- oder Zwischenergebnisse | :heavy_check_mark: | :heavy_check_mark: | Mit WebSockets-Protokoll oder SDK
Benutzerdefinierte Spracherkennungsmodelle | :heavy_check_mark: | :heavy_check_mark: | Die Bing-Spracheingabe erfordert ein separates Abonnement für die benutzerdefinierte Spracherkennung
Benutzerdefinierte Voicefonts | :heavy_check_mark: | :heavy_check_mark: | Die Bing-Spracheingabe erfordert ein separates Abonnement für Custom Voice
24-KHz-Stimmen | :heavy_minus_sign: | :heavy_check_mark: 
Erkennung der Absicht bei gesprochenen Inhalten | Erfordert separaten LUIS-API-Aufruf | Integriert (mit SDK) |  Ein LUIS-Schlüssel kann mit dem Spracherkennungsdienst verwendet werden.
Einfache Absichtserkennung | :heavy_minus_sign: | :heavy_check_mark: 
Batch-Transkription bei langen Audiodateien | :heavy_minus_sign: | :heavy_check_mark:
Erkennungsmodus | Manuell über den Endpunkt-URI | Automatisch | Der Erkennungsmodus ist im Spracherkennungsdienst nicht verfügbar
Endpunktregion | Global | Länderspezifisch | Regionale Endpunkte verbessern die Latenz. Ein globaler Endpunkt wird für den Spracherkennungsdienst in Betracht gezogen.
REST-APIs | :heavy_check_mark: | :heavy_check_mark: | Die Spracherkennungsdienst-REST-API ist kompatibel mit der Bing-Spracheingabe (unterschiedlicher Endpunkt). REST-APIs unterstützen Sprachsynthese- und eingeschränkte Spracherkennungsfunktionen.
WebSockets-Protokolle | :heavy_check_mark: | :heavy_check_mark: | Die Spracherkennungsdienst-WebSockets-API ist kompatibel mit der Bing-Spracheingabe (unterschiedlicher Endpunkt). Migrieren Sie nach Möglichkeit zum Spracherkennungs-SDK, um Ihren Code zu vereinfachen.
Dienst-zu-Dienst-API-Aufrufe | :heavy_check_mark: | :heavy_minus_sign: | Wird bei der Bing-Spracheingabe über die C#-Dienstbibliothek bereitgestellt. 
Open Source-SDK | :heavy_check_mark: | :heavy_minus_sign: |

Der Spracherkennungsdienst verwendet ein zeitbasiertes Preismodell (anstelle eines transaktionsbasierten Modells). Ausführlichere Informationen finden Sie unter [Cognitive Services-Preise – Sprachdienste](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

## <a name="migration-strategies"></a>Migrationsstrategien

Wenn Sie oder Ihr Unternehmen Anwendungen in der Entwicklungs- oder Produktionsumgebung verwenden, die eine Bing-Spracheingabe-API verwenden, sollten Sie diese aktualisieren, um den Spracherkennungsdienst so schnell wie möglich zu nutzen. Informationen zu verfügbaren SDKs, Codebeispielen und Tutorials finden Sie in der [Dokumentation zum Spracherkennungsdienst](index.yml).

Die Spracherkennungsdienst-[REST-APIs](rest-apis.md) sind mit den Bing-Spracheingabe-APIs kompatibel. Wenn Sie derzeit die REST-APIs der Bing-Spracherkennung verwenden, müssen Sie nur den REST-Endpunkt ändern und zu einem Abonnementschlüssel des Spracherkennungsdiensts wechseln.

Die WebSockets-Protokolle des Spracherkennungsdiensts sind auch mit den Protokollen der Bing-Spracheingabe kompatibel. Wir empfehlen, dass neue Entwicklungen auf das Spracherkennungsdienst-SDK ausgerichtet sind, anstatt WebSockets zu verwenden. Zudem empfehlen wird Ihnen, bestehenden Code ebenfalls für das SDK zu migrieren. Wie bei den REST-APIs erfordert jedoch bestehender Code, der die Bing-Spracheingabe via WebSockets verwendet, nur eine Änderung des Endpunkts und einen aktualisierten Schlüssel.

Wenn Sie eine Bing-Spracheingabe-Clientbibliothek für eine bestimmte Programmiersprache verwenden, erfordert die Migration auf das [Spracherkennungs-SDK](speech-sdk.md) Änderungen an Ihrer Anwendung, da es sich um eine andere API handelt. Das Spracherkennungs-SDK kann Ihren Code einfacher gestalten und Ihnen gleichzeitig Zugang zu neuen Features geben.

Das Spracherkennungs-SDK unterstützt derzeit C# (Windows 10, UWP, .NET Standard), Java (Android und benutzerdefinierte Geräte), Objective C (iOS), C++ (Windows und Linux) und JavaScript. APIs sind auf den verschiedenen Plattformen ähnlich, was die Entwicklung für mehrere Plattformen erleichtert.

Spracherkennungsdienst bietet derzeit keinen globalen Endpunkt. Sie müssen feststellen, ob Ihre Anwendung effizient funktioniert, indem Sie einen einzigen regionalen Endpunkt für den gesamten Datenverkehr verwenden. Wenn nicht, verwenden Sie die Geolokalisierung, um den effizientesten Endpunkt zu bestimmen. Sie benötigen für jede verwendete Region ein separates Spracherkennungsdienst-Abonnement.

Wenn Ihre Anwendung langlebige Verbindungen verwendet und kein verfügbares SDK verwenden kann, können Sie eine WebsSockets-Verbindung verwenden und das 10-minütige Zeitlimit verwalten, indem Sie sich zu den entsprechenden Zeiten erneut verbinden.

Erste Schritte mit dem Spracherkennungs-SDK:

1. Laden Sie das [Spracherkennungs-SDK](speech-sdk.md) herunter.
1. Lesen Sie die [Schnellstartanleitungen](quickstart-csharp-dotnet-windows.md) und [Tutorials](how-to-recognize-intents-from-speech-csharp.md) des Spracherkennungsdiensts und betrachten Sie sich die [Codebeispiele](samples.md) an, um Erfahrungen mit den neuen APIs zu sammeln.
1. Aktualisieren Sie Ihre Anwendung, um den Spracherkennungsdienst und die APIs zu verwenden.

## <a name="support"></a>Support

Kunden der Bing-Spracheingabe sollten sich an den Kundendienst wenden, indem sie ein [Supportticket](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) öffnen. Sie können sich auch an uns wenden, wenn Ihr Supportbedarf einen [technischen Supportplan](https://azure.microsoft.com/support/plans/) erfordert.

Informationen zur Unterstützung von Spracherkennungsdienst, SDK und API finden Sie auf der [Supportseite](support.md) des Spracherkennungsdiensts.

## <a name="next-steps"></a>Nächste Schritte

* [Kostenloses Testen des Spracherkennungsdiensts](get-started.md)
* [Schnellstart: Erkennen von Sprache in einer UWP-App mit dem Spracherkennungsdienst-SDK](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Weitere Informationen
* [Spracherkennungsdienst: Anmerkungen zu dieser Version](releasenotes.md)
* [Worum handelt es sich beim Spracherkennungsdienst?](overview.md)
* [Spracherkennungsdienst und SDK: Dokumentation](speech-sdk.md#get-the-sdk)
