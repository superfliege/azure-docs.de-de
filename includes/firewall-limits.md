---
title: Includedatei
description: Includedatei
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 7/30/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 0a4a66543b323b1dbe56158851dafcb1dfe695f0
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47185037"
---
| Ressource | Standardlimit |
| --- | --- |
| Verarbeitete Datenmenge |1000 TB/Firewall/Monat <sup>1</sup> |
|Regeln|10-k-Anwendungsregeln, 10-k-Netzwerkregeln|
|VNet-Peering|Für Hub-Spoke-Implementierungen, max. 50-Spoke-VNETs|
|Globales Peering|Nicht unterstützt. Sie sollten über mindestens eine Firewallbereitstellung pro Region verfügen.|
|Maximale Anzahl von Ports in einer einzelnen Netzwerkregel|15<br>Ein Portbereich (z. B.: 2 bis 10) wird als zwei gezählt.


<sup>1</sup> Wenden Sie sich an den Azure-Support, falls Sie diese Limits erhöhen müssen.
