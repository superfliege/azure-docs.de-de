---
title: Verfügbarkeit der Azure-Infrastruktur
description: Der Artikel beschreibt Redundanzebenen zur Gewährleistung maximaler Verfügbarkeit von Kundendaten.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2018
ms.author: terrylan
ms.openlocfilehash: 18c6b87c9926b93eec26cca4028a38e472912c46
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2018
ms.locfileid: "37902576"
---
# <a name="azure-infrastructure-availability"></a>Verfügbarkeit der Azure-Infrastruktur
Azure bietet zuverlässige Verfügbarkeit auf Grundlage umfassender Redundanz mithilfe von Virtualisierungstechnologie. Azure bietet zahlreiche Redundanzebenen zur Gewährleistung maximaler Verfügbarkeit von Kundendaten.

## <a name="temporary-outages-and-natural-disaster"></a>Temporäre Ausfälle und Naturkatastrophen
Das Microsoft Cloud Infrastructure and Operations-Team entwirft, erstellt, betreibt und schützt die Cloudinfrastruktur. Dieses Team stellt für die Azure-Infrastruktur Hochverfügbarkeit und Zuverlässigkeit, hohe Effizienz, intelligente Skalierbarkeit und eine sichere, private und vertrauenswürdige Cloud sicher.

Unterbrechungsfreie Stromversorgungen und riesige Batteriebänke gewährleisten eine fortgesetzte Energieversorgung bei kurzfristigen Stromausfällen.

Notstromaggregate sorgen bei längeren Ausfallzeiten und geplanter Wartung für Reservestrom. Das Rechenzentrum wird im Falle einer Naturkatastrophe mit vor Ort befindlichen Brennstoffreserven betrieben.

Stabile Glasfasernetze für hohe Geschwindigkeit verbinden die Rechenzentren mit anderen wichtigen Hubs und Internetbenutzern. Computeknoten hosten Workloads näher am Endbenutzer, um die Latenz zu verringern, Georedundanz bereitzustellen und die Resilienz von Diensten insgesamt zu steigern. Ein Technikerteam arbeitet rund um die Uhr, um sicherzustellen, dass die Dienste den Kunden ständig zur Verfügung stehen.

Microsoft gewährleistet Hochverfügbarkeit durch erweiterte Überwachung und Reaktion auf Vorfälle, Dienstunterstützung sowie Sicherungs- und Failoverfunktion. Geografisch verteilte Microsoft-Betriebszentren sind 24 Stunden am Tag, 7 Tage die Woche und 365 Tage im Jahr in Betrieb. Das Azure-Netzwerk ist eines der größten der Welt. Das Glasfasernetz für die Inhaltsverteilung verbindet Rechenzentren und Edge-Knoten, um hohe Leistung und Zuverlässigkeit sicherzustellen.

## <a name="disaster-recovery"></a>Notfallwiederherstellung
Azure bewahrt Kundendaten dauerhaft an zwei Standorten auf, wobei der Kunde den Sicherungsstandort auswählen kann. An beiden Standorten verwaltet Azure ständig mehrere (3) fehlerfreie Replikate der Kundendaten.

## <a name="database-availability"></a>Datenbankverfügbarkeit
Azure stellt über ein Internetgateway mit anhaltender Datenbankverfügbarkeit sicher, dass über das Internet auf eine Datenbank zugegriffen werden kann. Bei der Überwachung werden Integrität und Status der aktiven Datenbanken in 5-Minuten-Intervallen ausgewertet.

## <a name="storage-availability"></a>Speicherverfügbarkeit
Azure bietet Speicherung über einen hochgradig skalierbaren und dauerhaften Speicherdienst, der Konnektivitätsendpunkte bietet, sodass von einer verarbeitenden Anwendung direkt darauf zugegriffen werden kann. Über den Speicherdienst werden eingehende Speicheranforderungen effizient mit Transaktionsintegrität verarbeitet.

## <a name="next-steps"></a>Nächste Schritte
Weitere Information dazu, wie Microsoft die Azure-Infrastruktur schützt, finden Sie in den folgenden Artikeln:

- [Azure-Einrichtungen, Gelände und physische Sicherheit](azure-physical-security.md)
- [Komponenten und Grenzen des Azure-Informationssystems](azure-infrastructure-components.md)
- [Azure-Netzwerkarchitektur](azure-infrastructure-network.md)
- [Azure-Produktionsnetzwerk](azure-production-network.md)
- [Sicherheitsfeatures für Microsoft Azure SQL-Datenbank](azure-infrastructure-sql.md)
- [Produktionsvorgänge und Verwaltung von Azure](azure-infrastructure-operations.md)
- [Überwachen der Azure-Infrastruktur](azure-infrastructure-monitoring.md)
- [Integrität der Azure-Infrastruktur](azure-infrastructure-integrity.md)
- [Schutz von Kundendaten in Azure](azure-protection-of-customer-data.md)
