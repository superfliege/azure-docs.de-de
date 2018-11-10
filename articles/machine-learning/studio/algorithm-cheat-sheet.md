---
title: Machine Learning Cheat Sheet für Algorithmen – Azure | Microsoft-Dokumentation
description: Eine druckbare Version des Machine Learning Cheat Sheet für Algorithmen, mit dem Sie den richtigen Algorithmus für Ihr Vorhersagemodell in Azure Machine Learning Studio auswählen können.
keywords: Algorithmus-Cheat Sheet,Cheat Sheet,Algorithmus für maschinelles Lernen
services: machine-learning
author: pakalra
ms.author: pakalra
manager: cgronlun
editor: cgronlun
ms.assetid: e1dc31ec-1acb-463f-ba77-de565d4ddf4d
ms.service: machine-learning
ms.component: studio
ms.topic: article
ms.date: 12/18/2017
ms.openlocfilehash: 4a6fdfec4c4c95ba47f17efeb0dc87521a86c03c
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51244983"
---
# <a name="machine-learning-algorithm-cheat-sheet-for-azure-machine-learning-studio"></a>Cheat Sheet mit Machine Learning-Algorithmen für Azure Machine Learning Studio

Mithilfe des **Cheat Sheet für Azure Machine Learning-Algorithmen** können Sie den richtigen Algorithmus für ein Predictive Analytics-Modell wählen.

[Azure Machine Learning Studio](https://studio.azureml.net/) bietet eine umfangreiche Bibliothek von Algorithmen der Typen ***Regression***, ***Klassifizierung***, ***Clustering*** und ***Erkennung von Anomalien***. Jede ist speziell auf eine andere Art von Machine Learning-Problem ausgelegt.

## <a name="download-machine-learning-algorithm-cheat-sheet"></a>Herunterladen: Cheat Sheet für Machine Learning-Algorithmen

**Das Cheat Sheet hier herunterladen:[ Cheat Sheet für Microsoft Azure Machine Learning-Algorithmen (28 x 43 cm) (in englischer Sprache)](https://download.microsoft.com/download/A/6/1/A613E11E-8F9C-424A-B99D-65344785C288/microsoft-machine-learning-algorithm-cheat-sheet-v7.pdf)**

![Cheat Sheet für Machine Learning-Algorithmen: Wie wähle ich einen Algorithmus für maschinelles Lernen aus?][cheat-sheet]

[cheat-sheet]: ./media/algorithm-cheat-sheet/machine-learning-algorithm-cheat-sheet-small_v_0_6-01.png

Sie können das Cheat Sheet für Machine Learning Studio-Algorithmen im Kleinformat herunterladen und drucken, um praktische Hilfe bei der Wahl eines Algorithmus zu erhalten.

> [!NOTE]
> Hilfe bei der Verwendung dieses Spickzettels für die Auswahl des richtigen Algorithmus sowie eine eingehendere Erläuterung der verschiedenen Typen von Algorithmen für maschinelles Lernen und ihrer Verwendung finden Sie unter [Auswählen von Algorithmen für Microsoft Azure Machine Learning](algorithm-choice.md).

## <a name="notes-and-terminology-definitions-for-the-machine-learning-studio-algorithm-cheat-sheet"></a>Hinweise und Terminologiedefinitionen für das Cheat Sheet für Machine Learning Studio-Algorithmen

* Die Vorschläge in diesem Cheat Sheet für Algorithmen stellen nur grobe Richtlinien dar. Einige können leicht abgeändert werden und andere sogar stark überarbeitet. Sie dienen lediglich als Ausgangspunkt und Anregung. Sie sollten auch direkte Vergleiche zwischen verschiedenen Algorithmen mit Ihren Daten durchführen. Es gibt keinen Ersatz für das grundsätzliche Verständnis der einzelnen Algorithmen und des Systems, das Ihre Daten generiert hat.

* Jeder Algorithmus für maschinelles Lernen hat seinen eigenen Stil oder *induktiven Bias*. Für ein bestimmtes Problem können verschiedene Algorithmen geeignet sein, aber ein Algorithmus passt möglicherweise besser als andere. Es ist jedoch nicht immer schon im Vorfeld klar, welches der am besten geeignete Algorithmus ist. In diesen Fällen werden im Cheat Sheet mehrere Algorithmen zusammen angegeben. Eine geeignete Strategie bestünde darin, einen Algorithmus zu testen und bei nicht zufriedenstellenden Ergebnissen einen anderen zu versuchen. Es folgt ein Beispiel aus dem [Azure AI-Katalog](http://gallery.azure.ai/). Dabei handelt es sich um ein Experiment, mit dem mehrere Algorithmen auf dieselben Daten angewendet und die Ergebnisse verglichen werden: [Vergleichen von Multiklassen-Klassifizierungen: Buchstabenerkennung](http://gallery.azure.ai/Details/a635502fc98b402a890efe21cec65b92).

* Es gibt drei Arten von maschinellem Lernen: **beaufsichtigtes Lernen**, **unbeaufsichtigtes Lernen** und **vertiefendes Lernen**.

  * Beim **beaufsichtigten Lernen** wird jeder Datenpunkt bezeichnet oder einer Kategorie oder einem Wert zugeordnet.  Ein Beispiel für eine Kategoriebezeichnung ist die Zuordnung eines Bildes zu "Katze" oder "Hund".  Ein Beispiel für eine Wertbezeichnung ist der Verkaufspreis für ein gebrauchtes Auto. Ziel beim beaufsichtigten Lernen ist es, viele bezeichnete Beispiele wie diese zu untersuchen und dann Vorhersagen zu zukünftigen Datenpunkten zu treffen. Zum Beispiel, um neue Fotos mit dem richtigen Tier zu identifizieren oder präzise Verkaufspreise für gebrauchte Pkw festzulegen. Dies ist eine häufige und nützliche Verwendung für das maschinelle Lernen. Alle Module in Azure Machine Learning sind Algorithmen zum beaufsichtigten Lernen, mit Ausnahme von [K-Means-Clustering][k-means-clustering].

  * Beim **unbeaufsichtigten Lernen** sind Datenpunkten keine Bezeichnungen zugeordnet. Stattdessen besteht das Ziel von Algorithmen zum unbeaufsichtigten Lernen im Organisieren der Daten in einer bestimmten Form oder in der Beschreibung ihrer Struktur. Dies kann das Gruppieren in Clustern bedeuten, wie bei K-Means, oder das Suchen nach unterschiedlichen Möglichkeiten zur Darstellung komplexer Daten in einfacherer Form.

  * Beim **vertiefenden Lernen** wählt der Algorithmus eine Aktion als Reaktion auf jeden Datenpunkt aus. Diese Vorgehensweise wird häufig in der Robotik angewendet. Dabei ist der Satz der Sensorenwerte zu einem bestimmten Zeitpunkt ein Datenpunkt, und der Algorithmus muss dann die nächste Aktion des Roboters auswählen. Diese Methode eignet sich auch für Anwendungen im Zusammenhang mit dem Internet der Dinge. Der Lernalgorithmus erhält außerdem kurz danach ein Erfolgssignal, das angibt, wie gut die Entscheidung war. Auf dieser Grundlage ändert der Algorithmus die Strategie zum Erreichen des bestmöglichen Ergebnisses. Derzeit bietet Azure ML keine Algorithmen zum vertiefenden Lernen.

* **Bayessche Methoden** machen Annahmen auf der Grundlage statistisch unabhängiger Datenpunkte. Dies bedeutet, dass die nicht modellierte Variabilität bei einem Datenpunkt nicht mit anderen in Zusammenhang gesetzt wird, d. h., es ist keine Vorhersage möglich. Wenn die aufgezeichneten Daten z. B. die Anzahl der Minuten bis zur Ankunft der nächsten U-Bahn ist, sind zwei Messungen im Abstand von einem Tag statistisch unabhängig voneinander. Allerdings sind zwei Messungen im Abstand von einer Minute nicht statistisch unabhängig – der eine Wert ist mithilfe des anderen Werts hochgradig vorhersagbar.

* Die **Boosted Decision Tree-Regression** nutzt die Vorteile der Funktionenüberlappung oder der Interaktion zwischen den Funktionen. Das bedeutet, dass an einen bestimmten Datenpunkt der Wert einer Funktion mithilfe des Werts einer anderen eingeschränkt vorhersagbar ist. Beispielsweise kann bei Daten zu den täglichen Tiefst- und Höchsttemperaturen mithilfe der Tiefsttemperatur für den Tag eine sinnvolle Prognose für die Höchsttemperatur getroffen werden. Die Informationen in den beiden Funktionen sind auf gewisse Weise redundant.

* Das Klassifizieren von Daten in mehr als zwei Kategorien kann entweder mithilfe einer inhärenten Multiklassen-Klassifizierung oder durch Kombination eines Satzes von Zweiklassen-Klassifizierungen in einem **Ensemble** durchgeführt werden. Bei der Vorgehensweise mithilfe eines Ensembles gibt es für jede Klasse eine separate Zweiklassen-Klassifizierung, die jeweils die Daten in zwei Kategorien unterteilen: „diese Klasse“ und „nicht diese Klasse“. Diese Klassifizierungen stimmen dann praktisch für die richtige Zuordnung des Datenpunkts ab. Dies ist das Prinzip hinter [One-vs-All Multiclass][one-vs-all-multiclass].

* Bei mehreren Methoden, einschließlich logistischer Regression und Bayes Point Machines, wird von **linearen Klassengrenzen** ausgegangen. Das heißt, dass die Grenzen zwischen den Klassen annähernd gerade Linien (oder Hyperebenen im allgemeineren Fall) sind. Oftmals kennen Sie Ihre Daten erst, nachdem Sie versucht haben, sie aufzuteilen, dies kann aber i. d. R. gut mit einer Visualisierung im Voraus erlernt werden. Wenn die Klassengrenzen sehr unregelmäßig sind, sollten Sie weiterhin Entscheidungsstrukturen, Decision Jungles, Support Vector Machines (SVM, Stützvektormethode) oder neuronale Netze verwenden.

* Sie können neuronale Netze mit kategorischen Variablen verwenden, indem Sie eine **Platzhaltervariable** für jede Kategorie erstellen und auf „1“ festlegen, wenn die Kategorie gilt, bzw. auf „0“, wenn sie nicht zutrifft.

## <a name="next-steps"></a>Nächste Schritte

* Eine herunterladbare Infografik, die Algorithmen beschreibt und Beispiele enthält, finden Sie unter [Herunterladbare Infografik: Grundlagen des maschinellen Lernens mit Algorithmusbeispielen](basics-infographic-with-algorithm-examples.md).

* Eine in Kategorien unterteilte Liste aller in Machine Learning Studio verfügbaren Algorithmen für maschinelles Lernen finden Sie in der Hilfe zu den Algorithmen und Modulen in Machine Learning unter [Modell initialisieren][initialize-model].

* Eine vollständige alphabetische Liste aller in Machine Learning Studio verfügbaren Algorithmen und Module finden Sie in der Hilfe zu den Algorithmen und Modulen in Machine Learning Studio unter [Modulliste von A bis Z für Machine Learning Studio][a-z-list].

* Informationen zum Herunterladen und Drucken des Diagramms, mit dem Sie sich einen Überblick über die Machine Learning Studio-Funktionen verschaffen können, finden Sie unter [Übersichtsdiagramm der Azure Machine Learning Studio-Funktionen](studio-overview-diagram.md).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

<!-- Module References -->
[a-z-list]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/a-z-module-list
[initialize-model]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/machine-learning-initialize-model
[k-means-clustering]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/k-means-clustering
[one-vs-all-multiclass]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/one-vs-all-multiclass
