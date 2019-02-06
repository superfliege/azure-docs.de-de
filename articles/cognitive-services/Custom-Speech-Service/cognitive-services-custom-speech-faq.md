---
title: Häufig gestellte Fragen zum Custom Speech Service in Azure | Microsoft-Dokumentation
description: Hier erhalten Sie Antworten auf die am häufigsten gestellten Fragen zum Custom Speech Service.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 11/21/2016
ms.author: panosper
ms.openlocfilehash: 88486ec9d1ca11d25ca31ca0abb4a34509d19a27
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228031"
---
# <a name="custom-speech-service-frequently-asked-questions"></a>Häufig gestellte Fragen zum Custom Speech Service

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-custom-speech-deprecation-note.md)] 

Wenn Sie in diesen FAQs keine Antwort auf Ihre Frage finden, können Sie die Fragen unter [Stack Overflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) und [UserVoice](https://cognitive.uservoice.com/) an die Custom Speech Service-Community stellen.

## <a name="general"></a>Allgemein

**Frage:** Wie erfahre ich, wann die Verarbeitung meines Datasets oder Modells abgeschlossen ist?

**Antwort:** Derzeit ist der Status des Modells oder Datasets in der Tabelle die einzige Möglichkeit, dies zu erfahren.
Wenn die Bearbeitung abgeschlossen ist, lautet der Status „Bereit“.
Wir arbeiten an verbesserten Methoden für den Kommunikationsverarbeitungsstatus, z.B. E-Mail-Benachrichtigung.

**Frage:** Kann ich mehrere Modelle gleichzeitig erstellen?

**Antwort:** Die Anzahl der Modelle in Ihrer Sammlung ist nicht begrenzt, aber auf jeder Seite kann jeweils nur ein Modell erstellt werden.
Beispielsweise können Sie keinen Prozess zur Erstellung eines Sprachmodells starten, wenn sich derzeit ein Sprachmodell in der Bearbeitungsphase befindet.
Sie können jedoch ein Akustikmodell und ein Sprachmodell gleichzeitig verarbeiten. 

**Frage:** Ich habe festgestellt, dass ich einen Fehler gemacht habe. Wie kann ich meinen Datenimport oder die laufende Modellerstellung abbrechen? 

**Antwort:** Derzeit können Sie einen akustischen oder sprachlichen Anpassungsprozess nicht rückgängig machen.
Importierte Daten können nach Abschluss des Imports gelöscht werden.

**Frage:** Was ist der Unterschied zwischen Such- und Diktiermodellen und Konversationsmodellen?

**Antwort:** In Custom Speech Service stehen zwei Basis-Akustik- und -Sprachmodelle zur Auswahl.
Suchabfragen oder Diktieren. Das Konversationsakustikmodell von Microsoft eignet sich für die Erkennung von Spracheingaben im Gesprächsstil.
Diese Art von Spracheingabe richtet sich üblicherweise an eine andere Person, z.B. in Callcentern oder Besprechungen.

**Frage:** Kann ich mein vorhandenes Modell aktualisieren (Modellstapel)?

**Antwort:** Wir bieten nicht die Möglichkeit, ein vorhandenes Modell mit neuen Daten zu aktualisieren.
Wenn Sie über ein neues Dataset verfügen und ein vorhandenes Modell anpassen möchten, müssen sie es mit den neuen Daten und dem alten Dataset, das Sie verwendet haben, erneut anpassen.
Altes und neues Dataset müssen in einer einzelnen ZIP-Datei (bei akustischen Daten) oder einer TXT-Datei (bei Sprachdaten) kombiniert werden. Wenn die Anpassung erfolgt ist, muss das neue aktualisierte Modell erneut bereitgestellt werden, um einen neuen Endpunkt zu erhalten.

**Frage:** Was geschieht, wenn ich eine höhere Parallelität als den Standardwert benötige? 

**Antwort:** Sie können Ihr Modell in Schritten von 5 gleichzeitigen Anforderungen hochskalieren, die wir als Skalierungseinheiten bezeichnen. Jede Skalierungseinheit garantiert, dass Ihr Modell 5 Audiodatenströme gleichzeitig verarbeiten kann. Sie können bis zu 100 Skalierungseinheiten (oder 500 gleichzeitige Anforderungen) erwerben.

Kontaktieren Sie uns, wenn Sie eine höhere Skalierung benötigen.

**Frage:** Kann ich mein Modell herunterladen und lokal ausführen?

**Antwort:** Wir ermöglichen nicht, Modelle herunterzuladen und lokal auszuführen.

**Frage:** Werden meine Anforderungen protokolliert?

**Antwort:** Sie haben während der Erstellung einer Bereitstellung die Wahl, die Ablaufverfolgung auszuschalten. Dadurch werden kein Audio und keine Transkriptionen mehr protokolliert. Andernfalls werden Anforderungen normalerweise im sicheren Speicher in Azure protokolliert. Wenn Sie weitere Datenschutzbedenken im Hinblick auf die Nutzung von Custom Speech Service haben, wenden Sie sich an uns.

## <a name="importing-data"></a>Importieren von Daten

**Frage:** Wie groß darf das Dataset maximal sein? Warum? 

**Antwort:** Das aktuelle Limit für ein Dataset beträgt 2GB, da die Größe einer Datei für den HTTP-Upload begrenzt ist. 

**Frage:** Kann ich meine Textdateien komprimieren, um eine größere Textdatei hochzuladen? 

**Antwort:** Nein, derzeit sind nur unkomprimierte Textdateien zulässig.

**Frage:** Der Datenbericht besagt, dass fehlerhafte Äußerungen gefunden wurden. Handelt es sich um ein Problem?

**Antwort:** Wenn nur wenige Äußerungen nicht erfolgreich importiert werden konnten, ist dies kein Problem.
Wenn der Großteil der Äußerungen in einem Akustik- oder Sprachdataset (z.B. > 95 %) erfolgreich importiert wird, kann das Dataset verwendet werden. Allerdings sollten Sie herausfinden, warum Fehler bei den Äußerungen aufgetreten sind, und die Probleme beheben.
Die meisten Probleme, z.B. Formatierungsfehler, sind einfach zu beheben. 

## <a name="creating-am"></a>Erstellen eines Akustikmodells (AM)

**Frage:** Wie viele Akustikdaten benötige ich?

**Antwort:** Sie sollten zunächst mit 30-60 Minuten Akustikdaten beginnen.

**Frage:** Welche Art von Daten soll ich sammeln?

**Antwort:** Sammeln Sie Daten, die dem Anwendungsszenario und dem Anwendungsfall möglichst nahe kommen.
D.h., die Datensammlung sollte in Bezug auf Geräte, Umgebungen und Sprechertypen mit der Zielanwendung und den Benutzern übereinstimmen. Generell sollten Sie Daten von möglichst vielen Sprechern sammeln. 

**Frage:** Wie soll ich Daten erfassen? 

**Antwort:** Sie können eine eigenständige Datensammlungsanwendung erstellen oder eine handelsübliche Audioaufzeichnungssoftware verwenden.
Sie können auch eine Version der Anwendung erstellen, die Audiodaten protokolliert und diese dann verwendet. 

**Frage:** Muss ich die Anpassungsdaten selbst transkribieren? 

**Antwort:** Die Daten müssen transkribiert werden. Sie können sie selbst transkribieren oder einen professionellen Transkriptionsdienst nutzen. Einige verwenden professionelle Transkribenten, und andere verwenden Crowdsourcing. Wir können zudem auf Anfrage einen Transkriptionsdienst empfohlen.

**Frage:** Wie lange dauert es, ein benutzerdefiniertes Akustikmodell zu erstellen?

**Antwort:** Die Bearbeitungszeit zum Erstellen eines benutzerdefinierten Akustikmodells entspricht in etwa der Länge des akustischen Datasets.
Daher dauert die Bearbeitung eines benutzerdefinierten Akustikmodells, das aus einem fünfstündigen Dataset erstellt wird, etwa fünf Stunden. 

## <a name="offline-testing"></a>Offlinetests

**Frage:** Kann ich mein benutzerdefiniertes Akustikmodell mit einem benutzerdefinierten Sprachmodell offline testen?

**Antwort:** Ja, wählen Sie beim Einrichten des Offlinetests einfach das benutzerdefinierte Sprachmodell im Dropdownmenü aus.

**Frage:** Kann ich mein benutzerdefiniertes Sprachmodell mit einem benutzerdefinierten Akustikmodell offline testen?

**Antwort:** Ja, wählen Sie beim Einrichten des Offlinetests einfach das benutzerdefinierte Akustikmodell im Dropdownmenü aus.

**Frage:** Was ist die Wort-Fehler-Rate, und wie wird sie berechnet?

**Antwort:** Die Wort-Fehler-Rate ist die Auswertungsmetrik für die Spracherkennung. Sie wird berechnet als die Gesamtanzahl von Fehlern, einschließlich Einfügungen, Löschungen und Ersetzungen, dividiert durch die Gesamtzahl der Wörter in der Referenztranskription.

**Frage:** Ich kenne jetzt die Testergebnisse meines benutzerdefinierten Modells, ist dies ein guter oder schlechter Wert?

**Antwort:** Die Ergebnisse stellen einen Vergleich zwischen dem Basismodell und dem von Ihnen angepassten Modell dar.
Es empfiehlt sich, das Basismodell zu übertreffen, damit sich die Anpassung lohnt.

**Frage:** Wie finde ich die Windows-Fehlerberichterstattung (WER) der Basismodelle, damit ich sehen kann, ob es Verbesserungen gab? 

**Antwort:** Die Offlinetestergebnisse zeigen die Genauigkeit des Basismodells und des benutzerdefinierten Modells sowie die Verbesserung gegenüber dem Basismodell.

## <a name="creating-lm"></a>Erstellen eines Sprachmodells (LM)

**Frage:** Wie viele Textdaten muss ich hochladen?

**Antwort:** Diese Frage ist schwierig zu beantworten, denn es hängt davon ab, wie stark sich die in der Anwendung verwendeten Vokabeln und Ausdrücke von den Ausgangssprachmodellen unterscheiden. Für alle neuen Wörter ist es hilfreich, so viele Beispiele wie möglich für ihre Verwendung bereitzustellen. Für gängige Ausdrücke, die in Ihrer Anwendung verwendet werden, sind auch Ausdrücke in den Sprachdaten nützlich, da sie das System anweisen, auch auf diese Begriffe zu achten.
Üblicherweise sollte das Sprachdataset mindestens einhundert und typischerweise mehrere hundert Äußerungen umfassen.
Auch wenn bestimmte Arten * von Abfragen häufiger als andere erwartet werden, können Sie mehrere Kopien der häufigen Abfragen in das Dataset einfügen.

**Frage:** Kann ich nur eine Liste von Wörtern hochladen?

**Antwort:** Das Hochladen einer Liste von Wörtern bringt die Wörter in den Wortschatz, trainiert das System aber nicht, wie die Wörter typischerweise verwendet werden.
Durch die Bereitstellung vollständiger oder teilweiser Äußerungen (Sätze oder Ausdrücke, die von Benutzern üblicherweise verwendet werden) kann das Sprachmodell die neuen Wörter und deren Verwendung lernen. Das benutzerdefinierte Sprachmodell eignet sich nicht nur gut dazu, neue Wörter in das System aufzunehmen, sondern auch, die Wahrscheinlichkeit bekannter Wörter für Ihre Anwendung anzupassen. Vollständige Äußerungen helfen dem System, besser zu lernen. 

-----

 * [Übersicht](cognitive-services-custom-speech-home.md)
 * [Gestartet](cognitive-services-custom-speech-get-started.md)
 * [Glossar](cognitive-services-custom-speech-glossary.md)
