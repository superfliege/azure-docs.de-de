---
title: Twitter-Stimmungsklassifizierung mit dem Azure Machine Learning-Paket für Textanalysen (AMLPTA) und dem Team Data Science-Prozess (TDSP) | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die Verwendung von TDSP (Team Data Science-Prozess) und AMLPTA für die Stimmungsklassifizierung.
services: machine-learning, team-data-science-process
documentationcenter: ''
author: deguhath
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: deguhath
ms.openlocfilehash: 9e5018bc4c7b90897f7f8c91169410284217b172
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45577005"
---
# <a name="twitter-sentiment-classification-with-azure-machine-learning-aml-package-for-text-analytics-amlpta-and-team-data-science-process-tdsp"></a>Twitter-Stimmungsklassifizierung mit dem Azure Machine Learning-Paket für Textanalysen (AMLPTA) und dem Team Data Science-Prozess (TDSP)

## <a name="introduction"></a>Einführung
Die Standardisierung der Struktur und Dokumentation von Data Science-Projekten, die in einem feststehenden [Data Science-Lebenszyklus](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md) verankert sind, erleichtern die effektive Zusammenarbeit in Data Science-Teams.

Wir haben zuvor ein [GitHub-Repository für die TDSP-Projektstruktur und -Vorlagen](https://github.com/Azure/Azure-TDSP-ProjectTemplate) veröffentlicht. Wir haben jetzt die Erstellung von Azure Machine Learning-Projekten aktiviert, die mit [TDSP-Struktur und -Dokumentationsvorlagen für Azure Machine Learning](https://github.com/amlsamples/tdsp) instanziiert sind. Anweisungen zur Verwendung von TDSP-Struktur und -Vorlagen in Azure Machine Learning werden [hier](https://docs.microsoft.com/azure/machine-learning/preview/how-to-use-tdsp-in-azure-ml) bereitgestellt. 

In diesem Beispiel veranschaulichen wir die Nutzung des Azure Machine Learning-Pakets für die Textanalyse und TDSP zur Entwicklung und Bereitstellung von Vorhersagemodellen für die Twitter-Stimmungsklassifizierung.


## <a name="use-case"></a>Anwendungsfall
### <a name="twitter-sentiment-polarity-sample"></a>Beispiel für Twitter-Stimmungspolarität
In diesem Artikel wird ein Beispiel verwendet, um das Instanziieren und Ausführen eines Machine Learning-Projekts zu veranschaulichen. Das Beispiel verwendet TDSP-Struktur und -Vorlagen in Azure Machine Learning. Das vollständige Beispiel wird in dieser exemplarischen Vorgehensweise bereitgestellt. Die Modellierungsaufgabe sagt anhand des Texts von Tweets die Stimmungspolarität voraus (positiv oder negativ). In diesem Artikel werden die Datenmodellierungsaufgaben gezeigt, die in der exemplarischen Vorgehensweise beschrieben werden. Die exemplarische Vorgehensweise umfasst die folgenden Aufgaben:

1. Datenuntersuchung, Training und Bereitstellung eines Machine Learning-Modells, das das Problem der Vorhersage in der Übersicht der Anwendungsfälle berücksichtigt. Für diese Aufgaben werden die Twitter-Stimmungsdaten verwendet.
2. Ausführung des Projekts unter Verwendung der TDSP-Vorlage aus Azure Machine Learning für dieses Projekt. Für Projektausführung und Berichterstellung wird der TDSP-Lebenszyklus verwendet.
3. Operationalisierung der Lösung direkt aus Azure Machine Learning in Azure Container Service.

Diese Projekt verdeutlicht die Verwendung des Textanalysepakets für Azure Machine Learning.


## <a name="link-to-github-repository"></a>Link zum GitHub-Repository
Den Link zum GitHub-Repository finden Sie [hier](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction). 

### <a name="purpose"></a>Zweck
Der Hauptzweck dieses Beispiels ist zu zeigen, wie ein Machine Learning-Projekt mit Team Data Science-Prozess (TDSP)-Struktur und -Vorlagen in Azure Machine Learning Workbench instanziiert und ausgeführt wird. Zu diesem Zweck verwenden wir [Twitter-Stimmungsdaten](http://cs.stanford.edu/people/alecmgo/trainingandtestdata.zip). Mit der Modellierungsaufgabe soll anhand des Texts von Tweets die Stimmungspolarität vorausgesagt werden (positiv oder negativ).

### <a name="scope"></a>Bereich
- Durchsuchen von Daten, Schulungen und Bereitstellung von Machine Learning-Modellen, die das Problem der Vorhersage in der Übersicht der Anwendungsfälle berücksichtigt.
- Die Projektausführung in Azure Machine Learning verwendet die Vorlage des Team Data Science-Prozesses (TDSP) aus Azure Machine Learning für dieses Projekt. Für die Ausführung und Berichterstattung des Projekts werden wir den TDSP-Lebenszyklus verwenden.
- Operationalisierung der Projektmappe direkt aus Azure Machine Learning in Azure Container Service.

Das Projekt hebt mehrere Funktionen von Azure Machine Learning hervor, wie z.B. die Instanziierung und Verwendung der TDSP-Struktur, die Ausführung von Code in Azure Machine Learning Workbench und die einfache Operationalisierung in Azure Container Services mit Docker und Kubernetes.

## <a name="team-data-science-process-tds"></a>Team Data Science-Prozess (TDSP)
Um dieses Beispiel auszuführen, verwenden Sie die TDSP-Projektstruktur und die Dokumentationsvorlagen. Es folgt dem [TDSP-Lebenszyklus](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/lifecycle). Das Projekt wird nach den [hier](https://github.com/amlsamples/tdsp/blob/master/docs/how-to-use-tdsp-in-azure-ml.md) bereitgestellten Anweisungen erstellt.


<img src="./media/predict-twitter-sentiment-amltextpackage/tdsp-lifecycle2.png" alt="tdsp-lifecycle" width="800" height="600">


## <a name="use-case-overview"></a>Übersicht über den Anwendungsfall
Die Aufgabe besteht darin, die binäre Polarität jeder einzelnen Tweet-Stimmung mithilfe von Wortmerkmalen vorherzusagen, die aus dem Tweet-Text extrahiert wurden. Eine ausführliche Beschreibung finden Sie in diesem [Repository](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction).

### <a name="data-acquisition-and-understandinghttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode01dataacquisitionandunderstanding"></a>[Datenerfassung und -auswertung](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/01_data_acquisition_and_understanding)
Der erste Schritt in diesem Beispiel besteht darin, das sentiment140-Dataset herunterzuladen und es in Trainings- und Testdaten zu unterteilen. Das sentiment140-Dataset enthält den eigentlichen Inhalt des Tweets (Emoticons wurden entfernt) und außerdem die Polarität für jeden Tweet (negativ = 0, positiv = 4), neutrale Tweets wurden entfernt. Nach Aufteilung der Daten umfassen die Trainingsdaten 1,3 Millionen Zeilen und die Testdaten 320.000 Zeilen.

### <a name="modelinghttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode02modeling"></a>[Modellierung](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/02_modeling)
Dieser Teil des Beispiels ist in drei Teile gegliedert: 
- **Featureentwicklung** entspricht die Generierung von Features durch Verwendung verschiedener Worteinbettungsalgorithmen, und zwar Word2Vec. 
- **Modellerstellung** beschäftigt sich mit dem Trainieren verschiedener Modelle wie _logistische Regression_ und _Gradient Boosting_, um die Stimmung des Eingabetexts vorherzusagen. 
- **Modellauswertung** wertet das trainierte Modell anhand der Testdaten aus.

#### <a name="feature-engineering"></a>Featureentwicklung
Wir verwenden <b>Word2Vec</b> zum Generieren der Worteinbettung. Zunächst verwenden wir den Algorithmus Word2Vec im Skip-Gram-Modus, der in diesem Aufsatz [Mikolov Tomas, Et al. Distributed representations of words and phrases and their compositionality. Advances in neural information processing systems. (Verteilte Darstellung von Wörtern und Ausdrücken und ihre Kompositionalität. Fortschritte bei den neuronalen Systemen für die Informationsverarbeitung.), 2013.](https://arxiv.org/abs/1310.4546) erläutert wird, um Worteinbettungen zu generieren.

Das Skip-Gram-Modell ist ein flaches neuronales Netzwerk, beim dem die Eingabe das Zielwort ist, das als One-Hot-Vektor codiert ist und zur Vorhersage naheliegender Wörter verwendet wird. Wenn V der Umfang des Vokabulars ist, lautet der Umfang der Ausgabe __C * V__, wobei C die Größe des Kontextfensters angibt. Die folgende Abbildung zeigt eine Architektur, die auf dem Skip-Gram-Modell basiert:
 
<table class="image" align="center">
<caption align="bottom">Skip-Gram-Modell</caption>
<tr><td><img src="https://s3-ap-south-1.amazonaws.com/av-blog-media/wp-content/uploads/2017/06/05000515/Capture2-276x300.png" alt="Skip-gram model"/></td></tr>
</table>

Die Details des Word2Vec-Algorithmus und des Skip-Gram-Modells würden den Rahmen dieses Beispiels sprengen. Interessierte Leser finden weitere Informationen in den folgenden Links: 02_A_Word2Vec.py-Code mit referenzierten [TensorFlow-Beispielen](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/word2vec/word2vec_basic.py)

* [Vector Representations of Words](https://www.tensorflow.org/tutorials/word2vec) (Vektordarstellungen von Wörtern)
* [How exactly does word2vec work?](http://www.1-4-5.net/~dmm/ml/how_does_word2vec_work.pdf) (Wie genau arbeitet word2vec?)
* [Notes on Noise Contrastive Estimation and Negative Sampling](http://demo.clab.cs.cmu.edu/cdyer/nce_notes.pdf) (Anmerkungen zu Noise Contrastive Estimation und negativen Stichproben)

Nach Abschluss des Trainingsprozesses werden zwei Einbettungsdateien im TSV-Format für die Modellierungsphasen generiert.

#### <a name="model-training"></a>Modelltraining
Nachdem die Wortvektoren mithilfe des SSWE- oder Word2vec-Algorithmus generiert wurden, werden im nächsten Schritt die Klassifizierungsmodelle trainiert, um die tatsächliche Stimmungspolarität vorherzusagen. Wir wenden zwei Arten von Features an: Word2Vec und SSWE in zwei Modellen: logistische Regression und Convolutional Neural Networks (CNN). 

#### <a name="model-evaluation"></a>Modellauswertung
Wir stellen Code zum Laden und Auswerten von mehreren trainierten Modelle mit Testdatensätzen bereit.


### <a name="deploymenthttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode03deployment"></a>[Bereitstellung](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/03_deployment)
In diesem Teil stellen wir Zeiger zu Anleitungen zum Operationalisieren eines vortrainierten Stimmungsvorhersagemodells für einen Webdienst auf einem Cluster im Azure Container Service (AKS) bereit. Die Operationalisierungsumgebung stellt Docker und Kubernetes im Cluster bereit, um die Bereitstellung der Webdienste zu verwalten. Weitere Informationen zum Operationalisierungsvorgang finden Sie [hier](https://docs.microsoft.com/azure/machine-learning/preview/model-management-service-deploy).

## <a name="conclusion"></a>Zusammenfassung
Wir haben uns die Details zum Trainieren eines Worteinbettungsmodells mithilfe von Word2Vec und der anschließenden Anwendung der extrahierten Einbettungen als Features zum Trainieren zwei verschiedener Modelle zur Vorhersage der Stimmungspunktzahl von Twitter-Textdaten angesehen. Eines dieser Modelle wird in Azure Container Services (so) bereitgestellt. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Einstieg finden Sie in der Dokumentation zum [Azure Machine Learning-Paket für Textanalysen (AMLPTA)](https://docs.microsoft.com/python/api/overview/azure-machine-learning/textanalytics?view=azure-ml-py-latest) und zum [Team Data Science-Prozess (TDSP)](https://aka.ms/tdsp).

## <a name="references"></a>Referenzen
* [Team Data Science-Prozess](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview) 
* [How to use Team Data Science Process (TDSP) in Azure Machine Learning (Verwenden von Team Data Science-Prozess (TDSP) in Azure Machine Learning)](https://aka.ms/how-to-use-tdsp-in-aml)
* [TDSP project template for Azure Machine Learning (TDSP-Projektvorlage für Azure Machine Learning)](https://aka.ms/tdspamlgithubrepo)
* [Azure ML Workbench](https://docs.microsoft.com/azure/machine-learning/preview/)
* [Mikolov, Tomas, et al.: Distributed representations of words and phrases and their compositionality. Advances in neural information processing systems. (Verteilte Darstellung von Wörtern und Ausdrücken und ihre Kompositionalität. Fortschritte bei den neuronalen Systemen für die Informationsverarbeitung.), 2013.](https://arxiv.org/abs/1310.4546)
