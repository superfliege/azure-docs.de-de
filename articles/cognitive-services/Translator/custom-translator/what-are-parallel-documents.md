---
title: Was sind parallele Dokumente? - Custom Translator
titleSuffix: Azure Cognitive Services
description: Parallele Dokumente sind Dokumentpaare, bei denen ein Dokument die Übersetzung des anderen enthält. Das eine Dokument des Paars enthält also die Sätze in der Ausgangssprache und das andere die entsprechenden Sätze in der Zielsprache.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: e9214314bab40e3ce402fed84f429f3542519240
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51627031"
---
# <a name="what-are-parallel-documents"></a>Was sind parallele Dokumente?

Parallele Dokumente sind Dokumentpaare, bei denen ein Dokument die Übersetzung des anderen enthält. Das eine Dokument des Paars enthält also die Sätze in der Ausgangssprache und das andere die entsprechenden Sätze in der Zielsprache.
Es spielt keine Rolle, welche Sprache als „Quelle“ markiert ist und welche als „Ziel“. Mit einem parallelen Dokument kann ein Übersetzungssystem in beide Richtungen trainiert werden.

## <a name="use-of-parallel-documents"></a>Verwenden von parallelen Dokumenten

Parallele Dokumente werden vom System zu folgenden Zwecken verwendet:

1.  Um zu lernen, wie Wörter, Ausdrücke und Sätze zwischen den beiden Sprachen einander im Allgemeinen entsprechen bzw. zugeordnet werden.

2.  Um zu lernen, wie der jeweilige Kontext abhängig von den umgebenden Ausdrücke verarbeitet werden sollte. Manche Wörter müssen je nach Kontext unterschiedlich übersetzt werden.

Eine bewährte Methode besteht darin, eine 1:1-Entsprechung zwischen den Sätzen in den Quell- und Zielsprachversionen der Dokumente sicherzustellen.

Dokumente, die hochgeladen werden, sind private Dokumente der jeweiligen Arbeitsbereiche und können in beliebig vielen Projekten oder Trainingsvorgängen verwendet werden. Aus Ihren Dokumenten extrahierte Sätze werden separat als Unicode-Nur-Text-Dateien in Ihrem Repository gespeichert und können von Ihnen gelöscht werden. Verwenden Sie Custom Translator nicht als Dokumentrepository. Es ist nicht möglich, die Dokumente in dem Format herunterzuladen, in dem Sie sie hochgeladen haben.

## <a name="recommendations"></a>Empfehlungen

Wenn Ihr Projekt domänen- bzw. kategoriespezifisch ist, sollte die Terminologie dieser Kategorie in den Dokumenten konsistent verwendet werden. Die Qualität des resultierenden Übersetzungssystems hängt von der Anzahl von Sätzen in Ihren Dokumenten und von der Qualität der Sätze ab. Je mehr Beispiele für den unterschiedlichen Gebrauch eines kategoriespezifischen Worts Ihre Dokumente enthalten, desto besser kann es vom System übersetzt werden.

Sie benötigen mindestens 10.000 parallele Sätze zum Trainieren eines Systems. Als bewährte Methode empfehlen wir, kontinuierlich weitere parallele Inhalte hinzuzufügen und das Training erneut auszuführen, um die Qualität des Übersetzungssystems zu verbessern.

Gemäß den Bestimmungen von Microsoft dürfen in Custom Translator hochgeladene Dokumente die Copyright-, Schutz- und Urheberrechte von Dritten nicht verletzen. Weitere Informationen finden Sie in den [Nutzungsbestimmungen](https://azure.microsoft.com/support/legal/cognitive-services-terms/).
Durch das Hochladen eines Dokuments über das Portal ändern sich die Rechte am geistigen Eigentum des Dokuments selbst nicht.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie ein [Wörterbuch](what-is-dictionary.md) in Custom Translator verwenden.