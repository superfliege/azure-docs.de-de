---
title: 'Azure Data Factory Mapping Data Flow: Filtertransformation'
description: 'Azure Data Factory Mapping Data Flow: Filtertransformation'
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: b7e7b123560aae3a2d3086c8536969297d31f7ba
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271166"
---
# <a name="azure-data-factory-mapping-data-flow-filter-transformation"></a>Azure Data Factory Mapping Data Flow: Filtertransformation

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Die Filtertransformation ermöglicht die Zeilenfilterung. Erstellen Sie einen Ausdruck, der die Filterbedingung definiert. Klicken Sie in das Textfeld, um den Ausdrucks-Generator zu starten. Erstellen Sie im Ausdrucks-Generator einen Filterausdruck, um zu steuern, welche Zeilen aus dem aktuellen Datenstrom zur nächsten Transformation durchlaufen (filtern) dürfen.

Filtern Sie beispielsweise nach der Spalte „loan_status“:

```
in([‘Default’, ‘Charged Off’, ‘Fully Paid’], loan_status).
```

Filtern Sie in der Filmedemo nach der Spalte „year“:

```
year > 1980
```
