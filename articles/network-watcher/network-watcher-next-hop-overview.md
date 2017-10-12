---
title: "Einführung in Nächster Hop in Azure Network Watcher | Microsoft Docs"
description: "Diese Seite enthält eine Übersicht über die Network Watcher-Funktion „Nächster Hop“."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: bb2ca0486b3b3d27a77b70927cb3cbfbeac12c7c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-next-hop-in-azure-network-watcher"></a>Einführung in Nächster Hop in Azure Network Watcher

Der Datenverkehr von einem virtuellen Computer an ein Ziel wird basierend auf den gültigen Routen, die einer Netzwerkkarte zugeordnet sind, gesendet. Nächster Hop ruft den Typ und die IP-Adresse des nächsten Hops eines Pakets von einem angegebenen virtuellen Computer und der Netzwerkkarte ab. Dadurch wird ermittelt, ob das Paket an das Ziel gesendet oder ob der Datenverkehr blockiert wird. Eine falsche Konfiguration von Routen durch den Benutzer, bei der Datenverkehr an einem lokalen Standort oder ein virtuelles Gerät geleitet wird, kann zu Verbindungsproblemen führen. Nächster Hop gibt auch die Routentabelle, die dem nächsten Hop zugeordnet ist, zurück. Wenn vom nächsten Hop abgefragt wird, ob die Route als eine benutzerdefinierte Route definiert ist, wird diese Route zurückgegeben. Andernfalls gibt Nächster Hop die „Systemroute“ zurück.

![Übersicht über Nächster Hop][1]

Im Folgenden finden Sie eine Liste der Typen von nächsten Hops, die beim Abfragen des nächsten Hops zurückgegeben werden können.

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* Keine

### <a name="next-steps"></a>Nächste Schritte

Erfahren Sie unter [Überprüfen des nächsten Hops auf einem virtuellen Computer](network-watcher-check-next-hop-portal.md), wie Sie Nächster Hop verwenden, um nach Problemen mit der Netzwerkkonnektivität zu suchen.

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png













