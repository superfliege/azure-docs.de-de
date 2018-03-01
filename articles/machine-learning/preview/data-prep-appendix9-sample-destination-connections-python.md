---
title: "Mögliche Beispielziele bzw. -ausgaben für die Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
description: "Dieses Dokument enthält eine Reihe von Beispielen für benutzerdefinierte Datenziele/-ausgaben für die Azure Machine Learning-Datenvorbereitung."
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
ms.openlocfilehash: 203c8399153d2bc2d855fc2602b01ed074852687
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="sample-of-destination-connections-python"></a>Beispiele für Zielverbindungen (Python) 
Lesen Sie vor diesem Anhang den Artikel [Python-Erweiterungen für die Datenvorbereitung](data-prep-python-extensibility-overview.md).


## <a name="write-to-excel"></a>Schreiben in Excel 


Das Schreiben in Excel erfordert eine zusätzliche Bibliothek. Hinzufügen von neuen Bibliotheken ist in der Erweiterbarkeitsübersicht dokumentiert. `openpyxl` ist die Bibliothek, die Sie hinzufügen müssen.

Vor dem Schreiben in Excel sind möglicherweise weitere Änderungen erforderlich. Einige der bei der Datenvorbereitung verwendeten Datentypen werden in einigen Zielformaten nicht unterstützt. Wenn beispielsweise „Error“-Objekte vorhanden sind, werden diese nicht ordnungsgemäß nach Excel serialisiert. Bevor Sie versuchen, nach Excel zu schreiben, benötigen Sie daher eine Transformation „Fehlerwerte ersetzen“, die Fehler aus den Spalten entfernt.

Wenn alle oben aufgeführten Aufgaben durchgeführt wurden, schreibt die folgende Zeile die Datentabelle in ein einziges Arbeitsblatt eines Excel-Dokuments. Fügen Sie eine Transformation „Datenfluss transformieren (Skript)“ hinzu. Geben Sie dann den folgenden Code in einen Ausdrucksabschnitt ein.


### <a name="on-windows"></a>Unter Windows 
```python
df.to_excel('c:\dev\data\Output\Customer.xlsx', sheet_name='Sheet1')
```

### <a name="on-macosos-x"></a>Unter macOS/OS X ###
```python
df.to_excel('c:/dev/data/Output/Customer.xlsx', sheet_name='Sheet1')
```
