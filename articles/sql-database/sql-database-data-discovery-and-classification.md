---
title: Azure SQL-Datenbank – Datenermittlung und -klassifizierung | Microsoft-Dokumentation
description: Azure SQL-Datenbank – Datenermittlung und -klassifizierung
services: sql-database
author: giladmit
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: security
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: giladm
ms.openlocfilehash: cc093bebb4b3c39140d6fa5370a78d59168990fa
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37950807"
---
# <a name="azure-sql-database-data-discovery-and-classification"></a>Azure SQL-Datenbank – Datenermittlung und -klassifizierung
Datenermittlung und -klassifizierung (zurzeit in der Vorschau) bietet erweiterte Funktionen für Azure SQL-Datenbank zum **Ermitteln**, **Klassifizieren**, **Bezeichnen** & **Schützen** sensibler Daten in Ihren Datenbanken.
Das Ermitteln und Klassifizieren Ihrer sensiblen Daten (Geschäfts-/Finanz-/Gesundheitsdaten, PII usw.) kann eine entscheidende Rolle in der Strategie Ihrer Organisation zum Datenschutz spielen. Das Feature kann als Infrastruktur für Folgendes dienen:
* Unterstützen der Einhaltung von Datenschutzstandards und gesetzlicher Bestimmungen
* Verschiedene Sicherheitsszenarien, z.B. Überwachung und Warnungen bei abweichendem Zugriff auf sensible Daten
* Steuern des Zugriffs auf und Härten der Sicherheit von Datenbanken, die sensible Daten enthalten

Datenermittlung und -klassifizierung ist Teil des Angebote [SQL Advanced Threat Protection (ATP)](sql-advanced-threat-protection.md). Dabei handelt es sich um ein vereinheitlichtes Paket für erweiterte SQL-Sicherheitsfunktionen. Der Zugriff auf und die Verwaltung von Datenermittlung und -klassifizierung ist über das zentrale SQL-ATP-Portal möglich.

> [!NOTE]
> Dieses Dokument bezieht sich nur auf Azure SQL-Datenbank. Informationen zu SQL Server (lokal) finden Sie unter [SQL-Datenermittlung und -klassifizierung](https://go.microsoft.com/fwlink/?linkid=866999).

## <a id="subheading-1"></a>Was ist Datenermittlung und -klassifizierung?
Datenermittlung und -klassifizierung führt eine Reihe von erweiterten Diensten und neuen SQL-Funktionen ein und bildet damit ein neues SQL Information Protection-Paradigma für den Schutz der Daten – nicht nur der Datenbank:
* **Ermittlung und Empfehlungen:** Das Klassifizierungsmodul scannt Ihre Datenbank und identifiziert Spalten, die möglicherweise sensible Daten enthalten. Es bietet dann eine einfache Möglichkeit, diese zu überprüfen und die entsprechenden Klassifizierungsempfehlungen über das Azure Portal anzuwenden.
* **Bezeichnung:** Mithilfe der neu im SQL-Modul eingeführten Metadatenattribute können sensible Daten dauerhaft mit Klassifizierungsbezeichnungen gekennzeichnet werden. Diese Metadaten können dann für erweiterte Überwachungs- und Schutzszenarien für sensible Daten genutzt werden.
* **Vertraulichkeit von Abfrageergebnissen:** Die Vertraulichkeit von Abfrageergebnissen wird zu Überwachungszwecken in Echtzeit berechnet.
* **Sichtbarkeit:** Der Klassifizierungsstatus der Datenbank kann in einem detaillierten Dashboard im Portal angezeigt werden. Darüber hinaus können Sie einen Bericht (im Excel-Format) herunterladen, der für Compliance- und Überwachungszwecke und für weitere Zwecke verwendet werden kann.

## <a id="subheading-2"></a>Ermitteln, Klassifizieren und Bezeichnen von vertraulichen Spalten
Im folgenden Abschnitt werden die Schritte zum Ermitteln, Klassifizieren und Bezeichnen von Spalten mit sensiblen Daten in der Datenbank sowie das Anzeigen des aktuellen Klassifizierungsstatus der Datenbank und das Exportieren von Berichten beschrieben.

Die Klassifizierung umfasst zwei Metadatenattribute:
* Bezeichnungen – die wichtigsten Klassifizierungsattribute zum Definieren der Vertraulichkeitsstufe der in der Spalte gespeicherten Daten  
* Informationstypen – bieten zusätzliche Granularität für den Typ der in der Spalte gespeicherten Daten

## <a name="classify-your-sql-database"></a>Klassifizieren Ihrer SQL-Datenbank

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

2. Navigieren Sie im Bereich von Azure SQL-Datenbank unter der Überschrift „Sicherheit“ zu **Advanced Threat Protection**. Klicken Sie zum Aktivieren von Advanced Threat Protection, und klicken Sie dann auf die Karte **Datenermittlung und -klassifizierung (Vorschauversion)**.

   ![Überprüfen einer Datenbank](./media/sql-data-discovery-and-classification/data_classification.png)

3. Die Registerkarte **Übersicht** enthält eine Zusammenfassung des aktuellen Klassifizierungsstatus der Datenbank. Dazu gehört auch eine ausführliche Liste aller klassifizierten Spalten, die Sie filtern können, um nur bestimmte Schemateile, Informationstypen und Bezeichnungen anzuzeigen. Wenn Sie noch keine Spalten klassifiziert haben, [fahren Sie mit Schritt 5 fort](#step-5).

   ![Zusammenfassung des aktuellen Klassifizierungsstatus](./media/sql-data-discovery-and-classification/2_data_classification_overview_dashboard.png)

4. Um einen Bericht im Excel-Format herunterzuladen, klicken Sie oben im Fenster im Menü auf die Option **Exportieren**.

   ![Exportieren in Excel](./media/sql-data-discovery-and-classification/3_data_classification_export_report.png)

5.  <a id="step-5"></a>Um die Klassifizierung Ihrer Daten zu starten, klicken Sie oben im Fenster auf die Registerkarte **Klassifikation**.

    ![Klassifizieren von Daten](./media/sql-data-discovery-and-classification/4_data_classification_classification_tab_click.png)

6. Das Klassifizierungsmodul scannt Ihre Datenbank nach Spalten mit potenziell sensiblen Daten und stellt eine Liste der **empfohlenen Spaltenklassifizierungen** bereit. So zeigen Sie Klassifizierungsempfehlungen an und übernehmen sie

    * Um die Liste der empfohlenen Spaltenklassifizierungen anzuzeigen, klicken Sie am unteren Rand des Fensters auf den Empfehlungsbereich:

      ![Klassifizieren von Daten](./media/sql-data-discovery-and-classification/5_data_classification_recommendations_panel.png)

    * Überprüfen Sie die Liste der Empfehlungen. Um eine Empfehlung für eine bestimmte Spalte zu akzeptieren, aktivieren Sie das Kontrollkästchen in der linken Spalte der entsprechenden Zeile. Sie können auch *alle Empfehlungen* als akzeptiert markieren, indem Sie das Kontrollkästchen im Tabellenkopf der Empfehlungen aktivieren.

       ![Überprüfen der Empfehlungsliste](./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png)

    * Um die ausgewählten Empfehlungen anzuwenden, klicken Sie auf die blaue Schaltfläche **Accept selected recommendations** (Ausgewählte Empfehlungen akzeptieren).

      ![Anwenden von Empfehlungen](./media/sql-data-discovery-and-classification/7_data_classification_accept_selected_recommendations.png)

7. Sie können Spalten auch **manuell klassifizieren** – entweder als Alternative zur empfehlungsbasierten Klassifizierung oder als Ergänzung:

    * Klicken Sie im oberen Menü des Fensters auf **Klassifizierung hinzufügen**.

      ![Manuelles Hinzufügen einer Klassifizierung](./media/sql-data-discovery-and-classification/8_data_classification_add_classification_button.png)

    * Wählen Sie im daraufhin geöffneten Kontextfenster das Schema, die Tabelle und dann die Spalte, die Sie klassifizieren möchten, sowie die den Informationstyp und die Vertraulichkeitsbezeichnung aus. Klicken Sie dann am unteren Rand des Kontextfensters auf die blaue Schaltfläche **Klassifizierung hinzufügen**.

      ![Auswählen der klassifizierenden Spalte](./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png)

8. Klicken Sie im Menü oben im Fenster auf **Speichern**, um die Klassifizierung abzuschließen und die Datenbankspalten dauerhaft mit den neuen Klassifizierungsmetadaten zu bezeichnen (mit Tags zu versehen).

   ![Speichern](./media/sql-data-discovery-and-classification/10_data_classification_save.png)

## <a id="subheading-3"></a>Überwachen des Zugriffs auf sensible Daten

Ein wichtiger Aspekt des Paradigmas für den Schutz von Informationen ist die Möglichkeit, den Zugriff auf sensible Daten zu überwachen. Die [Azure SQL-Datenbank-Überwachung](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) wurde erweitert, um das Überwachungsprotokoll um das neue Feld *data_sensitivity_information* zu ergänzen. In diesem werden die Vertraulichkeitsklassifizierungen (Bezeichnungen) der eigentlichen Daten erfasst, die bei der Abfrage zurückgegeben wurden.

![Überwachungsprotokoll](./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png)

## <a id="subheading-4"></a>Automatisierte bzw. programmgesteuerte Klassifizierung

Mit T-SQL können Sie Spaltenklassifizierungen hinzufügen/entfernen sowie alle Klassifizierungen für die gesamte Datenbank abrufen.

> [!NOTE]
> Bei der Verwendung von T-SQL zur Verwaltung von Bezeichnungen gibt es keine Validierung, dass zu einer Spalte hinzugefügte Bezeichnungen in der Richtlinie zum Schutz von Organisationsinformationen (die in den Portalempfehlungen erscheinen) vorhanden sind. Aus diesem Grund müssen Sie dies überprüfen.

* Hinzufügen/Aktualisieren der Klassifizierung einer oder mehrerer Spalten: [VERTRAULICHKEITSKLASSIFIZIERUNG HINZUFÜGEN](https://docs.microsoft.com/en-us/sql/t-sql/statements/add-sensitivity-classification-transact-sql)
* Entfernen der Klassifizierung einer oder mehrerer Spalten: [VERTRAULICHKEITSKLASSIFIZIERUNG VERWERFEN](https://docs.microsoft.com/en-us/sql/t-sql/statements/drop-sensitivity-classification-transact-sql)
* Anzeigen aller Klassifizierungen in der Datenbank: [sys.sensitivity_classifications](https://docs.microsoft.com/en-us/sql/relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql)

## <a id="subheading-5"></a>Nächste Schritte

- Erfahren Sie mehr über [SQL Advanced Threat Protection](sql-advanced-threat-protection.md).
- Sie sollten in Betracht ziehen, die [Azure SQL-Datenbank-Überwachung](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) für die Überwachung und Überprüfung des Zugriffs auf Ihre klassifizierten sensiblen Daten zu konfigurieren.

<!--Anchors-->
[SQL Data Discovery & Classification overview]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Auditing access to sensitive data]: #subheading-3
[Automated/Programmatic classification]: #subheading-4
[Next Steps]: #subheading-5
