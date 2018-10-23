---
title: Was ist der Gesichtserkennungs-API-Dienst?
titleSuffix: Azure Cognitive Services
description: In diesem Thema werden der Gesichtserkennungs-API-Dienst sowie entsprechende Begriffe erläutert.
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: overview
ms.date: 10/11/2018
ms.author: sbowles
ms.openlocfilehash: 6c1e0d0a51bc01c581c05e7ce3215f5501b4be76
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2018
ms.locfileid: "49310367"
---
# <a name="what-is-the-face-api-service"></a>Was ist der Gesichtserkennungs-API-Dienst?

Der Gesichtserkennungs-API-Dienst ist ein cloudbasierter Dienst, der Algorithmen für die Analyse menschlicher Gesichter auf Bildern und in Videos bereitstellt. Die Gesichtserkennungs-API erfüllt zwei Hauptfunktionen: Gesichtserfassung mit Attributen und Gesichtserkennung.

## <a name="face-detection"></a>Gesichtserfassung

Die Gesichtserkennungs-API kann auf einem Bild bis zu 64 menschliche Gesichter erfassen und deren jeweilige Position mit hoher Präzision bestimmen. Als Bild kann eine Datei (Bytedatenstrom) oder eine gültige URL angegeben werden.

![Übersicht – Gesichtserfassung](./Images/Face.detection.jpg)

Das Gesichtsrechteck (links, oben, Breite und Höhe), das die Position des Gesichts im Bild angibt, wird zusammen mit dem jeweils erfassten Gesicht zurückgegeben. Optional extrahiert die Gesichtserfassung eine Reihe gesichtsbezogener Attribute wie z.B. Pose, Geschlecht, Alter, Kopfhaltung, Gesichtsbehaarung und Brille. Weitere Informationen finden Sie unter [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="face-recognition"></a>Gesichtserkennung

Eine Gesichtserkennung ist in vielen Szenarien hilfreich. Hierzu zählen beispielsweise Sicherheit, natürliche Benutzeroberflächen, Analyse und Verwaltung von Bildinhalten sowie mobile Apps und Robotik. Der Gesichtserkennungs-API-Dienst bietet vier Gesichtserkennungsfunktionen: Gesichtsüberprüfung, Suchen ähnlicher Gesichter, Gesichtsgruppierung und Personenidentifizierung.

### <a name="face-verification"></a>Gesichtsüberprüfung

Bei der Gesichtsüberprüfung wird eine Authentifizierung anhand von zwei erfassten Gesichtern oder anhand eines erfassten Gesichts und eines Personenobjekts durchgeführt. Ausführlichere Informationen finden Sie unter [Face - Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

### <a name="finding-similar-faces"></a>Suchen ähnlicher Gesichter

Bei Vorgabe eines erfassten Zielgesichts und einer Reihe möglicher Gesichter für die Suche sucht der Dienst nach einer kleinen Gruppe von Gesichtern, die dem Zielgesicht am meisten ähneln. Zur Verfügung stehen zwei Modi: **matchFace** und **matchPerson**. Der Modus **matchPerson** gibt ähnliche Gesichter zurück, nachdem ein von [Face - Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) abgeleiteter Schwellenwert für die Personenübereinstimmung angewendet wurde. Beim Modus **matchFace** wird der Schwellenwert für die Personenübereinstimmung ignoriert, und es werden mögliche Gesichter mit der größten Ähnlichkeit zurückgegeben. Im folgenden Beispiel werden mögliche Gesichter aufgelistet.
![Übersicht – Suchen ähnlicher Gesichter](./Images/FaceFindSimilar.Candidates.jpg) Hier sehen Sie das Abfragegesicht:
![Übersicht – Suchen ähnlicher Gesichter](./Images/FaceFindSimilar.QueryFace.jpg)

Für die Suche nach vier ähnlichen Gesichtern gibt der Modus **matchPerson** (a) und (b) zurück. Diese zeigen die gleiche Person wie das Abfragegesicht. Der Modus **matchFace** gibt (a), (b), (c) und (d) zurück, also genau vier mögliche Gesichter, selbst wenn einige nur eine geringe Ähnlichkeit aufweisen. Weitere Informationen finden Sie unter [Face - Find Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

### <a name="face-grouping"></a>Gesichtsgruppierung

Bei Vorgabe einer Gruppe unbekannter Gesichter unterteilt die Gesichtsgruppierungs-API diese auf der Grundlage der Ähnlichkeit automatisch in mehrere Gruppen. Jede Gruppe ist eine separate echte Teilmenge der ursprünglichen Gruppe unbekannter Gesichter und enthält ähnliche Gesichter. Alle Gesichter in der gleichen Gruppe können als zur gleichen Person gehörig betrachtet werden. Weitere Informationen finden Sie unter [Face - Group](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

### <a name="person-identification"></a>Personenidentifizierung

Die Gesichtserkennungs-API kann zum Identifizieren von Personen auf Grundlage eines erfassten Gesichts und einer Personendatenbank verwendet werden. Die Datenbank wird im Voraus erstellt und kann im Laufe der Zeit bearbeitet werden.

Die folgende Abbildung zeigt ein Beispiel für eine Datenbank namens „myfriends“. Jede Gruppe kann bis zu 1.000.000/10.000 unterschiedliche Personenobjekte enthalten. Für jedes Personenobjekt können bis zu 248 Gesichter registriert sein.

![Übersicht – LargePersonGroup/PersonGroup](./Images/person.group.clare.jpg)

Nachdem eine Datenbank erstellt und trainiert wurde, kann anhand der Gruppe eine Identifizierung mit einem neu erfassten Gesicht durchgeführt werden. Wenn das Gesicht als eine Person aus der Gruppe erkannt wird, wird das Personenobjekt zurückgegeben.

Weitere Informationen zur Personenidentifizierung finden Sie in den folgenden API-Leitfäden:

[Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)  
[PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)  
[PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)  
[PersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249)  
[LargePersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)  
[LargePersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)  
[LargePersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4)  

#### <a name="face-storage-and-pricing"></a>Gesichtserkennungsspeicher und Preise

Mit dem Gesichtserkennungsspeicher kann ein Standardabonnement bei Verwendung von „LargePersonGroup/PersonGroup Person“-Objekten ([PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)/[LargePersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42)) oder „LargeFaceLists/FaceLists“ ([FaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250)/[LargeFaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3)) zusätzliche Gesichter für die Identifikation oder den Ähnlichkeitsabgleich mithilfe der Gesichtserkennungs-API speichern. Die gespeicherten Bilder werden mit 0,50 US-Dollar pro 1.000 Gesichter abgerechnet. Diese Rate wird täglich anteilig berechnet. Abonnements im Free-Tarif sind auf eine Gesamtanzahl von 1.000 Personen begrenzt.

Beispiel: In Ihrem Konto wurden in der ersten Monatshälfte täglich 10.000 Gesichter gespeichert und in der zweiten Monatshälfte keines. In diesem Fall werden Ihnen lediglich die 10.000 Gesichter in Rechnung gestellt, die für diese Tage gespeichert wurden. Wenn Sie jeden Tag im Monat 1.000 Gesichter für einige Stunden speichern und diese jede Nacht löschen, werden Ihnen dennoch 1.000 gespeicherte Gesichter in Rechnung gestellt.

## <a name="sample-apps"></a>Beispiel-Apps

Sehen Sie sich diese Beispielanwendungen an, bei denen die Gesichtserkennungs-API verwendet wird.

- [Gesichtserkennungs-API von Microsoft: Windows-Clientbibliothek und Beispiel](https://github.com/Microsoft/Cognitive-Face-Windows)
  - WPF-Beispiel-App, die mehrere Szenarien der Erfassung, Analyse und Identifizierung von Gesichtern veranschaulicht.
- [UWP-App FamilyNotes](https://github.com/Microsoft/Windows-appsample-familynotes)
  - UWP-Beispiel-App (Universelle Windows-Plattform), bei der die Verwendung von Sprache, Cortana, Freihand und Kamera in einem Szenario gemeinsam verwendeter Familiennotizen gezeigt wird.
- [Beispiel einer Videobildanalyse](https://github.com/microsoft/cognitive-samples-videoframeanalysis)
  - Win32-Beispiel-App, die das Analysieren von Videolivestreams nahezu in Echtzeit mithilfe von APIs für Gesichtserkennung, maschinelles Sehen und Emotionen veranschaulicht.

## <a name="tutorials"></a>Lernprogramme
Die folgenden Tutorials zeigen die grundlegenden Funktionen und Abonnementprozesse der Gesichtserkennungs-API:
- [Tutorial für erste Schritte mit der Gesichtserkennungs-API in C#](Tutorials/FaceAPIinCSharpTutorial.md)
- [Tutorial für erste Schritte mit der Gesichtserkennungs-API in Java für Android](Tutorials/FaceAPIinJavaForAndroidTutorial.md)
- [Tutorial für erste Schritte mit der Gesichtserkennungs-API in Python](Tutorials/FaceAPIinPythonTutorial.md)

## <a name="next-steps"></a>Nächste Schritte

Implementieren Sie anhand einer Schnellstartanleitung ein einfaches Szenario für die Gesichtserkennungs-API.
- [Schnellstart: Erkennen von Gesichtern in einem Bild mit C#](quickstarts/csharp.md) (auch für andere Sprachen verfügbar)
