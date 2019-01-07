---
title: Includedatei
description: Includedatei
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 12/14/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 7d905550114bb76a0a091146b3972bab4a652022
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53429971"
---
| Ressource | Standardlimit |
| --- | --- |
| Verarbeitete Datenmenge |1000 TB/Firewall/Monat <sup>1</sup> |
|Regeln|10k – Kombination aller Regeltypen|
|Globales Peering|Nicht unterstützt. Sie sollten über mindestens eine Firewallbereitstellung pro Region verfügen.|
|Maximale Anzahl von Ports in einer einzelnen Netzwerkregel|15<br>Ein Portbereich (z.B.: 2 bis 10) wird als zwei gezählt.
|Mindestgröße für AzureFirewallSubnet |/26|
|Portbereich in Netzwerk- und Anwendungsregeln|0-64.000. Wir arbeiten daran, diese Einschränkung zu lockern.|
|


<sup>1</sup> Wenden Sie sich an den Azure-Support, falls Sie diese Limits erhöhen müssen.
