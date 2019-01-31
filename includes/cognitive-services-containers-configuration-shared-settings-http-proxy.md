---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/22/2019
ms.openlocfilehash: 98a6eb024e723e0225711adaccf385a2790e5bc8
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55302783"
---
Wenn Sie einen HTTP-Proxy für ausgehende Anforderungen konfigurieren müssen, verwenden Sie diese zwei Argumente:

| NAME | Datentyp | BESCHREIBUNG |
|--|--|--|
|HTTP_PROXY|Zeichenfolge|der zu verwendende Proxy, z.B. http://proxy:8888|
|HTTP_PROXY_CREDS|Zeichenfolge|beliebige Anmeldeinformationen, die zum Authentifizieren des Proxys erforderlich sind, z.B. Benutzername:Kennwort.|

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<billing-endpoint> \
ApiKey=<api-key> \
HTTP_PROXY=http://190.169.1.6:3128 \
HTTP_PROXY_CREDS=jerry:123456 \
Logging:Disk:LogLevel=Debug Logging:Disk:Format=json
```