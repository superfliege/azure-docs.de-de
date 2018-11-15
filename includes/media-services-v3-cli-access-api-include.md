---
title: Includedatei
description: Includedatei
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 11/11/2018
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: 4dde0a47f0452da2dd951df86ccb6e02a44521ed
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51616638"
---
## <a name="access-the-media-services-api"></a>Zugreifen auf die Media Services-API

Verwenden Sie die Authentifizierung per Azure AD-Dienstprinzipal, um eine Verbindung mit den Azure Media Services-APIs herzustellen. Der folgende Befehl erstellt eine Azure AD-Anwendung und fügt einen Dienstprinzipal an das Konto an. Konfigurieren Sie Ihre Anwendung mit den zurückgegebenen Werten.

Vor der Ausführung des Skripts können Sie `amsaccount` und `amsResourceGroup` durch die Namen ersetzen, die Sie beim Erstellen dieser Ressourcen ausgewählt haben. `amsaccount` ist der Name des Azure Media Services-Kontos, an das der Dienstprinzipal angefügt wird.

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
