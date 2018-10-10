---
title: Übersicht über den relationalen Datenbankdienst Azure Database for MariaDB
description: Übersicht über den relationalen Datenbankdienst Azure Database for MariaDB
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: overview
ms.date: 09/24/2018
ms.custom: mvc
ms.openlocfilehash: 1ba4a9bd597473b71ac7561c41b81783b899766a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971602"
---
# <a name="what-is-azure-database-for-mariadb"></a>Was ist Azure Database for MariaDB?
Azure Database for MariaDB ist ein relationaler Datenbankdienst in der Microsoft-Cloud, der auf der Datenbank-Engine [MariaDB Community Edition](https://mariadb.org/download/) basiert. Dieser Dienst befindet sich in der öffentlichen Vorschau. Azure Database for MariaDB bietet Folgendes:

- Integrierte Hochverfügbarkeit ohne zusätzliche Kosten.
- Vorhersagbare Leistung inklusive nutzungsbasierter Bezahlung.
- Bedarfsgerechte Skalierung in Sekundenschnelle
- Gesicherte Datenbank zum Schutz sensibler Daten in Ruhe und Bewegung.
- Automatische Sicherungen und Point-in-Time-Wiederherstellung für bis zu 35 Tage.
- Sicherheit und Konformität auf Unternehmensniveau.

Diese Funktionen erfordern fast keine Administration und werden alle ohne zusätzliche Kosten bereitgestellt. Mit diesen Funktionen können Sie sich auf die schnelle Entwicklung von Apps und die Verkürzung des Zeitraums bis zur Markteinführung konzentrieren, anstatt wertvolle Zeit und Ressourcen für die Verwaltung von virtuellen Computern und der Infrastruktur aufwenden zu müssen. Darüber hinaus können Sie Ihre Anwendung weiterhin mit den Open-Source-Tools und der Plattform Ihrer Wahl entwickeln und mit der Geschwindigkeit und Effizienz liefern, die Ihr Unternehmen verlangt, ohne dass Sie sich dafür neue Fähigkeiten aneignen müssen.

Dieser Artikel bietet eine Einführung in die wesentlichen Konzepte und Features von Azure Database for MariaDB im Hinblick auf Leistung, Skalierbarkeit und Verwaltbarkeit sowie Links zu detaillierten Informationen. Diese Schnellstarts erleichtern Ihnen den Einstieg:
- [Erstellen eines Servers für Azure Database for MariaDB mithilfe des Azure-Portals](quickstart-create-mariadb-server-database-using-azure-portal.md)
- [Erstellen eines Servers für Azure Database for MariaDB mithilfe der Azure-Befehlszeilenschnittstelle](quickstart-create-mariadb-server-database-using-azure-cli.md)

<!--
For a set of Azure CLI samples, see:
- [Azure CLI samples for Azure Database for MariaDB](sample-scripts-azure-cli.md) 
-->

## <a name="adjust-performance-and-scale-within-seconds"></a>Anpassen von Leistung und Skalierung in nur wenigen Sekunden
Für den Azure Database for MariaDB-Dienst sind in der Vorschauversion verschiedene Dienstebenen verfügbar: „Basic“, „Universell“ und „Arbeitsspeicheroptimiert“. Jeder Tarif bietet verschiedene Leistungsstufen und Funktionen zur Unterstützung geringer und hoher Datenbankworkloads. Sie können Ihre erste App mit einer kleinen Datenbank für wenige USD im Monat erstellen und die Skalierung dann so ändern, dass sie den Anforderungen Ihrer Lösung entspricht. Durch die dynamische Skalierung kann Ihre Datenbank transparent auf schnell wechselnde Ressourcenanforderungen reagieren. Sie bezahlen nur für die benötigten Ressourcen und auch nur dann, wenn Sie sie verwenden. Einzelheiten hierzu finden Sie unter [Tarife](concepts-pricing-tiers.md).

## <a name="monitoring-and-alerting"></a>Überwachung und Warnung
Wie entscheiden Sie, wann Sie nach oben oder nach unten skalieren? Verwenden Sie die integrierten Features für Leistungsüberwachung und Warnungen in Kombination mit den Leistungsbewertungen nach virtuellen Kernen. Mit diesen Tools können Sie die Auswirkungen des zentralen Hoch- oder Herunterskalierens von virtuellen Kernen je nach Ihren derzeitigen oder erwarteten Leistungsanforderungen schnell bewerten. <!--See [Alerts](howto-alert-on-metric.md) for details.-->

## <a name="keep-your-app-and-business-running"></a>Aufrechterhalten von App-Ausführung und Geschäftskontinuität
Durch die Unterstützung eines globalen Netzwerks von durch Microsoft verwaltete Rechenzentren stellt die branchenführende Verfügbarkeit von Azure mit einer Vereinbarung zum Servicelevel (SLA) von 99,99 % (während der öffentlichen Vorschauphase nicht angeboten) sicher, dass Ihre Apps rund um die Uhr ausgeführt werden. Mit jedem Azure Database for MariaDB-Server nutzen Sie die integrierte Sicherheit, die Fehlertoleranz und die Datensicherheit, die Sie andernfalls entwerfen oder kaufen, erstellen und verwalten müssten. Mit Azure Database for MariaDB können Sie über die Point-in-Time-Wiederherstellung den früheren Status eines Servers innerhalb der letzten 35 Tage wiederherstellen.

## <a name="secure-your-data"></a>Sichern der Daten
Azure Database for MariaDB führt die Tradition der Datensicherheit von Azure-Datenbankdiensten fort – mit Features zum Einschränken des Zugriffs, zum Schützen ruhender und bewegter Daten und zum Überwachen von Aktivitäten. Weitere Informationen zur Sicherheit der Azure-Plattform finden Sie im [Azure Trust Center](https://www.microsoft.com/en-us/trustcenter/security).

Der Azure Database for MariaDB-Dienst verwendet Speicherverschlüsselung für ruhende Daten. Die Daten, einschließlich der Sicherungen, werden auf dem Datenträger verschlüsselt (mit Ausnahme von temporären Dateien, die von der Engine während der Ausführung von Abfragen erstellt werden). Der Dienst verwendet das in der Azure-Speicherverschlüsselung inbegriffene AES-256-Bit-Verschlüsselungsverfahren und die Schlüssel werden vom System verwaltet. Die Speicherverschlüsselung ist immer aktiviert und kann nicht deaktiviert werden.

In der Standardeinstellung ist der Azure Database for MariaDB-Dienst so konfiguriert, dass SSL-Verbindungssicherheit <!--[SSL connection security](./concepts-ssl-connection-security.md)--> für Daten bei Übertragungen innerhalb des Netzwerks erforderlich ist. Das Erzwingen von SSL-Verbindungen zwischen dem Datenbankserver und Clientanwendungen trägt zum Schutz vor Man-in-the-Middle-Angriffen bei, indem der Datenstrom zwischen dem Server und der Anwendung verschlüsselt wird. Wenn Ihre Clientanwendung keine SSL-Konnektivität unterstützt, können Sie optional die Erzwingung von SSL zum Herstellen einer Verbindung mit dem Datenbankdienst deaktivieren.

## <a name="contacts"></a>Kontakte
Sollten Sie Fragen oder Vorschläge im Zusammenhang mit der Verwendung von Azure Database for MariaDB haben, senden Sie eine E-Mail an das zuständige Team ([@Ask Azure DB for MariaDB](mailto:AskAzureDBforMariaDB@service.microsoft.com)). Beachten Sie, dass es sich hierbei nicht um einen Alias für den technischen Support handelt.

Weitere Kontaktmöglichkeiten:
- Wenn Sie den Azure-Support kontaktieren möchten, [fordern Sie im Azure-Portal ein Ticket an](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Um ein Problem mit Ihrem Konto zu beheben, richten Sie im Azure-Portal eine [Anfrage an den Support](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
- Wenn Sie Feedback abgeben oder Vorschläge für neue Features einreichen möchten, erstellen Sie einen Eintrag über [UserVoice](https://feedback.azure.com/forums/915439-azure-database-for-mariadb).

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun eine Einführung in Azure Database for MariaDB erhalten haben, in der die Frage „Was ist Azure Database for MariaDB?“ beantwortet wurde, sind Sie für Folgendes bereit:
- Auf der Seite mit der Preisgestaltung finden Sie Kostenvergleiche und Rechner. [Preise](https://azure.microsoft.com/pricing/details/mariadb/)
- Legen Sie los, und erstellen Sie Ihren ersten Server. [Erstellen eines Servers für Azure Database for MariaDB mithilfe des Azure-Portals](quickstart-create-mariadb-server-database-using-azure-portal.md)

<!--- - Build your first app using your preferred language: [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [Go](./connect-go.md) --->
