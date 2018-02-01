---
title: "Übersicht über Verfügbarkeitszonen | Microsoft-Dokumentation"
description: "Dieser Artikel enthält eine Übersicht über Verfügbarkeitszonen in Azure."
services: 
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
tags: 
ms.assetid: 
ms.service: azure
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2017
ms.author: markgal
ms.custom: mvc I am an ITPro and application developer, and I want to protect (use Availability Zones) my applications and data against data center failure (to build Highly Available applications).
ms.openlocfilehash: a0e654637bc4aca4230c56cc7c1706f5cd73622e
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="overview-of-availability-zones-in-azure-preview"></a>Übersicht über Verfügbarkeitszonen in Azure (Vorschauversion)

Verfügbarkeitszonen tragen zum Schutz vor Fehlern auf Datencenterebene bei. Sie befinden sich in einer Azure-Region und verfügen jeweils über eine eigene unabhängige Stromversorgung, Kühlung und Netzwerkumgebung. Zur Gewährleistung der Resilienz sind in allen aktivierten Regionen mindestens drei separate Zonen vorhanden. Die physische und logische Trennung von Verfügbarkeitszonen innerhalb einer Region schützt Anwendungen und Daten vor Fehlern auf Zonenebene. 

![Konzeptionelle Ansicht einer Zone, die in einer Region ausfällt](./media/az-overview/az-graphic-two.png)

## <a name="regions-that-support-availability-zones"></a>Regionen, die Verfügbarkeitszonen unterstützen

- USA (Ost) 2
- USA, Mitte
- Europa, Westen
- Frankreich, Mitte

## <a name="services-that-support-availability-zones"></a>Dienste, die Verfügbarkeitszonen unterstützen

Die folgenden Azure Dienste unterstützen Verfügbarkeitszonen:

- Virtuelle Linux-Computer
- Virtuelle Windows-Computer
- Skalierungsgruppen für virtuelle Computer
- Managed Disks
- Lastenausgleichsmodul
- Öffentliche IP-Adresse
- Zonenredundanter Speicher

## <a name="get-started-with-the-availability-zones-preview"></a>Erste Schritte mit der Vorschauversion für Verfügbarkeitszonen

Die Vorschauversion für Verfügbarkeitszonen ist in den Regionen „USA, Osten 2“, „USA, Mitte“, „Europa, Westen“ und „Frankreich, Mitte“ für bestimmte Azure-Dienste verfügbar. 

1. [Registrieren Sie sich für die Vorschauversion für Verfügbarkeitszonen](http://aka.ms/azenroll). 
2. Melden Sie sich bei Ihrem Azure-Abonnement an.
3. Wählen Sie eine Region, die Verfügbarkeitszonen unterstützt.
4. Verwenden Sie einen der folgenden Links, um Verfügbarkeitszonen mit Ihrem Dienst zu verwenden. 
    - [Erstellen eines virtuellen Computers](../virtual-machines/windows/create-portal-availability-zone.md)
    - [Erstellen einer VM-Skalierungsgruppe](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
    - [Anfügen eines Datenträgers an einen virtuellen Windows-Computer mithilfe von PowerShell](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
    - [Load Balancer](../load-balancer/load-balancer-standard-overview.md)

## <a name="next-steps"></a>Nächste Schritte
- [Schnellstartvorlagen](http://aka.ms/azqs)
