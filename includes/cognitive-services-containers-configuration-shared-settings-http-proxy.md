---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2019
ms.openlocfilehash: 2d3d7b37721ca1b19f5d73133352cabdbffe6d68
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58886835"
---
Wenn Sie einen HTTP-Proxy für ausgehende Anforderungen konfigurieren müssen, verwenden Sie diese zwei Argumente:

| NAME | Datentyp | BESCHREIBUNG |
|--|--|--|
|HTTP_PROXY|Zeichenfolge|der zu verwendende Proxy, z.B. `http://proxy:8888`|
|HTTP_PROXY_CREDS|Zeichenfolge|beliebige Anmeldeinformationen, die zum Authentifizieren des Proxys erforderlich sind, z.B. Benutzername:Kennwort.|

```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<billing-endpoint> \
ApiKey=<api-key> \
HTTP_PROXY=http://190.169.1.6:3128 \
HTTP_PROXY_CREDS=jerry:123456 \
```