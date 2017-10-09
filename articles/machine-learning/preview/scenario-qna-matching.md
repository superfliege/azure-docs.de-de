---
title: Zuordnung von Fragen und Antworten mit Azure Machine Learning Workbench | Microsoft-Dokumentation
description: "Wie verschiedene effektive Machine Learning-Methoden verwendet werden können, um offene Abfragen bereits vorhandenen, häufig gestellten Frage/Antwort-Paaren zuzuordnen."
services: machine-learning
documentationcenter: 
author: mezmicrosoft
editor: mezmicrosoft
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: mez
ms.manager: tihazen
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 8edc21fb8f42ee5897c4e938045cc1f42aedb3ce
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

#  <a name="q--a-matching-using-azure-machine-learning-workbench"></a>Zuordnung von Fragen und Antworten mit Azure Machine Learning Workbench
Die Beantwortung von offenen Fragen ist schwierig und erfordert oft manuellen Aufwand von Experten (SMEs). Um die Arbeitsbelastung der internen SMEs zu reduzieren, erstellen Unternehmen oft Listen mit häufig gestellten Fragen (FAQs), um die Benutzer zu unterstützen. Dieses Beispiel zeigt, wie verschiedene effektive Machine Learning-Methoden verwendet werden können, um offene Abfragen bereits vorhandenen, häufig gestellten Frage/Antwort-Paaren zuzuordnen. Dieses Beispiel zeigt einen einfachen Entwicklungsprozess für die Erstellung einer Lösung, die Azure Machine Learning Workbench verwendet. 

## <a name="link-to-the-gallery-github-repository"></a>Link zum Katalog des GitHub-Repositorys
[https://github.com/Azure/MachineLearningSamples-QnAMatching](https://github.com/Azure/MachineLearningSamples-QnAMatching)

## <a name="overview"></a>Übersicht

In diesem Beispiel wird das Problem der Zuordnung von Benutzerfragen zu bereits vorhandenen Q&A-Paaren erläutert, die in der Regel in einer Liste mit häufig gestellten Fragen (d.h. FAQ) oder in den Q&A-Paaren auf Websites wie [Stack Overflow](https://stackoverflow.com/) bereitgestellt werden. Es gibt viele Ansätze zur Zuordnung einer Frage zur richtigen Antwort, z.B. die Antwort zu finden, die der Frage am ähnlichsten ist. Allerdings werden in diesem Beispiel offene Fragen zu zuvor gestellten Fragen zugeordnet, vorausgesetzt, dass jede Antwort in den häufig gestellten Fragen mehrere semantisch gleichwertige Fragen beantworten kann.

Die wichtigsten erforderlichen Schritte für diese Lösung sind die folgenden:

1. Bereinigen und Verarbeiten Sie Textdaten.
2. Lernen Sie informative Ausdrücke kennen, die aus Mehrwortsequenzen bestehen, die weitere Informationen bereitstellen, wenn sie in der Sequenz dargestellt und nicht unabhängig behandelt werden.
3. Extrahieren Sie Funktionen aus Textdaten.
4. Trainieren Sie Textklassifizierungsmodelle, und bewerten Sie die Modellleistung.


## <a name="prerequisites"></a>Voraussetzungen

Folgende Voraussetzungen müssen zum Ausführen dieses Beispiels erfüllt sein:

1. Ein [Azure-Konto](https://azure.microsoft.com/free/) (kostenlose Testversionen verfügbar).
2. Eine installierte Kopie der [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) nach dem [Schnellstart-Installationshandbuch](./quickstart-installation.md) zur Installation des Programms und der Erstellung eines Arbeitsbereichs.
3. Dieses Beispiel kann in beliebigen Computekontexten ausgeführt werden. Es wird jedoch empfohlen, für die Ausführung einen Computer mit mehreren Kernen und mindestens 16 GB Arbeitsspeicher und 5 GB Speicherplatz auf dem Datenträger zu verwenden.

## <a name="create-a-new-workbench-project"></a>Erstellen eines neuen Workbench-Projekts

Erstellen Sie ein neues Projekt mit diesem Beispiel als Vorlage:
1.  Öffnen Sie Azure Machine Learning Workbench.
2.  Klicken Sie auf der Seite **Projekte** auf **+**, wählen Sie **Neues Projekt** aus, und signieren Sie es.
3.  Geben Sie die Informationen für das neue Projekt im Bereich **Neues Projekt erstellen** ein
4.  Geben Sie im Suchfeld **Projektvorlagen suchen** „Zuordnung von Fragen und Antworten“ ein, und wählen Sie die Vorlage aus
5.  Klicken Sie auf **Erstellen**

## <a name="data-description"></a>Datenbeschreibung

Das in diesem Beispiel verwendete Dataset ist eine in [archive.org](https://archive.org/details/stackexchange) verwendete Stack Exchange-Datensicherung. Bei diesen Daten handelt es sich um eine anonymisierte Sicherung aller von Benutzern bereitgestellten Inhalten im [Stack Exchange-Netzwerk](https://stackexchange.com/). Jede Website im Netzwerk ist als ein separates Archiv formatiert, das aus mit 7-zip gezippten XML-Dateien mit bzip2-Komprimierung besteht. Jedes Websitearchive enthält Beiträge, Benutzer, Stimmen, Kommentare, PostHistory (Beitragsverlauf) und PostLinks (Beitragslinks). 

### <a name="data-source"></a>Datenquelle

Dieses Beispiel verwendet die [Posts-Daten (10 GB)](https://archive.org/download/stackexchange/stackoverflow.com-Posts.7z) und [PostLinks-Daten (515 MB)](https://archive.org/download/stackexchange/stackoverflow.com-PostLinks.7z) aus der Stack Overflow-Website. Vollständige Schemainformationen finden Sie unter ["Readme.txt"](https://ia800500.us.archive.org/22/items/stackexchange/readme.txt). 

Das `PostTypeId`-Feld in den Posts-Daten gibt an, ob es sich bei einem Post um `Question` oder `Answer` handelt. Das `PostLinkTypeId`-Feld in den PostLinks-Daten gibt an, ob zwei Beiträge dupliziert oder verknüpft sind. Fragebeiträge enthalten in der Regel einige Tags, die Schlüsselwörter sind, die eine Frage mit anderen ähnlichen/duplizierten Fragen kategorisieren. Es gibt einige Tags mit hoher Frequenz, wie `javascript`, `java`, `c#`, `php`, usw. Sie bestehen aus einer größeren Anzahl von Fragebeiträgen. In diesem Beispiel wird eine Teilmenge von Q&A-Paaren mit dem `javascript`-Tag extrahiert.

Zusätzlich dazu kann ein Fragebeitrag in Zusammenhang mit mehreren Antwortbeiträgen oder doppelten Fragebeiträgen stehen. Um eine Liste der FAQs aus diesen beiden Datasets zu erstellen, werden einige Kriterien der Datensammlung berücksichtigt. Die drei Sätze der kompilierten Daten werden mithilfe eines SQL-Skripts ausgewählt, das in diesem Beispiel nicht enthalten ist. Die resultierende Datenbeschreibung lautet wie folgt:

- `Original Questions (Q)`: Fragebeiträge werden auf der Stack Overflow-Website gestellt und beantwortet.
- `Duplications (D)`: Fragebeiträge duplizieren andere bereits vorhandene Fragen (`PostLinkTypeId = 3`), die die ursprünglichen Fragen sind. Doppelte Beiträge gelten als semantisch gleichwertig mit den ursprünglichen Fragen, wenn die Antwort auf die ursprüngliche Frage auch die neue, doppelte Frage beantwortet.
- `Answers (A)`: Jede ursprüngliche Frage und ihre Duplikationen können mit mehr als einem Antwortbeitrag verknüpft sein. Dieses Beispiel wählt nur die Antwortbeiträge aus, die entweder vom ursprünglichen Autor akzeptiert werden (durch `AcceptedAnswerId` gefiltert) oder die höchste Bewertung (mit dem höchsten `Score`) in den ursprünglichen Fragen aufweisen. 

Die Kombination aus diesen drei Datasets erstellt Q&A-Paare, in denen eine Antwort (A) einer ursprünglichen Frage (Q) und mehreren doppelten Fragen (D) zugeordnet ist, wie das folgende Diagramm veranschaulicht:

<table><tr><td><img id ="data_diagram" src="media/scenario-qna-matching/data_diagram.png"></td></tr></table>

### <a name="data-structure"></a>Datenstruktur

Das Datenschema und direkte Downloadlinks der drei Datasets können in der folgenden Tabelle gefunden werden:

| Datensatz | Feld | Typ | Beschreibung
| ----------|------------|------------|--------
| [questions](https://bostondata.blob.core.windows.net/stackoverflow/orig-q.tsv.gz) | ID | String | Die eindeutige Frage-ID (Primärschlüssel)
|  | AnswerId | String | Die eindeutige Antwort-ID je Frage
|  | Text0 | String | Die unformatierten Textdaten, einschließlich Titel und Text der Frage
|  | CreationDate | Zeitstempel | Der Zeitstempel, zu dem die Frage gestellt wurde
| [dupes](https://bostondata.blob.core.windows.net/stackoverflow/dup-q.tsv.gz) | ID | String | Die eindeutige Duplikations-ID (Primärschlüssel)
|  | AnswerId | String | Die Antwort-ID, die der Duplizierung zugeordnet ist
|  | Text0 | String | Die unformatierten Textdaten, einschließlich Titel und Text der Duplikation
|  | CreationDate | Zeitstempel | Der Zeitstempel , zu dem das Duplikat gepostet wurde
| [answers](https://bostondata.blob.core.windows.net/stackoverflow/ans.tsv.gz)  | ID | String | Die eindeutige Antwort-ID (Primärschlüssel)
|  | Text0 | String | Die unformatierten Textdaten der Antwort


## <a name="scenario-structure"></a>Szenariostruktur

Das übereinstimmende Q&A-Beispiel wird durch drei Typen von Dateien angezeigt. Der erste Typ ist eine Reihe von Jupyter Notebooks, die ausführliche Beschreibungen des gesamten Workflows zeigen. Der zweite Typ ist ein Satz von Python-Dateien, die benutzerdefinierte Python-Module für das Lernen von Ausdrücken und die Extraktion von Features enthalten. Diese Python-Module sind allgemein genug, um nicht nur für dieses Beispiel, sondern auch für andere Anwendungsfälle, verwendet zu werden. Der dritte Typ ist ein Satz von Python-Dateien zum Optimieren der Hyperparameter und Nachverfolgen der Modellleistung mithilfe der Azure Machine Learning Workbench.

Die Dateien in diesem Beispiel sind wie folgt organisiert.

| Dateiname | Typ | Beschreibung
| ----------|------------|--------
| `Image` | Ordner | Der Ordner zum Speichern von Bildern für die Infodatei
| `notebooks` | Ordner | Der Ordner für Jupyter Notebooks
| `modules` | Ordner | Der Ordner für Python-Module
| `scripts` | Ordner | Der Ordner für der Python-Dateien
| `notebooks/Part_1_Data_Preparation.ipynb` | Jupyter Notebook | Greifen Sie auf die Beispieldaten zu, bereiten Sie den Text im Vorab vor, und bereiten Sie die Trainings- und Testdatasets vor
| `notebooks/Part_2_Phrase_Learning.ipynb` | Jupyter Notebook | Lernen Sie informative Ausdrücke, und versehen Sie Text mit einem Token, um Bag-of-Words-Darstellungen (BOWs) zu erhalten
| `notebooks/Part_3_Model_Training_and_Evaluation.ipynb` | Jupyter Notebook | Extrahieren Sie Features, trainieren Sie Textklassifizierungsmodelle, und bewerten Sie die Modellleistung
| `modules/__init__.py` | Python-Datei | Die init-Datei für das Python-Paket
| `modules/phrase_learning.py` | Python-Datei | Das Python-Modul, mit dem die Rohdaten transformiert und informative Ausdrücke gelernt werden
| `modules/feature_extractor.py` | Python-Datei | Die Python-Module extrahieren Features für das Modelltraining
| `scripts/naive_bayes.py` | Python-Datei | Die Python-Datei, mit der Hyperparameter im Naive Bayes-Modell optimiert werden
| `scripts/random_forest.py` | Python-Datei | Die Python-Datei, mit dem Hyperparameter im Random Forest-Modell optimiert werden
| `README.md` | Markdowndatei | Die Info-Markdowndatei

> [!NOTE]
> Die Reihe von Notebooks wird unter 3.5 Python erstellt.
> 

### <a name="data-ingestion-and-transformation"></a>Erfassen und Transformieren von Daten

Die drei kompilierten Datasets werden in einem Blob-Speicher gespeichert und in `Part_1_Data_Preparation.ipynb`-Notebook abgerufen.  

Vor dem Trainieren der Textklassifizierungsmodelle wird der Text in den Fragen bereinigt und vorverarbeitet, um Codeausschnitte extrahieren zu können. Nicht überwachtes Lernen von Ausdrücken wird als Schulungsmaterial verwendet, um informative Mehrwortsequenzen zu lernen. Diese Ausdrücke werden als einzelne, zusammengesetzte Worteinheiten in Downstream-Features des Bag-of-Words (BOWs) dargestellt, die von Textklassifizierungsmodellen verwendet werden.

Eine ausführliche Schritt-für-Schritt-Beschreibung der Textvorverarbeitung und des Lernens von Ausdrücken finden Sie jeweils in den Notebooks `Part_1_Data_Preparation.ipynb` und `Part_2_Phrase_Learning.ipynb`.

### <a name="modeling"></a>Modellierung

Dieses Beispiel wurde entworfen, um neue Fragen für bereits vorhandene Q&A-Paare zu bewerten. Dazu werden Textklassifizierungsmodelle trainiert, in denen jedes bereits vorhandene Q&A-Paar eine eindeutige Klasse darstellt, und eine Teilmenge der doppelten Fragen für jedes Q&A-Paar als Schulungsmaterialien verfügbar ist. Im Beispiel werden die ursprünglichen Fragen und 75 % der doppelten Fragen für das Training beibehalten, und die zuletzt geposteten 25 % der doppelten Fragen werden als Auswertungsdaten verwendet.

Das Klassifizierungsmodell verwendet eine Ensemble-Methode, um drei Basisklassifizierungen zu aggregieren, darunter **Naive Bayes**, **Support Vector Machine** und **Random Forest**. In jeder Basisklassifizierung wird `AnswerId` als Klassenbeschriftung und die BOWs-Darstellungen als Features verwendet.

Der Trainingsprozess des Modells wird in `Part_3_Model_Training_and_Evaluation.ipynb` veranschaulicht.

### <a name="evaluation"></a>Auswertung

Es werden zwei verschiedene Auswertungsmetriken zur Leistungsbewertung verwendet. 
1. `Average Rank (AR)`: Gibt die mittlere Position an, in der die richtige Antwort in der Liste der abgerufenen Q&A-Paare (aus dem vollständigen Satz aus 103 Antwortklassen) gefunden wird. 
2. `Top 3 Percentage`: Gibt den Prozentsatz der Testfragen an, deren richtige Antwort in den ersten drei Optionen der zurückgegebenen Liste abgerufen werden kann. 

Die Auswertung wird in `Part_3_Model_Training_and_Evaluation.ipynb`veranschaulicht.


## <a name="conclusion"></a>Zusammenfassung

In diesem Beispiel wird hervorgehoben, wie bekannte Methoden der Textanalyse, wie das Lernen von Ausdrücken und die Textklassifizierung, zur Erstellung eines robusten Modells verwendet werden können. Es wird veranschaulicht, wie Azure Machine Learning Workbench helfen kann, interaktive Lösungen zu entwickeln und die Modellleistung nachzuverfolgen. 

Einige der wichtigsten Highlights dieses Beispiels sind:

- Das Problem der Zuordnung von Fragen und Antworten kann durch das Lernen von Ausdrücken und die Textklassifizierungsmodelle effektiv behoben werden.
- Es zeigt die interaktive Modellentwicklung mit Azure Machine Learning Workbench und Jupyter Notebook.
- Azure Machine Learning Workbench verwaltet den Ausführungsverlauf und gelernte Modelle durch die Protokollierung der Auswertungsmetriken zu Vergleichszwecken. Diese Features ermöglicht eine schnelle Optimierung der Hyperparameter und hilft bei der Identifizierung der leistungsstärksten Modelle.


## <a name="references"></a>Referenzen

Timothy J. Hazen, Fred Richardson, [_Modeling Multiword Phrases with Constrained Phrases Tree for Improved Topic Modeling of Conversational Speech (Modellieren von Mehr-Wort-Ausdrücken mit eingeschränkter Ausdrucksstruktur für verbesserte Themenmodellierung bei natürlicher Sprache)_](http://people.csail.mit.edu/hazen/publications/Hazen-SLT-2012.pdf). Spoken Language Technology Workshop (SLT), 2012 IEEE. IEEE, 2012.

Timothy J. Hazen, [_MCE Training Techniques for Topic Identification of Spoken Audio Documents (MCE-Trainingsmethoden zu Themenidentifikation gesprochener Audiodokumente)_](http://ieeexplore.ieee.org/abstract/document/5742980/) in IEEE Transactions on Audio, Speech, and Language Processing, vol. 19, no. 8, ff. 2451-2460, Nov. 2011.

