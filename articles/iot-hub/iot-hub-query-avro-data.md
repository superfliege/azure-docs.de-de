---
title: Abfragen von Avro-Daten mit Azure Data Lake Analytics | Microsoft-Dokumentation
description: Nutzen Sie Nachrichtentexteigenschaften, um Telemetriedaten von Geräten an Blobspeicher weiterzuleiten und die im Avro-Format in Blobspeicher geschriebenen Daten abzufragen.
author: ash2017
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: asrastog
ms.openlocfilehash: a17df39c55b5c02c83e3f0b74a91d7109ddb4d3d
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188943"
---
# <a name="query-avro-data-by-using-azure-data-lake-analytics"></a>Abfragen von Avro-Daten mit Azure Data Lake Analytics

In diesem Artikel geht es um die Abfrage von Avro-Daten für ein effizientes Routing von Nachrichten von Azure IoT Hub zu Azure-Diensten. Gemäß der Ankündigung im Blogbeitrag [Azure IoT Hub message routing: now with routing on message body] (Azure IoT Hub-Nachrichtenweiterleitung: jetzt mit Routing nach Nachrichtentext) unterstützt IoT Hub das Routing entweder nach Eigenschaften oder dem Nachrichtentext. Weitere Informationen finden Sie unter [Routing nach Nachrichtentext][Routing on message bodies]. 

Die Herausforderung bestand darin, dass wenn Azure IoT Hub Nachrichten an Azure-Blobspeicher weiterleitet, IoT Hub den Inhalt im Avro-Format schreibt, das sowohl eine Nachrichtentext- als auch Nachrichteneigenschaft aufweist. IoT Hub unterstützt das Schreiben von Daten in Blobspeicher nur im Avro-Datenformat, und dieses Format wird für keine anderen Endpunkte verwendet. Weitere Informationen finden Sie unter [Gründe für die Verwendung von Azure Storage-Containern][When using Azure storage containers]. Obwohl das Avro-Format hervorragend für die Beibehaltung von Daten und Nachrichten geeignet ist, ist es schwierig für Abfragedaten zu verwenden. Im Vergleich dazu lassen sich Daten im JSON- oder CSV-Format wesentlich einfacher abfragen.

Um nicht relationale Big Data-Anforderungen und Formate zu unterstützen und dies in den Griff zu bekommen, können Sie viele der Big Data-Muster sowohl für die Transformation als auch für die Skalierung von Daten verwenden. Eines der Muster, „Zahlung pro Abfrage“, ist Azure Data Lake Analytics, der Themenschwerpunkt dieses Artikels. Obwohl Sie die Abfrage bequem in Hadoop oder anderen Lösungen ausführen können, ist Data Lake Analytics oft besser für diesen „Zahlung pro Abfrage“-Ansatz geeignet. 

In U-SQL gibt es einen „Extraktor“ für Avro. Weitere Informationen finden Sie im [Beispiel zu U-SQL Avro].

## <a name="query-and-export-avro-data-to-a-csv-file"></a>Abfragen und Exportieren von Avro-Daten in eine CSV-Datei
In diesem Abschnitt fragen Sie Avro-Daten ab und exportieren Sie in eine CSV-Datei in Azure Blob Storage, wenngleich Sie die Daten problemlos in anderen Repositorys oder Datenspeichern ablegen können.

1. Richten Sie Azure IoT Hub für das Weiterleiten von Daten an einen Azure Blob Storage-Endpunkt ein, indem Sie eine Eigenschaft im Nachrichtentext verwenden, um Nachrichten auszuwählen.

    ![Der „Benutzerdefinierte Endpunkte“-Abschnitt][img-query-avro-data-1a]

    ![Der Routes-Befehl][img-query-avro-data-1b]

2. Vergewissern Sie sich, dass Ihr Gerät entsprechend der Produktdokumentation die Codierung, den Inhaltstyp und die benötigten Daten entweder in den Eigenschaften oder im Nachrichtentext aufweist. Wenn Sie diese Attribute wie hier gezeigt im Device Explorer sehen, können Sie überprüfen, ob sie ordnungsgemäß festgelegt sind.

    ![Der Event Hub-Datenbereich][img-query-avro-data-2]

3. Richten Sie eine Azure Data Lake Store- und eine Azure Data Lake Analytics-Instanz ein. Azure IoT Hub leitet nicht an eine Data Lake Store-Instanz weiter, aber eine Data Lake Analytics-Instanz benötigt eine.

    ![Data Lake Store- und Data Lake Analytics-Instanzen][img-query-avro-data-3]

4. Konfigurieren Sie in Data Lake Analytics Azure Blob Storage als zusätzlichen Speicher, d.h. den Blobspeicher, an den Azure IoT Hub Daten weiterleitet.

    ![Der Bereich „Datenquellen“][img-query-avro-data-4]
 
5. Wie im [Beispiel zu U-SQL Avro] erörtert, benötigen Sie vier DLL-Dateien. Laden Sie diese Dateien an einen Speicherort in Ihrer Data Lake Store-Instanz hoch.

    ![Vier hochgeladene DLL-Dateien][img-query-avro-data-5] 

6. Erstellen Sie in Visual Studio ein U-SQL-Projekt.
 
    ![Erstellen eines U-SQL-Projekts][img-query-avro-data-6]

7. Fügen Sie den Inhalt des folgenden Skripts in die neu erstellte Datei ein. Ändern Sie die drei hervorgehobenen Abschnitte: Ihr Data Lake Analytics-Konto, die zugehörigen DLL-Dateipfade und den richtigen Pfad Ihres Speicherkontos.
    
    ![Die drei zu ändernden Abschnitte][img-query-avro-data-7a]

    Das tatsächliche U-SQL-Skript für eine einfache Ausgabe in eine CSV-Datei:
    
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

    Data Lake Analytics benötigte fünf Minuten zur Ausführung des folgenden Skripts, das auf 10 analytische Einheiten und 177 verarbeitete Dateien beschränkt war. Das Ergebnis wird in der CSV-Dateiausgabe in der folgenden Abbildung angezeigt:
    
    ![Ergebnisse der Ausgabe in die CSV-Datei][img-query-avro-data-7b]

    ![In CSV-Datei konvertierte Ausgabe][img-query-avro-data-7c]

    Fahren Sie mit Schritt 8 fort, wenn Sie den JSON-Code analysieren möchten.
    
8. Die meisten IoT-Nachrichten haben das JSON-Dateiformat. Durch Hinzufügen der folgenden Zeilen können Sie die Nachricht in eine JSON-Datei analysieren, sodass Sie die WHERE-Klauseln hinzufügen und nur die benötigten Daten ausgeben können.

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

    In der Ausgabe wird für jedes Element im `SELECT`-Befehl eine Spalte angezeigt. 
    
    ![Ausgabe mit einer Spalte für jedes Element][img-query-avro-data-8]

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

[Beispiel zu U-SQL Avro]:https://github.com/Azure/usql/tree/master/Examples/AvroExamples

[lnk-iot-sa-land]: ../iot-accelerators/index.yml
[Entwicklungsleitfaden für IoT Hub]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
