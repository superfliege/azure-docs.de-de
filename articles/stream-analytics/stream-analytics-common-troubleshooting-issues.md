---
title: Problembehandlung für falsch formatierte Eingabeereignisse in Azure Stream Analytics
description: Wie ermittle ich, welches Ereignis in meinen Eingabedaten ein Problem in einem Stream Analytics-Auftrag verursacht?
services: stream-analytics
author: jasonwhowell
manager: Kfile
ms.author: jasonh
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/05/2018
ms.openlocfilehash: fcbb03b4d9aed797cf99c374661c743d39f81276
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="common-issues-in-stream-analytics-and-steps-to-troubleshoot"></a>Häufig auftretende Probleme bei Stream Analytics und Schritten zur Problembehandlung

## <a name="troubleshoot-for-malformed-input-events"></a>Problembehandlung für falsch formatierte Eingabeereignisse

Wenn der Eingabedatenstrom Ihres Stream Analytics-Auftrags falsch formatierte Nachrichten enthält, werden Serialisierungsprobleme verursacht. Zu falsch formatierten Nachrichten zählen unter anderem Nachrichten mit fehlerhafter Serialisierung (also etwa mit fehlenden Klammern in einem JSON-Objekt oder mit einem falschen Zeitstempelformat). Wenn ein Stream Analytics-Auftrag eine falsch formatierte Nachricht empfängt, wird diese gelöscht und der Benutzer mit einer Warnung benachrichtigt. Ein Warnsymbol wird auf der Kachel **Eingaben** des Stream Analytics-Auftrags angezeigt (dieses Warnzeichen wird so lange angezeigt, wie sich der Auftrag im Ausführungsstatus befindet):

![Kachel „Eingaben“](media/stream-analytics-malformed-events/inputs_tile.png)

Sie können die Diagnoseprotokolle aktivieren, um Details zur Warnung anzuzeigen. Bei Ereignissen zu falsch formatierten Eingaben enthalten die Ausführungsprotokolle einen Eintrag mit einer Meldung ähnlich der folgenden: „Meldung: Fehler beim Deserialisieren der Eingabeereignisse von der Ressource <blob URI> als JSON“. 

### <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

1. Navigieren Sie zur Eingabekachel, und klicken Sie darauf, um Warnungen anzuzeigen.
2. Auf der Kachel „Eingabedetails“ werden Warnungen mit Details zum Problem angezeigt. Nachfolgend sehen Sie ein Beispiel für eine Warnmeldung. Diese Warnmeldung enthält die Partition, den Offset und die Sequenznummern mit falsch formatierten JSON-Daten. 

   ![Warnmeldung mit Offset](media/stream-analytics-malformed-events/warning_message_with_offset.png)

3. Um die JSON-Daten mit dem falschen Format abzurufen, führen Sie den Code in „CheckMalformedEvents.cs“ aus. Sie erhalten diese Datei über das [GitHub-Beispielrepository](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/CheckMalformedEventsEH). Dieser Code liest die Partitions-ID und den Offset und gibt die Daten aus dem Offset aus. 

4. Nach dem Lesen der Daten können Sie das Serialisierungsformat analysieren und korrigieren. 

## <a name="handling-duplicate-records-when-using-azure-sql-database-as-output-for-a-stream-analytics-job"></a>Behandeln doppelter Datensätze bei Verwendung von Azure SQL-Datenbank als Ausgabe für einen Stream Analytics-Auftrag

Wenn Sie Azure SQL-Datenbank als Ausgabe an einen Stream Analytics-Auftrag konfigurieren, fügt dieser die Datensätze als Masseneintrag in der Zieltabelle hinzu. Im Allgemeinen garantiert Azure Stream Analytics [mindestens eine Übermittlung]( https://msdn.microsoft.com/azure/stream-analytics/reference/event-delivery-guarantees-azure-stream-analytics) an die Ausgabesenke. Sie können jedoch auch eine [genau einmalige Übermittlung]( https://blogs.msdn.microsoft.com/streamanalytics/2017/01/13/how-to-achieve-exactly-once-delivery-for-sql-output/) an die SQL-Ausgabe erreichen, wenn für die SQL-Tabelle eine UNIQUE-Einschränkung definiert wurde. 

Wenn für eine SQL-Tabelle UNIQUE-Einschränkungen für den Schlüssel eingerichtet wurden und doppelte Datensätze in die SQL-Tabelle eingefügt werden, entfernt Azure Stream Analytics den doppelten Datensatz, indem es die Daten in Batches aufteilt und diese rekursiv einfügt, bis ein doppelter Datensatz gefunden wird. Wenn die Pipeline sehr viele doppelte Zeilen enthält, ist das Teilen und rekursive Einfügen der Daten mit Ignorieren einzelner Duplikate sehr zeitaufwendig. Wenn Sie im Aktivitätsprotokoll mehrere Warnmeldungen zu Schlüsselverstößen innerhalb der letzten Stunde finden, ist es äußerst wahrscheinlich, dass Ihre SQL-Ausgabe den gesamten Auftrag verlangsamt. Um dieses Problem zu beheben, sollten Sie [den Index konfigurieren]( https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql), der die Schlüsselverstöße verursacht, indem Sie die Option IGNORE_DUP_KEY aktivieren. Durch Aktivieren dieser Option wird bei Masseneinfügungen das Ignorieren doppelter Werte durch SQL zugelassen. SQL Azure generiert anstelle eines Fehlers einfach eine Warnung. Azure Stream Analytics gibt daraufhin keine Fehler zu Primärschlüsselverstößen mehr zurück.

Beachten Sie die folgenden Beobachtungen, wenn Sie IGNORE_DUP_KEY für mehrere Typen von Indizes konfigurieren:

* IGNORE_DUP_KEY kann nicht für eine PRIMARY KEY- oder UNIQUE-Einschränkung festgelegt werden, die ALTER INDEX verwendet. Sie müssen stattdessen den Index löschen und neu erstellen.  
* Sie können die Option IGNORE_DUP_KEY mit ALTER INDEX für einen eindeutigen Index festlegen, wenn sich dieser von der PRIMARY KEY-/UNIQUE-Einschränkung unterscheidet und mit einer CREATE INDEX- oder INDEX-Definition erstellt wurde.  
* IGNORE_DUP_KEY gilt nicht für Spaltenspeicherindizes, da für solche Indizes keine Eindeutigkeit erzwungen werden kann.  

## <a name="next-steps"></a>Nächste Schritte

* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

