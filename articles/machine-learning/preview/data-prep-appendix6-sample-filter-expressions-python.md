---
title: "Mögliche Beispielfilterausdrücke für die Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
description: "Dieses Dokument enthält eine Reihe von Beispielen für mögliche Filterausdrücke für die Azure Machine Learning-Datenvorbereitung."
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 973c56b8b2821c8e3d63161e6a233243639c74f4
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2018
---
# <a name="sample-of-filter-expressions-python"></a>Beispiel für Filterausdrücke (Python) 
Lesen Sie vor diesem Anhang den Artikel [Python-Erweiterungen für die Datenvorbereitung](data-prep-python-extensibility-overview.md).

## <a name="filter-with-equivalence-test"></a>Filtern mit Äquivalenztest
Filtern Sie nur in Zeilen, bei denen der (numerische) Wert von Col2 größer ist als 4. 

```python
    row["Col2"] > 4
```

## <a name="filter-with-multiple-columns"></a>Filtern mit mehreren Spalten 
Filtern Sie nur in Zeilen, bei denen Col1 den Wert **Good** und Col2 den (numerischen) Wert 1 enthält. 
```python
    row["Col1"] == 'Good' and row["Col2"] == 1
```

## <a name="test-filter-against-null"></a>Testen von Filtern mit NULL
Filtern Sie nur in Zeilen, bei denen Col1 einen NULL-Wert enthält. 
```python
    pd.isnull(row["Col1"])
```
