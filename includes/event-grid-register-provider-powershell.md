---
title: Includedatei
description: Includedatei
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 07/05/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 0946b703b45f69ede409167213f2629a796f18b5
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37869626"
---
## <a name="enable-event-grid-resource-provider"></a>Aktivieren des Event Grid-Ressourcenanbieters

Wenn Sie zuvor in Ihrem Azure-Abonnement noch nicht Event Grid verwendet haben, sollten Sie den Event Grid-Ressourcenanbieter registrieren. Führen Sie den folgenden Befehl aus:

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.EventGrid
```

Es kann einen Moment dauern, bis die Registrierung abgeschlossen ist. Führen Sie Folgendes aus, um den Status zu überprüfen:

```azurepowershell-interactive
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.EventGrid
```

Wenn `RegistrationStatus` `Registered` ist, können Sie den Vorgang fortsetzen.