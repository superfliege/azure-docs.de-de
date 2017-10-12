---
title: "Mögliche Beispielziele bzw. -ausgaben für die Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
description: "Dieses Dokument enthält eine Reihe von Beispielen für benutzerdefinierte Datenziele/-ausgaben für die Azure ML-Datenvorbereitung."
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
ms.openlocfilehash: e85ab5d52c2265536f72fa7301c0a25fa3a112cc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="sample-of-destination-connections-python"></a>Beispiele für Zielverbindungen (Python) 
Lesen Sie vor diesem Anhang den Artikel [Python-Erweiterbarkeit – Übersicht](data-prep-python-extensibility-overview.md).


### <a name="write-to-excel"></a>Schreiben in Excel 


Für das Schreiben in Excel ist eine zusätzliche Bibliothek erforderlich. Das Hinzufügen neuer Bibliotheken wird in der Übersicht über die Erweiterbarkeit dokumentiert. `openpyxl` ist die Bibliothek, die hinzugefügt werden muss.

Vor dem Schreibvorgang sind möglicherweise weitere Änderungen erforderlich. Einige der bei der Datenvorbereitung verwendeten Datentypen werden in einigen Zielformaten nicht unterstützt. Wenn beispielsweise „Error“-Objekte vorhanden sind, werden diese nicht ordnungsgemäß nach Excel serialisiert. Bevor Sie einen Schreibvorgang nach Excel durchführen, ist daher eine Transformation „Fehlerwerte ersetzen“ erforderlich, durch die Fehler aus allen Spalten entfernt werden.

Wenn alle oben aufgeführten Aufgaben durchgeführt wurden, schreibt die folgende Zeile die Datentabelle in ein einziges Arbeitsblatt eines Excel-Dokuments. Fügen Sie die Transformation „Datenfluss schreiben (Skript)“ hinzu, und geben Sie den folgenden Code in einen Abschnitt für den Ausdruck ein:


#### <a name="on-windows"></a>Unter Windows 
```python
df.to_excel('c:\dev\data\Output\Customer.xlsx', sheet_name='Sheet1')
```

#### <a name="on-macosos-x"></a>Unter macOS/OS X ###
```python
df.to_excel('c:/dev/data/Output/Customer.xlsx', sheet_name='Sheet1')
```
