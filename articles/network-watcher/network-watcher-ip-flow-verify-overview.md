---
title: Einführung in die IP-Datenflussüberprüfung in Azure Network Watcher | Microsoft Docs
description: Diese Seite enthält eine Übersicht über die Network Watcher-Funktion zur IP-Datenflussüberprüfung.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: d352fb2d-4b4f-4ac4-9c2e-1cfccf0e7e03
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 8a59047a586f3d7ad7c1f29b218396bd688caafd
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="introduction-to-ip-flow-verify-in-azure-network-watcher"></a>Einführung in die IP-Datenflussüberprüfung in Azure Network Watcher

Die IP-Datenflussüberprüfung überprüft, ob ein Paket an einen oder von einem virtuellen Computer übermittelt werden darf. Diese Informationen enthalten die Richtung, das Protokoll, die lokale IP-Adresse, die IP-Remoteadresse, den lokalen Port und den Remoteport. Wenn das Paket von einer Sicherheitsgruppe abgelehnt wird, wird der Name der Regel, die das Paket verweigert hat, zurückgegeben. Es können beliebige Quell- oder Ziel-IP-Adressen ausgewählt werden. Damit unterstützt die IP-Datenflussüberprüfung Administratoren bei der schnellen Diagnose von Verbindungsproblemen mit dem Internet und in der lokalen Umgebung.

Die IP-Datenflussüberprüfung nutzt als Ziel eine Netzwerkschnittstelle eines virtuellen Computers. Der Datenfluss zu oder von dieser Netzwerkschnittstelle wird dann basierend auf den konfigurierten Einstellungen überprüft. Die IP-Datenflussüberprüfung ist nützlich, um zu bestätigen, ob eine Regel in einer Netzwerksicherheitsgruppe ein- oder ausgehenden Datenverkehr eines virtuellen Computers blockiert.

Es muss in jeder Region, in der Sie die IP-Datenflussüberprüfung ausführen möchten, eine Instanz von Network Watcher erstellt werden. Network Watcher ist ein regionaler Dienst, der nur für Ressourcen in derselben Region ausgeführt werden kann. Die verwendete Instanz besitzt jedoch keine Auswirkungen auf die Ergebnisse der IP-Datenflussüberprüfung, da trotzdem die Route zurückgegeben wird, die der Netzwerkschnittstelle zugeordnet ist.

![1][1]

## <a name="next-steps"></a>Nächste Schritte

Im folgenden Artikel erfahren Sie, wie Sie im Portal ermitteln, ob ein Paket für einen bestimmten virtuellen Computer zugelassen oder verweigert wird. [Überprüfen mit der IP-Datenflussüberprüfung im Portal, ob Datenverkehr auf einem virtuellen Computer zugelassen wird](diagnose-vm-network-traffic-filtering-problem.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png












