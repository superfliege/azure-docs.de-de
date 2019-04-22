---
title: Was sind Azure-Verfügbarkeitszonen? | Microsoft-Dokumentation
description: Zur Erstellung hoch verfügbarer und robuster Anwendungen in Azure bieten Verfügbarkeitszonen physisch voneinander getrennte Orte für die Ressourcenausführung.
services: ''
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/02/2019
ms.author: cynthn
ms.custom: mvc I am an ITPro and application developer, and I want to protect (use Availability Zones) my applications and data against data center failure (to build Highly Available applications).
ms.openlocfilehash: 557757fc4d99fe57ad545e9d2eebcce61ddb3a8f
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59268720"
---
# <a name="what-are-availability-zones-in-azure"></a>Was sind Verfügbarkeitszonen in Azure?
Verfügbarkeitszonen sind ein Hochverfügbarkeitsangebot, das Anwendungen und Daten vor Ausfällen von Rechenzentren schützt. Verfügbarkeitszonen sind eindeutige physische Standorte in einer Azure-Region. Jede Zone besteht aus mindestens einem Rechenzentrum, dessen Stromversorgung, Kühlung und Netzwerkbetrieb unabhängig funktionieren. Zur Gewährleistung der Resilienz sind in allen aktivierten Regionen mindestens drei separate Zonen vorhanden. Die physische Trennung von Verfügbarkeitszonen innerhalb einer Region schützt Anwendungen und Daten vor Ausfällen von Rechenzentren. Zonenredundante Dienste replizieren Ihre Anwendungen und Daten zum Schutz vor einzelnen Fehlerquellen über Verfügbarkeitszonen hinweg. Mit Verfügbarkeitszonen bietet Azure die branchenweit beste Betriebszeit-SLA von 99,99 % für VMs. Die vollständige [Azure-SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) erläutert die garantierte Verfügbarkeit von Azure insgesamt.

Eine Verfügbarkeitszone in einer Azure-Region ist eine Kombination aus einer Fehlerdomäne und einer Updatedomäne. Wenn Sie z.B. drei oder mehr virtuelle Computer über drei Zonen verteilt in einer Azure-Region erstellen, werden Ihre virtuellen Computer effektiv auf drei Fehlerdomänen und drei Updatedomänen verteilt. Die Azure-Plattform erkennt diese Updatedomänen übergreifende Verteilung, um sicherzustellen, dass virtuelle Computer in unterschiedlichen Zonen nicht gleichzeitig aktualisiert werden.

Integrieren Sie hohe Verfügbarkeit in Ihre Anwendungsarchitektur, indem Sie Ihre Compute-, Speicher-, Netzwerk- und Datenressourcen in eine Zone aufnehmen und in anderen Zonen replizieren. Azure-Dienste, die Verfügbarkeitszonen unterstützen, können in zwei Kategorien unterteilt werden:

- **Zonale Dienste**: Sie binden die Ressource in eine bestimmte Zone ein (z.B. virtuelle Computer, verwaltete Datenträger, IP-Adressen).
- **Zonenredundante Dienste**: Die Plattform repliziert automatisch zonenübergreifend (z.B. zonenredundanter Speicher, SQL-Datenbank).

Um eine umfassende Geschäftskontinuität in Azure zu erreichen, erstellen Sie die Anwendungsarchitektur über die Kombination von Verfügbarkeitszonen mit Azure-Regionspaaren. Sie können Anwendungen und Daten mithilfe von Verfügbarkeitszonen für hohe Verfügbarkeit innerhalb einer Azure-Region synchron replizieren und für Notfallwiederherstellungen über Azure-Regionen asynchron replizieren.
 
![Konzeptionelle Ansicht einer Zone, die in einer Region ausfällt](./media/az-overview/az-graphic-two.png)

## <a name="regions-that-support-availability-zones"></a>Regionen, die Verfügbarkeitszonen unterstützen

- USA (Mitte)
- USA (Ost)
- USA (Ost) 2
- Frankreich, Mitte
- Nordeuropa
- Asien, Südosten 
- Vereinigtes Königreich, Süden &#42;
- Europa, Westen
- USA, Westen 2



## <a name="services-that-support-availability-zones"></a>Dienste, die Verfügbarkeitszonen unterstützen
Die folgenden Azure Dienste unterstützen Verfügbarkeitszonen:

- Virtuelle Linux-Computer
- Virtuelle Windows-Computer
- Virtual Machine Scale Sets
- Managed Disks
- Load Balancer Standard &#42;
- Öffentliche Standard-IP-Adresse &#42;
- Zonenredundanter Speicher
- SQL-Datenbank
- Event Hubs
- Service Bus (nur Premium-Tarif)
- VPN Gateway
- ExpressRoute
- Application Gateway (Vorschau)

&#42; Ressourcen, die im Vereinigten Königreich, Süden vor dem 25. März 2019 erstellt wurden, werden bald in zonenredundante Ressourcen konvertiert. Ressourcen, die nach dem 25. März 2019 erstellt werden, sind sofort zonenredundant.

## <a name="services-resiliency"></a>Resilienz von Diensten
Alle Azure-Verwaltungsdienste sind so konstruiert, dass Sie gegenüber regionsweiten Ausfällen resilient sind. Im Spektrum möglicher Ausfälle besitzen ein oder mehrere Verfügbarkeitszonenausfälle innerhalb einer Region einen geringeren Ausfallradius als der Ausfall einer ganzen Region. Azure kann von einem zonenweiten Ausfall von Verwaltungsdiensten innerhalb der Region oder aus einer anderen Azure-Region wiederhergestellt werden. Azure führt wichtige Wartungsaufgaben immer zonenweise innerhalb einer Region durch, um Ausfälle zu verhindern, die sich auf Kundenressourcen auswirken, die über Verfügbarkeitszonen innerhalb einer Region hinweg bereitgestellt sind.

## <a name="pricing"></a>Preise
Es fallen keine zusätzlichen Kosten für virtuelle Computer an, die in einer Verfügbarkeitszone bereitgestellt werden. Die Betriebszeit-SLA von 99,99% für VMs wird angeboten, wenn mindestens zwei virtuelle Computer in zwei oder mehr Verfügbarkeitszonen innerhalb einer Azure-Region bereitgestellt werden. Es fallen zusätzliche Gebühren für Datenübertragungen zwischen VMs in unterschiedlichen Verfügbarkeitszonen an. Weitere Informationen finden Sie auf der Seite [Bandbreite – Preisdetails](https://azure.microsoft.com/pricing/details/bandwidth/).


## <a name="get-started-with-availability-zones"></a>Erste Schritte mit Verfügbarkeitszonen
- [Erstellen eines virtuellen Computers](../virtual-machines/windows/create-portal-availability-zone.md)
- [Hinzufügen eines verwalteten Datenträgers mithilfe von PowerShell](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
- [Erstellen einer zonenredundanten VM-Skalierungsgruppe](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
- [Lastenausgleich für VMs über alle Verfügbarkeitszonen hinweg mithilfe von Load Balancer Standard mit einem zonenredundanten Front-End](../load-balancer/load-balancer-standard-public-zone-redundant-cli.md)
- [Lastenausgleich für VMs innerhalb einer Zone mithilfe von Load Balancer Standard mit einem Zonen-Front-End](../load-balancer/load-balancer-standard-public-zonal-cli.md)
- [Zonenredundanter Speicher](../storage/common/storage-redundancy-zrs.md)
- [SQL-Datenbank](../sql-database/sql-database-high-availability.md#zone-redundant-configuration)
- [Georedundante Notfallwiederherstellung in Event Hubs](../event-hubs/event-hubs-geo-dr.md#availability-zones)
- [Georedundante Notfallwiederherstellung in Service Bus](../service-bus-messaging/service-bus-geo-dr.md#availability-zones)
- [Erstellen eines zonenredundanten Gateways für virtuelle Netzwerke](../vpn-gateway/create-zone-redundant-vnet-gateway.md)


## <a name="next-steps"></a>Nächste Schritte
- [Schnellstartvorlagen](https://aka.ms/azqs)
