---
title: Ausgabetypen von Azure Stream Analytics-Aufträgen
description: Erfahren Sie, wie Sie Optionen für Stream Analytics-Datenausgaben einschließlich Power BI für Analyseergebnisse einsetzen.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/18/2017
ms.openlocfilehash: afaadc12d056f42a75795073d480fe26757649d8
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="stream-analytics-outputs-options-for-storage-and-analysis"></a>Stream Analytics-Ausgaben: Optionen für Speicher und Analyse
Überlegen Sie beim Erstellen eines Stream Analytics-Auftrags, wie die sich ergebenden Daten genutzt werden. Wie können Sie die Ergebnisse des Stream Analytics-Auftrags anzeigen, und wo können Sie sie speichern?

Um verschiedene Anwendungsmuster zu ermöglichen, stellt Azure Stream Analytics verschiedene Optionen zum Speichern der Ausgabe und zum Anzeigen von Analyseergebnissen bereit. Dadurch wird sowohl einfaches Anzeigen der Auftragsausgabe als auch Flexibilität bei der Nutzung und Speicherung der Auftragsausgabe für Data Warehousing und andere Zwecke erreicht. Jede Ausgabe, die im Auftrag konfiguriert wird, muss vorhanden sein, bevor der Auftrag gestartet wird und Ereignisse übertragen werden. Wird beispielsweise Blob Storage als Ausgabe verwendet, erstellt der Auftrag nicht automatisch ein Speicherkonto. Erstellen Sie ein Speicherkonto, bevor der Stream Analytics-Auftrag gestartet wird.

## <a name="azure-data-lake-store"></a>Azure Data Lake Store
Stream Analytics unterstützt [Azure Data Lake-Speicher](https://azure.microsoft.com/services/data-lake-store/). Dieser Speicher bietet Ihnen die Möglichkeit, Daten von beliebiger Größe, Art und Erfassungsgeschwindigkeit zur Durchführung operativer und explorativer Analysen zu speichern. Darüber hinaus muss Stream Analytics autorisiert werden, um auf Data Lake-Speicher zuzugreifen. Informationen zur Autorisierung sowie zur Anmeldung für Data Lake Store (falls erforderlich) finden Sie im Artikel zur [Data Lake-Ausgabe](stream-analytics-data-lake-output.md).

### <a name="authorize-an-azure-data-lake-store"></a>Autorisieren eines Azure Data Lake Store
Wenn Data Lake Store im Azure-Portal als Ausgabe ausgewählt ist, werden Sie aufgefordert, eine Verbindung mit einem vorhandenen Data Lake Store zu autorisieren.  

![Autorisieren von Data Lake-Speicher](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

Füllen Sie dann die Eigenschaften für die Data Lake Store-Ausgabe wie unten dargestellt aus:

![Autorisieren von Data Lake-Speicher](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

Die folgende Tabelle enthält die Namen und Beschreibungen der Eigenschaften, die für die Erstellung einer Data Lake-Speicherausgabe erforderlich sind.

<table>
<tbody>
<tr>
<td><B>EIGENSCHAFTENNAME</B></td>
<td><B>BESCHREIBUNG</B></td>
</tr>
<tr>
<td>Ausgabealias</td>
<td>Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an diesen Data Lake Store weiterzuleiten.</td>
</tr>
<tr>
<td>Kontoname</td>
<td>Der Name des Data Lake-Speicherkontos, an das Sie die Ausgabe senden. Es wird eine Dropdownliste der Data Lake Store-Konten angezeigt, auf die der im Portal angemeldete Benutzer Zugriff besitzt.</td>
</tr>
<tr>
<td>Präfixmuster des Pfads</td>
<td>Die Dateibenennung erfolgt gemäß der nachstehenden Konvention: <BR>{Präfixmuster des Pfads}/schemaHashcode_Guid_Number.extension <BR> <BR>Beispielausgabedateien:<BR>Myoutput/20170901/00/45434_gguid_1.csv <BR>Myoutput/20170901/01/45434_gguid_1.csv <BR> <BR>Im Folgenden werden nun die Fälle vorgestellt, in denen eine neue Datei erstellt wird:<BR>1. Änderung im Ausgabeschema <BR>2. Externer oder interner Neustart eines Auftrags<BR><BR>Wenn das Dateipfadmuster darüber hinaus kein nachfolgendes Zeichen „/“ enthält, wird auch das letzte Muster im Dateipfad als Dateinamenpräfix behandelt.<BR><BR>Beispiel:<BR>Bei dem Pfadmuster „Ordner1/logs/HH“ kann die generierte Datei wie folgt aussehen: „Ordner1/logs/02_134343_gguid_1.csv“.</td>
</tr>
<tr>
<td>Datumsformat [<I>optional</I>]</td>
<td>Wenn das date-Token im Pfadpräfix verwendet wird, können Sie das Datumsformat auswählen, unter dem die Dateien gespeichert werden. Beispiel: YYYY/MM/TT</td>
</tr>
<tr>
<td>Zeitformat [<I>optional</I>]</td>
<td>Wenn das time-Token im Pfadpräfix verwendet wird, können Sie das Zeitformat auswählen, unter dem die Dateien gespeichert werden. Der einzige derzeit unterstützte Wert ist HH</td>
</tr>
<tr>
<td>Ereignisserialisierungsformat</td>
<td>Das Serialisierungsformat für Ausgabedaten. Es werden JSON, CSV und Avro unterstützt.</td>
</tr>
<tr>
<td>Codieren</td>
<td>Bei Verwendung des CSV- oder JSON-Formats muss eine Codierung angegeben werden. UTF-8 ist derzeit das einzige unterstützte Codierungsformat.</td>
</tr>
<tr>
<td>Trennzeichen</td>
<td>Gilt nur für die CSV-Serialisierung. Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen zum Serialisieren der CSV-Daten. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Tabstopp und senkrechter Strich.</td>
</tr>
<tr>
<td>Format</td>
<td>Gilt nur für die JSON-Serialisierung. „Separate Zeile“ gibt an, dass die Ausgabe so formatiert wird, dass jedes JSON-Objekt in einer neuen Zeile enthalten ist. „Array“ gibt an, dass die Ausgabe als Array aus JSON-Objekten formatiert wird. Dieses Array wird nur geschlossen, wenn der Auftrag beendet wird oder Stream Analytics mit dem nächsten Zeitfenster fortfährt. Im Allgemeinen ist es besser, in separaten Zeilen geschriebenen JSON-Code zu verwenden, da er keine spezielle Behandlung erfordert, während noch in die Ausgabedatei geschrieben wird.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Erneuern der Data Lake-Speicherautorisierung
Sie müssen Ihr Data Lake Store-Konto erneut authentifizieren, wenn das Kennwort seit der Erstellung oder letzten Authentifizierung Ihres Auftrags geändert wurde.

![Autorisieren von Data Lake-Speicher](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  

## <a name="sql-database"></a>SQL-Datenbank
[Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) kann als Ausgabe für relationale Daten oder für Anwendungen verwendet werden, die auf Inhalten aufsetzen, die in einer relationalen Datenbank gehostet werden. Stream Analytics-Aufträge schreiben in eine vorhandene Tabelle in einer Azure SQL-Datenbank.  Das Tabellenschema muss genau mit den Feldern und ihren Typen übereinstimmen, die aus Ihrem Auftrag ausgegeben werden. Ein [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) kann auch über die SQL-Datenbank-Ausgabeoption als Ausgabe angegeben werden (diese Funktionen steht Ihnen in der Vorschau zur Verfügung). Die folgende Tabelle enthält die Eigenschaftennamen und die entsprechenden Beschreibungen zum Erstellen einer SQL-Datenbank-Ausgabe.

| Eigenschaftenname | BESCHREIBUNG |
| --- | --- |
| Ausgabealias |Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an diese Datenbank weiterzuleiten. |
| Datenbank |Der Name der Datenbank, an die Sie die Ausgabe senden. |
| Servername |Der Servername der SQL-Datenbank. |
| Username |Der Benutzername, der Schreibzugriff auf die Datenbank besitzt. |
| Password |Das Kennwort, um eine Verbindung zur Datenbank herzustellen. |
| Table |Der Name der Tabelle, in die die Ausgabe geschrieben wird. Für den Tabellennamen muss zwischen Groß-/Kleinschreibung unterschieden werden. Das Schema dieser Tabelle sollte genau mit der Anzahl Felder und ihrer Typen übereinstimmen, die von der Auftragsausgabe generiert werden. |

> [!NOTE]
> Zurzeit wird das Azure SQL-Datenbank-Angebot für eine Auftragsausgabe in Stream Analytics unterstützt. Eine Azure-VM mit SQL Server und angefügter Datenbank wird jedoch nicht unterstützt. Dies soll in zukünftigen Versionen geändert werden.
> 
> 

## <a name="blob-storage"></a>Blob Storage
Blobspeicher bietet eine kostengünstige und skalierbare Lösung zum Speichern von großen Mengen unstrukturierter Daten in der Cloud.  Eine Einführung in Azure Blob Storage und dessen Nutzung finden Sie in der Dokumentation unter [Verwenden von Blobs](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

Die folgende Tabelle enthält die Eigenschaftennamen und die entsprechenden Beschreibungen zum Erstellen einer Blobausgabe.

<table>
<tbody>
<tr>
<td>Eigenschaftenname</td>
<td>Beschreibung</td>
</tr>
<tr>
<td>Ausgabealias</td>
<td>Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an diesen Blobspeicher weiterzuleiten.</td>
</tr>
<tr>
<td>Speicherkonto</td>
<td>Der Name des Speicherkontos, an das Sie die Ausgabe senden.</td>
</tr>
<tr>
<td>Speicherkontoschlüssel</td>
<td>Der geheime Schlüssel, der dem Speicherkonto zugeordnet ist.</td>
</tr>
<tr>
<td>Speichercontainer</td>
<td>Container stellen eine logische Gruppierung für Blobs bereit, die im Microsoft Azure-Blobdienst gespeichert sind. Wenn Sie ein Blob in den Blobdienst hochladen, müssen Sie einen Container für das Blob angeben.</td>
</tr>
<tr>
<td>Präfixmusters des Pfads [optional]</td>
<td>Das Dateipfadmuster, mit dem Ihre Blobs im angegebenen Container geschrieben werden. <BR> In dem Pfadmuster können Sie mindestens eine Instanz der beiden folgenden Variablen verwenden, um die Frequenz anzugeben, in der Blobs geschrieben werden: <BR> {date}, {time} <BR> Beispiel 1: cluster1/logs/{date}/{time} <BR> Beispiel 2: cluster1/logs/{date} <BR> <BR> Die Dateibenennung erfolgt gemäß der nachstehenden Konvention: <BR> {Präfixmuster des Pfads}/schemaHashcode_Guid_Number.extension <BR> <BR> Beispielausgabedateien: <BR> Myoutput/20170901/00/45434_gguid_1.csv <BR> Myoutput/20170901/01/45434_gguid_1.csv <BR> <BR> Im Folgenden werden nun die Fälle vorgestellt, in denen eine neue Datei erstellt wird: <BR> 1. Überschreitung der maximal zulässigen Anzahl von Blöcken (derzeit 50.000) in der aktuellen Datei <BR> 2. Änderung im Ausgabeschema <BR> 3. Externer oder interner Neustart eines Auftrags  </td>
</tr>
<tr>
<td>Datumsformat [optional]</td>
<td>Wenn das date-Token im Pfadpräfix verwendet wird, können Sie das Datumsformat auswählen, unter dem die Dateien gespeichert werden. Beispiel: YYYY/MM/TT</td>
</tr>
<tr>
<td>Zeitformat [optional]</td>
<td>Wenn das time-Token im Pfadpräfix verwendet wird, können Sie das Zeitformat auswählen, unter dem die Dateien gespeichert werden. Der einzige derzeit unterstützte Wert ist HH</td>
</tr>
<tr>
<td>Ereignisserialisierungsformat</td>
<td>Das Serialisierungsformat für Ausgabedaten.  Es werden JSON, CSV und Avro unterstützt.</td>
</tr>
<tr>
<td>Codieren</td>
<td>Bei Verwendung des CSV- oder JSON-Formats muss eine Codierung angegeben werden. UTF-8 ist derzeit das einzige unterstützte Codierungsformat.</td>
</tr>
<tr>
<td>Trennzeichen</td>
<td>Gilt nur für die CSV-Serialisierung. Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen zum Serialisieren der CSV-Daten. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Tabstopp und senkrechter Strich.</td>
</tr>
<tr>
<td>Format</td>
<td>Gilt nur für die JSON-Serialisierung. „Separate Zeile“ gibt an, dass die Ausgabe so formatiert wird, dass jedes JSON-Objekt in einer neuen Zeile enthalten ist. „Array“ gibt an, dass die Ausgabe als Array aus JSON-Objekten formatiert wird. Dieses Array wird nur geschlossen, wenn der Auftrag beendet wird oder Stream Analytics mit dem nächsten Zeitfenster fortfährt. Im Allgemeinen ist es besser, in separaten Zeilen geschriebenen JSON-Code zu verwenden, da er keine spezielle Behandlung erfordert, während noch in die Ausgabedatei geschrieben wird.</td>
</tr>
</tbody>
</table>

Wenn Blobspeicher als Ausgabe verwendet wird, wird in den folgenden Fällen eine neue Datei im Blob erstellt:

* Wenn die Datei die maximale Anzahl der zulässigen Blöcke überschreitet (beachten Sie, dass die maximal zulässige Anzahl der Blöcke erreicht werden kann, ohne die maximal zulässige Blobgröße erreicht wird. Wenn die Ausgaberate z.B. hoch ist, können Sie mehr Bytes pro Block vorhanden sein, und die Datei ist größer. Wenn die Ausgaberate niedrig ist, weist jeder Block weniger Daten auf, und die Datei ist kleiner.)  
* Wenn es eine Schemaänderung in der Ausgabe gibt und das Ausgabeformat ein festes Schema erfordert (CSV und Avro).  
* Wenn ein Auftrag entweder extern oder intern neu gestartet wird.  
* Wenn die Abfrage vollständig partitioniert ist, wird für jede Ausgabepartition eine neue Datei erstellt.  
* Wenn eine Datei oder ein Container des Speicherkontos vom Benutzer gelöscht wird.  
* Wenn die Ausgabe zeitpartitioniert ist, indem das Pfadpräfixmuster verwendet wird, wird ein neues Blob verwendet, wenn die Abfrage zur nächsten Stunde wechselt.

## <a name="event-hub"></a>Event Hub
[Event Hubs](https://azure.microsoft.com/services/event-hubs/) ist ein hoch skalierbarer Veröffentlichen-Abonnieren-Ereigniserfasser. Er kann mehrere Millionen Ereignisse pro Sekunde erfassen. Eine Verwendungsmöglichkeit eines Event Hubs als Ausgabe ergibt sich, wenn die Ausgabe eines Stream Analytics-Auftrags zur Eingabe eines anderen Streamingauftrags wird.

Es gibt einige Parameter, die erforderlich sind, um Event Hub-Datenströme als Ausgabe zu konfigurieren.

| Eigenschaftenname | BESCHREIBUNG |
| --- | --- |
| Ausgabealias |Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an diesen Event Hub weiterzuleiten. |
| Service Bus- Namespace |Ein Service Bus-Namespace ist ein Container für einen Satz von Nachrichtenentitäten. Sie haben bei der Erstellung eines neuen Event Hubs auch einen Service Bus-Namespace erstellt. |
| Event Hub |Der Name Ihrer Event Hub-Ausgabe. |
| Event Hub-Richtlinienname |Die Richtlinie für den gemeinsamen Zugriff, die auf der Registerkarte "Event Hub-Konfiguration" erstellt werden kann. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. |
| Event Hub-Richtlinienschlüssel |Der Schlüssel für den gemeinsamen Zugriff, der für die Authentifizierung des Zugriffs auf den Service Bus-Namespace verwendet wird. |
| Partitionsschlüsselspalte [optional] |Diese Spalte enthält den Partitionsschlüssel für die Event Hub-Ausgabe. |
| Ereignisserialisierungsformat |Das Serialisierungsformat für Ausgabedaten.  Es werden JSON, CSV und Avro unterstützt. |
| Codieren |Bei CSV und JSON ist UTF-8 gegenwärtig das einzige unterstützte Codierungsformat. |
| Trennzeichen |Gilt nur für die CSV-Serialisierung. Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen zum Serialisieren der Daten im CSV-Format. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Tabstopp und senkrechter Strich. |
| Format |Gilt nur für die JSON-Serialisierung. „Separate Zeile“ gibt an, dass die Ausgabe so formatiert wird, dass jedes JSON-Objekt in einer neuen Zeile enthalten ist. „Array“ gibt an, dass die Ausgabe als Array aus JSON-Objekten formatiert wird. Dieses Array wird nur geschlossen, wenn der Auftrag beendet wird oder Stream Analytics mit dem nächsten Zeitfenster fortfährt. Im Allgemeinen ist es besser, in separaten Zeilen geschriebenen JSON-Code zu verwenden, da er keine spezielle Behandlung erfordert, während noch in die Ausgabedatei geschrieben wird. |

## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/) kann als Ausgabe für einen Stream Analytics-Auftrag verwendet werden, um eine umfassende Visualisierungsumgebung für die Analyseergebnisse bereitzustellen. Diese Funktionalität kann für Vorgangsdashboards, die Erstellung von Berichten und eine metrikgesteuerte Berichterstellung verwendet werden.

### <a name="authorize-a-power-bi-account"></a>Autorisieren eines Power BI-Kontos
1. Wenn Power BI im Azure-Portal als Ausgabe ausgewählt ist, werden Sie aufgefordert, einen vorhandenen Power BI-Benutzer zu autorisieren oder ein neues Power BI-Konto zu erstellen.  
   
   ![Autorisieren von Power BI-Benutzern](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  
2. Erstellen Sie ein neues Konto, wenn Sie noch keines haben, und klicken Sie dann auf "Jetzt autorisieren".  Es wird ein Bildschirm ähnlich dem folgenden angezeigt.  
   
   ![Azure-Konto Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  
3. In diesem Schritt geben Sie das Geschäfts- oder Schulkonto für die Autorisierung der Power BI-Ausgabe an. Wenn Sie noch nicht für Power BI angemeldet sind, wählen Sie "Jetzt anmelden" aus. Das für Power BI verwendete Geschäfts- Schul- oder Unikonto kann sich vom Azure-Abonnementkonto unterscheiden, bei dem Sie zurzeit angemeldet sind.

### <a name="configure-the-power-bi-output-properties"></a>Konfigurieren der Eigenschaften der Power BI-Ausgabe
Sobald Sie das Power BI-Konto authentifiziert haben, können Sie die Eigenschaften für die Power BI-Ausgabe konfigurieren. Die folgende Tabelle enthält eine Liste von Eigenschaftennamen und der entsprechenden Beschreibung zum Konfigurieren der Power BI-Ausgabe.

| Eigenschaftenname | BESCHREIBUNG |
| --- | --- |
| Ausgabealias |Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an diese Power BI-Ausgabe weiterzuleiten. |
| Gruppenarbeitsbereich |Um die gemeinsame Datennutzung mit anderen Power BI-Benutzern zu ermöglichen, können Sie Gruppen in Ihrem Power BI-Konto auswählen. Wählen Sie alternativ „Arbeitsbereich“, wenn Sie nicht in eine Gruppe schreiben möchten.  Zum Aktualisieren einer vorhandenen Gruppe muss die Power BI-Authentifizierung erneuert werden. |
| Datasetname |Geben Sie einen Datasetnamen an, der für die Power BI-Ausgabe verwendet werden soll. |
| Tabellenname |Geben Sie einen Tabellennamen unter dem Dataset der Power BI-Ausgabe ein. Derzeit darf die Power BI-Ausgabe von Stream Analytics-Aufträgen nur eine Tabelle pro Dataset aufweisen. |

Eine Schritt-für-Schritt-Anleitung zum Konfigurieren eine Power BI-Ausgabe und eines Power BI-Dashboards erhalten Sie im Artikel [Stream Analytics und Power BI: Ein Dashboard mit Echtzeitanalyse von Streamingdaten](stream-analytics-power-bi-dashboard.md).

> [!NOTE]
> Erstellen Sie das Dataset und die Tabelle nicht explizit im Power BI-Dashboard. Das Dataset und die Tabelle werden automatisch mit Daten aufgefüllt, wenn der Auftrag gestartet wird und damit beginnt, Ausgaben an Power BI weiterzuleiten. Wenn die Auftragsabfrage keine Ergebnisse generiert, werden das Dataset und die Tabelle nicht erstellt. Beachten Sie Folgendes: Wenn Power BI bereits über ein Dataset und eine Tabelle mit demselben Namen verfügt, der in diesem Stream Analytics-Auftrag angegeben wurde, werden die vorhandenen Daten überschrieben.
> 
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
Sie müssen Ihr Power BI-Konto erneut authentifizieren, wenn das Kennwort seit der Erstellung oder letzten Authentifizierung Ihres Auftrags geändert wurde. Wenn Multi-Factor Authentication (MFA) für Ihren AAD-Mandanten (Azure Active Directory) konfiguriert ist, müssen Sie auch Power BI-Autorisierung alle zwei Wochen erneuern. Dieses Problem zeigt sich daran, dass keine Auftragsausgabe erfolgt und in den Vorgangsprotokollen ein Benutzerauthentifizierungsfehler angezeigt wird:

  ![Power BI-Aktualisierungstoken-Fehler](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

Um dieses Problem zu beheben, halten Sie den laufenden Auftrag an, und wechseln Sie zur Power BI-Ausgabe.  Klicken Sie auf den Link "Autorisierung erneuern", und starten Sie den Auftrag ab dem letzten Anhaltepunkt neu, um Datenverlust zu vermeiden.

  ![Power BI erneuert die Autorisierung](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Tabellenspeicher
[Azure Table Storage](../storage/common/storage-introduction.md) bietet einen hoch verfügbaren, in hohem Maße skalierbaren Speicher, sodass eine Anwendung automatisch an die Bedürfnisse der Benutzer angepasst werden kann. Tabellenspeicher ist Microsofts NoSQL-Schlüssel-/Attributspeicher, der für strukturierte Daten genutzt werden kann, die weniger Einschränkungen hinsichtlich des Schemas aufweisen. Azure-Tabellenspeicher kann zum Speichern von Daten für dauerhafte Archivierung und effizienten Abruf verwendet werden.

Die folgende Tabelle enthält die Eigenschaftennamen und die entsprechenden Beschreibungen zum Erstellen einer Tabellenausgabe.

| Eigenschaftenname | BESCHREIBUNG |
| --- | --- |
| Ausgabealias |Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an diesen Tabellenspeicher weiterzuleiten. |
| Speicherkonto |Der Name des Speicherkontos, an das Sie die Ausgabe senden. |
| Speicherkontoschlüssel |Der Zugriffsschlüssel, der dem Speicherkonto zugeordnet ist. |
| Tabellenname |Der Name der Tabelle. Die Tabelle wird erstellt, wenn sie nicht vorhanden ist. |
| Partitionsschlüssel |Der Name der Ausgabespalte mit dem Partitionsschlüssel. Der Partitionsschlüssel ist ein eindeutiger Bezeichner für die Partition innerhalb einer Tabelle, die den ersten Teil des Primärschlüssels einer Entität bildet. Dabei handelt es sich um einen Zeichenfolgenwert, der bis zu 1 KB groß sein kann. |
| Zeilenschlüssel |Der Name der Ausgabespalte, die den Zeilenschlüssel enthält. Der Zeilenschlüssel ist ein eindeutiger Bezeichner für eine Entität innerhalb einer Partition. Sie bildet den zweiten Teil des Primärschlüssels einer Entität. Der Zeilenschlüssel ist ein Zeichenfolgenwert, der bis zu 1 KB groß sein kann. |
| Batchgröße |Dies ist die Anzahl von Datensätzen für einen Batchvorgang. In der Regel ist die Standardeinstellung für die meisten Aufträge ausreichend. Ausführlichere Informationen zum Ändern dieser Einstellung finden Sie unter [TableBatchOperation Class (TableBatchOperation-Klasse)](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx). |
 
## <a name="service-bus-queues"></a>Service Bus-Warteschlangen
[Service Bus-Warteschlangen](https://msdn.microsoft.com/library/azure/hh367516.aspx) bieten eine FIFO-Nachrichtenzustellung (First In, First Out) an einen Consumer oder an mehrere konkurrierende Consumer. Typischerweise wird erwartet, dass Nachrichten in der zeitlichen Reihenfolge von den Consumern empfangen und verarbeitet werden, in der sie der Warteschlange hinzugefügt wurden, und jede Nachricht wird nur von einem Nachrichtenconsumer empfangen und verarbeitet.

Die folgende Tabelle enthält die Eigenschaftennamen und die entsprechenden Beschreibungen zum Erstellen einer Warteschlangenausgabe.

| Eigenschaftenname | BESCHREIBUNG |
| --- | --- |
| Ausgabealias |Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an diese Service Bus-Warteschlange weiterzuleiten. |
| Service Bus- Namespace |Ein Service Bus-Namespace ist ein Container für einen Satz von Nachrichtenentitäten. |
| Warteschlangenname |Der Name der Service Bus-Warteschlange. |
| Name der Warteschlangenrichtlinie |Beim Erstellen einer Warteschlange können Sie auf der Registerkarte "Warteschlange konfigurieren" Richtlinien für den gemeinsamen Zugriff erstellen. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. |
| Schlüssel der Warteschlangenrichtlinie |Der Schlüssel für den gemeinsamen Zugriff, der für die Authentifizierung des Zugriffs auf den Service Bus-Namespace verwendet wird. |
| Ereignisserialisierungsformat |Das Serialisierungsformat für Ausgabedaten.  Es werden JSON, CSV und Avro unterstützt. |
| Codieren |Bei CSV und JSON ist UTF-8 gegenwärtig das einzige unterstützte Codierungsformat. |
| Trennzeichen |Gilt nur für die CSV-Serialisierung. Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen zum Serialisieren der Daten im CSV-Format. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Tabstopp und senkrechter Strich. |
| Format |Gilt nur für den JSON-Typ. „Separate Zeile“ gibt an, dass die Ausgabe so formatiert wird, dass jedes JSON-Objekt in einer neuen Zeile enthalten ist. „Array“ gibt an, dass die Ausgabe als Array aus JSON-Objekten formatiert wird. |

## <a name="service-bus-topics"></a>Service Bus-Themen
Während Service Bus-Warteschlangen eine 1:1-Kommunikationsmethode vom Absender zum Empfänger bereitstellen, bieten [Service Bus-Themen](https://msdn.microsoft.com/library/azure/hh367516.aspx) eine 1:n-Form der Kommunikation.

Die folgende Tabelle enthält die Eigenschaftennamen und die entsprechenden Beschreibungen zum Erstellen einer Tabellenausgabe.

| Eigenschaftenname | BESCHREIBUNG |
| --- | --- |
| Ausgabealias |Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an dieses Service Bus-Thema weiterzuleiten. |
| Service Bus- Namespace |Ein Service Bus-Namespace ist ein Container für einen Satz von Nachrichtenentitäten. Sie haben bei der Erstellung eines neuen Event Hubs auch einen Service Bus-Namespace erstellt. |
| Themenname |Themen sind Messagingentitäten, vergleichbar mit Event Hubs und Warteschlangen. Sie wurden für die Erfassung von Ereignisstreams von einer Reihe unterschiedlicher Geräte und Dienste entwickelt. Wenn ein Thema erstellt wird, wird ihm auch ein bestimmter Name zugewiesen. Die an ein Thema gesendeten Nachrichten sind nur verfügbar, wenn ein Abonnement erstellt wurde. Stellen Sie daher sicher, dass es mindestens ein Abonnement unter dem Thema gibt. |
| Name der Themenrichtlinie |Beim Erstellen eines Themas können Sie auf der Registerkarte „Thema konfigurieren“ Richtlinien für den gemeinsamen Zugriff erstellen. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. |
| Schlüssel der Themenrichtlinie |Der Schlüssel für den gemeinsamen Zugriff, der für die Authentifizierung des Zugriffs auf den Service Bus-Namespace verwendet wird. |
| Ereignisserialisierungsformat |Das Serialisierungsformat für Ausgabedaten.  Es werden JSON, CSV und Avro unterstützt. |
 | Codieren |Bei Verwendung des CSV- oder JSON-Formats muss eine Codierung angegeben werden. Das einzige derzeit unterstützte Codierungsformat ist UTF-8. |
| Trennzeichen |Gilt nur für die CSV-Serialisierung. Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen zum Serialisieren der Daten im CSV-Format. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Tabstopp und senkrechter Strich. |

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) ist ein global verteilter Datenbankdienst mit mehreren Modellen, der eine grenzenlose elastische Skalierung rund um den Globus, umfangreiche Abfragen und automatische Indizierung über schemaunabhängige Datenmodelle, garantiert niedrige Latenz und branchenführende umfassende SLAs bietet.

Die folgende Liste enthält die Eigenschaftennamen und die entsprechenden Beschreibungen zum Erstellen einer Azure Cosmos DB-Ausgabe.

* **Ausgabealias**: Ein Alias zum Verweisen auf diese Ausgabe in Ihrer Stream Analytics-Abfrage.  
* **Kontoname**: Der Name oder Endpunkt-URI des Cosmos DB-Kontos.  
* **Kontoschlüssel**: Der gemeinsame Zugriffsschlüssel für das Cosmos DB-Konto.  
* **Datenbank**: Der Name der Cosmos DB-Datenbank.  
* **Muster für Sammlungsnamen**: Der Sammlungsname oder das Muster für die zu verwendenden Sammlungen. Das Sammlungsnamenformat kann mit dem optionalen Token {partition} gebildet werden, wobei Partitionen bei 0 beginnen. Im Folgenden finden Sie Beispiele gültiger Eingaben:  
  1\) MyCollection – Eine Sammlung mit dem Namen „MyCollection“ muss vorhanden sein.  
  2\) MyCollection{partition} – Solche Auflistungen müssen vorhanden sein: „MyCollection0“, „MyCollection1“, „MyCollection2“ usw.  
* **Partitionsschlüssel**: Optional. Nur erforderlich, wenn Sie im Muster Ihres Sammlungsnamens ein partition-Token verwenden. Der Name des Felds in Ausgabeereignissen, das zur Angabe des Schlüssels für die Partitionierung der Ausgabe über Sammlungen hinweg verwendet wird. Für die Ausgabe einer einzelnen Sammlung kann eine beliebige Ausgabespalte wie PartitionId verwendet werden.  
* **Dokument-ID** : Optional. Der Name des Felds in Ausgabeereignissen, das zur Angabe des Primärschlüssels verwendet wird, auf dem Einfüge- und Aktualisierungsvorgänge basieren.  

## <a name="azure-functions-in-preview"></a>Azure Functions (Vorschauversion)
Azure Functions ist ein serverloser Computedienst, mit dem Sie Code bedarfsgesteuert ausführen können, ohne eine explizite Infrastruktur bereitstellen oder verwalten zu müssen. Mit diesem Dienst können Sie Codes implementieren, die durch in Azure- oder Drittanbieterdiensten auftretende Ereignisse ausgelöst werden.  Aufgrund der Möglichkeit, auf Trigger zu antworten, ist Azure Functions die ideale Ausgabe für Azure Stream Analytics. Mithilfe dieses Ausgabeadapters können Benutzer eine Verbindung zwischen Stream Analytics und Azure Functions herstellen und als Reaktion auf verschiedenste Ereignisse ein Skript oder einen Codeausschnitt ausführen.

Azure Stream Analytics ruft Azure Functions über HTTP-Trigger auf. Der neue Azure Functions-Ausgabeadapter wird mit folgenden konfigurierbaren Eigenschaften zur Verfügung gestellt:

| Eigenschaftenname | BESCHREIBUNG |
| --- | --- |
| Funktionen-App |Name der Azure Functions-App |
| Funktion |Name der Funktion in der Azure Functions-App |
| Max Batch Size |Mit dieser Eigenschaft kann die maximale Größe für jeden Ausgabebatch festgelegt werden, der an Ihre Azure Functions-Instanz gesendet wird. Standardmäßig ist dieser Wert auf 256 KB festgelegt. |
| Max Batch Count  |Wie der Name schon sagt, können Sie mit dieser Eigenschaft die maximale Anzahl von Ereignissen in jedem Batch angeben, der an Azure Functions gesendet wird. Der Standardwert für „Max Batch Count“ ist 100. |
| Schlüssel |Wenn Sie eine Azure-Funktion aus einem anderen Abonnement verwenden möchten, können Sie dies tun, indem Sie den Schlüssel für den Zugriff auf Ihre Funktion angeben. |

Beachten Sie, dass die Größe der an Azure Functions gesendeten Batches verringert wird, wenn in Azure Stream Analytics Ausnahme 413 (HTTP-Anforderungseinheit zu groß) durch Azure Functions auftritt. Verwenden Sie in Ihrem Azure-Funktionscode diese Ausnahme, um sicherzustellen, dass Azure Stream Analytics keine übermäßig großen Batches sendet. Stellen Sie außerdem sicher, dass die Werte für die maximal zulässige Batchanzahl und -größe in der Funktion mit den Werten übereinstimmen, die im Stream Analytics-Portal eingegeben wurden. 

Zudem wird in Fällen, in denen kein Ereignis in einem Zeitfenster auftritt, keine Ausgabe generiert. Als Ergebnis wird die computeResult-Funktion nicht aufgerufen. Dieses Verhalten entspricht den integrierten Aggregatfunktionen im Fenstermodus.


## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, besuchen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
Sie haben nun Stream Analytics kennengelernt, einen verwalteten Dienst für Stream Analytics für Daten aus dem Internet der Dinge. Weitere Informationen zu diesem Dienst finden Sie unter:

* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
