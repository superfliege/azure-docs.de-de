---
title: Includedatei
description: Includedatei
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 09/21/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: f1b53c53b1e5fb089eb9b8a9b816b11a1eea126d
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47044508"
---
Wenn Sie Funktionen lokal entwickeln, können Sie die erforderlichen Erweiterungen mithilfe der Azure Functions Core Tools über das Terminal oder über eine Eingabeaufforderung installieren.

Nachdem Sie Ihre Datei *function.json* aktualisiert haben, sodass sie alle von der Funktion benötigten Bindungen einschließt, führen Sie den folgenden Befehl im Projektordner aus.

```bash
func extensions install
```

Der Befehl liest die Datei *function.json*, um zu ermitteln, welche Pakete Sie benötigen, installiert diese und erzeugt das Erweiterungenprojekt neu. Er fügt alle neuen Bindungen an die aktuelle Version hinzu, aktualisiert aber keine vorhandenen Bindungen. Verwenden Sie die Option `--force`, um vorhandene Bindungen beim Installieren neuer auf die neueste Version zu aktualisieren.

Wenn Sie eine bestimmte Version eines Pakets installieren möchten oder wenn Sie Pakete installieren möchten, bevor Sie die Datei *function.json* bearbeiten, verwenden Sie den Befehl `func extensions install` mit dem Namen des Pakets, wie im folgenden Beispiel gezeigt:

```bash
func extensions install --package Microsoft.Azure.WebJobs.ServiceBus --version <target_version>
```

Ersetzen Sie `<target_version>` durch eine bestimmte Version des Pakets, z.B. `3.0.0-beta5`. Gültige Versionen sind auf den Seiten der einzelnen Pakete auf [NuGet.org](https://nuget.org) aufgeführt.
