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
ms.openlocfilehash: 543e8d6fb68a351dfe75c962debaf15eeb080a3f
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223886"
---
# <a name="speech-to-text-frequently-asked-questions"></a>Häufig gestellte Fragen zur Spracherkennung

Wenn Sie in diesen FAQs keine Antwort auf Ihre Frage finden, können Sie die Fragen unter [Stack Overflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) und [UserVoice](https://cognitive.uservoice.com/) an die Custom Speech Service-Community stellen.

## <a name="general"></a>Allgemein

**Frage:** Worin besteht der Unterschied zwischen Basis- und benutzerdefinierten Spracherkennungsmodellen?

**Antwort:** Basismodelle wurden mit Daten von Microsoft trainiert und sind bereits in der Cloud verfügbar. Benutzerdefinierte Modelle ermöglichen dem Benutzer, ein Modell optimal an eine bestimmte Umgebung mit speziellen Umweltgeräuschen oder individueller Sprache anzupassen. Für Fabrikhallen, Autos, laute Straßen ist beispielsweise ein angepasstes Akustikmodell erforderlich, während für bestimmte Themen wie z.B. Biologie, Physik, Radiologie, Produktnamen und benutzerdefinierte Akronyme ein angepasstes Sprachmodell benötigt wird.

**Frage:** Wo fange ich an, wenn ich ein Basismodell verwenden möchte?

**Antwort:** Zuerst müssen Sie einen [Abonnementschlüssel](get-started.md) erhalten. Wenn Sie REST-Aufrufe an die vorab bereitgestellten Basismodelle ausführen möchten, ziehen Sie [diese Details](rest-apis.md) zurate. Wenn Sie WebSockets verwenden möchten, laden Sie das [SDK](speech-sdk.md) herunter.

**Frage:** Muss ich immer ein benutzerdefiniertes Sprachmodell erstellen?

**Antwort:** Nein, wenn Ihre Anwendung generische, alltägliche Sprache verwendet, dann müssen Sie kein Modell anpassen. Wenn Ihre Anwendung zudem in einer Umgebung verwendet wird, in der es wenig oder gar keine Hintergrundgeräusche gibt, müssen Sie sie auch nicht anpassen. Über das Portal können Benutzer Basismodelle und angepasste Modelle bereitstellen und Genauigkeitsprüfungen für diese durchführen. Benutzer können dieses Feature verwenden, um die Genauigkeit eines Basismodells im Vergleich zu einem benutzerdefinierten Modell zu messen.

**Frage:** Wie erfahre ich, wann die Verarbeitung meines Datasets oder Modells abgeschlossen ist?

**Antwort:** Derzeit ist der Status des Modells oder Datasets in der Tabelle die einzige Möglichkeit, dies zu erfahren.
Wenn die Bearbeitung abgeschlossen ist, lautet der Status „Erfolgreich“.

**Frage:** Kann ich mehrere Modelle erstellen?

**Antwort**: Es gibt keine Beschränkung, wie viele Modelle Ihre Sammlung enthält.

**Frage:** Ich habe festgestellt, dass ich einen Fehler gemacht habe. Wie kann ich meinen Datenimport oder die laufende Modellerstellung abbrechen? 

**Antwort:** Derzeit können Sie einen akustischen oder sprachlichen Anpassungsprozess nicht rückgängig machen. Importierte Daten und Modelle können gelöscht werden, wenn sie in einem Endzustand sind.

**Frage:** Was ist der Unterschied zwischen Such- und Diktiermodellen und Konversationsmodellen?

**Antwort**: Im Speech Service stehen mehrere Basismodelle zur Wahl. Das Konversationsmodell eignet sich für die Erkennung von Spracheingaben im Gesprächsstil. Dieses Modell wäre ideal für das Transkribieren von Anrufen, während Suchen und Diktat für Apps ideal ist, die durch Sprache ausgelöst werden. Universell ist ein neues Modell, das darauf abzielt, beide Szenarien anzusprechen.

**Frage:** Kann ich mein vorhandenes Modell aktualisieren (Modellstapel)?

**Antwort:** Vorhandene Modelle können nicht aktualisiert werden. Als Problemumgehung können Sie das alte Dataset mit dem neuen kombinieren und neu anpassen.

Das alte und das neue Dataset müssen in einer einzigen ZIP-Datei (wenn es sich um akustische Daten handelt) bzw. TXT-Datei (wenn es sich um Sprachdaten handelt) zusammengefasst werden. Nachdem die Anpassung abgeschlossen ist, muss die Bereitstellung des neuen, aktualisierten Modells entfernt werden, um einen neuen Endpunkt zu erhalten.

**Frage:** Was geschieht, wenn ich eine höhere Parallelität für mein bereitgestelltes Modell benötige, als im Portal angeboten? 

**Antwort:** Sie können Ihr Modell in Schritten von 20 gleichzeitigen Anforderungen hochskalieren. 

Kontaktieren Sie uns, wenn Sie eine höhere Skalierung benötigen.

**Frage:** Kann ich mein Modell herunterladen und lokal ausführen?

**Antwort:** Modelle können nicht heruntergeladen und lokal ausgeführt werden.

**Frage:** Werden meine Anforderungen protokolliert?

**Antwort:** Sie haben während der Erstellung einer Bereitstellung die Wahl, die Ablaufverfolgung auszuschalten. Dadurch werden kein Audio und keine Transkriptionen mehr protokolliert. Andernfalls werden Anforderungen normalerweise im sicheren Speicher in Azure protokolliert. Wenn Sie weitere Datenschutzbedenken im Hinblick auf die Nutzung von Custom Speech Service haben, wenden Sie sich an einen der Supportkanäle.

## <a name="importing-data"></a>Importieren von Daten

**Frage:** Wie groß darf das Dataset maximal sein? Warum? 

**Antwort:** Das aktuelle Limit für ein Dataset beträgt 2 GB, da die Größe einer Datei für den HTTP-Upload begrenzt ist. 

**Frage:** Kann ich meine Textdateien komprimieren, um eine größere Textdatei hochzuladen? 

**Antwort:** Nein, derzeit sind nur unkomprimierte Textdateien zulässig.

**Frage:** Der Datenbericht besagt, dass fehlerhafte Äußerungen gefunden wurden. Was ist das Problem?

**Antwort:** Ein Fehler beim Hochladen von 100 % der Äußerungen in einer Datei stellt kein Problem dar.
Wenn der Großteil der Äußerungen in einem Akustik- oder Sprachdataset (z.B. > 95 %) erfolgreich importiert wird, kann das Dataset verwendet werden. Allerdings sollten Sie herausfinden, warum Fehler bei den Äußerungen aufgetreten sind, und die Probleme beheben. Die meisten Probleme, z.B. Formatierungsfehler, sind einfach zu beheben. 

## <a name="creating-am"></a>Erstellen eines Akustikmodells (AM)

**Frage:** Wie viele Akustikdaten benötige ich?

**Antwort:** Sie sollten zunächst mit 30-60 Minuten Akustikdaten beginnen.

**Frage:** Welche Daten soll ich sammeln?

**Antwort:** Sammeln Sie Daten, die dem Anwendungsszenario und dem Anwendungsfall möglichst nahe kommen.
Die Datensammlung sollte in Bezug auf Geräte, Umgebungen und Sprechertypen mit der Zielanwendung und den Benutzern übereinstimmen. Generell sollten Sie Daten von möglichst vielen Sprechern sammeln. 

**Frage:** Wie soll ich Daten erfassen? 

**Antwort:** Sie können eine eigenständige Datensammlungsanwendung erstellen oder eine handelsübliche Audioaufzeichnungssoftware verwenden.
Sie können auch eine Version der Anwendung erstellen, die Audiodaten protokolliert und diese dann verwendet. 

**Frage:** Muss ich die Anpassungsdaten selbst transkribieren? 

**Antwort**: Ja! Sie können sie selbst transkribieren oder einen professionellen Transkriptionsdienst nutzen. Manche Benutzer bevorzugen professionelle Transkriptionsdienste, während andere Crowdsourcing nutzen oder die Transkriptionen selbst durchführen.

## <a name="accuracy-testing"></a>Genauigkeitstests

**Frage:** Kann ich mein benutzerdefiniertes Akustikmodell mit einem benutzerdefinierten Sprachmodell offline testen?

**Antwort:** Ja, wählen Sie beim Einrichten des Offlinetests einfach das benutzerdefinierte Sprachmodell im Dropdownmenü aus.

**Frage:** Kann ich mein benutzerdefiniertes Sprachmodell mit einem benutzerdefinierten Akustikmodell offline testen?

**Antwort:** Ja, wählen Sie beim Einrichten des Offlinetests einfach das benutzerdefinierte Akustikmodell im Dropdownmenü aus.

**Frage:** Was ist die Wort-Fehler-Rate (Word Error Rate, WER), und wie wird sie berechnet?

**Antwort:** Die Wort-Fehler-Rate (WER) ist die Auswertungsmetrik für die Spracherkennung. Sie wird berechnet als die Gesamtanzahl von Fehlern, einschließlich Einfügungen, Löschungen und Ersetzungen, dividiert durch die Gesamtzahl der Wörter in der Referenztranskription. Ausführlichere Informationen finden Sie [hier](https://en.wikipedia.org/wiki/Word_error_rate).

**Frage:** Wie kann ich feststellen, ob die Ergebnisse einer Genauigkeitsprüfung gut sind?

**Antwort:** Die Ergebnisse stellen einen Vergleich zwischen dem Basismodell und dem von Ihnen angepassten Modell dar.
Sie sollten das Basismodell übertreffen, damit sich die Anpassung lohnt.

**Frage:** Wie finde ich die Wort-Fehler-Rate der Basismodelle heraus, damit ich sehen kann, ob es Verbesserungen gab? 

**Antwort:** Die Offlinetestergebnisse zeigen die Genauigkeit des Basismodells und des benutzerdefinierten Modells sowie die Verbesserung gegenüber dem Basismodell.

## <a name="creating-lm"></a>Erstellen eines Sprachmodells (LM)

**Frage:** Wie viele Textdaten muss ich hochladen?

**Antwort:** Es hängt davon ab, wie stark sich die in der Anwendung verwendeten Vokabeln und Ausdrücke von den Ausgangssprachmodellen unterscheiden. Für alle neuen Wörter ist es hilfreich, so viele Beispiele wie möglich für ihre Verwendung bereitzustellen. Für gängige Ausdrücke, die in Ihrer Anwendung verwendet werden, sind auch Ausdrücke in den Sprachdaten nützlich, da sie das System anweisen, auch auf diese Begriffe zu achten. Üblicherweise sollte das Sprachdataset mindestens einhundert und typischerweise mehrere hundert Äußerungen umfassen. Auch wenn bestimmte Arten von Abfragen häufiger als andere erwartet werden, können Sie mehrere Kopien der häufigen Abfragen in das Dataset einfügen.

**Frage:** Kann ich nur eine Liste von Wörtern hochladen?

**Antwort:** Das Hochladen einer Liste von Wörtern bringt die Wörter in den Wortschatz, trainiert das System aber nicht, wie die Wörter typischerweise verwendet werden.
Durch die Bereitstellung vollständiger oder teilweiser Äußerungen (Sätze oder Ausdrücke, die von Benutzern üblicherweise verwendet werden) kann das Sprachmodell die neuen Wörter und deren Verwendung lernen. Das benutzerdefinierte Sprachmodell eignet sich nicht nur gut dazu, neue Wörter in das System aufzunehmen, sondern auch, die Wahrscheinlichkeit bekannter Wörter für Ihre Anwendung anzupassen. Vollständige Äußerungen helfen dem System, besser zu lernen. 

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung](troubleshooting.md)
* [Anmerkungen zu dieser Version](releasenotes.md)
