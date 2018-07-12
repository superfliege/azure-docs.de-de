---
title: Includedatei
description: Includedatei
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 05/29/2018
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: 7454e96a2a05bf89a0455674a4f144534c374c71
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38733288"
---
## <a name="access-the-media-services-api"></a>Zugreifen auf die Media Services-API

Verwenden Sie die Authentifizierung per Azure AD-Dienstprinzipal, um eine Verbindung mit den Azure Media Services-APIs herzustellen. Der folgende Befehl erstellt eine Azure AD-Anwendung und fügt einen Dienstprinzipal an das Konto an. Konfigurieren Sie Ihre Anwendung mit den zurückgegebenen Werten.

Vor der Ausführung des Skripts können Sie `amsaccount` und `amsResourceGroup` durch die Namen ersetzen, die Sie beim Erstellen dieser Ressourcen ausgewählt haben. `amsaccount` ist der Name des Azure Media Services-Kontos, an das der Dienstprinzipal angefügt wird.

Der folgende Befehl gibt eine `json`-Ausgabe zurück:

```azurecli-interactive
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup
```

Dieser Befehl erzeugt eine Antwort wie die folgende:

```json
{
  "AadClientId": "00000000-4cdd-418a-8a72-0755ace03de5",
  "AadEndpoint": "https://login.microsoftonline.com",
  "AadSecret": "00000000-02f5-4bf2-9057-1c4f7baff155",
  "AadTenantId": "00000000-86f1-41af-91ab-2d7cd011db47",
  "AccountName": "amsaccount22",
  "ArmAadAudience": "https://management.core.windows.net/",
  "ArmEndpoint": "https://management.azure.com/",
  "Region": "West US 2",
  "ResourceGroup": "amsResourceGroup2",
  "SubscriptionId": "00000000-6753-4ca2-b1ae-193798e2c9d8"
}
```

Wenn die Antwort `xml` enthalten soll, verwenden Sie den folgenden Befehl:

```azurecli-interactive
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup --xml
```
