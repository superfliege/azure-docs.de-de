---
title: Bedrohungserkennung – Verwaltete Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation
description: Die Bedrohungserkennung erkennt anomale Datenbankaktivitäten, die auf potenzielle Sicherheitsrisiken für die Datenbank hindeuten.
services: sql-database
author: rmatchoro
manager: craigg
ms.service: sql-database
ms.custom: security, managed instance
ms.topic: conceptual
ms.date: 03/07/2018
ms.author: ronmat
ms.reviewer: carlrab
ms.openlocfilehash: 14ef907717045e2e0cf297694d92468a65e57e4d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34650076"
---
# <a name="azure-sql-database-managed-instance-threat-detection"></a>Bedrohungserkennung für eine verwaltete Azure SQL-Datenbank-Instanz

Die SQL-Bedrohungserkennung erkennt anomale Aktivitäten, die auf ungewöhnliche und möglicherweise schädliche Versuche hinweisen, bei denen auf Datenbanken in einer verwalteten Azure SQL-Datenbank-Instanz (Vorschauversion) zugegriffen oder diese missbraucht werden sollen.

## <a name="overview"></a>Übersicht

Die Bedrohungserkennung erkennt anomale Datenbankaktivitäten, die auf potenzielle Sicherheitsrisiken für die verwaltete Instanz hinweisen. Die Bedrohungserkennung ist jetzt für verwaltete Instanzen als Vorschauversion verfügbar.

Die Bedrohungserkennung bietet eine neue Sicherheitsebene, die es den Kunden ermöglicht, auf erkannte potenzielle Bedrohungen zu reagieren. Zu diesem Zweck werden Sicherheitswarnungen zu anomalen Datenbankaktivitäten bereitgestellt. Die Bedrohungserkennung vereinfacht den Umgang mit potenziellen Bedrohungen für die verwaltete Instanz. Das Fachwissen eines Sicherheitsexperten oder die Verwaltung komplexer Sicherheitsüberwachungssysteme sind dabei nicht erforderlich. Zur vollständigen Untersuchung empfiehlt es sich, die Überwachung der verwalteten Instanz in Azure zu aktivieren, bei der Datenbankereignisse in ein Überwachungsprotokoll in Ihrem Azure Storage-Konto geschrieben werden. 

Durch die SQL-Bedrohungserkennung werden Warnungen in [Azure Security Center](https://azure.microsoft.com/services/security-center/) integriert. Jede geschützte verwaltete Instanz wird zum selben Preis wie der Standard-Tarif von Azure Security Center abgerechnet (15 $/Knoten/Monat). Hierbei zählt jede verwaltete Instanz als ein Knoten.  

## <a name="set-up-threat-detection-for-your-managed-instance-in-the-azure-portal"></a>Einrichten der Bedrohungserkennung für Ihre verwaltete Instanz im Azure-Portal
1. Starten Sie das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com).
2. Navigieren Sie zur Konfigurationsseite der verwalteten Instanz, die geschützt werden soll. Wählen Sie auf der Seite **Einstellungen** die Option **Bedrohungserkennung** aus. 
3. Auf der Konfigurationsseite für die Bedrohungserkennung 
   - Aktivieren **Sie** die Bedrohungserkennung.
   - Konfigurieren Sie die **Liste der E-Mail-Empfänger**, die bei Erkennung anomaler Datenbankaktivitäten Sicherheitswarnungen erhalten sollen.
   - Wählen Sie das **Azure Storage-Konto** aus, in dem Überwachungsdatensätze zu anomalen Bedrohungen gespeichert werden. 
4.  Klicken Sie auf **Speichern**, um die neue oder aktualisierte Richtlinie für die Bedrohungserkennung zu speichern.

   ![Bedrohungserkennung](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

## <a name="explore-anomalous-managed-instance-activities-upon-detection-of-a-suspicious-event"></a>Untersuchen anomaler Aktivitäten in der verwalteten Instanz bei Erkennung eines verdächtigen Ereignisses

1. Bei Erkennung anormaler Datenbankaktivitäten erhalten Sie eine E-Mail-Benachrichtigung. 

   Die E-Mail enthält Informationen zum verdächtigen Sicherheitsereignis (wie etwa Art der anomalen Aktivitäten, Datenbankname, Servername und Zeit des Ereignisses). Darüber hinaus enthält sie Angaben zu möglichen Ursachen und empfohlenen Maßnahmen zur Untersuchung und Abwehr der potenziellen Bedrohung für die verwaltete Instanz.

   ![E-Mail zur Bedrohungserkennung](./media/sql-database-managed-instance-threat-detection/threat-detection-email.png)

2. Klicken Sie in der E-Mail auf den Link **View recent SQL alerts** (Aktuelle SQL-Warnungen anzeigen), um das Azure-Portal zu starten und die Azure Security Center-Seite für Warnungen zu öffnen, auf der eine Übersicht über die aktiven SQL-Bedrohungen angezeigt wird, die in der Datenbank der verwalteten Instanz erkannt wurden.

   ![Aktive Bedrohungen](./media/sql-database-managed-instance-threat-detection/active-threats.png)

3. Klicken Sie auf eine bestimmte Warnung, um weitere Details und Aktionen zum Untersuchen der entsprechenden Bedrohung und Abwehren zukünftiger Bedrohungen anzuzeigen.

   Beispielsweise ist die Einschleusung von SQL-Befehlen ein verbreitetes Sicherheitsproblem für Webanwendungen im Internet. Die Einschleusung von SQL-Befehlen wird für Angriffe auf datengesteuerte Anwendungen verwendet. Die Angreifer nutzen Sicherheitslücken der Anwendung, um böswillige SQL-Anweisungen in Eingabefelder der Anwendung einzuschleusen, sodass Daten in der Datenbank manipuliert oder verändert werden können. Bei Warnungen in Bezug auf die Einschleusung von SQL-Befehlen schließen die Details der Warnung die anfällige missbräuchlich genutzte SQL-Anweisung ein.

   ![Einschleusung von SQL-Befehlen](./media/sql-database-managed-instance-threat-detection/sql-injection.png)

## <a name="managed-instance-threat-detection-alerts"></a>Warnungen zur Bedrohungserkennung für eine verwaltete Instanz 

Die Bedrohungserkennung für eine verwaltete Instanz erkennt anomale Aktivitäten, die auf ungewöhnliche und möglicherweise schädliche Versuche hinweisen, bei denen auf Datenbanken zugegriffen oder diese missbraucht werden sollen. Hierbei werden die folgenden Warnungen ausgelöst:
- **Anfälligkeit für die Einschleusung von SQL-Befehlen**: Diese Warnung wird ausgelöst, wenn eine Anwendung in der Datenbank eine fehlerhafte SQL-Anweisung generiert. Dies kann ein Hinweis auf ein mögliches Sicherheitsrisiko in Bezug auf Angriffe mit Einschleusung von SQL-Befehlen sein. Es gibt zwei mögliche Gründe für die Generierung einer fehlerhaften Anweisung:
 - Ein Fehler im Anwendungscode, der zur fehlerhaften SQL-Anweisung führt
 - Anwendungscode oder gespeicherte Prozeduren führen bei der Erstellung der fehlerhaften SQL-Anweisung keine Bereinigung der Benutzereingabe durch, und dies kann für eine Einschleusung von SQL-Befehlen ausgenutzt werden
- **Potenzielle Einschleusung von SQL-Befehlen**: Diese Warnung wird ausgelöst, wenn ein aktiver Exploit für ein identifiziertes Anwendungssicherheitsrisiko in Bezug auf die Einschleusung von SQL-Befehlen besteht. Das bedeutet, dass der Angreifer versucht, schädliche SQL-Anweisungen einzuschleusen, indem er den anfälligen Anwendungscode bzw. die gespeicherten Prozeduren verwendet.
- **Access from unusual location** (Zugriff von einem ungewöhnlichen Ort): Diese Warnung wird ausgelöst, wenn eine Änderung des Zugriffsmusters für eine verwaltete Instanz erfolgt, bei der sich eine Person von einem ungewöhnlichen Ort aus bei der verwalteten Instanz angemeldet hat. In einigen Fällen erkennt die Warnung eine legitime Aktion (eine neue Anwendung oder Wartungsvorgänge von Entwicklern). In anderen Fällen erkennt die Warnung eine schädliche Aktion (ehemaliger Mitarbeiter, externer Angreifer usw.).
- **Access from unusual Azure data center** (Zugriff aus einem ungewöhnlichen Azure-Rechenzentrum): Diese Warnung wird ausgelöst, wenn sich das Zugriffsmuster für eine verwaltete Instanz geändert hat, weil sich eine Person über ein ungewöhnliches Azure-Rechenzentrum bei der verwalteten Instanz angemeldet hat, das in letzter Zeit beim Zugriff auf diese verwaltete Instanz nicht ermittelt wurde. In einigen Fällen erkennt die Warnung eine legitime Aktion (Ihre neue Anwendung in Azure, Power BI, Azure SQL-Abfrage-Editor usw.). In anderen Fällen erkennt die Warnung ggf. eine schädliche Aktion einer Azure-Ressource bzw. eines -Diensts (ehemaliger Mitarbeiter, externer Angreifer).
- **Access from unfamiliar principal** (Zugriff über einen ungewöhnlichen Prinzipal): Diese Warnung wird ausgelöst, wenn eine Änderung des Zugriffsmusters für eine verwaltete Instanz erfolgt, bei der sich eine Person über einen ungewöhnlichen Prinzipal (SQL-Benutzer) bei der verwalteten Instanz angemeldet hat. In einigen Fällen erkennt die Warnung eine legitime Aktion (eine neue Anwendung oder Wartungsvorgänge von Entwicklern). In anderen Fällen erkennt die Warnung eine schädliche Aktion (ehemaliger Mitarbeiter, externer Angreifer).
- **Access from a potentially harmful application** (Zugriff über eine potenziell schädliche Anwendung): Diese Warnung wird ausgelöst, wenn zum Zugreifen auf die Datenbank eine potenziell schädliche Anwendung verwendet wird. In einigen Fällen erkennt die Warnung aktive Eindringversuche. In anderen Fällen erkennt die Warnung einen Angriff mit allgemeinen Angriffstools.
- **Brute force SQL credentials** (Brute-Force-Angriff auf SQL-Anmeldeinformationen): Diese Warnung wird ausgelöst, wenn eine ungewöhnlich hohe Zahl von fehlgeschlagenen Anmeldungen mit unterschiedlichen Anmeldeinformationen vorliegt. In einigen Fällen erkennt die Warnung aktive Eindringversuche. In anderen Fällen erkennt die Warnung einen Brute-Force-Angriff.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur verwalteten Instanz finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md).
- Weitere Informationen zur [Überwachung einer verwalteten Instanz](https://go.microsoft.com/fwlink/?linkid=869430) 
- Weitere Informationen zu [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
