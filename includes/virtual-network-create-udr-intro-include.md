---
title: Includedatei
description: Includedatei
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 482241deb1081ac8a5265a076eabbdc3fb6d659e
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170852"
---
Obwohl durch die Verwendung von Systemrouten der Datenverkehr für Ihre Bereitstellung automatisch ermöglicht wird, gibt es Fälle, in denen Sie das Routing von Paketen über ein virtuelles Gerät steuern möchten. Hierzu können Sie benutzerdefinierte Routen erstellen, die den nächsten Hop für Pakete in ein bestimmtes Subnetz angeben, und festlegen, dass die Pakete stattdessen an Ihre virtuelle Anwendung gesendet werden sollen. Außerdem können Sie die IP-Weiterleitung für den virtuellen Computer aktivieren, der als virtuelles Gerät ausgeführt wird.

Beispiele für Fälle, in denen virtuelle Geräte verwendet werden können:

* Überwachen des Netzwerkverkehrs mit einem Intrusion Detection System (IDS)
* Steuerung des Datenverkehrs mit einer Firewall

Weitere Informationen zu UDR und IP-Weiterleitung finden Sie unter [Benutzerdefinierte Routen und IP-Weiterleitung](../articles/virtual-network/virtual-networks-udr-overview.md).

