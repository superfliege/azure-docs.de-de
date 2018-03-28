---
title: Standpunktanalyse mithilfe von Deep Learning und Azure Machine Learning | Microsoft-Dokumentation
description: Informationen zum Ausführen von Standpunktanalysen mithilfe von Deep Learning mit Azure ML Workbench.
services: machine-learning
documentationcenter: ''
author: miprasad
manager: kristin.tolle
editor: miprasad
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/20/2018
ms.author: miprasad
ms.openlocfilehash: ead8edcefa8705af82f4b223512a671c1245718a
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="sentiment-analysis-using-deep-learning-with-azure-machine-learning"></a>Standpunktanalyse mithilfe von Deep Learning und Azure Machine Learning

Die Standpunktanalyse ist eine übliche Aufgabe im Bereich der maschinellen Verarbeitung natürlicher Sprache. Bei einer Reihe von Texten geht es darum, die Stimmung des jeweiligen Texts zu bestimmen. Ziel dieser Lösung ist die Verwendung von Deep Learning zur Vorhersage von Standpunkten aus Filmkritiken.

Die Lösung befindet sich hier: https://github.com/Azure/MachineLearningSamples-SentimentAnalysis.

## <a name="link-to-the-gallery-github-repository"></a>Link zum Katalog des GitHub-Repositorys

Über diesen Link gelangen Sie zum öffentlichen GitHub-Repository:

[https://github.com/Azure/MachineLearningSamples-SentimentAnalysis](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis)

## <a name="use-case-overview"></a>Übersicht über den Anwendungsfall

Die Datenexplosion und die zunehmende Verbreitung mobiler Geräte haben für Kunden viele Möglichkeiten eröffnet, ihre Gefühle und Einstellungen zu allem Möglichen jederzeit auszudrücken. Diese Meinung bzw. „Stimmung“ wird häufig über soziale Medien in Form von Kritiken, Chats, geteilten und mit „Gefällt mir“ markierten Beiträgen, Tweets usw. erzeugt. Der Stimmung kann für Unternehmen von unschätzbarem Wert sein, die ihre Produkte und Dienstleistungen verbessern, fundiertere Entscheidungen treffen und ihre Marken besser bewerben möchten.

Um aus der Standpunktanalyse einen Nutzen zu ziehen, müssen Unternehmen in der Lage sein, riesige Mengen unstrukturierter Daten aus sozialen Medien zu durchforsten, um daraus umsetzbare Erkenntnisse zu gewinnen. In diesem Beispiel entwickeln wir Deep Learning-Modelle für die Standpunktanalyse von Filmkritiken mithilfe von Azure ML Workbench.

## <a name="prerequisites"></a>Voraussetzungen

* Ein [Azure-Konto](https://azure.microsoft.com/free/) (kostenlose Testversionen verfügbar)

* Eine installierte Kopie der [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) nach dem [Schnellstart-Installationshandbuch](./quickstart-installation.md) zur Installation des Programms und der Erstellung eines Arbeitsbereichs.

* Für die Operationalisierung wird empfohlen, dass Sie die Docker-Engine installieren und lokal ausführen. Falls nicht gewünscht, können Sie die Clusteroption nutzen. Das Ausführen eines Azure Container Service (ACS) kann allerdings kostspielig sein.

* Bei dieser Lösung wird davon ausgegangen, dass Sie die Azure Machine Learning Workbench unter Windows 10 mit einer lokal installierten Docker-Engine ausführen. Die Anweisung für macOS ist weitgehend identisch.

## <a name="data-description"></a>Datenbeschreibung

Das Dataset, das für dieses Beispiel verwendet wird, ist ein kleines manuell erstelltes Dataset, das sich im [Ordner „Data“](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/tree/master/data) befindet.

Die erste Spalte enthält Filmkritiken und die zweite Spalte enthält die Stimmung (0 für „negativ“ und „1“ für positiv). Das Dataset wird lediglich zu Demonstrationszwecken verwendet. Um zuverlässiges Stimmungswerte zu erhalten, benötigen Sie in der Regel ein größeres Dataset. Das [IMDB Movie reviews sentiment classification problem](https://keras.io/datasets/#datasets ) von Keras besteht beispielsweise aus einem Dataset mit 25.000 Filmkritiken aus der Filmdatenbank IMDB, die mit der Stimmung (positiv oder negativ) bezeichnet sind. Die Absicht dieser Anleitung ist das Veranschaulichen der Standpunktanalyse mithilfe von Deep Learning und Azure ML Workbench.

## <a name="scenario-structure"></a>Struktur des Szenarios

Die Ordnerstruktur ist wie folgt angeordnet:

1. Der gesamte Code bezüglich der Stimmungsanalyse mit Azure ML Workbench befindet sich im Stammverzeichnis.
2. Data: Enthält das Dataset, das in der Lösung verwendet wird
3. Dokumente: Enthält praktische Übungseinheiten

Die Reihenfolge der praktische Übungseinheiten zum Ausführen der Lösung lautet wie folgt:

| Reihenfolge| Dateiname | Zugehörige Dateien |
|--|-----------|------|
| 1 | [`SentimentAnalysisDataPreparation.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/docs/SentimentAnalysisDataPreparation.md) | 'data/sampleReviews.txt' |
| 2 | [`SentimentAnalysisModelingKeras.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/docs/SentimentAnalysisModelingKeras.md) | 'SentimentExtraction.py' |
| 3 | [`SentimentAnalysisOperationalization.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/docs/SentimentAnalysisOperationalization.md) | 'Operaionalization' |

## <a name="conclusion"></a>Zusammenfassung

Diese Lösung dient als Einführung in das Verwenden von Deep Learning zum Durchführen einer Standpunktanalyse mithilfe der Azure Machine Learning Workbench. Eine Operationalisierung erfolgt auch mit HDF5-Modellen.
