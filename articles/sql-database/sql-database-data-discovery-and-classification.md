---
title: Azure SQL-Datenbank – Datenermittlung und -klassifizierung | Microsoft-Dokumentation
description: Azure SQL-Datenbank – Datenermittlung und -klassifizierung
services: sql-database
author: giladm
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: security
ms.topic: article
ms.date: 01/29/2018
ms.author: giladm
ms.openlocfilehash: 18afebefaee9117244767e0a00e293a297bbf9af
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="azure-sql-database-data-discovery-and-classification"></a>Azure SQL-Datenbank – Datenermittlung und -klassifizierung
Datenermittlung und -klassifizierung (zurzeit in der Vorschau) bietet erweiterte Funktionen für Azure SQL-Datenbank zum **Ermitteln**, **Klassifizieren**, **Bezeichnen** & **Schützen** sensibler Daten in Ihren Datenbanken.
Das Ermitteln und Klassifizieren Ihrer sensiblen Daten (Geschäfts-/Finanz-/Gesundheitsdaten, PII usw.) kann eine entscheidende Rolle in der Strategie Ihrer Organisation zum Datenschutz spielen. Das Feature kann als Infrastruktur für Folgendes dienen:
* Unterstützen der Einhaltung von Datenschutzstandards und behördlichen Vorschriften wie GDPR
* Verschiedene Sicherheitsszenarien, z.B. Überwachung und Warnungen bei abweichendem Zugriff auf sensible Daten
* Steuern des Zugriffs auf und Härten der Sicherheit von Datenbanken, die sensible Daten enthalten

## <a id="subheading-1"></a>Übersicht
Datenermittlung und -klassifizierung führt eine Reihe von erweiterten Diensten und neuen SQL-Funktionen ein und bildet damit ein neues SQL Information Protection-Paradigma für den Schutz der Daten – nicht nur der Datenbank:
* **Ermittlung und Empfehlungen:** Das Klassifizierungsmodul scannt Ihre Datenbank und identifiziert Spalten, die möglicherweise sensible Daten enthalten. Es bietet dann eine einfache Möglichkeit, diese zu überprüfen und die entsprechenden Klassifizierungsempfehlungen über das Azure Portal anzuwenden.
* **Bezeichnung:** Mithilfe der neu im SQL-Modul eingeführten Metadatenattribute können sensible Daten dauerhaft mit Klassifizierungsbezeichnungen gekennzeichnet werden. Diese Metadaten können dann für erweiterte Überwachungs- und Schutzszenarien für sensible Daten genutzt werden.
* **Vertraulichkeit von Abfrageergebnissen:** Die Vertraulichkeit von Abfrageergebnissen wird zu Überwachungszwecken in Echtzeit berechnet.
* **Sichtbarkeit:** Der Klassifizierungsstatus der Datenbank kann in einem detaillierten Dashboard im Portal angezeigt werden. Darüber hinaus können Sie einen Bericht (im Excel-Format) herunterladen, der für Compliance- und Überwachungszwecke und für weitere Zwecke verwendet werden kann.

## <a id="subheading-2"></a>Ermitteln, Klassifizieren und Bezeichnen von Spalten mit sensiblen Daten
Im folgenden Abschnitt werden die Schritte zum Ermitteln, Klassifizieren und Bezeichnen von Spalten mit sensiblen Daten in der Datenbank sowie das Anzeigen des aktuellen Klassifizierungsstatus der Datenbank und das Exportieren von Berichten beschrieben.

Die Klassifizierung umfasst zwei Metadatenattribute:
* Bezeichnungen – die wichtigsten Klassifizierungsattribute zum Definieren der Vertraulichkeitsstufe der in der Spalte gespeicherten Daten  
* Informationstypen – bieten zusätzliche Granularität für den Typ der in der Spalte gespeicherten Daten

<br>
**So klassifizieren Sie Ihre SQL-Datenbank**

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

2. Navigieren Sie zur Einstellung **Datenermittlung und -klassifizierung (Vorschau)** in Ihrer SQL-Datenbank.

    ![Navigationsbereich][1]

3. Die Registerkarte **Übersicht** enthält eine Zusammenfassung des aktuellen Klassifizierungsstatus der Datenbank. Dazu gehört auch eine ausführliche Liste aller klassifizierten Spalten, die Sie filtern können, um nur bestimmte Schemateile, Informationstypen und Bezeichnungen anzuzeigen. Wenn Sie noch keine Spalten klassifiziert haben, [fahren Sie mit Schritt 5 fort](#step-5).

    ![Navigationsbereich][2]

4. Um einen Bericht im Excel-Format herunterzuladen, klicken Sie oben im Fenster im Menü auf die Option **Exportieren**.

    ![Navigationsbereich][3]

5.  <a id="step-5"></a>Um die Klassifizierung Ihrer Daten zu starten, klicken Sie oben im Fenster auf die Registerkarte **Klassifikation**.

    ![Navigationsbereich][4]

6. Das Klassifizierungsmodul scannt Ihre Datenbank nach Spalten mit potenziell sensiblen Daten und stellt eine Liste der **empfohlenen Spaltenklassifizierungen** bereit. So zeigen Sie Klassifizierungsempfehlungen an und übernehmen sie

    * Um die Liste der empfohlenen Spaltenklassifizierungen anzuzeigen, klicken Sie am unteren Rand des Fensters auf den Empfehlungsbereich:

        ![Navigationsbereich][5]

    * Überprüfen Sie die Liste der Empfehlungen. Um eine Empfehlung für eine bestimmte Spalte zu akzeptieren, aktivieren Sie das Kontrollkästchen in der linken Spalte der entsprechenden Zeile. Sie können auch *alle Empfehlungen* als akzeptiert markieren, indem Sie das Kontrollkästchen im Tabellenkopf der Empfehlungen aktivieren.

        ![Navigationsbereich][6]

    * Um die ausgewählten Empfehlungen anzuwenden, klicken Sie auf die blaue Schaltfläche **Accept selected recommendations** (Ausgewählte Empfehlungen akzeptieren).

        ![Navigationsbereich][7]

7. Sie können Spalten auch **manuell klassifizieren** – entweder als Alternative zur empfehlungsbasierten Klassifizierung oder als Ergänzung:

    * Klicken Sie im oberen Menü des Fensters auf **Klassifizierung hinzufügen**.

        ![Navigationsbereich][8]

    * Wählen Sie im daraufhin geöffneten Kontextfenster das Schema, die Tabelle und dann die Spalte, die Sie klassifizieren möchten, sowie die den Informationstyp und die Vertraulichkeitsbezeichnung aus. Klicken Sie dann am unteren Rand des Kontextfensters auf die blaue Schaltfläche **Klassifizierung hinzufügen**.

        ![Navigationsbereich][9]

8. Klicken Sie im Menü oben im Fenster auf **Speichern**, um die Klassifizierung abzuschließen und die Datenbankspalten dauerhaft mit den neuen Klassifizierungsmetadaten zu bezeichnen (mit Tags zu versehen).

    ![Navigationsbereich][10]

## <a id="subheading-3"></a>Überwachen des Zugriffs auf sensible Daten

Ein wichtiger Aspekt des Paradigmas für den Schutz von Informationen ist die Möglichkeit, den Zugriff auf sensible Daten zu überwachen.

Die [Azure SQL-Datenbank-Überwachung](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) wurde erweitert, um das Überwachungsprotokoll um das neue Feld *data_sensitivity_information* zu ergänzen. In diesem werden die Vertraulichkeitsklassifizierungen (Bezeichnungen) der eigentlichen Daten erfasst, die bei der Abfrage zurückgegeben wurden.

![Navigationsbereich][11]

## <a id="subheading-4"></a>Nächste Schritte
Sie sollten in Betracht ziehen, die [Azure SQL-Datenbank-Überwachung](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) für die Überwachung und Überprüfung des Zugriffs auf Ihre klassifizierten sensiblen Daten zu konfigurieren.

<!--Anchors-->
[SQL Data Discovery & Classification overview]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Auditing access to sensitive data]: #subheading-3
[Next Steps]: #subheading-4

<!--Image references-->
[1]: ./media/sql-data-discovery-and-classification/1_data_classification_settings_menu.png
[2]: ./media/sql-data-discovery-and-classification/2_data_classification_overview_dashboard.png
[3]: ./media/sql-data-discovery-and-classification/3_data_classification_export_report.png
[4]: ./media/sql-data-discovery-and-classification/4_data_classification_classification_tab_click.png
[5]: ./media/sql-data-discovery-and-classification/5_data_classification_recommendations_panel.png
[6]: ./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png
[7]: ./media/sql-data-discovery-and-classification/7_data_classification_accept_selected_recommendations.png
[8]: ./media/sql-data-discovery-and-classification/8_data_classification_add_classification_button.png
[9]: ./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png
[10]: ./media/sql-data-discovery-and-classification/10_data_classification_save.png
[11]: ./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png
