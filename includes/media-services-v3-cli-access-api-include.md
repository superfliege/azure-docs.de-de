---
title: Includedatei
description: Includedatei
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 05/01/2019
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: b0f93f950b55052ea8d8b31538c47226413dc82a
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66155758"
---
## <a name="access-the-media-services-api"></a>Zugreifen auf die Media Services-API

Verwenden Sie die Authentifizierung per Azure AD-Dienstprinzipal, um eine Verbindung mit den Azure Media Services-APIs herzustellen. Der folgende Befehl erstellt eine Azure AD-Anwendung und fügt einen Dienstprinzipal an das Konto an. Konfigurieren Sie Ihre Anwendung mit den zurückgegebenen Werten.

Ersetzen Sie vor der Ausführung des Skripts `amsaccount` und `amsResourceGroup` durch die Namen, die Sie beim Erstellen dieser Ressourcen ausgewählt haben. `amsaccount` ist der Name des Azure Media Services-Kontos, an das der Dienstprinzipal angefügt wird.

Der folgende Befehl gibt eine `json`-Ausgabe zurück:

```azurecli
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup
```

Dieser Befehl erzeugt eine Antwort wie die folgende:

```json
{
  "AadClientId": "00000000-0000-0000-0000-000000000000",
  "AadEndpoint": "https://login.microsoftonline.com",
  "AadSecret": "00000000-0000-0000-0000-000000000000",
  "AadTenantId": "00000000-0000-0000-0000-000000000000",
  "AccountName": "amsaccount",
  "ArmAadAudience": "https://management.core.windows.net/",
  "ArmEndpoint": "https://management.azure.com/",
  "Region": "West US 2",
  "ResourceGroup": "amsResourceGroup",
  "SubscriptionId": "00000000-0000-0000-0000-000000000000"
}
```

Wenn die Antwort `xml` enthalten soll, verwenden Sie den folgenden Befehl:

```azurecli
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup --xml
```
