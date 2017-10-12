---
title: "Einführung in die Topologie in Azure Network Watcher | Microsoft-Dokumentation"
description: "Diese Seite enthält eine Übersicht über die Network Watcher-Topologiefunktionen."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: e753a435-38e0-482b-846b-121cb547555c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: d5cb5ba431eeae1956a9dbf1d48561c66faef9a6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-topology-in-azure-network-watcher"></a>Einführung in die Topologie in Azure Network Watcher

Die Topologie gibt ein Diagramm der Netzwerkressourcen in einem virtuellen Netzwerk zurück. Das Diagramm zeigt die Verbindung zwischen den Ressourcen, um die End-to-End-Netzwerkkonnektivität darzustellen.

![Übersicht der Topologie][1]

Im Portal gibt die Topologie die Ressourcenobjekte pro virtuelles Netzwerk zurück. Die Beziehungen werden durch Linien zwischen den Ressourcen dargestellt. Außerhalb der Network Watcher-Region liegende Ressourcen werden auch dann nicht dargestellt, wenn sie zur Ressourcengruppe gehören. Die in der Portalansicht zurückgegebenen Ressourcen sind eine Teilmenge der Netzwerkkomponenten, die grafisch dargestellt werden. Um die vollständige Liste der Netzwerkressourcen anzuzeigen, können Sie [PowerShell](network-watcher-topology-powershell.md) oder [REST](network-watcher-topology-rest.md) verwenden.

> [!NOTE]
> In jeder Region, in der die Topologie ausgeführt werden soll, ist eine Network Watcher-Instanz erforderlich.

Wenn die Ressourcen zurückgegeben werden, werden die Verbindungen zwischen ihnen unter zwei Beziehungen modelliert.

- **Einschlussbeziehung** – Beispiel: Virtuelles Netzwerk enthält ein Subnetz mit einer NIC.
- **Zuordnungsbeziehung** – Beispiel: Eine NIC ist einem virtuellen Computer zugeordnet.

### <a name="next-steps"></a>Nächste Schritte

Erfahren Sie unter [Anzeigen der Network Watcher-Topologie mit PowerShell](network-watcher-topology-powershell.md), wie Sie PowerShell verwenden, um die Topologieansicht abzurufen.

<!--Image references-->

[1]: ./media/network-watcher-topology-overview/topology.png
