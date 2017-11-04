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
ms.openlocfilehash: b241bad7b0060551eba5e78efbb1b729bf5d0098
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2017
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Aufgabenhubs in Durable Functions (Azure Functions)

Ein *Aufgabenhub* in [Durable Functions](durable-functions-overview.md) ist ein logischer Container für Azure Storage-Ressourcen, die zur Orchestrierung verwendet werden. Orchestrator- und Aktivitätsfunktionen können nur miteinander interagieren, wenn sie zum selben Aufgabenhub gehören.

Jede Funktions-App verfügt über einen separaten Aufgabenhub. Wenn mehrere Funktions-Apps ein Speicherkonto gemeinsam nutzen, enthält das Speicherkonto mehrere Aufgabenhubs. Das folgende Diagramm veranschaulicht einen Aufgabenhub pro Funktions-App in freigegebenen und dedizierten Speicherkonten.

![Diagramm mit freigegebenen und dedizierten Speicherkonten](media/durable-functions-task-hubs/task-hubs-storage.png)

## <a name="azure-storage-resources"></a>Azure Storage-Ressourcen

Ein Aufgabenhub umfasst folgende Speicherressourcen: 

* Mindestens eine Steuerwarteschlange.
* Eine Arbeitselement-Warteschlange.
* Eine Verlaufstabelle.
* Einen Speichercontainer, der mindestens ein Lease-Blob enthält.

Alle diese Ressourcen werden automatisch im standardmäßigen Azure Storage-Konto erstellt, wenn Orchestrator- oder Aktivitätsfunktionen ausgeführt werden oder ihre Ausführung geplant wird. Im Artikel [Performance and Scale](durable-functions-perf-and-scale.md) (Leistung und Skalierung) wird erläutert, wie diese Ressourcen verwendet werden.

## <a name="task-hub-names"></a>Aufgabenhubnamen

Aufgabenhubs werden über den Namen identifiziert, der in der Datei *host.json* deklariert ist, wie im folgenden Beispiel gezeigt:

```json
{
  "durableTask": {
    "HubName": "MyTaskHub"
  }
}
```

Aufgabenhubnamen müssen mit einem Buchstaben beginnen und bestehen nur aus Buchstaben und Ziffern. Wenn kein Name angegeben ist, wird der Standardname **DurableFunctionsHub** verwendet.

> [!NOTE]
> Die einzelnen Aufgabenhubs werden über den Namen unterschieden, wenn in einem freigegebenen Speicherkonto mehrere Aufgabenhubs vorhanden sind. Wenn sich mehrere Funktions-Apps ein freigegebenes Speicherkonto teilen, müssen Sie für die einzelnen Aufgabenhubs in der Datei *host.json* unterschiedliche Namen konfigurieren.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Versioning in Durable Functions (Azure Functions)](durable-functions-versioning.md) (Versionsverwaltung in Durable Functions [Azure Functions])
