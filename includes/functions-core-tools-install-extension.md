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
ms.openlocfilehash: 6fb497a5b6da00dece43c7f41ea3c411f385a2ba
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726886"
---
Wenn Sie Funktionen lokal entwickeln, können Sie die erforderlichen Erweiterungen mithilfe der Azure Functions Core Tools über das Terminal oder über eine Eingabeaufforderung installieren. 

Nachdem Sie Ihre Datei *function.json* aktualisiert haben, sodass sie alle von der Funktion benötigten Bindungen einschließt, führen Sie den Befehl `func extensions install` im Projektordner aus. Der Befehl liest die Datei *function.json*, um zu ermitteln, welche Pakete Sie benötigen, und installiert diese dann.

Wenn Sie eine bestimmte Version eines Pakets installieren möchten oder wenn Sie Pakete installieren möchten, bevor Sie die Datei *function.json* bearbeiten, verwenden Sie den Befehl `func extensions install` mit dem Namen des Pakets, wie im folgenden Beispiel gezeigt:

```
func extensions install --package Microsoft.Azure.WebJobs.Extensions.CosmosDB --version <target_version>
```

Ersetzen Sie `<target_version>` durch eine bestimmte Version des Pakets. Gültige Versionen sind auf den Seiten der einzelnen Pakete auf [NuGet.org](https://nuget.org) aufgeführt.
