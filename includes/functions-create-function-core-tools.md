---
title: Includedatei
description: Includedatei
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 10/20/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 26789a12053fa6275b09836e706c391e181c8efd
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53429963"
---
## <a name="create-a-function"></a>Erstellen einer Funktion

Der folgende Befehl erstellt eine per HTTP ausgelöste Funktion namens `MyHttpTrigger`.

```bash
func new --name MyHttpTrigger --template "HttpTrigger"
```

Wenn der Befehl ausgeführt wird, sehen Sie etwa folgende Ausgabe:

```output
The function "MyHttpTrigger" was created successfully from the "HttpTrigger" template.
```
