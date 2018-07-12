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
ms.openlocfilehash: d166a77a0636efea3b63660fde2187e3f2ec15c0
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38942963"
---
Wenn Sie Funktionen lokal entwickeln, können Sie die erforderlichen Erweiterungen mithilfe der Azure Functions Core Tools über das Terminal oder über eine Eingabeaufforderung installieren. 

Nachdem Sie Ihre Datei *function.json* aktualisiert haben, sodass sie alle von der Funktion benötigten Bindungen einschließt, führen Sie den Befehl `func extensions install` im Projektordner aus. Der Befehl liest die Datei *function.json*, um zu ermitteln, welche Pakete Sie benötigen, und installiert diese dann.

Wenn Sie eine bestimmte Version eines Pakets installieren möchten oder wenn Sie Pakete installieren möchten, bevor Sie die Datei *function.json* bearbeiten, verwenden Sie den Befehl `func extensions install` mit dem Namen des Pakets, wie im folgenden Beispiel gezeigt:

```
func extensions install --package Microsoft.Azure.WebJobs.ServiceBus --version <target_version>
```

Ersetzen Sie `<target_version>` durch eine bestimmte Version des Pakets, z.B. `3.0.0-beta5`. Gültige Versionen sind auf den Seiten der einzelnen Pakete auf [NuGet.org](https://nuget.org) aufgeführt.
