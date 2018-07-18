---
title: Was ist das Unterhaltungslernmodul? – Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Hier erhalten Sie Informationen zum Unterhaltungslernmodul und zu dessen Funktionsweise.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 9cbf0e60382ef17d68aab47cf5f24ea9b8434f13
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376218"
---
# <a name="what-is-conversation-learner"></a>Was ist das Unterhaltungslernmodul?

Mit dem Unterhaltungslernmodul können Sie Unterhaltungsschnittstellen erstellen und trainieren, die anhand von Beispielinteraktionen lernen. 

Im Gegensatz zu herkömmlichen Konzepten berücksichtigt das Unterhaltungslernmodul den End-to-End-Kontext eines Dialogs, um die Antworten zu verbessern und überzeugendere Benutzererlebnisse zu ermöglichen. Das Unterhaltungslernmodul umfasst eine breite Palette von aufgabenorientierten Anwendungsfällen und wendet maschinelles Lernen im Hintergrund an, um dafür zu sorgen, dass Bots und intelligente Agents die Benutzer weniger frustrieren, keine zusätzlichen Kundendienstkosten entstehen und die Interaktion damit intuitiver wird.

Zu Beginn gibt der Entwickler prototypische Dialoge ein, die imitiert werden sollen. Bei der Eingabe weiterer Dialoge wird das Modell kontinuierlich aktualisiert, und der Entwickler kann sehen, wie gut sich das Modell verallgemeinert. Sobald das Modell gut funktioniert, kann der Bot für Endbenutzer bereitgestellt werden. Das Unterhaltungslernmodul protokolliert Unterhaltungen mit Benutzern, und der Entwickler kann diese Unterhaltungen überprüfen. Wenn Fehler entdeckt werden, kann der Entwickler direkt eine Korrektur vornehmen. Das Modell wird neu trainiert und steht sofort wieder zur Verfügung.

Dieser Ansatz reduziert die manuelle Codierung der Dialogsteuerungslogik und ermöglicht Geschäftsinhabern oder Domänenexperten die Mitwirkung an einer Unterhaltungsschnittstelle ohne vorherige Kenntnisse des maschinellen Lernens. Unabhängig davon, ob das Unterhaltungslernmodul als Teil eines Bot, eines intelligenten Geräts oder eines intelligenten Agents bereitgestellt wird, kann es neue Fähigkeiten, Verhaltensweisen oder Kompetenzen schnell iterieren und deren Qualität schnell verbessern. 

Das Unterhaltungslernmodul ermöglicht es Entwicklern, die Markteinführung zu beschleunigen und erfolgreiche Dialoge über mehrere Unterhaltungskanäle über das Microsoft Bot Framework oder eigenständig über die eigene Infrastruktur zu führen.

Zusammenfassung und Highlights:

- Das Unterhaltungslernmodul bietet eine erstklassige KI-Methode der Erstellung aufgabenorientierter Bots.

- Es basiert auf einem wiederkehrenden neuronalen End-to-End-Netzwerk (LSTM) und lernt direkt anhand von Unterhaltungsbeispielen mit mehreren Sequenzen. 

- Es ermöglicht Designern, Entwicklern, Geschäftsbenutzern und Call Center-Mitarbeitern das Erstellen und Verwalten von Bots. 

- Es bietet die Möglichkeit, Geschäftsregeln und gesunden Menschenverstand in Code auszudrücken.

- In den Trainingssitzungen wird das neuronale Netzwerkmodell verwendet, um den nächsten Satz von erwarteten Aktionen in der Unterhaltung zu bewerten. Der Botentwickler kann dann die richtige Aktion auswählen und das Netzwerk trainieren, um die richtige Antwort bereitzustellen.
 
- Nach Abschluss des Trainings kann der Entwickler anhand der protokollierten Dialoge der Benutzerinteraktionen Korrekturen an den Botantworten vornehmen und das Modell neu trainieren. 

- Zur Ausführung der Aufgaben können domänenspezifische und Drittanbieter-APIs aufgerufen werden.

