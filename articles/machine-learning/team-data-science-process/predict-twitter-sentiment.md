---
title: Vorhersagen der Twitter-Stimmung mit Worteinbettungen unter Verwendung des Team Data Science-Prozesses in Azure | Microsoft-Dokumentation
description: "Erfahren Sie, welche Schritte zum Ausführen Ihrer Data Science-Projekte erforderlich sind."
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
ms.date: 12/15/2017
ms.author: bradsev;
ms.openlocfilehash: df1124ddb436f0cfeec8f4ed9728fa15278b9325
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="predict-twitter-sentiment-with-word-embeddings-by-using-the-team-data-science-process"></a>Vorhersagen der Twitter-Stimmung mit Worteinbettungen unter Verwendung des Team Data Science-Prozesses

In diesem Artikel wird gezeigt, wie Sie eine effiziente Zusammenarbeit des Worteinbettungsalgorithmus _Word2Vec_ und des _SSWE-Algorithmus (Sentiment-Specific Word Embedding)_ erzielen, um die Twitter-Stimmung mit [Azure Machine Learning](../preview/index.yml) vorherzusagen. Weitere Informationen zur Vorhersage der Twitter-Stimmungspolarität finden Sie im [MachineLearningSamples-TwitterSentimentPrediction-Repository](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction) auf GitHub. Der Schlüssel für eine effektive Zusammenarbeit bei Data Science-Projekten ist die Standardisierung der Struktur und Dokumentation von Projekten mit einem feststehenden Data Science-Lebenszyklus. Der [Team Data Science-Prozess (TDSP)](overview.md) bietet diese Art eines strukturierten [Lebenszyklus](lifecycle.md). 

Durch das Erstellen von Data Science-Projekten mit der _TDSP-Vorlage_ wird dieses standardisierte Framework für Azure Machine Learning-Projekte bereitgestellt. Das TDSP-Team hat bereits ein [GitHub-Repository für TDSP-Projektstruktur und -Vorlagen](https://github.com/Azure/Azure-TDSP-ProjectTemplate) veröffentlicht. Jetzt werden auch Machine Learning-Projekte unterstützt, die mit [TDSP-Vorlagen für Azure Machine Learning](https://github.com/amlsamples/tdsp) instanziiert wurden. Anweisungen zur Verwendung finden Sie unter [Strukturieren von Projekten mit Team Data Science-Prozessvorlagen](../preview/how-to-use-tdsp-in-azure-ml.md) in Azure Machine Learning. 


## <a name="twitter-sentiment-polarity-sample"></a>Beispiel für Twitter-Stimmungspolarität

In diesem Artikel wird ein Beispiel verwendet, um das Instanziieren und Ausführen eines Machine Learning-Projekts zu veranschaulichen. Das Beispiel verwendet TDSP-Struktur und -Vorlagen in Azure Machine Learning. Das vollständige Beispiel wird in [dieser exemplarischen Vorgehensweise](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/blob/master/docs/deliverable_docs/Step_By_Step_Tutorial.md) bereitgestellt. Die Modellierungsaufgabe sagt anhand des Texts von Tweets die Stimmungspolarität voraus (positiv oder negativ). In diesem Artikel werden die Datenmodellierungsaufgaben gezeigt, die in der exemplarischen Vorgehensweise beschrieben werden. Die exemplarische Vorgehensweise umfasst die folgenden Aufgaben:

- Datenuntersuchung, Training und Bereitstellung eines Machine Learning-Modells, das das Problem der Vorhersage in der Übersicht der Anwendungsfälle berücksichtigt. Für diese Aufgaben werden [Twitter-Stimmungsdaten](http://cs.stanford.edu/people/alecmgo/trainingandtestdata.zip) verwendet.
- Ausführung des Projekts unter Verwendung der TDSP-Vorlage aus Azure Machine Learning für dieses Projekt. Für Projektausführung und Berichterstellung wird der TDSP-Lebenszyklus verwendet.
- Operationalisierung der Lösung direkt aus Azure Machine Learning in Azure Container Service.

Das Projekt veranschaulicht die folgenden Features von Azure Machine Learning:

- Instanziierung und Verwendung der TDSP-Struktur.
- Codeausführung in Azure Machine Learning Workbench.
- Einfache Operationalisierung in Container Service durch Verwendung von Docker und Kubernetes.

## <a name="team-data-science-process"></a>Team Data Science-Prozess
Um dieses Beispiel auszuführen, verwenden Sie die TDSP-Projektstruktur und die Dokumentationsvorlagen in Azure Machine Learning Workbench. Das Beispiel implementiert den [TDSP-Lebenszyklus](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md), wie in der folgenden Abbildung dargestellt:

![TDSP-Lebenszyklus](./media/predict-twitter-sentiment/tdsp-lifecycle.PNG)

Das TDSP-Projekt wird basierend auf [diesen Anweisungen](https://github.com/amlsamples/tdsp/blob/master/docs/how-to-use-tdsp-in-azure-ml.md) in Azure Machine Learning Workbench erstellt, wie in der folgenden Abbildung gezeigt wird:

![Erstellen des TDSP-Projekts in Azure Machine Learning Workbench](./media/predict-twitter-sentiment/tdsp-instantiation.PNG) 


## <a name="data-acquisition-and-preparationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode01dataacquisitionandunderstanding"></a>[Datenabruf und -vorbereitung](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/01_data_acquisition_and_understanding)
Der erste Schritt in diesem Beispiel besteht darin, das sentiment140-Dataset herunterzuladen und die Daten in Trainings- und Testdaten zu unterteilen. Das sentiment140-Dataset enthält den eigentlichen Inhalt des Tweets (Emoticons wurden entfernt). Das Dataset enthält außerdem die Polarität für jeden Tweet (negativ = 0, positiv = 4), neutrale Tweets wurden entfernt. Nach Aufteilung der Daten umfassen die Trainingsdaten 1,3 Millionen Zeilen und die Testdaten 320.000 Zeilen.


## <a name="model-developmenthttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling"></a>[Modellentwicklung](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling)

Der nächste Schritt im Beispiel besteht darin, ein Modell für die Daten zu entwickeln. Die Modellierungsaufgabe ist in drei Teile gegliedert:

- Featureentwicklung: Durch Verwendung verschiedener Worteinbettungsalgorithmen werden Features für das Modell generiert. 
- Modellerstellung: Durch das Trainieren verschiedener Modelle wird die Stimmung des Eingabetexts vorhergesagt. Beispiele für diese Modelle sind die _logistische Regression_ und _Gradient Boosting_.
- Modellauswertung: Auswerten des trainierten Modells anhand der Testdaten.


### <a name="feature-engineeringhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling01featureengineering"></a>[Featureentwicklung](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/01_FeatureEngineering)

Die Algorithmen Word2Vec und SSWE werden verwendet, um Worteinbettungen zu generieren. 


#### <a name="word2vec-algorithm"></a>Word2Vec-Algorithmus

Der Word2Vec-Algorithmus wird im Skip-Gram-Modell verwendet. Dieses Modell wird im folgenden Aufsatz von Tomas Mikolov et al. erläutert: [Distributed Representations of Words and Phrases and their Compositionality. Advances in neural information processing systems.](https://arxiv.org/abs/1310.4546) (Verteilte Darstellung von Wörtern und Ausdrücken und ihre Kompositionalität. Fortschritte bei den neuronalen Systemen für die Informationsverarbeitung.), 2013.

Das Skip-Gram-Modell ist ein flaches neuronales Netzwerk. Die Eingabe ist das Zielwort, das als One-Hot-Vektor codiert ist und zur Vorhersage naheliegender Wörter verwendet wird. Wenn **V** der Umfang des Vokabulars ist, lautet der Umfang der Ausgabe __C×V__, wobei C die Größe des Kontextfensters angibt. Die folgende Abbildung zeigt eine Architektur, die auf dem Skip-Gram-Modell basiert:

![Skip-Gram-Modell](./media/predict-twitter-sentiment/skip-gram-model.PNG)

Die detaillierten Mechanismen des Word2Vec-Algorithmus und des Skip-Gram-Modells würden den Rahmen dieses Beispiels sprengen. Weitere Informationen finden Sie in den folgenden Referenzen:

- [02_A_Word2Vec.py-Code mit referenzierten TensorFlow-Beispiele](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/word2vec/word2vec_basic.py) 
- [Vector Representations of Words](https://www.tensorflow.org/tutorials/word2vec) (Vektordarstellungen von Wörtern)
- [How exactly does word2vec work?](http://www.1-4-5.net/~dmm/ml/how_does_word2vec_work.pdf) (Wie genau arbeitet word2vec?)
- [Notes on Noise Contrastive Estimation and Negative Sampling](http://demo.clab.cs.cmu.edu/cdyer/nce_notes.pdf) (Anmerkungen zu Noise Contrastive Estimation und negativen Stichproben)




#### <a name="sentiment-specific-word-embedding-algorithm"></a>Stimmungsspezifischer Worteinbettungsalgorithmus
Der SSWE-Algorithmus versucht, eine Schwäche des Word2Vec-Algorithmus zu beheben, die zur Folge hat, dass Wörter mit ähnlichem Kontext und umgekehrter Polarität ähnliche Wortvektoren aufweisen können. Die Ähnlichkeiten können dazu führen, dass der Word2Vec-Algorithmus bei Aufgaben wie beispielsweise der Stimmungsanalyse nicht genau arbeitet. Der SSWE-Algorithmus versucht, diese Schwäche zu beheben, indem sowohl die Satzpolarität als auch der Wortkontext in die Verlustfunktion eingehen.

Das Beispiel verwendet eine Variante des SSWE-Algorithmus wie folgt:

- Sowohl das ursprüngliche _N-Gramm_ als auch das fehlerhafte _N-Gramm_ werden als Eingaben verwendet.
- Eine Hinge-Verlustfunktion im Rangfolgenstil wird sowohl für den syntaktischen als auch für den semantischen Verlust eingesetzt.
- Die Ultimate-Verlustfunktion ist die gewichtete Kombination aus syntaktischem und semantischem Verlust.
- Zur Vereinfachung wird nur die semantische Kreuzentropie als Verlustfunktion verwendet.

Das Beispiel zeigt, dass selbst bei der vereinfachten Verlustfunktion die Leistung der SSWE-Einbettung besser ist als die der Word2Vec-Einbettung. Die folgende Abbildung zeigt das Konvolutionsmodell, das zum Generieren einer stimmungsspezifischen Worteinbettung verwendet wird:

![Neuronales Netzwerkmodell basierend auf SSWE](./media/predict-twitter-sentiment/embedding-model-2.PNG)

Nach Abschluss des Trainingsprozesses werden zwei Einbettungsdateien im TSV-Format (durch Tabstopp getrennte Werte) für die Modellierungsphasen generiert.

Weitere Informationen zum SSWE-Algorithmus finden Sie im folgenden Aufsatz von Duyu Tang, et al.: [Learning Sentiment-Specific Word Embedding for Twitter Sentiment Classification](http://www.aclweb.org/anthology/P14-1146) (Stimmungsspezifische Worteinbettung für die Twitter-Stimmungsklassifizierung). Association for Computational Linguistics (1), 2014.


### <a name="model-creationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling02modelcreation"></a>[Modellerstellung](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/02_ModelCreation)
Nachdem die Wortvektoren mithilfe des SSWE- oder Word2Vec-Algorithmus generiert wurden, werden die Klassifizierungsmodelle trainiert, um die tatsächliche Stimmungspolarität vorherzusagen. Zwei Featuretypen, Word2Vec und SSWE, werden auf zwei Modelle angewendet: auf das Gradient Boosting-Modell und auf das Modell der logistischen Regression. Als Ergebnis werden vier verschiedene Modelle trainiert.


### <a name="model-evaluationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling03modelevaluation"></a>[Modellauswertung](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/03_ModelEvaluation)
Nachdem die Modelle trainiert wurden, werden die Modelle zum Testen von Twitter-Textdaten und zum Auswerten der jeweiligen Modellleistung herangezogen. Das Beispiel wertet die folgenden vier Modelle aus:

- Gradient Boosting mit SSWE-Einbettung
- Logistische Regression mit SSWE-Einbettung
- Gradient Boosting mit Word2Vec-Einbettung
- Logistische Regression mit Word2Vec-Einbettung

Ein Vergleich der Leistung der vier Modelle ist in der folgenden Abbildung zusammengefasst:

![ROC-Modellvergleich (Receiver Operating Characteristic, Grenzwertoptimierung)](./media/predict-twitter-sentiment/roc-model-comparison.PNG)

Das Gradient Boosting-Modell mit SSWE-Feature liefert bei einem Vergleich der Modelle anhand der AUC-Metrik (Area Under Curve, Fläche unter der Kurve) die beste Leistung.


## <a name="deploymenthttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode03deployment"></a>[Bereitstellung](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/03_deployment)

Der letzte Schritt besteht darin, das trainierte Modell zur Stimmungsvorhersage für einen Webdienst in einem Cluster in Azure Container Service bereitzustellen. Das Beispiel verwendet als trainiertes Modell das Gradient Boosting-Modell mit SSWE-Einbettungsalgorithmus. Die Operationalisierungsumgebung stellt Docker und Kubernetes im Cluster bereit, um die Bereitstellung der Webdienste zu verwalten, siehe folgende Abbildung: 

![Kubernetes-Dashboard](./media/predict-twitter-sentiment/kubernetes-dashboard.PNG)

Weitere Informationen zum Operationalisierungsprozess finden Sie unter [Bereitstellen eines Machine Learning-Modells als Webdienst](https://docs.microsoft.com/azure/machine-learning/preview/model-management-service-deploy).

## <a name="conclusion"></a>Zusammenfassung

In diesem Artikel haben Sie erfahren, wie ein Worteinbettungsmodell unter Verwendung des Word2Vec- und des SSWE-Algorithmus trainiert wird. Die extrahierten Einbettungen wurden als Features zum Trainieren verschiedener Modelle herangezogen, um Stimmungsbewertungen für Twitter-Textdaten vorherzusagen. Das SSWE-Feature lieferte hierbei in Kombination mit dem Gradient Boosting-Modell die beste Leistung. Das Modell wurde anschließend unter Verwendung von Azure Machine Learning Workbench als Echtzeitwebdienst in Container Service bereitgestellt.


## <a name="references"></a>Referenzen

* [Team Data Science-Prozess](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview) 
* [Verwenden des Team Data Science-Prozesses (TDSP) in Azure Machine Learning](https://aka.ms/how-to-use-tdsp-in-aml)
* [TDSP-Projektvorlage für Azure Machine Learning](https://aka.ms/tdspamlgithubrepo)
* [Azure Machine Learning Workbench](https://docs.microsoft.com/azure/machine-learning/preview/)
* [Datensatz zum Einkommen in den USA aus dem UCI ML-Repository](https://archive.ics.uci.edu/ml/datasets/adult)
* [Erkennung von biomedizinischen Entitäten unter Verwendung der Team Data Science-Prozessvorlage (TDSP)](https://docs.microsoft.com/azure/machine-learning/preview/scenario-tdsp-biomedical-recognition)
* [Mikolov, Tomas, et al.: „Distributed representations of words and phrases and their compositionality. Advances in neural information processing systems“, 2013.](https://arxiv.org/abs/1310.4546)
* [Tang, Duyu, et al.: „Learning Sentiment-Specific Word Embedding for Twitter Sentiment Classification“, ACL (1), 2014.](http://www.aclweb.org/anthology/P14-1146)
