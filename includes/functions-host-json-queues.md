---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 7139b88ee9ef137ca28484538262b8bb2fe804db
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133360"
---
```json
{
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    }
}
```

|Eigenschaft  |Standard | BESCHREIBUNG |
|---------|---------|---------| 
|maxPollingInterval|60000|Das maximale Intervall in Millisekunden zwischen Warteschlangenumfragen.| 
|visibilityTimeout|0|Das Zeitintervall zwischen Wiederholungsversuchen, wenn bei der Verarbeitung einer Nachricht ein Fehler auftritt.| 
|batchSize|16|Die Anzahl der Warteschlangennachrichten, die die Functions-Runtime gleichzeitig abruft und parallel verarbeitet. Wenn die zu verarbeitende Anzahl `newBatchThreshold` erreicht, ruft die Runtime einen weiteren Batch ab und beginnt mit der Verarbeitung dieser Nachrichten. Aus diesem Grund beträgt die maximale Anzahl der pro Funktion verarbeiteten Nachrichten `batchSize` plus `newBatchThreshold`. Dieser Grenzwert gilt separat für jede Funktion, die durch die Warteschlange ausgelöst wird. <br><br>Wenn Sie eine parallele Ausführung für in einer Warteschlange empfangene Nachrichten vermeiden möchten, können Sie `batchSize` auf „1“ festlegen. Diese Einstellung verhindert Parallelität jedoch nur so lange, wie Ihre Funktions-App auf einem einzelnen virtuellen Computer (VM) ausgeführt wird. Wenn die Funktions-App horizontal auf mehrere virtuelle Computer hochskaliert wird, kann jeder virtuelle Computer eine Instanz jeder durch die Warteschlange ausgelösten Funktion ausführen.<br><br>Die maximale `batchSize` beträgt 32. | 
|maxDequeueCount|5|Die Anzahl der Versuche zum Verarbeiten einer Nachricht, bevor diese in die Warteschlange für nicht verarbeitete Nachrichten verschoben wird.| 
|newBatchThreshold|batchSize/2|Wenn die Anzahl der gleichzeitig verarbeiteten Nachrichten auf diesen Wert sinkt, ruft die Runtime einen weiteren Batch ab.| 



