---
title: Advanced Threat Protection – Azure SQL-Datenbank | Microsoft-Dokumentation
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
ms.date: 05/17/2018
ms.openlocfilehash: cb3d8629d5d72644e647da14e7c3b31a4914814d
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47159557"
---
# <a name="advanced-threat-protection-for-azure-sql-database"></a>Advanced Threat Protection für Azure SQL-Datenbank

SQL Advanced Threat Protection ist ein einheitliches Paket für erweiterte SQL-Sicherheitsfunktionen. Dazu zählen die Funktionen zur Ermittlung und Klassifizierung sensibler Daten, Ermittlung und Verringerung potenzieller Datenbankschwachstellen und Erkennung ungewöhnlicher Aktivitäten, die eine Bedrohung für Ihre Datenbank darstellen können. Es bietet einen einzelnen Anlaufpunkt zum Aktivieren und Verwalten dieser Funktionen. 

## <a name="overview"></a>Übersicht

SQL Advanced Threat Protection (ATP) stellt eine Reihe erweiterter SQL-Sicherheitsfunktionen bereit, darunter Datenermittlung und -klassifizierung, Risikoanalyse und Erkennung von Bedrohungen. 

- [Datenermittlung und -klassifizierung](sql-database-data-discovery-and-classification.md) (zurzeit in der Vorschau) bietet Funktionen in Azure SQL-Datenbank zum Ermitteln, Klassifizieren, Bezeichnen und Schützen sensibler Daten in Ihren Datenbanken. Das Feature kann Einblicke in den Zustand Ihrer Datenbankklassifizierung bereitstellen und den Zugriff auf sensible Daten innerhalb der Datenbank und außerhalb ihrer Grenzen verfolgen.
- [Sicherheitsrisikobewertung](sql-vulnerability-assessment.md) ist ein einfach zu konfigurierender Service, mit dem potenzielle Schwachstellen in der Datenbank ermittelt, nachverfolgt und behoben werden können. Er bietet Einblicke in Ihren Sicherheitsstatus, enthält umsetzbare Schritte zum Beheben von Sicherheitsproblemen und verbessert Ihre Datenbanksicherheit.
- [Bedrohungserkennung](sql-database-threat-detection.md) dient zum Erkennen anomaler Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Ihre Datenbank zuzugreifen oder sie missbräuchlich zu nutzen. Sie überwacht Ihre Datenbank fortlaufend auf verdächtige Aktivitäten und bietet sofortige Sicherheitswarnungen zu potenziellen Sicherheitslücken, Angriffe durch Einschleusung von SQL-Befehlen und ungewöhnliche Datenbankzugriffsmuster. Die Warnungen der Bedrohungserkennung enthalten Details zu verdächtigen Aktivitäten sowie Empfehlungen, wie die Bedrohung untersucht und abgewendet werden kann.

Aktivieren Sie SQL ATP einmal, um alle diese enthaltenen Features zu aktivieren. Sie können mit einem Klick ATP für Ihren gesamten Datenbankserver aktivieren und diesen Dienst so für alle Datenbanken auf dem Server verfügbar machen. 

Die Preise für ATP sind mit 15 US-Dollar pro Knoten und Monat am Standardtarif von Azure Security Center ausgerichtet, wobei jeder geschützte SQL Datenbank-Server als ein Knoten betrachtet wird. Die ersten 60 Tage nach der Aktivierung werden als kostenloser Testzeitraum betrachtet und nicht in Rechnung gestellt. Weitere Informationen finden Sie auf der Seite mit den [Preisen für Azure Security Center](https://azure.microsoft.com/pricing/details/security-center/).


## <a name="getting-started-with-atp"></a>Erste Schritte mit ATP 
Die folgenden Schritte bilden den Einstieg in ATP. 

## <a name="1-enable-atp"></a>1. Aktivieren von ATP

Aktivieren Sie ATP, indem Sie im Azure SQL-Datenbank-Bereich unter der Überschrift **Sicherheit** zu **Advanced Threat Protection** navigieren. Um ATP für alle Datenbanken auf dem Server zu aktivieren, klicken Sie auf **Enable Advanced Threat Protection on the server** (Advanced Threat Protection auf dem Server aktivieren).

![Aktivieren von ATP](./media/sql-advanced-protection/enable_atp.png) 

> [!NOTE]
> Die Kosten für ATP betragen 15 US-Dollar pro Knoten und Monat, wobei ein Knoten den gesamten logischen SQL-Server umfasst. Sie bezahlen daher nur einmal, um alle Datenbanken auf dem Server mit ATP zu schützen. Die ersten 60 Tage gelten als kostenlose Testversion.

## <a name="2-configure-vulnerability-assessment"></a>2. Konfigurieren der Sicherheitsrisikobewertung

Um mit der Nutzung der Sicherheitsrisikobewertung zu beginnen, müssen Sie ein Speicherkonto konfigurieren, in dem Scanergebnisse gespeichert werden. Klicken Sie dazu auf die Karte „Sicherheitsrisikobewertung“.

![Konfigurieren der Sicherheitsrisikobewertung](./media/sql-advanced-protection/configure_va.png) 

Wählen Sie ein Speicherkonto zum Speichern der Scanergebnisse aus, oder erstellen Sie eines. Sie können auch regelmäßig wiederholte Scans aktivieren, indem Sie die Sicherheitsrisikobewertung so konfigurieren, dass automatische Überprüfungen einmal pro Woche ausgeführt werden. Eine Zusammenfassung der Scanergebnisse wird an die angegebenen E-Mail-Adressen gesendet.

![Einstellungen der Sicherheitsrisikobewertung](./media/sql-advanced-protection/va_settings.png) 

## <a name="3-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>3. Beginnen mit dem Klassifizieren von Daten, Nachverfolgen von Sicherheitsrisiken und Untersuchen von Bedrohungswarnungen

Klicken Sie auf die Karte **Data Discovery and Classification** (Datenermittlung und -klassifizierung), um empfohlene sensible Spalten anzuzeigen und Ihre Daten mit permanenten Sensibilitätsbezeichnungen zu klassifizieren. Klicken Sie auf die Karte **Sicherheitsrisikobewertung**, um Sicherheitsrisikoscans und -berichte anzuzeigen und zu verwalten und Ihren Sicherheitsstatus zu verfolgen. Wenn Sie Sicherheitswarnungen empfangen haben, klicken Sie auf die Karte **Bedrohungserkennung**, um Details zu den Warnungen anzuzeigen und einen konsolidierten Bericht für alle Warnungen in Ihrem Azure-Abonnement über die Seite mit den Sicherheitswarnungen im Azure Security Center abzurufen.

## <a name="4-manage-atp-settings-on-your-sql-server"></a>4. Verwalten von ATP-Einstellungen auf Ihrem SQL-Server

Navigieren Sie zum Anzeigen und Verwalten von Advanced Threat Protection-Einstellungen im SQL Server-Bereich unter der Überschrift **Sicherheit** zu **Advanced Threat Protection**. Auf dieser Seite können Sie ATP aktivieren oder deaktivieren und die Einstellungen für die Bedrohungserkennung für Ihren gesamten SQL-Server anpassen.

![Servereinstellungen](./media/sql-advanced-protection/server_settings.png) 

## <a name="5-manage-atp-settings-for-a-sql-database"></a>5. Verwalten von ATP-Einstellungen für eine SQL-Datenbank

Um die ATP-Einstellungen für die Bedrohungserkennung für eine bestimmte Datenbank außer Kraft zu setzen, aktivieren Sie das Kontrollkästchen **Advanced Threat Protection auf Datenbankebene aktivieren**. Verwenden Sie diese Option nur, wenn eine besondere Anforderung zum Empfangen von separaten Bedrohungserkennungswarnungen für diese Datenbank vorliegt, die anstelle der Warnungen für alle Datenbanken auf dem Server oder zusätzlich zu diesen empfangen werden müssen. 

Wenn das Kontrollkästchen aktiviert ist, klicken Sie auf **Einstellungen der Bedrohungserkennung für diese Datenbank**, und konfigurieren Sie dann die entsprechenden Einstellungen für diese Datenbank.

![Datenbank- und Bedrohungserkennungseinstellungen](./media/sql-advanced-protection/database_threat_detection_settings.png) 

Die Advanced Threat Protection-Einstellungen für den Server sind auch vom ATP-Datenbankbereich aus zugänglich. Klicken Sie im ATP-Hauptbereich auf **Einstellungen**, und klicken Sie dann auf **Advanced Threat Protection-Servereinstellungen anzeigen**. 

![Datenbankeinstellungen](./media/sql-advanced-protection/database_settings.png) 

## <a name="next-steps"></a>Nächste Schritte 

- Weitere Informationen zur [Datenermittlung und -klassifizierung](sql-database-data-discovery-and-classification.md) 
- Weitere Informationen zur [Sicherheitsrisikobewertung](sql-vulnerability-assessment.md) 
- Weitere Informationen zur [Bedrohungserkennung](sql-database-threat-detection.md)
- Weitere Informationen zu [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
