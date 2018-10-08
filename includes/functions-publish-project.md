---
title: Includedatei
description: Includedatei
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/27/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 1b553cbd720fcb76899844712ce5053af46f7ccb
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452954"
---
## <a name="deploy-the-function-app-project-to-azure"></a>Bereitstellen des Funktions-App-Projekts in Azure

Nach dem Erstellen der Funktions-App in Azure können Sie den Befehl [`func azure functionapp publish`](../articles/azure-functions/functions-run-local.md#project-file-deployment) verwenden, um Ihren Projektcode in Azure bereitzustellen.

```bash
func azure functionapp publish <FunctionAppName>
```

Sie sehen eine Ausgabe ähnlich der folgenden, die zur besseren Lesbarkeit beschnitten wurde.

```output
Getting site publishing info...

...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
```

Sie können jetzt Ihre Funktionen in Azure testen.