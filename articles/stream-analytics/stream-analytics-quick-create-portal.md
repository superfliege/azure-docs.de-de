---
title: Erstellen eines Stream Analytics-Auftrags mithilfe des Azure-Portals | Microsoft-Dokumentation
description: In dieser Schnellstartanleitung werden die ersten Schritte zum Erstellen eines Stream Analytic-Auftrags, zum Konfigurieren von Ein- und Ausgaben sowie zum Definieren einer Abfrage erläutert.
services: stream-analytics
keywords: Stream Analytics, Cloud-Aufträge, Azure-Portal, Auftragseingabe, Auftragsausgabe, Auftragstransformation
author: SnehaGunda
ms.author: sngun
ms.date: 03/16/2018
ms.topic: quickstart
ms.service: stream-analytics
ms.custom: mvc
manager: kfile
ms.openlocfilehash: c421ab96585da011cdaef9933ceb8a78ffe356a9
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="quickstart-create-a-stream-analytics-job-by-using-the-azure-portal"></a>Schnellstart: Erstellen eines Stream Analytics-Auftrags mithilfe des Azure-Portals

Dieser Schnellstart veranschaulicht die ersten Schritte beim Erstellen eines Stream Analytics-Auftrags. In dieser Schnellstartanleitung definieren Sie einen Stream Analytics-Auftrag, der Sensorbeispieldaten liest und Zeilen filtert, bei denen die Durchschnittstemperatur 30 Sekunden lang über 100 liegt. In diesem Artikel lesen Sie Daten aus Blobspeicher, transformieren die Daten und schreiben sie zurück in einen anderen Container im gleichen Blobspeicher.

## <a name="before-you-begin"></a>Voraussetzungen

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.

* Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="prepare-the-input-data"></a>Vorbereiten der Eingabedaten

Vor dem Definieren des Stream Analytics-Auftrags sollten Sie die Daten vorbereiten, die als Eingabe für den Auftrag konfiguriert werden. Führen Sie die folgenden Schritte aus, um die für den Auftrag erforderlichen Eingabedaten vorzubereiten:

1. Laden Sie die [Sensorbeispieldaten](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/GettingStarted/HelloWorldASA-InputStream.json) von GitHub herunter. Die Beispieldaten enthalten Sensorinformationen im folgenden JSON-Format:  

   ```json
   {
     "time": "2016-01-26T21:18:52.0000000",
     "dspl": "sensorC",
     "temp": 87,
     "hmdt": 44
   }
   ```
2. Melden Sie sich auf dem Azure-Portal an.  

3. Klicken Sie in der linken oberen Ecke des Azure-Portals auf **Ressource erstellen** > **Speicher** > **Speicherkonto**. Geben Sie auf dem Blatt für den Speicherkontoauftrag die entsprechenden Informationen ein: Legen Sie für **Name** den Namen „myasastorageaccount“, für **Standort** die Option „West US 2“ und für **Ressourcengruppe** die Option „MyRG“ fest. (Hosten Sie für eine höhere Leistung das Speicherkonto in der gleichen Ressourcengruppe wie den Streamingauftrag.) Bei den übrigen Einstellungen können Sie die Standardwerte übernehmen.  

   ![Speicherkonto erstellen](./media/stream-analytics-quick-create-portal/create-a-storage-account.png)

4. Navigieren Sie auf dem Blatt **Alle Ressourcen** zu dem im vorherigen Schritt erstellten Speicherkonto. Öffnen Sie das Blatt **Übersicht**, und klicken Sie auf die Kachel **Blobs**.  

5. Klicken Sie auf dem Blatt **Blob-Dienst** auf **Container**, geben Sie im Feld **Name** einen Namen für den Container ein (etwa *container1*), und ändern Sie die Angabe für **Öffentliche Zugriffsebene** in „Blob (anonymous read access for blobs only)“ (Blob (anonymer Lesezugriff nur für Blobs)). Klicken Sie dann auf **OK**.  

   ![Erstellen eines Containers](./media/stream-analytics-quick-create-portal/create-a-storage-container.png)

6. Navigieren Sie zu dem im vorherigen Schritt erstellten Container, klicken Sie auf **Hochladen**, und laden Sie die in Schritt 1 abgerufenen Sensordaten hoch.  

   ![Hochladen von Beispieldaten in den Blob](./media/stream-analytics-quick-create-portal/upload-sample-data-to-blob.png)

## <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags

1. Melden Sie sich auf dem Azure-Portal an.  

2. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.  

3. Klicken Sie in der Ergebnisliste auf **Daten + Analysen** > **Stream Analytics-Auftrag**.  

4. Geben Sie auf dem Blatt für den Stream Analytics-Auftrag die folgenden Informationen ein:

   |**Einstellung**  |**Empfohlener Wert**  |**Beschreibung**  |
   |---------|---------|---------|
   |Auftragsname   |  myJob   |   Geben Sie einen Namen zur Identifizierung des Stream Analytics-Auftrags ein. Der Stream Analytics-Auftragsname darf nur alphanumerische Zeichen, Bindestriche und Unterstriche enthalten und muss zwischen 3 und 63 Zeichen lang sein. |
   |Abonnement  | \<Ihr Abonnement\> |  Wählen Sie das Azure-Abonnement aus, das Sie für diesen Auftrag verwenden möchten. |
   |Ressourcengruppe   |   myResourceGroup  |   Wählen Sie **Neu erstellen** aus, und geben Sie einen neuen Ressourcengruppenname für Ihr Konto ein. |
   |Speicherort  |  \<Die Region, die Ihren Benutzern am nächsten liegt\> | Wählen Sie den geografischen Standort aus, in dem Sie Ihren Stream Analytics-Auftrag hosten können. Verwenden Sie den Standort, der Ihren Benutzern am nächsten liegt, um die Leistung zu steigern und die Datenübertragungskosten zu senken. |
   |Streamingeinheiten  | 1  |   Streamingeinheiten stellen die Computingressourcen dar, die für die Ausführung eines Auftrags erforderlich sind. Standardmäßig ist dieser Wert auf 1 festgelegt. Informationen zum Skalieren von Streamingeinheiten finden Sie im Artikel [Überblick über Streamingeinheiten und Informationen zu Anpassungen](stream-analytics-streaming-unit-consumption.md).   |
   |Hosting-Umgebung  |  Cloud  |   Für Stream Analytics-Aufträge ist eine Cloud- oder Edge-Bereitstellung möglich. Die Aufträge können mit der Option „Cloud“ in Azure Cloud und mit der Option „Edge“ auf einem IoT Edge-Gerät bereitgestellt werden. |

   ![Erstellen eines Auftrags](./media/stream-analytics-quick-create-portal/create-job.png)

5. Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, um den Auftrag auf Ihrem Dashboard zu platzieren, und klicken Sie dann auf **Erstellen**.  

6. Oben rechts im Browserfenster sollte „Die Bereitstellung wird ausgeführt.“ angezeigt werden. 

## <a name="configure-input-to-the-job"></a>Konfigurieren einer Eingabe für den Auftrag

In diesem Abschnitt konfigurieren Sie Blobspeicher als Eingabe für den Stream Analytics-Auftrag. Erstellen Sie vor dem Konfigurieren der Eingabe ein Blobspeicherkonto.  

### <a name="add-the-input"></a>Hinzufügen der Eingabe 

1. Navigieren Sie zum Stream Analytics-Auftrag.  

2. Klicken Sie auf **Eingaben** > **Datenstromeingabe hinzufügen** > **Blobspeicher**.  

3. Fügen Sie auf dem Blatt **Blobspeicher** die folgenden Werte ein:

   |**Einstellung**  |**Empfohlener Wert**  |**Beschreibung**  |
   |---------|---------|---------|
   |Eingabealias  |  BlobInput   |  Geben Sie einen Namen zur Identifizierung der Auftragseingabe ein.   |
   |Abonnement   |  \<Ihr Abonnement\> |  Wählen Sie das Azure-Abonnement mit dem von Ihnen erstellten Speicherkonto aus. Das Speicherkonto kann sich im gleichen oder in einem anderen Abonnement befinden. In diesem Beispiel wird davon ausgegangen, dass Sie ein Speicherkonto im gleichen Abonnement erstellt haben. |
   |Speicherkonto  |  myasastorageaccount |  Wählen Sie den Namen des Speicherkontos aus, oder geben Sie ihn ein. Speicherkontonamen werden automatisch erkannt, wenn sie im gleichen Abonnement erstellt werden. |
   |Container  | container1 | Wählen Sie den Namen des Containers aus, der Beispieldaten enthält. Containernamen werden automatisch erkannt, wenn sie im gleichen Abonnement erstellt werden. |

4. Übernehmen Sie für die anderen Optionen die Standardwerte, und klicken Sie zum Speichern der Einstellungen auf **Speichern**.  

   ![Konfigurieren der Eingabedaten](./media/stream-analytics-quick-create-portal/configure-input.png)
 
## <a name="configure-output-to-the-job"></a>Konfigurieren einer Ausgabe für den Auftrag

1. Navigieren Sie zum zuvor erstellten Stream Analytics-Auftrag.  

2. Klicken Sie auf **Ausgaben > Hinzufügen > Blobspeicher**.  

3. Fügen Sie auf dem Blatt **Blobspeicher** die folgenden Werte ein:

   |**Einstellung**  |**Empfohlener Wert**  |**Beschreibung**  |
   |---------|---------|---------|
   |Ausgabealias |   BlobOutput   |   Geben Sie einen Namen zur Identifizierung der Auftragsausgabe ein. |
   |Abonnement  |  \<Ihr Abonnement\>  |  Wählen Sie das Azure-Abonnement mit dem von Ihnen erstellten Speicherkonto aus. Das Speicherkonto kann sich im gleichen oder in einem anderen Abonnement befinden. In diesem Beispiel wird davon ausgegangen, dass Sie ein Speicherkonto im gleichen Abonnement erstellt haben. |
   |Speicherkonto |  myasastorageaccount |   Wählen Sie den Namen des Speicherkontos aus, oder geben Sie ihn ein. Speicherkontonamen werden automatisch erkannt, wenn sie im gleichen Abonnement erstellt werden.       |
   |Container |   container2  |  Erstellen Sie einen neuen Container im gleichen Speicherkonto, das Sie für die Eingabe verwendet haben.   |

4. Übernehmen Sie für die anderen Optionen die Standardwerte, und klicken Sie zum Speichern der Einstellungen auf **Speichern**.  

   ![Konfigurieren der Ausgabe](./media/stream-analytics-quick-create-portal/configure-output.png)
 
## <a name="define-the-transformation-query"></a>Definieren der Transformationsabfrage

1. Navigieren Sie zum zuvor erstellten Stream Analytics-Auftrag.  

2. Klicken Sie auf **Abfrage**, und aktualisieren Sie die Abfrage wie folgt:  

   ```sql
   SELECT 
   System.Timestamp AS OutputTime,
   dspl AS SensorName,
   Avg(temp) AS AvgTemperature
   INTO
     MyBlobOutput
   FROM
     MyBlobInput TIMESTAMP BY time
   GROUP BY TumblingWindow(second,30),dspl
   HAVING Avg(temp)>100
   ```

3. In diesem Beispiel liest die Abfrage die Daten aus dem Blob und kopiert sie in eine neue Datei im Blob. Klicken Sie auf **Speichern**.  

   ![Konfigurieren der Auftragstransformation](./media/stream-analytics-quick-create-portal/configure-job-transformation.png)

## <a name="start-the-stream-analytics-job-and-check-the-output"></a>Starten des Stream Analytics-Auftrags und Überprüfen der Ausgabe

1. Wechseln Sie zurück zum Blatt mit der Auftragsübersicht, und klicken Sie auf **Starten**.  

2. Klicken Sie unter **Auftrag starten** für das Feld **Startzeit** auf **Benutzerdefiniert**. Da die Datei vor dem aktuellen Datum in den Blobspeicher hochgeladen wurde, muss das Datum auf den Tag vor dem Upload festgelegt werden. Klicken Sie auf **Starten**, wenn Sie fertig sind.  

   ![Starten des Auftrags](./media/stream-analytics-quick-create-portal/start-the-job.png)

3. Navigieren Sie nach einigen Minuten im Portal zu dem Konto und dem Container, die Sie als Ausgabe für den Auftrag konfiguriert haben. Sie sehen nun die Ausgabedatei im Container. Beim ersten Mal dauert es einige Minuten, bis der Auftrag gestartet wird. Nach dem Start wird er weiterhin ausgeführt, während die Daten eingehen.  

   ![Transformierte Ausgabe](./media/stream-analytics-quick-create-portal/transformed-output.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den Streamingauftrag und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Durch das Löschen des Auftrags verhindern Sie, dass Kosten für die vom Auftrag verbrauchten Streamingeinheiten anfallen. Wenn Sie den Auftrag in Zukunft verwenden möchten, können Sie ihn beenden und später bei Bedarf neu starten. Wenn Sie diesen Auftrag nicht weiter verwenden möchten, löschen Sie alle von diesem Schnellstart erstellten Ressourcen. Führen Sie dazu folgende Schritte aus:

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen** und dann auf den Namen der erstellten Ressource.  

2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld den Namen der zu löschenden Ressource ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie einen einfachen Stream Analytics-Auftrag bereitgestellt. Wenn Sie Informationen zum Konfigurieren anderer Eingabequellen sowie zum Ausführen der Echtzeiterkennung benötigen, lesen Sie den folgenden Artikel:

> [!div class="nextstepaction"]
> [Erste Schritte mit Azure Stream Analytics: Betrugserkennung in Echtzeit](stream-analytics-real-time-fraud-detection.md)

