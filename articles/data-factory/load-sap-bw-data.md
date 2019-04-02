---
title: Laden von Daten aus SAP Business Warehouse mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: Verwenden von Azure Data Factory zum Kopieren von Daten aus SAP Business Warehouse (BW)
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: jingwang
ms.openlocfilehash: 9458903378576a50db9be92b9377987829e1ba41
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58200156"
---
# <a name="load-data-from-sap-business-warehouse-bw-by-using-azure-data-factory"></a>Laden von Daten aus SAP Business Warehouse (BW) mithilfe von Azure Data Factory

In diesem Artikel erhalten Sie eine exemplarische Vorgehensweise, wie Sie Data Factory verwenden, um _Daten aus SAP Business Warehouse (BW) über Open Hub in Azure Data Lake Storage Gen2 zu laden_. Sie können ähnliche Schritte ausführen, um Daten in andere [unterstützte Senkendatenspeicher](copy-activity-overview.md#supported-data-stores-and-formats) zu kopieren. 

> [!TIP]
> Allgemeine Informationen zum Kopieren von Daten aus SAP BW, einschließlich einer Einführung zur SAP BW Open Hub-Integration und zum Deltaextraktionsflow finden Sie im Artikel [SAP BW Open Hub Connector](connector-sap-business-warehouse-open-hub.md).

## <a name="prerequisites"></a>Voraussetzungen

- **Azure Data Factory (ADF):** Wenn Sie keine Data Factory besitzen, befolgen Sie den Abschnitt „[Erstellen einer Data Factory](quickstart-create-data-factory-portal.md#create-a-data-factory)“, um eine zu erstellen. 

- **SAP BW Open Hub Destination (OHD) mit Zieltyp „Datenbanktabelle“.** Befolgen Sie den Abschnitt [SAP BW Open Hub Destination-Konfigurationen](#sap-bw-open-hub-destination-configurations), um eine zu erstellen oder um zu bestimmen, ob Ihr vorhandenes OHD ordnungsgemäß konfiguriert ist, um in ADF integriert zu werden.

- **Der verwendete SAP BW-Benutzer muss über folgende Berechtigungen verfügen:**

  - Autorisierung für RFC und SAP BW.
  - Berechtigungen für die Aktivität „**Execute**“ des Autorisierungsobjekts „**S_SDSAUTH**“.

- **[Selbstgehostete Integration Runtime](concepts-integration-runtime.md#self-hosted-integration-runtime) mit SAP .NET 3.0 Connector ist erforderlich**. Im Folgenden finden Sie die detaillierten Vorbereitungen, die ausgeführt werden müssen:

  1. Installieren und registrieren Sie die selbstgehostete IR mit Version >= 3.13 (in der folgenden exemplarischen Vorgehensweise abgedeckt). 

  2. Laden Sie den [SAP .NET Connector 3.0 (64 Bit)](https://support.sap.com/en/product/connectors/msnet.html) von der SAP-Website herunter, und installieren Sie ihn auf dem selbstgehosteten IR-Computer.  Vergewissern Sie sich bei der Installation im Fenster „Optionale Einrichtungsschritte“, dass Sie die Option „**Assemblys in GAC installieren**“ wie in der folgenden Abbildung gezeigt auswählen.

     ![Richten Sie den SAP .NET Connector ein.](media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

## <a name="full-copy-from-sap-bw-open-hub"></a>Vollständiges Kopieren aus SAP BW Open Hub

Wechseln Sie im Azure-Portal zu Ihrer Data Factory, wählen Sie **Erstellen und überwachen** aus, um die ADF-Benutzeroberfläche auf einer separaten Registerkarte zu starten. 

1. Klicken Sie auf der Seite **Erste Schritte** auf **Daten kopieren**, um das Tool zum Kopieren von Daten zu starten. 

2. Geben Sie auf der Seite **Eigenschaften** einen Namen für das Feld **Aufgabenname** ein, und klicken Sie dann auf **Weiter**.

3. Klicken Sie auf der Seite **Quelldatenspeicher** auf **+ Neue Verbindung erstellen**, wählen Sie **SAP BW Open Hub** aus dem Connectorkatalog aus, und wählen Sie **Weiter** aus. Sie können in das Suchfeld zum Filtern der Connectors „SAP“ eingeben.

4. Auf der Seite **SAP BW Open Hub-Verbindung angeben** 

   ![erstellen Sie einen verknüpften SAP BW Open Hub-Dienst, und](media/load-sap-bw-data/create-sap-bw-open-hub-linked-service.png)

   1. wählen Sie **Über Integration Runtime verbinden** aus: Klicken Sie auf die Dropdownliste, um eine vorhandene, selbstgehostete IR auszuwählen, oder erstellen Sie eine, wenn Sie die selbstgehostete IR noch nicht eingerichtet haben. 

      Um sie neu zu erstellen, klicken Sie im Dropdown auf **+ Neu**, wählen Sie als Typ **Selbstgehostet** aus, geben Sie einen **Namen** an, und klicken Sie auf **Weiter**, wählen Sie **Express-Setup** aus, um auf dem aktuellen Computer zu installieren, oder führen Sie dort die **manuellen Einrichtungs**schritte aus.

      Wie unter [Voraussetzungen](#prerequisites) bereits erwähnt, stellen Sie sicher, dass Sie auch **SAP .NET Connector 3.0** auf demselben Computer installiert haben, auf dem auch die selbstgehostete IR ausgeführt wird.

   2. Geben Sie für SAP BW den **Servernamen**, die **Systemnummer**, die **Client-ID**, die **Sprache** (sofern nicht EN), den **Benutzernamen** und das **Kennwort** an.

   3. Klicken Sie auf **Verbindung testen**, um die Einstellungen zu überprüfen, und wählen Sie dann **Fertig stellen** aus.

   4. Eine neue Verbindung wird erstellt. Klicken Sie auf **Weiter**.

5. Durchsuchen Sie auf der Seite **Open Hub-Ziele auswählen** die in Ihrem SAP BW verfügbaren Open Hub-Ziele, und wählen Sie das aus, aus dem Sie Daten kopieren möchten, und klicken Sie dann auf **Weiter**.

   ![Auswählen der SAP BW Open Hub-Tabelle](media/load-sap-bw-data/select-sap-bw-open-hub-table.png)

6. Geben Sie den Filter an, falls erforderlich. Wenn Ihr Open Hub Destination nur Daten aus einer einzelnen Ausführung eines Datenübertragungsprozesses (Data Transfer Process, DTP) mit einer einzelnen Anforderungs-ID enthält, oder wenn Sie sicher sind, dass Ihr DTP fertig gestellt ist und Sie alle Daten kopieren möchten, deaktivieren Sie**Letzte Anforderung ausschließen**. Weitere Informationen dazu, wie sich diese Einstellungen auf Ihre SAP BW-Konfiguration beziehen, finden Sie unter [SAP BW Open Hub Destination-Konfigurationen](#sap-bw-open-hub-destination-configurations). Klicken Sie auf **Überprüfen**, um die zurückgegebenen Daten erneut zu überprüfen, und wählen Sie dann **Weiter** aus.

   ![Konfigurieren des SAP BW Open Hub-Filters](media/load-sap-bw-data/configure-sap-bw-open-hub-filter.png)

7. Klicken Sie auf der Seite **Zieldatenspeicher** auf **+ Neue Verbindung erstellen**, und wählen Sie anschließend **Azure Data Lake Storage Gen2** und dann **Weiter** aus.

8. Auf der Seite **Specify Azure Data Lake Storage connection** (Azure Data Lake Storage-Verbindung angeben) 

   ![erstellen Sie den verknüpften ADLS Gen2-Dienst, und](media/load-sap-bw-data/create-adls-gen2-linked-service.png)

   1. wählen Sie in der Dropdownliste „Speicherkontoname“ das Data Lake Storage Gen2-fähige Konto aus.
   2. Wählen Sie **Fertig stellen** aus, um die Verbindung zu erstellen. Klicken Sie anschließend auf **Weiter**.

9. Geben Sie auf der Seite **Ausgabedatei oder -ordner auswählen** „copyfromopenhub“ als Ausgabeordnernamen ein, und wählen Sie **Weiter** aus.

   ![Auswählen des Ausgabeordners](media/load-sap-bw-data/choose-output-folder.png)

10. Wählen Sie auf der Seite **Dateiformateinstellung** **Weiter** aus, um die Standardeinstellungen zu verwenden.

    ![Angeben des Senkenformats](media/load-sap-bw-data/specify-sink-format.png)

11. Erweitern Sie auf der Seite **Einstellungen** die **Leistungseinstellungen**, und legen Sie **Degree of copy parallelism** (Parallelitätsgrad des Kopierens) fest, z. B. 5, um parallel aus SAP BW zu laden. Klicken Sie auf **Weiter**.

    ![Konfigurieren von Kopiereinstellungen](media/load-sap-bw-data/configure-copy-settings.png)

12. Überprüfen Sie auf der Seite **Zusammenfassung** die Einstellungen, und klicken Sie dann auf **Weiter**.

13. Klicken Sie auf der Seite **Bereitstellung** auf **Überwachen**, um die Pipeline zu überwachen.

    ![Bereitstellungsseite](media/load-sap-bw-data/deployment.png)

14. Beachten Sie, dass die Registerkarte **Überwachen** auf der linken Seite automatisch ausgewählt ist. In der Spalte **Aktionen** werden Links zum Anzeigen von Aktivitätsausführungsdetails und zum erneuten Ausführen der Pipeline angezeigt:

    ![Pipelineüberwachung](media/load-sap-bw-data/pipeline-monitoring.png)

15. Klicken Sie in der Spalte **Aktionen** auf den Link **Aktivitätsausführungen anzeigen**, um mit der Pipelineausführung verknüpfte Aktivitätsausführungen anzuzeigen. Da die Pipeline nur eine Aktivität (Copy-Aktivität) enthält, wird nur ein Eintrag angezeigt. Klicken Sie oben auf den Link **Pipelines**, um zurück zur Ansicht mit den Pipelineausführungen zu wechseln. Klicken Sie zum Aktualisieren der Liste auf **Aktualisieren**.

    ![Aktivitätsüberwachung](media/load-sap-bw-data/activity-monitoring.png)

16. Zum Überwachen der Ausführungsdetails jeder Kopieraktivität klicken Sie in der Aktivitätsüberwachungsansicht unter **Aktionen** auf den Link **Details** (Brillensymbol). Sie können Details wie die Menge der Daten, die aus der Quelle in die Senke kopiert wurden, den Datendurchsatz, die Ausführungsschritte mit entsprechender Dauer sowie die verwendeten Konfigurationen überwachen:

    ![Details der Aktivitätsüberwachung](media/load-sap-bw-data/activity-monitoring-details.png)

17. Überprüfen Sie die **maximale Anforderungs-ID**, die kopiert wird. Wechseln Sie zurück zur Aktivitätsüberwachungsansicht, und klicken Sie auf die **Ausgabe** unter **Aktionen**.

    ![Aktivitätsausgabe](media/load-sap-bw-data/activity-output.png)

    ![Details der Aktivitätsausgabe](media/load-sap-bw-data/activity-output-details.png)

## <a name="incremental-copy-from-sap-bw-open-hub"></a>Inkrementelles Kopieren aus SAP BW Open Hub

> [!TIP]
>
> Weitere Informationen zur Funktionsweise von ADF SAP BW Open Hub-Connector zum Kopieren inkrementeller Daten aus SAP BW finden Sie unter [SAP BW Open Hub Connector-Deltaextraktionsflow](connector-sap-business-warehouse-open-hub.md#delta-extraction-flow). Lesen Sie diesen Artikel von Anfang an, um die Grundlagen connectorbezogener Konfigurationen zu verstehen.

Lassen Sie uns nun das inkrementelle Kopieren aus SAP BW Open Hub konfigurieren. 

Das inkrementelle Kopieren verwendet Mechanismen, die auf hohen Grenzwerten basieren und die **Anforderungs-ID** verwenden, die in SAP BW Open Hub Destination automatisch von DTP generiert wird. Der Workflow für diesen Ansatz ist im folgenden Diagramm dargestellt:

![Workflow für inkrementelles Kopieren](media/load-sap-bw-data/incremental-copy-workflow.png)

Wählen Sie auf der ADF-Benutzeroberflächenseite **Erste Schritte** **Create pipeline from template** (Pipeline aus Vorlage erstellen) aus, um die integrierte Vorlage zunutzen. 

1. Suchen Sie nach „SAP BW“, um die Vorlage mit dem Namen **Incremental copy from SAP BW to Azure Data Lake Storage Gen2** (Inkrementelles Kopieren aus SAP BW in Azure Data Lake Storage Gen2) zu finden und auszuwählen. Diese Vorlage kopiert Daten in ADLS Gen2. Sie können später dem ähnlichen Ablauf folgen, um in andere Senkentypen zu kopieren.

2. Wählen Sie auf der Hauptseite des Vorlage die folgenden drei Verbindungen aus, oder erstellen Sie sie, und wählen Sie dann unten rechts **Use this template**  (Diese Vorlage verwenden) aus.

   - **Azure-Blob**: In dieser exemplarischen Vorgehensweise verwenden wir Azure-Blob, um den hohen Grenzwert zu speichern, der die maximale Anzahl der kopierten Anforderungs-IDs ist.
   - **SAP BW Open Hub**: Ihre Quelle, aus der Daten kopiert werden sollen. Detaillierte Konfigurationen finden Sie in der vorherigen exemplarischen Vorgehensweise zum vollständigen Kopieren.
   - **ADLS Gen2**: Ihre Senke, in die Daten kopiert werden sollen. Detaillierte Konfigurationen finden Sie in der vorherigen exemplarischen Vorgehensweise zum vollständigen Kopieren.

   ![Inkrementelles Kopieren aus SAP BW-Vorlage](media/load-sap-bw-data/incremental-copy-from-sap-bw-template.png)

3. Diese Vorlage generiert eine Pipeline mit drei Aktivitäten – **Suchen, Daten kopieren und Web**, und verkettet sie bei Erfolg. Wechseln Sie zur Pipelineregisterkarte **Parameter**, wo alle Konfigurationen angezeigt werden, die Sie bereitstellen müssen.

   ![Inkrementelles Kopieren aus SAP BW-Konfiguration](media/load-sap-bw-data/incremental-copy-from-sap-bw-pipeline-config.png)

   - **SAPOpenHubDestinationName**: Geben Sie den Namen der Open Hub-Tabelle an, aus der Daten kopiert werden sollen.

   - **ADLSGen2SinkPath**: Geben Sie den ADLS Gen2-Zielpfad an, in den Daten kopiert werden sollen. Wenn der Pfad nicht vorhanden ist, erstellt die Aktivität „ADF Copy“ einen während der Ausführung.

   - **HighWatermarkBlobPath**: Geben Sie den Pfad zum Speichern des hohen Grenzwerts an, z. B. `container/path`. 

   - **HighWatermarkBlobName**: Geben Sie den Namen des Blobs zum Speichern des hohen Grenzwerts an, z. B. `requestIdCache.txt`. Erstellen Sie in Ihrem Blob-Speicher im entsprechenden Pfad von „HighWatermarkBlobPath+HighWatermarkBlobName“, z. B. „*container/path/requestIdCache.txt*“ einen Blob mit Inhalt 0. 

      ![Blobinhalt](media/load-sap-bw-data/blob.png)

   - **LogicAppURL**: In dieser Vorlage verwenden wir eine Webaktivität zum Aufrufen von Logic Apps, um den hohen Grenzwert im Blob-Speicher festzulegen. Alternativ können Sie auch SQL-Datenbank verwenden, um ihn zu speichern und eine „Gespeicherte Prozedur“-Aktivität verwenden, um den Wert zu aktualisieren. 

      Hier müssen Sie zuerst eine Logik-App wie die folgende erstellen und dann die **HTTP POST URL** in dieses Feld kopieren. 

      ![Konfiguration der Logik-App](media/load-sap-bw-data/logic-app-config.png)

      1. Wechseln Sie zum Azure-Portal, „Neu“, wählen Sie einen **Logic Apps**-Dienst aus, klicken Sie auf **+Leere Logik-App**, um zum **Designer für Logik-Apps** zu wechseln.

      2. Erstellen Sie einen Trigger **Beim Empfang einer HTTP-Anforderung**. Geben Sie den HTTP-Anforderungstext wie folgt an:

         ```json
         {
            "properties": {
               "sapOpenHubMaxRequestId": {
                  "type": "string"
               },
               "type": "object"
            }
         }
         ```

      3. Fügen Sie die Aktion **Blob erstellen** hinzu. Verwenden Sie für „Ordnerpfad“ und „Blob-Name“ dieselben Werte, die Sie in den oben aufgeführten „HighWatermarkBlobPath“ und „HighWatermarkBlobName“ konfiguriert haben.

      4. Klicken Sie auf **Speichern**, und kopieren Sie den Wert von **HTTP POST URL**, um ihn in der ADF-Pipeline zu verwenden.

4. Nachdem Sie alle Werte für die ADF-Pipelineparameter angegeben haben, können Sie auf **Debuggen** -> **Fertig stellen** klicken, um eine Ausführung aufzurufen, um die Konfiguration zu überprüfen. Oder Sie können **alle veröffentlichen** auswählen, um alle Änderungen zu veröffentlichen, und dann auf **Trigger** klicken, um eine Ausführung auszuführen.

## <a name="sap-bw-open-hub-destination-configurations"></a>SAP BW Open Hub Destination-Konfigurationen

In diesem Abschnitt wird die erforderliche Konfiguration auf der SAP BW-Seite eingeführt, um den SAP BW Open Hub-Connector in ADF zu verwenden, um Daten zu kopieren.

### <a name="configure-delta-extraction-in-sap-bw"></a>Konfigurieren der Deltaextraktion in SAP BW

Ob Sie sowohl historisches Kopieren als auch inkrementelles Kopieren oder nur inkrementelles Kopieren benötigen, konfigurieren Sie Deltaextraktion in SAP BW.

1. Erstellen von Open Hub Destination (OHD, Open Hub-Ziel)

   Sie können das OHD in SAP Transaction RSA1 erstellen, das automatisch die erforderliche Transformation und den Datenübertragungsprozess (Data Transfer Process, DTP) erstellt. Verwenden Sie folgende Einstellungen:

   - Der Objekttyp kann beliebig sein. Hier verwenden wir InfoCube als Beispiel.
   - **Zieltyp**: *Datenbanktabelle*
   - **Schlüssel der Tabelle:** *Technischer Schlüssel*
   - **Extraktion**: *Daten beibehalten und Datensätze in Tabelle einfügen*

   ![Erstellen der SAP BW OHD-Deltaextraktion](media/load-sap-bw-data/create-sap-bw-ohd-delta.png)

   ![create-sap-bw-ohd-delta2](media/load-sap-bw-data/create-sap-bw-ohd-delta2.png)

   Sie können die Anzahl parallel ausgeführter SAP-Arbeitsprozesse für die DTP erhöhen:

   ![create-sap-bw-ohd-delta3](media/load-sap-bw-data/create-sap-bw-ohd-delta3.png)

2. Planen des DTP in Prozessketten

   Ein Delta-DTP für einen Cube funktioniert nur, wenn die erforderlichen Zeilen noch nicht komprimiert wurden. Daher müssen Sie sicherstellen, dass die BW-Cube-Komprimierung nicht ausgeführt wird vor der DTP zur Open Hub-Tabelle. Die einfachste Möglichkeit hierfür ist die Integrierung dieses DTP in Ihre vorhandenen Prozessketten. Im folgenden Beispiel wird der DTP (zum OHD) in die Prozesskette zwischen dem Schritt „Anpassen“ (Rollup aggregieren) und „Reduzieren“ (Cube-Komprimierung) eingefügt.

   ![create-sap-bw-process-chain](media/load-sap-bw-data/create-sap-bw-process-chain.png)

### <a name="configure-full-extraction-in-sap-bw"></a>Konfigurieren der vollständigen Extraktion in SAP BW

Zusätzlich zur Deltaextraktion können Sie auch eine vollständige Extraktion desselben InfoProvider einrichten. Dies trifft in der Regel zu, wenn Sie vollständig kopieren möchten, weil Sie keinen Bedarf für inkrementelles Kopieren haben oder die [Deltaextraktion neu synchronisieren](#re-sync-delta-extraction) möchten.

Sie dürfen nicht mehr als einen DTP für dasselbe OHD hben. Deshalb müssen Sie ein zusätzliches OHD erstellen, dann die Deltaextraktion.

![create-sap-bw-ohd-full](media/load-sap-bw-data/create-sap-bw-ohd-full.png)

Wählen Sie für ein OHD mit vollständigem Ladevorgang andere Optionen aus als für die Deltaextraktion:

- Im OHD: Legen Sie die Option „Extraction“ (Extraktion) auf „*Delete Data and Insert Records*“ (Daten löschen und Datensätze einfügen) fest. Andernfalls würden die Daten viele Male extrahiert, wenn der DTP in einer BW-Prozesskette wiederholt wird.

- Im DTP: Legen Sie „Extraction Mode“ (Extraktionsmodus) auf „*Full*“ (Vollständig) fest. Sie müssen den automatisch erstellten DTP unmittelbar nach dem Erstellen des OHD von „Delta“ auf „Full“ ändern:

   ![create-sap-bw-ohd-full2](media/load-sap-bw-data/create-sap-bw-ohd-full2.png)

- Im ADF SAP BW Open Hub-Connector: Deaktivieren Sie die Option „*Exclude last request*“ (Letzte Anforderung ausschließen). Andernfalls würde nichts extrahiert. 

Den vollständigen DTP („Full“) führen Sie in der Regel manuell aus. Alternativ können Sie auch eine Prozesskette für den vollständigen DTP erstellen. Dies wäre normalerweise eine separate Prozesskette, die von Ihren vorhandenen Prozessketten unabhängig ist. In beiden Fällen müssen Sie **sicherstellen, dass der DTP fertig gestellt wurde, bevor die Extraktion mittels „ADF Copy“ gestartet wird**, andernfalls werden nur Teile der Daten kopiert.

### <a name="run-delta-extraction-the-first-time"></a>Erstmalige Ausführung der Deltaextraktion

Die erste Deltaextraktion ist technisch gesehen eine **vollständige Extraktion**. Beachten Sie, das ADF SAP BW Open Hub-Connector standardmäßige die letzte Anforderung beim Kopieren der Daten ausschließt. Im Fall der erstmaligen Deltaextraktion werden in der Aktion „ADF Copy“ erst dann Daten extrahiert, nachdem nachfolgende DTP Deltadaten mit einer gesonderten Anforderungs-ID in der Tabelle generiert haben. Es gibt zwei Methoden, um dieses Szenario zu vermeiden:

1. Deaktivieren Sie die Option „Exclude last request“ (Letzte Anforderung ausschließen) für die erste Deltaextraktion. In diesem Fall müssen Sie sicherstellen, dass der erste Delta-DTP fertig gestellt ist, bevor die Deltaextraktion erstmalig gestartet wird.
2. Verwenden Sie das Verfahren zur erneuten Synchronisierung der Deltaextraktion, wie unten beschrieben.

### <a name="re-sync-delta-extraction"></a>Erneutes Synchronisieren der Deltaextraktion

Es gibt ein paar Szenarien, in denen die Daten in SAP BW Cubes geändert, vom Delta-DTP aber nicht berücksichtigt werden:

- SAP BW Selective Deletion (selektives Löschen; von Zeilen, die eine beliebige Filterbedingung verwenden)
- SAP BW Request Deletion (Anforderung löschen; von fehlerhaften Anforderungen)

Ein SAP Open Hub Destination ist kein Data Mart-kontrolliertes Datenziel (in allen SAP BW-Supportpaketen seit dem Jahr 2015). Aus diesem Grund ist es möglich, Daten aus einem Cube zu löschen, ohne die Daten im OHD zu ändern. In diesem Fall müssen Sie die Daten des Cubes mit den Daten in ADF erneut synchronisieren, indem Sie die folgenden Schritte ausführen:

1. Ausführen einer vollständige Extraktion in ADF (mithilfe eines vollständigen DTP in SAP)
2. Löschen aller Zeilen in der Open Hub-Tabelle für den Delta-DTP
3. Festlegen des Status des abzurufenden Delta-DTP

Anschließend funktionieren alle nachfolgenden Delta-DTPs und ADF-Deltaextraktionen problemlos wie erwartet.

Sie können den Status des abzurufenden Delta-DTP festlegen, indem Sie den Delta-DTP manuell mithilfe der folgenden Option ausführen: „*No Data Transfer; Delta Status in Source: Fetched*“ (Keine Datenübertragung; Deltastatus in Quelle: Abgerufen).

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie den folgenden Artikel, um mehr über die Unterstützung von SAP BW Open Hub-Connector zu erfahren: 

> [!div class="nextstepaction"]
>[SAP Business Warehouse Open Hub-Connector](connector-sap-business-warehouse-open-hub.md)
