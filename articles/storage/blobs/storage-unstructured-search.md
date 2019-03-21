---
title: 'Tutorial: Suchen nach unstrukturierten Daten in Azure Blob Storage'
description: 'Tutorial: Suchen nach unstrukturierten Daten in Blob Storage mithilfe von Azure Search'
author: roygara
services: storage
ms.service: storage
ms.topic: tutorial
ms.date: 12/13/2018
ms.author: rogarana
ms.custom: mvc
ms.openlocfilehash: acf01788a1ac8ed8ca476941c402801abfbd8adf
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58011306"
---
# <a name="tutorial-search-unstructured-data-in-cloud-storage"></a>Tutorial: Suchen von unstrukturierten Daten in Cloudspeicher

In diesem Tutorial erfahren Sie, wie Sie mit [Azure Search](../../search/search-what-is-azure-search.md) anhand von gespeicherten Daten in Azure Blob Storage nach unstrukturierten Daten suchen. Unstrukturierte Daten sind Daten, die entweder nicht in vordefinierter Weise angeordnet sind oder nicht über ein Datenmodell verfügen. Ein Beispiel ist eine TXT-Datei.

Dieses Tutorial setzt voraus, dass Sie über ein Azure-Abonnement verfügen. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Ressourcengruppe
> * Speicherkonto erstellen
> * Erstellen eines Containers
> * Hochladen von Daten in Ihren Container
> * Erstellen eines Azure Search-Diensts über das Azure-Portal
> * Verbinden eines Suchdiensts mit einem Speicherkonto
> * Erstellen einer Datenquelle
> * Konfigurieren des Index
> * Erstellen eines Indexers
> * Verwenden des Azure Search-Diensts zum Suchen Ihres Containers

## <a name="prerequisites"></a>Voraussetzungen

Jedes Speicherkonto muss zu einer Azure-Ressourcengruppe gehören. Eine Ressourcengruppe ist ein logischer Container zur Gruppierung Ihrer Azure-Dienste. Beim Erstellen eines Speicherkontos haben Sie die Möglichkeit, eine neue Ressourcengruppe zu erstellen oder eine vorhandene Ressourcengruppe zu verwenden. In diesem Tutorial wird eine neue Ressourcengruppe erstellt.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

Es wurde ein Beispieldataset vorbereitet, das Sie für dieses Tutorial verwenden können. Laden Sie [clinical-trials.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials.zip) herunter, und entzippen Sie die Datei in ihrem eigenen Ordner.

Das Beispiel besteht aus Textdateien, die von [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results) abgerufen wurden. In diesem Tutorial werden die Dateien als Beispieltextdateien verwendet und mithilfe von Azure Search-Diensten durchsucht.

## <a name="create-a-container"></a>Erstellen eines Containers

Container ähneln Ordnern und werden zum Speichern von Blobs verwendet.

Für dieses Tutorial verwenden Sie einen einzelnen Container zum Speichern der Textdateien, die Sie von „clinicaltrials.gov“ abgerufen haben.

1. Wechseln Sie im Azure-Portal zu Ihrem Speicherkonto.

2. Wählen Sie unter **Blob-Dienst** die Option **Blobs durchsuchen** aus.

3. Fügen Sie einen neuen Container hinzu.

4. Geben Sie für den Container den Namen **data** ein, und wählen Sie für „Öffentliche Zugriffsebene“ die Option **Container** aus.

5. Wählen Sie **OK** aus, um den Container zu erstellen.

   ![Unstrukturierte Suche](media/storage-unstructured-search/storageactinfo.png)

## <a name="upload-the-example-data"></a>Hochladen der Beispieldaten

Nachdem Sie jetzt über einen Container verfügen, können Sie die Beispieldaten in den Container hochladen.

1. Wählen Sie den Container und dann **Hochladen** aus.

2. Klicken Sie auf das blaue Ordnersymbol neben dem Feld **Dateien**, und navigieren Sie zu dem lokalen Ordner, in dem Sie die Beispieldaten extrahiert haben.

3. Wählen Sie alle extrahierten Dateien aus, und klicken Sie dann auf **Öffnen**.

4. Wählen Sie **Hochladen** aus, um mit dem Hochladen zu beginnen.

   ![Unstrukturierte Suche](media/storage-unstructured-search/upload.png)

Der Uploadprozess kann einen Moment dauern.

Kehren Sie nach Abschluss des Uploads zu Ihrem Datencontainer zurück, und überprüfen Sie, ob die Textdateien hochgeladen wurden.

  ![Unstrukturierte Suche](media/storage-unstructured-search/clinicalfolder.png)

## <a name="create-a-search-service"></a>Erstellen eines Suchdiensts

Azure Search ist eine Search-as-a-Service-Cloudlösung, die Entwicklern APIs und Tools zum Hinzufügen von Suchfunktionen für Ihre Daten bietet.

In diesem Tutorial verwenden Sie einen Suchdienst, um die aus „clinicaltrials.gov“ abgerufenen Textdateien zu durchsuchen.

1. Wechseln Sie im Azure-Portal zu Ihrem Speicherkonto.

2. Scrollen Sie nach unten, und wählen Sie unter **BLOB-DIENST** die Option **Azure Search hinzufügen** aus.

3. Wählen Sie unter **Daten importieren** die Option **Dienst auswählen** aus.

4. Wählen Sie **Hier klicken, um einen neuen Suchdienst zu erstellen** aus.

5. Geben Sie in **Neuer Suchdienst** einen eindeutigen Namen für Ihren Suchdienst in das Feld **URL** ein.

6. Aktivieren Sie unter **Ressourcengruppe** das Optionsfeld **Vorhandene verwenden**, und wählen Sie die zuvor erstellte Ressourcengruppe aus.

7. Wählen Sie für **Tarif** den **Free**-Tarif aus, und klicken Sie dann auf **Auswählen**.

8. Wählen Sie **Erstellen** aus, um den Suchdienst zu erstellen.

   ![Unstrukturierte Suche](media/storage-unstructured-search/createsearch2.png)

## <a name="connect-your-search-service-to-your-container"></a>Verbinden des Azure Search-Diensts mit Ihrem Container

Nachdem Sie jetzt über einen Suchdienst verfügen, können Sie ihn an Ihren Blobspeicher anfügen. Dieser Abschnitt führt Sie durch den Prozess zum Auswählen einer Datenquelle, zum Erstellen eines Index und zum Erstellen eines Indexers.

1. Wechseln Sie zum Speicherkonto.

2. Wählen Sie unter **BLOB-DIENST** die Option **Azure Search hinzufügen** aus.

3. Wählen Sie unter **Daten importieren** die Option **Suchdienst** aus, und klicken Sie dann auf den Suchdienst, den Sie im vorherigen Abschnitt erstellt haben. Der Bereich **Neue Datenquelle** wird geöffnet.

### <a name="create-a-data-source"></a>Erstellen einer Datenquelle

  Eine Datenquelle gibt an, welche Daten indiziert werden und wie der Zugriff auf die Daten erfolgt. Eine Datenquelle kann von demselben Suchdienst mehrmals verwendet werden.

1. Geben Sie einen Namen für die Datenquelle ein. Wählen Sie unter **Zu extrahierende Daten** die Option **Inhalt und Metadaten** aus. Die Datenquelle gibt an, welche Teile des Blobs indiziert werden.

2. Da es sich bei den von Ihnen verwendeten Blobs um Textdateien handelt, legen Sie **Analysemodus** auf **Text** fest.

   ![Unstrukturierte Suche](media/storage-unstructured-search/datasources.png)

3. Wählen Sie **Speichercontainer** aus, um die verfügbaren Speicherkonten aufzulisten.

4. Wählen Sie Ihr Speicherkonto und dann den Container aus, den Sie zuvor erstellt haben.

   ![Unstrukturierte Suche](media/storage-unstructured-search/datacontainer.png)

5. Klicken Sie auf **Auswählen**, um zu **Neue Datenquelle** zurückzukehren, und wählen Sie dann **OK** aus, um fortzufahren.

### <a name="configure-the-index"></a>Konfigurieren des Index

  Ein Index besteht aus einer Sammlung von Feldern aus Ihrer Datenquelle, die durchsucht werden können. Sie konfigurieren Parameter für diese Felder, damit der Suchdienst weiß, mit welchen Methoden Ihre Daten durchsucht werden sollen.

1. Wählen Sie unter **Daten importieren** die Option **Zielindex anpassen** aus.

2. Geben Sie im Feld **Indexname** einen Namen für Ihren Index ein.

3. Aktivieren Sie unter **metadata_storage_name** das Kontrollkästchen des Attributs **Abrufbar**.

   ![Unstrukturierte Suche](media/storage-unstructured-search/valuestoselect.png)

4. Klicken Sie auf **OK**, um die Seite **Indexer erstellen** zu öffnen.

Die Parameter des Index und die Attribute, die Sie diesen Parametern zuweisen, sind wichtig. Die Parameter legen fest, *welche* Daten gespeichert werden, und die Attribute bestimmen, *wie* diese Daten gespeichert werden.

Die Spalte **FELDNAME** enthält die Parameter. Die folgende Tabelle enthält eine Liste der verfügbaren Attribute und deren Beschreibungen.

#### <a name="field-attributes"></a>Feldattribute

| Attribut | BESCHREIBUNG |
| --- | --- |
| *Schlüssel* |Eine Zeichenfolge, die die eindeutige ID der einzelnen Dokumente darstellt und für die Dokumentsuche verwendet wird. Jeder Index muss über einen Schlüssel verfügen. Als Schlüssel kann immer nur ein einzelnes Feld fungieren, und sein Typ muss auf „Edm.String“ festgelegt sein. |
| *Abrufbar* |Gibt an, ob ein Feld in einem Suchergebnis zurückgegeben werden kann. |
| *Filterbar* |Ermöglicht die Verwendung des Felds in Filterabfragen. |
| *Sortierbar* |Ermöglicht einer Abfrage das Sortieren von Suchergebnissen mithilfe dieses Felds. |
| *Facettierbar* |Ermöglicht die Verwendung eines Felds in einer Facettennavigationsstruktur für benutzerdefiniertes Filtern. Felder mit repetitiven Werten, die zum Gruppieren von Dokumenten verwendet werden können (z. B. mehrere Dokumente der gleichen Marken- oder Dienstkategorie), eignen sich in der Regel am besten als Facetten. |
| *Durchsuchbar* |Markiert das Feld als in die Volltextsuche einbeziehbar. |

### <a name="create-an-indexer"></a>Erstellen eines Indexers

  Ein Indexer verbindet eine Datenquelle mit einem Suchindex und stellt einen Zeitplan zum erneuten Indizieren Ihrer Daten bereit.

1. Geben Sie einen Namen in das Feld **Name** ein, und wählen Sie **OK** aus.

   ![Unstrukturierte Suche](media/storage-unstructured-search/exindexer.png)

2. Der Bereich **Daten importieren** wird wieder angezeigt. Wählen Sie **OK** aus, um den Verbindungsprozess abzuschließen.

Sie haben Ihren Blob jetzt erfolgreich mit dem Suchdienst verbunden. Es dauert einige Minuten, bis das Portal anzeigt, dass der Index aufgefüllt ist. Der Suchdienst beginnt jedoch sofort mit der Indizierung, daher können Sie unmittelbar mit der Suche beginnen.

## <a name="search-your-text-files"></a>Suchen Ihrer Textdateien

Öffnen Sie den Suchexplorer innerhalb des Index Ihres neu erstellten Suchdiensts, um Ihre Dateien zu suchen.

Die folgenden Schritte zeigen Ihnen, wo Sie den Suchexplorer finden. Zudem werden einige Beispielabfragen bereitgestellt:

1. Navigieren Sie zu „Alle Ressourcen“, und suchen Sie nach dem neu erstellten Suchdienst.

   ![Unstrukturierte Suche](media/storage-unstructured-search/exampleurl.png)

2. Wählen Sie Ihren Index aus, um ihn zu öffnen.

   ![Unstrukturierte Suche](media/storage-unstructured-search/overview.png)

3. Wählen Sie **Suchexplorer** aus, um den Suchexplorer zu öffnen, in dem Sie Liveabfragen für Ihre Daten ausführen können.

   ![Unstrukturierte Suche](media/storage-unstructured-search/indexespane.png)

4. Wählen Sie **Suchen** aus, während das Feld für die Abfragezeichenfolge leer ist. Eine leere Abfrage gibt *alle* Daten aus Ihren Blobs zurück.

   ![Unstrukturierte Suche](media/storage-unstructured-search/emptySearch.png)

### <a name="perform-a-full-text-search"></a>Durchführen einer Volltextsuche

Geben Sie **Kurzsichtigkeit** in das Feld **Abfragezeichenfolge** ein, und wählen Sie dann **Suchen** aus. Dadurch wird eine Suche im Inhalt der Dateien gestartet und eine Teilmenge des Inhalts zurückgegeben, der das Wort „Kurzsichtigkeit“ enthält.

  ![Unstrukturierte Suche](media/storage-unstructured-search/secondMyopia.png)

### <a name="perform-a-system-properties-search"></a>Durchführen einer Suche nach Systemeigenschaften

Neben einer Volltextsuche können Sie auch Abfragen erstellen, die mithilfe des Parameters `$select` nach Systemeigenschaften suchen.

Geben Sie `$select=metadata_storage_name` als Abfragezeichenfolge ein, und drücken Sie die **EINGABETASTE**. Dadurch wird nur das betreffende Feld zurückgegeben.

Die URL wird direkt von der Abfragezeichenfolge geändert, daher sind Leerzeichen nicht zulässig. Verwenden Sie ein Komma, um mehrere Felder zu suchen. Beispiel: `$select=metadata_storage_name,metadata_storage_path`

Der Parameter `$select` kann nur mit Feldern verwendet werden, die beim Definieren des Index als „abrufbar“ markiert wurden.

  ![Unstrukturierte Suche](media/storage-unstructured-search/metadatasearchunstructured.png)

Sie haben dieses Tutorial jetzt abgeschlossen und verfügen über einen durchsuchbaren Satz von unstrukturierten Daten.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Am einfachsten lassen sich die erstellten Ressourcen entfernen, indem Sie die Ressourcengruppe löschen. Beim Entfernen der Ressourcengruppe werden auch alle in der Gruppe enthaltenen Ressourcen gelöscht. Im folgenden Beispiel wird durch Entfernen der Ressourcengruppe das Speicherkonto zusammen mit der Ressourcengruppe entfernt.

1. Navigieren Sie im Azure-Portal zur Liste der Ressourcengruppen in Ihrem Abonnement.
2. Wählen Sie die Ressourcengruppe aus, die Sie löschen möchten.
3. Klicken Sie auf die Schaltfläche **Ressourcengruppe löschen**, und geben Sie den Namen der Ressourcengruppe in das entsprechende Feld ein.
4. Klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

Im folgenden Artikel erfahren Sie mehr über das Indizieren von Dokumenten mit Azure Search:

> [!div class="nextstepaction"]
> [Indizieren von Dokumenten in Azure Blob Storage mit Azure Search](../../search/search-howto-indexing-azure-blob-storage.md)
