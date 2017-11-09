---
title: "Mögliche Beispielziele bzw. -ausgaben für die Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
description: "Dieses Dokument enthält eine Reihe von Beispielen für benutzerdefinierte Datenziele/-ausgaben für die Azure Machine Learning-Datenvorbereitung."
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
ms.openlocfilehash: ca9e7d8c318100ba498ee15be15e213905f1e8d2
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="sample-of-destination-connections-python"></a>Beispiele für Zielverbindungen (Python) 
Lesen Sie vor diesem Anhang den Artikel [Python-Erweiterungen für die Datenvorbereitung](data-prep-python-extensibility-overview.md).


## <a name="write-to-excel"></a>Schreiben in Excel 


Das Schreiben in Excel erfordert eine zusätzliche Bibliothek. Hinzufügen von neuen Bibliotheken ist in der Erweiterbarkeitsübersicht dokumentiert. `openpyxl` ist die Bibliothek, die Sie hinzufügen müssen.

Vor dem Schreiben in Excel sind möglicherweise weitere Änderungen erforderlich. Einige der bei der Datenvorbereitung verwendeten Datentypen werden in einigen Zielformaten nicht unterstützt. Wenn beispielsweise „Error“-Objekte vorhanden sind, werden diese nicht ordnungsgemäß nach Excel serialisiert. Bevor Sie versuchen, nach Excel zu schreiben, benötigen Sie daher eine Transformation „Fehlerwerte ersetzen“, die Fehler aus den Spalten entfernt.

Wenn alle oben aufgeführten Aufgaben durchgeführt wurden, schreibt die folgende Zeile die Datentabelle in ein einziges Arbeitsblatt eines Excel-Dokuments. Fügen Sie eine Transformation „DataFlow schreiben (Skript)“ hinzu. Geben Sie dann den folgenden Code in einen Ausdrucksabschnitt ein.


### <a name="on-windows"></a>Unter Windows 
```python
df.to_excel('c:\dev\data\Output\Customer.xlsx', sheet_name='Sheet1')
```

### <a name="on-macosos-x"></a>Unter macOS/OS X ###
```python
df.to_excel('c:/dev/data/Output/Customer.xlsx', sheet_name='Sheet1')
```
