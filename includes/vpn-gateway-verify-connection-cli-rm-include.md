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
ms.openlocfilehash: 0e009354e66ab13cdb9fbc3cf9e4b37e904bdfd1
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56246164"
---
Mit dem Befehl [az network vpn-connection show](/cli/azure/network/vpn-connection) können Sie überprüfen, ob die Verbindung erfolgreich hergestellt wurde. In dem Beispiel verweist „-Name“ auf den Namen der Verbindung, die Sie testen möchten. Während der Verbindungsherstellung wird als Verbindungsstatus „Connecting“ (Wird verbunden...) angezeigt. Sobald die Verbindung hergestellt wurde, ändert sich der Status in „Connected“ (Verbunden).

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```
