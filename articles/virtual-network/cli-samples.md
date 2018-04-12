---
title: Azure CLI-Beispiele für virtuelle Netzwerke | Microsoft-Dokumentation
description: Azure CLI-Beispiele für virtuelle Netzwerke.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: baa4dbc8f95e068eb1a939fdee53fb2a4ee8117f
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
# <a name="azure-cli-samples-for-virtual-network"></a>Azure CLI-Beispiele für virtuelle Netzwerke

Die folgende Tabelle enthält Links zu Bashskripts mit Azure CLI-Befehlen:

| | |
|----|----|
| [Erstellen eines virtuellen Netzwerks für Anwendungen mit mehreren Ebenen](./scripts/virtual-network-cli-sample-multi-tier-application.md) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen. Der Datenverkehr an das Front-End-Subnetz ist auf HTTP und SSH beschränkt, während der Datenverkehr an das Back-End-Subnetz auf MySQL Port 3306 beschränkt ist. |
| [Herstellen einer Peerverbindung zwischen zwei virtuellen Netzwerken](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md) | Erstellt zwei virtuelle Netzwerke in derselben Region und stellt eine Verbindung zwischen diesen her. |
| [Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen und einer VM, die Datenverkehr zwischen den zwei Subnetzen weiterleiten kann. |
| [Filtern von ein- und ausgehenden VM-Netzwerkdatenverkehr](./scripts/virtual-network-cli-sample-filter-network-traffic.md) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen. Der im Front-End-Subnetz eingehende Netzwerkdatenverkehr ist auf HTTP, HTTPS und SSH beschränkt. Aus dem Back-End-Subnetz an das Internet ausgehender Datenverkehr ist nicht zulässig. |