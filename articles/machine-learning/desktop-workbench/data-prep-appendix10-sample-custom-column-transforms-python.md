---
title: Python-Beispiel für das Ableiten neuer Spalten in der Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation
description: Dieses Dokument enthält Python-Codebeispiele zum Erstellen neuer Spalten in der Azure Machine Learning-Datenvorbereitung.
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: d9f8bb20325ff15b6ba67253de5e66d1d1d8e643
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2018
ms.locfileid: "35635234"
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

## <a name="hash-a-column-value-into-a-new-column"></a>Hashen eines Spaltenwerts in eine neue Spalte
```python
    import hashlib
    hash(row["MyColumnToHashCol1"])

```




