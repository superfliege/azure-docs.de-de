---
title: Includedatei
description: Includedatei
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 03/12/2019
ms.author: glenga
ms.custom: include file, fasttrack-edit
ms.openlocfilehash: 3b0be31afbce78c0de8dc919245e92bee7af04b0
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57964151"
---
## <a name="update-the-function"></a>Aktualisieren der Funktion

Standardmäßig erstellt die Vorlage eine Funktion, die bei Anforderungen einen Funktionsschlüssel benötigt. Um das Testen der Funktion in Azure zu vereinfachen, müssen Sie die Funktion aktualisieren, um anonymen Zugriff zuzulassen. Wie Sie diese Änderung vornehmen, hängt von Ihrer Funktionenprojektsprache ab.

### <a name="c"></a>C\#

Öffnen Sie die MyHttpTrigger.cs-Codedatei, die Ihre neue Funktion ist, aktualisieren Sie das **AuthorizationLevel**-Attribut in der Funktionsdefinition mit dem Wert `Anonymous`, und speichern Sie die Änderungen.

```csharp
[FunctionName("MyHttpTrigger")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)] HttpRequest req,
    ILogger log)
```

### <a name="javascript"></a>JavaScript

Öffnen Sie die Datei „function.json“ für Ihre neue Funktion, öffnen Sie sie in einem Text-Editor, aktualisieren Sie die **authLevel**-Eigenschaft in **bindings.httpTrigger** in `anonymous`, und speichern Sie die Änderungen.

```json
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
```

Jetzt können Sie die Funktion in Azure aufrufen, ohne den Funktionsschlüssel anzugeben. Der Funktionsschlüssel ist bei lokaler Ausführung nie erforderlich.
