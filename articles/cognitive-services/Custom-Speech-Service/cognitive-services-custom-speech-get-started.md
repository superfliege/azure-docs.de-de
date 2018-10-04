---
title: Erste Schritte mit Custom Speech Service
titlesuffix: Azure Cognitive Services
description: Abonnieren Sie Custom Speech Service, und verknüpfen Sie die Dienstaktivitäten mit einem Azure-Abonnement, um ein Modell zu trainieren und eine Bereitstellung durchzuführen.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: conceptual
ms.date: 02/08/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: ae72edd626bd91dea7cd2812a3ef821b905f59a4
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225241"
---
# <a name="get-started-with-custom-speech-service"></a>Erste Schritte mit Custom Speech Service

Machen Sie sich mit den wichtigsten Features von Custom Speech Service vertraut, und erfahren Sie, wie Sie Akustik- und Sprachmodelle für Ihre Einsatzzwecke erstellen, bereitstellen und verwenden können. Eine ausführlichere Dokumentation sowie detaillierte Anweisungen werden Ihnen nach Ihrer Registrierung im Custom Speech Services-Portal bereitgestellt.

## <a name="samples"></a>Beispiele  
Ein nützliches Beispiel, das Ihnen den Einstieg vereinfacht, finden Sie [hier](https://github.com/Microsoft/Cognitive-Custom-Speech-Service).

## <a name="prerequisites"></a>Voraussetzungen  

### <a name="subscribe-to-custom-speech-service-and-get-a-subscription-key"></a>Abonnieren von Custom Speech Service und Abrufen eines Abonnementschlüssels
Bevor Sie sich mit dem oben genannten Beispiel auseinander setzen, müssen Sie Custom Speech Service abonnieren und einen Abonnementschlüssel beziehen. Lesen Sie hierfür die Informationen unter [Abonnements](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/CustomSpeech) oder die [Erläuterungen](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-subscribe.md). In diesem Tutorial können sowohl der Primär- als auch der Sekundärschlüssel verwendet werden. Befolgen Sie die empfohlenen Vorgehensweisen, um Ihren API-Schlüssel sicher zu speichern.

### <a name="get-the-client-library-and-example"></a>Abrufen der Clientbibliothek und des Beispiels
Sie können über das [SDK](https://www.microsoft.com/cognitive-services/en-us/SDK-Sample?api=bing%20speech&category=sdk) eine Clientbibliothek und ein Beispiel herunterladen. Die heruntergeladene ZIP-Datei muss in einen Ordner Ihrer Wahl extrahiert werden. Viele Benutzer wählen hierfür den Visual Studio 2015-Ordner.

## <a name="creating-a-custom-acoustic-model"></a>Erstellen eines benutzerdefinierten Akustikmodells
Für die Anpassung des Akustikmodells an eine bestimmte Domäne ist eine Sammlung von Sprachdaten erforderlich. Diese Sammlung besteht aus einer Reihe von Audiodateien mit Sprachdaten sowie einer Textdatei mit Transkriptionen von jeder Audiodatei. Die Audiodaten sollten repräsentativ für das Szenario sein, in dem Sie das Erkennungsfeature einsetzen möchten.

Beispiel: Wenn Sie die Spracherkennung in einer lauten Werksumgebung verbessern möchten, sollten in den Audiodateien Personen zu hören sein, die in einer lauten Werksumgebung sprechen.
Wenn Sie dagegen die Leistung eines einzelnen Lautsprechers optimieren möchten, um z.B. alle informellen Gespräche zu transkribieren, sollten die Audiodateien dementsprechend viele Beispiele nur für diesen Lautsprecher enthalten.

Eine ausführliche Beschreibung zur Erstellung eines benutzerdefinierten Akustikmodells finden Sie [hier](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-acoustic-model.md).

## <a name="creating-a-custom-language-model"></a>Erstellen eines benutzerdefinierten Sprachmodells
Die Vorgehensweise zum Erstellen eines benutzerdefinierten Sprachmodells ist mit der Erstellung eines Akustikmodells vergleichbar, ausgenommen der Tatsache, dass nicht Audiodaten, sondern ausschließlich Texte Gegenstand der Analyse sind. Der Text sollte viele Beispiele für Abfragen oder Äußerungen umfassen, die Benutzer aller Wahrscheinlichkeit nach äußern würden oder die Sie in Ihrer Anwendung protokolliert (bzw. in diese eingegeben) haben.

Eine ausführliche Beschreibung zur Erstellung eines benutzerdefinierten Sprachmodells finden Sie [hier](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-language-model.md).

## <a name="creating-a-custom-speech-to-text-endpoint"></a>Erstellen eines benutzerdefinierten Spracherkennungsendpunkts
Wenn Sie benutzerdefinierte Akustik- und/oder Sprachmodelle erstellt haben, können diese in einem benutzerdefinierten Endpunkt für die Spracherkennung bereitgestellt werden. Um einen neuen benutzerdefinierten Endpunkt zu erstellen, klicken Sie oben auf der Seite im Menü „CRIS“ auf „Bereitstellungen“. Sie werden zu einer Tabelle mit dem Namen „Bereitstellungen“ der aktuellen benutzerdefinierten Endpunkte weitergeleitet. Wenn Sie noch keine Endpunkte erstellt haben, ist die Tabelle leer. Das aktuelle Gebietsschema ist im Tabellentitel angegeben. Wenn Sie eine Bereitstellung für eine andere Sprache erstellen möchten, klicken Sie auf „Gebietsschema ändern“. Weitere Informationen zu unterstützten Sprachen finden Sie im Abschnitt zum Ändern des Gebietsschemas.

Eine ausführliche Beschreibung zur Erstellung eines benutzerdefinierten Spracherkennungsendpunkts finden Sie [hier](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-endpoint.md).

## <a name="using-a-custom-speech-endpoint"></a>Verwendung eines benutzerdefinierten Spracherkennungsendpunkts
Anforderungen können in sehr ähnlicher Weise wie beim Spracherkennungsendpunkt von Azure Cognitive Services an den Spracherkennungsendpunkt des CRIS gesendet werden. Beachten Sie, dass diese Endpunkte funktionell mit den Standardendpunkten der Sprach-API identisch sind. Folglich steht die gleiche Funktionalität über die Clientbibliothek oder REST-API für die Sprach-API auch für Ihren benutzerdefinierten Endpunkt zur Verfügung.

Eine ausführliche Beschreibung zur Verwendung eines benutzerdefinierten Spracherkennungsendpunkts finden Sie [hier](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-use-endpoint.md).


Beachten Sie, dass die mit dem CRIS erstellten Endpunkte eine andere Anzahl gleichzeitiger Anforderungen verarbeiten können. Dies ist vom Tarif abhängig, dem das Abonnement zugeordnet ist. Wenn mehr Erkennungen erforderlich sind, wird der Fehlercode 429 (zu viele Anfragen) zurückgegeben. Weitere Informationen finden Sie auf der Seite mit den [Preisen](https://www.microsoft.com/cognitive-services/en-us/pricing). Darüber hinaus wird ein monatliches Kontingent von Anforderungen für den Free-Tarif bereitgestellt. Falls Sie im Free-Tarif über das oben genannte monatliche Kontingent hinaus auf Ihren Endpunkt zugreifen, gibt der Dienst den Fehlercode 403 (Verboten) zurück.

Bei der Verwendung des Diensts wird davon ausgegangen, dass Audiodaten in Echtzeit übertragen wird. Wenn Daten noch schneller gesendet werden, wird die Anforderung so lange ausgeführt, bis deren Dauer in Echtzeit verstrichen ist.

* [Übersicht](cognitive-services-custom-speech-home.md)
* [HÄUFIG GESTELLTE FRAGEN](cognitive-services-custom-speech-faq.md)
* [Glossar](cognitive-services-custom-speech-glossary.md)
