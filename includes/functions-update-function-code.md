---
title: Includedatei
description: Includedatei
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 09/16/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: d264477693458ff4132c1f69704a480eed2756e0
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988001"
---
## <a name="update-the-function"></a>Aktualisieren der Funktion

Standardmäßig erstellt die Vorlage eine Funktion, die bei Anforderungen einen Funktionsschlüssel benötigt. Um das Testen der Funktion in Azure zu vereinfachen, müssen Sie die Funktion aktualisieren, um anonymen Zugriff zuzulassen. Wie Sie diese Änderung vornehmen, hängt von Ihrer Funktionenprojektsprache ab.

### <a name="c"></a>C\#

Öffnen Sie die MyHttpTrigger.cs-Codedatei, die Ihre neue Funktion ist, aktualisieren Sie das **AuthorizationLevel**-Attribut in der Funktionsdefinition mit dem Wert `anonymous`, und speichern Sie die Änderungen.

```csharp
[FunctionName("MyHttpTrigger")]
        public static IActionResult Run([HttpTrigger(AuthorizationLevel.Anonymous, 
            "get", "post", Route = null)]HttpRequest req, ILogger log)
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
