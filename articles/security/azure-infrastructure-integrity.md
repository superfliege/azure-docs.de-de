---
title: Integrität der Azure-Infrastruktur
description: Dieser Artikel behandelt die Integrität der Azure-Infrastruktur.
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
ms.openlocfilehash: 867bc66a68bec662153d8336e649cf46df02f101
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901315"
---
# <a name="azure-infrastructure-integrity"></a>Integrität der Azure-Infrastruktur

## <a name="software-installation"></a>Softwareinstallation
Alle Komponenten im Softwarestack, die in der Azure-Umgebung installiert sind, werden gemäß dem SDL-Prozess (Security Development Lifecycle) von Microsoft speziell erstellt. Alle Softwarekomponenten (einschließlich Betriebssystem-Images und SQL-Datenbank) werden im Rahmen des Prozesses für Change Management und Release Management bereitgestellt. Bei dem Betriebssystem, das auf allen Knoten ausgeführt wird, handelt es sich um eine angepasste Version von Windows Server 2008 oder Windows Server 2012. Die genaue Version wird vom FC entsprechend der Rolle ausgewählt, die das Betriebssystem übernehmen soll. Darüber hinaus gestattet das Hostbetriebssystem keine Installation von nicht autorisierten Softwarekomponenten.

Einige Microsoft Azure-Komponenten (z.B. RDFE, Entwicklerportal usw.) werden als Azure-Kunden auf einer Gast-VM unter einem Gastbetriebssystem bereitgestellt.

## <a name="virus-scans-on-builds"></a>Virenscans auf Builds
Builds von Azure-Softwarekomponenten (einschließlich Betriebssystem) müssen einen Virenscan mit dem Virenschutztool Microsoft Endpoint Protection (MEP) durchlaufen. Bei jedem Virenscan wird ein Protokoll im zugehörigen Buildverzeichnis erstellt, in dem die gescannten Komponenten und die Ergebnisse des Scans detailliert aufgeführt sind. Der Virenscan ist Teil des Buildquellcodes für jede Komponente in Azure. Code wird ohne einen sauberen und erfolgreichen Virensan nicht in die Produktionsumgebung übernommen. Wenn Probleme erkannt werden, wird der Build eingefroren und dann an die Sicherheitsteams bei Microsoft Security weitergeleitet, um festzustellen, wo der „gefährliche“ Code in den Build gelangt ist.

## <a name="closedlocked-environment"></a>Geschlossene/gesperrte Umgebung
Standardmäßig werden auf Knoten und Gast-VMs in der Azure-Infrastruktur keine Benutzerkonten erstellt. Zudem sind auch standardmäßige Windows-Administratorkonten deaktiviert. Administratoren des Microsoft Azure Live Support (WALS) können sich – bei ordnungsgemäßer Authentifizierung – an diesen Computern anmelden und das Azure-Produktionsnetzwerk für Notfallreparaturen verwalten.

## <a name="microsoft-azure-sql-database-authentication"></a>Microsoft Azure SQL-Datenbankauthentifizierung
Wie bei jeder Implementierung von SQL Server muss die Verwaltung von Benutzerkonten streng kontrolliert werden. Microsoft Azure SQL-Datenbank unterstützt nur SQL Server-Authentifizierung. Als Ergänzung zum Datensicherheitsmodell des Kunden sollten zudem Benutzerkonten verwendet werden, die sichere Kennwörter haben und mit spezifischen Rechten konfiguriert sind.

## <a name="firewallacls-between-msft-corpnet-and-microsoft-azure-cluster"></a>Firewall/ACLs zwischen MSFT-Unternehmensnetzwerk und Microsoft Azure-Cluster
ACLs bzw. eine Firewall zwischen der Dienstplattform und dem MS-Unternehmensnetzwerk schützen die Microsoft Azure SQL-Datenbank vor nicht autorisiertem Insiderzugriff. Darüber hinaus können nur Benutzer von IP-Adressbereichen des Microsoft-Unternehmensnetzwerks aus auf den Verwaltungsendpunkt der WinFabric-Plattform zugreifen.

## <a name="firewallacls-between-nodes-in-an-azure-sql-db-cluster"></a>Firewall/ACLs zwischen Knoten in einem Azure SQL-Datenbankcluster
Als zusätzlicher Schutz im Rahmen der Strategie für tiefgreifende Verteidigung wurden ACLs bzw. eine Firewall zwischen Knoten in einem Microsoft Azure SQL-Datenbankcluster implementiert. Die gesamte Kommunikation innerhalb des WinFabric-Plattformclusters sowie der gesamte ausgeführte Code sind vertrauenswürdig.

## <a name="custom-mas-watchdogs"></a>Benutzerdefinierte Verwaltungs-Agents (Watchdogs)
Microsoft Azure SQL-Datenbank setzt benutzerdefinierte Verwaltungs-Agents (Watchdogs genannt) ein, um die Integrität des Microsoft Azure SQL-Datenbankclusters zu überwachen.

## <a name="web-protocols"></a>Webprotokolle

### <a name="role-instance-monitoring-and-restart"></a>Überwachung und Neustart von Rolleninstanzen
Azure stellt sicher, dass für alle ausgeführten Rollen (Webrolle mit Internetzugriff oder Workerrolle für Back-End-Verarbeitung) eine dauerhafte Integritätsüberwachung erfolgt, um zu gewährleisten, dass diese auf effektive und effiziente Weise die Dienste liefern, in denen sie bereitgestellt wurden. Falls die Integrität einer Rolle entweder durch einen kritischen Fehler in der gehosteten Anwendung oder aufgrund eines Konfigurationsproblems in der Rolleninstanz selbst nicht mehr gewährleistet ist, erkennt Microsoft Azure FC das Problem in der Rolleninstanz und initiiert einen Korrekturstatus.

### <a name="compute-connectivity"></a>Compute-Konnektivität
Azure stellt sicher, dass die bereitgestellte Anwendung bzw. der bereitgestellte Dienst über webbasierte Standardprotokolle erreichbar ist. Virtuelle Instanzen der Webrolle mit Internetzugriff verfügen über externe Internetkonnektivität und sind für Webbenutzer direkt erreichbar. Virtuelle Instanzen der Workerrolle für die Back-End-Verarbeitung verfügen über externe Internetkonnektivität, doch kann ein externer Webbenutzer nicht direkt darauf zugreifen, um die Vertraulichkeit und Integrität der Vorgänge zu schützen, die von Workerrollen für die öffentlich zugänglichen virtuellen Instanzen der Webrolle ausgeführt werden.

## <a name="next-steps"></a>Nächste Schritte
Weitere Information dazu, wie Microsoft die Azure-Infrastruktur schützt, finden Sie in den folgenden Artikeln:

- [Azure-Einrichtungen, Gelände und physische Sicherheit](azure-physical-security.md)
- [Verfügbarkeit der Azure-Infrastruktur](azure-infrastructure-availability.md)
- [Komponenten und Grenzen des Azure-Informationssystems](azure-infrastructure-components.md)
- [Azure-Netzwerkarchitektur](azure-infrastructure-network.md)
- [Azure-Produktionsnetzwerk](azure-production-network.md)
- [Sicherheitsfeatures für Microsoft Azure SQL-Datenbank](azure-infrastructure-sql.md)
- [Produktionsvorgänge und Verwaltung von Azure](azure-infrastructure-operations.md)
- [Überwachen der Azure-Infrastruktur](azure-infrastructure-monitoring.md)
- [Schutz von Kundendaten in Azure](azure-protection-of-customer-data.md)
