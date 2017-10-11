---
title: "Azure Batch-Pool-Größe ausgelöste Ereignis | Microsoft Docs"
description: "Referenz für die Batch-Pool Größe ausgelöste Ereignis."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 7072293d98526812cb42ce9c2f8e33bfcafaa149
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="pool-resize-complete-event"></a>Vollständige Adresspool Resize-Ereignis

 Dieses Ereignis wird ausgegeben, wenn ein Pool-Größe abgeschlossen oder fehlgeschlagen ist.

 Das folgende Beispiel zeigt den Text, der eine vollständige Adresspool Resize-Ereignis für einen Pool, die vergrößert und erfolgreich abgeschlossen.

```
{
    "id": "p_1_0_01503750-252d-4e57-bd96-d6aa05601ad8",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 4,
    "targetDedicated": 4,
    "enableAutoScale": false,
    "isAutoPool": false,
    "startTime": "2016-09-09T22:13:06.573Z",
    "endTime": "2016-09-09T22:14:01.727Z",
    "result": "Success",
    "resizeError": "The operation succeeded"
}
```

|Element|Typ|Hinweise|
|-------------|----------|-----------|
|id|String|Die Id des Pools.|
|nodeDeallocationOption|String|Gibt beim Knoten aus dem Pool entfernt werden können, wenn Verkleinerung des Pools.<br /><br /> Mögliche Werte:<br /><br /> **wieder in Warteschlange** – beenden Sie derzeit ausgeführte Tasks, und sie requeue. Die Tasks werden erneut ausgeführt, wenn der Auftrag aktiviert ist. Entfernen Sie Knoten aus, sobald die Tasks abgeschlossen wurden.<br /><br /> **Beenden** – terminieren zurzeit ausgeführter Tasks. Die Tasks werden nicht erneut ausgeführt. Entfernen Sie Knoten aus, sobald die Tasks abgeschlossen wurden.<br /><br /> **Taskcompletion** – zulassen gleichzeitig ausgeführten Aufgaben abgeschlossen. Planen Sie keine neuen Tasks beim Warten. Entfernen Sie Knoten aus, wenn alle Aufgaben abgeschlossen wurden.<br /><br /> **Retaineddata** -aktuell ausgeführter Aufgaben abgeschlossen ist, und warten, bis alle aufbewahrungsdauern für Daten ablaufen soll Aufgabe ermöglichen. Planen Sie keine neuen Tasks beim Warten. Entfernen Sie Knoten aus, wenn die Aufbewahrungsdauer für alle Tasks abgelaufen sind.<br /><br /> Der Standardwert ist wieder in Warteschlange.<br /><br /> Wenn die Poolgröße zunimmt, und klicken Sie dann der Wert, um festgelegt wird **ungültige**.|
|"currentdedicated"|Int32|Die Anzahl der derzeit dem Pool zugewiesenen Serverknoten.|
|"targetdedicated" ist|Int32|Die Anzahl der Serverknoten, die für den Pool angefordert werden.|
|"enableautoscale"|Bool|Gibt an, ob automatisch anpasst und die Poolgröße über die Zeit.|
|isAutoPool|Bool|Gibt an, ob der Pool über einen Auftrag Auftragseigenschaft Mechanismus erstellt wurde.|
|startTime|DateTime|Die Zeit, die Größe der Pool wurde gestartet.|
|EndTime|DateTime|Die Uhrzeit der poolgrößenänderungsfehler abgeschlossen wurde.|
|Ergebniscode|String|Das Ergebnis der Größenänderung.|
|resultMessage|String|Der größenänderungsfehler zurück enthält die Details des Ergebnisses.<br /><br /> Wenn die Größenänderung erfolgreich abgeschlossen gibt an, dass der Vorgang erfolgreich war.|