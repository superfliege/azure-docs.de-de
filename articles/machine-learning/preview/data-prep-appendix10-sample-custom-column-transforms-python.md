---
title: "Python-Beispiel für das Ableiten neuer Spalten in der Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
description: "Dieses Dokument enthält Python-Codebeispiele zum Erstellen neuer Spalten in der Azure Machine Learning-Datenvorbereitung."
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: 143031ce804f4a8dcd4e328c413478f5ea669090
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="sample-of-custom-column-transforms-python"></a>Beispiel für benutzerdefinierte Spaltentransformationen (Python) 
Der Name dieser Transformation im Menü ist **Add Column (Script)** (Spalte hinzufügen (Skript)).

Lesen Sie vor diesem Anhang den Artikel [Python-Erweiterungen für die Datenvorbereitung](data-prep-python-extensibility-overview.md).

## <a name="test-equivalence-and-replace-values"></a>Testen von Äquivalenz und Ersetzen von Werten 
Wenn der Wert in „Col1“ kleiner als 4 ist, sollte die neue Spalte einen Wert von 1 haben. Wenn der Wert in „Col1“ größer als 4 ist, hat die neue Spalte den Wert 2. 

```python
    1 if row["Col1"] < 4 else 2
```
## <a name="current-date-and-time"></a>Aktuelles Datum und Uhrzeit 

```python
    datetime.datetime.now()
```
## <a name="typecasting"></a>Typumwandlung 
```python
    float(row["Col1"]) / float(row["Col2"] - 1)
```
## <a name="evaluate-for-nullness"></a>Evaluieren auf Nullwertigkeit (NULL) 
Wenn „Col1“ eine Null enthält, soll die neue Spalte als **Bad** markiert werden. Markieren Sie sie andernfalls als **Good**. 

```python
    'Bad' if pd.isnull(row["Col1"]) else 'Good'
```
## <a name="new-computed-column"></a>Neu berechnete Spalte 
```python
    np.log(row["Col1"])
```
## <a name="epoch-computation"></a>Epochenberechnung 
Anzahl Sekunden seit der Unix-Epoche (unter der Voraussetzung, dass „Col1“ bereits ein Datum ist): 
```python
    row["Col1"] - datetime.datetime.utcfromtimestamp(0)).total_seconds()
```





