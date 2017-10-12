---
title: "Beispiele für mögliche Transformationen „Datenfluss transformieren“ für die Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
description: "Dieses Dokument enthält eine Reihe von Beispielen für mögliche Transformationen „Datenfluss transformieren“ für die Azure ML-Datenvorbereitung."
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
ms.openlocfilehash: f43f65ca89349fc790684e9bd7acd2f19e15abe5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="sample-of-custom-data-flow-transforms-python"></a>Beispiel für benutzerdefinierte Datenflusstransformationen (Python) 
Der Name dieser Transformation im Menü lautet „Datenfluss transformieren (Skript)“. Lesen Sie vor diesem Anhang den Artikel [Python-Erweiterbarkeit – Übersicht](data-prep-python-extensibility-overview.md).

## <a name="transform-frame"></a>Transformieren von Frames
### <a name="create-a-new-column-dynamically"></a>Dynamisches Erstellen einer neuen Spalte 
Erstellen Sie dynamisch eine Spalte (city2), und stimmen Sie mehrere Versionen von San Francisco in eine der vorhandenen Spalte „city“ aufeinander ab.
```python
    df.loc[(df['city'] == 'San Francisco') | (df['city'] == 'SF') | (df['city'] == 'S.F.') | (df['city'] == 'SAN FRANCISCO'), 'city2'] = 'San Francisco'
```

### <a name="add-new-aggregates"></a>Hinzufügen von neuen Aggregaten
Erstellen Sie einen neuen Frame mit dem ersten und letzten Aggregat, die für die nach der Spalte „risk_category“ gruppierten Ergebnisspalte berechnet wurde.
```python
    df = df.groupby(['risk_category'])['Score'].agg(['first','last'])
```
### <a name="winsorize-a-column"></a>Winsorisieren einer Spalte 
Formulieren Sie die Daten neu, um eine Formel zur Reduzierung der Ausreißer in einer Spalte zu erfüllen.
```python
    import scipy.stats as stats
    df['Last Order'] = stats.mstats.winsorize(df['Last Order'].values, limits=0.4)
```

## <a name="transform-data-flow"></a>Transformieren des Datenflusses
### <a name="fill-down"></a>Abwärtsgerichtetes Ausfüllen 
Für das abwärtsgerichtete Ausfüllen sind zwei Transformationen erforderlich.
Angenommen, die Daten sehen wie folgt aus:


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

Erstellen Sie zuerst eine Transformation namens „Spalte hinzufügen (Skript)“, die den folgenden Code enthält:
```python
    row['State'] if len(row['State']) > 0 else None
```
Erstellen Sie zuerst eine Transformation namens „Datenfluss transformieren (Skript)“, die den folgenden Code enthält:
```python
    df = df.fillna( method='pad')
```

Die Daten sehen nun folgendermaßen aus:

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

