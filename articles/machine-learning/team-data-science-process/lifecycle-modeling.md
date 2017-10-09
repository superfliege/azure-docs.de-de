---
title: "Modellierungsphase des Team Data Science-Prozess-Lebenszyklus – Azure | Microsoft-Dokumentation"
description: "Die Ziele, Aufgaben und Projektleistungen für die Modellierungsphase Ihrer Data Science-Projekte."
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/02/2017
ms.author: bradsev;
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 0c855faa96d7feb9f762ecaed7654669a5a8a5b7
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="modeling"></a>Modellierung

In diesem Thema werden die Ziele, Aufgaben und Projektleistungen in Zusammenhang mit der **Modellierungsphase** des Team Data Science-Prozesses behandelt. Dieser Prozess bietet einen empfohlenen Lebenszyklus, mit dem Sie Ihre Data Science-Projekte strukturieren können. Der Lebenszyklus beschreibt die wichtigsten Phasen, die Projekte typischerweise, oft iterativ, durchlaufen:

* **Geschäftliche Aspekte**
* **Datenerfassung und -auswertung**
* **Modellierung**
* **Bereitstellung**
* **Kundenakzeptanz**

Hier ist der **Team Data Science-Prozesslebenszyklus** grafisch dargestellt. 

![TDSP-Lebenszyklus 2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Ziele
* Optimale Datenfeatures für das Machine Learning-Modell.
* Ein informatives ML-Modell, mit dem das Ziel am genauesten vorhergesagt wird.
* Ein ML-Modell, das für die Produktion geeignet ist.

## <a name="how-to-do-it"></a>Vorgehensweise
In dieser Phase werden drei Hauptaufgaben durchgeführt:

* **Featureentwicklung**: Es werden Datenfeatures aus den Rohdaten erstellt, um das Trainieren des Modells zu ermöglichen.
* **Modelltraining**: Es wird nach dem Modell gesucht, mit dem die Frage am genauesten beantwortet wird, indem die Erfolgsmetriken verglichen werden.
* Ermitteln Sie, ob Ihr Modell **für die Produktion geeignet** ist.

### <a name="31-feature-engineering"></a>3.1 Featureentwicklung
Die Featureentwicklung umfasst die Einbeziehung, Aggregation und Transformation von Rohvariablen zum Erstellen der Features, die in der Analyse verwendet werden. Wenn Sie einen Einblick in die Grundlagen eines Modells erhalten möchten, müssen Sie verstehen, wie Features miteinander in Beziehung stehen und wie die Machine Learning-Algorithmen diese Features nutzen sollen. Dieser Schritt erfordert eine kreative Kombination aus Sachkenntnis und den im Datenuntersuchungsschritt gewonnenen Erkenntnissen. Dies ist ein Balanceakt, weil Variablen mit hohem Informationsgehalt gesucht und eingefügt werden, während gleichzeitig vermieden werden muss, zu viele irrelevante Variablen zu verwenden. Mit informativen Variablen verbessern wir unser Ergebnis, und mit wenig relevanten Variablen überladen wir das Modell unnötig. Sie müssen diese Features auch für alle neuen Daten generieren, die während der Bewertung ermittelt werden. Die Generierung dieser Features kann also nur von Daten abhängen, die zum Zeitpunkt der Bewertung verfügbar sind. Eine technische Anleitung zur Featureentwicklung bei Verwendung verschiedener Azure-Datentechnologien finden Sie unter [Featureentwicklung im Cortana-Analyseprozess](create-features.md). 

### <a name="32-model-training"></a>3.2 Modelltraining
Je nach Art der Frage, die beantwortet werden soll, stehen gegebenenfalls relativ viele Algorithmen für die Modellierung zur Verfügung. Eine Anleitung zur Auswahl der Algorithmen finden Sie unter [Auswählen von Algorithmen für Microsoft Azure Machine Learning](../studio/algorithm-choice.md). Dieser Artikel ist zwar für Azure Machine Learning geschrieben, aber die darin enthaltenen Informationen sind für alle Machine Learning-Projekte nützlich. 

Der Prozess für das Modelltraining umfasst die folgenden Schritte: 

* Sie können die Eingabedaten beliebig für die Modellierung eines Trainingsdatasets und eines Testdatasets **aufteilen**.
* **Erstellen Sie die Modelle** mit dem Trainingsdataset.
* Sie können eine Reihe von konkurrierenden Machine Learning-Algorithmen (Trainings- und Testdataset) zusammen mit den verschiedenen zugehörigen Optimierungsparametern (als Parameter-Sweeping bezeichnet) **auswerten**, die auf die Beantwortung der jeweiligen Fragen mit den aktuellen Daten ausgerichtet sind.
* **Ermitteln Sie die „beste“ Lösung** für die Beantwortung der Frage, indem Sie die Erfolgsmetrik für die alternativen Methoden vergleichen.

> [!NOTE]
> **Vermeiden von Datenlecks:** Datenlecks können dadurch verursacht werden, dass Daten von außerhalb des Trainingsdatasets einbezogen werden, sodass ein Modell oder ein Machine Learning-Algorithmus unrealistisch gute Vorhersagen ergibt. Datenlecks sind häufig der Grund dafür, warum Data Scientists nervös werden, wenn die Vorhersageergebnisse zu schön sind, um wahr zu sein. Es kann schwierig sein, diese Abhängigkeiten zu erkennen. Um Datenlecks zu vermeiden, ist häufig das wiederholte Durchlaufen der Schritte zur Erstellung eines Analyse-DataSets, Erstellung eines Modells und Auswertung der Genauigkeit erforderlich. 
> 
> 

Wir stellen für TDSP ein [Tool für die Automatisierung der Modellierung und Berichterstellung](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) bereit, mit dem mehrere Algorithmen und Parameter-Sweeping-Vorgänge durchlaufen werden können, um ein Grundmodell zu erhalten. Außerdem wird ein Basisbericht für die Modellierung erstellt, in dem die Leistung der einzelnen Modell/Parameter-Kombinationen, einschließlich Variablenwichtigkeit, zusammengefasst wird. Dieser Prozess ist ebenfalls iterativ, da er weitere Maßnahmen zur Featureentwicklung nach sich ziehen kann. 

## <a name="artifacts"></a>Artefakte
In dieser Phase werden die folgenden Artefakte produziert:

* [**Funktionssätze**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#feature-sets): Die für die Modellierung entwickelten Funktionen werden im Abschnitt **Feature Sets** (Funktionssätze) des Berichts **Datendefinition** beschrieben. Er enthält Verknüpfungen zum Code, mit dem die Features generiert werden, und eine Beschreibung zur Generierung der Features.
* [**Model Report**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md) (Modellbericht): Für jedes Modell, das ausprobiert wird, wird ein vorlagenbasierter Standardbericht mit Details zu den einzelnen Experimenten erstellt.
* **Prüfpunktentscheidung**: Werten Sie aus, ob das Modell gut genug funktioniert, um in einem Produktionssystem bereitgestellt zu werden. Einige wichtige Fragen lauten:
  * Kann die Frage mit dem Modell basierend auf den Testdaten ausreichend beantwortet werden? 
  * Sollten Alternativen ausprobiert werden: Erfassung weiterer Daten, weitere Featureentwicklung oder Experimente mit anderen Algorithmen?

## <a name="next-steps"></a>Nächste Schritte

Hier finden Sie Links zu jedem Schritt im Lebenszyklus des Team Data Science-Prozess:

* [1. Geschäftliche Aspekte](lifecycle-business-understanding.md)
* [2. Datenerfassung und -auswertung](lifecycle-data.md)
* [3. Modellierung](lifecycle-modeling.md)
* [4. Bereitstellung](lifecycle-deployment.md)
* [5. Kundenakzeptanz](lifecycle-acceptance.md)

Vollständige exemplarische Vorgehensweisen, in denen sämtliche Schritte im Prozess für **bestimmte Szenarien** gezeigt werden, sind ebenfalls verfügbar. Sie sind im Thema [Exemplarische Vorgehensweisen](walkthroughs.md) aufgeführt und mit Miniaturansichtsbeschreibungen verlinkt. Sie zeigen, wie Cloud- und lokale Tools und Dienste in einem Workflow oder einer Pipeline zum Erstellen einer intelligenten Anwendung kombiniert werden. 

Beispiele zum Ausführen der Schritte im Team Data Science-Prozess mit Azure Machine Learning Studio finden Sie im Lernpfad [Mit Azure ML](http://aka.ms/datascienceprocess). 
