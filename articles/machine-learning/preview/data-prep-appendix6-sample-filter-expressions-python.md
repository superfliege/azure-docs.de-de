---
title: "Mögliche Beispielfilterausdrücke für die Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
description: "Dieses Dokument enthält eine Reihe von Beispielen für mögliche Filterausdrücke für die Azure Machine Learning-Datenvorbereitung."
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
ms.date: 09/11/2017
ms.openlocfilehash: f8b18b2a94885a76df4026a29400f4d651cbcad4
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
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
