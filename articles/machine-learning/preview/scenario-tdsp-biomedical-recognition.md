---
title: "Erkennung von biomedizinischen Entitäten – Team Data Science-Prozess – Azure Machine Learning | Microsoft-Dokumentation"
description: "Schnellstart für Projekte des Team Data Science-Prozesses, bei dem Deep Learning zur Erkennung von biomedizinischen Entitäten in Azure Machine Learning Workbench verwendet wird."
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
ms.date: 09/10/2017
ms.author: bradsev
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: a13bbd5d32eaab96dfb97e60652dbe9bcbdfb1b1
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="biomedical-entity-recognition-using-team-data-science-process-tdsp-template"></a>Erkennung von biomedizinischen Entitäten unter Verwendung der Team Data Science-Prozessvorlage (TDSP)

Ziel dieses realen Szenarios ist es, zu verdeutlichen, wie mithilfe von Azure Machine Learning Workbench eine komplexere NLP-Aufgabe (Natural Language Processing, Verarbeitung natürlicher Sprache), z.B. die Extraktion von Entitäten aus unstrukturiertem Text, gelöst werden kann:

1. Trainieren eines neuronalen Modells für Worteinbettungen mit einem Textkorpus von ca. 18 Millionen PubMed-Abstracts unter Verwendung der [Spark Word2Vec-Implementierung](https://spark.apache.org/docs/latest/mllib-feature-extraction.html#word2vec)
2. Erstellen eines tiefgehenden LSTM-Modells (Long Short-Term Memory) eines rekurrenten neuronalen Netzes zur Entitätsextraktion auf einem GPU-fähigen virtuellen Azure Data Science-Computer (GPU DS VM) in Azure
2. Veranschaulichen, dass das Modell für bereichsspezifische Worteinbettungen Modelle für generische Worteinbettungen bei der Erkennung von Entitäten übertreffen kann 
3. Veranschaulichen des Trainings und der Operationalisierung von Deep Learning-Modellen mithilfe von Azure Machine Learning Workbench

4. Veranschaulichen der folgenden Funktionen in Azure Machine Learning Workbench:

    * Instanziierung von [TDSP-Strukturen und -Vorlagen](how-to-use-tdsp-in-azure-ml.md)
    * Ausführung von Code in Jupyter Notebooks sowie in Python-Skripts
    * Verfolgen des Ausführungsverlaufs für Python-Dateien
    * Ausführung von Aufträgen im Spark-Remoterechenkontext mithilfe von HDInsight Spark 2.1-Clustern
    * Ausführung von Aufträgen auf virtuellen GPU-Remotecomputern in Azure
    * Einfache Operationalisierung von Deep Learning-Modellen als Webdienste in Azure Container Service

## <a name="use-case-overview"></a>Übersicht über Anwendungsfälle
Die Erkennung biomedizinischer benannter Entitäten ist ein wichtiger Schritt für komplexe biomedizinische NLP-Aufgaben wie beispielsweise: 
* Extraktion von Krankheiten und Symptomen aus elektronischen medizinischen oder Gesundheitsdaten
* Erkennung von Medikamenten
* Aufzeigen der Interaktionen zwischen verschiedenen Entitätstypen, z.B. Interaktion zwischen Medikamenten, Beziehung zwischen Medikamenten und Krankheiten und Beziehung zwischen Genen und Proteinen

Der Anwendungsfall konzentriert sich darauf, wie ein Korpus von umfangreichen unstrukturierten Daten, z.B. Medline PubMed-Abstracts, für das Trainieren eines Modells für Worteinbettungen analysiert werden kann. Dann werden die Ausgabeeinbettungen als automatisch generierte Funktionen zum Trainieren einer neuronalen Entitätsextraktionsfunktion angesehen.

Unsere Ergebnisse zeigen, dass das Modell zur Extraktion von biomedizinischen Entitäten, das mit den bereichsspezifischen Worteinbettungen trainiert wird, das Modell übertrifft, das mit den generischen Einbettungen trainiert wird. Das bereichsspezifische Modell kann 7.012 (von 9.475) Entitäten mit dem F1-Maß 0,73 richtig erkennen im Vergleich zu 5.274 Entitäten mit dem F1-Maß 0,61 beim generischen Modell.

In der folgenden Abbildung ist die Architektur dargestellt, die zum Verarbeiten der Daten und Trainieren der Modelle verwendet wurde.

![Architektur](./media/scenario-tdsp-biomedical-recognition/architecture.png)

## <a name="data-description"></a>Datenbeschreibung

### <a name="1-word2vec-model-training-data"></a>1. Trainingsdaten für das Word2Vec-Modell
Wir haben zunächst die Rohdaten der MEDLINE-Abstracts von [MEDLINE](https://www.nlm.nih.gov/pubs/factsheets/medline.html) heruntergeladen. Die Daten stehen auf dem [FTP-Server](https://ftp.ncbi.nlm.nih.gov/pubmed/baseline) öffentlich als XML-Dateien zur Verfügung. Auf dem Server sind 892 XML-Dateien verfügbar. Jede der XML-Dateien umfasst Informationen von 30.000 Artikeln. Weitere Informationen zum Schritt der Datensammlung werden im Abschnitt zur Projektstruktur bereitgestellt. In jeder Datei sind folgende Felder vorhanden: 
        
        abstract
        affiliation
        authors
        country 
        delete: boolean if False means paper got updated so you might have two XMLs for the same paper.
        file_name   
        issn_linking    
        journal 
        keywords    
        medline_ta: this is abbreviation of the journal nam 
        mesh_terms: list of MeSH terms  
        nlm_unique_id   
        other_id: Other IDs 
        pmc: Pubmed Central ID  
        pmid: Pubmed ID
        pubdate: Publication date
        title

### <a name="2-lstm-model-training-data"></a>2. Trainingsdaten für das LSTM-Modell

Das neuronale Modell zur Extraktion von Entitäten wurde basierend auf öffentlich verfügbaren DataSets trainiert und ausgewertet. Eine ausführliche Beschreibung dieser DataSets finden Sie beispielsweise in den folgenden Quellen:
 * [Bio-Entity Recognition Task at BioNLP/NLPBA 2004](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/report.html)
 * [BioCreative V CDR task corpus](http://www.biocreative.org/tasks/biocreative-v/track-3-cdr/)
 * [Semeval 2013 - Task 9.1 (Drug Recognition)](https://www.cs.york.ac.uk/semeval-2013/task9/)

## <a name="link-to-the-azure-gallery-github-repository"></a>Link zum Azure-Katalog des GitHub-Repositorys
Es folgt der Link zum öffentlichen GitHub-Repository des realen Szenarios, das den Code und eine ausführliche Beschreibung umfasst: 

[https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction)


## <a name="prerequisites"></a>Voraussetzungen 

* Ein [Azure-Abonnement](https://azure.microsoft.com/free/)
* Azure Machine Learning Workbench. Weitere Informationen finden Sie in der [Installationsanweisung](quickstart-installation.md). Derzeit kann Azure Machine Learning Workbench nur unter folgenden Betriebssystemen installiert werden: 
    * Windows 10 oder Windows Server 2016
    * macOS Sierra (oder neuer)


### <a name="azure-services"></a>Azure-Dienste
* [HDInsight Spark-Cluster](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql) Version Spark 2.1 unter Linux (HDI 3.6) zur Berechnung mit horizontaler Skalierung. Zur Verarbeitung der nachfolgend erörterten vollständigen Menge an MEDLINE-Abstracts ist folgende Mindestkonfiguration erforderlich: 
    * Hauptknoten: Größe [D13_V2](https://azure.microsoft.com/pricing/details/hdinsight/)
    * Workerknoten: mindestens 4 der Größe [D12_V2](https://azure.microsoft.com/pricing/details/hdinsight/). Im Rahmen unserer Arbeit haben wir 11 Workerknoten der Größe D12_V2 verwendet.
* [Virtueller NC6-Data Science-Computer (DSVM)](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-linux-dsvm-intro) zur Berechnung mit horizontaler Skalierung

### <a name="python-packages"></a>Python-Pakete

Alle erforderlichen Abhängigkeiten sind in der Datei „aml_config/conda_dependencies.yml“ im Projektordner des Szenarios definiert. Die in dieser Datei definierten Abhängigkeiten werden für Ausführungen für Docker-, VM- und HDI-Cluster-Ziele automatisch bereitgestellt. Informationen zum Dateiformat der Conda-Umgebung finden Sie [hier](https://conda.io/docs/using/envs.html#create-environment-file-by-hand).

* [TensorFlow](https://www.tensorflow.org/install/)
* [CNTK 2.0](https://docs.microsoft.com/cognitive-toolkit/using-cntk-with-keras)
* [Keras](https://keras.io/#installation)
* NLTK
* Fastparquet

### <a name="basic-instructions-for-azure-machine-learning-aml-workbench"></a>Allgemeine Anweisungen für Azure Machine Learning (AML) Workbench
* [Übersicht](overview-what-is-azure-ml.md)
* [Installation](quickstart-installation.md)
* [Verwenden von TDSP](how-to-use-tdsp-in-azure-ml.md)
* [Lesen und Schreiben von Dateien](how-to-read-write-files.md)
* [Verwenden von Jupyter Notebooks](how-to-use-jupyter-notebooks.md)
* [Verwenden von GPU](how-to-use-gpu.md)

## <a name="scenario-structure"></a>Szenariostruktur
Für das Szenario verwenden wir die TDSP-Projektstruktur und die TDSP-Dokumentationsvorlagen (Abbildung 1), die dem [TDSP-Lebenszyklus](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md) folgen. Das Projekt wird nach den [hier](./how-to-use-tdsp-in-azure-ml.md) bereitgestellten Anweisungen erstellt.


![Eingeben von Projektinformationen](./media/scenario-tdsp-biomedical-recognition/instantiation-3.png) 

Die Schritte des Data Science-Workflows sehen wie folgt aus:

### <a name="1-data-acquisition-and-understanding"></a>1. Datenerfassung und -auswertung

Siehe [Datenerfassung und -auswertung](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/blob/master/Code/01_Data_Acquisition_and_Understanding/ReadMe.md).

Der Korpus der MEDLINE-Rohdaten umfasst insgesamt 27 Millionen Abstracts, wobei ungefähr 10 Millionen Artikel ein leeres Feld „abstract“ aufweisen. Mithilfe von Azure HDInsight Spark werden Big Data verarbeitet, die nicht als [Pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) in den Arbeitsspeicher eines einzelnen Computers geladen werden können. Zunächst werden die Daten in den Spark-Cluster heruntergeladen. Dann werden die folgenden Schritte im [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html) ausgeführt: 
* Analysieren der XML-Dateien mit dem Medline-XML-Parser
* Vorverarbeiten des Texts der Abstracts, einschließlich Satzteilung, Tokenisierung und Normalisierung der Groß-/Kleinschreibung
* Ausschließen von Artikeln, bei denen das Feld „abstract“ leer ist oder Kurztext enthält 
* Erstellen des Wortschatzes aus den Training-Abstracts
* Trainieren des neuronalen Modells für Worteinbettungen Weitere Informationen zu den ersten Schritten finden Sie unter dem [Link zum GitHub-Code](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/blob/master/Code/01_DataPreparation/ReadMe.md).


Nach der Analyse der XML-Dateien weisen die Daten das folgende Format auf: 

![Datenbeispiel](./media/scenario-tdsp-biomedical-recognition/datasample.png)

Das neuronale Modell zur Extraktion von Entitäten wurde basierend auf öffentlich verfügbaren DataSets trainiert und ausgewertet. Eine ausführliche Beschreibung dieser DataSets finden Sie beispielsweise in den folgenden Quellen:
 * [Bio-Entity Recognition Task at BioNLP/NLPBA 2004](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/report.html)
 * [BioCreative V CDR task corpus](http://www.biocreative.org/tasks/biocreative-v/track-3-cdr/)
 * [Semeval 2013 - Task 9.1 (Drug Recognition)](https://www.cs.york.ac.uk/semeval-2013/task9/)

### <a name="2-modeling"></a>2. Modellierung

Siehe [Modellierung](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/Code/02_Modeling).

Die Modellierung ist die Phase, in der wir zeigen, wie Sie anhand der im vorherigen Abschnitt heruntergeladenen Daten Ihr eigenes Model für Worteinbettungen trainieren und für andere Downstreamaufgaben verwenden können. Obwohl wir die PubMed Daten verwenden, ist die Pipeline zum Generieren der Einbettungen generisch und kann zum Trainieren von Worteinbettungen für andere Bereiche wiederverwendet werden. Damit Einbettungen eine genaue Darstellung der Daten liefern, ist es wichtig, dass das Word2Vec-Modell mit einer großen Datenmenge trainiert wird.
Nach dem Generieren der Worteinbettungen können wir ein tiefgehendes neuronales Netzmodell trainieren, in dem die gelernten Einbettungen verwendet werden, um die Einbettungsebene zu initialisieren. Wir markieren die Einbettungsebene als nicht trainierbar. Dies ist jedoch nicht obligatorisch. Das Training des Modells für Worteinbettungen wird nicht überwacht. Daher können wir nicht markierten Text nutzen. Das Training des Modells zur Erkennung von Entitäten ist jedoch eine überwachte Lernaufgabe, und seine Genauigkeit hängt von der Menge und der Qualität der manuell gekennzeichneten Daten ab. 


#### <a name="21-feature-generation"></a>2.1. Generieren von Funktionen

Siehe [Generieren von Funktionen](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/Code/02_Modeling/01_FeatureEngineering).

Word2Vec ist der Lernalgorithmus für Worteinbettungen, mit dem ein neuronales Netzmodell mit einem nicht markierten Trainingskorpus trainiert wird. Er generiert einen kontinuierlichen Vektor für jedes Wort im Korpus, der seine semantischen Informationen darstellt. Diese Modelle sind einfache neuronale Netze mit einer verborgenen Ebene. Die Wortvektoren/-einbettungen werden durch Rückpropagierung und das stochastische Gradientenverfahren gelernt. Es gibt zwei Arten von Word2Vec-Modellen: das Skip-Gram-Modell und das Continuous-Bag-of-Words-Modell (weitere Informationen finden Sie [hier](https://arxiv.org/pdf/1301.3781.pdf)). Da wir die MLlib-Implementierung des Word2Vec-Modells verwenden, das das Skip-Gram-Modell unterstützt, wird es hier kurz beschrieben (die Abbildung stammt von diesem [Link](https://ahmedhanibrahim.wordpress.com/2017/04/25/thesis-tutorials-i-understanding-word2vec-for-word-embedding-i/)): 

![Skip-Gram-Modell](./media/scenario-tdsp-biomedical-recognition/skip-gram.png)

Das Modell verwendet Hierarchical Softmax und Negative Sampling zur Optimierung der Leistung. Hierarchical Softmax (H-Softmax) ist eine an Binärbäume angelehnte Annäherung. H-Softmax ersetzt im Wesentlichen die flache Softmax-Ebene durch eine hierarchische Ebene, die Wörter als Blätter enthält. Dies ermöglicht uns, die Berechnung der Wahrscheinlichkeit eines Worts in eine Sequenz von Wahrscheinlichkeitsberechnungen zu zerlegen, sodass wir nicht die aufwendige Normalisierung für alle Wörter berechnen müssen. Da ein ausgeglichener Binärbaum eine Tiefe von log2(|V|) aufweist (V steht für das Vokabular), müssen wir für die endgültige Wahrscheinlichkeit eines Worts nur höchstens log2(|V|)-Knoten auswerten. Die Wahrscheinlichkeit eines Worts w entsprechend seinem Kontext c ist dann einfach das Produkt der Wahrscheinlichkeiten von rechten bzw. linken Abzweigungen, die zu seinem Blattknoten führen. Wir können einen Huffman-Baum basierend auf der Häufigkeit der Wörter im DataSet erstellen, um sicherzustellen, dass häufiger auftretende Wörter kürzere Darstellungen erhalten. Weitere Informationen finden Sie unter [diesem Link](http://sebastianruder.com/word-embeddings-softmax/).
Die Abbildung stammt von [hier](https://ahmedhanibrahim.wordpress.com/2017/04/25/thesis-tutorials-i-understanding-word2vec-for-word-embedding-i/).

##### <a name="visualization"></a>Visualisierung

Nach dem Generieren der Einbettungen möchten wir diese visualisieren und die Beziehung zwischen semantisch ähnlichen Wörtern anzeigen. 

![W2V-Ähnlichkeit](./media/scenario-tdsp-biomedical-recognition/w2v-sim.png)

Wir zeigen zwei verschiedene Möglichkeiten zur Visualisierung der Einbettungen. Bei der ersten wird der hochdimensionale Vektor mithilfe einer PCA in einen 2D-Vektorraum projiziert. Dies führt zu einem erheblichen Verlust von Informationen, sodass die Visualisierung nicht so genau ist. Bei der zweiten Möglichkeit wird die PCA mit [t-SNE](https://distill.pub/2016/misread-tsne/) verwendet. t-SNE ist ein Verfahren zur nicht linearen Dimensionalitätsreduzierung, das sich gut für die Einbettung hochdimensionaler Daten in einen Raum von zwei oder drei Dimensionen eignet, die dann in einem Punktdiagramm visualisiert werden können.  Jedes hochdimensionale Objekt wird durch einen zwei- oder dreidimensionalen Punkt so modelliert, dass ähnliche Objekte durch benachbarte Punkte und unterschiedliche Objekte durch entfernte Punkte modelliert werden. Dies erfolgt in zwei Teilen. Erstens wird eine Wahrscheinlichkeitsverteilung für die Paare im hochdimensionalen Raum so erstellt, dass ähnliche Objekte mit hoher Wahrscheinlichkeit und unterschiedliche Objekte mit geringer Wahrscheinlichkeit ausgewählt werden. Zweitens wird eine ähnliche Wahrscheinlichkeitsverteilung für die Punkte in einer niedrigdimensionalen Karte definiert und die KL-Divergenz zwischen den beiden Verteilungen in Bezug auf die Position der Punkte auf der Karte minimiert. Die Position der Punkte in der niedrigdimensionalen Karte ergibt sich durch Minimierung der KL-Divergenz mithilfe des Gradientenverfahrens. t-SNE ist jedoch möglicherweise nicht immer zuverlässig. Details zur Implementierung finden Sie [hier](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/Code/02_Modeling/01_FeatureEngineering). 


Wie in der folgenden Abbildung dargestellt, enthält die t-SNE-Visualisierung mehr Trennungen von Wortvektoren und potenziellen Clusteringmustern. 


* Visualisierung mit der PCA

![PCA](./media/scenario-tdsp-biomedical-recognition/pca.png)

* Visualisierung mit t-SNE

![t-SNE](./media/scenario-tdsp-biomedical-recognition/tsne.png)

* Am nächsten zu „cancer“ gelegene Verteilungspunkte (alles Untertypen von „cancer“)

![Am nächsten zu „cancer“ gelegene Verteilungspunkte](./media/scenario-tdsp-biomedical-recognition/nearesttocancer.png)

#### <a name="22-train-the-neural-entity-extractor"></a>2.2. Trainieren der neuronalen Entitätsextraktionsfunktion

Siehe [Trainieren der neuronalen Entitätsextraktionsfunktion](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/Code/02_Modeling/02_ModelCreation/ReadMe.md).

Die Architektur von neuronalen Feedforward-Netzen weist das Problem auf, dass alle Eingaben und Ausgaben unabhängig von den anderen Ein- und Ausgaben verarbeitet werden. Mit dieser Architektur können keine Sequence-to-Sequence-Bezeichnungsaufgaben wie maschinelle Übersetzung und Entitätsextraktion modelliert werden. In rekurrenten neuronalen Netzmodellen besteht dieses Problem nicht, da die bis zum aktuellen Zeitpunkt berechneten Informationen an den nächsten Knoten übergeben werden können. Diese Eigenschaft wird als Speicherkapazität im Netz bezeichnet, da es möglich ist, die zuvor berechneten Informationen zu verwenden, wie in der folgenden Abbildung dargestellt:

![Rekurrentes neuronales Netz](./media/scenario-tdsp-biomedical-recognition/rnn-expanded.png)

Einfache rekurrente neuronale Netze weisen das [Vanishing Gradient Problem](https://en.wikipedia.org/wiki/Vanishing_gradient_problem) (Problem der verschwindenden Gradienten) auf, aufgrund dessen sie alle zuvor erfassten Informationen nicht nutzen können. Das Problem tritt nur zutage, wenn für eine Vorhersage eine große Kontextmenge erforderlich ist. Modelle wie das LSTM-Modell weisen dieses Problem dagegen nicht auf. Im Gegenteil sind sie dafür ausgelegt, langfristige Abhängigkeiten zu berücksichtigen. Im Gegensatz zu einfachen rekurrenten neuronalen Einzelnetzen enthalten die LSTMs Interaktionen zwischen vier neuronalen Netzen für jede Zelle. Eine ausführliche Erläuterung der Funktionsweise von LSTM finden Sie in [diesem Beitrag](http://colah.github.io/posts/2015-08-Understanding-LSTMs/).

![LSTM-Zelle](./media/scenario-tdsp-biomedical-recognition/lstm-cell.png)

Versuchen wir, unser eigenes LSTM-basiertes rekurrentes neuronales Netz zu erstellen und Entitätstypen wie Medikamenten-, Krankheits- und Symptomnennungen aus den PubMed-Daten zu extrahieren. Im ersten Schritt gilt es, eine große Menge markierter Daten zu sammeln. Dies ist, wie Sie sicher vermuten, kein einfaches Unterfangen. Die meisten medizinischen Daten umfassen viele vertrauliche Informationen zu Personen und sind daher nicht öffentlich verfügbar. Wir verwenden eine Kombination aus zwei verschiedenen öffentlich verfügbaren DataSets. Das erste DataSet stammt von „Semeval 2013 - Task 9.1 (Drug Recognition)“ und das zweite von „BioCreative V CDR task corpus“. Wir kombinieren diese beiden DataSets und markieren sie automatisch, sodass wir Medikamente und Krankheiten aus medizinischen Texten extrahieren und die Worteinbettungen auswerten können. Details zur Implementierung finden Sie unter dem [Link zum GitHub-Code](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/Code/02_Modeling/02_ModelCreation).

Die Modellarchitektur, die wir für alle Codes und zum Vergleich verwendet haben, ist nachfolgend dargestellt. Der Parameter, der sich für verschiedene DataSets ändert, ist die maximale Sequenzlänge (hier 613).

![LSTM-Modell](./media/scenario-tdsp-biomedical-recognition/d-a-d-model.png)

#### <a name="23-model-evaluation"></a>2.3. Modellauswertung
Wir verwenden das Auswertungsskript aus der freigegebenen Aufgabe [Bio-Entity Recognition Task at Bio NLP/NLPBA 2004](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/report.html), um die Genauigkeit, die Sensitivität und das F1-Maß des Modells auszuwerten. 

#### <a name="in-domain-versus-generic-word-embedding-models"></a>Modell für bereichsspezifische Worteinbettungen und Modell für generische Worteinbettungen im Vergleich

Im Folgenden wird die Genauigkeit zweier Funktionstypen verglichen: (1) Worteinbettungen, die mit PubMed-Abstracts trainiert wurden, und (2) Worteinbettungen, die mit Google News trainiert wurden. Es ist deutlich zu erkennen, dass das bereichsspezifische Modell das generische Modell übertrifft. Daher ist die Verwendung eines spezifischen Modells sehr viel nützlicher als die Verwendung eines generischen Modells. 

* Aufgabe 1: Erkennung von Medikamenten und Krankheiten

![Modellvergleich 1](./media/scenario-tdsp-biomedical-recognition/mc1.png)

Wir führen die Auswertung der Worteinbettungen für andere DataSets auf ähnliche Weise durch und können feststellen, dass das bereichsspezifische Modell immer besser abschneidet.

* Aufgabe 2: Erkennung von Proteinen, Zelllinie, Zelltyp, DNA und RNA

![Modellvergleich 2](./media/scenario-tdsp-biomedical-recognition/mc2.png)

* Aufgabe 3: Erkennung von chemischen Substanzen und Krankheiten

![Modellvergleich 3](./media/scenario-tdsp-biomedical-recognition/mc3.png)

* Aufgabe 4: Erkennung von Medikamenten

![Modellvergleich 4](./media/scenario-tdsp-biomedical-recognition/mc4.png)

* Aufgabe 5: Erkennung von Genen

![Modellvergleich 5](./media/scenario-tdsp-biomedical-recognition/mc5.png)

#### <a name="tensorflow-versus-cntk"></a>TensorFlow und CNTK im Vergleich
Alle genannten Modelle werden unter Verwendung von Keras mit TensorFlow als Back-End trainiert. Keras mit CNTK-Back-End unterstützt zum Zeitpunkt dieser Arbeit keine „Umkehrung“. Aus diesem Grund haben wir zu Vergleichszwecken ein unidirektionales LSTM-Modell mit CNTK-Back-End trainiert und mit einem unidirektionalen LSTM-Modell mit TensorFlow-Back-End verglichen. Installieren Sie CNTK 2.0 für Keras entsprechend den [hier](https://docs.microsoft.com/cognitive-toolkit/using-cntk-with-keras) bereitgestellten Anweisungen. 

![Modellvergleich 6](./media/scenario-tdsp-biomedical-recognition/mc6.png)

Wir kamen zu dem Schluss, dass CNTK genauso gut funktioniert wie TensorFlow, sowohl im Hinblick auf die Trainingszeit pro Zeitraum (60 Sekunden für CNTK und 75 Sekunden für TensorFlow) als auch auf die Anzahl der erkannten Testentitäten. Wir verwenden die unidirektionalen Ebenen für die Auswertung.


### <a name="3-deployment"></a>3. Bereitstellung

Siehe [Bereitstellung](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/Code/03_Deployment).

Wir haben einen Webdienst in einem Cluster im [Azure Container Service (ACS)](https://azure.microsoft.com/services/container-service/) bereitgestellt. Die Operationalisierungsumgebung stellt Docker und Kubernetes im Cluster bereit, um die Bereitstellung der Webdienste zu verwalten. Weitere Informationen zum Operationalisierungsvorgang finden Sie [hier](model-management-service-deploy.md ).


## <a name="conclusion"></a>Zusammenfassung

Wir haben die Einzelheiten dazu erörtert, wie Sie ein Modell für Worteinbettungen unter Verwendung des Word2Vec-Algorithmus in Spark trainieren und dann die extrahierten Einbettungen als Funktionen zum Trainieren eines tiefgehenden neuronalen Netzes zur Entitätsextraktion verwenden können. Wir haben die Trainingspipeline auf den Bereich der Biomedizin angewendet. Die Pipeline ist jedoch ausreichend generisch, dass sie auch zum Erkennen benutzerdefinierter Entitätstypen in allen anderen Bereichen angewendet werden kann. Sie benötigen lediglich ausreichend Daten. Dann können Sie den hier vorgestellten Workflow auf einfache Weise für einen anderen Bereich anpassen.

## <a name="references"></a>Referenzen

* Tomas Mikolov, Kai Chen, Greg Corrado, Jeffrey Dean. 2013a. Efficient estimation of word representations in vector space. In Proceedings of ICLR.
* Tomas Mikolov, Ilya Sutskever, Kai Chen, Greg S Corrado, Jeff Dean. 2013b. Distributed representations of words and phrases and their compositionality. In Proceedings of NIPS, S. 3111–3119.
* Billy Chiu, Gamal Crichton, Anna Korhonen, Sampo Pyysalo. 2016. [How to Train Good Word Embeddings for Biomedical NLP](http://aclweb.org/anthology/W/W16/W16-2922.pdf), In Proceedings of the 15th Workshop on Biomedical Natural Language Processing, S. 166–174.
* [Vector Representations of Words](https://www.tensorflow.org/tutorials/word2vec) (Vektordarstellungen von Wörtern)
* [Recurrent Neural Networks](https://www.tensorflow.org/tutorials/recurrent) (Rekurrente neuronale Netze)
* [Problems encountered with Spark ml Word2Vec](https://intothedepthsofdataengineering.wordpress.com/2017/06/26/problems-encountered-with-spark-ml-word2vec/) (Probleme bei Spark ML Word2Vec)
* [Spark Word2Vec: lessons learned](https://intothedepthsofdataengineering.wordpress.com/2017/06/26/spark-word2vec-lessons-learned/) (Spark Word2Vec: Gewonnene Erkenntnisse)


