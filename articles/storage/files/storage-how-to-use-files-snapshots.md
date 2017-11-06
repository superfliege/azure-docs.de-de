---
title: Arbeiten mit Freigabemomentaufnahmen (Vorschau) | Microsoft-Dokumentation
description: "Eine Freigabemomentaufnahme ist eine schreibgeschützte Version einer Azure Files-Freigabe, die zu einem bestimmten Zeitpunkt erstellt wird, um die Freigabe zu sichern."
services: storage
documentationcenter: .net
author: renash
manager: aungoo
editor: tysonn
ms.assetid: edabe3ee-688b-41e0-b34f-613ac9c3fdfd
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: renash
ms.openlocfilehash: 5212866bda9ff775d32ebb57874b3d58e11f1eb3
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2017
---
# <a name="work-with-share-snapshots-preview"></a>Arbeiten mit Freigabemomentaufnahmen (Vorschau)
Eine Freigabemomentaufnahme (Vorschau) ist eine schreibgeschützte Version einer Azure Files-Freigabe, die zu einem bestimmten Zeitpunkt erstellt wird. Erstellte Freigabemomentaufnahmen können gelesen, kopiert oder gelöscht, aber nicht geändert werden. Eine Freigabemomentaufnahme bietet eine Möglichkeit, die Freigabe so zu sichern, wie sie zu einem bestimmten Zeitpunkt dargestellt wird. 

In diesem Artikel lernen Sie das Erstellen, Verwalten und Löschen von Freigabemomentaufnahmen. Weitere Informationen finden Sie unter [Übersicht über Freigabemomentaufnahmen für Azure Files (Vorschauversion)](storage-snapshots-files.md) oder [Häufig gestellte Fragen zu Azure Files](storage-files-faq.md).

## <a name="create-a-share-snapshot"></a>Erstellen einer Freigabemomentaufnahme

Sie können eine Freigabemomentaufnahme mit Azure-Portal, PowerShell, Azure CLI, REST-API oder jedem beliebigen Azure Storage SDK erstellen. In den folgenden Abschnitten erfahren Sie, wie Sie über das Portal, die CLI und PowerShell eine Freigabemomentaufnahme erstellen. 

Sie können einen Freigabemomentaufnahme von einer Dateifreigabe erstellen, während sie verwendet wird. Freigabemomentaufnahmen erfassen jedoch nur Daten, die zum Zeitpunkt, an dem der Momentaufnahmebefehl ausgegeben wurde, bereits in eine Dateifreigabe geschrieben wurden. Dies kann dazu führen, dass Daten, die von Anwendungen oder dem Betriebssystem zwischengespeichert wurden, eventuell ausgeschlossen werden.

### <a name="create-a-share-snapshot-by-using-the-portal"></a>Erstellen einer Freigabemomentaufnahme mit dem Portal  
Um eine Point-in-Time-Freigabemomentaufnahme zu erstellen, navigieren Sie zu Ihrer Dateifreigabe im Portal und wählen Sie **Momentaufnahme erstellen**.

>   ![Momentaufnahmemenü im Portal](./media/storage-snapshots-create/portal-create-snapshot.png)


### <a name="create-a-share-snapshot-by-using-azure-cli-20"></a>Erstellen einer Freigabemomentaufnahme mit Azure CLI 2.0
Mit dem Befehl `az storage share snapshot` können Sie eine Freigabemomentaufnahme erstellen:

```azurecli-interactive
az storage share snapshot -n <share name>
```

Beispielausgabe:
```json
{
  "metadata": {},
  "name": "<share name>",
  "properties": {
    "etag": "\"0x8D50B7F9A8D7F30\"",
    "lastModified": "2017-10-04T23:28:22+00:00",
    "quota": null
  },
  "snapshot": "2017-10-04T23:28:35.0000000Z"
}
```

### <a name="create-a-share-snapshot-by-using-powershell"></a>Erstellen einer Freigabemomentaufnahme mit PowerShell
Mit dem Befehl `$share.Snapshot()` können Sie eine Freigabemomentaufnahme erstellen:

```powershell
$connectionstring="DefaultEndpointsProtocol=http;FileEndpoint=http:<Storage Account Name>.file.core.windows.net /;AccountName=:<Storage Account Name>;AccountKey=:<Storage Account Key>"
$sharename=":<file share name>"

$ctx = New-AzureStorageContext -ConnectionString $connectionstring

##create snapshot
$share=Get-AzureStorageShare -Context $ctx -Name <file share name>
$share.Properties.LastModified
$share.IsSnapshot
$snapshot=$share.Snapshot()

```

## <a name="perform-common-share-snapshot-operations"></a>Durchführen allgemeiner Freigabemomentaufnahmen-Vorgänge

Sie können die mit Ihrer Dateifreigabe verknüpften Freigabemomentaufnahmen mithilfe der Registerkarte **Vorherige Versionen** unter Windows, über REST, die Clientbibliothek, PowerShell und das Portal auflisten. Sobald die Dateifreigabe bereitgestellt ist, können Sie alle vorherigen Versionen der Datei mithilfe der Registerkarte **Vorherige Versionen** unter Windows anzeigen. 

In den folgenden Abschnitten erfahren Sie, wie Sie mit dem Azure-Portal, Windows und der Azure CLI 2.0 Freigabemomentaufnahmen auflisten, zu ihnen navigieren und sie wiederherstellen können.

### <a name="share-snapshot-operations-in-the-portal"></a>Freigabemomentaufnahmen-Vorgänge im Portal

Im Portal können Sie alle Ihre Freigabemomentaufnahmen einer Dateifreigabe anzeigen und zu einer Freigabemomentaufnahme navigieren, um den Inhalt anzuzeigen.

#### <a name="view-a-share-snapshot"></a>Anzeigen einer Freigabemomentaufnahme
Wählen Sie auf dem Blatt Ihrer Dateifreigabe unter **Momentaufnahme** die Option **Momentaufnahmen anzeigen** aus.

![Befehl „Momentaufnahmen anzeigen“ im Portal](./media/storage-snapshots-list-browse/snapshot-view-portal.png)

#### <a name="list-and-browse-to-share-snapshot-content"></a>Auflisten von Freigabemomentaufnahmen und Navigieren zum Inhalt einer Freigabemomentaufnahme
Zeigen Sie die Liste der Freigabemomentaufnahmen an, und navigieren Sie dann direkt zum Inhalt einer Freigabemomentaufnahme, indem Sie den gewünschten Zeitstempel auswählen.

![Ausgewählte Momentaufnahme in der Zeitstempelliste](./media/storage-snapshots-list-browse/snapshot-browsefiles-portal.png)

Sie können in der Ansicht Ihrer Momentaufnahmenliste auch die Schaltfläche **Verbinden** auswählen, um den Befehl `net use` und den Verzeichnispfad zu einer bestimmten Freigabemomentaufnahme abzurufen. Sie können dann direkt zu dieser Momentaufnahme navigieren.


![Bereich „Verbinden“ mit Befehlsfeld](./media/storage-snapshots-list-browse/snapshot-connect-portal.png)

#### <a name="download-or-restore-from-a-share-snapshot"></a>Herunterladen oder Wiederherstellen aus Freigabemomentaufnahmen
Vom Portal aus können Sie eine Datei aus einer Momentaufnahme über die Schaltfläche **Herunterladen** oder **Wiederherstellen** herunterladen oder wiederherstellen.

![Schaltflächen „Herunterladen“ und „Wiederherstellen“](./media/storage-snapshots-list-browse/snapshot-download-restore-portal.png)

### <a name="share-snapshot-operations-in-windows"></a>Freigabemomentaufnahmen-Vorgänge unter Windows
Wenn Sie bereits Momentaufnahmen von Ihrer Dateifreigabe erstellt haben, können Sie frühere Versionen einer Freigabe, eines Verzeichnisses oder einer bestimmten Datei aus Ihrer bereitgestellten Dateifreigabe unter Windows anzeigen. Im folgenden Beispiel sehen Sie, wie mit der Funktion „Vorherige Versionen“ die vorherige Version eines bestimmten Verzeichnisses unter Windows angezeigt und wiederhergestellt wird.

> [!Note]  
> Sie können die gleichen Vorgänge auf Freigabeebene und Dateiebene ausführen. Nur die Version, die Änderungen für dieses Verzeichnis oder diese Datei enthält, wird in der Liste angezeigt. Wenn ein bestimmtes Verzeichnis oder eine bestimmte Datei zwischen zwei Freigabemomentaufnahmen nicht geändert wurde, wird die Freigabemomentaufnahme nur in der Liste der vorherigen Versionen auf Freigabeebene angezeigt, jedoch nicht in der Liste der vorherigen Versionen eines Verzeichnisses oder einer Datei.

#### <a name="mount-a-file-share"></a>Bereitstellen einer Dateifreigabe
Stellen Sie zuerst mit dem Befehl `net use` die Dateifreigabe bereit.

#### <a name="open-a-mounted-share-in-file-explorer"></a>Öffnen einer bereitgestellten Freigabe im Datei-Explorer
Navigieren Sie zum Datei-Explorer, und suchen Sie nach der bereitgestellten Freigabe.

![Bereitgestellte Freigabe im Datei-Explorer](./media/storage-snapshots-list-browse/snapshot-windows-mount.png)

#### <a name="list-previous-versions"></a>Liste der vorherigen Versionen
Navigieren Sie zu dem Element oder übergeordneten Element, das wiederhergestellt werden soll. Doppelklicken Sie darauf, um zum gewünschten Verzeichnis zu navigieren. Klicken Sie mit der rechten Maustaste darauf, und wählen Sie im Menü **Eigenschaften** aus.

![Kontextmenü für ein ausgewähltes Verzeichnis](./media/storage-snapshots-list-browse/snapshot-windows-previous-versions.png)

Wählen Sie **Vorherige Versionen** aus, um die Liste der Freigabemomentaufnahmen für dieses Verzeichnis anzuzeigen. Je nach Netzwerkgeschwindigkeit und Anzahl der Freigabemomentaufnahmen im Verzeichnis kann es einige Sekunden dauern, bis die Liste geladen wird.

![Registerkarte „Vorherige Versionen“](./media/storage-snapshots-list-browse/snapshot-windows-list.png)

Sie können **Öffnen** auswählen, um eine bestimmte Momentaufnahme zu öffnen. 

![Geöffnete Momentaufnahme](./media/storage-snapshots-list-browse/snapshot-browse-windows.png)

#### <a name="restore-from-a-previous-version"></a>Wiederherstellen von einer vorherigen Version
Wählen Sie **Wiederherstellen**, um den Inhalt des gesamten Verzeichnisses rekursiv zum Erstellungszeitpunkt der Freigabemomentaufnahme am ursprünglichen Speicherort zu kopieren.
 ![Wiederherstellungsschaltfläche in Warnmeldung](./media/storage-snapshots-list-browse/snapshot-windows-restore.png)

### <a name="share-snapshot-operations-in-azure-cli-20"></a>Freigabemomentaufnahmen-Vorgänge in Azure CLI 2.0
Mit Azure CLI 2.0 können Sie Aktionen wie das Auflisten von Freigabemomentaufnahmen, das Navigieren zu Inhalten von Freigabemomentaufnahmen, das Wiederherstellen oder Herunterladen von Dateien aus Freigabemomentaufnahmen oder das Löschen von Freigabemomentaufnahmen durchführen.

#### <a name="list-share-snapshots"></a>Auflisten von Freigabemomentaufnahmen

Wenn Sie [`az storage share list`](/cli/azure/storage/share?view=azure-cli-latest#az_storage_share_list) mit `--include-snapshots` verwenden, können Sie Freigabemomentaufnahmen einer bestimmten Freigabe auflisten:

```azurecli-interactive 
az storage share list --include-snapshots
```

Der Befehl gibt eine Liste der Freigabemomentaufnahmen zusammen mit ihren jeweiligen Eigenschaften aus. Die folgende Ausgabe ist ein Beispiel:

```json
[
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": "2017-10-04T19:44:13.0000000Z"
  },
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": "2017-10-04T19:45:18.0000000Z"
  },
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": null
  }
]
```

#### <a name="browse-to-a-share-snapshot"></a>Navigieren zu einer Freigabemomentaufnahme
Sie können mit [`az storage file list`](/cli/azure/storage/share?view=azure-cli-latest#az_storage_share_list) zu einer bestimmten Freigabemomentaufnahme navigieren und den jeweiligen Inhalt anzeigen. Geben Sie mit Freigabenamen und Zeitstempel an, wohin Sie navigieren möchten, wie im folgenden Beispiel gezeigt:

```azurecli-interactive 
az storage file list --share-name sharesnapshotdefs --snapshot '2017-10-04T19:45:18.0000000Z' -otable
```

In der Ausgabe sehen Sie, dass der Inhalt der Freigabemomentaufnahme mit dem Inhalt der Freigabe zum Zeitpunkt, an dem diese Freigabemomentaufnahme erstellt wurde, identisch ist:

```
Name            Content Length    Type    Last Modified
--------------  ----------------  ------  ---------------
HelloWorldDir/                    dir
IMG_0966.JPG    533568            file
IMG_1105.JPG    717711            file
IMG_1341.JPG    608459            file
IMG_1405.JPG    652156            file
IMG_1611.JPG    442671            file
IMG_1634.JPG    1495999           file
IMG_1635.JPG    974058            file

```
#### <a name="restore-from-a-share-snapshot"></a>Wiederherstellen von einer Freigabemomentaufnahme

Sie können eine Datei wiederherstellen, indem Sie sie aus der Freigabemomentaufnahme kopieren oder herunterladen. Verwenden Sie den Befehl `az storage file download` wie im folgenden Beispiel:

```azurecli-interactive 
az storage file download --path IMG_0966.JPG --share-name sharesnapshotdefs --snapshot '2017-10-04T19:45:18.0000000Z'
```

In der Ausgabe sehen Sie, dass der Inhalt der heruntergeladenen Datei und die zugehörigen Eigenschaften mit dem Inhalt und den Eigenschaften zu dem Zeitpunkt, an dem die Freigabemomentaufnahme erstellt wurde, identisch sind:

```json
{
  "content": null,
  "metadata": {},
  "name": "IMG_0966.JPG",
  "properties": {
    "contentLength": 533568,
    "contentRange": "bytes 0-533567/533568",
    "contentSettings": {
      "cacheControl": null,
      "contentDisposition": null,
      "contentEncoding": null,
      "contentLanguage": null,
      "contentType": "application/octet-stream"
    },
    "copy": {
      "completionTime": null,
      "id": null,
      "progress": null,
      "source": null,
      "status": null,
      "statusDescription": null
    },
    "etag": "\"0x8D50B5F49F7ACDF\"",
    "lastModified": "2017-10-04T19:37:03+00:00",
    "serverEncrypted": true
  }
}
```

## <a name="delete-a-share-snapshot"></a>Löschen einer Freigabemomentaufnahme

Sie können Freigabemomentaufnahmen mit Azure-Portal, PowerShell, CLI, REST-API oder einem beliebigen Storage SDK löschen. In den folgenden Abschnitten erfahren Sie, wie Sie mit Azure-Portal, CLI und PowerShell eine Freigabemomentaufnahme löschen.

Sie können mit jedem Vergleichstool zu Freigabemomentaufnahmen navigieren und Unterschiede zwischen zwei Momentaufnahmen anzeigen. Dann können Sie bestimmen, welche Freigabemomentaufnahme Sie löschen möchten. 

Sie können keine Freigabe löschen, zu der eine Freigabemomentaufnahme vorhanden ist. Sie müssen zuerst alle zugehörigen Freigabemomentaufnahmen löschen, um die Freigabe löschen zu können.

### <a name="delete-a-share-snapshot-by-using-the-portal"></a>Löschen einer Freigabemomentaufnahme mit dem Portal  
Im Portal können Sie zum Blatt Ihrer Dateifreigabe navigieren und mit der Schaltfläche **Löschen** eine oder mehrere Freigabemomentaufnahmen löschen.

>   ![Schaltfläche „Löschen“](./media/storage-snapshots-delete/portal-snapshots-delete.png)


### <a name="delete-a-share-snapshot-by-using-azure-cli-20"></a>Löschen einer Freigabemomentaufnahme mit Azure CLI 2.0
Mit dem Befehl `[az storage share delete]` können Sie eine Freigabemomentaufnahme löschen. Verwenden Sie den Zeitstempel der Freigabemomentaufnahme im folgenden Beispiel für den `--snapshot '2017-10-04T23:28:35.0000000Z' `-Parameter:

```azurecli-interactive
az storage share delete -n <share name> --snapshot '2017-10-04T23:28:35.0000000Z' 
```

Beispielausgabe:
```json
{
  "deleted": true
}
```

### <a name="delete-a-share-snapshot-by-using-powershell"></a>Löschen einer Freigabemomentaufnahme mit PowerShell
Mit dem Befehl `Remove-AzureStorageShare -Share` können Sie eine Freigabemomentaufnahme löschen:

```powershell
$connectionstring="DefaultEndpointsProtocol=http;FileEndpoint=http:<Storage Account Name>.file.core.windows.net /;AccountName=:<Storage Account Name>;AccountKey=:<Storage Account Key>"
$sharename=":<file share name>"

$ctx = New-AzureStorageContext -ConnectionString $connectionstring

##Create snapshot
$share=Get-AzureStorageShare -Context $ctx -Name <file share name>
$share.Properties.LastModified
$share.IsSnapshot
$snapshot=$share.Snapshot()

##Delete snapshot
Remove-AzureStorageShare -Share $snapshot

```

## <a name="next-steps"></a>Nächste Schritte
* [Momentaufnahme – Übersicht](storage-snapshots-files.md)
* [Häufig gestellte Fragen zu Azure Files](storage-files-faq.md)
