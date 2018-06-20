---
title: Übersicht über den relationalen Datenbankdienst Azure Database for PostgreSQL
description: Bietet eine Übersicht über den relationalen Datenbankdienst Azure-Datenbank für PostgreSQL.
services: postgresql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 06/02/2018
ms.openlocfilehash: fab3aa7d0b1fe0f4fafd4b2fdca24ad0e8a3d03c
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34737230"
---
# <a name="what-is-azure-database-for-postgresql"></a>Was ist Azure-Datenbank für PostgreSQL?

Azure Database for PostgreSQL ist ein relationaler, für Entwickler konzipierter Datenbankdienst in Microsoft Cloud, der auf der Communityversion des Open-Source-Datenbankmoduls [PostgreSQL](https://www.postgresql.org/) basiert. Azure-Datenbank für PostgreSQL bietet Folgendes:

- Integrierte Hochverfügbarkeit ohne zusätzliche Kosten
- Vorhersagbare Leistung inklusive nutzungsbasierter Bezahlung
- Bedarfsgerechte Skalierung in Sekundenschnelle
- Schutz von sensiblen Daten in Ruhe und während der Übertragung
- Automatische Sicherungen und Point-in-Time-Wiederherstellung für bis zu 35 Tage
- Sicherheit und Konformität auf Unternehmensniveau

All diese Funktionen erfordern fast keine Administration und werden alle ohne zusätzliche Kosten bereitgestellt. Mit diesen Funktionen können Sie sich auf die schnelle Entwicklung von Anwendungen und die Verkürzung des Zeitraums bis zur Markteinführung konzentrieren, anstatt wertvolle Zeit und Ressourcen für die Verwaltung von virtuellen Computern und der Infrastruktur aufwenden zu müssen. Darüber hinaus können Sie Ihre Anwendung weiterhin mit den Open-Source-Tools und der Plattform Ihrer Wahl entwickeln und mit der Geschwindigkeit und Effizienz liefern, die Ihr Unternehmen erfordert, ohne sich neue Fähigkeiten aneignen zu müssen. 

Dieser Artikel bietet eine Einführung in die wesentlichen Konzepte und Features von Azure-Datenbank für PostgreSQL im Hinblick auf Leistung, Skalierbarkeit und Verwaltbarkeit. Diese Schnellstarts erleichtern Ihnen den Einstieg:

- [Erstellen einer Azure-Datenbank für PostgreSQL mithilfe des Azure-Portals](quickstart-create-server-database-portal.md)
- [Erstellen einer Azure-Datenbank für PostgreSQL mithilfe der Azure-CLI](quickstart-create-server-database-azure-cli.md)

Eine Reihe von Azure CLI-Beispielen finden Sie unter:

- [Azure CLI-Beispiele für Azure-Datenbank für PostgreSQL](./sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Anpassen von Leistung und Skalierung in nur wenigen Sekunden
Für den Azure Database for PostgreSQL-Dienst sind drei Tarife verfügbar: „Basic“, „Universell“ und „Arbeitsspeicheroptimiert“. Jeder Tarif bietet unterschiedliche Ressourcenfunktionen für Ihre Datenbankworkloads. Sie können Ihre erste App mit einer kleinen Datenbank für wenige USD im Monat erstellen und die Skalierung dann so ändern, dass sie den Anforderungen Ihrer Lösung entspricht. Durch die dynamische Skalierung kann Ihre Datenbank transparent auf schnell wechselnde Ressourcenanforderungen reagieren. Sie bezahlen nur für die benötigten Ressourcen und auch nur dann, wenn Sie sie verwenden. Einzelheiten hierzu finden Sie unter [Tarife](concepts-pricing-tiers.md).

## <a name="monitoring-and-alerting"></a>Überwachung und Warnung
Wie entscheiden Sie, wann Sie hoch- oder herunterskalieren? Mit den integrierten Überwachungs- und Warnungsfeatures von Azure. Mit diesen Tools können Sie abhängig von Ihrem aktuellen oder voraussichtlichen Leistungs- oder Speicherbedarf blitzschnell die Auswirkungen des zentralen Hoch- oder Herunterskalierens bewerten. Einzelheiten hierzu finden Sie unter [Warnungen](howto-alert-on-metric.md).

## <a name="keep-your-app-and-business-running"></a>Aufrechterhalten von App-Ausführung und Geschäftskontinuität
Durch die Unterstützung eines globalen Netzwerks von Microsoft-verwalteten Rechenzentren stellt die branchenführende Verfügbarkeit von Azure mit einer Vereinbarung zum Servicelevel (SLA) von 99,99 % sicher, dass Ihre Apps rund um die Uhr ausgeführt werden. Mit jedem Azure-Datenbank für PostgreSQL-Server nutzen Sie die integrierte Sicherheit, die Fehlertoleranz und die Datensicherheit, die Sie andernfalls entwerfen oder kaufen, erstellen und verwalten müssten. In Azure Database for PostgreSQL bietet jeder Tarif einen umfassenden Satz von Features für Geschäftskontinuität sowie Optionen, die sicherstellen, dass alles reibungslos funktioniert. Über die [Point-in-Time-Wiederherstellung](howto-restore-server-portal.md) können Sie den früheren Status einer Datenbank für bis zu 35 Tage wiederherstellen. Ferner können Sie bei einem Ausfall des Rechenzentrums, in dem Ihre Datenbanken gehostet werden, Ihre Datenbanken aus georedundanten Kopien kürzlich durchgeführter Sicherungen wiederherstellen.

## <a name="secure-your-data"></a>Sichern der Daten
Azure-Datenbank für PostgreSQL führt die Tradition der Datensicherheit von Azure-Datenbankdiensten fort – mit Features zum Einschränken des Zugriffs, zum Schützen ruhender und bewegter Daten und zum Überwachen von Aktivitäten. Informationen zur Sicherheit der Azure-Plattform finden Sie im [Azure Trust Center](https://www.microsoft.com/en-us/trustcenter/security).

Die Azure-Datenbank für PostgreSQL-Dienst verwendet Speicherverschlüsselung für ruhende Daten. Daten (einschließlich Sicherungen) werden auf dem Datenträger verschlüsselt – mit Ausnahme von temporären Dateien, die im Rahmen von Abfragen erstellt werden. Der Dienst verwendet das in der Azure-Speicherverschlüsselung inbegriffene AES-256-Bit-Verschlüsselungsverfahren und die Schlüssel werden vom System verwaltet. Die Speicherverschlüsselung ist immer aktiviert und kann nicht deaktiviert werden.

Standardmäßig ist der Azure-Datenbank für PostgreSQL-Dienst so konfiguriert, dass [SSL-Verbindungssicherheit](./concepts-ssl-connection-security.md) für Daten während Übertragungen innerhalb des Netzwerks erforderlich ist. Das Erzwingen von SSL-Verbindungen zwischen dem Datenbankserver und Clientanwendungen trägt zum Schutz vor Man-in-the-Middle-Angriffen bei, indem der Datenstrom zwischen dem Server und der Anwendung verschlüsselt wird. Wenn Ihre Clientanwendung keine SSL-Konnektivität unterstützt, können Sie optional die Erzwingung von SSL zum Herstellen einer Verbindung mit dem Datenbankdienst deaktivieren.

## <a name="contacts"></a>Kontakte
Sollten Sie Fragen oder Vorschläge im Zusammenhang mit der Verwendung von Azure Database for MySQL haben, senden Sie eine E-Mail an das zuständige Team ([@Ask Azure DB for PostgreSQL](mailto:AskAzureDBforPostgreSQL@service.microsoft.com)). Beachten Sie, dass es sich hierbei nicht um einen Alias für den technischen Support handelt.

Weitere Kontaktmöglichkeiten:
- Wenn Sie den Azure-Support kontaktieren möchten, [fordern Sie im Azure-Portal ein Ticket an](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Um ein Problem mit Ihrem Konto zu beheben, richten Sie im Azure-Portal eine [Anfrage an den Support](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
- Wenn Sie Feedback abgeben oder Vorschläge für neue Features einreichen möchten, erstellen Sie einen Eintrag über [UserVoice](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).

## <a name="next-steps"></a>Nächste Schritte
- Auf der Seite mit der [Preisgestaltung](https://azure.microsoft.com/pricing/details/postgresql/) finden Sie Kostenvergleiche und Rechner.
- Beginnen Sie mit dem [Erstellen einer ersten Azure-Datenbank für PostgreSQL](./quickstart-create-server-database-portal.md).
- Erstellen Ihrer ersten App in Python, PHP, Ruby, C\#, Java oder Node.js: [Verbindungsbibliotheken](./concepts-connection-libraries.md)
