---
title: 'Advanced Data Security: Azure SQL-Datenbank | Microsoft-Dokumentation'
description: Erfahren Sie etwas über die Funktionen zur Ermittlung und Klassifizierung sensibler Daten, die Verwaltung von Datenbankrisiken und die Erkennung ungewöhnlicher Aktivitäten, die eine Bedrohung für Ihre Azure SQL-Datenbank darstellen können.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: conceptual
author: ronitr
ms.author: ronitr
ms.reviewer: vanto
manager: craigg
ms.date: 02/04/2019
ms.openlocfilehash: 388a1a2ff70f4d00e66511811bffdb76806ed713
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2019
ms.locfileid: "55735213"
---
# <a name="advanced-data-security-for-azure-sql-database"></a>Advanced Data Security für Azure SQL-Datenbank

Advanced Data Security ist ein einheitliches Paket für erweiterte SQL-Sicherheitsfunktionen. Dazu zählen die Funktionen zur Ermittlung und Klassifizierung sensibler Daten, Ermittlung und Verringerung potenzieller Datenbankschwachstellen und Erkennung ungewöhnlicher Aktivitäten, die eine Bedrohung für Ihre Datenbank darstellen können. Es bietet einen einzelnen Anlaufpunkt zum Aktivieren und Verwalten dieser Funktionen.

## <a name="overview"></a>Übersicht

Advanced Data Security (ADS) stellt eine Reihe erweiterter SQL-Sicherheitsfunktionen bereit, darunter Datenermittlung und -klassifizierung, Risikoanalyse und Erkennung von Bedrohungen.

- [Datenermittlung und -klassifizierung](sql-database-data-discovery-and-classification.md) (zurzeit in der Vorschau) bietet Funktionen in Azure SQL-Datenbank zum Ermitteln, Klassifizieren, Bezeichnen und Schützen sensibler Daten in Ihren Datenbanken. Das Feature kann Einblicke in den Zustand Ihrer Datenbankklassifizierung bereitstellen und den Zugriff auf sensible Daten innerhalb der Datenbank und außerhalb ihrer Grenzen verfolgen.
- [Sicherheitsrisikobewertung](sql-vulnerability-assessment.md) ist ein einfach zu konfigurierender Dienst, mit dem potenzielle Schwachstellen in der Datenbank ermittelt, nachverfolgt und behoben werden können. Er bietet Einblicke in Ihren Sicherheitsstatus, enthält umsetzbare Schritte zum Beheben von Sicherheitsproblemen und verbessert Ihre Datenbanksicherheit.
- [Bedrohungserkennung](sql-database-threat-detection-overview.md) dient zum Erkennen anomaler Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Ihre Datenbank zuzugreifen oder sie missbräuchlich zu nutzen. Sie überwacht Ihre Datenbank fortlaufend auf verdächtige Aktivitäten und bietet sofortige Sicherheitswarnungen zu potenziellen Sicherheitslücken, Angriffe durch Einschleusung von SQL-Befehlen und ungewöhnliche Datenbankzugriffsmuster. Die Warnungen der Bedrohungserkennung enthalten Details zu verdächtigen Aktivitäten sowie Empfehlungen, wie die Bedrohung untersucht und abgewendet werden kann.

Aktivieren Sie SQL ADS einmalig, um sämtliche der enthaltenen Features zu aktivieren. Mit nur einem Klick können Sie ADS für alle Datenbanken auf Ihrem SQL-Datenbank-Server oder Ihrer verwalteten Instanz aktivieren. Um ADS-Einstellungen aktivieren oder verwalten zu können, müssen Sie der Rolle [SQL-Sicherheits-Manager](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager), „SQL-Datenbankadministrator“ oder „SQL Server-Administrator“ angehören. 

Die Preise für ADS richten sich nach dem Standardtarif für Azure Security Center, wobei jeder geschützte SQL-Datenbank-Server bzw. jede verwaltete Instanz als ein Knoten betrachtet wird. Für neue zu schützende Ressourcen kann der Standardtarif von Security Center kostenlos getestet werden. Weitere Informationen finden Sie auf der Seite mit den [Preisen für Azure Security Center](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="getting-started-with-ads"></a>Erste Schritte mit ADS

Die folgenden Schritte bilden den Einstieg in ADS.

## <a name="1-enable-ads"></a>1. Aktivieren von ADS

Aktivieren Sie ADS, indem Sie unterhalb der Überschrift **Sicherheit** für Ihren SQL-Datenbank-Server oder Ihre verwaltete Instanz zu **Advanced Data Security** wechseln. Um ADS für alle Datenbanken auf dem Datenbankserver oder der verwalteten Instanz zu aktivieren, klicken Sie auf **Advanced Data Security für den Server aktivieren**.

![Aktivieren von ADS](./media/sql-advanced-protection/enable_atp.png) 

> [!NOTE]
> Die Kosten von ADS orientieren sich am Azure Security Center-Standardtarif pro Knoten, wobei ein Knoten dem gesamten logischen SQL-Server oder der verwalteten Instanz entspricht. Sie bezahlen daher nur einmal, um alle Datenbanken auf dem Server oder der verwalteten Instanz mit ADS zu schützen. Sie können ADS anfänglich mit einer kostenlosen Testversion testen.

## <a name="2-configure-vulnerability-assessment"></a>2. Konfigurieren der Sicherheitsrisikobewertung

Um mit der Nutzung der Sicherheitsrisikobewertung zu beginnen, müssen Sie ein Speicherkonto konfigurieren, in dem Scanergebnisse gespeichert werden. Klicken Sie dazu auf die Karte „Sicherheitsrisikobewertung“.

![Konfigurieren der Sicherheitsrisikobewertung](./media/sql-advanced-protection/configure_va.png) 

Wählen Sie ein Speicherkonto zum Speichern der Scanergebnisse aus, oder erstellen Sie eines. Sie können auch regelmäßig wiederholte Scans aktivieren, indem Sie die Sicherheitsrisikobewertung so konfigurieren, dass automatische Überprüfungen ein Mal pro Woche ausgeführt werden. Eine Zusammenfassung der Scanergebnisse wird an die angegebenen E-Mail-Adressen gesendet.

![Einstellungen der Sicherheitsrisikobewertung](./media/sql-advanced-protection/va_settings.png) 

## <a name="3-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>3. Beginnen mit dem Klassifizieren von Daten, Nachverfolgen von Sicherheitsrisiken und Untersuchen von Bedrohungswarnungen

Klicken Sie auf die Karte **Datenermittlung und -klassifizierung**, um empfohlene sensible Spalten anzuzeigen und Ihre Daten mit permanenten Sensibilitätsbezeichnungen zu klassifizieren. Klicken Sie auf die Karte **Sicherheitsrisikobewertung**, um Sicherheitsrisikoscans und -berichte anzuzeigen und zu verwalten und Ihren Sicherheitsstatus zu verfolgen. Wenn Sie Sicherheitswarnungen empfangen haben, klicken Sie auf die Karte **Bedrohungserkennung**, um Details zu den Warnungen anzuzeigen und einen konsolidierten Bericht für alle Warnungen in Ihrem Azure-Abonnement über die Seite mit den Sicherheitswarnungen im Azure Security Center abzurufen.

## <a name="4-manage-ads-settings-on-your-sql-database-server-or-managed-instance"></a>4. Verwalten der ADS-Einstellungen auf Ihrem SQL-Datenbank-Server oder der verwalteten Instanz

Um die ADS-Einstellungen anzuzeigen und zu verwalten, navigieren Sie unterhalb der Überschrift **Sicherheit** für Ihren SQL-Datenbank-Server oder Ihre verwaltete Instanz zu **Advanced Data Security**. Auf dieser Seite können Sie ADS aktivieren oder deaktivieren und die Einstellungen für die Bedrohungserkennung für Ihren gesamten SQL-Datenbank-Server oder Ihre verwaltete Instanz anpassen.

![Servereinstellungen](./media/sql-advanced-protection/server_settings.png) 

## <a name="5-manage-ads-settings-for-a-sql-database"></a>5. Verwalten von ADS-Einstellungen für eine SQL-Datenbank

Um die ADS-Einstellungen für eine bestimmte Datenbank außer Kraft zu setzen, aktivieren Sie das Kontrollkästchen **Advanced Data Security auf Datenbankebene aktivieren**. Verwenden Sie diese Option nur, wenn eine besondere Anforderung zum Empfangen von separaten Bedrohungserkennungswarnungen für diese Datenbank vorliegt, die anstelle der Warnungen für alle Datenbanken auf dem Datenbankserver oder der verwalteten Instanz oder zusätzlich zu diesen empfangen werden müssen. 

Wenn das Kontrollkästchen aktiviert ist, klicken Sie auf **Einstellungen der Bedrohungserkennung für diese Datenbank**, und konfigurieren Sie dann die entsprechenden Einstellungen für diese Datenbank.

![Datenbank- und Bedrohungserkennungseinstellungen](./media/sql-advanced-protection/database_threat_detection_settings.png) 

Sie können auch über den ADS-Datenbankbereich auf die Advanced Data Security-Einstellungen für Ihren Datenbankserver oder Ihre verwaltete Instanz zugreifen. Klicken Sie im ADS-Bereich auf **Einstellungen**, und klicken Sie dann auf **Advanced Data Security-Servereinstellungen anzeigen**. 

![Datenbankeinstellungen](./media/sql-advanced-protection/database_settings.png) 

## <a name="next-steps"></a>Nächste Schritte 

- Weitere Informationen zur [Datenermittlung und -klassifizierung](sql-database-data-discovery-and-classification.md) 
- Weitere Informationen zur [Sicherheitsrisikobewertung](sql-vulnerability-assessment.md) 
- Weitere Informationen zur [Bedrohungserkennung](sql-database-threat-detection.md)
- Weitere Informationen zu [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
