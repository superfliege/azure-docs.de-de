---
title: "Exportieren von U-SQL-Datenbanken mithilfe von Azure Data Lake Tools für Visual Studio | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mithilfe von Azure Data Lake Tools für Visual Studio eine U-SQL-Datenbank exportieren und gleichzeitig in ein lokales Konto importieren."
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: 
editor: 
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/27/2017
ms.author: yanacai
ms.openlocfilehash: 9f11e07f7fbaf2b708d6fbc8a746238745132bda
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2017
---
# <a name="how-to-export-u-sql-database"></a>Exportieren einer U-SQL-Datenbank

In diesem Dokument erfahren Sie, wie Sie mithilfe von [Azure Data Lake Tools für Visual Studio](http://aka.ms/adltoolsvs) eine U-SQL-Datenbank als einzelnes U-SQL-Skript und heruntergeladene Ressourcen exportieren. Im gleichen Vorgang kann die exportierte Datenbank auch in ein lokales Konto importiert werden.

Kunden verwalten in der Regel mehrere Umgebungen für die Entwicklung, zum Testen und für die Produktion. Diese Umgebungen werden sowohl in einem lokalen Konto auf dem lokalen Computer des Entwicklers als auch im Azure Data Lake Analytics-Konto in Azure gehostet. Beim Entwickeln und Optimieren von U-SQL-Abfragen in Entwicklungs- und Testumgebungen ist es üblich, dass Entwickler in der Produktionsdatenbank alles neu erstellen müssen. Mit dem **Datenbankexport-Assistenten** kann dieser Vorgang beschleunigt werden. Entwickler können mithilfe des Assistenten die vorhandene Datenbankumgebung und die Beispieldaten in anderen Azure Data Lake Analytics-Konten klonen.

## <a name="export-steps"></a>Exportschritte

### <a name="step-1-right-click-the-database-in-server-explorer-and-click-export"></a>Schritt 1: Klicken mit der rechten Maustaste auf die Datenbank im Server-Explorer und Klicken auf „Exportieren“

Im Server-Explorer sind alle Azure Data Lake Analytics-Konten aufgeführt, für die Sie über eine Berechtigung verfügen. Erweitern Sie das Konto, das die zu exportierende Datenbank enthält, und klicken Sie mit der rechten Maustaste auf die Datenbank, um **Exportieren** auszuwählen. Wenn das Kontextmenü nicht angezeigt wird, müssen Sie [das Tool auf die aktuelle Version aktualisieren](http://aka.ms/adltoolsvs).

![Data Lake Analytics-Tools – Exportieren der Datenbank](./media/data-lake-analytics-data-lake-tools-export-database/export-database.png)

### <a name="step-2-configure-the-objects-you-want-to-export"></a>Schritt 2: Konfigurieren der zu exportierenden Objekte

In einigen Fällen ist eine Datenbank umfangreich, Sie benötigen jedoch nur einen kleinen Teil davon. Dann können Sie im Export-Assistenten eine Teilmenge der Objekte konfigurieren, die exportiert werden sollen. Beachten Sie, dass die Exportaktion durch Ausführen eines U-SQL-Auftrags abgeschlossen wird und dass daher durch den Export vom Azure-Konto Kosten anfallen.

![Data Lake Analytics-Tools – Datenbankexport-Assistent](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard.png)

### <a name="step-3-check-the-objects-list-and-more-configurations"></a>Schritt 3: Überprüfen der Objektliste und weiterer Konfigurationen

In diesem Schritt können Sie die ausgewählten Objekte oben im Dialogfeld erneut überprüfen. Wenn Fehler vorliegen, können Sie auf „Zurück“ klicken, um zum vorherigen Schritt zurückzukehren und die zu exportierenden Objekte neu konfigurieren.

Sie können zudem weitere Konfigurationen zum Exportziel durchführen. Diese Konfigurationen sind in der folgenden Tabelle beschrieben:

|Konfiguration|Beschreibung|
|-------------|-----------|
|Zielname|Dieser Name gibt an, wo die exportierten Datenbankressourcen, z.B. Assemblys, zusätzliche Dateien und Beispieldaten, gespeichert werden sollen. Unter dem Stammordner für Ihre lokalen Daten wird ein Ordner mit diesem Namen erstellt.|
|Projektverzeichnis|Dieser Pfad definiert, wo das exportierte U-SQL-Skript, das alle Datenbankobjektdefinitionen enthält, gespeichert werden soll.|
|Nur Schema|Bei Auswahl dieser Option werden nur Datenbankdefinitionen und Ressourcen (z.B. Assemblys und zusätzliche Dateien) exportiert.|
|Schema und Daten|Bei Auswahl dieser Option werden Datenbankdefinitionen, Ressourcen und Daten exportiert. Die oberen N Tabellenzeilen werden exportiert.|
|Automatisch in lokale Datenbank importieren|Die Auswahl dieser Konfiguration bedeutet, dass die exportierte Datenbank nach Abschluss des Exports automatisch in Ihre lokale Datenbank importiert wird.|

![Data Lake Analytics-Tools – Konfiguration des Datenbankexport-Assistenten](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-configuration.png)

### <a name="step-4-check-the-export-results"></a>Schritt 4: Überprüfen der Exportergebnisse

Nach dem Festlegen aller Einstellungen und dem Abschluss des Exports werden die exportierten Ergebnisse im Protokollfenster im Assistenten angezeigt. Über das im folgenden Screenshot mit einem roten Rechteck gekennzeichnete Protokoll können Sie den Speicherort des exportierten U-SQL-Skripts und der Datenbankressourcen, einschließlich Assemblys, zusätzlicher Dateien und Beispieldaten, finden.

![Data Lake Analytics-Tools – Datenbankexport-Assistent abgeschlossen](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-completed.png)

## <a name="how-to-import-the-exported-database-to-local-account"></a>Importieren der exportierten Datenbank in ein lokales Konto

Für diesen Import aktivieren Sie am besten während des Exports in Schritt 3 die Option **Automatisch in lokale Datenbank importieren**. Wenn Sie vergessen haben, diese Option zu aktivieren, können Sie das exportierte U-SQL-Skript über das Exportprotokoll suchen und das Skript lokal ausführen, um die Datenbank in das lokale Konto zu importieren.

## <a name="how-to-import-the-exported-database-to-azure-data-lake-analytics-account"></a>Importieren der exportierten Datenbank in ein Azure Data Lake Analytics-Konto

Für den Import der Datenbank in ein anderes Azure Data Lake Analytics-Konto sind zwei Schritte erforderlich:

1. Laden Sie die exportierten Ressourcen (einschließlich Assemblys, zusätzlicher Dateien und Beispieldaten) in das Azure Data Lake Store-Standardkonto des Azure Data Lake Analytics-Kontos hoch, in das die Ressourcen importiert werden sollen. Sie finden den Ordner der exportierten Ressourcen unter dem Stammordner für lokale Daten. Importieren Sie den gesamten Ordner in den Stammordner des Standardspeicherkontos.
2. Senden Sie nach Abschluss des Uploads das exportierte U-SQL-Skript an das Azure Data Lake Analytics-Konto, in das die Datenbank importiert werden soll.

## <a name="known-limitation"></a>Bekannte Einschränkung

Wenn Sie im Assistenten die Option **Schema und Data** ausgewählt haben, führt das Tool derzeit einen U-SQL-Auftrag aus, um die in Tabellen gespeicherten Daten zu exportieren. Daher kann der Vorgang zum Exportieren der Daten langsam sein. Zudem fallen Kosten an. 

## <a name="next-steps"></a>Nächste Schritte

* [Grundlegendes zur U-SQL-Datenbank](https://msdn.microsoft.com/library/azure/mt621299.aspx) 
* [Testen und Debuggen von U-SQL-Aufträgen mit lokalen Testläufen und dem Azure Data Lake U-SQL SDK](data-lake-analytics-data-lake-tools-local-run.md)


