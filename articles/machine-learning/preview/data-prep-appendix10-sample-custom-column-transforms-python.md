---
title: "Python-Beispiel für das Ableiten neuer Spalten in der Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
description: "Dieses Dokument enthält Python-Codebeispiele zum Erstellen neuer Spalten in der Azure ML-Datenvorbereitung."
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
ms.openlocfilehash: 9a6e331e622b007232a62b34c2220d60d1d050ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="sample-of-custom-column-transforms-python"></a>Beispiel für benutzerdefinierte Spaltentransformationen (Python) 
Der Name dieser Transformation im Menü ist „Add Column (Script)“.

Lesen Sie vor diesem Anhang den Artikel [Python-Erweiterbarkeit – Übersicht](data-prep-python-extensibility-overview.md).

## <a name="test-equivalence-and-replace-values"></a>Testen von Äquivalenz und Ersetzen von Werten 
Wenn der Wert in „Col1“ kleiner als 4 ist, sollte die neue Spalte einen Wert von 1 haben – andernfalls hat sie den Wert 2. 

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
Wenn „Col1“ einen NULL-Wert enthält, soll die neue Spalte als „Bad“ markiert werden, andernfalls als „Good“. 

```python
    'Bad' if pd.isnull(row["Col1"]) else 'Good'
```
## <a name="new-computed-column"></a>Neu berechnete Spalte 
```python
    np.log(row["Col1"])
```
## <a name="epoch-computation"></a>Epochenberechnung 
Anzahl Sekunden seit der Unix-Epoche (unter der Voraussetzung, dass „Col1“ bereits ein Datum ist) 
```python
    row["Col1"] - datetime.datetime.utcfromtimestamp(0)).total_seconds()
```





