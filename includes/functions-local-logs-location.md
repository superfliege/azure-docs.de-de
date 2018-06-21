---
title: Includedatei
description: Includedatei
services: functions
author: ggailey777
manager: cfowler
ms.service: functions
ms.topic: include
ms.date: 05/01/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 88c01e8e57d4a92478b8b1ca0689ff0f8e499b39
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "33814627"
---
Wenn der Functions-Host lokal ausgeführt wird, schreibt er Protokolle in den folgenden Pfad:

```
<DefaultTempDirectory>\LogFiles\Application\Functions
```

Unter Windows ist `<DefaultTempDirectory>` der erste gefundene Wert der Umgebungsvariablen TMP, TEMP, USERPROFILE oder das Windows-Verzeichnis.
Unter macOS oder Linux ist `<DefaultTempDirectory>` die Umgebungsvariable TMPDIR.

> [!NOTE]
> Wenn der Functions-Host gestartet wird, überschreibt er die vorhandene Dateistruktur im Verzeichnis.