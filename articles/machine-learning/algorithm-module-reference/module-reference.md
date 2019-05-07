---
title: 'Algorithmen und Module: Referenz'
titleSuffix: Azure Machine Learning service
description: Erfahren Sie mehr über die Module, die in der grafischen Benutzeroberfläche von Azure Machine Learning verfügbar sind.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: 6602eb4bacdc3b6382c1b6873a465cdfc0632693
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027784"
---
# <a name="algorithm--module-reference-overview"></a>Algorithmen und Module: Referenzübersicht

Dieser Referenzinhalt bietet den technischen Hintergrund zu den einzelnen Machine Learning-Algorithmen und -Modulen, die in der grafischen Benutzeroberfläche (Vorschau) von Azure Machine Learning Service verfügbar sind. 

Jedes Modul stellt eine Codegruppe dar, die unabhängig ausgeführt werden und eine Machine Learning-Aufgabe ausführen kann, wenn die erforderlichen Eingaben erfolgen. Die Module enthalten möglicherweise bestimmte Algorithmen oder führen Aufgaben aus, die beim Machine Learning wichtig sind, z.B. Ersatz eines fehlenden Werts oder statistische Analyse. 

> [!TIP]
> In allen Experimenten in der grafischen Benutzeroberfläche können Sie Informationen zu einem bestimmten Modul abrufen. Wählen Sie das Modul und dann den **Weitere Hilfe**-Link im Bereich **Direkthilfe** aus.

Module sind nach Funktionalität organisiert:

**Datenformatkonvertierungen**

  + [Modul zum Konvertieren in das CSV-Format](convert-to-csv.md)

**Module für Dateneingabe und -ausgabe** verschieben Daten aus Cloudquellen in Ihr Experiment. Sie können Ihre Ergebnisse oder Zwischendaten während der Ausführung eines Experiments in Azure Storage, eine SQL-Datenbank-Instanz oder Hive schreiben oder Cloudspeicher zum Datenaustausch zwischen Experimenten verwenden.  

  + [Daten importieren](import-data.md)

  + [Daten exportieren](export-data.md)

  + [Manuelles Eingeben von Daten](enter-data-manually.md)


**Datentransformationsmodule** unterstützen Vorgänge für Daten, die für Machine Learning typisch sind, z.B. Normalisieren oder Diskretisieren von Daten, Featureauswahl und Verringerung der Dimensionalität.

  + [Auswählen von Spalten im Dataset](select-columns-in-dataset.md)

  + [Bearbeiten von Metadaten](edit-metadata.md)

  + [Bereinigen fehlender Daten](clean-missing-data.md)

  + [Hinzufügen von Spalten](add-columns.md)

  + [Hinzufügen von Zeilen](add-rows.md)

  + [Entfernen doppelter Zeilen](remove-duplicate-rows.md)

  + [Aufteilen von Daten](split-data.md)

  + [Normalisieren von Daten](normalize-data.md)

  + [Partition und Beispiel](partition-and-sample.md)


**Machine Learning-Algorithmen** wie Clustern, Support Vector Machine oder neuronale Netzwerke stehen in einzelnen Modulen zur Verfügung, mit denen Sie die Machine Learning-Aufgabe mit entsprechenden Parametern anpassen können.  
  + [Bewertungsmodell](score-model.md)

  + [Zuweisen von Daten zu Clustern ](assign-data-to-clusters.md)

  + [Trainieren eines Modells](train-model.md)

  + [Trainieren des Clusteringmodells](train-clustering-model.md)

  + [Auswertungsmodell](evaluate-model.md)

  + [Anwenden der Transformation](apply-transformation.md)

  + [Lineare Regression](linear-regression.md)

  + [Regression mit neuronalen Netzwerken](neural-network-regression.md)

  + [Entscheidungswaldregression](decision-forest-regression.md)

  + [Regression bei verstärktem Entscheidungsbaum](boosted-decision-tree-regression.md)

  + [Verstärkter Entscheidungsbaum mit zwei Klassen](two-class-boosted-decision-tree.md)

  + [Logistische Regression mit zwei Klassen](two-class-logistic-regression.md)

  + [Logistische Regression mit mehreren Klassen](multiclass-logistic-regression.md)

  + [Mehrklassiges neuronales Netzwerk](multiclass-neural-network.md)

  + [Entscheidungswald mit mehreren Klassen](multiclass-decision-forest.md)

  + [Gemitteltes Perzeptron mit zwei Klassen](two-class-averaged-perceptron.md)

  + [Entscheidungswald mit zwei Klassen](two-class-decision-forest.md)

  + [Zweiklassiges neuronales Netzwerk](two-class-neural-network.md)

  + [Zweiklassige Support Vector Machine](two-class-support-vector-machine.md)
  
  + [K-Means-Clustering](k-means-clustering.md)


Mit dem **Python-Modul** kann ganz einfach eine benutzerdefinierte Funktion ausgeführt werden. Sie schreiben den Code und betten ihn in ein Modul ein, um Python in einen Experimentdienst zu integrieren.
  + [Ausführen von Python-Skripts](execute-python-script.md)

  + [Erstellen eines Python-Modells](create-python-model.md)


