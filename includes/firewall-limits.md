---
title: Includedatei
description: Includedatei
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 3/25/2019
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 0467359cd9d6a067e519a62532f00459bc5f68cb
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59804128"
---
| Ressource | Standardlimit |
| --- | --- |
| Datendurchsatz |30 GBit/s<sup>1</sup> |
|Regeln|10.000, alle Regeltypen kombiniert|
|Mindestgröße für AzureFirewallSubnet |/26|
|Portbereich in Netzwerk- und Anwendungsregeln|0-64.000. Wir arbeiten daran, diese Einschränkung zu lockern.|
|Routingtabelle|Standardmäßig verfügt AzureFirewallSubnet über die Route „0.0.0.0/0“, bei der der Wert „NextHopType“ auf **Internet** festgelegt ist.<br><br>Wenn Sie die Tunnelerzwingung für Verbindungen mit dem lokalen Netzwerk über ExpressRoute oder VPN Gateway aktiviert haben, müssen Sie explizit eine benutzerdefinierte Route für 0.0.0.0/0 konfigurieren, für die der Wert NextHopType auf „Internet“ festgelegt ist, und diese dem AzureFirewallSubnet zuweisen. Dadurch wird eine mögliche BGP-Anzeige für das Standardgateway in Ihrem lokalen Netzwerk außer Kraft gesetzt. Wenn Ihr Unternehmen erzwungenes Tunneling für Azure Firewall erfordert, um den standardmäßigen Gatewaydatenverkehr über Ihr lokales Netzwerk zurückzuleiten, wenden Sie sich an den Support. Wir können Ihr Abonnement in die Whitelist aufnehmen, um sicherzustellen, dass die erforderliche Internetkonnektivität der Firewall erhalten bleibt.|

<sup>1</sup> Wenden Sie sich an den Azure-Support, falls Sie diese Grenzwerte erhöhen müssen.
