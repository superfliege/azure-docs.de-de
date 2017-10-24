---
title: "Aufgabenhubs in Durable Functions – Azure"
description: "Sie erfahren, was ein Aufgabenhub in der Erweiterung Durable Functions für Azure Functions ist. Informationen zum Konfigurieren von Aufgabenhubs."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 313daf1c105caa8569ed43e59d9e18f184599214
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Aufgabenhubs in Durable Functions (Azure Functions)

Ein *Aufgabenhub* für [Durable Functions](durable-functions-overview.md) ist ein logischer Container für Orchestrierungen und Aktivitäten im Kontext eines einzelnen Azure-Speicherkontos. Mehrere Funktionen und sogar Funktionen-Apps können im selben Hub vorhanden sein, und der Aufgabenhub dient häufig als Anwendungscontainer.

Aufgabenhubs müssen nicht explizit erstellt werden. Sie werden automatisch von der Laufzeit unter Verwendung eines Namens initialisiert, der in der *host.json*-Datei deklariert wird. Jeder Aufgabenhub verfügt über einen eigenen Satz von Speicherwarteschlangen, -tabellen und -blobs in einem einzelnen Speicherkonto. Alle Funktionen-Apps, die in einem bestimmten Aufgabenhub ausgeführt werden, nutzen dieselben Speicherressourcen. Orchestrator- und Aktivitätsfunktionen können nur miteinander interagieren, wenn sie zum selben Aufgabenhub gehören.

## <a name="configuring-a-task-hub-in-hostjson"></a>Konfigurieren eines Aufgabenhubs in „host.json“

Ein Aufgabenhubname kann in der Datei *host.json* einer Funktionen-App wie folgt konfiguriert werden.

```json
{
  "durableTask": {
    "HubName": "MyTaskHub"
  }
}
```

Aufgabenhubnamen müssen mit einem Buchstaben beginnen und bestehen nur aus Buchstaben und Ziffern. Wenn nicht angegeben, lautet der standardmäßige Aufgabenhubname für eine Funktionen-App **DurableFunctionsHub**.

> [!NOTE]
> Wenn Sie über mehrere Funktionen-Apps verfügen, die ein gemeinsames Speicherkonto nutzen, sollten Sie für jede Funktionen-App einen anderen Aufgabenhubnamen konfigurieren. Dadurch wird sichergestellt, dass die Funktionen-Apps ordnungsgemäß voneinander isoliert sind.

## <a name="azure-storage-resources"></a>Azure Storage-Ressourcen

Ein Aufgabenhub umfasst mehrere Azure Storage-Ressourcen:

* Mindestens eine Steuerwarteschlange.
* Eine Arbeitselement-Warteschlange.
* Eine Verlaufstabelle.
* Einen Speichercontainer, der mindestens ein Lease-Blob enthält.

Alle diese Ressourcen werden automatisch im standardmäßigen Azure Storage-Konto erstellt, wenn Orchestrator- oder Aktivitätsfunktionen ausgeführt werden oder ihre Ausführung geplant wird. Im Artikel [Performance and scale in Durable Functions (Azure Functions)](durable-functions-perf-and-scale.md) (Leistung und Skalierung in Durable Functions [Azure Functions]) wird erläutert, wie diese Ressourcen verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Versioning in Durable Functions (Azure Functions)](durable-functions-versioning.md) (Versionsverwaltung in Durable Functions [Azure Functions])

