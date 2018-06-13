---
title: Bedrohungserkennung – Azure SQL Data Warehouse | Microsoft-Dokumentation
description: Konfigurieren Sie die Bedrohungserkennung, und untersuchen Sie verdächtige Ereignisse in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 8dc1ef0432536c6bfd4fe069406cd057ca069ea2
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
ms.locfileid: "31523896"
---
# <a name="threat-detection-in-azure-sql-data-warehouse"></a>Bedrohungserkennung in Azure SQL Data Warehouse
Konfigurieren Sie die Bedrohungserkennung, und untersuchen Sie verdächtige Ereignisse in Azure SQL Data Warehouse.

## <a name="what-is-threat-detection"></a>Worum handelt es sich bei der Bedrohungserkennung?
Die Bedrohungserkennung erkennt anomale Datenbankaktivitäten, die auf potenzielle Sicherheitsrisiken für die Datenbank hindeuten. 

Die Bedrohungserkennung bietet eine neue Sicherheitsebene und ermöglicht es den Kunden, auf erkannte potenzielle Bedrohungen zu reagieren. Zu diesem Zweck werden Sicherheitshinweise zu anormalen Aktivitäten bereitgestellt. Die Benutzer können die verdächtigen Ereignisse mithilfe der [Azure SQL Data Warehouse-Überwachung](sql-data-warehouse-auditing-overview.md) untersuchen, um zu ermitteln, ob diese Ereignisse durch einen Datenzugriff, die Verletzung der Datensicherheit oder die missbräuchliche Nutzung von Daten im Data Warehouse verursacht wurden.
Die Bedrohungserkennung vereinfacht den Umgang mit potenziellen Bedrohungen für das Data Warehouse. Das Fachwissen eines Sicherheitsexperten oder die Verwaltung komplexer Sicherheitsüberwachungssysteme sind dabei nicht erforderlich.

So erkennt die Bedrohungserkennung beispielsweise bestimmte anormale Datenbankaktivitäten, die auf eine potenzielle Einschleusung von SQL-Befehlen hindeuten. Die Einschleusung von SQL-Befehlen ist ein verbreitetes Sicherheitsproblem für Webanwendungen im Internet und wird für Angriffe auf datengesteuerte Anwendungen verwendet. Die Angreifer nutzen Sicherheitslücken der Anwendung, um böswillige SQL-Anweisungen in Eingabefelder der Anwendung einzuschleusen und so an die in der Datenbank enthaltenen Daten zu gelangen oder diese zu verändern.

## <a name="set-up-threat-detection-for-your-database"></a>Einrichten der Bedrohungserkennung für Ihre Datenbank
1. Starten Sie das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com).
2. Navigieren Sie zum Konfigurationsblatt des SQL Data Warehouse, das Sie überwachen möchten. Wählen Sie auf dem Blatt „Einstellungen“ die Option **Überwachung und Bedrohungserkennung**.
   
    ![Navigationsbereich](media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png)
3. **Aktivieren Sie** auf dem Konfigurationsblatt für die **Überwachung und Bedrohungserkennung** die Überwachung. Daraufhin werden die Einstellungen für die Bedrohungserkennung angezeigt.
   
    ![Navigationsbereich](media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png)
4. Aktivieren **Sie** die Bedrohungserkennung.
5. Konfigurieren Sie die Liste der E-Mail-Empfänger, die bei Erkennung anomaler Data Warehouse-Aktivitäten eine Sicherheitsbenachrichtigung erhalten sollen.
6. Klicken Sie auf dem Blatt für die Konfiguration der **Überwachung und Bedrohungserkennung** auf **Speichern**, um die neue oder aktualisierte Richtlinie für die Überwachung und Bedrohungserkennung zu speichern.
   
    ![Navigationsbereich](media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png)

## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Untersuchen anomaler Data Warehouse-Aktivitäten bei Erkennung eines verdächtigen Ereignisses
1. Bei Erkennung anomaler Datenbankaktivitäten erhalten Sie eine E-Mail-Benachrichtigung. <br/>
   Die E-Mail enthält Informationen zum verdächtigen Sicherheitsereignis (wie etwa Art der anomalen Aktivitäten, Datenbankname, Servername und Zeit des Ereignisses). Darüber hinaus enthält sie Angaben zu möglichen Ursachen und empfohlenen Maßnahmen zur Untersuchung und Abwehr der potenziellen Bedrohung für die Datenbank.<br/>
   
    ![Navigationsbereich](media/sql-data-warehouse-security-threat-detection/4_td_email.png)
2. Klicken Sie in der E-Mail auf den Link für das Azure SQL-Überwachungsprotokoll, **um das Azure-Portal aufzurufen** und die relevanten Überwachungsdatensätze für die Zeit des verdächtigen Ereignisses anzuzeigen.
   
    ![Navigationsbereich](media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png)
3. Klicken Sie auf die Überwachungsdatensätze, um weitere Details zu den verdächtigen Datenbankaktivitäten (wie etwa SQL-Anweisung, Fehlerursache und Client-IP) anzuzeigen.
   
    ![Navigationsbereich](media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png)
4. Klicken Sie auf dem Blatt mit den Überwachungsdatensätzen auf **In Excel öffnen**, um eine vorkonfigurierte Excel-Vorlage zu öffnen, das Überwachungsprotokoll zu importieren und eine ausführlichere Analyse für die Zeit des verdächtigen Ereignisses durchzuführen.<br/>
   **Hinweis:** Ab Excel 2010 werden Power Query und die Einstellung **Schnelles Kombinieren** benötigt.
   
    ![Navigationsbereich](media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png)
5. So konfigurieren Sie die Einstellung **Schnelles Kombinieren**: Klicken Sie auf der Registerkarte **POWER QUERY** des Menübands auf **Optionen**, um das gleichnamige Dialogfeld anzuzeigen. Navigieren Sie zum Datenschutzabschnitt, und wählen Sie die zweite Option zum Ignorieren der Sicherheitsstufen und zur potenziellen Verbesserung der Leistung:
   
    ![Navigationsbereich](media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png)
6. Wenn Sie SQL-Überwachungsprotokolle laden möchten, vergewissern Sie sich, dass die Parameter auf der Einstellungsregisterkarte ordnungsgemäß festgelegt sind. Klicken Sie dann im Menüband auf „Daten“ und anschließend auf die Schaltfläche „Alle aktualisieren“.
   
    ![Navigationsbereich](media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png)
7. Die Ergebnisse werden auf dem Blatt mit den **SQL-Überwachungsprotokollen** angezeigt. Dadurch können Sie die erkannten anomalen Aktivitäten eingehender untersuchen und die Auswirkungen des Sicherheitsereignisses in Ihrer Anwendung begrenzen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Sicherheit finden Sie unter [Schützen einer Datenbank in SQL Data Warehouse](sql-data-warehouse-overview-manage-security.md).
