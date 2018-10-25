---
title: Migrieren von der Sprachübersetzungs-API zum Spracherkennungsdienst
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diesen Artikel, um Ihre Anwendungen von der Sprachübersetzungs-API zum Spracherkennungsdienst zu migrieren.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: Speech
ms.topic: article
ms.date: 10/01/2018
ms.author: aahi
ms.openlocfilehash: b50cdc6978519c0ec9da447c324237c00577d9fd
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2018
ms.locfileid: "48885271"
---
# <a name="migrate-from-the-translator-speech-api-to-the-speech-service"></a>Migrieren von der Sprachübersetzungs-API zum Spracherkennungsdienst

Verwenden Sie diesen Artikel, um Ihre Anwendungen von der Sprachübersetzungs-API von Microsoft zum [Spracherkennungsdienst](index.yml) zu migrieren. Diese Anleitung beschreibt die Unterschiede zwischen der Sprachübersetzungs-API von Microsoft und dem Spracherkennungsdienst und schlägt Strategien für die Migration Ihrer Anwendungen vor.

> [!NOTE]
> Ihr Abonnementschlüssel für die Sprachübersetzungs-API wird nicht vom Spracherkennungsdienst akzeptiert. Sie müssen ein neues Abonnement für den Spracherkennungsdienst starten.

## <a name="comparison-of-features"></a>Vergleich der Features

| Feature                                           | Sprachübersetzungs-API                                  | Spracherkennungsdienst | Details                                                                                                                                                                                                                                                                            |
|---------------------------------------------------|-----------------------------------------------------------------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Übersetzung in Text                               | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Übersetzung in Sprache                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Globaler Endpunkt                                   | :heavy_check_mark:                                              | :heavy_minus_sign:                 | Der Spracherkennungsdienst bietet derzeit keinen globalen Endpunkt. Ein globaler Endpunkt kann den Datenverkehr automatisch zum nächstgelegenen regionalen Endpunkt leiten, wodurch die Latenz in Ihrer Anwendung verringert wird.                                                    |
| Regionale Endpunkte                                | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Verbindungszeitlimit                             | 90 Minuten                                               | Mit SDK unbegrenzt 10 Minuten mit einer Websocketverbindung                                                                                                                                                                                                                                                                                   |
| Authentifizierungsschlüssel im Header                                | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Mehrere Sprachen in einer einzelnen Anforderung übersetzt | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| SDKs verfügbar                                    | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Informationen zu verfügbaren SDKs finden Sie in der [Dokumentation zum Spracherkennungsdienst](index.yml).                                                                                                                                                    |
| Websocketverbindungen                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Sprachen-API                                     | :heavy_check_mark:                                              | :heavy_minus_sign:                 | Der Spracherkennungsdienst unterstützt denselben Sprachumfang, der im Artikel [Sprachenreferenz für Sprachübersetzungs-API](../translator-speech/languages-reference.md) beschrieben ist. |
| Filter und Markierung für Obszönitäten                       | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| .WAV/PCM als Eingabe                                 | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Andere Dateitypen als Eingabe                         | :heavy_minus_sign:                                              | :heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Teilergebnisse                                   | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Informationen zur zeitlichen Steuerung                                       | :heavy_check_mark:                                              | :heavy_minus_sign:                 |                                                                                                                                                                 |
| Korrelations-ID                                    | :heavy_check_mark:                                              | :heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Custom Speech-Modelle                              | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Der Spracherkennungsdienst bietet benutzerdefinierte Sprachmodelle, mit denen Sie die Spracherkennung an das spezifische Vokabular Ihres Unternehmens anpassen können.                                                                                                                                           |
| Benutzerdefinierte Übersetzungsmodelle                         | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Wenn Sie die Textübersetzungs-API von Microsoft abonnieren, können Sie [Custom Translator](https://www.microsoft.com/translator/business/customization/) (derzeit in der Vorschauversion) verwenden, um Ihre eigenen Daten für präzisere Übersetzungen zu verwenden.                                                 |

## <a name="migration-strategies"></a>Migrationsstrategien

Wenn Sie oder Ihr Unternehmen Anwendungen in der Entwicklungs- oder Produktionsumgebung verwenden, die die Sprachübersetzungs-API verwenden, sollten Sie diese aktualisieren, um den Spracherkennungsdienst zu nutzen. Informationen zu verfügbaren SDKs, Codebeispielen und Tutorials finden Sie in der Dokumentation zum [Spracherkennungsdienst](index.yml). Nachfolgend sind einige Punkte aufgeführt, die bei der Migration zu beachten sind:

* Der Spracherkennungsdienst bietet derzeit keinen globalen Endpunkt. Sie müssen feststellen, ob Ihre Anwendung effizient funktioniert, indem Sie einen einzigen regionalen Endpunkt für den gesamten Datenverkehr verwenden. Wenn dies nicht der Fall ist, müssen Sie die Geolokalisierung verwenden, um den effizientesten Endpunkt zu bestimmen.

* Wenn Ihre Anwendung langlebige Verbindungen verwendet und nicht die verfügbaren SDKs verwenden kann, können Sie eine Websocketverbindung verwenden und das 10-minütige Zeitlimit verwalten, indem Sie sich zu den entsprechenden Zeiten erneut verbinden.

* Wenn Ihre Anwendung die Textübersetzungs-API und die Sprachübersetzungs-API verwendet, um benutzerdefinierte Übersetzungsmodelle zu ermöglichen, können Sie Kategorie-IDs direkt über den Spracherkennungsdienst hinzufügen.

* Der Spracherkennungsdienst kann Übersetzungen in mehrere Sprachen in einer einzigen Anforderung durchführen, im Gegensatz zur Sprachübersetzungs-API.

## <a name="next-steps"></a>Nächste Schritte

* [Kostenloses Testen des Spracherkennungsdiensts](get-started.md)
* [Schnellstart: Erkennen von Sprache in einer UWP-App mit dem Spracherkennungs-SDK](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Weitere Informationen

* [Worum handelt es sich beim Spracherkennungsdienst?](overview.md)
* [Spracherkennungsdienst und SDK: Dokumentation](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-qsg)
