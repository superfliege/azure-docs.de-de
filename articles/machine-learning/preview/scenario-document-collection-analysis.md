---
title: "Analysieren von Dokumentsammlungen – Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine umfangreiche Sammlung von Dokumenten zusammenfassen und analysieren. Dazu gehören auch Techniken wie das Lernen von Ausdrücken, das Modellieren von Themen und die Themenmodellanalyse mithilfe der Azure ML Workbench."
services: machine-learning
documentationcenter: 
author: kehuan
ms.author: kehuan
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 669fc7a9ec5dfb446ef2755919c498fe6f60c9df
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="document-collection-analysis"></a>Analysieren von Dokumentsammlungen

Dieses Szenario veranschaulicht, wie Sie eine umfangreiche Sammlung von Dokumenten zusammenfassen und analysieren. Dazu gehören auch Techniken wie das Lernen von Ausdrücken, das Modellieren von Themen und die Themenmodellanalyse mithilfe der Azure ML Workbench. Die Azure Machine Learning Workbench ermöglicht das einfache zentrale Hochskalieren sehr großer Dokumentsammlungen und stellt Verfahren zum Trainieren und Optimieren von Modellen in einer Vielzahl von Rechenkontexten bereit – angefangen bei lokalen Computeoperationen bis hin zu Data Science bei VMs in Spark-Clustern. Die einfache Entwicklung wird über Jupyter Notebooks in der Azure Machine Learning Workbench gewährleistet.

## <a name="link-to-the-gallery-github-repository"></a>Link zum Katalog des GitHub-Repositorys

Das öffentliche GitHub-Repository für dieses reale Szenario enthält alle Materialien, die in diesem Beispiel benötigt werden, einschließlich Codebeispielen:

[https://github.com/Azure/MachineLearningSamples-DocumentCollectionAnalysis](https://github.com/Azure/MachineLearningSamples-DocumentCollectionAnalysis)

## <a name="overview"></a>Übersicht

Wenn jeden Tag eine große Menge von Daten (insbesondere unstrukturierte Textdaten) gesammelt wird, stellt das Organisieren, Durchsuchen und Analysieren der großen Textmengen eine erhebliche Herausforderung dar. Dieses Szenario zur Analyse von Dokumentsammlungen veranschaulicht einen effizienten und automatisierten vollständigen Workflow für das Analysieren großer Dokumentsammlungen und das Aktivieren von Downstream-NLP-Tasks.

Die Hauptaspekte, die in diesem Szenario behandelt werden:

1. Lernen der wichtigsten Wortgruppen in Dokumenten

1. Ermitteln der zugrunde liegenden Themen in der Dokumentsammlung

1. Darstellen von Dokumenten nach thematischer Verteilung

1. Präsentieren von Methoden zum Organisieren, Durchsuchen und Zusammenfassen von Dokumenten anhand der thematischen Inhalte

Die in diesem Szenario gezeigten Methoden können für eine Vielzahl von wichtigen industriellen Workloads verwendet werden. Dies umfasst z.B. die Ermittlung von Abweichungen zu Thementrends, die Zusammenfassung von Dokumentsammlungen und die Suche nach ähnlichen Dokumenten. Sie können auf viele verschiedene Arten von Dokumentanalysen wie staatliche Gesetzgebung, Nachrichten, Produktbewertungen, Kundenfeedback und wissenschaftliche Forschungsartikel angewendet werden.

Folgende Techniken/Algorithmen zum maschinellen Lernen werden in diesem Szenario verwendet:

1. Textverarbeitung und -bereinigung

1. Ausdrucklernen

1. Themenmodellierung

1. Textzusammenfassung

1. Thementrend- und Anomalieerkennung

## <a name="prerequisites"></a>Voraussetzungen

Folgende Voraussetzungen müssen zum Ausführen dieses Beispiels erfüllt sein:

* Stellen Sie sicher, dass Sie [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) ordnungsgemäß installiert haben. Gehen Sie dazu wie in der [Schnellstartanleitung zum Installieren und Erstellen](quickstart-installation.md) beschrieben vor.

* Dieses Beispiel kann in beliebigen Rechenkontexten ausgeführt werden. Es wird jedoch empfohlen, für die Ausführung einen Computer mit mehreren Kernen und mindestens 16 GB Arbeitsspeicher und 5 GB Speicherplatz auf dem Datenträger zu verwenden.

## <a name="create-a-new-workbench-project"></a>Erstellen eines neuen Workbench-Projekts

Erstellen Sie ein neues Projekt mit diesem Beispiel als Vorlage:
1.  Öffnen Sie Azure Machine Learning Workbench.
2.  Klicken Sie auf der Seite **Projekte** auf **+**, wählen Sie **Neues Projekt** aus, und signieren Sie es.
3.  Geben Sie die Informationen für das neue Projekt im Bereich **Neues Projekt erstellen** ein.
4.  Geben Sie im Suchfeld **Projektvorlagen suchen** den Suchbegriff „Document Collection Analysis“ (Dokumentsammlungsanalyse) ein, und wählen Sie die Vorlage aus.
5.  Klicken Sie auf **Erstellen**

## <a name="data-description"></a>Datenbeschreibung

Das in diesem Szenario verwendete DataSet enthält Textzusammenfassungen und die zugehörigen Metadaten für jede gesetzgebende Handlung des US-Kongresses. Die Datenerfassung erfolgt durch [GovTrack.us](https://www.govtrack.us/). Die Site verfolgt alle Aktivitäten des United States Congress und hilft US-Bürgern damit, an der nationalen Gesetzgebung teilzuhaben. Die Massendaten können über [diesen Link](https://www.govtrack.us/data/congress/) mithilfe eines manuellen Skripts heruntergeladen werden, das in diesem Szenario nicht enthalten ist. Informationen zum Herunterladen der Daten finden Sie in der [GovTrack-API-Dokumentation](https://www.govtrack.us/developers/api).

### <a name="data-source"></a>Datenquelle

In diesem Szenario enthalten die unformatierten Daten eine Reihe von legislativen Maßnahmen, die vom US-Kongress (Gesetzesvorschläge und Beschlüsse) zwischen 1973 und Juni 2017 (93. bis 115. Kongress) eingeführt wurden. Die gesammelten Daten liegen im JSON-Format vor und enthalten einen umfangreichen Satz von Informationen zu den legislativen Maßnahmen. Ausführliche Beschreibung der Datenfelder finden Sie unter [diesem GitHub-Link](https://github.com/unitedstates/congress/wiki/bills). Zur Veranschaulichung in diesem Szenario wurde nur eine Teilmenge der Datenfelder aus den JSON-Dateien extrahiert. Eine vorkompilierte TSV-Datei (`CongressionalDataAll_Jun_2017.tsv`) mit Datensätzen der entsprechenden legislativen Maßnahmen wird für dieses Szenario bereitgestellt. Die TSV-Datei kann über die Notebooks `1_Preprocess_Text.ipynb` im Ordner „notebook“ oder mit `preprocessText.py` im Python-Paket automatisch heruntergeladen werden.

### <a name="data-structure"></a>Datenstruktur

Es gibt neun Datenfelder in der Datendatei. Die Datenfeldnamen und Beschreibungen werden im Folgenden aufgelistet.

| Feldname | Typ | Beschreibung | Enthält fehlende Werte |
|------------|------|-------------|---------------|
| `ID` | String | Die ID des Gesetzes bzw. Beschlusses. Das Format dieses Felds lautet [Gesetzesart][Nummer]-[Kongress]. „hconres1-93“ bedeutet z.B., dass die Gesetzesart „hconres“ (für „House Concurrent Resolution“, siehe [dieses Dokument](https://github.com/unitedstates/congress/wiki/bills#basic-information)), die Gesetznummer „1“ und die Kongressnummer „93“ lauten. | Nein |
| `Text` | String | Der Inhalt des Gesetzes bzw. Beschlusses. | Nein |
| `Date` | String | Das Datum, an dem das Gesetz/der Beschluss ursprünglich eingereicht wurde. Im Format „jjjj-mm-tt“. | Nein |
| `SponsorName` | String | Der Name des ursprünglichen Unterstützers, der das Gesetz/den Beschluss eingereicht hat. | Ja |
| `Type` | String | Der Titeltyp des primären Unterstützers, entweder „rep“ (Representative) oder „s“ (Senator). | Ja |
| `State` | String | Der Status des ursprünglichen Unterstützers. | Ja |
| `District` | Integer | Die Distriktnummer des ursprünglichen Unterstützers, wenn der Titeltyp „Representative“ ist. | Ja |
| `Party` | String | Die Partei des ursprünglichen Unterstützers. | Ja |
| `Subjects` | String | Die Themenbegriffe, die dem Gesetz von der Library of Congress kumulativ hinzugefügt wurden. Die Ausdrücke werden durch Kommas getrennt. Diese Begriffe werden von einer Person in der Library of Congress geschrieben und sind in der Regel noch nicht enthalten, wenn die Informationen zum Gesetz das erste Mal veröffentlicht werden. Sie können jederzeit hinzugefügt werden. Daher ist es möglich, dass einige Begriffe am Ende des Lebenszyklus eines Gesetzes nicht mehr relevant sind. | Ja |

## <a name="scenario-structure"></a>Szenariostruktur

Das Analysebeispiel für die Dokumentsammlung ist in zwei Arten von Ergebnissen unterteilt. Der erste Typ ist eine Reihe von IPython Notebooks, die ausführliche Beschreibungen des gesamten Workflows zeigen. Der zweite Typ umfasst ein Python-Paket sowie einige Codebeispiele zum Verwenden dieses Pakets. Das Python-Paket ist hinreichend allgemein, um für viele Anwendungsfälle verwendet zu werden.

Die Dateien in diesem Beispiel sind wie folgt organisiert.

| Dateiname | Typ | Beschreibung |
|-----------|------|-------------|
| `aml_config` | Ordner | Der Konfigurationsordner der Azure Machine Learning Workbench. Informationen zur Konfiguration der Durchführung der Experimente finden Sie in [dieser Dokumentation](./experiment-execution-configuration-reference.md). |
| `Code` | Ordner | Der Codeordner, in dem die Python-Skripts und das Python-Paket gespeichert werden |
| `Data` | Ordner | Der Datenordner, in dem Zwischendateien gespeichert werden |
| `notebooks` | Ordner | Der Ordner für Jupyter Notebooks |
| `Code/documentAnalysis/__init__.py` | Python-Datei | Die init-Datei für das Python-Paket |
| `Code/documentAnalysis/configs.py` | Python-Datei | Die Konfigurationsdatei, die vom Python-Paket für die Dokumentanalyse verwendet wird, einschließlich der vordefinierten Konstanten |
| `Code/documentAnalysis/preprocessText.py` | Python-Datei | Die Python-Datei für die Vorverarbeitung der Daten für Downstreamtasks |
| `Code/documentAnalysis/phraseLearning.py` | Python-Datei | Die Python-Datei zum Lernen der Ausdrücke in den Daten und zum Transformieren der unformatierten Daten |
| `Code/documentAnalysis/topicModeling.py` | Python-Datei | Die Python-Datei zum Trainieren eines LDA-Themenmodells (Latent Dirichlet Allocation) |
| `Code/step1.py` | Python-Datei | Schritt 1 der Dokumentsammlungsanalyse: Vorverarbeiten des Texts |
| `Code/step2.py` | Python-Datei | Schritt 2 der Dokumentsammlungsanalyse: Lernen von Ausdrücken |
| `Code/step3.py` | Python-Datei | Schritt 3 der Dokumentsammlungsanalyse: Trainieren und Auswerten des LDA-Themenmodells |
| `Code/runme.py` | Python-Datei | Beispiel für die Ausführung aller Schritte in einer Datei |
| `notebooks/1_Preprocess_Text.ipynb` | IPython Notebook | Vorverarbeiten von Text und Transformieren der unformatierten Daten |
| `notebooks/2_Phrase_Learning.ipynb` | IPython Notebook | Lernen von Ausdrücken aus den Textdaten (nach der Datentransformation) |
| `notebooks/3_Topic_Model_Training.ipynb` | IPython Notebook | Trainieren des LDA-Themenmodells |
| `notebooks/4_Topic_Model_Summarization.ipynb` | IPython Notebook | Zusammenfassen der Inhalte der Dokumentsammlung anhand eines trainierten LDA-Themenmodells |
| `notebooks/5_Topic_Model_Analysis.ipynb` | IPython Notebook | Analysieren der thematischen Inhalte einer Sammlung von Textdokumenten und Korrelieren der Themeninformationen mit anderen zugeordneten Metadaten, die der Dokumentsammlung zugeordnet sind |
| `notebooks/winprocess.py` | Python-Datei | Das Python-Skript für das Multiprocessing der verwendeten Notebooks |
| `README.md` | Markdowndatei | Die Info-Markdowndatei |

### <a name="data-ingestion-and-transformation"></a>Erfassen und Transformieren von Daten

Das kompilierte DataSet `CongressionalDataAll_Jun_2017.tsv` wird in Blob Storage gespeichert. Der Zugriff darauf ist sowohl aus den Notebooks als auch über die Python-Skripts möglich. Die Erfassung und Transformation der Daten erfolgt in zwei Schritten: Vorverarbeiten der Textdaten und Lernen der Ausdrücke.

#### <a name="preprocess-text-data"></a>Vorverarbeiten der Textdaten

Beim Vorverarbeitungsschritt werden Verarbeitungstechniken für natürliche Sprache angewendet, um die unformatierten Textdaten zu bereinigen und vorzubereiten. Er stellt die Vorstufe für die nicht überwachte Phase zum Lernen von Ausdrücken und Modellieren der enthaltenen Themen dar. Die ausführliche Beschreibung finden Sie im Notebook `1_Preprocess_Text.ipynb`. Es gibt auch ein Python-Skript (`step1.py`) zu diesem Notebook.

In diesem Schritt wird die TSV-Datendatei aus Azure Blob Storage heruntergeladen und als Pandas-Datenrahmen importiert. Jedes Zeilenelement im Datenrahmen ist eine einzelne zusammenhängende lange Textzeichenfolge oder ein „Dokument“. Jedes Dokument wird dann in Textabschnitte aufgeteilt. Dies sind häufig Sätze, Wortgruppen oder Ausdrücke. Die Aufteilung soll verhindern, dass beim Lernen von Ausdrücken satz- oder wortgruppenüberspannende Zeichenfolgen verwendet werden.

Es gibt mehrere Funktionen, die für den Vorverarbeitungsschritt im Notebook und im Python-Paket definiert wurden. Der Großteil des Auftrags kann durch Aufrufen dieser beiden Codezeilen durchgeführt werden.

```python
# Read raw data into a Pandas DataFrame
textDF = getData()

# Write data frame with preprocessed text out to TSV file
cleanedDataFrame = CleanAndSplitText(textDF, saveDF=True)
```

#### <a name="phrase-learning"></a>Lernen von Ausdrücken

Der Schritt zum Lernen von Ausdrücken implementiert ein einfaches Framework zum Erlernen von Schlüsselausdrücken in einer umfangreichen Sammlung von Dokumenten. Eine Beschreibung finden Sie im Dokument [Modeling Multiword Phrases with Constrained Phrases Tree for Improved Topic Modeling of Conversational Speech](http://people.csail.mit.edu/hazen/publications/Hazen-SLT-2012.pdf) (Modellieren von Mehr-Wort-Ausdrücken mit eingeschränkter Ausdrucksstruktur für verbesserte Themenmodellierung bei natürlicher Sprache), der ursprünglich im IEEE-Workshop 2012 zu Technologien für gesprochene Sprachen präsentiert wurde. Die ausführliche Implementierung des Schritts zum Ausdrucklernen wird im IPython Notebook `2_Phrase_Learning.ipynb` und im Python-Skript `step2.py` gezeigt.

Dieser Schritt verwendet den bereinigten Text als Eingabe und lernt die wichtigsten Ausdrücke in einer umfangreichen Sammlung von Dokumenten. Das Ausdrucklernen ist ein iterativer Prozess, der in drei Aufgaben unterteilt werden kann: Zählen der N-Gramme, Einordnen der möglichen Ausdrücke anhand der gewichteten punktweisen Transinformationen (Weighted Pointwise Mutual Information) ihrer enthaltenen Wörter und Schreiben des Ausdrucks in den Text. Diese drei Aufgaben werden in mehreren Iterationen sequenziell ausgeführt, bis die angegebenen Ausdrücke gelernt wurden.

Im Python-Paket zur Dokumentanalyse ist die Python-Klasse `PhraseLearner` in der Datei `phraseLearning.py` definiert. Im Folgenden finden Sie den Codeausschnitt zum Lernen der Ausdrücke.

```python
# Instantiate a PhraseLearner and run a configuration
phraseLearner = PhraseLearner(cleanedDataFrame, "CleanedText", numPhrase,
                        maxPhrasePerIter, maxPhraseLength, minInstanceCount)

# The chunks of text in a list
textData = list(phraseLearner.textFrame['LowercaseText'])

# Learn most salient phrases present in a large collection of documents
phraseLearner.RunConfiguration(textData,
            phraseLearner.learnedPhrases,
            addSpace=True,
            writeFile=True,
            num_workers=cpu_count()-1)
```

> [!NOTE]
> Der Schritt zum Lernen von Ausdrücken wird mit Multiprocessing implementiert. Mehr CPU-Kerne führen jedoch **NICHT** zu einer kürzeren Ausführungszeit. Bei unseren Tests wird die Leistung nicht durch mehr als acht Kerne verbessert, da das Multiprocessing einen Mehraufwand erzeugt. Es dauerte ungefähr 2,5 Stunden, 25.000 Ausdrücke auf einem Computer mit acht Kernen (3,6 GHz) zu lernen.
>

### <a name="topic-modeling"></a>Themenmodellierung

Das Trainieren eines latenten Themenmodells mit LDA stellt den dritten Schritt in diesem Szenario dar. Das Python-Paket [Gensim](https://radimrehurek.com/gensim/) wird in diesem Schritt dazu verwendet, ein [LDA-Themenmodell](https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation) zu erlernen. Das Notebook für diesen Schritt ist `3_Topic_Model_Training.ipynb`. Weitere Informationen zur Verwendung des Pakets zur Dokumentanalyse finden Sie in `step3.py`.

In diesem Schritt besteht die Hauptaufgabe darin, ein LDA-Themenmodell zu trainieren und Hyperparameter zu optimieren. Beim Trainieren eines LDA-Modells müssen mehrere Parameter optimiert werden, der wichtigste Parameter ist jedoch die Anzahl der Themen. Bei zu wenigen Themen ist eventuell kein ausreichender Einblick in die Dokumentsammlung möglich, während die Auswahl von zu vielen Themen zu einer Aufteilung eigentlich zusammenhängender Themen in kleinere und sehr ähnliche Themen führen kann (Over-Clustering). Dieses Szenario soll die Optimierung der Anzahl von Themen veranschaulichen. Die Azure Machine Learning Workbench ermöglicht das Ausführen von Experimenten mit verschiedenen Parameterkonfigurationen in unterschiedlichen Rechenkontexten.

Im Python-Paket zur Dokumentanalyse wurden einige Funktionen definiert, die Benutzern helfen sollen, die optimale Anzahl von Themen herauszufinden. Beim ersten Ansatz wird die Kohärenz des Themenmodells bewertet. Es werden vier Auswertungsmatrizen unterstützt: `u_mass`, `c_v`, `c_uci` und `c_npmi`. Einzelheiten dieser vier Metriken werden in [diesem Dokument](http://svn.aksw.org/papers/2015/WSDM_Topic_Evaluation/public.pdf) erläutert. Beim zweiten Ansatz wird die Perplexität für einen Korpus bewertet.

Bei der Auswertung der Perplexität wird eine Kurve in U-Form erwartet, um die optimale Anzahl von Themen zu ermitteln. Dabei ist die Position im Scheitelpunkt die beste Wahl. Es wird empfohlen, die Bewertung mehrmals mit verschiedenen Zufallsstartwerten durchzuführen und einen Mittelwert zu berechnen. Als Auswertung der Kohärenz wird eine n-Form erwartet. Dies bedeutet, dass die Kohärenz mit Erhöhung der Anzahl von Themen zunächst ansteigt und dann wieder sinkt. Im Folgenden finden Sie Beispieldarstellungen der Perplexität und der `c_v`-Kohärenz.

![Perplexität](./media/scenario-document-collection-analysis/Perplexity_Value.png)

![c_v-Kohärenz](./media/scenario-document-collection-analysis/c_v_Coherence.png)

In diesem Szenario nimmt die Perplexität nach 200 Themen signifikant zu, während sich der Kohärenzwert nach 200 Themen ebenfalls deutlich verringert. Basierend auf diesen Graphen und mit der Absicht, allgemeinere Themen zu erhalten und Over-Clustering zu vermeiden, sollte eine Auswahl von 200 Themen eine gute Option sein.

Sie können ein LDA-Themenmodell in einer Experimentausführung trainieren. Es ist aber auch möglich, in einer einzelnen Experimentausführung mehrere LDA-Modelle mit unterschiedlichen Konfigurationen der Themenanzahl zu trainieren und zu bewerten. Es wird empfohlen, jede Konfiguration mehrmals auszuführen, um dann Auswertungen für die durchschnittliche Kohärenz und/oder Perplexität zu erhalten. Einzelheiten zum Verwenden des Paktes zur Dokumentanalyse finden Sie in der Datei `step3.py`. Der folgende Codeausschnitt ist ein Beispiel.

```python
topicmodeler = TopicModeler(docs,
        stopWordFile=FUNCTION_WORDS_FILE,
        minWordCount=MIN_WORD_COUNT,
        minDocCount=MIN_DOC_COUNT,
        maxDocFreq=MAX_DOC_FREQ,
        workers=cpu_count()-1,
        numTopics=NUM_TOPICS,
        numIterations=NUM_ITERATIONS,
        passes=NUM_PASSES,
        chunksize=CHUNK_SIZE,
        random_state=RANDOM_STATE,
        test_ratio=test_ratio)

# Train an LDA topic model
lda = topicmodeler.TrainLDA(saveModel=saveModel)

# Evaluate coherence metrics
coherence = topicmodeler.EvaluateCoherence(lda, coherence_types)

# Evaluate perplexity on a held-out corpus
perplex = topicmodeler.EvaluatePerplexity(lda)
```

> [!NOTE]
> Die Ausführungszeit zum Trainieren des LDA-Themenmodells hängt von mehreren Faktoren ab. Dazu gehören z.B. die Textgröße, die Konfiguration der Hyperparameter sowie die Anzahl der Kerne auf dem Computer. Durch die Verwendung mehrerer CPU-Kerne kann das Modell schneller trainiert werden. Allerdings bedeuten bei derselben Einstellung für Hyperparameter mehr Prozessorkerne auch weniger Aktualisierungen während des Trainings. Es wird empfohlen, **mindestens 100 Aktualisierungen zum Trainieren eines zusammengeführten LDA-Modells** zu verwenden. Der Zusammenhang zwischen der Anzahl von Aktualisierungen und Hyperparametern wird in [diesem Beitrag](https://groups.google.com/forum/#!topic/gensim/ojySenxQHi4) und [diesem Beitrag](http://miningthedetails.com/blog/python/lda/GensimLDA/) erläutert. Bei unseren Tests dauerte es ungefähr 3 Stunden, ein LDA-Modell mit 200 Themen mit der Konfiguration `workers=15`, `passes=10`, `chunksize=1000` auf einem Computer mit 16 Kernen (2,0 GHz) zu trainieren.
>

### <a name="topic-summarization-and-analysis"></a>Zusammenfassen und Analysieren von Themen

Die Themenzusammenfassung und -analyse umfasst zwei Notebooks. Das Paket zur Dokumentanalyse enthält hingegen keine entsprechenden Funktionen.

In `4_Topic_Model_Summarization.ipynb` wird das Zusammenfassen der Inhalte des Dokuments anhand eines trainierten LDA-Themenmodells gezeigt. Die Zusammenfassung wird auf ein LDA-Themenmodell angewendet, das in Schritt 3 trainiert wurde. Es zeigt, wie die Wichtigkeit oder Qualität eines Themas durch eine Bewertung der Themenreinheit im Dokument gemessen wird. Diese Reinheitsbewertung setzt voraus, dass dominierende zugrunde liegende Themen in den Dokumenten semantisch bedeutsamer sind als Themen, die weniger häufig und über viele Dokumente verteilt sind. Dieses Konzept wurde im Artikel [Latent Topic Modeling for Audio Corpus Summarization](http://people.csail.mit.edu/hazen/publications/Hazen-Interspeech11.pdf) (Modellierung latenter Themen für Audiotextzusammenfassungen) eingeführt.

Das Notebook `5_Topic_Model_Analysis.ipynb` veranschaulicht das Analysieren der thematischen Inhalte einer Sammlung von Textdokumenten und das Korrelieren der Themeninformationen mit anderen zugeordneten Metadaten, die der Dokumentsammlung zugeordnet sind. In diesem Notebook werden einige Plots eingeführt, damit die Benutzer das gelernte Thema und die Dokumentsammlung besser verstehen.

## <a name="conclusion"></a>Zusammenfassung

Dieses Echtwelt-Szenario veranschaulicht die Verwendung bekannter Textanalyseverfahren (in diesem Fall Ausdrucklernen und LDA-Themenmodellierung) zum Erstellen eines stabilen Modells und wie die Azure Machine Learning Workbench dabei helfen kann, die Modellleistung zu verfolgen und Lernmodule nahtlos mit einer höheren Skalierung auszuführen. Ausführliche Informationen:

* Verwenden Sie Ausdrucklernen und Themenmodellierung, um eine Sammlung von Dokumenten zu verarbeiten und ein stabiles Modell zu erstellen. Wenn die Dokumentsammlung sehr umfangreich ist, können Sie mit der Azure Machine Learning Workbench problemlos zentral hoch- oder herunterskalieren. Darüber hinaus haben Benutzer die Möglichkeit, aus der Azure Machine Learning Workbench heraus Experimente in verschiedenen Rechenkontexten durchzuführen.

* Die Azure Machine Learning Workbench stellt Optionen für die Ausführung von Notebooks in einzelnen Schritten sowie unter Verwendung von Python-Skripts für ein vollständiges Experiment bereit.

* Für das Lernen sollten Hyperparameter mithilfe der Azure Machine Learning Workbench optimiert werden, um die ideale Anzahl von Themen zu bestimmen, die gelernt werden müssen. Die Azure Machine Learning Workbench kann dabei helfen, die Modellleistung zu verfolgen und unterschiedliche Lernmodule nahtlos mit höherer Skalierung auszuführen.

* Sie können die Azure Machine Learning Workbench auch verwenden, um den Ausführungsverlauf und die gelernten Modelle zu verwalten. Dadurch können Datenanalysten schnell die Modelle mit der besten Leistung ermitteln und die Skripts und Daten zu deren Generierung finden.

## <a name="references"></a>Referenzen

* **Timothy J. Hazen, Fred Richardson**, [_Modeling Multiword Phrases with Constrained Phrases Tree for Improved Topic Modeling of Conversational Speech_](http://people.csail.mit.edu/hazen/publications/Hazen-SLT-2012.pdf). Spoken Language Technology Workshop (SLT), 2012 IEEE. IEEE, 2012.

* **Timothy J. Hazen**, [_Latent Topic Modeling for Audio Corpus Summarization_](http://people.csail.mit.edu/hazen/publications/Hazen-Interspeech11.pdf). 12th Annual Conference of the International Speech Communication Association. 2011.

* **Michael Roder, Andreas Both, Alexander Hinneburg**, [_Exploring the Space of Topic Coherence Measures_](http://svn.aksw.org/papers/2015/WSDM_Topic_Evaluation/public.pdf). Proceedings of the eighth ACM international conference on Web search and data mining. ACM, 2015.
