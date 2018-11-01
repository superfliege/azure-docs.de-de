---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 0a3f739bae3ec758c8e25d581d3e2b9feb4af5b1
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50134474"
---
```json
{
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    }
}
```

|Eigenschaft  |Standard | BESCHREIBUNG |
|---------|---------|---------| 
|maxConcurrentCalls|16|Die maximale Anzahl gleichzeitiger Aufrufe für den Rückruf, der vom Nachrichtensystem initiiert werden soll. Die Functions-Runtime verarbeitet standardmäßig mehrere Nachrichten gleichzeitig. Um die Runtime anzuweisen, jeweils nur eine Warteschlangen- oder Themennachricht zu verarbeiten, legen Sie `maxConcurrentCalls` auf „1“ fest. | 
|prefetchCount|–|Das standardmäßige PrefetchCount, das von dem zugrunde liegenden MessageReceiver verwendet wird.| 
|autoRenewTimeout|00:05:00|Die maximale Zeitspanne, in der die Nachrichtensperre automatisch erneuert wird.| 
