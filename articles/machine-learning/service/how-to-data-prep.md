---
title: Vorbereiten von Daten mit dem Machine Learning Data Prep SDK für Python – Azure
description: Erfahren Sie, wie Sie mit dem Azure Machine Learning Data Prep SDK für Python Daten in verschiedenen Formaten laden, zur besseren Nutzbarkeit umwandeln und diese Daten in einen Speicherort schreiben, damit diese durch Ihre Modelle aufgerufen werden können.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 11/27/2018
ms.openlocfilehash: 91d0f3565db484504a67a3b6ae0989b9291cd24f
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52446424"
---
# <a name="prepare-data-for-modeling-with-azure-machine-learning"></a>Vorbereiten von Daten zum Modellieren mit Azure Machine Learning

In diesem Artikel erfahren Sie mehr über die Anwendungsfälle und die einzigartigen Funktionen des Azure Machine Learning Data Prep SDK. Die Vorbereitung der Daten ist der wichtige Bestandteil eines Machine Learning-Workflows. Daten aus der realen Welt sind oft fehlerhaft, inkonsistent oder können ohne signifikante Bereinigung und Transformation nicht als Trainingsdaten verwendet werden. Die Korrektur von Fehlern und Anomalien in Rohdaten und die Entwicklung neuer Funktionen, die für das Problem, das Sie zu lösen versuchen, relevant sind, erhöhen die Modellgenauigkeit. Das SDK wurde für Benutzer anderer gängiger Datenvorbereitungsbibliotheken entwickelt und bietet gleichzeitig Vorteile für Schlüsselszenarien und die Interoperabilität mit diesen anderen Bibliotheken.

Sie können Ihre Daten in Python mit dem [Azure Machine Learning Data Prep SDK](https://aka.ms/data-prep-sdk) vorbereiten.

## <a name="azure-machine-learning-data-prep-sdk"></a>Azure Machine Learning – Data Prep SDK

Das [Azure Machine Learning Data Prep SDK](https://aka.ms/data-prep-sdk) ist eine Python-Bibliothek bietet:

* Intelligente zeitsparende Transformationen wie Fuzzygruppierung, abgeleitete Spalte nach Beispiel, automatische Aufteilung, Smart Read File und Schemaverarbeitung mit Flatterrand rechts.
* Eine einzige API, die mit kleinen Daten lokal oder großen Daten in der Cloud arbeitet, mit **wenig bis gar keinen Codeänderungen**.
* Die Fähigkeit, auf einem einzigen Computer effektiver zu skalieren, indem ein Streamingansatz für die Verarbeitung der Daten verwendet wird, anstatt in den Speicher zu laden.

### <a name="install-the-sdk"></a>Installieren des SDKs

Installieren Sie das SDK in Ihrer Python-Umgebung mithilfe des folgenden Befehls.

```shell
pip install azureml-dataprep
```

Verwenden Sie den folgenden Code, um das Paket zu importieren.

```python
import azureml.dataprep as dprep
```

### <a name="examples-and-reference"></a>Beispiele und Referenz

Weitere Informationen zu der Module und Funktionen in diesem SDK finden Sie unter der [Data Prep SDK-Referenzdokumentation](https://aka.ms/data-prep-sdk).

Die folgenden Beispiele zeigen einige der einzigartigen Funktionen des SDK, wie zum Beispiel:

* Automatische Dateityperkennung
* Intelligente Transformationen
* Zusammenfassungsstatistik
* Umgebungsübergreifende Funktionen


#### <a name="automatic-file-type-detection"></a>Automatische Dateityperkennung

Verwenden Sie die Funktion `smart_read_file()`, um Ihre Daten zu laden, ohne den Dateityp angeben zu müssen. Diese Funktion erkennt und analysiert den Dateityp automatisch.

```python
dataflow = dprep.smart_read_file(path="<your-file-path>")
```

#### <a name="intelligent-transforms"></a>Intelligente Transformationen

Verwenden Sie das SDK, um Spalten nach Beispiel und Rückschluss abzuleiten, um die Funktionsentwicklung zu automatisieren. Angenommen, Sie haben ein Feld in Ihrem Dataflowobjekt namens `datetime` mit einem Wert von `2018-09-15 14:30:00`.

Um das Feld `datetime` automatisch zu teilen, rufen Sie die folgende Funktion auf.

```python
new_dataflow = dataflow.split_column_by_example(source_column="datetime")
```

Wenn Sie den Beispielparameter nicht definieren, teilt die Funktion das Feld `datetime` automatisch in zwei neue Felder `datetime_1` und `datetime_2` auf. Die resultierenden Werte sind `2018-09-15` bzw. `14:30:00`. Es ist auch möglich, ein Beispielmuster anzugeben, und das SDK wird Ihre beabsichtigte Transformation vorhersagen und ausführen. Mit dem gleichen `datetime`-Objekt erstellt der folgende Code eine neue Spalte `datetime_weekday` für den Wochentag basierend auf dem angegebenen Beispiel.

```python
new_dataflow = dataflow.derive_column_by_example(
        source_columns="datetime", 
        new_column_name="datetime_weekday", 
        example_data=[("2009-01-04 10:12:00", "Sunday"), ("2013-08-22 17:00:00", "Thursday")]
    )
```

#### <a name="summary-statistics"></a>Zusammenfassungsstatistik

Sie können schnelle Zusammenfassungsstatistiken für einen Dataflow mit einer Codezeile erstellen. Diese Methode bietet eine bequeme Möglichkeit, Ihre Daten und deren Verteilung zu verstehen.

```python
dataflow.get_profile()
```

Der Aufruf dieser Funktion auf einem Dataflowobjekt führt zu einer Ausgabe wie in der folgenden Tabelle.

![Ausgabe der Zusammenfassungsstatistik](./media/concept-data-preparation/output-example.png)

## <a name="multiple-environment-compatibilities"></a>Kompatibel mit mehreren Umgebungen

Das SDK ermöglicht es auch, Dataflowobjekte zu serialisieren und in einer *beliebigen* Python-Umgebung zu öffnen. Die Umgebung, in der es geöffnet wird, kann sich von der Umgebung unterscheiden, in der es gespeichert ist. Diese Funktionalität ermöglicht einen einfachen Transfer zwischen Python-Umgebungen und eine schnelle Integration in Azure Machine Learning-Modelle.

Verwenden Sie den folgenden Code, um Ihre Dataflowobjekte zu speichern.

```python
package = dprep.Package([dataflow_1, dataflow_2])
package.save("<your-local-path>")
```

Verwenden Sie den folgenden Code, um Ihr Paket in jeder Umgebung wieder zu öffnen und eine Liste von Dataflowobjekten abzurufen.

```python
package = dprep.Package.open("<your-local-path>")
dataflow_list = package.dataflows
```

## <a name="data-preparation-pipeline"></a>Datenaufbereitungspipeline

Um detaillierte Beispiele und Code für jeden Vorbereitungsschritt anzuzeigen, gehen Sie wie folgt vor:

1. [Laden von Daten](how-to-load-data.md), ggf. in verschiedenen Formaten
2. [Transformieren](how-to-transform-data.md) in eine besser nutzbare Struktur
3. [Schreiben der Daten](how-to-write-data.md) in einen Speicherort, auf den Ihre Modelle zugreifen können

![Datenvorbereitungsprozess](./media/concept-data-preparation/data-prep-process.png)

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich ein [Beispielnotebook](https://github.com/Microsoft/AMLDataPrepDocs/tree/master/tutorials/getting-started/getting-started.ipynb) zur Datenvorbereitung mit dem Azure Machine Learning Data Prep SDK an.

Azure Machine Learning Data Prep SDK-[Referenzdokumentation](https://docs.microsoft.com/python/api/overview/azure/dataprep/intro?view=azure-dataprep-py).
