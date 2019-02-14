---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e368b1590f263618969423d57cdf0531fc2bb54d
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56246160"
---
### <a name="to-modify-the-local-network-gateway-gatewayipaddress"></a>So ändern Sie die Angabe für „gatewayIpAddress“ für ein Gateway des lokalen Netzwerks

Wenn für das VPN-Gerät, mit dem Sie eine Verbindung herstellen möchten, die öffentliche IP-Adresse geändert wurde, müssen Sie das Gateway des lokalen Netzwerks entsprechend anpassen. Die Gateway-IP-Adresse kann geändert werden, ohne dass eine vorhandene VPN-Gatewayverbindung (sofern vorhanden) entfernt werden muss. Ersetzen Sie zum Ändern der Gateway-IP-Adresse die Werte „Site2“ und „TestRG1“ durch eigene Werte. Verwenden Sie dazu den Befehl [az network local-gateway update](https://docs.microsoft.com/cli/azure/network/local-gateway).

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 --name Site2 --resource-group TestRG1
```

Überprüfen Sie, ob die IP-Adresse in der Ausgabe korrekt ist:

```
"gatewayIpAddress": "23.99.222.170",
```
