---
title: Grundlegendes zu den Ausgaben von Azure Stream Analytics
description: In diesem Artikel werden die Datenausgabeoptionen beschrieben, die in Azure Stream Analytics verfügbar sind, z.B. Power BI für Analyseergebnisse.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/14/2018
ms.openlocfilehash: d717737bc2b15e57ae32faffaece96f78a7cc013
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45577819"
---
# <a name="understand-outputs-from-azure-stream-analytics"></a>Grundlegendes zu den Ausgaben von Azure Stream Analytics
In diesem Artikel werden die unterschiedlichen Arten von Ausgaben beschrieben, die für einen Azure Stream Analytics-Auftrag verfügbar sind. Mit Ausgaben können Sie die Ergebnisse des Stream Analytics-Auftrags aufbewahren und speichern. Indem Sie die Ausgabedaten verwenden, können Sie weitere Geschäftsanalysen und Data Warehousing-Vorgänge für Ihre Daten durchführen. 

Verweisen Sie beim Entwerfen Ihrer Stream Analytics-Abfrage auf den Namen der Ausgabe, indem Sie die [INTO-Klausel](https://msdn.microsoft.com/azure/stream-analytics/reference/into-azure-stream-analytics) verwenden. Sie können eine einzelne Ausgabe pro Auftrag oder bei Bedarf auch mehrere Ausgaben pro Streamingauftrag verwenden, indem Sie in der Abfrage mehrere INTO-Klauseln angeben.

Zum Erstellen, Bearbeiten und Testen von Stream Analytics-Auftragsausgaben können Sie das [Azure-Portal](stream-analytics-quick-create-portal.md#configure-output-to-the-job), [Azure PowerShell](stream-analytics-quick-create-powershell.md#configure-output-to-the-job), die [.NET-API](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.ioutputsoperations?view=azure-dotnet), die [REST-API](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-output) und [Visual Studio](stream-analytics-quick-create-vs.md) verwenden.

Für einige Ausgabetypen wird die [Partitionierung](#partitioning) unterstützt, und die [Ausgabebatchgrößen](#output-batch-size) variieren, um den Durchsatz zu optimieren.


## <a name="azure-data-lake-store"></a>Azure Data Lake Store
Stream Analytics unterstützt [Azure Data Lake-Speicher](https://azure.microsoft.com/services/data-lake-store/). Azure Data Lake-Speicher ist ein unternehmensweites riesiges Repository für Big Data-Analyseworkloads. Data Lake-Speicher ermöglicht es Ihnen, Daten von beliebiger Größe, Art und Erfassungsgeschwindigkeit zur Durchführung operativer und explorativer Analysen zu speichern. Stream Analytics muss autorisiert werden, um auf Data Lake Store zugreifen zu können.

Die Azure Data Lake Store-Ausgabe aus Stream Analytics ist zurzeit nicht in den Regionen Azure China (21Vianet) und Azure Deutschland (T-Systems International) verfügbar.

### <a name="authorize-an-azure-data-lake-store-account"></a>Autorisieren eines Azure Data Lake Store-Kontos

1. Wenn Data Lake Store im Azure-Portal als Ausgabe ausgewählt ist, werden Sie aufgefordert, eine Verbindung mit einem vorhandenen Data Lake Store zu autorisieren.  

   ![Autorisieren von Data Lake-Speicher](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

2. Falls Sie bereits Zugriff auf Data Lake Store haben, können Sie die Option **Jetzt autorisieren** wählen. Daraufhin wird für einen kurzen Moment eine Seite mit dem Hinweis **Umleitung an die Autorisierung...** angezeigt. Nach erfolgter Autorisierung wird die Seite angezeigt, über die Sie die Data Lake Store-Ausgabe konfigurieren können.

3. Sobald Sie das Data Lake-Speicherkonto authentifiziert haben, können Sie die Eigenschaften für die Data Lake-Speicherausgabe konfigurieren. Die folgende Tabelle enthält eine Liste von Eigenschaftennamen und deren Beschreibung zum Konfigurieren der Data Lake-Speicherausgabe.

   ![Autorisieren von Data Lake-Speicher](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

| Eigenschaftenname | BESCHREIBUNG | 
| --- | --- |
| Ausgabealias | Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an diesen Data Lake Store weiterzuleiten. | 
| Kontoname | Der Name des Data Lake-Speicherkontos, an das Sie die Ausgabe senden. Eine Dropdownliste der in Ihrem Abonnement verfügbaren Data Lake Store-Konten wird angezeigt. |
| Präfixmuster des Pfads | Der Dateipfad, mit dem Ihre Dateien im angegebenen Data Lake-Speicherkonto geschrieben werden. Sie können eine oder mehrere Instanzen der Variablen {date} und {time} angeben.</br><ul><li>Beispiel 1: folder1/logs / {date} / {time}</li><li>Beispiel 2: folder1/logs / {date}</li></ul><br>Der Zeitstempel der erstellten Ordnerstruktur folgt der UTC, nicht der lokalen Zeit.</br><br>Wenn das Dateipfadmuster keinen nachgestellten Schrägstrich („/“) enthält, wird auch das letzte Muster im Dateipfad als Dateinamenpräfix behandelt. </br></br>In diesen Fällen werden neue Dateien erstellt:<ul><li>Änderung im Ausgabeschema</li><li>Externer oder interner Neustart eines Auftrags</li></ul> |
| Datumsformat | Optional. Wenn das date-Token im Pfadpräfix verwendet wird, können Sie das Datumsformat auswählen, unter dem die Dateien gespeichert werden. Beispiel: YYYY/MM/TT |
|Zeitformat | Optional. Wenn das time-Token im Pfadpräfix verwendet wird, können Sie das Zeitformat auswählen, unter dem die Dateien gespeichert werden. Der einzige derzeit unterstützte Wert ist HH |
| Ereignisserialisierungsformat | Das Serialisierungsformat für Ausgabedaten. Es werden JSON, CSV und Avro unterstützt.| 
| Codieren | Bei Verwendung des CSV- oder JSON-Formats muss eine Codierung angegeben werden. UTF-8 ist derzeit das einzige unterstützte Codierungsformat.|
| Trennzeichen | Gilt nur für die CSV-Serialisierung. Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen zum Serialisieren der CSV-Daten. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Tabstopp und senkrechter Strich.|
| Format | Gilt nur für die JSON-Serialisierung. „Separate Zeile“ gibt an, dass die Ausgabe so formatiert wird, dass jedes JSON-Objekt in einer neuen Zeile enthalten ist. „Array“ gibt an, dass die Ausgabe als Array aus JSON-Objekten formatiert wird. Dieses Array wird nur geschlossen, wenn der Auftrag beendet wird oder Stream Analytics mit dem nächsten Zeitfenster fortfährt. Im Allgemeinen ist es besser, in separaten Zeilen geschriebenen JSON-Code zu verwenden, da er keine spezielle Behandlung erfordert, während noch in die Ausgabedatei geschrieben wird.|

### <a name="renew-data-lake-store-authorization"></a>Erneuern der Data Lake Store-Autorisierung
Sie müssen Ihr Data Lake Store-Konto erneut authentifizieren, wenn das Kennwort seit der Erstellung oder letzten Authentifizierung Ihres Auftrags geändert wurde. Wenn Sie sich nicht erneut authentifizieren, ergibt Ihr Auftrag keine Ausgabeergebnisse und zeigt einen Fehler mit dem Hinweis an, dass in den Vorgangsprotokollen eine erneute Autorisierung erfolgen muss. Derzeit besteht die Einschränkung, dass das Authentifizierungstoken alle 90 Tage manuell für sämtliche Aufträge mit der Data Lake-Speicherausgabe aktualisiert werden muss. 

Um die Autorisierung zu erneuern, klicken Sie in Ihrem Auftrag auf **Beenden**, navigieren zu Ihrem Data Lake Store und klicken dann auf den Link **Autorisierung erneuern**. Für einen kurzen Zeitraum wird die Seite **Umleitung an die Autorisierung...** angezeigt. Die Seite wird automatisch geschlossen, und im Erfolgsfall wird **Autorisierung wurde erfolgreich erneuert** angezeigt. Sie müssen dann unten auf der Seite auf **Speichern** klicken und können fortfahren, indem Sie Ihren Auftrag von der **letzten Beendigungszeit** aus neu starten, um einen Datenverlust zu vermeiden.

![Autorisieren von Data Lake-Speicher](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  

## <a name="sql-database"></a>SQL-Datenbank
[Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) kann als Ausgabe für relationale Daten oder für Anwendungen verwendet werden, die auf Inhalten aufsetzen, die in einer relationalen Datenbank gehostet werden. Stream Analytics-Aufträge schreiben in eine vorhandene Tabelle in einer Azure SQL-Datenbank.  Das Tabellenschema muss genau mit den Feldern und ihren Typen übereinstimmen, die aus Ihrem Auftrag ausgegeben werden. Eine [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/)-Instanz kann auch über die SQL-Datenbank-Ausgabeoption als Ausgabe angegeben werden. Die folgende Tabelle enthält die Eigenschaftennamen und die entsprechenden Beschreibungen zum Erstellen einer SQL-Datenbank-Ausgabe.

| Eigenschaftenname | BESCHREIBUNG |
| --- | --- |
| Ausgabealias |Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an diese Datenbank weiterzuleiten. |
| Datenbank | Der Name der Datenbank, an die Sie die Ausgabe senden. |
| Servername | Der Servername der SQL-Datenbank. |
| Username | Der Benutzername, der Schreibzugriff auf die Datenbank besitzt. |
| Kennwort | Das Kennwort, um eine Verbindung zur Datenbank herzustellen. |
| Table | Der Name der Tabelle, in die die Ausgabe geschrieben wird. Für den Tabellennamen muss zwischen Groß-/Kleinschreibung unterschieden werden. Das Schema dieser Tabelle sollte genau mit der Anzahl Felder und ihrer Typen übereinstimmen, die von der Auftragsausgabe generiert werden. |

> [!NOTE]
> Zurzeit wird das Azure SQL-Datenbank-Angebot für eine Auftragsausgabe in Stream Analytics unterstützt. Eine Azure-VM mit SQL Server und angefügter Datenbank wird jedoch nicht unterstützt. Dies soll in zukünftigen Versionen geändert werden.
> 

## <a name="blob-storage"></a>Blob Storage
Blobspeicher bietet eine kostengünstige und skalierbare Lösung zum Speichern von großen Mengen unstrukturierter Daten in der Cloud.  Eine Einführung in Azure Blob Storage und dessen Nutzung finden Sie in der Dokumentation unter [Verwenden von Blobs](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

Die folgende Tabelle enthält die Eigenschaftennamen und die entsprechenden Beschreibungen zum Erstellen einer Blobausgabe.

| Eigenschaftenname       | BESCHREIBUNG                                                                      |
| ------------------- | ---------------------------------------------------------------------------------|
| Ausgabealias        | Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an diesen Blobspeicher weiterzuleiten. |
| Speicherkonto     | Der Name des Speicherkontos, an das Sie die Ausgabe senden.               |
| Speicherkontoschlüssel | Der geheime Schlüssel, der dem Speicherkonto zugeordnet ist.                              |
| Speichercontainer   | Container stellen eine logische Gruppierung für Blobs bereit, die im Microsoft Azure-Blobdienst gespeichert sind. Wenn Sie ein Blob in den Blobdienst hochladen, müssen Sie einen Container für das Blob angeben. |
| Pfadmuster | Optional. Das Dateipfadmuster, mit dem Ihre Blobs im angegebenen Container geschrieben werden. <br /><br /> In dem Pfadmuster können Sie mindestens eine Instanz der date/time-Variablen verwenden, um die Häufigkeit anzugeben, mit der Blobs geschrieben werden: <br /> {date}, {time} <br /><br />Wenn Sie sich für die [Vorschauversion](https://aka.ms/ASAPreview) registriert haben, können Sie einen benutzerdefinierten {Feld}-Namen aus Ihren Ereignisdaten für Partitionsblobs angeben. Der Feldname ist alphanumerisch und kann Leerstellen, Bindestriche und Unterstriche enthalten. Für benutzerdefinierte Felder gelten die folgenden Einschränkungen: <ul><li>Keine Berücksichtigung der Groß-/Kleinschreibung (kein Unterschied zwischen Spalte „ID“ und Spalte „id“)</li><li>Geschachtelte Felder sind unzulässig (stattdessen Verwendung eines Alias in der Auftragsabfrage zum Vereinfachen des Felds)</li><li>Ausdrücke können nicht als Feldname verwendet werden.</li></ul> <br /><br /> Die Vorschauversion ermöglicht auch die Verwendung von Konfigurationen für benutzerdefinierte Angaben des Datums-/Uhrzeitformats im Pfad. Benutzerdefinierte Datums- und Uhrzeitformate müssen einzeln nacheinander angegeben werden und in das Schlüsselwort {datetime:\<Spezifizierer>} eingeschlossen sein. Zulässige Eingaben für \<Spezifizierer> sind „yyyy“, „MM“, „M“, „dd“, „d“, „HH“, „H“, „mm“, „m“, „ss“ und „s“. Das Schlüsselwort {datetime:\<Spezifizierer>} kann mehrmals im Pfad verwendet werden, um benutzerdefinierte Konfigurationen für Datum und Uhrzeit zu bilden. <br /><br />Beispiele: <ul><li>Beispiel 1: cluster1/logs/{date}/{time}</li><li>Beispiel 2: cluster1/logs/{date}</li><li>Beispiel 3 (Vorschauversion): cluster1/{client_id}/{date}/{time}</li><li>Beispiel 4 (Vorschauversion): cluster1/{datetime:ss}/{myField}; hierbei lautet die Abfrage: SELECT data.myField AS myField FROM Input;</li><li>Beispiel 4 (Vorschauversion): cluster1/year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}</ul><br /><br />Der Zeitstempel der erstellten Ordnerstruktur folgt der UTC, nicht der lokalen Zeit.<br /><br/>Die Dateibenennung erfolgt gemäß der nachstehenden Konvention: <br /><br />{Präfixmuster des Pfads}/schemaHashcode_Guid_Number.extension<br /><br />Beispielausgabedateien:<ul><li>Myoutput/20170901/00/45434_gguid_1.csv</li>  <li>Myoutput/20170901/01/45434_gguid_1.csv</li></ul> |
| Datumsformat | Optional. Wenn das date-Token im Pfadpräfix verwendet wird, können Sie das Datumsformat auswählen, unter dem die Dateien gespeichert werden. Beispiel: YYYY/MM/TT |
| Zeitformat | Optional. Wenn das time-Token im Pfadpräfix verwendet wird, können Sie das Zeitformat auswählen, unter dem die Dateien gespeichert werden. Der einzige derzeit unterstützte Wert ist HH |
| Ereignisserialisierungsformat | Das Serialisierungsformat für Ausgabedaten.  Es werden JSON, CSV und Avro unterstützt. |
| Codieren    | Bei Verwendung des CSV- oder JSON-Formats muss eine Codierung angegeben werden. UTF-8 ist derzeit das einzige unterstützte Codierungsformat. |
| Trennzeichen   | Gilt nur für die CSV-Serialisierung. Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen zum Serialisieren der CSV-Daten. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Tabstopp und senkrechter Strich. |
| Format      | Gilt nur für die JSON-Serialisierung. „Separate Zeile“ gibt an, dass die Ausgabe so formatiert wird, dass jedes JSON-Objekt in einer neuen Zeile enthalten ist. „Array“ gibt an, dass die Ausgabe als Array aus JSON-Objekten formatiert wird. Dieses Array wird nur geschlossen, wenn der Auftrag beendet wird oder Stream Analytics mit dem nächsten Zeitfenster fortfährt. Im Allgemeinen ist es besser, in separaten Zeilen geschriebenen JSON-Code zu verwenden, da er keine spezielle Behandlung erfordert, während noch in die Ausgabedatei geschrieben wird. |

Wenn Blobspeicher als Ausgabe verwendet wird, wird in den folgenden Fällen eine neue Datei im Blob erstellt:

* Wenn die Datei die maximale Anzahl von zulässigen Blöcken (derzeit 50.000) überschreitet. Die maximale zulässige Anzahl von Blöcken kann erreicht werden, ohne die maximal zulässige Blobgröße zu erreichen. Wenn die Ausgaberate z.B. hoch ist, können Sie mehr Bytes pro Block vorhanden sein, und die Datei ist größer. Wenn die Ausgaberate niedrig ist, weist jeder Block weniger Daten auf, und die Datei ist kleiner.
* Wenn es eine Schemaänderung in der Ausgabe gibt und das Ausgabeformat ein festes Schema erfordert (CSV und Avro).  
* Wenn ein Auftrag neu gestartet wird, entweder extern, weil er von einem Benutzer beendet und gestartet wurde, oder intern zu Systemwartungs- oder Fehlerbehebungszwecken.  
* Wenn die Abfrage vollständig partitioniert ist, wird für jede Ausgabepartition eine neue Datei erstellt.  
* Wenn eine Datei oder ein Container des Speicherkontos vom Benutzer gelöscht wird.  
* Wenn die Ausgabe zeitpartitioniert ist, indem das Pfadpräfixmuster verwendet wird, wird ein neues Blob verwendet, wenn die Abfrage zur nächsten Stunde wechselt.
* Wenn die Ausgabe über ein benutzerdefiniertes Feld partitioniert wird, wird pro Partitionsschlüssel ein neues Blob erstellt, falls es noch nicht vorhanden ist.
* Falls die Ausgabe über ein benutzerdefiniertes Feld partitioniert wird, für das die Kardinalität des Partitionsschlüssels den Wert 8000 übersteigt, kann pro Partitionsschlüssel ein neues Blob erstellt werden.

## <a name="event-hub"></a>Event Hub
Der Dienst [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) ist ein hoch skalierbarer Veröffentlichen-Abonnieren-Ereignisingestor. Er kann mehrere Millionen Ereignisse pro Sekunde erfassen. Eine Verwendungsmöglichkeit eines Event Hubs als Ausgabe ergibt sich, wenn die Ausgabe eines Stream Analytics-Auftrags zur Eingabe eines anderen Streamingauftrags wird.

Es gibt einige Parameter, die erforderlich sind, um Event Hub-Datenströme als Ausgabe zu konfigurieren.

| Eigenschaftenname | BESCHREIBUNG |
| --- | --- |
| Ausgabealias | Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an diesen Event Hub weiterzuleiten. |
| Event Hub-Namespace |Ein Event Hub-Namespace ist ein Container für einen Satz mit Nachrichtenentitäten. Sie haben bei der Erstellung eines neuen Event Hubs auch einen Event Hub-Namespace erstellt. |
| Event Hub-Name | Der Name Ihrer Event Hub-Ausgabe. |
| Event Hub-Richtlinienname | Die Richtlinie für den gemeinsamen Zugriff, die auf der Registerkarte "Event Hub-Konfiguration" erstellt werden kann. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. |
| Event Hub-Richtlinienschlüssel | Der Schlüssel für den gemeinsamen Zugriff, der für die Authentifizierung des Zugriffs auf den Event Hub-Namespace verwendet wird. |
| Partitionsschlüsselspalte [optional] | Diese Spalte enthält den Partitionsschlüssel für die Event Hub-Ausgabe. |
| Ereignisserialisierungsformat | Das Serialisierungsformat für Ausgabedaten.  Es werden JSON, CSV und Avro unterstützt. |
| Codieren | Bei CSV und JSON ist UTF-8 gegenwärtig das einzige unterstützte Codierungsformat. |
| Trennzeichen | Gilt nur für die CSV-Serialisierung. Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen zum Serialisieren der Daten im CSV-Format. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Tabstopp und senkrechter Strich. |
| Format | Gilt nur für die JSON-Serialisierung. „Separate Zeile“ gibt an, dass die Ausgabe so formatiert wird, dass jedes JSON-Objekt in einer neuen Zeile enthalten ist. „Array“ gibt an, dass die Ausgabe als Array aus JSON-Objekten formatiert wird. Dieses Array wird nur geschlossen, wenn der Auftrag beendet wird oder Stream Analytics mit dem nächsten Zeitfenster fortfährt. Im Allgemeinen ist es besser, in separaten Zeilen geschriebenen JSON-Code zu verwenden, da er keine spezielle Behandlung erfordert, während noch in die Ausgabedatei geschrieben wird. |

## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/) kann als Ausgabe für einen Stream Analytics-Auftrag verwendet werden, um eine umfassende Visualisierungsumgebung für die Analyseergebnisse bereitzustellen. Diese Funktionalität kann für Vorgangsdashboards, die Erstellung von Berichten und eine metrikgesteuerte Berichterstellung verwendet werden.

Die Power BI-Ausgabe aus Stream Analytics ist zurzeit nicht in den Regionen Azure China (21Vianet) und Azure Deutschland (T-Systems International) verfügbar.

### <a name="authorize-a-power-bi-account"></a>Autorisieren eines Power BI-Kontos
1. Wenn Power BI im Azure-Portal als Ausgabe ausgewählt ist, werden Sie aufgefordert, einen vorhandenen Power BI-Benutzer zu autorisieren oder ein neues Power BI-Konto zu erstellen.  
   
   ![Autorisieren von Power BI-Benutzern](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  

2. Erstellen Sie ein neues Konto, wenn Sie noch keines haben, und klicken Sie dann auf "Jetzt autorisieren".  Die folgende Seite wird angezeigt:
   
   ![Azure-Konto Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  

3. In diesem Schritt geben Sie das Geschäfts- oder Schulkonto für die Autorisierung der Power BI-Ausgabe an. Wenn Sie noch nicht für Power BI angemeldet sind, wählen Sie "Jetzt anmelden" aus. Das für Power BI verwendete Geschäfts- Schul- oder Unikonto kann sich vom Azure-Abonnementkonto unterscheiden, bei dem Sie zurzeit angemeldet sind.

### <a name="configure-the-power-bi-output-properties"></a>Konfigurieren der Eigenschaften der Power BI-Ausgabe
Sobald Sie das Power BI-Konto authentifiziert haben, können Sie die Eigenschaften für die Power BI-Ausgabe konfigurieren. Die folgende Tabelle enthält eine Liste von Eigenschaftennamen und der entsprechenden Beschreibung zum Konfigurieren der Power BI-Ausgabe.

| Eigenschaftenname | Beschreibung |
| --- | --- |
| Ausgabealias |Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an diese Power BI-Ausgabe weiterzuleiten. |
| Gruppenarbeitsbereich |Um die gemeinsame Datennutzung mit anderen Power BI-Benutzern zu ermöglichen, können Sie Gruppen in Ihrem Power BI-Konto auswählen. Wählen Sie alternativ „Mein Arbeitsbereich“, wenn Sie nicht in eine Gruppe schreiben möchten.  Zum Aktualisieren einer vorhandenen Gruppe muss die Power BI-Authentifizierung erneuert werden. |
| Datasetname |Geben Sie einen Datasetnamen an, der für die Power BI-Ausgabe verwendet werden soll. |
| Tabellenname |Geben Sie einen Tabellennamen unter dem Dataset der Power BI-Ausgabe ein. Derzeit darf die Power BI-Ausgabe von Stream Analytics-Aufträgen nur eine Tabelle pro Dataset aufweisen. |

Eine Schritt-für-Schritt-Anleitung zum Konfigurieren einer Power BI-Ausgabe und eines Power BI-Dashboards erhalten Sie im Artikel [Stream Analytics und Power BI: Ein Dashboard mit Echtzeitanalyse von Streamingdaten](stream-analytics-power-bi-dashboard.md).

> [!NOTE]
> Erstellen Sie das Dataset und die Tabelle nicht explizit im Power BI-Dashboard. Das Dataset und die Tabelle werden automatisch mit Daten aufgefüllt, wenn der Auftrag gestartet wird und damit beginnt, Ausgaben an Power BI weiterzuleiten. Wenn die Auftragsabfrage keine Ergebnisse generiert, werden das Dataset und die Tabelle nicht erstellt. Beachten Sie Folgendes: Wenn Power BI bereits über ein Dataset und eine Tabelle mit demselben Namen verfügt, der in diesem Stream Analytics-Auftrag angegeben wurde, werden die vorhandenen Daten überschrieben.
> 

### <a name="schema-creation"></a>Schemaerstellung
Azure Stream Analytics erstellt ein Power BI-Dataset und eine Tabelle im Namen des Benutzers (sofern noch nicht vorhanden). In allen anderen Fällen wird die Tabelle mit neuen Werten aktualisiert. Zurzeit besteht eine Beschränkung, dass nur eine Tabelle innerhalb eines Datasets vorhanden sein kann.

### <a name="data-type-conversion-from-stream-analytics-to-power-bi"></a>Datentypkonvertierung aus Stream Analytics in Power BI
Azure Stream Analytics aktualisiert das Datenmodell dynamisch zur Laufzeit, wenn sich das Ausgabeschema ändert. Nachverfolgt werden Änderungen an Spaltennamen und -typen sowie das Hinzufügen/Entfernen von Spalten.

Die folgende Tabelle enthält die Datentypkonvertierungen von [Stream Analytics-Datentypen](https://msdn.microsoft.com/library/azure/dn835065.aspx) in [EDM-Typen (Entity Data Model)](https://powerbi.microsoft.com/documentation/powerbi-developer-walkthrough-push-data/) von Power BI für den Fall, dass noch kein POWER BI-Dataset und noch keine Tabelle vorhanden sind.

Quelle: Stream Analytics | Ziel: Power BI
-----|-----|------------
bigint | Int64
nvarchar(max) | Zeichenfolge
Datetime | DateTime
Gleitkommawert | Double
Datensatzarray | Zeichenfolgentyp, Konstantenwert „IRecord“ oder „IArray“

### <a name="schema-update"></a>Schemaaktualisierung
Stream Analytics leitet das Datenmodellschema vom ersten Ereignissatz in der Ausgabe ab. Später wird das Datenmodellschema bei Bedarf aktualisiert, um eingehende Ereignisse zu berücksichtigen, die unter Umständen nicht in das ursprüngliche Schema passen.

Die `SELECT *`-Abfrage sollte nicht verwendet werden, um zeilenübergreifende dynamische Schemaaktualisierungen zu verhindern. Neben einer möglichen Beeinträchtigung der Leistung ist möglicherweise auch der Zeitaufwand für die Ergebnisse ungewiss. Es empfiehlt sich, die genauen Felder auszuwählen, die auf dem Power BI-Dashboard angezeigt werden sollen. Außerdem müssen die Datenwerte mit dem ausgewählten Datentyp kompatibel sein.


Vorher/Aktuell | Int64 | Zeichenfolge | DateTime | Double
-----------------|-------|--------|----------|-------
Int64 | Int64 | Zeichenfolge | Zeichenfolge | Double
Double | Double | Zeichenfolge | Zeichenfolge | Double
Zeichenfolge | Zeichenfolge | Zeichenfolge | Zeichenfolge |  | Zeichenfolge | 
DateTime | Zeichenfolge | Zeichenfolge |  DateTime | Zeichenfolge


### <a name="renew-power-bi-authorization"></a>Erneuern der Power BI-Autorisierung
Wenn sich das Kennwort Ihres Power BI-Kontos ändert, nachdem der Stream Analytics-Auftrag erstellt oder zuletzt authentifiziert wurde, müssen Sie Stream Analytics erneut authentifizieren. Wenn Multi-Factor Authentication (MFA) für Ihren AAD-Mandanten (Azure Active Directory) konfiguriert ist, müssen Sie auch die Power BI-Autorisierung alle zwei Wochen erneuern. Dieses Problem zeigt sich daran, dass keine Auftragsausgabe erfolgt und in den Vorgangsprotokollen ein Benutzerauthentifizierungsfehler angezeigt wird:

  ![Power BI-Aktualisierungstoken-Fehler](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

Um dieses Problem zu beheben, halten Sie den laufenden Auftrag an, und wechseln Sie zur Power BI-Ausgabe.  Wählen Sie den Link **Autorisierung erneuern**, und starten Sie den Auftrag ab **Letzte Beendigungszeit** neu, um Datenverlust zu vermeiden.

  ![Power BI erneuert die Autorisierung](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Table Storage
[Azure Table Storage](../storage/common/storage-introduction.md) bietet einen hoch verfügbaren, in hohem Maße skalierbaren Speicher, sodass eine Anwendung automatisch an die Bedürfnisse der Benutzer angepasst werden kann. Tabellenspeicher ist Microsofts NoSQL-Schlüssel-/Attributspeicher, der für strukturierte Daten genutzt werden kann, die weniger Einschränkungen hinsichtlich des Schemas aufweisen. Azure-Tabellenspeicher kann zum Speichern von Daten für dauerhafte Archivierung und effizienten Abruf verwendet werden.

Die folgende Tabelle enthält die Eigenschaftennamen und die entsprechenden Beschreibungen zum Erstellen einer Tabellenausgabe.

| Eigenschaftenname | Beschreibung |
| --- | --- |
| Ausgabealias |Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an diesen Tabellenspeicher weiterzuleiten. |
| Speicherkonto |Der Name des Speicherkontos, an das Sie die Ausgabe senden. |
| Speicherkontoschlüssel |Der Zugriffsschlüssel, der dem Speicherkonto zugeordnet ist. |
| Tabellenname |Der Name der Tabelle. Die Tabelle wird erstellt, wenn sie nicht vorhanden ist. |
| Partitionsschlüssel |Der Name der Ausgabespalte mit dem Partitionsschlüssel. Der Partitionsschlüssel ist ein eindeutiger Bezeichner für die Partition innerhalb einer Tabelle, die den ersten Teil des Primärschlüssels einer Entität bildet. Dabei handelt es sich um einen Zeichenfolgenwert, der bis zu 1 KB groß sein kann. |
| Zeilenschlüssel |Der Name der Ausgabespalte, die den Zeilenschlüssel enthält. Der Zeilenschlüssel ist ein eindeutiger Bezeichner für eine Entität innerhalb einer Partition. Sie bildet den zweiten Teil des Primärschlüssels einer Entität. Der Zeilenschlüssel ist ein Zeichenfolgenwert, der bis zu 1 KB groß sein kann. |
| Batchgröße |Dies ist die Anzahl von Datensätzen für einen Batchvorgang. Der Standardwert (100) ist für die meisten Aufträge ausreichend. Weitere Details zur Änderung dieser Einstellung finden Sie in der [TableBatchOperation-Spezifikation](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx). |
 
## <a name="service-bus-queues"></a>Service Bus-Warteschlangen
[Service Bus-Warteschlangen](https://msdn.microsoft.com/library/azure/hh367516.aspx) bieten eine FIFO-Nachrichtenzustellung (First In, First Out) an einen Consumer oder an mehrere konkurrierende Consumer. Typischerweise wird erwartet, dass Nachrichten in der zeitlichen Reihenfolge von den Consumern empfangen und verarbeitet werden, in der sie der Warteschlange hinzugefügt wurden, und jede Nachricht wird nur von einem Nachrichtenconsumer empfangen und verarbeitet.

Die folgende Tabelle enthält die Eigenschaftennamen und die entsprechenden Beschreibungen zum Erstellen einer Warteschlangenausgabe.

| Eigenschaftenname | Beschreibung |
| --- | --- |
| Ausgabealias |Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an diese Service Bus-Warteschlange weiterzuleiten. |
| Service Bus-Namespace |Ein Service Bus-Namespace ist ein Container für einen Satz von Nachrichtenentitäten. |
| Warteschlangenname |Der Name der Service Bus-Warteschlange. |
| Name der Warteschlangenrichtlinie |Beim Erstellen einer Warteschlange können Sie auf der Registerkarte "Warteschlange konfigurieren" Richtlinien für den gemeinsamen Zugriff erstellen. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. |
| Schlüssel der Warteschlangenrichtlinie |Der Schlüssel für den gemeinsamen Zugriff, der für die Authentifizierung des Zugriffs auf den Service Bus-Namespace verwendet wird. |
| Ereignisserialisierungsformat |Das Serialisierungsformat für Ausgabedaten.  Es werden JSON, CSV und Avro unterstützt. |
| Codieren |Bei CSV und JSON ist UTF-8 gegenwärtig das einzige unterstützte Codierungsformat. |
| Trennzeichen |Gilt nur für die CSV-Serialisierung. Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen zum Serialisieren der Daten im CSV-Format. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Tabstopp und senkrechter Strich. |
| Format |Gilt nur für den JSON-Typ. „Separate Zeile“ gibt an, dass die Ausgabe so formatiert wird, dass jedes JSON-Objekt in einer neuen Zeile enthalten ist. „Array“ gibt an, dass die Ausgabe als Array aus JSON-Objekten formatiert wird. |

Die Anzahl der Partitionen [basiert auf der Service Bus-SKU und -Größe](../service-bus-messaging/service-bus-partitioning.md). Ein Partitionsschlüssel gibt einen eindeutigen ganzzahligen Wert für jede Partition an.

## <a name="service-bus-topics"></a>Service Bus-Themen
Während Service Bus-Warteschlangen eine 1:1-Kommunikationsmethode vom Absender zum Empfänger bereitstellen, bieten [Service Bus-Themen](https://msdn.microsoft.com/library/azure/hh367516.aspx) eine 1:n-Form der Kommunikation.

Die folgende Tabelle enthält die Eigenschaftennamen und die entsprechenden Beschreibungen zum Erstellen einer Tabellenausgabe.

| Eigenschaftenname | BESCHREIBUNG |
| --- | --- |
| Ausgabealias |Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an dieses Service Bus-Thema weiterzuleiten. |
| Service Bus-Namespace |Ein Service Bus-Namespace ist ein Container für einen Satz von Nachrichtenentitäten. Sie haben bei der Erstellung eines neuen Event Hubs auch einen Service Bus-Namespace erstellt. |
| Themenname |Themen sind Messagingentitäten, vergleichbar mit Event Hubs und Warteschlangen. Sie wurden für die Erfassung von Ereignisstreams von einer Reihe unterschiedlicher Geräte und Dienste entwickelt. Wenn ein Thema erstellt wird, wird ihm auch ein bestimmter Name zugewiesen. Die an ein Thema gesendeten Nachrichten sind nur verfügbar, wenn ein Abonnement erstellt wurde. Stellen Sie daher sicher, dass es mindestens ein Abonnement unter dem Thema gibt. |
| Name der Themenrichtlinie |Beim Erstellen eines Themas können Sie auf der Registerkarte „Thema konfigurieren“ Richtlinien für den gemeinsamen Zugriff erstellen. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. |
| Schlüssel der Themenrichtlinie |Der Schlüssel für den gemeinsamen Zugriff, der für die Authentifizierung des Zugriffs auf den Service Bus-Namespace verwendet wird. |
| Ereignisserialisierungsformat |Das Serialisierungsformat für Ausgabedaten.  Es werden JSON, CSV und Avro unterstützt. |
| Codieren |Bei Verwendung des CSV- oder JSON-Formats muss eine Codierung angegeben werden. Das einzige derzeit unterstützte Codierungsformat ist UTF-8. |
| Trennzeichen |Gilt nur für die CSV-Serialisierung. Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen zum Serialisieren der Daten im CSV-Format. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Tabstopp und senkrechter Strich. |

Die Anzahl der Partitionen [basiert auf der Service Bus-SKU und -Größe](../service-bus-messaging/service-bus-partitioning.md). Ein Partitionsschlüssel gibt einen eindeutigen ganzzahligen Wert für jede Partition an.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) ist ein global verteilter Datenbankdienst mit mehreren Modellen, der eine grenzenlose elastische Skalierung rund um den Globus, umfangreiche Abfragen und automatische Indizierung über schemaunabhängige Datenmodelle, garantiert niedrige Latenz und branchenführende umfassende SLAs bietet. Weitere Informationen zu den Cosmos DB-Sammlungsoptionen für Stream Analytics finden Sie im Artikel [Azure Stream Analytics-Ausgabe an Cosmos DB](stream-analytics-documentdb-output.md).

Die Azure Cosmos DB-Ausgabe aus Stream Analytics ist zurzeit nicht in den Regionen Azure China (21Vianet) und Azure Deutschland (T-Systems International) verfügbar.

> [!Note]
> Derzeit unterstützt Azure Stream Analytics die Verbindung mit Cosmos DB nur über die **SQL-API**.
> Andere Azure Cosmos DB-APIs werden noch nicht unterstützt. Wenn Sie Azure Stream Analytics auf die mit anderen APIs erstellten Azure Cosmos DB-Konten verweisen, werden die Daten unter Umständen nicht richtig gespeichert. 

In der folgenden Tabelle werden die Eigenschaften zum Erstellen einer Azure Cosmos DB-Ausgabe beschrieben.
| Eigenschaftenname | Beschreibung |
| --- | --- |
| Ausgabealias | Ein Alias zum Verweisen auf diese Ausgabe in Ihrer Stream Analytics-Abfrage. |
| Senke | Cosmos DB |
| Importoption | Wählen Sie entweder „Cosmos DB über das eigene Abonnement auswählen“ oder „Cosmos DB-Einstellungen manuell festlegen“ aus.
| Konto-ID | Der Name oder Endpunkt-URI des Cosmos DB-Kontos. |
| Kontoschlüssel | Der Schlüssel für den gemeinsamen Zugriff für das Cosmos DB-Konto. |
| Datenbank | Der Name der Cosmos DB-Datenbank. |
| Muster des Sammlungsnamen | Der Sammlungsname oder die jeweiligen Muster für die zu verwendenden Sammlungen. <br/>Das Sammlungsnamenformat kann mit dem optionalen Token {partition} gebildet werden, wobei Partitionen bei 0 beginnen. Zwei Beispiele:  <br/>1. _MyCollection_ – Eine Sammlung mit dem Namen „MyCollection“ muss vorhanden sein.  <br/>2. _MyCollection {partition}_ – Basiert auf der Partitionierungsspalte. <br/>Die Sammlungen für die Partitionierungsspalte müssen vorhanden sein: „MyCollection0“, „MyCollection1“, „MyCollection2“ usw. |
| Partitionsschlüssel | Optional. Nur erforderlich, wenn Sie im Muster Ihres Sammlungsnamens ein {partition}-Token verwenden.<br/> Der Partitionierungsschlüssel ist der Name des Felds in Ausgabeereignissen, das zur Angabe des Schlüssels für die sammlungsübergreifende Partitionierung der Ausgabe verwendet wird.<br/> Für die Ausgabe einer einzelnen Sammlung kann eine beliebige Ausgabespalte verwendet werden. Beispiel: PartitionId. |
| Dokument-ID |Optional. Der Name des Felds in Ausgabeereignissen, das zur Angabe des Primärschlüssels verwendet wird, auf dem Einfüge- und Aktualisierungsvorgänge basieren.  

## <a name="azure-functions"></a>Azure-Funktionen
Azure Functions ist ein serverloser Computedienst, mit dem Sie Code bedarfsgesteuert ausführen können, ohne eine explizite Infrastruktur bereitstellen oder verwalten zu müssen. Mit diesem Dienst können Sie Codes implementieren, die durch in Azure- oder Drittanbieterdiensten auftretende Ereignisse ausgelöst werden.  Aufgrund der Möglichkeit, auf Trigger zu antworten, ist Azure Functions die ideale Ausgabe für Azure Stream Analytics. Mithilfe dieses Ausgabeadapters können Benutzer eine Verbindung zwischen Stream Analytics und Azure Functions herstellen und als Reaktion auf verschiedenste Ereignisse ein Skript oder einen Codeausschnitt ausführen.

Die Azure Functions-Ausgabe aus Stream Analytics ist zurzeit nicht in den Regionen Azure China (21Vianet) und Azure Deutschland (T-Systems International) verfügbar.

Azure Stream Analytics ruft Azure Functions über HTTP-Trigger auf. Der neue Azure Functions-Ausgabeadapter wird mit folgenden konfigurierbaren Eigenschaften zur Verfügung gestellt:

| Eigenschaftenname | Beschreibung |
| --- | --- |
| Funktionen-App |Name der Azure Functions-App |
| Funktion |Name der Funktion in der Azure Functions-App |
| Schlüssel |Wenn Sie eine Azure-Funktion aus einem anderen Abonnement verwenden möchten, können Sie dies tun, indem Sie den Schlüssel für den Zugriff auf Ihre Funktion angeben. |
| Max Batch Size |Mit dieser Eigenschaft kann die maximale Größe für jeden Ausgabebatch festgelegt werden, der an Ihre Azure Functions-Instanz gesendet wird. Standardmäßig ist dieser Wert auf 256 KB festgelegt. |
| Max Batch Count  |Wie der Name schon sagt, können Sie mit dieser Eigenschaft die maximale Anzahl von Ereignissen in jedem Batch angeben, der an Azure Functions gesendet wird. Der Standardwert für „Max Batch Count“ ist 100. |

Die Größe der an Azure Functions gesendeten Batches wird verringert, wenn in Azure Stream Analytics die Ausnahme 413 (HTTP-Anforderungseinheit zu groß) von Azure Functions eingeht. Verwenden Sie in Ihrem Azure-Funktionscode diese Ausnahme, um sicherzustellen, dass Azure Stream Analytics keine übermäßig großen Batches sendet. Stellen Sie außerdem sicher, dass die Werte für die maximal zulässige Batchanzahl und -größe in der Funktion mit den Werten übereinstimmen, die im Stream Analytics-Portal eingegeben wurden. 

Zudem wird in Fällen, in denen kein Ereignis in einem Zeitfenster auftritt, keine Ausgabe generiert. Als Ergebnis wird die computeResult-Funktion nicht aufgerufen. Dieses Verhalten entspricht den integrierten Aggregatfunktionen im Fenstermodus.

## <a name="partitioning"></a>Partitionierung

In der folgenden Tabelle werden die Partitionsunterstützung und die Anzahl der Ausgabeschreiber für die einzelnen Ausgabetypen zusammengefasst:

| Ausgabetyp | Unterstützung der Partitionierung | Partitionsschlüssel  | Anzahl der Ausgabeschreiber | 
| --- | --- | --- | --- |
| Azure Data Lake Store | JA | Verwenden Sie die {date}- und {time}-Tokens im Pfadpräfixmuster. Wählen Sie ein Datumsformat wie JJJJ/MM/TT, TT/MM/JJJJ oder MM-TT-JJJJ. „HH“ wird für das Uhrzeitformat verwendet. | Hierbei wird die Eingabepartitionierung für [vollständig parallelisierbare Abfragen](stream-analytics-scale-jobs.md) befolgt. | 
| Azure SQL-Datenbank | Nein  | Keine | Nicht zutreffend | 
| Azure Blob Storage | JA | Verwenden Sie die Token {date} und {time} aus Ihren Ereignisfeldern im Pfadmuster. Wählen Sie ein Datumsformat wie JJJJ/MM/TT, TT/MM/JJJJ oder MM-TT-JJJJ. „HH“ wird für das Uhrzeitformat verwendet. Im Rahmen der [Vorschauversion](https://aka.ms/ASAPreview) kann die Blobausgabe durch ein einzelnes benutzerdefiniertes Ereignisattribut {fieldname} oder {datetime:\<Spezifizierer>} partitioniert werden. | Hierbei wird die Eingabepartitionierung für [vollständig parallelisierbare Abfragen](stream-analytics-scale-jobs.md) befolgt. | 
| Azure Event Hub | JA | JA | Variiert je nach Partitionsausrichtung.</br> Wenn der Event Hub-Ausgabepartitionsschlüssel entsprechend am vorgeschalteten (vorherigen) Abfrageschritt ausgerichtet ist, ist die Anzahl von Schreibern mit der Anzahl von Event Hub-Ausgabepartitionen identisch. Jeder Schreiber verwendet die [EventHubSender-Klasse](/dotnet/api/microsoft.servicebus.messaging.eventhubsender?view=azure-dotnet) des EventHub-Elements, um Ereignisse an die jeweilige Partition zu senden. </br> Wenn der Event Hub-Ausgabepartitionsschlüssel nicht am vorgeschalteten (vorherigen) Abfrageschritt ausgerichtet ist, ist die Anzahl von Schreibern mit der Anzahl von Partitionen in diesem vorherigen Schritt identisch. Für jeden Schreiber wird die [SendBatchAsync-Klasse](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.eventhubclient.sendasync?view=azure-dotnet) des EventHubClient-Elements verwendet, um Ereignisse an alle Ausgabepartitionen zu senden. |
| Power BI | Nein  | Keine | Nicht zutreffend | 
| Azure-Tabellenspeicher | JA | Eine beliebige Ausgabespalte.  | Hierbei wird die Eingabepartitionierung für [vollständig parallelisierte Abfragen](stream-analytics-scale-jobs.md) befolgt. | 
| Azure Service Bus-Thema | JA | Wird automatisch ausgewählt. Die Anzahl der Partitionen basiert auf der [Service Bus-SKU und -Größe](../service-bus-messaging/service-bus-partitioning.md). Ein Partitionsschlüssel gibt einen eindeutigen ganzzahligen Wert für jede Partition an.| Entspricht der Anzahl von Partitionen im Ausgabethema.  |
| Azure Service Bus-Warteschlange | JA | Wird automatisch ausgewählt. Die Anzahl der Partitionen basiert auf der [Service Bus-SKU und -Größe](../service-bus-messaging/service-bus-partitioning.md). Ein Partitionsschlüssel gibt einen eindeutigen ganzzahligen Wert für jede Partition an.| Entspricht der Anzahl von Partitionen in der Ausgabewarteschlange. |
| Azure Cosmos DB | JA | Verwenden Sie das {partition}-Token im Sammlungsnamensmuster. Der {partition}-Wert basiert auf der PARTITION BY-Klausel in der Abfrage. | Hierbei wird die Eingabepartitionierung für [vollständig parallelisierte Abfragen](stream-analytics-scale-jobs.md) befolgt. |
| Azure-Funktionen | Nein  | Keine | Nicht zutreffend | 

## <a name="output-batch-size"></a>Ausgabebatchgröße
Für Azure Stream Analytics werden Batches mit variabler Größe verwendet, um Ereignisse zu verarbeiten und in Ausgaben zu schreiben. Normalerweise schreibt das Stream Analytics-Modul nicht nur jeweils eine Nachricht, sondern Batches, um die Effizienz zu steigern. Wenn sowohl die Rate an eingehenden als auch an ausgehenden Ereignissen hoch ist, werden größere Batches verwendet. Falls die Ausgangsrate niedrig ist, werden kleinere Batches genutzt, um die Latenz gering zu halten. 

In der folgenden Tabelle sind einige Aspekte von Ausgabebatches beschrieben:

| Ausgabetyp | Maximale Nachrichtengröße | Optimierung der Batchgröße |
| :--- | :--- | :--- | 
| Azure Data Lake Store | Siehe [Grenzwerte für Data Lake Store](../azure-subscription-service-limits.md#data-lake-store-limits) | Bis zu 4 MB pro Schreibvorgang |
| Azure SQL-Datenbank | Max. 10.000 Zeilen pro Masseneinfügen-Vorgang</br>Min. 100 Zeilen pro Masseneinfügen-Vorgang </br>Siehe auch [Einschränkungen des DTU-basierten Ressourcenmodells für Azure SQL-Datenbank](../sql-database/sql-database-resource-limits.md) |  Für jeden Batch wird anfänglich das Masseneinfügen mit der maximalen Batchgröße durchgeführt, und der Batch kann (bis zur Mindestbatchgröße) basierend auf wiederholbaren Fehlern aus SQL halbiert werden. |
| Azure Blob Storage | Siehe [Speichergrenzwerte](../azure-subscription-service-limits.md#storage-limits) | Maximale Größe von Blobblöcken beträgt 4 MB</br>Maximale Anzahl von Blobblöcken beträgt 50.000 |
| Azure Event Hub   | 256 KB pro Nachricht </br>Siehe auch [Event Hubs-Kontingente](../event-hubs/event-hubs-quotas.md) |    Wenn die E/A-Partitionierung nicht aneinander ausgerichtet ist, wird jedes Ereignis einzeln in einem EventData-Element verpackt und als Batch gesendet, dessen Größe bis zur maximalen Nachrichtengröße reichen kann (1 MB für Premium-SKU). </br></br>  Falls die E/A-Partitionierung aneinander ausgerichtet ist, werden mehrere Ereignisse in einem einzelnen EventData-Element verpackt, das bis zur maximalen Nachrichtengröße reichen kann, und gesendet.    |
| Power BI | Siehe [Einschränkungen für Power BI-REST-API](https://msdn.microsoft.com/library/dn950053.aspx) |
| Azure-Tabellenspeicher | Siehe [Speichergrenzwerte](../azure-subscription-service-limits.md#storage-limits) | Der Standardwert beträgt 100 Entitäten pro Einzeltransaktion. Bei Bedarf kann auch ein niedrigerer Wert konfiguriert werden. |
| Azure Service Bus-Warteschlange   | 256 KB pro Nachricht</br> Siehe auch [Service Bus-Kontingente](../service-bus-messaging/service-bus-quotas.md) | Einzelnes Ereignis pro Nachricht |
| Azure Service Bus-Thema | 256 KB pro Nachricht</br> Siehe auch [Service Bus-Kontingente](../service-bus-messaging/service-bus-quotas.md) | Einzelnes Ereignis pro Nachricht |
| Azure Cosmos DB   | Siehe [Einschränkungen für Azure Cosmos DB](../azure-subscription-service-limits.md#azure-cosmos-db-limits) | Die Batchgröße und Schreibfrequenz wird basierend auf den CosmosDB-Antworten dynamisch angepasst. </br> Es gelten keine vordefinierten Stream Analytics-Einschränkungen. |
| Azure-Funktionen   | | Die Standardbatchgröße beträgt 246 KB. </br> Die Standardereignisanzahl pro Batch beträgt 100. </br> Die Batchgröße ist konfigurierbar und kann in den [Ausgabeoptionen](#azure-functions) von Stream Analytics erhöht oder verringert werden. 

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Schnellstart: Erstellen eines Stream Analytics-Auftrags mithilfe des Azure-Portals](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
