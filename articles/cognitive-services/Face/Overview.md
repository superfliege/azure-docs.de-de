---
title: Was ist die Gesichtserkennungs-API?
titleSuffix: Azure Cognitive Services
description: Es wird beschrieben, wie Sie den Gesichtserkennungsdienst zum Erkennen und Analysieren von Gesichtern in Bildern verwenden.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: overview
ms.date: 02/20/2019
ms.author: pafarley
ms.openlocfilehash: dcfb50c58f1205a5ab31c3fc6b9b22fdb503e4ec
ms.sourcegitcommit: 89b5e63945d0c325c1bf9e70ba3d9be6888da681
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57588957"
---
# <a name="what-is-the-azure-face-api"></a>Was ist die Azure-Gesichtserkennungs-API?

Die Azure-Gesichtserkennungs-API ist ein „Cognitive Service“, über den Algorithmen zum Erkennen und Analysieren von menschlichen Gesichtern in Bildern bereitgestellt werden. Die Möglichkeit der Verarbeitung von Informationen zu menschlichen Gesichtern ist in vielen verschiedenen Softwareszenarien wichtig, z.B. Sicherheit, natürliche Benutzeroberfläche, Bildinhaltsanalyse und -verwaltung, mobile Apps und Robotik.

Die Gesichtserkennungs-API verfügt über mehrere unterschiedliche Funktionen, die in den folgenden Abschnitten beschrieben werden. Weitere Informationen zu den einzelnen Funktionen finden Sie im Folgenden.

## <a name="face-detection"></a>Gesichtserkennung

Mit der Gesichtserkennungs-API können menschliche Gesichter in einem Bild erkannt und die Rechteckkoordinaten ihrer Positionen zurückgegeben werden. Optional kann die Gesichtserkennung eine Reihe gesichtsbezogener Attribute extrahieren, z.B. Pose, Kopfhaltung, Geschlecht, Alter, Emotionen, Gesichtsbehaarung und Brille.

> [!NOTE] 
> Das Feature für die Gesichtserkennung ist auch über die [Maschinelles Sehen-API](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) verfügbar, aber wenn Sie weitere Vorgänge für Gesichtsdaten durchführen möchten, sollten Sie die Gesichtserkennungs-API (dieser Dienst) nutzen.

![Bild von einer Frau und einem Mann mit Rechtecken um die Gesichter und Anzeige von Alter und Geschlecht](./Images/Face.detection.jpg)

Weitere Informationen zur Gesichtserkennung finden Sie in der [Anleitung zur Gesichtserkennung](face-api-how-to-topics/howtodetectfacesinimage.md) oder in der Referenzdokumentation zur [Erkennungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) (Detect API).

## <a name="face-verification"></a>Gesichtsüberprüfung

Mit der API für die Überprüfung wird eine Authentifizierung anhand von zwei erfassten Gesichtern oder anhand eines erfassten Gesichts und eines Personenobjekts durchgeführt. Es wird praktisch ausgewertet, ob zwei Gesichter zu derselben Person gehören. Dies kann für Sicherheitsszenarien nützlich sein. Weitere Informationen finden Sie unter [Verify API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) (Überprüfungs-API).

## <a name="find-similar-faces"></a>Suchen ähnlicher Gesichter

Bei der API vom Typ „Ähnliches suchen“ werden ein Zielgesicht und mehrere Kandidatengesichter verwendet, und es wird eine kleinere Gruppe von Gesichtern ermittelt, die dem Zielgesicht am ähnlichsten sehen. Es werden zwei Arbeitsmodi unterstützt: **matchPerson** und **matchFace**. Im Modus **matchPerson** werden ähnliche Gesichter zurückgegeben, nachdem nach derselben Person gefiltert wurde (mit der [Überprüfungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a)). Im Modus **matchFace** wird die Filterung nach derselben Person ignoriert und eine Liste mit ähnlichen Kandidatengesichtern zurückgegeben, die unter Umständen zu derselben Person gehören.

Im folgenden Beispiel wird dieses Zielgesicht verwendet:

![Lächelnde Frau](./Images/FaceFindSimilar.QueryFace.jpg)

Und dies sind die Kandidatengesichter:

![Fünf Bilder mit lächelnden Personen. Die Bilder a) und b) gehören zu derselben Person.](./Images/FaceFindSimilar.Candidates.jpg)

Für die Suche nach vier ähnlichen Gesichtern gibt der Modus **matchPerson** (a) und (b) zurück. Diese zeigen die gleiche Person wie das Zielgesicht. Im Modus **matchFace** werden (a), (b), (c) und (d) zurückgegeben, also genau vier Kandidaten – auch wenn es sich teilweise um andere Personen handelt oder die Ähnlichkeit nur gering ist. Weitere Informationen finden Sie unter [Find Similar API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) (API „Ähnliches suchen“).

## <a name="face-grouping"></a>Gesichtsgruppierung

Mit der API für die Gruppierung wird eine Gruppe von unbekannten Gesichtern anhand der Ähnlichkeit in mehrere Untergruppen aufgeteilt. Jede Gruppe ist eine zusammenhanglose korrekte Teilmenge der ursprünglichen Gesichtergruppe. Für alle Gesichter einer Gruppe ist die Wahrscheinlichkeit hoch, dass sie zu derselben Person gehören, aber es kann für eine Person auch mehrere unterschiedliche Gruppen geben (Unterscheidung durch einen anderen Faktor, z.B. den Ausdruck). Weitere Informationen finden Sie unter [Group API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) (Gruppierungs-API).

## <a name="person-identification"></a>Personenidentifizierung

Die API für die Identifizierung kann zum Identifizieren eines erkannten Gesichts anhand einer Datenbank mit Personen verwendet werden. Dies kann bei der automatischen Bildmarkierung von Software für die Fotoverwaltung hilfreich sein. Die Datenbank wird im Voraus erstellt und kann im Laufe der Zeit bearbeitet werden.

Die folgende Abbildung enthält ein Beispiel für eine Datenbank mit dem Namen „myfriends“. Jede Gruppe kann bis zu 1.000.000 unterschiedliche Personenobjekte enthalten, und für jedes Personenobjekt können bis zu 248 Gesichter registriert werden.

![Raster mit drei Spalten für unterschiedliche Personen, jeweils mit drei Zeilen mit Bildern von Gesichtern](./Images/person.group.clare.jpg)

Nachdem eine Datenbank erstellt und trainiert wurde, können Sie die Identifizierung eines neuen erkannten Gesichts für die Gruppe durchführen. Wenn das Gesicht als eine Person aus der Gruppe erkannt wird, wird das Personenobjekt zurückgegeben.

Weitere Informationen zur Personenidentifizierung finden Sie unter [Identify API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) (Identifizierungs-API).

## <a name="use-containers"></a>Verwenden von Containern

[Verwenden Sie den Gesichtserkennungscontainer](face-how-to-install-containers.md), um Gesichter zu erkennen und zu identifizieren. Installieren Sie dazu einen standardisierten Docker-Container, der sich näher bei Ihren Daten befindet.

## <a name="sample-apps"></a>Beispiel-Apps

In den folgenden Beispielanwendungen werden einige Vorgehensweisen veranschaulicht, die mit der Gesichtserkennungs-API möglich sind.

- [Microsoft Face API: Windows Client Library & Sample](https://github.com/Microsoft/Cognitive-Face-Windows) (Microsoft-Gesichtserkennungs-API: Windows-Clientbibliothek und -Beispiel): Eine WPF-App, mit der verschiedene Szenarien der Gesichtserkennung, -analyse und -identifizierung veranschaulicht werden.
- [FamilyNotes UWP app](https://github.com/Microsoft/Windows-appsample-familynotes) (FamilyNotes-UWP-App): Eine UWP-App (Universelle Windows-Plattform), bei der die Gesichtsidentifizierung zusammen mit Spracheingabe, Cortana, Freihand und Kamera in einem Szenario zum Austauschen von Familiennotizen genutzt wird.

## <a name="data-privacy-and-security"></a>Datenschutz und Sicherheit

Wie bei allen Cognitive Services-Diensten müssen Entwickler, die den Gesichtserkennungsdienst nutzen, die Microsoft-Richtlinien zu Kundendaten beachten. Weitere Informationen finden Sie im Microsoft Trust Center auf der [Seite zu Cognitive Services](https://www.microsoft.com/en-us/trustcenter/cloudservices/cognitiveservices).

## <a name="next-steps"></a>Nächste Schritte

Arbeiten Sie eine Schnellstartanleitung zum Implementieren eines einfachen Gesichtserkennungsszenarios im Code durch.
- [Schnellstart: Erkennen von Gesichtern in einem Bild mit dem .NET SDK und C#](quickstarts/csharp.md) (auch für andere Sprachen verfügbar)
