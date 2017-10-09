---
title: Standpunktanalyse mithilfe von Deep Learning und Azure Machine Learning | Microsoft-Dokumentation
description: "Informationen zum Ausführen von Standpunktanalysen mithilfe von Deep Learning mit Azure ML Workbench."
services: machine-learning
documentationcenter: 
author: miprasad
manager: kristin.tolle
editor: miprasad
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2017
ms.author: miprasad
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 6ace1ec1130898d5cdc4e5c8b957e13aecc87174
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="sentiment-analysis-using-deep-learning-with-azure-machine-learning"></a>Standpunktanalyse mithilfe von Deep Learning und Azure Machine Learning

Die Standpunktanalyse ist eine übliche Aufgabe im Bereich der maschinellen Verarbeitung natürlicher Sprache. Bei einer Reihe von Texten geht es darum, die Stimmung des jeweiligen Texts zu bestimmen. Ziel dieser Lösung ist das Verwenden von CNTK als Back-End für Keras (eine Bibliothek auf Modellebene, die allgemeine Bausteine für die Entwicklung von Deep Learning-Modellen bereitstellt) und das Implementieren der Standpunktanalyse mithilfe von Filmkritiken.

Die Lösung befindet sich unter „https://github.com/Azure/MachineLearningSamples-SentimentAnalysis“.

## <a name="link-to-the-gallery-github-repository"></a>Link zum Katalog des GitHub-Repositorys

[https://github.com/Azure/MachineLearningSamples-SentimentAnalysis](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis)

## <a name="use-case-overview"></a>Übersicht über den Anwendungsfall

Die Datenexplosion und die zunehmende Verbreitung mobiler Geräte haben für Kunden viele Möglichkeiten eröffnet, ihre Gefühle und Einstellungen zu allem Möglichen jederzeit auszudrücken. Diese Meinung bzw. „Stimmung“ wird häufig über soziale Medien in Form von Kritiken, Chats, Angaben in der Art „Geteilt mit“ oder „Gefällt mir“, Tweets usw. generiert. Der Stimmung kann für Unternehmen von unschätzbarem Wert sein, die ihre Produkte und Dienstleistungen verbessern, fundiertere Entscheidungen treffen und ihre Marken besser bewerben möchten.

Um aus der Standpunktanalyse einen Nutzen zu ziehen, müssen Unternehmen in der Lage sein, riesige Mengen unstrukturierter Daten aus sozialen Medien zu durchforsten, um daraus umsetzbare Erkenntnisse zu gewinnen. In diesem Beispiel entwickeln wir Deep Learning-Modelle für die Standpunktanalyse von Filmkritiken mithilfe von Azure ML Workbench.

## <a name="prerequisites"></a>Voraussetzungen

* Ein [Azure-Konto](https://azure.microsoft.com/free/) (kostenlose Testversionen verfügbar).

* Eine gemäß dem [Schnellstart-Installationshandbuch](./quickstart-installation.md) installierte Instanz von [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) zur Installation des Programms und der Erstellung eines Arbeitsbereichs.

* Für die Operationalisierung wird empfohlen, dass Sie das Docker-Modul installieren und lokal ausführen. Falls nicht gewünscht, können Sie die Clusteroption nutzen. Das Ausführen eines Azure Container Service (ACS) kann allerdings kostspielig sein.

* Bei dieser Lösung wird davon ausgegangen, dass Sie die Azure Machine Learning Workbench unter Windows 10 mit einem lokal installierten Docker-Modul ausführen. Die Anweisung für macOS ist weitgehend identisch.

## <a name="create-a-new-workbench-project"></a>Erstellen eines neuen Workbench-Projekts

Erstellen Sie ein neues Projekt mit diesem Beispiel als Vorlage:
1.  Öffnen Sie Azure Machine Learning Workbench.
2.  Klicken Sie auf der Seite **Projekte** auf die Schaltfläche **+**, und wählen Sie **Neues Projekt** aus.
3.  Geben Sie im Bereich **Neues Projekt erstellen** die Informationen für das neue Projekt ein.
4.  Geben Sie im Suchfeld **Projektvorlagen suchen** den Suchbegriff „Sentiment Analysis with Deep Learning“ (Standpunktanalyse mit Deep Learning) ein, und wählen Sie die Vorlage aus.
5.  Klicken Sie auf **Erstellen**.

## <a name="data-description"></a>Datenbeschreibung

Das Dataset, der für dieses Beispiel verwendet wird, ist ein kleines manuell erstelltes Dataset, das sich im [GitHub-Repository](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/Data) im Ordner „Data“ befindet.

Die erste Spalte enthält Filmkritiken und die zweite Spalte enthält die Stimmung (0 für „negativ“ und „1“ für positiv). Das Dataset wird lediglich zu Demonstrationszwecken verwendet. Um zuverlässiges Stimmungswerte zu erhalten, benötigen Sie in der Regel ein größeres Dataset. Das [IMDB Movie reviews sentiment classification problem](https://keras.io/datasets/#datasets ) von Keras besteht beispielsweise aus einem Dataset mit 25.000 Filmkritiken aus der Filmdatenbank IMDB, die mit der Stimmung (positiv oder negativ) bezeichnet sind. Die Absicht dieser Anleitung ist das Veranschaulichen der Standpunktanalyse mithilfe von Deep Learning und Azure ML Workbench.

## <a name="scenario-structure"></a>Struktur des Szenarios

Die Ordnerstruktur ist wie folgt angeordnet:

1. Code: Enthält den gesamten Code in Zusammenhang mit der Standpunktanalyse mithilfe von Azure ML Workbench  
2. Data: Enthält das Dataset, das in der Lösung verwendet wird  
3. Labs: Enthält die praktischen Übungseinheiten  

Die Reihenfolge der praktische Übungseinheiten zum Ausführen der Lösung lautet wie folgt:

| Reihenfolge| Dateiname | Zugehörige Dateien |
|--|-----------|------|
| 1 | [`DataPreparation.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/Docs/SentimentAnalysisDataPreparation.md) | Data/sampleReviews.txt |
| 2 | [`ModelingAndEvaluation.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/Docs/SentimentAnalysisModelingDocker.md) | Code/SentimentExtraction.py |
| 3 | [`ModelingAndEvaluationDocker.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/Docs/SentimentAnalysisModelingKerasWithCNTKBackend.md) | Code/SentimentExtractionDocker.py |
| 4 | [`Operationalization.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/Docs/SentimentAnalysisOperationalization.md) | Code/Operationalization |

## <a name="conclusion"></a>Zusammenfassung

Diese Lösung dient als Einführung in das Verwenden von Deep Learning zum Durchführen einer Standpunktanalyse mithilfe der Azure Machine Learning Workbench. Die vorgestellte Lösung kann flexibel mit CNTK/Tensorflow und mit Keras als Back-End verwendet werden. Darüber hinaus erfolgt eine Operationalisierung auch mit HDF5-Modellen.

