---
title: Beispiele für mögliche Transformationen „Datenfluss transformieren“ für die Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation
description: Dieses Dokument enthält eine Reihe von Beispielen für Datenflusstransformationen, die mit der Azure Machine Learning-Datenvorbereitung möglich sind.
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
ROBOTS: NOINDEX
ms.openlocfilehash: 82295e412c70065ff8ce3a686aec790614f39f2e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46947199"
---
# <a name="sample-of-custom-data-flow-transforms-python"></a>Beispiel für benutzerdefinierte Datenflusstransformationen (Python) 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


Der Name der Transformation im Menü ist **Transform Dataflow (Script)**. Lesen Sie vor diesem Anhang den Artikel [Python-Erweiterungen für die Datenvorbereitung](data-prep-python-extensibility-overview.md).

## <a name="transform-frame"></a>Transformieren von Frames
### <a name="create-a-new-column-dynamically"></a>Dynamisches Erstellen einer neuen Spalte 
Erstellt dynamisch eine Spalte (**city2**) und bringt mehrere Versionen von „San Francisco“ mit einer aus der vorhandenen Spalte „city“ in Einklang.
```python
    df.loc[(df['city'] == 'San Francisco') | (df['city'] == 'SF') | (df['city'] == 'S.F.') | (df['city'] == 'SAN FRANCISCO'), 'city2'] = 'San Francisco'
```

### <a name="add-new-aggregates"></a>Hinzufügen von neuen Aggregaten
Erstellt einen neuen Frame mit dem ersten und letzten Aggregat, das für die Ergebnisspalte berechnet wurde. Diese werden nach der Spalte **risk_category** gruppiert.
```python
    df = df.groupby(['risk_category'])['Score'].agg(['first','last'])
```
### <a name="winsorize-a-column"></a>Winsorisieren einer Spalte 
Formuliert die Daten neu, um eine Formel zur Reduzierung der Ausreißer in einer Spalte zu erfüllen.
```python
    import scipy.stats as stats
    df['Last Order'] = stats.mstats.winsorize(df['Last Order'].values, limits=0.4)
```

## <a name="transform-data-flow"></a>Transformieren des Datenflusses
### <a name="fill-down"></a>Abwärtsgerichtetes Ausfüllen 

Für das abwärtsgerichtete Ausfüllen sind zwei Transformationen erforderlich. Es werden Daten vorausgesetzt, die wie in der folgenden Tabelle aussehen:

|Zustand         |City       |
|--------------|-----------|
|Washington    |Redmond    |
|              |Bellevue   |
|              |Issaquah   |
|              |Seattle    |
|Kalifornien    |Los Angeles|
|              |San Diego  |
|              |San Jose   |
|Texas         |Dallas     |
|              |San Antonio|
|              |Houston    |

1. Erstellen Sie eine Transformation namens „Add Column (Script)“ mit folgendem Code:
```python
    row['State'] if len(row['State']) > 0 else None
```

2. Erstellen Sie eine Transformation namens „Transform Data Flow (Script)“, die den folgenden Code enthält:
```python
    df = df.fillna( method='pad')
```

Die Daten sehen nun wie in der folgenden Tabelle aus:

|Zustand         |newState         |City       |
|--------------|--------------|-----------|
|Washington    |Washington    |Redmond    |
|              |Washington    |Bellevue   |
|              |Washington    |Issaquah   |
|              |Washington    |Seattle    |
|Kalifornien    |Kalifornien    |Los Angeles|
|              |Kalifornien    |San Diego  |
|              |Kalifornien    |San Jose   |
|Texas         |Texas         |Dallas     |
|              |Texas         |San Antonio|
|              |Texas         |Houston    |


### <a name="min-max-normalization"></a>Min-Max-Normalisierung
```python
    df["NewCol"] = (df["Col1"]-df["Col1"].mean())/df["Col1"].std()
```