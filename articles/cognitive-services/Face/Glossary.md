---
title: Glossar – Gesichtserkennungs-API
titleSuffix: Azure Cognitive Services
description: In diesem Glossar werden Begriffe erläutert, auf die Sie möglicherweise beim Arbeiten mit der Gesichtserkennungs-API stoßen.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: bd4285a2d0f882b8c766563f2304031f1f6e2898
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59787939"
---
# <a name="glossary"></a>Glossar

## <a name="a"></a>Eine Datei

#### <a name="attributes"></a>Attribute

Attribute sind optionale Gesichtsmerkmale, die erkannt werden können, z.B. [Alter](#age-attribute), [Geschlecht](#gender-attribute), [Kopfhaltung](#head-pose-attribute), [Gesichtsbehaarung](#facial-hair-attribute) und [Lächeln](#smile-attribute). Sie können von der Erkennungs-API durch Angabe des Abfrageparameters _returnFaceAttributes_ abgerufen werden.

Eine vollständige Liste von Gesichtsmerkmalen finden Sie in der Referenzdokumentation: [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="age-attribute"></a>Alter (Attribut)

Alter ist eines der [Attribute](#attributes), die das Alter eines bestimmten Gesichts beschreiben. Das Attribut „Alter“ ist in den Erkennungsergebnissen optional und kann mit einer Erkennungsanforderung durch Festlegen des Parameters „returnFaceAttributes“ gesteuert werden.

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="c"></a>C

#### <a name="candidate"></a>Kandidat

Kandidaten sind im Wesentlichen [Identifikationsergebnisse](#identification) (z.B. identifizierte Personen und Vertrauenswürdigkeit in Erkennungen). Ein Kandidat wird durch [PersonID](#person-id) und [Vertrauen](#confidence) dargestellt, was darauf hinweist, dass die Person mit einem hohen Maß an Vertrauenswürdigkeit identifiziert wird.

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)

#### <a name="confidence"></a>Zuverlässigkeit

Vertrauen ist eine Maßeinheit, die die Ähnlichkeit zwischen [Gesichtern](#face) oder [Personen](#person) in numerischen Werten darstellt – die in [Identifikation](#identification) und [Überprüfung](#verification) verwendet werden, um die Ähnlichkeiten gesuchter, identifizierter sowie überprüfter Ergebnisse anzuzeigen.

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Find Similar (Gesicht – Ähnliches suchen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [Face – Identify (Gesicht – Identifizieren)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [Face – Verify (Gesicht – Überprüfen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="d"></a>D

#### <a name="detectionface-detection"></a>Erkennung/Gesichtserkennung

Gesichtserkennung ist die Aktion zum Auffinden von Gesichtern in Bildern. Benutzer können ein Bild hochladen oder eine Bild-URL in der Anforderung angeben. Die erkannten Gesichter werden mit [Gesichts-IDs](#face-id) zurückgegeben, die eine eindeutige Identität in der Gesichtserkennungs-API angeben. Die Rechtecke geben sowohl die Positionen der Gesichter im Bild in Pixeln an als auch die optionalen [Attribute](#attributes) für jedes Gesicht, wie z.B. [Alter](#age-attribute), [Geschlecht](#gender-attribute), [Kopfhaltung](#head-pose-attribute), [Barthaar](#facial-hair-attribute) und [Lächeln](#smile-attribute).

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="e"></a>E

#### <a name="emotion-attribute"></a>Emotion (Attribut)

Emotion ist eines der [Gesichtsattribute](#attributes). Wenn es abgefragt wird, wird eine Liste von Emotionen und deren Erkennungssicherheit für das jeweilige Gesicht zurückgegeben. Die Vertrauensbewertungen werden normalisiert: Die Bewertungen über alle Emotionen hinweg ergeben eine Summe von eins. Die zurückgegebenen Emotionen sind Glück, Traurigkeit, Neutralität, Wut, Verachtung, Ekel, Überraschung und Angst.

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="f"></a>F

#### <a name="face"></a>Gesicht

Gesicht ist ein einheitlicher Begriff für die von der Gesichtserkennungs-API im Zusammenhang mit erkannten Gesichtern abgeleiteten Ergebnisse. Letztlich wird ein Gesicht durch eine einheitliche Identität dargestellt ([Gesichts-ID](#face-id)), eine angegebene Region in Bildern ([Gesichtsrechteck](#face-rectangle)) und zusätzliche gesichtsbezogene Attribute, z.B. [Alter](#age-attribute), [Geschlecht](#gender-attribute), besondere Merkmale und [Kopfhaltung](#head-pose-attribute). Darüber hinaus können Gesichter von der Erkennung zurückgegeben werden.

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="face-api"></a>Gesichtserkennungs-API

Die Gesichtserkennungs-API ist eine cloudbasierte API, die die fortschrittlichsten Algorithmen zur Gesichtserkennung und -wiedererkennung bietet. Die wichtigsten Funktionen der Gesichtserkennungs-API können in zwei Kategorien unterteilt werden: Gesichtserkennung mit Attributen und [Gesichtswiedererkennung](#recognition).

Weitere Informationen finden Sie in der Referenzdokumentation: [Face API Overview (Übersicht zur Gesichtserkennungs-API)](./Overview.md), [Face – Detect (Gesicht – Erkennen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [Face – Find Similar (Gesicht – Ähnliches suchen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [Face – Group (Gesicht – Gruppe)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238), [Face – Identify (Gesicht – Identifizieren)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [Face – Verify (Gesicht – Überprüfen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

#### <a name="face-attributesfacial-attributes"></a>Gesichtsattribute

Siehe [Attribute](#attributes).

#### <a name="face-id"></a>Gesichts-ID

Die Gesichts-ID wird aus den Erkennungsergebnissen abgeleitet, in denen eine Zeichenfolge in der [Gesichtserkennungs-API](#face-api) ein [Gesicht](#face) darstellt.

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="face-landmarksfacial-landmarks"></a>Besondere Gesichtsmerkmale

Besondere Merkmale sind in den Erkennungsergebnissen optional. Es handelt sich dabei um semantische Gesichtsmerkmale wie Augen, Nase und Mund (in der folgenden Abbildung dargestellt). Besondere Merkmale können mit einer Erkennungsanforderung durch den booleschen Wert „returnFaceLandmarks“ gesteuert werden. Wenn für returnFaceLandmarks „True“ festgelegt ist, besitzen die zurückgegebenen Gesichter besondere Merkmale als Attribute.

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

![HowToDetectFace](./Images/landmarks.1.jpg)

#### <a name="face-rectangle"></a>Gesichtsrechteck

Das Gesichtsrechteck wird aus den Erkennungsergebnissen abgeleitet. Es ist ein aufrechtes Rechteck (links, oben, Breite, Höhe) in Bildern in Pixeln. Die obere linke Ecke eines [Gesichts](#face) (links, oben) gibt neben der Breite und Höhe die Größe von Gesichtern in X- bzw. Y-Achsenwerten an.

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="facial-hair-attribute"></a>Barthaar (Attribut)

Barthaar ist eines der zum Beschreiben der Bartlänge der verfügbaren Gesichter verwendeten [Attribute](#attributes). Das Attribut „Barthaar“ ist optional in den Erkennungsergebnissen und kann mit einer Erkennungsanforderung durch „returnFaceAttributes“ gesteuert werden. Wenn returnFaceAttributes „facialHair“ enthält, haben die zurückgegebenen Gesichter Barthaarattribute.

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="facelist"></a>FaceList

FaceList ist eine Sammlung von [PersistedFace](#persistedface) und die Einheit von [Suchen von Ähnlichem](#find-similar). Eine FaceList verfügt über eine [FaceList-ID](#facelist-id) wie andere Attribute, z.B. Name und Benutzerdaten.

Weitere Informationen finden Sie in der Referenzdokumentation: [FaceList – Create (FaceList – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b), [FaceList – Get (FaceList – Abrufen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c).

#### <a name="facelist-id"></a>FaceList-ID

FaceList-ID ist eine vom Benutzer bereitgestellte Zeichenfolge, die als Bezeichner einer [FaceList](#facelist) verwendet wird. Die FaceList-ID muss innerhalb des Abonnements eindeutig sein.

Weitere Informationen finden Sie in der Referenzdokumentation: [FaceList – Create (FaceList – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b), [FaceList – Get (FaceList – Abrufen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c).

#### <a name="find-similar"></a>Suchen von Ähnlichem

Diese API wird verwendet, um auf Grundlage einer Sammlung von Gesichtern ähnliche Gesichter zu suchen/abzufragen. Abfragegesichter und Sammlungen von Gesichtern werden als [Gesichts-IDs](#face-id) oder [FaceList-ID](#facelist-id)/[LargeFaceList-ID](#largefacelist-id) in der Anforderung dargestellt. Zurückgegebene Ergebnisse sind gesuchte ähnliche Gesichter, dargestellt durch [Gesichts-IDs](#face-id) oder PersistedFace-IDs.

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Find Similar (Face – Ähnliches suchen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [LargeFaceList – Create (LargeFaceList – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [FaceList – Create (FaceList – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b).

## <a name="g"></a>G

#### <a name="gender-attribute"></a>Geschlecht (Attribut)

Geschlecht ist eines der zum Beschreiben der Geschlechter der verfügbaren Gesichter verwendeten [Attribute](#attributes). Das Attribut „Geschlecht“ ist optional in den Erkennungsergebnissen und kann mit einer Erkennungsanforderung durch „returnFaceAttributes“ gesteuert werden. Wenn returnFaceAttributes „gender“ enthält, haben die zurückgegebenen Gesichter Geschlechtsattribute.

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="grouping"></a>Gruppierung

Gesichtergruppierung ist die Gruppierung einer Sammlung von Gesichtern nach Ähnlichkeiten. Sammlungen von Gesichtern werden durch die Gesichts-ID-Sammlungen in der Anforderung angegeben. Als Ergebnis der Gruppierung werden ähnliche Gesichter zu [Gruppen](#groups) zusammengestellt, und Gesichter, die keine Ähnlichkeit mit anderen Gesichtern aufweisen, werden als unübersichtliche Gruppe zusammengeführt. Es gibt höchstens eine [unübersichtliche Gruppe](#messy-group) im Gruppierungsergebnis.

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Group](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

#### <a name="groups"></a>Gruppen

Gruppen werden von den [Gruppierungsergebnissen](#grouping) abgeleitet. Jede Gruppe enthält eine Sammlung ähnlicher Gesichter, wobei Gesichter durch [Gesichts-IDs](#face-id) angezeigt werden.

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Group](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

## <a name="h"></a>H

#### <a name="head-pose-attribute"></a>Kopfhaltung (Attribut)

Kopfhaltung ist eines der [Attribute](#attributes), die die Ausrichtung des Gesichts im Raum wie in der folgenden Abbildung nach zur Seite geneigt, gehoben/gesenkt und gewendet darstellt. Der Wertebereich liegt für zur Seite geneigt und gewendet bei [-180, 180] und [-90, 90] Grad. In der aktuellen Version gibt die Erkennung für gehoben/gesenkt immer den Wert 0 zurück. Das Attribut „Kopfhaltung“ ist optional in den Erkennungsergebnissen und kann mit einer Erkennungsanforderung durch den Parameter „returnFaceAttributes“ gesteuert werden. Wenn der returnFaceAttributes-Parameter „headPose“ enthält, haben die zurückgegebenen Gesichter Kopfhaltungsattribute.

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

![GlossaryHeadPose](./Images/headpose.1.jpg)

## <a name="i"></a>I

#### <a name="identification"></a>Identifikation

Identifikation dient zum Identifizieren eines oder mehrerer Gesichter in einer LargePersonGroup/PersonGroup.
Eine [PersonGroup](#persongroup)/[LargePersonGroup](#largepersongroup) ist eine Sammlung von [Personen](#person).
Gesichter und die LargePersonGroup/PersonGroup werden jeweils durch [Gesichts-IDs](#face-id) und [LargePersonGroup-IDs](#largepersongroup-id)/[PersonGroup-IDs](#persongroup-id) in der Anforderung dargestellt.
Identifizierte Ergebnisse sind [Kandidaten](#candidate), dargestellt durch [Personen](#person) mit Vertrauen.
Mehrere Gesichter in der Eingabe werden separat betrachtet, und jedes Gesicht hat ein eigenes identifiziertes Ergebnis.

> [!NOTE]
> Die LargePersonGroup/PersonGroup sollte vor der Identifikation erfolgreich trainiert werden. Wenn die LargePersonGroup/PersonGroup nicht trainiert wird, oder der [Trainingsstatus](#status-train) nicht als „erfolgreich“ angezeigt wird (d.h. „wird ausgeführt“, „Fehler“ oder „Timeout“), ist die Anforderungsantwort 400.
> 

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Identify (Gesicht – Identifizieren)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [LargePersonGroup Person – Create (LargePersonGroup Person – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup – Create (LargePersonGroup – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup – Train (LargePersonGroup – Trainieren)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4), [PersonGroup Person – Create (PersonGroup Person – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [PersonGroup – Create (PersonGroup – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup – Train (PersonGroup – Trainieren)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249).

#### <a name="isidentical"></a>IsIdentical

IsIdentical ist ein boolesches Feld von [Überprüfungsergebnissen](#verification), das angibt, ob zwei Gesichter zur selben Person gehören.

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a)

## <a name="l"></a>L

#### <a name="landmarks"></a>Besondere Merkmale

Siehe „Besondere Gesichtsmerkmale“.

#### <a name="largefacelist"></a>LargeFaceList

LargeFaceList ist eine Sammlung von [PersistedFace](#persistedface) und die Einheit von [Suchen von Ähnlichem](#find-similar). Eine LargeFaceList verfügt über eine [LargeFaceList-ID](#largefacelist-id) wie andere Attribute, z.B. Name und Benutzerdaten.

Weitere Informationen finden Sie in der Referenzdokumentation: [LargeFaceList – Create (LargeFaceList – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [LargeFaceList – Get (LargeFaceList – Abrufen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a15827cd2de3616c086f2ce), [LargeFaceList – List Face (LargeFaceList – Gesicht auflisten)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158db4d2de3616c086f2d6).

#### <a name="largefacelist-id"></a>LargeFaceList-ID

LargeFaceList-ID ist eine vom Benutzer bereitgestellte Zeichenfolge, die als Bezeichner einer [LargeFaceList](#largefacelist) verwendet wird. Die LargeFaceList-ID muss innerhalb des Abonnements eindeutig sein.

Weitere Informationen finden Sie in der Referenzdokumentation: [LargeFaceList – Create (LargeFaceList – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [LargeFaceList – Get (LargeFaceList – Abrufen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a15827cd2de3616c086f2ce).

#### <a name="largepersongroup"></a>LargePersonGroup

LargePersonGroup ist eine Sammlung von [Personen](#person) und die Einheit von [Identifikation](#identification). Eine LargePersonGroup verfügt über eine [LargePersonGroup-ID](#largepersongroup-id) wie andere Attribute, z.B. Name und Benutzerdaten.

Weitere Informationen finden Sie in der Referenzdokumentation: [LargePersonGroup – Create (LargePersonGroup – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup – Get (LargePersonGroup – Abrufen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e), [LargePersonGroup Person – List (LargePersonGroup Person – Liste)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adda06ac60f11b48b5aa1).

#### <a name="largepersongroup-id"></a>LargePersonGroup-ID

LargePersonGroup-ID ist eine vom Benutzer bereitgestellte Zeichenfolge, die als Bezeichner einer [LargePersonGroup](#largepersongroup) verwendet wird. Die LargePersonGroup-ID muss innerhalb des Abonnements eindeutig sein.

Weitere Informationen finden Sie in der Referenzdokumentation: [LargePersonGroup – Create (LargePersonGroup – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup – Get (LargePersonGroup – Abrufen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e).

## <a name="m"></a>M

#### <a name="messy-group"></a>Unübersichtliche Gruppe

Eine unübersichtliche Gruppe ist aus den [Gruppierungsergebnissen](#grouping) abgeleitet. Sie enthält Gesichter, die keinem anderen Gesicht ähneln. Jedes Gesicht in einer unübersichtlichen Gruppe wird mit einer [Gesichts-ID](#face-id) angegeben.

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Group](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

## <a name="n"></a>N

#### <a name="name-person"></a>Name (Person)

Der Name ist eine benutzerfreundliche beschreibende Zeichenfolge für eine [Person](#person). Im Gegensatz zur [Person-ID](#person-id) kann der Name von Personen innerhalb einer Gruppe dupliziert werden.

Weitere Informationen finden Sie in der Referenzdokumentation: [LargePersonGroup Person – Create (LargePersonGroup Person – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup Person – Get (LargePersonGroup Person – Abrufen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0), [PersonGroup Person – Create (PersonGroup Person – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [PersonGroup Person – Get (PersonGroup Person – Abrufen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f).

#### <a name="name-largepersongrouppersongroup"></a>Name (LargePersonGroup/PersonGroup)

Der Name ist ebenfalls eine benutzerfreundliche beschreibende Zeichenfolge für [LargePersonGroup](#largepersongroup)/[PersonGroup](#persongroup). Im Gegensatz zur [LargePersonGroup-ID](#largepersongroup-id)/[PersonGroup-ID](#persongroup-id) kann der Name von LargePersonGroups/PersonGroups innerhalb eines Abonnements dupliziert werden.

Weitere Informationen finden Sie in der Referenzdokumentation: [LargePersonGroup – Create (LargePersonGroup – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup – Get (LargePersonGroup – Abrufen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e), [PersonGroup – Create (PersonGroup – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup – Get (PersonGroup – Abrufen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246).

## <a name="p"></a>P

#### <a name="persistedface"></a>PersistedFace

PersistedFace ist eine Datenstruktur in der Gesichtserkennungs-API. PersistedFace verfügt über eine [PersistedFace-ID](#persisted-face-id) wie andere Attribute, z. B. Name und Benutzerdaten.

Weitere Informationen finden Sie in der Referenzdokumentation: [LargeFaceList – Add Face (LargeFaceList – Gesicht hinzufügen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [FaceList – Add Face (FaceList – Gesicht hinzufügen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargePersonGroup Person – Add Face (LargePersonGroup Person – Gesicht hinzufügen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42), [PersonGroup Person – Add Face (PersonGroup Person – Gesicht hinzufügen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

#### <a name="persisted-face-id"></a>Persistierte Gesichtserkennungs-ID

Die persistierte Gesichtserkennungs-ID wird generiert, wenn ein [PersistedFace](#persistedface) erfolgreich erstellt wurde. Eine Zeichenfolge wird erstellt, um dieses Gesicht in der [Gesichtserkennungs-API](#face-api) darzustellen.

Weitere Informationen finden Sie in der Referenzdokumentation: [LargeFaceList – Add Face (LargeFaceList – Gesicht hinzufügen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [FaceList – Add Face (FaceList – Gesicht hinzufügen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargePersonGroup Person – Add Face (LargePersonGroup Person – Gesicht hinzufügen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42), [PersonGroup Person – Add Face (PersonGroup Person – Gesicht hinzufügen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

#### <a name="person"></a>Person

Person ist eine Datenstruktur, die in der Gesichtserkennungs-API verwaltet wird. Person verfügt über eine [Personen-ID](#person-id) wie andere Attribute, z.B. Name, eine Sammlung von [PersistedFace](#persistedface) und Benutzerdaten.

Weitere Informationen finden Sie in der Referenzdokumentation: [LargePersonGroup Person – Create (LargePersonGroup Person – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup Person – Get (LargePersonGroup Person – Abrufen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0), [PersonGroup Person – Create (PersonGroup Person – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [PersonGroup Person – Get (PersonGroup Person – Abrufen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f).

#### <a name="person-id"></a>Personen-ID

Die Personen-ID wird generiert, wenn eine [Person](#person) erfolgreich erstellt wurde. Eine Zeichenfolge wird erstellt, um diese Person in der [Gesichtserkennungs-API](#face-api) darzustellen.

Weitere Informationen finden Sie in der Referenzdokumentation: [LargePersonGroup Person – Create (LargePersonGroup Person – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup Person – Get (LargePersonGroup Person – Abrufen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0), [PersonGroup Person – Create (PersonGroup Person – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [PersonGroup Person – Get (PersonGroup Person – Abrufen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f).

#### <a name="persongroup"></a>PersonGroup

PersonGroup ist eine Sammlung von [Personen](#person) und die Einheit von [Identifikation](#identification). Eine PersonGroup verfügt über eine [PersonGroup-ID](#persongroup-id) wie andere Attribute, z.B. Name und Benutzerdaten.

Weitere Informationen finden Sie in der Referenzdokumentation: [PersonGroup – Create (PersonGroup – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup – Get (PersonGroup – Abrufen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246), [PersonGroup Person – List (PersonGroup Person – Liste)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241).

#### <a name="persongroup-id"></a>PersonGroup-ID

PersonGroup-ID ist eine vom Benutzer bereitgestellte Zeichenfolge, die als Bezeichner einer [PersonGroup](#persongroup) verwendet wird. Die Gruppen-ID muss innerhalb des Abonnements eindeutig sein.

Weitere Informationen finden Sie in der Referenzdokumentation: [PersonGroup – Create (PersonGroup – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup – Get (PersonGroup – Abrufen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246).

#### <a name="pose-attribute"></a>Haltung (Attribut)

Siehe [Kopfhaltung](#head-pose-attribute).

## <a name="r"></a>R

#### <a name="recognition"></a>Erkennung

Wiedererkennung ist ein beliebter Anwendungsbereich für Gesichtserkennungstechnologien, wie z.B. [Suchen von Ähnlichem](#find-similar), [Gruppieren](#grouping), [Identifikation](#identification), [Überprüfen, ob zwei Gesichter identisch sind oder nicht](#verification).

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Find Similar (Gesicht – Ähnliches suchen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [Face – Group (Gesicht – Gruppe)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238), [Face – Identify (Gesicht – Identifizieren)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [Face – Verify (Gesicht – Überprüfen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

#### <a name="rectangle-face"></a>Rechteck (Gesicht)

Siehe [Gesichtsrechteck](#face-rectangle).

## <a name="s"></a>S

#### <a name="similar-face-searching"></a>Suchen ähnlicher Gesichter

Siehe [Suchen von Ähnlichem](#find-similar).

#### <a name="smile-attribute"></a>Lächeln (Attribut)

Lächeln ist eines der zum Beschreiben des Gesichtsausdrucks der verfügbaren Gesichter verwendeten [Attribute](#attributes). Das Attribut „Lächeln“ ist optional in den Erkennungsergebnissen und kann mit einer Erkennungsanforderung durch „returnFaceAttributes“ gesteuert werden. Wenn returnFaceAttributes „smile“ enthält, haben die zurückgegebenen Gesichter das Attribut Lächeln.

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="snapshot"></a>Momentaufnahme

Eine Momentaufnahme ist ein temporärer Remotespeicher für bestimmte Arten von Gesichtserkennungsdaten. Die Momentaufnahme fungiert als eine Art Zwischenablage, um Daten von einem Abonnement in ein anderes zu kopieren. Zuerst erstellt der Benutzer eine Momentaufnahme der Daten im Quellabonnement. Anschließend wird die Momentaufnahme auf ein neues Datenobjekt im Zielabonnement angewendet. 

Weitere Informationen finden Sie in der [Anleitung zur Migration der Gesichtserkennung](./face-api-how-to-topics/how-to-migrate-face-data.md) sowie in der Referenzdokumentation (REST) unter [Momentaufnahme – Erstellen](/rest/api/cognitiveservices/face/snapshot/take) und [Momentaufnahme – Anwenden](/rest/api/cognitiveservices/face/snapshot/apply).

#### <a name="status-train"></a>Status (Trainieren)

Status ist eine Zeichenfolge einschließlich „notstarted“, „running“, „succeeded“, „failed“, die das Verfahren zum Trainieren von LargeFaceList/LargePersonGroups/PersonGroups beschreibt.

Weitere Informationen finden Sie in der Referenzdokumentation: [LargeFaceList – Train (LargeFaceList – Trainieren)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158422d2de3616c086f2d1), [LargePersonGroup – Train (LargePersonGroup – Trainieren)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4), [PersonGroup – Train (PersonGroup – Trainieren)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249).

#### <a name="subscription-key"></a>Abonnementschlüssel

Ein Abonnementschlüssel ist eine Zeichenfolge, die Sie als Abfragezeichenfolgenparameter benötigen, um die Gesichtserkennungs-API aufzurufen. Den Abonnementschlüssel finden Sie auf der Seite „Meine Abonnements“, nachdem Sie sich im Microsoft Cognitive Services-Portal angemeldet haben. Zwei Schlüssel sind jedem Abonnement zugeordnet: ein primärer und ein sekundärer Schlüssel. Beide können verwendet werden, um die API identisch aufzurufen. Sie müssen die Abonnementschlüssel sicher aufbewahren, und Sie können die Schlüssel auch jederzeit auf der Seite „Meine Abonnements“ neu generieren.

## <a name="t"></a>T

#### <a name="train-largefacelistlargepersongrouppersongroup"></a>Trainieren (LargeFaceList/LargePersonGroup/PersonGroup)

Diese API wird zur Vorverarbeitung von [LargeFaceList](#largefacelist)/[LargePersonGroup](#largepersongroup)/[PersonGroup](#persongroup) verwendet, um die Leistung von [Suchen nach Ähnlichem](#find-similar)/[Identifikation](#identification) sicherzustellen. Wenn das Training nicht betrieben oder der [Trainingsstatus](#status-train) als nicht erfolgreich angezeigt wird, führt die Identifikation für diese PersonGroup zu einem Fehler.

Weitere Informationen finden Sie in der Referenzdokumentation: [LargeFaceList – Train (LargeFaceList – Trainieren)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158422d2de3616c086f2d1), [LargePersonGroup – Train (LargePersonGroup – Trainieren)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4), [PersonGroup – Train (PersonGroup – Trainieren)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249), [Face – Identify (Gesicht – Identifizieren)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

## <a name="u"></a>U

#### <a name="userdatauser-data"></a>UserData/Benutzerdaten

Benutzerdaten sind zusätzliche Informationen, die [Personen](#person) und [PersonGroup](#persongroup)/[LargePersonGroup](#largepersongroup) zugeordnet sind. Benutzerdaten werden von Benutzern festgelegt, um Daten leichter verwenden, verstehen und im Gedächtnis behalten zu können.

Weitere Informationen finden Sie in der Referenzdokumentation: [LargePersonGroup – Create (LargePersonGroup – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup – Update (LargePersonGroup – Aktualisieren)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acfc83a7b9412a4d53f3f), [LargePersonGroup Person – Create (LargePersonGroup Person – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup Person – Update (LargePersonGroup Person – Aktualisieren)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ade043a7b9412a4d53f41), [PersonGroup – Create (PersonGroup – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup – Update (PersonGroup – Aktualisieren)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524a), [PersonGroup Person – Create (PersonGroup Person – Erstellen)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [PersonGroup Person – Update (PersonGroup Person – Aktualisieren)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395242).

## <a name="v"></a>V

#### <a name="verification"></a>Überprüfung

Mit dieser API wird überprüft, ob zwei Gesichter übereinstimmen oder nicht. Beide Gesichter werden in der Anforderung als IDs dargestellt. Überprüfte Ergebnisse enthalten ein boolesches Feld (isIdentical), das dasselbe angibt, wenn „True“, und ein Zahlenfeld ([Vertrauen](#confidence)), das das Maß an Vertrauenswürdigkeit angibt.

Weitere Informationen finden Sie in der Referenzdokumentation: [Face – Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a)
