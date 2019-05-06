---
title: Funktionsweise der Personalisierung – Personalisierung
titleSuffix: Azure Cognitive Services
description: Personalisierung verwendet maschinelles Lernen, um zu ermitteln, welche Aktion in einem Kontext verwendet werden soll. Jede Lernschleife verfügt über ein Modell, das ausschließlich mit Daten trainiert wird, die Sie über Rangfolge- und Relevanzaufrufe an es gesendet haben. Jede Lernschleife ist vollkommen unabhängig von den anderen.
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: 6b2237f27fba5eaf952932cd6592052649400b96
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025493"
---
# <a name="how-personalizer-works"></a>Funktionsweise der Personalisierung

Personalisierung verwendet maschinelles Lernen, um zu ermitteln, welche Aktion in einem Kontext verwendet werden soll. Jede Lernschleife verfügt über ein Modell, das ausschließlich mit Daten trainiert wird, die Sie über **Rangfolge**- und **Relevanz**aufrufe an es gesendet haben. Jede Lernschleife ist vollkommen unabhängig von den anderen. Erstellen Sie für jeden Teil oder jedes Verhalten Ihrer Anwendung, den/das Sie anpassen möchten, eine Lernschleife.

Für jede Schleife **rufen Sie die Rangfolge-API** basierend auf dem aktuellen Kontext auf mit:

* Liste der möglichen Aktionen: Inhaltselemente, aus denen die Top-Aktion ausgewählt werden soll.
* Liste der [Kontextmerkmale](concepts-features.md): kontextbezogen relevante Daten wie Benutzer, Inhalt und Kontext.

Die **Rangfolge**-API entscheidet über die Verwendung einer der beiden folgenden Optionen:

* _Exploit_ (Nutzen): Das aktuelle Modell entscheidet über die beste Aktion auf Grundlage zurückliegender Daten.
* _Explore_ (Erkunden): Eine andere Aktion anstelle der Top-Aktion auswählen.

Die **Relevanz**-API:

* Sammelt Daten zum Trainieren des Modells durch Aufzeichnung der Merkmale und Relevanzbewertungen jedes Rangfolgeaufrufs.
* Verwendet diese Daten zum Aktualisieren des Modells, basierend auf den in der _Lernrichtlinie_ angegebenen Einstellungen.

## <a name="architecture"></a>Architecture

Die folgende Abbildung zeigt den Fluss der Rangfolge- und Relevanzaufrufe durch die Architektur:

![Alternativtext](./media/how-personalizer-works/personalization-how-it-works.png "Funktionsweise der Personalisierung")

1. Personalisierung verwendet ein internes KI-Modell, um den Rang der Aktion zu bestimmen.
1. Der Dienst entscheidet, ob das aktuelle Modell genutzt oder neue Auswahlmöglichkeiten für das Modell erkundet werden sollen.  
1. Das Rangfolgeergebnis wird an EventHub gesendet.
1. Wenn die Personalisierung die Relevanz empfängt, wird diese an EventHub gesendet. 
1. Rang und Relevanz sind korreliert.
1. Das KI-Modell wird auf Grundlage der Korrelationsergebnisse aktualisiert.
1. Die Rückschluss-Engine wird mit dem neuen Modell aktualisiert. 

## <a name="research-behind-personalizer"></a>Die Forschung hinter der Personalisierung

Personalisierung basiert auf innovativer Wissenschaft und Forschung im Bereich des [vertiefenden Lernens](concepts-reinforcement-learning.md), einschließlich Artikeln, Forschungsaktivitäten und Bereichen der laufenden Forschung bei Microsoft Research.

## <a name="terminology"></a>Begriff

* **Lernschleife**: Sie können eine Lernschleife für jeden Teil Ihrer Anwendung erstellen, der von der Personalisierung profitieren kann. Wenn Sie mehr als eine zu personalisierende Erfahrung haben, erstellen Sie für jede eine Schleife. 

* **Aktionen**: Aktionen sind die Inhaltselemente, z. B. Produkte oder Werbeaktionen, unter denen ausgewählt wird. Die Personalisierung wählt über die Rangfolge-API die Top-Aktion aus, die Ihren Benutzern angezeigt werden soll, bekannt als die _Relevanzaktion_. Jede Aktion kann Merkmale besitzen, die zusammen mit der Rangfolgeanforderung übermittelt werden.

* **Context**: Um eine genauere Rangfolge bereitzustellen, geben Sie Informationen zu Ihrem Kontext an, beispielsweise:
    * Ihre Benutzer.
    * Das Gerät, das sie verwenden. 
    * Die aktuelle Zeit.
    * Andere Daten über die aktuelle Situation.
    * Vergangene Daten über den Benutzer oder Kontext.

    Ihre spezifische Anwendung kann abweichende Kontextinformationen haben. 

* **[Merkmale](concepts-features.md)**: Eine Einheit von Informationen über ein Inhaltselement oder einen Benutzerkontext.

* **Relevanz**: Ein Maß dafür, wie der Benutzer auf die von der Rangfolge-API zurückgegebene Aktion reagiert hat, als eine Bewertung zwischen 0 und 1. Der Wert von 0 bis 1 wird von Ihrer Geschäftslogik festgelegt, basierend darauf, wie die Auswahl dabei geholfen hat, Ihre geschäftlichen Ziele der Personalisierung zu erreichen. 

* **Exploration** (Erkunden): Der Personalisierungsdienst erkundet, wenn er, anstatt die beste Aktion zurückzugeben, eine andere Aktion für den Benutzer auswählt. Der Personalisierungsdienst kann durch Erkunden Verschiebungen und Stagnation vermeiden sowie sich an laufendes Benutzerverhalten anpassen. 

* **Experimentdauer**: Die Zeitspanne, die der Personalisierungsdienst auf eine Relevanz wartet, beginnend ab dem Zeitpunkt, zu dem der Rangfolgeaufruf für dieses Ereignis erfolgte.

* **Inaktive Ereignisse**: Ein inaktives Ereignis ist eines, für das Sie die Rangfolge aufgerufen haben, sich aber nicht sicher sind, ob der Benutzer jemals das Ergebnis sehen wird, weil dies Entscheidungen der Clientanwendung unterliegt. Inaktive Ereignisse ermöglichen es Ihnen, Personalisierungsergebnisse zu erstellen und zu speichern und dann zu entscheiden, sie später zu verwerfen, ohne dass dies das Machine Learning-Modell beeinträchtigt.

* **Modell**: Ein Personalisierungsmodell erfasst alle Daten, die über Benutzerverhalten gelernt wurden, erhält Trainingsdaten aus der Kombination der Argumente, die Sie an die Rangfolge- und Relevanzaufrufe senden, und besitzt ein Trainingsverhalten, das von der Lernrichtlinie bestimmt wird. 

## <a name="next-steps"></a>Nächste Schritte

Versteshen, [wo Sie Personalisierung verwenden können](where-can-you-use-personalizer.md).