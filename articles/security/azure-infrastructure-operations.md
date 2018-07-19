---
title: Produktionsvorgänge und Verwaltung von Azure
description: Dieser Artikel enthält eine allgemeine Beschreibung der Verwaltung und des Betriebs des Azure-Produktionsnetzwerks.
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
ms.openlocfilehash: dc389f5f5c155555deb860f041b15b0ea49ee416
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37101598"
---
# <a name="azure-production-operations-and-management"></a>Produktionsvorgänge und Verwaltung von Azure    
Für Verwaltung und Betrieb des Azure-Produktionsnetzwerks sind die Operations-Teams von Azure und der Azure SQL-Datenbank gemeinsam zuständig. Mehrere System- und Leistungsüberwachungstools werden in der Umgebung verwendet. Netzwerkgeräte, Server, Dienste und Anwendungsprozesse werden mit den entsprechenden Tools überwacht.

Mehrere Ebenen der Überwachung, Protokollierung und Berichterstellung werden implementiert, um die sichere Ausführung der Dienste in der Microsoft Azure-Umgebung zu gewährleisten. Dazu gehören folgende Aktionen:

- Der Microsoft Azure Monitoring Agent (MA) erfasst in erster Linie Überwachungs- und Diagnoseprotokollinformationen aus vielen Quellen, einschließlich FC und Stammbetriebssystem, und schreibt sie in Protokolldateien. Er überträgt schließlich eine Teilmenge der Informationen in Digestform in ein vorkonfiguriertes Azure Storage-Konto. Der Monitoring and Diagnostic Service (MDS) ist ein eigenständiger Dienst, der verschiedene Überwachungs- und Diagnoseprotokolldaten liest und die Informationen zusammenfasst. MDS schreibt die Informationen in ein integriertes Protokoll. Azure verwendet das benutzerdefinierte Azure Security Monitoring (ASM), eine Erweiterung des Azure-Überwachungssystems. Es verfügt über Komponenten, die sicherheitsrelevante Ereignisse an verschiedenen Stellen in der Plattform beobachten, analysieren und melden.
- Die Microsoft Azure SQL-Datenbank WinFabric-Plattform bietet die Verwaltung, Bereitstellung, Entwicklung und Betriebsüberwachungsdienste für die Microsoft Azure SQL-Datenbank. Es bietet verteilte Bereitstellungsdienste mit mehreren Schritten, Überwachung der Integrität, automatische Reparaturen und Serviceversionskonformität. Sie bietet folgende Dienste:

   - Dienstmodellierungsfunktionen mit realitätsnaher Entwicklungsumgebung (Rechenzentrumscluster sind teuer und selten).
   - Bereitstellung per Mausklick und Upgradeworkflows für Dienstbootstrap und Wartung.
   - Integritätsberichterstellung mit automatisiertem Reparaturworkflow zum Aktivieren der Selbstreparatur.
   - Echtzeitüberwachung, Warnungen und Debuggingoptionen für die Knoten eines verteilten Systems.
   - Zentrale Sammlung von operativen Daten und Metriken für die verteilte Analyse der Grundursache und Dienstinformationen.
   - Operative Tools für die Bereitstellung, für das Change Management und für die Überwachung.
   - WinFabric-Plattform und Watchdog-Skripts der Microsoft Azure SQL-Datenbank werden fortlaufend ausgeführt und überwachen in Echtzeit.

Wenn Anomalien auftreten, wird der Prozess zur Reaktion auf Vorfälle gefolgt vom Azure-Incident-Selektierungsteam aktiviert. Die entsprechenden Azure-Supportmitarbeiter werden benachrichtigt, damit sie auf den Vorfall reagieren können. Nachverfolgung und Lösung von Problemen werden in einem zentralen Ticketausstellungssystem dokumentiert und verwaltet. Systemverfügbarkeitsmetriken sind im Rahmen der Vertraulichkeitsvereinbarung und auf Anfrage erhältlich.

## <a name="corporate-network-and-multi-factor-access-to-production"></a>Unternehmensnetzwerk- und Multi-Factor-Zugriff auf die Produktion
Die Benutzerbasis des Unternehmensnetzwerks umfasst Microsoft Azure-Supportmitarbeiter. Das Unternehmensnetzwerk unterstützt interne Unternehmensfunktionen und umfasst den Zugriff auf interne Anwendungen, die für den Azure-Kundensupport verwendet werden. Das Unternehmensnetzwerk ist logisch und physisch vom Azure-Produktionsnetzwerk getrennt. Microsoft Azure-Mitarbeiter greifen mit Microsoft Azure-Arbeitsstationen und -Laptops auf das Unternehmensnetzwerk zu. Alle Benutzer benötigen ein Active Directory- (AD-)Konto, einschließlich Benutzername und Kennwort, um auf die Unternehmensnetzwerkressourcen zuzugreifen. Der Unternehmensnetzwerkzugriff verwendet AD-Konten, die für alle Mitarbeiter, Auftragnehmer, Lieferanten von Microsoft ausgestellt sind und von MSIT verwaltet werden. Eindeutige Benutzer-IDs unterscheiden Mitarbeiter basierend auf deren Beschäftigungsstatus bei Microsoft.

Der Zugriff auf interne Azure-Anwendungen wird über die Authentifizierung mit Active Directory Federation Services (ADFS) gesteuert. ADFS ist ein von MSIT gehosteter Dienst, der die Authentifizierung von Benutzern des Unternehmensnetzwerks über die Anwendung eines sicheren Tokens und von Benutzeransprüchen bereitstellt. ADFS ermöglicht internen Microsoft Azure-Anwendungen, Benutzer anhand der AD-Unternehmensdomäne von Microsoft zu authentifizieren. Um über die Unternehmensumgebung auf das Produktionsnetzwerk zuzugreifen, muss der Benutzer mithilfe der mehrstufigen Authentifizierung authentifizieren.

## <a name="next-steps"></a>Nächste Schritte
Weitere Information dazu, wie Microsoft die Azure-Infrastruktur schützt, finden Sie in den folgenden Artikeln:

- [Azure-Einrichtungen, Gelände und physische Sicherheit](azure-physical-security.md)
- [Verfügbarkeit der Azure-Infrastruktur](azure-infrastructure-availability.md)
- [Komponenten und Grenzen des Azure-Informationssystems](azure-infrastructure-components.md)
- [Azure network architecture (Azure-Netzwerkarchitektur)](azure-infrastructure-network.md)
- [Azure-Produktionsnetzwerk](azure-production-network.md)
- [Sicherheitsfeatures für Microsoft Azure SQL-Datenbank](azure-infrastructure-sql.md)
- [Überwachen der Azure-Infrastruktur](azure-infrastructure-monitoring.md)
- [Integrity of Azure infrastructure (Integrität der Azure-Infrastruktur)](azure-infrastructure-integrity.md)
- [Schutz von Kundendaten in Azure](azure-protection-of-customer-data.md)
