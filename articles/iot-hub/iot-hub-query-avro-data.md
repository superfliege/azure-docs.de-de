---
title: Abfragen von Avro-Daten mit Azure Data Lake Analytics | Microsoft-Dokumentation
description: Nutzen Sie Nachrichtentexteigenschaften, um Telemetriedaten von Geräten an Blobspeicher weiterzuleiten und die im Avro-Format in Blobspeicher geschriebenen Daten abzufragen.
services: iot-hub
documentationcenter: ''
author: ksaye
manager: obloch
ms.service: iot-hub
ms.topic: article
ms.date: 05/29/2018
ms.author: Kevin.Saye
ms.openlocfilehash: 98a30155c73a937042b4bea6568543fb5152d748
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726633"
---
# <a name="query-avro-data-using-azure-data-lake-analytics"></a>Abfragen von Avro-Daten mit Azure Data Lake Analytics

In diesem Artikel geht es um die Abfrage von Avro-Daten für ein effizientes Routing von Nachrichten von Azure IoT Hub zu Azure-Diensten. Gemäß der Ankündigung im Blogbeitrag [Azure IoT Hub message routing: now with routing on message body] (Azure IoT Hub-Nachrichtenweiterleitung: jetzt mit Routing nach Nachrichtentext) unterstützt IoT Hub das Routing entweder nach Eigenschaften oder dem Nachrichtentext. Siehe auch [Routing nach Nachrichtentext][Routing on message bodies]. 

Die Herausforderung bestand darin, dass wenn Azure IoT Hub Nachrichten an Blobspeicher weiterleitet, IoT Hub den Inhalt im Avro-Format schreibt, das sowohl Nachrichtentext als auch Nachrichteneigenschaften aufweist. Beachten Sie, dass nur IoT Hub das Schreiben von Daten in Blobspeicher im Avro-Datenformat unterstützt und dass dieses Format für keine anderen Endpunkte verwendet wird. Siehe [Gründe für die Verwendung von Azure Storage-Containern][When using Azure storage containers]. Das Avro-Format eignet sich zwar hervorragend für die Daten-/Nachrichtenarchivierung, stellt aber für die Abfrage der Daten eine Herausforderung dar. Im Vergleich dazu lassen sich Daten im JSON- oder CSV-Format wesentlich einfacher abfragen.

Um dies in Griff zu bekommen, können Sie viele der Big Data-Muster sowohl für die Transformation als auch für die Skalierung von Daten verwenden, um nicht relationale Big Data-Anforderungen und -Formate zu unterstützen. Eines der Muster, „Zahlung pro Abfrage“, ist Azure Data Lake Analytics (ADLA). Es bildet den Schwerpunkt dieses Artikels. Obwohl Sie die Abfrage bequem in Hadoop oder anderen Lösungen ausführen können, ist ADLA oft besser für diesen „Zahlung pro Abfrage“-Ansatz geeignet. In U-SQL gibt es einen „Extraktor“ für Avro. Siehe das [Beispiel zu U-SQL Avro].

## <a name="query-and-export-avro-data-to-a-csv-file"></a>Abfragen und Exportieren von Avro-Daten in eine CSV-Datei
Der Abschnitt begleitet Sie durch das Abfragen von Avro-Daten und deren Export in eine CSV-Datei in Azure Blob Storage, wenngleich Sie die Daten problemlos in anderen Repositorys oder Datenspeichern ablegen können.

1. Richten Sie Azure IoT Hub für das Weiterleiten von Daten an einen Azure Blob Storage-Endpunkt ein, indem Sie eine Eigenschaft im Nachrichtentext verwenden, um Nachrichten auszuwählen.

    ![Bildschirmaufnahme für Schritt 1a][img-query-avro-data-1a]

    ![Bildschirmaufnahme für Schritt 1b][img-query-avro-data-1b]

2. Vergewissern Sie sich, dass Ihr Gerät entsprechend der Produktdokumentation die Codierung, den Inhaltstyp und die benötigten Daten entweder in den Eigenschaften oder im Nachrichtentext aufweist. Prüfen Sie im Device Explorer (siehe unten), ob diese Attribute ordnungsgemäß festgelegt sind.

    ![Bildschirmaufnahme für Schritt 2][img-query-avro-data-2]

3. Richten Sie eine Azure Data Lake Store- (ADLS) und eine Azure Data Lake Analytics-Instanz ein. Während Azure IoT Hub keine Daten zu einer Azure Data Lake Store-Instanz weiterleitet, benötigt ADLA eine.

    ![Bildschirmaufnahme für Schritt 3][img-query-avro-data-3]

4. Konfigurieren Sie in ADLA den Azure Blob Storage als zusätzlichen Speicher, d. h. den Blobspeicher, an den Azure IoT Hub Daten weiterleitet.

    ![Bildschirmaufnahme für Schritt 4][img-query-avro-data-4]
 
5. Wie im [Beispiel zu U-SQL Avro] erläutert, werden vier DLL-Dateien benötigt.  Laden Sie diese Dateien an einen Speicherort in Ihrer ADLS-Instanz hoch.

    ![Bildschirmaufnahme für Schritt 5][img-query-avro-data-5] 

6. Erstellen Sie in Visual Studio ein U-SQL-Projekt.
 
    ![Bildschirmaufnahme für Schritt 6][img-query-avro-data-6]

7. Kopieren Sie den Inhalt des folgenden Skripts, und fügen Sie ihn in die neu erstellte Datei ein. Ändern Sie die drei hervorgehobenen Abschnitte entsprechend Ihrem ADLA-Konto, den Pfaden der zugehörigen DLLs und dem Pfad Ihres Speicherkontos.
    
    ![Bildschirmaufnahme für Schritt 7a][img-query-avro-data-7a]

    Das tatsächliche U-SQL-Skript für eine einfache Ausgabe im CSV-Format:
    
    ```sql
        DROP ASSEMBLY IF EXISTS [Avro];
        CREATE ASSEMBLY [Avro] FROM @"/Assemblies/Avro/Avro.dll";
        DROP ASSEMBLY IF EXISTS [Microsoft.Analytics.Samples.Formats];
        CREATE ASSEMBLY [Microsoft.Analytics.Samples.Formats] FROM @"/Assemblies/Avro/Microsoft.Analytics.Samples.Formats.dll";
        DROP ASSEMBLY IF EXISTS [Newtonsoft.Json];
        CREATE ASSEMBLY [Newtonsoft.Json] FROM @"/Assemblies/Avro/Newtonsoft.Json.dll";
        DROP ASSEMBLY IF EXISTS [log4net];
        CREATE ASSEMBLY [log4net] FROM @"/Assemblies/Avro/log4net.dll";

        REFERENCE ASSEMBLY [Newtonsoft.Json];
        REFERENCE ASSEMBLY [log4net];
        REFERENCE ASSEMBLY [Avro];
        REFERENCE ASSEMBLY [Microsoft.Analytics.Samples.Formats];

        // Blob container storage account filenames, with any path
        DECLARE @input_file string = @"wasb://hottubrawdata@kevinsayazstorage/kevinsayIoT/{*}/{*}/{*}/{*}/{*}/{*}";
        DECLARE @output_file string = @"/output/output.csv";

        @rs =
        EXTRACT
        EnqueuedTimeUtc string,
        Body byte[]
        FROM @input_file

        USING new Microsoft.Analytics.Samples.Formats.ApacheAvro.AvroExtractor(@"
        {
        ""type"":""record"",
        ""name"":""Message"",
        ""namespace"":""Microsoft.Azure.Devices"",
        ""fields"":[{
        ""name"":""EnqueuedTimeUtc"",
        ""type"":""string""
        },
        {
        ""name"":""Properties"",
        ""type"":{
        ""type"":""map"",
        ""values"":""string""
        }
        },
        {
        ""name"":""SystemProperties"",
        ""type"":{
        ""type"":""map"",
        ""values"":""string""
        }
        },
        {
        ""name"":""Body"",
        ""type"":[""null"",""bytes""]
        }
        ]
        }");

        @cnt =
        SELECT EnqueuedTimeUtc AS time, Encoding.UTF8.GetString(Body) AS jsonmessage
        FROM @rs;

        OUTPUT @cnt TO @output_file USING Outputters.Text(); 
    ```    

    Die Ausführung des unten gezeigten Skripts durch ADLA dauerte bei Beschränkung auf 10 Analyseeinheiten 5 Minuten, wobei 177 Dateien verarbeitet wurden und die Ausgabe in einer CSV-Datei zusammengefasst wurde.
    
    ![Bildschirmaufnahme für Schritt 7b][img-query-avro-data-7b]

    Bei der Überprüfung der Ausgabe können Sie sehen, dass der Avro-Inhalt in eine CSV-Datei konvertiert wurde. Fahren Sie mit Schritt 8 fort, wenn Sie den JSON-Code analysieren möchten.
    
    ![Bildschirmaufnahme für Schritt 7c][img-query-avro-data-7c]

    
8. Die meisten IoT-Nachrichten haben das JSON-Format.  Durch Hinzufügen der folgenden Zeilen können Sie die Nachricht in das JSON-Format analysieren, sodass Sie die WHERE-Klauseln hinzufügen und nur die benötigten Daten ausgeben können.

    ```sql
       @jsonify = SELECT Microsoft.Analytics.Samples.Formats.Json.JsonFunctions.JsonTuple(Encoding.UTF8.GetString(Body)) AS message FROM @rs;
    
        /*
        @cnt =
            SELECT EnqueuedTimeUtc AS time, Encoding.UTF8.GetString(Body) AS jsonmessage
            FROM @rs;
        
        OUTPUT @cnt TO @output_file USING Outputters.Text();
        */
        
        @cnt =
            SELECT message["message"] AS iotmessage,
                   message["event"] AS msgevent,
                   message["object"] AS msgobject,
                   message["status"] AS msgstatus,
                   message["host"] AS msghost
            FROM @jsonify;
            
        OUTPUT @cnt TO @output_file USING Outputters.Text();
    ```

9. Beim Überprüfen der Ausgabe sehen Sie nun Spalten für jedes Element im SELECT-Befehl. 
    
    ![Bildschirmaufnahme für Schritt 8][img-query-avro-data-8]

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Avro-Daten für ein effizientes Routing von Nachrichten von Azure IoT Hub zu Azure-Diensten abgefragt werden.

Beispiele vollständiger Lösungen, die IoT Hub nutzen, finden Sie unter [Azure IoT-Solution Accelerator für Remoteüberwachung][lnk-iot-sa-land].

Weitere Informationen zum Entwickeln von Lösungen mit IoT Hub finden Sie im [Entwicklungsleitfaden für IoT Hub].

Weitere Informationen zum Nachrichtenrouting in IoT Hub finden Sie unter [Senden und Empfangen von Nachrichten mit IoT Hub][lnk-devguide-messaging].

<!-- Images -->
[img-query-avro-data-1a]: ./media/iot-hub-query-avro-data/query-avro-data-1a.png
[img-query-avro-data-1b]: ./media/iot-hub-query-avro-data/query-avro-data-1b.png
[img-query-avro-data-2]: ./media/iot-hub-query-avro-data/query-avro-data-2.png
[img-query-avro-data-3]: ./media/iot-hub-query-avro-data/query-avro-data-3.png
[img-query-avro-data-4]: ./media/iot-hub-query-avro-data/query-avro-data-4.png
[img-query-avro-data-5]: ./media/iot-hub-query-avro-data/query-avro-data-5.png
[img-query-avro-data-6]: ./media/iot-hub-query-avro-data/query-avro-data-6.png
[img-query-avro-data-7a]: ./media/iot-hub-query-avro-data/query-avro-data-7a.png
[img-query-avro-data-7b]: ./media/iot-hub-query-avro-data/query-avro-data-7b.png
[img-query-avro-data-7c]: ./media/iot-hub-query-avro-data/query-avro-data-7c.png
[img-query-avro-data-8]: ./media/iot-hub-query-avro-data/query-avro-data-8.png

<!-- Links -->
[Azure IoT Hub message routing: now with routing on message body]: https://azure.microsoft.com/blog/iot-hub-message-routing-now-with-routing-on-message-body/ (Azure IoT Hub-Nachrichtenweiterleitung: jetzt mit Routing nach Nachrichtentext)

[Routing on message bodies]: iot-hub-devguide-query-language.md#routing-on-message-bodies
[When using Azure storage containers]:iot-hub-devguide-endpoints.md#when-using-azure-storage-containers

[Beispiel zu U-SQL Avro]:https://github.com/Azure/usql/tree/master/Examples/AvroExamples.

[lnk-iot-sa-land]: ../iot-accelerators/index.md
[Entwicklungsleitfaden für IoT Hub]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
