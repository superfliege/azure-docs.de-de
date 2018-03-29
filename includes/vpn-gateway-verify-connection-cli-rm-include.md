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
ms.openlocfilehash: 1c3103889bd11df45710cc0d6afaeeba022c9ace
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
Mit dem Befehl [az network vpn-connection show](/cli/azure/network/vpn-connection#show) können Sie überprüfen, ob die Verbindung erfolgreich hergestellt wurde. In dem Beispiel verweist „-Name“ auf den Namen der Verbindung, die Sie testen möchten. Während der Verbindungsherstellung wird als Verbindungsstatus „Connecting“ (Wird verbunden...) angezeigt. Sobald die Verbindung hergestellt wurde, ändert sich der Status in „Connected“ (Verbunden).

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```