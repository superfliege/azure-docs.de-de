---
title: Azure PowerShell-Beispiele für virtuelle Netzwerke | Microsoft-Dokumentation
description: Azure PowerShell-Beispiele für virtuelle Netzwerke.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: c9cb8da2a6a7512daa6721af90d5b21c6ba5e8e5
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="azure-powershell-samples-for-virtual-network"></a>Azure PowerShell-Beispiele für virtuelle Netzwerke

Die folgende Tabelle enthält Links zu Azure PowerShell-Skripts:

| | |
|----|----|
| [Erstellen eines virtuellen Netzwerks für Anwendungen mit mehreren Ebenen](./scripts/virtual-network-powershell-sample-multi-tier-application.md) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen. Der Datenverkehr am Front-End-Subnetz ist auf HTTP beschränkt, während der Datenverkehr zum Back-End-Subnetz auf SQL und Port 1433 beschränkt ist. |
| [Herstellen einer Peerverbindung zwischen zwei virtuellen Netzwerken](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md) | Erstellt zwei virtuelle Netzwerke in derselben Region und stellt eine Verbindung zwischen diesen her. |
| [Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen und einer VM, die Datenverkehr zwischen den zwei Subnetzen weiterleiten kann. |
| [Filtern von ein- und ausgehenden VM-Netzwerkdatenverkehr](./scripts/virtual-network-powershell-sample-filter-network-traffic.md) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen. Der im Front-End-Subnetz eingehende Netzwerkdatenverkehr ist auf HTTP und HTTPS beschränkt. Aus dem Back-End-Subnetz an das Internet ausgehender Datenverkehr ist nicht zulässig. |
