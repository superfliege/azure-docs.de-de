---
title: Übersicht über Custom Speech Service in Azure | Microsoft Docs
description: Custom Speech Service ist ein cloudbasierter Dienst, der Benutzern das Anpassen der Sprachmodelle für die Sprache-in-Text-Transkription ermöglicht.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/07/2017
ms.author: panosper
ms.openlocfilehash: 97eee2b6440dbbf740ad5fa856bd518facabbfef
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49342295"
---
# <a name="what-is-custom-speech-service"></a>Worum handelt es sich bei Custom Speech Service?

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Custom Speech Service ist ein cloudbasierter Dienst, der Benutzern die Möglichkeit bietet, die Sprachmodelle für die Sprache-in-Text-Transkription anzupassen.
Wenn Sie Custom Speech Service verwenden möchten, finden Sie weitere Informationen im [Custom Speech Service-Portal](https://cris.ai).

Mit Custom Speech Service können Sie maßgeschneiderte Sprachmodelle und Akustikmodelle erstellen, die auf Ihre Anwendung und Ihre Benutzer zugeschnitten sind. Indem Sie Ihre spezifischen Sprach- und/oder Textdaten in Custom Speech Service hochladen, können Sie benutzerdefinierte Modelle erstellen, die in Verbindung mit den vorhandenen modernen Sprachmodellen von Microsoft verwendet werden können.

Wenn Sie beispielsweise einem Mobiltelefon, einem Tablet oder einer PC-App Sprachinteraktion hinzufügen, können Sie ein benutzerdefiniertes Sprachmodell erstellen, das mit dem Akustikmodell von Microsoft kombiniert werden kann, um einen speziell für Ihre App entwickelten Sprache-in-Text-Endpunkt zu erstellen. Wenn Ihre Anwendung für den Einsatz in einer bestimmten Umgebung oder durch eine bestimmte Benutzergruppe konzipiert ist, können Sie mit diesem Dienst auch ein benutzerdefiniertes Akustikmodell erstellen und bereitstellen.


## <a name="how-do-speech-recognition-systems-work"></a>Wie funktionieren Spracherkennungssysteme?
Spracherkennungssysteme bestehen aus mehreren Komponenten, die zusammenwirken. Zwei der wichtigsten Komponenten sind das Akustikmodell und das Sprachmodell.

Das Akustikmodell ist eine Klassifizierung, die kurze Fragmente einer Audioeingabe einem von mehreren Phonemen (Klangeinheiten) in einer bestimmten Sprache zuordnet. Das englische Wort „Speech“ z.B. besteht aus den vier englischen Phonemen „s“, „p“, „iy“ und „ch“. Diese Klassifizierungen erfolgen mit einer Frequenz von ca. 100 pro Sekunde.

Das Sprachmodell ist eine Wahrscheinlichkeitsverteilung über eine Sequenz von Wörtern. Mithilfe des Sprachmodells kann das System basierend auf der Wahrscheinlichkeit der Wortsequenzen selbst aus verschiedenen ähnlich klingenden Wortsequenzen die richtige Sequenz auswählen. Die englischen Sequenzen „recognize speech“ und „wreck a nice beach“ z.B. klingen ähnlich, das Auftreten der ersten Sequenz ist jedoch weitaus wahrscheinlicher und erhält daher im Sprachmodell einen höheren Rang.

Sowohl das Akustik- als auch das Sprachmodell sind statistische Modelle, die aus Trainingsdaten erlernt wurden. Daher funktionieren sie am besten, wenn die Sprache, auf die sie in der Anwendung treffen, den während des Trainings beobachteten Daten ähnlich ist. Die Akustik- und Sprachmodelle der Microsoft Sprache-in-Text-Engine wurden mit einer sehr großen Sammlung von Sprache und Text trainiert und bieten modernste Leistung für die gängigsten Anwendungsszenarien, wie die Interaktion mit Cortana auf Ihrem Smartphone, Tablet oder PC, die Suche im Web über Sprache oder das Diktieren von Textnachrichten an einen Freund.

## <a name="why-use-the-custom-speech-service"></a>Was spricht für die Verwendung von Custom Speech Service?
Auch wenn die Microsoft Sprache-in-Text-Engine zur Weltklasse zählt, ist sie auf die oben beschriebenen Szenarien ausgerichtet. Wenn Sie jedoch erwarten, dass Ihre Anwendung Sprachabfragen mit einem ganz bestimmten Vokabular verarbeiten muss, beispielsweise mit Produktnamen oder Fachtermini, die im allgemeinen Sprachgebrauch selten vorkommen, können Sie durch Anpassung des Sprachmodells wahrscheinlich eine bessere Leistung erzielen.

Ein Beispiel: Sie entwickeln eine App zum Durchsuchen von MSDN mithilfe von Spracheingabe. Dabei ist es sehr wahrscheinlich, dass Begriffe wie „objektorientiert“, „Namespace“ oder „Dotnet“ häufiger vorkommen als in anderen Spracherkennungsanwendungen. Indem Sie das Sprachmodell anpassen, können Sie dem System ermöglichen, dies zu lernen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden von Custom Speech Service finden Sie im [Custom Speech Service-Portal](https://cris.ai).

* [Erste Schritte](cognitive-services-custom-speech-get-started.md)
* [Häufig gestellte Fragen](cognitive-services-custom-speech-faq.md)
* [Glossar](cognitive-services-custom-speech-glossary.md)
