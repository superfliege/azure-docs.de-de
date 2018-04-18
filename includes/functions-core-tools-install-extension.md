---
title: Includedatei
description: Includedatei
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 04/06/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 4c9b579534d9a7f2c55e9c589b1738fe060b1cf2
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
Wenn Sie Funktionen lokal entwickeln, können Sie die erforderlichen Erweiterungen mithilfe der Azure Functions Core Tools über das Terminal oder über eine Eingabeaufforderung installieren. Der folgende `func extensions install`-Befehl installiert die Azure Cosmos DB-Bindungserweiterung:

```
func extensions install --package Microsoft.Azure.WebJobs.Extensions.CosmosDB --version <target_version>
```

Ersetzen Sie `<taget_version>` durch eine bestimmte Version des Pakets. Gültige Versionen sind auf den Seiten der einzelnen Pakete auf [NuGet.org](https://nuget.org) aufgeführt.
