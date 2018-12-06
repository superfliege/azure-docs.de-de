---
title: Aufgabenhubs in Durable Functions – Azure
description: Sie erfahren, was ein Aufgabenhub in der Erweiterung Durable Functions für Azure Functions ist. Informationen zum Konfigurieren von Aufgabenhubs.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 7a6346e594c5a7cc4cf02f3ea658aac4977e641a
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "52637455"
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Aufgabenhubs in Durable Functions (Azure Functions)

Ein *Aufgabenhub* in [Durable Functions](durable-functions-overview.md) ist ein logischer Container für Azure Storage-Ressourcen, die zur Orchestrierung verwendet werden. Orchestrator- und Aktivitätsfunktionen können nur miteinander interagieren, wenn sie zum selben Aufgabenhub gehören.

Jede Funktions-App verfügt über einen separaten Aufgabenhub. Wenn mehrere Funktions-Apps ein Speicherkonto gemeinsam nutzen, enthält das Speicherkonto mehrere Aufgabenhubs. Das folgende Diagramm veranschaulicht einen Aufgabenhub pro Funktions-App in freigegebenen und dedizierten Speicherkonten.

![Diagramm mit freigegebenen und dedizierten Speicherkonten](./media/durable-functions-task-hubs/task-hubs-storage.png)

## <a name="azure-storage-resources"></a>Azure Storage-Ressourcen

Ein Aufgabenhub umfasst folgende Speicherressourcen: 

* Mindestens eine Steuerwarteschlange.
* Eine Arbeitselement-Warteschlange.
* Eine Verlaufstabelle.
* Eine Instanzentabelle.
* Einen Speichercontainer, der mindestens ein Lease-Blob enthält.

Alle diese Ressourcen werden automatisch im standardmäßigen Azure Storage-Konto erstellt, wenn Orchestrator- oder Aktivitätsfunktionen ausgeführt werden oder ihre Ausführung geplant wird. Im Artikel [Performance and Scale](durable-functions-perf-and-scale.md) (Leistung und Skalierung) wird erläutert, wie diese Ressourcen verwendet werden.

## <a name="task-hub-names"></a>Aufgabenhubnamen

Aufgabenhubs werden über den Namen identifiziert, der in der Datei *host.json* deklariert ist, wie im folgenden Beispiel gezeigt:

### <a name="hostjson-functions-v1"></a>host.json (Functions v1)
```json
{
  "durableTask": {
    "HubName": "MyTaskHub"
  }
}
```
### <a name="hostjson-functions-v2"></a>host.json (Functions v2)
```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "HubName": "MyTaskHub"
    }
  }
}
```
Aufgabenhubs können auch mithilfe von App-Einstellungen konfiguriert werden, wie in der folgenden Beispieldatei *host.json* gezeigt:

### <a name="hostjson-functions-v1"></a>host.json (Functions v1)
```json
{
  "durableTask": {
    "HubName": "%MyTaskHub%"
  }
}
```
### <a name="hostjson-functions-v2"></a>host.json (Functions v2)
```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "HubName": "%MyTaskHub%"
    }
  }
}
```
Der Name des Aufgabenhubs wird auf den Wert der App-Einstellung `MyTaskHub` festgelegt. Die folgende Datei `local.settings.json` veranschaulicht, wie die Einstellungen für `MyTaskHub` als `samplehubname` definiert werden:

```json
{
  "IsEncrypted": false,
  "Values": {
    "MyTaskHub" :  "samplehubname" 
  }
}
```

Aufgabenhubnamen müssen mit einem Buchstaben beginnen und bestehen nur aus Buchstaben und Ziffern. Wenn kein Name angegeben ist, wird der Standardname **DurableFunctionsHub** verwendet.

> [!NOTE]
> Die einzelnen Aufgabenhubs werden über den Namen unterschieden, wenn in einem freigegebenen Speicherkonto mehrere Aufgabenhubs vorhanden sind. Wenn sich mehrere Funktions-Apps ein freigegebenes Speicherkonto teilen, müssen Sie für die einzelnen Aufgabenhubs in der Datei *host.json* unterschiedliche Namen konfigurieren.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Versioning in Durable Functions (Azure Functions)](durable-functions-versioning.md) (Versionsverwaltung in Durable Functions [Azure Functions])
