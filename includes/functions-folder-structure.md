---
title: Includedatei
description: Includedatei
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 09/12/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 2808264b4641bda49a53677ebe216a3b53b7d0d9
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293562"
---
Der Code für alle Funktionen in einer bestimmten Funktions-App befindet sich in einem Stammprojektordner, der eine Hostkonfigurationsdatei und mindestens einen Unterordner enthält. Jeder Unterordner enthält den Code für eine separate Funktion, wie in folgender Darstellung gezeigt:

```
FunctionApp
 | - host.json
 | - Myfirstfunction
 | | - function.json
 | | - ...  
 | - mysecondfunction
 | | - function.json
 | | - ...  
 | - SharedCode
 | - bin
```

In der Version 2.x der Functions-Runtime müssen sich alle Funktionen in der Funktions-App denselben Spracharbeitsthread teilen.  

Die Datei [host.json](../articles/azure-functions/functions-host-json.md), die die laufzeitspezifische Konfiguration enthält, befindet sich im Stammordner der Funktions-App. Ein `bin`-Ordner enthält Pakete und andere Bibliotheksdateien, die von der Funktions-App benötigt werden. Sprachspezifische Anforderungen für ein Funktions-App-Projekt:

* [C#-Klassenbibliothek (.csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
* [C#-Skript (.csx)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
* [F#-Skript](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
* [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
* [JavaScript](../articles/azure-functions/functions-reference-node.md#folder-structure)



