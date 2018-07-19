---
title: Überwachen der Azure-Infrastruktur
description: In diesem Artikel wird die Überwachung des Azure-Produktionsnetzwerks erläutert.
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
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 17e7183ff56725462dc43cba21db418a86d86b51
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37101609"
---
# <a name="monitoring-of-azure-infrastructure"></a>Überwachen der Azure-Infrastruktur   

## <a name="configuration-and-change-management"></a>Konfigurationsverwaltung und Change Management
Microsoft Azure überprüft und aktualisiert daher im Jahresabstand die Konfigurationseinstellungen und Baseline-Konfigurationen von Hardware, Software und Netzwerkgeräten. Änderungen werden vor der Einführung in die Produktionsumgebung in einer Entwicklungs- und/oder Testumgebung entwickelt, getestet und genehmigt.

Die erforderlichen Baseline-Konfigurationen für Azure-basierte Dienste werden vom Azure Security- und Compliance-Team und von Serviceteams überprüft. Die Überprüfung durch ein Serviceteam ist Teil der Tests, die vor der Bereitstellung des Produktionsdiensts durchgeführt werden.

## <a name="vulnerability-management"></a>Verwaltung von Sicherheitsrisiken
Die Verwaltung von Sicherheitsupdates hilft beim Schützen von Systemen vor bekannten Sicherheitsrisiken. Azure verwendet integrierte Bereitstellungssysteme zur Verwaltung der Verteilung und Installation von Sicherheitsaktualisierungen für Microsoft-Software. Azure kann auch auf die Ressourcen des Microsoft Security Response Centers (MSRC) zurückgreifen. Das MSRC identifiziert und überwacht Sicherheitsvorfälle und Sicherheitsrisiken in Clouds rund um die Uhr an jedem Tag des Jahres, reagiert darauf und löst diese Probleme.

## <a name="vulnerability-scanning"></a>Überprüfung auf Sicherheitsrisiken
Die Überprüfung auf Sicherheitsrisiken wird für Serverbetriebssysteme, Datenbanken und Netzwerkgeräte durchgeführt. Die Sicherheitsrisikoüberprüfungen werden mindestens vierteljährlich ausgeführt. Microsoft Azure beauftragt unabhängige Prüfer mit der Durchführung von Penetrationstests für die Bereichsgrenzen von Microsoft Azure. Außerdem werden routinemäßig Red-Team-Übungen durchgeführt und die Ergebnisse zur Verbesserung der Sicherheit genutzt.

## <a name="protective-monitoring"></a>Vorbeugende Überwachung
Microsoft Azure Security weist definierte Anforderungen für die aktive Überwachung auf. Dienstteams konfigurieren die Tools für die aktive Überwachung in Übereinstimmung mit diesen Anforderungen. Zu den aktiven Überwachungstools zählen der Monitoring Agent (MA) und der System Center Operations Manager. Diese Tools werden für die Bereitstellung von Echtzeitwarnungen für Microsoft Azure Security-Personal in Situationen konfiguriert, die ein sofortiges Handeln erfordern.

## <a name="incident-management"></a>Incident Management
Microsoft implementiert einen Prozess zum Verwalten von Sicherheitsvorfällen, um eine koordinierte Reaktion auf Vorfälle zu erleichtern, sollte es zu einem Vorfall kommen.

Wenn Microsoft unberechtigte Zugriffe auf auf seiner Ausrüstung oder in seinen Einrichtungen gespeicherte Kundendaten bekannt wird, oder unberechtigte Zugriffe auf solche Ausrüstungen und Einrichtungen, die zu Verlust, Offenlegung oder Veränderung von Kundendaten führen, ergreift Microsoft folgende Maßnahmen:

- Schnelle Benachrichtigung des Kunden über den Sicherheitsvorfall;
- Schnelle Untersuchung des Sicherheitsvorfalls und Bereitstellung von detaillierten Informationen über den Sicherheitsvorfall für den Kunden;
- Unmittelbares Ergreifen angemessener Schritte zum Verringerung der Auswirkungen und zur Minimierung der aus dem Sicherheitsvorfall resultierenden Schäden.

Es wurde ein Incident-Management-Framework mit definierten Rollen und zugewiesenen Zuständigkeiten eingerichtet. Das WASIM-Team (Azure Security Incident Management) ist für das Verwalten von Sicherheitsvorfällen zuständig, einschließlich Eskalation und der Hinzuziehung von Spezialistenteams, sollte dies erforderlich sein. Azure-Betriebsmanager sind für die Überwachung der Untersuchung und die Behebung von Sicherheits- und Datenschutzvorfällen zuständig.

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie mehr über die Sicherung der Azure-Infrastruktur seitens Microsoft:

- [Azure-Einrichtungen, Gelände und physische Sicherheit](azure-physical-security.md)
- [Verfügbarkeit der Azure-Infrastruktur](azure-infrastructure-availability.md)
- [Komponenten und Grenzen des Azure-Informationssystems](azure-infrastructure-components.md)
- [Azure-Netzwerkarchitektur](azure-infrastructure-network.md)
- [Azure-Produktionsnetzwerk](azure-production-network.md)
- [Sicherheitsfeatures für Microsoft Azure SQL-Datenbank](azure-infrastructure-sql.md)
- [Produktionsvorgänge und Verwaltung von Azure](azure-infrastructure-operations.md)
- [Integrität der Azure-Infrastruktur](azure-infrastructure-integrity.md)
- [Schutz von Kundendaten in Azure](azure-protection-of-customer-data.md)
