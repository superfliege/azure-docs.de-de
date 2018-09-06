---
title: Includedatei
description: Includedatei
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 08/12/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 3cbe634d862682a5f6b06c2cfc77a4d3b03954f9
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2018
ms.locfileid: "42809876"
---
Der Code für alle Funktionen in einer bestimmten Funktions-App befindet sich in einem Stammordner (`wwwroot`), der eine Hostkonfigurationsdatei und mindestens einen Unterordner enthält. Jeder Unterordner enthält den Code für eine separate Funktion, wie im folgenden Beispiel gezeigt:

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
 | - bin
 | | - mycompiledcsharp.dll
```

Die Datei „host.json“ enthält die laufzeitspezifische Konfiguration und befindet sich im Stammordner der Funktions-App. Informationen zu den verfügbaren Einstellungen finden Sie in der [host.json-Referenz](../articles/azure-functions/functions-host-json.md).

Jede Funktion verfügt über einen Ordner, der mindestens eine Codedatei, die function.json-Konfiguration sowie weitere Abhängigkeiten enthält. Für ein C#-Klassenbibliotheksprojekt wird die Datei der kompilierten Bibliothek (.dll) im Unterordner `bin` bereitgestellt.

