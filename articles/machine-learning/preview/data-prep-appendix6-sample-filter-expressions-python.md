---
title: "Mögliche Beispielfilterausdrücke für die Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
description: "Dieses Dokument enthält eine Reihe von Beispielen für mögliche Filterausdrücke für die Azure Machine Learning-Datenvorbereitung."
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: df78e871625f4de406de5ba4ae0fea327b87b01e
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2018
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
