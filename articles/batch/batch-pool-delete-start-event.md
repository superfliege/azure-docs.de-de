---
title: "Azure Batch: Ereignis zum Starten des Löschvorgangs von Pools | Microsoft-Dokumentation"
description: "Referenz zum Batch-Ereignis zum Starten des Löschvorgangs von Pools."
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
ms.openlocfilehash: f8a5241dce422e5c826ab428da6d7bc93284a1cf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="pool-delete-start-event"></a>Ereignis zum Starten des Löschvorgangs von Pools

 Dieses Ereignis wird ausgegeben, wenn ein Löschvorgang eines Pools gestartet wurde. Da das Löschen des Pools ein asynchrones Ereignis ist, können Sie damit rechnen, dass ein Ereignis zum Abschluss des Löschvorgangs von Pools ausgegeben wird, sobald der Löschvorgang abgeschlossen ist.

 Das folgende Beispiel zeigt den Text eines Ereignisses zum Starten des Löschvorgangs von Pools.

```
{
    "id": "myPool1"
}
```

|Element|Typ|Hinweise|
|-------------|----------|-----------|
|id|String|Die ID des Pools.|