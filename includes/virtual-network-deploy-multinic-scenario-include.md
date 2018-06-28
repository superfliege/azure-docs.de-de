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
ms.openlocfilehash: cab04a7eafbc21e0d26cd5a287f3dbee8d3d22b7
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "31805099"
---
## <a name="scenario"></a>Szenario
In diesem Dokument wird eine Bereitstellung ausführlich erläutert, in der in einem bestimmten Szenario mehrere NICs in virtuellen Computern verwendet werden. In diesem Szenario wird eine IaaS-Workload mit zwei Ebenen in Azure gehostet. Jede Ebene wird in einem eigenen Subnetz in einem virtuellen Netzwerk (VNet) bereitgestellt. Die Front-End-Ebene besteht aus mehreren Webservern, die in einem Lastenausgleich für Hochverfügbarkeit gruppiert werden. Die Back-End-Ebene besteht aus mehreren Datenbankservern. Die Datenbankserver werden mit jeweils zwei Netzwerkkarten bereitgestellt: einer Karte für den Datenbankzugriff und einer anderen für die Verwaltung. Das Szenario umfasst auch Netzwerk-Sicherheitsgruppen (NSGs), die steuern, welcher Datenverkehr für jedes Subnetz und NIC in der Bereitstellung zulässig ist. In der folgenden Abbildung ist die grundlegende Architektur dieses Szenarios dargestellt:

![Multi-NIC-Szenario](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

