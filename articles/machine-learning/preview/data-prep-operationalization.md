---
title: "Ausführliche Anleitung zur Verwendung der Operationalisierung für die Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
description: "Dieses Dokument enthält Informationen zur Ausführung von zuvor entworfenen Datenquellen und Datenvorbereitungspaketen."
services: machine-learning
author: hughz
ms.author: cforbe
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 02/13/2018
ms.openlocfilehash: 76bf850df5c8f4a93268dd06f7b9254bbd2cfce6
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="data-preparation-operationalization"></a>Operationalisierung für die Datenvorbereitung 

## <a name="operationalization-scenario"></a>Operationalisierungsszenario

Im Folgenden werden die unterschiedlichen Operationalisierungsszenarien für die Datenvorbereitung beschrieben, auf die ein Benutzer stoßen kann.

### <a name="develop-your-model-based-on-training-data"></a>Entwickeln des Modells anhand von Trainingsdaten

Angenommen, Sie möchten einen Echtzeitbewertungs-API-Dienst erstellen und bereitstellen. Der erste Schritt besteht in der Entwicklung eines Modells für die Bewertung basierend auf bestimmten Trainingsdaten. Bei der Datenvorbereitung wird eine Stichprobe Ihrer Trainingsdaten als neue Datenquelle importiert. Sobald die Daten vorbereitet sind, enthält das Datenvorbereitungspaket die Schritte zur Vorbereitung. Mit einem Azure Machine Learning-Experimentieren-Konto kann der Benutzer ein Modell für maschinelles Lernen zum Generieren von Bezeichnungen für jede Eingabe in den Trainingsdaten durchlaufen.

Wenn die Features der Daten aktualisiert werden müssen, kehrt der Benutzer zum Datenvorbereitungspaket zurück, um die Daten neu vorzubereiten und diese Schritte zu speichern. Dieser Iterationsvorgang, bei dem neue Features generiert werden und das Modell des maschinellen Lernens angepasst wird, setzt sich so lange fort, bis das Modell die Testdaten präzise bewertet. 

### <a name="deploy-your-model-to-the-azure-model-management-service"></a>Bereitstellen Ihres Modells im Azure-Modellverwaltung-Dienst

Sie haben jetzt die Kundendaten, die Sie in Echtzeit bewerten möchten. Mit dem Azure-Modellverwaltung-Dienst können Sie dieses Modell aus der Azure ML-Python-CLI als Dienst bereitstellen. Der bereitgestellte Dienst macht einen REST-Endpunkt für den Empfang von Kundendaten in Echtzeit und das Zurückgeben der entsprechenden Ausgangsbezeichnungen aus dem trainierten Modell verfügbar.

Der Einfachheit halber akzeptiert der Modellendpunkt Daten im JSON-Format. Die Eingabe muss eine JSON-Zeichenfolge in Form eines 2-dimensionalen Datenarrays sein, die durch die Transformation ReadJSONLiteral übergeben und in eine Datenquelle der Datenvorbereitung konvertiert wird. Das in der Experimentierphase erstellte Datenvorbereitungspaket kann dann für die gestreamten JSON-Daten ausgeführt werden. Der resultierende Datenrahmen kann anschließend zur Bewertung an das trainierte Modell übergeben werden.

## <a name="read-json-literal-transformation"></a>ReadJsonLiteral-Transformation

### <a name="description"></a>BESCHREIBUNG

Für die Operationalisierung enthält die Datenvorbereitung die **ReadJsonLiteral**-Transformation zum Ausführen einer Aktivität und Generieren eines Pandas- oder Spark-Datenrahmens. Diese Transformation ist dahin gehend besonders, dass sie als Eingabe ein vorhandenes Datenvorbereitungspaket und eine JSON-Datenquelle akzeptiert. Die Transformation wird über die Python-CLI der Datenvorbereitung verfügbar gemacht.

### <a name="instructions"></a>Anleitung

#### <a name="pythoncli"></a>PythonCLI

Öffnen Sie in Azure Machine Learning Workbench die Befehlszeilenschnittstelle („Datei > Eingabeaufforderung öffnen“).

In diesem Beispiel wird das Datenvorbereitungspaket „TrainingPreparationSteps“ zum Vorbereiten der Daten und zum Hinzufügen des Volumefeatures zu jeder Eingabe verwendet.

```
>>> import azureml.dataprep.package as azdp

>>> azdp.run_on_data.__doc__
"Generate a dataframe based on a selected dataflow in a package using in-memory data sources.
'user_config' is expected as a dictionary that maps the absolute path of a dsource file to an in-memory data source represented as a list of lists."

>>> real_time_customer_data = [[1.0, 1.5, 2.5], [2.5, 1.5, 3]]
>>> azdp.run_on_data({ "C:\\TrainingSampleData.dsource": real_time_customer_data}, "C:\\TrainingPreparationSteps.dprep")
    Height   Width   Depth  Volumne
0   1.0       1.5    2.5    3.75
1   2.5       1.5    3.0    11.25
```

`run_on_data()` weist die folgenden optionalen Parameter auf:
 - `dataflow_idx`: gibt den Index des gewünschten Datenflusses an, der im Paket ausgeführt werden soll
 - `secrets`: Wörterbuch für Geheimnisspeicher (Schlüssel: secretId, Wert: gespeichertes Geheimnis)
 - `spark`: stellt eine Spark-Sitzung für die Spark-Ausführung bereit

#### <a name="input"></a>Eingabe

Eine Eingabe in Form eines zweidimensionalen Arrays („Array von Arrays“). In Python sollte die Eingabe eine Liste von Listen sein. Da JSON keinen nativen Datentyp für Datumsangaben aufweist, werden Python-Objekte des Typs datetime.datetime in ISO-formatierte Datumszeichenfolgen konvertiert. Für REST-Endpunkte sollte die Eingabe eine JSON-Literalzeichenfolge sein, die ein zweidimensionales JSON-Array darstellt.

#### <a name="output"></a>Output

Ein Pandas- oder Spark-Datenrahmen. Der Typ des Datenrahmens wird von dem Framework festgelegt, das in der Ausführungskonfiguration (`.runconfig`) ausgewählt wurde. Der resultierende Datenrahmen kann zur Bewertung als Eingabe an das trainierte Modell übergeben werden.
