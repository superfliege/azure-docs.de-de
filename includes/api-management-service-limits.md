---
title: Includedatei
description: Includedatei
services: api-management
author: vladvino
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: api-management
ms.topic: include
ms.date: 03/22/2018
ms.author: vlvinogr
ms.custom: include file
ms.openlocfilehash: bee289da3f18edd0cb425f3d9acde084567a3b13
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
| Ressource | Begrenzung |
| --- | --- |
| Skalierungseinheiten | Zehn pro Region<sup>1</sup> |
| Cache | 5 GB pro Einheit<sup>1</sup> |
| Gleichzeitige Back-End-Verbindungen<sup>2</sup> pro HTTP-Autorität | 2.048 pro Einheit<sup>3</sup> |
| Maximale Größe zwischengespeicherter Antworten | 10 MB |
| Maximale Anzahl benutzerdefinierter Gatewaydomänen | 20 pro Dienstinstanz<sup>4</sup> |


<sup>1</sup>API Management-Beschränkungen sind für jeden Tarif unterschiedlich. Die Tarife und die dazugehörigen Grenzwerte für die Skalierung finden Sie unter [API Management – Preise](https://azure.microsoft.com/pricing/details/api-management/).
<sup>2</sup> Verbindungen werden in Pools zusammengefasst und wiederverwendet, sofern sie nicht explizit durch das Back-End geschlossen wurden.
<sup>3</sup> Pro Einheit der Tarife „Basic“, „Standard“ und „Premium“. Der Developer-Tarif ist auf 1.024 beschränkt.
<sup>4</sup> Nur im Premium-Tarif verfügbar.


