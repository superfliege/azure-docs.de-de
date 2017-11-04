---
title: Suchen von unstrukturierten Daten in Azure-Cloudspeicher
description: Suchen von unstrukturierten Daten mithilfe von Azure Search
author: roygara
manager: timlt
services: storage
ms.service: storage
ms.topic: tutorial
ms.date: 10/12/2017
ms.author: rogara
ms.custom: mvc
ms.openlocfilehash: 930b735eb03aea6ce701b694ca527049b4c3f24d
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2017
---
# <a name="search-unstructured-data-in-cloud-storage"></a>Suchen von unstrukturierten Daten in Cloudspeicher

In diesem Tutorial erfahren Sie, wie Sie mit [Azure Search](../../search/search-what-is-azure-search.md) unstrukturierte Daten mithilfe von Daten suchen, die in Azure-Blobs gespeichert werden. Unstrukturierte Daten sind Daten, die entweder nicht in vordefinierter Weise angeordnet sind oder nicht über ein Datenmodell verfügen. Ein Beispiel wäre eine TXT-Datei.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Ressourcengruppe
> * Erstellen Sie ein Speicherkonto.
> * Erstellen eines Containers
> * Hochladen von Daten in Ihren Container
> * Erstellen eines Azure Search-Diensts über das Azure-Portal
> * Verwenden des Azure Search-Diensts zum Suchen Ihres Containers

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Es wurde ein Beispieldataset für Sie vorbereitet. **Laden Sie [clinical-trials.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials.zip)** herunter, und entzippen Sie die Datei in ihrem eigenen Ordner.

Das Beispiel besteht aus Textdateien, die von [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results) abgerufen wurden. Sie können sie als Beispieltextdateien für die Suche mit Azure verwenden.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.

## <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto.

Ein Speicherkonto stellt einen eindeutigen Speicherort zum Speichern Ihrer Azure Storage-Datenobjekte sowie für den Zugriff darauf bereit.

Aktuell gibt es zwei Arten von Speicherkonten: **Blob** und **Allgemein**. Für dieses Tutorial erstellen Sie ein Speicherkonto vom Typ **Allgemein**.

Wenn Sie mit dem Erstellen eines allgemeinen Speicherkontos nicht vertraut sind, folgen hier die erforderlichen Schritte:

1. Wählen Sie im linken Menü **Speicherkonten** aus, und wählen Sie dann **Hinzufügen** aus.

2. Geben Sie einen eindeutigen Namen für Ihr Speicherkonto ein. 

3. Wählen Sie **Ressourcen-Manager** für Ihr **Bereitstellungsmodell** und dann **Allgemein** aus der Dropdownliste **Kontoart** aus.

4. Wählen Sie in der Dropdownliste **Replikation** den Wert **Lokal redundanter Speicher** aus.

5. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus, und geben Sie einen eindeutigen Namen ein.

6. Wählen Sie ein entsprechendes Abonnement aus.

7. Wählen Sie einen Speicherort und dann **Erstellen** aus.

  ![Unstrukturierte Suche](media/storage-unstructured-search/storagepanes2.png)

## <a name="create-a-container"></a>Erstellen eines Containers

Container ähneln Ordnern und werden zum Speichern von Blobs verwendet.

Für dieses Tutorial verwenden Sie einen einzelnen Container zum Speichern der Textdateien, die Sie von „clinicaltrials.gov“ abgerufen haben.

1. Navigieren Sie zum Speicherkonto im Azure-Portal.

2. Wählen Sie unter **Blob-Dienst** die Option **Blobs durchsuchen** aus.

3. Fügen Sie einen neuen Container hinzu.

4. Benennen Sie den Container "Daten", und wählen Sie **Container** für die öffentliche Zugriffsebene aus.

5. Wählen Sie **OK** aus, um den Container zu erstellen. 

  ![Unstrukturierte Suche](media/storage-unstructured-search/storageactinfo.png)

## <a name="upload-the-example-data"></a>Hochladen der Beispieldaten

Nachdem Sie jetzt über einen Container verfügen, können Sie die Beispieldaten in den Container hochladen.

1. Wählen Sie den Container und dann **Hochladen** aus.

2. Wählen Sie das blaue Ordnersymbol neben dem Feld „Dateien“ aus, und navigieren Sie zum lokalen Ordner, in dem Sie die Beispieldaten extrahiert haben.

3. Wählen Sie alle extrahierten Dateien und dann **Öffnen** aus.

4. Wählen Sie **Hochladen** aus, um mit dem Hochladen zu beginnen.

  ![Unstrukturierte Suche](media/storage-unstructured-search/upload.png)

Das Hochladen kann einen Moment dauern.

Nachdem der Vorgang abgeschlossen ist, navigieren Sie zurück in Ihren Datencontainer, um das Hochladen der Textdateien zu bestätigen.

  ![Unstrukturierte Suche](media/storage-unstructured-search/clinicalfolder.png)

## <a name="create-a-search-service"></a>Erstellen eines Suchdiensts

Azure Search ist eine Search-as-a-Service-Cloudlösung, die Entwicklern APIs und Tools zum Hinzufügen von umfangreichen Suchfunktionen für Ihre Daten in Web- und Unternehmensanwendungen sowie in mobilen Anwendungen bietet.

Wenn Sie mit dem Erstellen eines Suchdiensts nicht vertraut sind, folgen hier die erforderlichen Schritte:

1. Navigieren Sie zum Speicherkonto im Azure-Portal.

2. Scrollen Sie nach unten, und klicken Sie unter **BLOB-DIENST** auf **Azure Search hinzufügen**.

3. Wählen Sie unter **Daten importieren** die Option **Dienst auswählen** aus.

4. Wählen Sie **Hier klicken, um einen neuen Suchdienst zu erstellen** aus.

5. Geben Sie in **Neuer Suchdienst** einen eindeutigen Namen für Ihren Suchdienst in das Feld **URL** ein.

6. Wählen Sie unter **Ressourcengruppe** die Option **Vorhandene verwenden** und dann die Ressourcengruppe aus, die Sie zuvor erstellt haben.

7. Wählen Sie für **Tarif** den **Free**-Tarif aus, und klicken Sie dann auf **Auswählen**.

8. Wählen Sie **Erstellen** aus, um den Suchdienst zu erstellen.

  ![Unstrukturierte Suche](media/storage-unstructured-search/createsearch2.png)

## <a name="connect-your-search-service-to-your-container"></a>Verbinden des Azure Search-Diensts mit Ihrem Container

Nachdem Sie jetzt über einen Suchdienst verfügen, können Sie ihn an Ihren Blobspeicher anfügen. Dieser Abschnitt führt Sie durch den Prozess zum Auswählen einer Datenquelle, zum Erstellen eines Index und zum Erstellen eines Indexers.

1. Navigieren Sie zum Speicherkonto.

2. Wählen Sie unter **BLOB-DIENST** die Option **Azure Search hinzufügen** aus.

3. Wählen Sie in **Daten importieren** die Option **Suchdienst** aus, und klicken Sie dann auf den Suchdienst, den Sie im vorherigen Abschnitt erstellt haben. Dadurch wird **Neue Datenquelle** geöffnet.

### <a name="new-data-source"></a>Neue Datenquelle

  Eine Datenquelle gibt an, welche Daten indiziert werden und wie der Zugriff auf die Daten erfolgt. Eine Datenquelle kann von demselben Suchdienst mehrmals verwendet werden.

1. Geben Sie einen Namen für die Datenquelle ein. Wählen Sie unter **Zu extrahierende Daten** die Option **Inhalt und Metadaten** aus. Die Datenquelle gibt an, welche Teile des Blobs indiziert werden.
    
    a. In Ihren eigenen zukünftigen Szenarien können Sie auch **Nur Speichermetadaten** auswählen. Sie würden diese Auswahl treffen, wenn Sie die zu indizierenden Daten auf standardmäßige Blob-Eigenschaften oder benutzerdefinierte Eigenschaften einschränken möchten.
    
    b. Sie könne auch **Alle Metadaten** auswählen, um sowohl standardmäßige Blob-Eigenschaften als auch *alle* inhaltstypspezifische Metadaten abzurufen. 

2. Da es sich bei den von Ihnen verwendeten Blobs um Textdateien handelt, legen Sie für **Analysemodus** den Wert **Text** fest.
    
    a. In eigenen zukünftigen Szenarien möchten Sie möglicherweise in Abhängigkeit von den Inhalten Ihrer Blobs [andere Analysemodi](../../search/search-howto-indexing-azure-blob-storage.md) auswählen.

  ![Unstrukturierte Suche](media/storage-unstructured-search/datasources.png)

3. Wählen Sie **Speichercontainer** aus, um die verfügbaren Speicherkonten aufzulisten.

4. Wählen Sie Ihr Speicherkonto und dann den Container aus, den Sie zuvor erstellt haben.

5. Klicken Sie auf **Auswählen**, um zu **Neue Datenquelle** zurückzukehren, und wählen Sie dann **OK** aus, um den Vorgang fortzusetzen.

  ![Unstrukturierte Suche](media/storage-unstructured-search/datacontainer.png)

### <a name="configure-the-index"></a>Konfigurieren des Index

  Ein Index besteht aus einer Sammlung von Feldern aus Ihrer Datenquelle, die durchsucht werden können. Der Index teilt dem Suchdienst mit, auf welche Weise Ihre Daten durchsucht werden sollen.

1. Wählen Sie in **Daten importieren** die Option **Zielindex anpassen** aus.
 
2. Geben Sie im Feld **Indexname** einen Namen für Ihren Index ein.

3. Aktivieren Sie das Kontrollkästchen **Abrufbar** des Attributs unter **metadata_storage_name**.

  ![Unstrukturierte Suche](media/storage-unstructured-search/valuestoselect.png)

4. Klicken Sie auf **OK**, um **Indexer erstellen** zu öffnen.

Die Parameter des Index und die Attribute, die Sie diesen Parametern zuweisen, sind wichtig. Die Parameter geben an, *welche* Daten gespeichert werden, während die Attribute angeben, *wie* die Daten gespeichert werden.

Die Spalte **FELDNAME** enthält die Parameter. Die folgende Tabelle enthält eine Liste der verfügbaren Attribute und deren Beschreibungen.

### <a name="field-attributes"></a>Feldattribute
| Attribut | Beschreibung |
| --- | --- |
| *Schlüssel* |Eine Zeichenfolge, die die eindeutige ID der einzelnen Dokumente darstellt und für die Dokumentsuche verwendet wird. Jeder Index muss über einen Schlüssel verfügen. Als Schlüssel kann immer nur ein einzelnes Feld fungieren, und sein Typ muss auf „Edm.String“ festgelegt sein. |
| *Abrufbar* |Gibt an, ob ein Feld in einem Suchergebnis zurückgegeben werden kann. |
| *Filterbar* |Ermöglicht die Verwendung des Felds in Filterabfragen. |
| *Sortierbar* |Ermöglicht einer Abfrage das Sortieren von Suchergebnissen mithilfe dieses Felds. |
| *Facettierbar* |Ermöglicht die Verwendung eines Felds in einer Facettennavigationsstruktur für benutzerdefiniertes Filtern. Repetitive Werte, mit denen sich mehrere Dokumente zu einer Gruppe zusammenfassen lassen (etwa mehrere Dokumente der gleichen Marken- oder Dienstleistungskategorie), sind in der Regel am besten für die Verwendung als Facetten geeignet. |
| *Durchsuchbar* |Markiert das Feld als in die Volltextsuche einbeziehbar. |


### <a name="create-an-indexer"></a>Erstellen eines Indexers
    
  Ein Indexer verbindet eine Datenquelle mit einem Suchindex und stellt einen Zeitplan zum erneuten Indizieren Ihrer Daten bereit.

1. Geben Sie einen Namen in das Feld **Name** ein, und wählen Sie **OK** aus.

  ![Unstrukturierte Suche](media/storage-unstructured-search/exindexer.png)

2. Sie gelangen zurück zu **Daten importieren**. Wählen Sie **OK** aus, um den Verbindungsprozess abzuschließen.

Sie haben Ihren Blob jetzt erfolgreich mit dem Suchdienst verbunden. Es dauert einige Minuten, bis das Portal anzeigt, dass der Index aufgefüllt ist. Der Suchdienst beginnt jedoch sofort mit der Indizierung, daher können Sie unmittelbar mit der Suche beginnen.

## <a name="search-your-text-files"></a>Suchen Ihrer Textdateien

Öffnen Sie den Suchexplorer innerhalb des Index Ihres neu erstellten Suchdiensts, um Ihre Dateien zu suchen.

Die folgenden Schritte zeigen Ihnen, wo Sie den Suchexplorer finden. Zudem werden einige Beispielabfragen bereitgestellt:

1. Navigieren Sie zu allen Ressourcen, und suchen Sie Ihren neu erstellten Suchdienst.

  ![Unstrukturierte Suche](media/storage-unstructured-search/exampleurl.png)

3. Wählen Sie Ihren Index aus, um ihn zu öffnen. 

  ![Unstrukturierte Suche](media/storage-unstructured-search/overview.png)

4. Wählen Sie **Suchexplorer** aus, um den Suchexplorer zu öffnen, in dem Sie Liveabfragen für Ihre Daten ausführen können.

  ![Unstrukturierte Suche](media/storage-unstructured-search/indexespane.png)

5. Wählen Sie **Suchen** aus, während das Feld für die Abfragezeichenfolge leer ist. Eine leere Abfrage gibt *alle* Daten aus Ihren Blobs zurück.

  ![Unstrukturierte Suche](media/storage-unstructured-search/emptySearch.png)

### <a name="full-text-search"></a>Volltextsuche 

Geben Sie „Kurzsichtigkeit“ in das Feld **Abfragezeichenfolge** ein, und wählen Sie dann **Suchen** aus. Dadurch wird eine Suche über den Inhalt der Dateien initiiert und eine Teilmenge des Inhalts zurückgegeben, die das Wort „Kurzsichtigkeit“ enthält.

  ![Unstrukturierte Suche](media/storage-unstructured-search/secondMyopia.png) 

### <a name="system-properties-search"></a>Suchen von Systemeigenschaften

Sie können auch Abfragen erstellen, die mithilfe des Parameters `$select` nach Systemeigenschaften suchen. Geben Sie `$select=metadata_storage_name` für die Abfragezeichenfolge ein, und drücken Sie die EINGABETASTE, wodurch nur das betreffende Feld zurückgegeben wird.
    
Die URL wird direkt von der Abfragezeichenfolge geändert, daher sind Leerzeichen nicht zulässig. Verwenden Sie ein Komma, um mehrere Felder zu suchen. Beispiel: `$select=metadata_storage_name,metadata_storage_path`
    
Der Parameter `$select` kann nur mit Feldern verwendet werden, die beim Definieren des Index als „abrufbar“ markiert wurden.

  ![Unstrukturierte Suche](media/storage-unstructured-search/metadatasearchunstructured.png) 

Sie haben dieses Tutorial jetzt abgeschlossen und verfügen über einen durchsuchbaren Satz von unstrukturierten Daten.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zum Suchen von unstrukturierten Daten mit Azure Search erhalten. Dazu zählen die folgenden Schritte:

> [!div class="checklist"]
> * Erstellen einer Ressourcengruppe
> * Erstellen Sie ein Speicherkonto.
> * Erstellen eines Containers
> * Hochladen von Daten in Ihren Container
> * Erstellen eines Suchdiensts
> * Verwenden des Suchdiensts zum Suchen Ihres Containers

Folgen Sie diesem Link, um weitere Informationen zum Indizieren von Dokumenten mit Azure Search zu erhalten.

> [!div class="nextstepaction"]
> [Indizieren von Dokumenten in Azure Blob Storage mit Azure Search](../../search/search-howto-indexing-azure-blob-storage.md)