---
title: Azure SQL-Datenbank Azure Fallstudie - Daxko/CSI | Microsoft Docs
description: Erfahren Sie, wie Daxko/CSI SQL-Datenbank verwendet seine Entwicklungszyklus zu beschleunigen und zu seiner-Kundenservice und die Leistung zu verbessern
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 00c8a713-f20c-4d6b-b8b7-0c1b9ba5f05b
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 3e43494721f03ce8e56cb069638ad24df7222386
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="daxkocsi-used-azure-to-accelerate-its-development-cycle-and-to-enhance-its-customer-services-and-performance"></a>Daxko/CSI verwendet Azure, um seine Entwicklungszyklus zu beschleunigen und seine-Kundenservice und die Leistung zu verbessern
![Daxko/CSI-Logo](./media/sql-database-implementation-daxko/csidaxkologo25.png)

Daxko/CSI Software Datenwachstums eine Herausforderung: Basistext Kunden Eignung und Neuerstellung von Rechenzentren wurde unser Dank gilt den Erfolg der Lösung umfassender Unternehmenssoftware schnell wachsende jedoch halten mit den IT-Infrastruktur-Anforderungen für diesen Kunden wachsenden Basis des Unternehmens testen wurde IT-Mitarbeiter. Das Unternehmen wurde durch steigende Vorgänge Mehraufwand, insbesondere für die Verwaltung ihrer wachsenden Datenbanken zunehmend eingeschränkt. Schlimmer, wurde diese Vorgänge Aufwand in Entwicklungsressourcen für neue Initiativen, wie neue Mobilitätsfunktionen für das Unternehmen Software Ausschneiden.

Entsprechend David Molina, Director der Produktentwicklung am Daxko/CSI, bereitgestellten Azure CSI-Software mit dem Platform-as-a-Service (PaaS)-Modell, das sie zum Vereinfachen der datenbankverwaltung Skalierbarkeit zu erhöhen und Freigeben von Ressourcen auf Software statt Ops konzentrieren erforderlich. "Azure SQL-Datenbank wurde eine sinnvolle Option für uns. Darum nicht kümmern, Verwalten von SQL Server, einem Failovercluster und alles, was andere-Infrastruktur muss Betrug ideal für uns."

Seit der Migration zu Azure benötigt CSI Software eine Betriebspersonal von nur zwei über 600 Kundendatenbanken verwalten. Das Unternehmen verwendet von Azure SQL-Datenbank elastische Pools zum Verschieben von Kundendatenbanken anhand der Größe und benötigen.

Molina weiterhin "unseren Kunden die Änderung sofort sind. Vor dem elastische Pools mussten sie gelegentlich Timeouts sowie zu weiteren Problemen Burst Zeiten. Mit Azure elastische Pools können sie nach Bedarf burst und verwenden Sie die Software ohne Probleme."

Zusätzlich zum Verbessern der Leistung für Kunden, Azure elastische Pools freigegeben CSI-Software-Ressourcen, um auf die Entwicklung neuer Dienste und Funktionen, anstelle von Betrieb und-Verwaltung konzentrieren. Die IT-Ressourcen beigetragen CSI Software verbessern die Enterprise Software angeboten, SpectrumNG, um Hilfe Fitnesscenter Elemente aufzunehmen, Mitarbeiter-Effizienz verbessern und Mitarbeiter und Mitglieder mobilen Zugriff gewähren für interaktive Tasks und Benachrichtigungen in Echtzeit.

Azure wurden auch CSI Software beschleunigen und Optimieren Sie die Entwicklung und qualitätssicherung-(QA)-Zyklus durch Automatisierungsoptionen zu aktivieren. Build-Manager können mit Azure-Implementierung des Unternehmens von Komponenten mit einem Klick auf eine Schaltfläche verpacken. Wie Molina beschrieben, die "im Rahmen des Releasezyklus kann QA nun in einer testumgebung in Azure bereitstellen, die eng unsere Produktion Stapel imitiert. Wir können unsere Umgebung Dev Änderungen eingehend Builds sofort bereitstellen. Ein großer Vorteil für uns hierfür ist es Parität zum Testen, bevor Sie hatten."

## <a name="offloading-to-the-cloud"></a>In die Cloud Auslagern
Vor dem Wechsel in die Cloud, hatte CSI Software eigene mehrinstanzenfähige Infrastruktur in einem lokalen Datencenter in Houston erfolgreich erstellt. Wenn das Unternehmen erweitert, Datenwachstums es zunehmenden wachsende Mühen aus erwerben, Bereitstellung und Verwaltung aller Hardware und Software erforderlich ist, um seinen Kunden zu unterstützen. IT "Personal", um Vorgänge zu behandeln ist eine andere Engpass, die eine Verlangsamung der Bereitstellung neuer Ressourcen und Einführen einer neuen Dienste für Kunden geführt hat.

CSI Software gesucht in Cloud-Optionen für die Eliminierung dieser Aufwand, damit es auf ihren Code, anstatt seine Vorgänge untersuchen kann. Das Unternehmen wurde festgestellt, dass viele der Top-Cloud-Anbieter nur Infrastructure-as-a-Service (IaaS)-Lösungen bieten, die noch eine große IT-Mitarbeiter die IaaS-Stapel verwalten müssen. Letztlich bestimmt CSI Software an, dass die Azure-PaaS-Lösung für ihre Anforderungen am besten passenden wurde. Molina erläutert, dass "Azure aus dem Weg, die Hardware und Software ruft wir für unsere Softwareangebote unserer IT Auslastung konzentrieren können."

## <a name="making-the-transition-to-azure"></a>Der Wechsel in Azure
Nach dem Auswählen von Azure für die PaaS-Lösung, begann CSI-Software die Back-End-Infrastruktur und die Datenbanken in die Cloud migrieren. Vor Azure für die Installation von einer Clientanwendung, die kommuniziert mit einem Windows Communication Foundation (WCF)-Dienst auf dem Back-End erforderlichen SpectrumNG Kunden. Entsprechend Molina erstellt", obwohl einige Kunden alles, was in ihren eigenen Rechenzentren gehostet wir das Produkt mehrinstanzenfähigen sein. Wir gehostet alles, was in einem Datencenter in Houston, SQL Server als Datenspeicher verwenden.

"Unser Produkt auch Angebot enthalten ein Member verbundene Webportal (in ASP.net geschrieben), das entwickelt wurde, um entsprechend der Kunde Web Vorhandensein und einer SOAP-API zur Unterstützung der online-Seiten und keine Integration eines Drittanbieters weiß beschriftet werden."

Die Migration in die Cloud nicht lange dauerte für die Architektur. Entsprechend Molina "Den Großteil der Aufwand, ändern die Möglichkeit, dass wir Lesen von Informationen der Config-Datei, eine Änderung zentrale Verbindungszeichenfolge und Automatisieren der Verpackung, hochladen und Bereitstellung von unseren Versionen behandelt."

Zum Entwickeln von der Build-Automatisierungs verwendet die CSI Softwareentwickler Azure PowerShell und REST-APIs zum Erstellen von Paketen und das Hochladen in einer Stagingumgebung für jede Nacht-Version.
Der gesamte Übergang zu einer Azure-Cloud-basierte Bereitstellung ging schnell und reibungslos für CSI Software IT-Team. Molina erläutert, dass "In allen, wir eine betaumgebung von in der Cloud innerhalb von drei bis vier Wochen dauert für das Projekt hatte. Das war uns überraschenden leistungsmäßig."

Nach dem Konfigurieren und testen die Umgebung, CSI Software begann Migrieren von Kunden. Kunden, die bereits über CSI-Software zu hosten wurde der Übergang nahezu nahtlosen. Für Kunden, die Migration von einer lokalen Bereitstellung, die Migration in die Cloud hat einige zusätzliche Zeit, war jedoch weiterhin in erster Linie für Kunden und CSI Software Problembereiche freizugeben.

Für neuer Kunden CSI-Software des IT-Mitarbeiter dem folgenden Prozess zum integrierten verwenden, um Azure:

1. Azure PowerShell-Skripts werden verwendet, um eine neue Datenbank für den Kunden zu starten. alle Kunden fangen auf eine Premium-Ebene, um die anfängliche durchsatzleistung für den Übergang zu gewährleisten.
2. Wenn möglich, mithilfe von CSI-Software der Azure SQL-Datenbankmigrations-Assistent um vorhandene Daten in einer Azure SQL-Datenbankinstanz zu verschieben.
3. Microsoft SQL Server Integration Services (SSIS) dienen schließlich zum Abstimmen von Diskrepanzen in den Daten oder alle Daten Cleanup nach Bedarf auszuführen.

Heute sind etwa 99 Prozent der Kunden CSI-Software in Azure, über vier regionale Rechenzentren (Norden / Mitte, Süden / Mitte, Osten und Westen) gehostet. Durch die Verwendung von Rechenzentren in geografischen Region des Kunden, ist Latenz auf ein Minimum reduziert.

## <a name="azure-elastic-pools-free-up-it-resources"></a>Azure elastische Pools Geben Sie IT-Ressourcen frei.
Mehrere Features von Azure haben CSI Software geholfen UMSCHALT nicht von Infrastruktur und Vorgängen, die sich der Fokus auf die Funktion und die Entwicklung konzentrieren. Möglicherweise wurde der größte Vorteil von elastische Pools.

CSI Software bietet derzeit ca. 550 Datenbanken für Kunden. Vor dem elastische Pools war es schwierig, dass viele Datenbanken innerhalb einer Ebene-Struktur zu verwalten. Operations Manager musste weisen Leistung Ebenen basierend auf den Bedürfnissen Burst Kunden, die bedeutende IT-Ressourcen Mehraufwand erforderlich. Mit elastische Pools können Manager Mandanten zuzuweisen, eine Premium- oder standard-Pool nach Bedarf, und klicken Sie dann Verschieben von Kunden auf Grundlage der Größe und benötigen. Kunden sind die Auswirkungen des elastischen Pools fast sofort; Bevor Sie elastische Pools Kunden sind Timeouts und andere Probleme aufgetreten Burst-Zeiten, jedoch mit elastische Pools Kunden Aktivität Anstieg Bedarf auftreten können und können weiterhin SpectrumNG problemlos verwendet.

## <a name="azure-active-geo-replication-accelerates-reporting"></a>Aktiver geografische Replikation beschleunigt reporting
Mehrere CSI-Software-Kunden sind auch Azure aktive geografische Replikation ausnutzen. Mit der aktiven georeplikation können bis zu vier lesbare sekundäre Datenbanken in der gleichen oder einem anderen Datencenter-Regionen konfiguriert werden. CSI Software aktive geografische Replikation auf zwei Arten verwenden: zuerst die sekundären Datenbanken bei einem Ausfall des Datencenters oder die Unfähigkeit zur Verbindung mit der primären Datenbank verfügbar sind und Zweitens, die sekundären Datenbanken lesbar sind, und können verwendet werden, um schreibgeschützte arbeitsauslastungen wie z. B. die berichterstellung von Aufträgen auslagern. Einige Kunden CSI-Software verwenden diesen Vorteil, um reporting Workflows zu beschleunigen.

## <a name="csi-software-application-logic-and-architecture"></a>Anwendungslogik CSI-Software und Architektur
SpectrumNG verwendet Webrollen. Da die Anwendung mit mehreren Mandanten ist, wird ein WCF-Dienst zur Verarbeitung der Anforderung anfangsverbindung von Kunden verwendet. Wie in Molina angegeben, identifiziert"die Anforderung pro Kunden auf, können Sie uns eine Verbindungszeichenfolge, zu tun, was müssen wir Ihre jeweiligen Datenbanken erstellen."

Für die Webebene seine nutzt CSI Software Azure automatische Skalierung, basierend auf den Tag und die Uhrzeit. Verfügbare Ressourcen werden automatisch erhöht, um höhere Auslastung während der Geschäftszeiten zu berücksichtigen gemäß der Zeitzone der einzelnen regionalen Rechenzentrum. Ressourcen sind für Wochenenden Herunterskalieren, wenn die Anforderungen des Kunden niedriger sind ebenfalls festgelegt.

![Daxko/CSI-Architektur](./media/sql-database-implementation-daxko/figure1.png)

Abbildung 1. Eine Cloud Services-Worker-Rolle zeichnet strukturierte Daten aus Azure SQL-Datenbank und die teilweise strukturierten Daten aus dem Tabellenspeicher. SpectrumNG Benutzer interagieren mit, dass Daten über einen Cloud--Rolle "Web Services".

## <a name="using-web-apps-and-a-web-plan-tier-for-mobile-apps"></a>Mithilfe von Web-apps und eine webplan Ebene für mobile apps
Mithilfe von Azure SQL-Datenbank reserviert werden, um Ressourcen für CSI-Software auf neue Initiativen, z. B. eine vollständige mobile-Plattform aktivieren basierend auf einer benutzerdefinierten API in Azure-Web-apps gehostet. Die Plattform ermöglicht Fitnesscenter Member und Mitarbeiter auf mobile Geräten überprüfen Sie Zeitpläne, book Klassen und Empfangen von Nachrichten verwenden.

Die Plattform dienstorientierten Architektur (SOA) verwendet, um eine einzelne Komponente zu nutzen, wie z. B. POS-Systeme (POS) oder einem Vertriebssystem – verschieben Sie sie bei laufendem Betrieb an einen anderen webplan aus, und klicken Sie dann auf einen Dienst für diese Komponente unterstützt wird, lassen Sie alle anderen auf dem ursprünglichen webplan bündelt. Dank dieser Fähigkeit ermöglicht enorme Flexibilität CSI-Software, und es hilft Ihnen, Ihre Kosten nach unten.

## <a name="azure-lets-csi-software-developers-focus-on-apps-and-services"></a>Azure ermöglicht CSI-Software-Entwickler den Fokus auf apps und Dienste
Azure SQL-Datenbank nicht einfach eine gesteigert werden SpectrumNG-Kunden, die genießen Sie den Dienst schnellen, zuverlässigen, es ist auch ein großer Vorteil für CSI-Software, IT-Mitarbeiter und Entwickler. Durch Auslagern Ops in Azure in der Cloud, CSI-Software reduziert der Aufwand für die Ressourcen und Infrastruktur, erheblich beschleunigt die Entwicklungszyklen und nicht mehr auf Anforderungen Micromanage Datenbanken zum Optimieren der Leistung für ihre Mandanten.

## <a name="more-information"></a>Weitere Informationen
* Weitere Informationen zu Azure elastische Pools finden Sie unter [elastische Pools](sql-database-elastic-pool.md).
* Weitere Informationen zu Datenbanktools und flexible Skalierung finden Sie unter [elastische Datenbanktools und flexible Skalierung](sql-database-elastic-scale-get-started.md).
* Weitere Informationen zum Migrieren einer SQL Server-Datenbank finden Sie unter finden Sie unter [Migrieren einer SQL Server-Datenbank in Azure](sql-database-cloud-migrate.md).
* Weitere Informationen zu aktiven geografischen Replikation finden Sie unter [aktive geografische Replikation](sql-database-geo-replication-overview.md).
* Weitere Informationen zu Webrollen und Workerrollen finden Sie unter [Workerrollen](../fundamentals-introduction-to-azure.md#compute).    
* Weitere Informationen zu Azure Service Bus finden Sie unter [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).
* Um weitere Informationen zur automatischen Skalierung finden Sie unter [Skalieren von Cloud Services](../cloud-services/cloud-services-how-to-scale.md).

