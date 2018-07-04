---
title: Übersicht über die Gesichtserkennungs-API | Microsoft-Dokumentation
titleSuffix: Microsoft Cognitive Services
description: In diesem Glossar werden Begriffe erläutert, auf die Sie möglicherweise beim Arbeiten mit der Gesichtserkennungs-API stoßen.
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: fb1d14ff80bf53adc3008d79cc998739ffffde1b
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37048668"
---
# <a name="what-is-face-api"></a>Worum handelt es sich bei der Gesichtserkennungs-API?

Willkommen bei der Gesichtserkennungs-API von Microsoft, einem cloudbasierten Dienst, der die fortschrittlichsten Gesichtserkennungsalgorithmen bereitstellt. Die Gesichtserkennungs-API erfüllt zwei Hauptfunktionen: Gesichtserfassung mit Attributen und Gesichtserkennung.

## <a name="face-detection"></a>Gesichtserfassung

Die Gesichtserkennungs-API erkennt bis zu 64 menschliche Gesichter durch Erfassen der jeweiligen Position in einem Bild mit hoher Genauigkeit. Das Bild kann durch eine Datei in Byte oder eine gültige URL angegeben werden.

![Übersicht – Gesichtserfassung](./Images/Face.detection.jpg)

Das Rechteck mit dem Gesicht (links, oben, Breite und Höhe), das die Position des Gesichts im Bild angibt, wird zusammen mit dem jeweils erfassten Gesicht zurückgegeben. Optional extrahiert die Gesichtserfassung eine Reihe gesichtsbezogener Attribute wie z.B. Pose, Geschlecht, Alter, Kopfhaltung, Gesichtsbehaarung und Brille. Weitere Informationen finden Sie unter [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="face-recognition"></a>Gesichtserkennung

Die Gesichtserkennung ist in vielen Szenarien verbreitet, einschließlich Sicherheit, natürliche Benutzeroberfläche, Bildinhaltsanalyse und -verwaltung, mobile Apps und Robotik. Es stehen vier Gesichtserkennungsfunktionen zur Verfügung: Gesichtsüberprüfung, Suchen ähnlicher Gesichter, Gesichtsgruppierung und Personenidentifizierung.

### <a name="face-verification"></a>Gesichtsüberprüfung

Bei der Überprüfung durch die Gesichtserkennungs-API wird eine Authentifizierung anhand zwei erfasster Gesichter oder eine Authentifizierung anhand eines erfassten Gesichts und eines Personenobjekts durchgeführt. Ausführlichere Informationen finden Sie unter [Face - Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

### <a name="finding-similar-face"></a>Suchen ähnlicher Gesichter

Bei Vorgabe eines erfassten Zielgesichts und einer Reihe möglicher Gesichter für die Suche sucht der Dienst nach einer kleinen Gruppe von Gesichtern, die dem Zielgesicht am meisten ähneln. Es werden zwei Arbeitsmodi, `matchFace` und `matchPerson`, unterstützt. Der `matchPerson`-Modus gibt ähnliche Gesichter zurück, nachdem ein Schwellenwert für die Personenübereinstimmung angewendet wurde, der von [Face - Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) abgeleitet ist. Beim `matchFace`-Modus wird der Schwellenwert für die Personenübereinstimmung ignoriert, und es werden die möglichen Gesichter mit der größten Ähnlichkeit zurückgegeben. Im folgenden Beispiel werden mögliche Gesichter aufgelistet.
![Übersicht – Suchen ähnlicher Gesichter](./Images/FaceFindSimilar.Candidates.jpg) Das Abfragegesicht ist ![Übersicht – Suchen ähnlicher Gesichter](./Images/FaceFindSimilar.QueryFace.jpg)

Für die Suche nach vier ähnlichen Gesichtern gibt der `matchPerson`-Modus (a) und (b) zurück, die mit der Person mit dem Abfragegesicht übereinstimmen. Der `matchFace`-Modus gibt (a), (b), (c) und (d) zurück, also genau vier mögliche Gesichter, selbst wenn diese nur eine geringe Ähnlichkeit aufweisen. Weitere Informationen finden Sie unter [Face - Find Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

### <a name="face-grouping"></a>Gesichtsgruppierung

Bei Vorgabe einer Gruppe unbekannter Gesichter unterteilt die Gesichtsgruppierungs-API diese auf Grundlage der Ähnlichkeit automatisch in mehrere Gruppen. Jede Gruppe ist eine separate echte Teilmenge der ursprünglichen Gruppe unbekannter Gesichter und enthält ähnliche Gesichter. Alle Gesichter in der gleichen Gruppe können als zum selben Personenobjekt gehörig betrachtet werden. Weitere Informationen finden Sie unter [Face - Group](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

### <a name="face-identification"></a>Gesichtsidentifikation

Die Gesichtserkennungs-API kann zum Identifizieren von Personen auf Grundlage eines erfassten Gesichts und einer Personendatenbank (definiert als eine „LargePersonGroup/PersonGroup“) verwendet werden. Erstellen Sie diese Datenbank im Voraus, die dann im Lauf der Zeit bearbeitet werden kann.

Die folgende Abbildung zeigt eine Beispieldatenbank vom Typ „LargePersonGroup/PersonGroup“ mit dem Namen „myfriends“. Jede Gruppe kann bis zu 1.000.000/10.000 Personenobjekte enthalten. Für jedes Personenobjekt können wiederum bis zu 248 Gesichter registriert sein.

![Übersicht – LargePersonGroup/PersonGroup](./Images/person.group.clare.jpg)

Nachdem eine „LargePersonGroup/PersonGroup“ erstellt und trainiert wurde, kann eine Identifizierung für die Gruppe und ein neu erfasstes Gesicht ausgeführt werden. Wenn das Gesicht als ein Personenobjekt in der Gruppe erkannt wird, wird das Personenobjekt zurückgegeben.

Weitere Informationen zur Personenidentifizierung finden Sie in den folgenden API-Leitfäden:

[Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)
[PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)
[PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)
[PersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249)
[LargePersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)
[LargePersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)
[LargePersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4)

### <a name="face-storage"></a>Gesichtserkennungsspeicher

Mit dem Gesichtserkennungsspeicher kann ein Standardabonnement bei Verwendung von „LargePersonGroup/PersonGroup Person“-Objekten ([PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)/[LargePersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42)) oder „LargeFaceLists/FaceLists“ ([FaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250)/[LargeFaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3)) zusätzliche Gesichter für die Identifikation oder den Ähnlichkeitsabgleich mithilfe der Gesichtserkennungs-API speichern. Die gespeicherten Bilder werden mit 0,5 US-Dollar pro 1000 Gesichter abgerechnet. Diese Rate wird täglich anteilig berechnet. Abonnements im Free-Tarif sind kostenlos, jedoch auf eine Gesamtzahl von 1.000 Personen begrenzt.

Der Preis für den Gesichtserkennungsspeicher wird täglich anteilig berechnet. Beispiel: In Ihrem Konto wurden in der ersten Monatshälfte täglich 10.000 Gesichter gespeichert und in der zweiten Monatshälfte keines. In diesem Fall werden Ihnen lediglich die 10.000 Gesichter in Rechnung gestellt, die für diese Tage gespeichert wurden. Wenn Sie jeden Tag im Monat 1.000 Gesichter für einige Stunden speichern und diese jede Nacht löschen, werden Ihnen dennoch 1.000 gespeicherte Gesichter in Rechnung gestellt.

## <a name="getting-started-tutorials"></a>Tutorials für erste Schritte

Die folgenden Tutorials zeigen die grundlegenden Funktionen und Abonnementprozesse der Gesichtserkennungs-API:

- [Tutorial für erste Schritte mit der Gesichtserkennungs-API in CSharp](Tutorials/FaceAPIinCSharpTutorial.md)
- [Tutorial für erste Schritte mit der Gesichtserkennungs-API in Java für Android](Tutorials/FaceAPIinJavaForAndroidTutorial.md)
- [Tutorial für erste Schritte mit der Gesichtserkennungs-API in Python](Tutorials/FaceAPIinPythonTutorial.md)

## <a name="sample-apps"></a>Beispiel-Apps

Sehen Sie sich diese Beispielanwendungen an, bei denen die Gesichtserkennungs-API verwendet wird.

- [Gesichtserkennungs-API von Microsoft: Windows-Clientbibliothek und Beispiel](https://github.com/Microsoft/Cognitive-Face-Windows)
  - WPF-Beispiel-App, die mehrere Szenarien der Erfassung, Analyse und Identifizierung von Gesichtern veranschaulicht.
- [UWP-App FamilyNotes](https://github.com/Microsoft/Windows-appsample-familynotes)
  - UWP-Beispiel-App (Universelle Windows-Plattform), bei der die Verwendung von Sprache, Cortana, Freihand und Kamera in einem Szenario gemeinsam verwendeter Familiennotizen gezeigt wird.
- [Beispiel einer Videobildanalyse](https://github.com/microsoft/cognitive-samples-videoframeanalysis)
  - UWP-Beispiel-App (Universelle Windows-Plattform), bei der das Analysieren von Videolivestreams nahezu in Echtzeit mithilfe von APIs für Gesichtserkennung, maschinelles Sehen und Emotionen gezeigt wird.

## <a name="related-topics"></a>Verwandte Themen

- [Anmerkungen zur Gesichtserkennungs-API Version 1.0](ReleaseNotes.md)
- [Informationen zur Gesichtserkennung in Bildern](Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
- [Informationen zur Gesichtsidentifikation in Bildern](Face-API-How-to-Topics/HowtoIdentifyFacesinImage.md)
