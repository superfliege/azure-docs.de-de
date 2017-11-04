---
title: Informationen zur Verwendung von Azure-Dateifreigabemomentaufnahmen (Vorschau) | Microsoft-Dokumentation
description: "Verwendung von Azure-Dateifreigabemomentaufnahmen Azure-Dateifreigabemomentaufnahmen sind schreibgeschützte Versionen einer Azure-Dateifreigabe, die zu einem bestimmten Zeitpunkt erstellt wird. Nachdem eine Freigabemomentaufnahme erstellt wurde, kann sie zwar gelesen, kopiert oder gelöscht, aber nicht geändert werden. Freigabemomentaufnahmen bieten eine Möglichkeit, die Freigabe so zu sichern, wie sie zu einem bestimmten Zeitpunkt dargestellt wird."
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
ms.openlocfilehash: 33b64e1ad3fd5a2a6954a02da0fb303acca54c40
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2017
---
# <a name="work-with-azure-file-share-snapshots-preview"></a>Arbeiten mit Azure-Dateifreigabemomentaufnahmen (Vorschau)
Azure-Dateifreigabemomentaufnahmen (Vorschau) sind schreibgeschützte Versionen einer Azure-Dateifreigabe, die zu einem bestimmten Zeitpunkt erstellt wird. Nachdem eine Freigabemomentaufnahme erstellt wurde, kann sie zwar gelesen, kopiert oder gelöscht, aber nicht geändert werden. Freigabemomentaufnahmen bieten eine Möglichkeit, die Freigabe so zu sichern, wie sie zu einem bestimmten Zeitpunkt dargestellt wird. In diesem Artikel erfahren Sie, wie Azure-Dateifreigabemomentaufnahmen erstellt, verwaltet und gelöscht werden. Weitere Informationen zu Freigabemomentaufnahmen finden Sie unter [Freigabemomentaufnahme – Übersicht](storage-snapshots-files.md) oder [Häufig gestellte Fragen zu Azure Files](storage-files-faq.md).

## <a name="create-azure-files-share-snapshots"></a>Erstellen von Azure-Dateifreigabemomentaufnahmen

Sie können über das Portal, PowerShell, die CLI, REST oder ein beliebiges Speicher-SDK eine Freigabemomentaufnahme erstellen. In den folgenden Abschnitten erfahren Sie, wie über das Portal, die CLI und PowerShell eine Freigabemomentaufnahme erstellt wird. 

Sie können einen Freigabemomentaufnahme von einer Dateifreigabe erstellen, während sie verwendet wird. Freigabemomentaufnahmen erfassen jedoch nur Daten, die zum Zeitpunkt, an dem der Momentaufnahmebefehl ausgegeben wurde, bereits in die Azure-Dateifreigabe geschrieben wurden. Dies kann dazu führen, dass Daten, die von Anwendungen oder dem Betriebssystem zwischengespeichert wurden, eventuell ausgeschlossen werden.

### <a name="create-share-snapshot-using-portal"></a>Erstellen einer Freigabemomentaufnahme mit dem Portal  
Sie können einfach zu Ihrer Dateifreigabe im Portal navigieren und die Schaltfläche `Create a Snapshot` auswählen, um eine Point-in-Time-Freigabemomentaufnahme zu erstellen.

>   ![./media/storage-snapshots-create/portal-create-snapshot.png](./media/storage-snapshots-create/portal-create-snapshot.png)


### <a name="create-share-snapshot-using-cli-20"></a>Erstellen einer Freigabemomentaufnahme mit der CLI 2.0
Mit dem Befehl `az storage share snapshot` können Sie eine Freigabemomentaufnahme erstellen:

```azurecli-interactive
az storage share snapshot -n <share name>
```

Beispielausgabe
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

### <a name="create-share-snapshot-using-powershell"></a>Erstellen einer Freigabemomentaufnahme mit PowerShell
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

## <a name="common-share-snapshot-operations"></a>Allgemeine Vorgänge zu Freigabemomentaufnahmen

Sie können die mit Ihrer Dateifreigabe verknüpften Freigabemomentaufnahmen mithilfe der Registerkarte „Vorherige Versionen“ unter Windows, über REST, die Clientbibliothek, PowerShell und das Portal auflisten. Sobald die Azure-Dateifreigabe bereitgestellt ist, können Sie alle vorherigen Versionen der Datei mithilfe der Registerkarte „Vorherige Versionen“ unter Windows anzeigen. In den folgenden Abschnitten erfahren Sie, wie mit dem Azure-Portal, Windows und der Azure CLI 2.0 Freigabemomentaufnahmen aufgelistet, durchsucht und wiederhergestellt werden.

### <a name="share-snapshot-operations-in-portal"></a>Vorgänge zu Freigabemomentaufnahmen im Portal

Im Portal können Sie sich all Ihre Freigabemomentaufnahmen einer Dateifreigabe ansehen und eine Freigabemomentaufnahme zum Anzeigen des Inhalts durchsuchen.

#### <a name="view-share-snapshot"></a>Anzeigen einer Freigabemomentaufnahme
Wählen Sie auf dem Blatt Ihrer Dateifreigabe unter „Momentaufnahme“ die Option **Momentaufnahmen anzeigen** aus.

![./media/storage-snapshots-list-browse/snapshot-view-portal.png](./media/storage-snapshots-list-browse/snapshot-view-portal.png)

#### <a name="list-and-browse-share-snapshot-content"></a>Auflisten und Durchsuchen des Inhalts einer Freigabemomentaufnahme
Zeigen Sie die Liste der Freigabemomentaufnahmen an, und durchsuchen Sie dann direkt den Inhalt, indem Sie die gewünschte Freigabemomentaufnahme anhand des gewünschten Zeitstempels auswählen.

![./media/storage-snapshots-list-browse/snapshot-browsefiles-portal.png](./media/storage-snapshots-list-browse/snapshot-browsefiles-portal.png)

Sie können in der Ansicht Ihrer Momentaufnahmeliste auch die Schaltfläche **Verbinden** auswählen, um den Befehl `net use` und den Verzeichnispfad zu einer bestimmten Freigabemomentaufnahme zu erhalten, zu der Sie direkt navigieren können.


![./media/storage-snapshots-list-browse/snapshot-connect-portal.pngsnapshot-list-portal.png](./media/storage-snapshots-list-browse/snapshot-connect-portal.png)

#### <a name="download-or-restore-from-share-snapshot"></a>Herunterladen oder Wiederherstellen von Freigabemomentaufnahmen
Laden Sie über das Portal die gewünschte Datei aus einer Momentaufnahme herunter, oder stellen Sie sie wieder her.

![./media/storage-snapshots-list-browse/snapshot-download-restore-portal.png](./media/storage-snapshots-list-browse/snapshot-download-restore-portal.png)

### <a name="file-share-snapshot-operations-in-windows"></a>Vorgänge zu Dateifreigabemomentaufnahmen unter Windows
Wenn Sie bereits Momentaufnahmen von Ihrer Dateifreigabe erstellt haben, können Sie frühere Versionen einer Freigabe, eines Verzeichnisses oder einer bestimmten Datei aus Ihrer bereitgestellten Azure-Dateifreigabe unter Windows anzeigen. Im folgenden Beispiel sehen Sie, wie mit der Funktion **Vorherige Versionen** die vorherige Version eines bestimmten Verzeichnisses unter Windows angezeigt und wiederhergestellt wird:

> [!Note]  
> Diese Vorgänge können sowohl auf Freigabeebene als auch auf Dateiebene durchgeführt werden. Nur die Version, die Änderungen für dieses Verzeichnis oder diese Datei enthält, werden in der Liste angezeigt. Wenn ein bestimmtes Verzeichnis oder eine bestimmte Datei zwischen zwei Freigabemomentaufnahmen nicht geändert wurde, wird die Freigabemomentaufnahme nur in der Liste der vorherigen Versionen auf Freigabeebene angezeigt, jedoch nicht in der Liste der vorherigen Versionen eines Verzeichnisses oder einer Datei.

#### <a name="mount-file-share"></a>Bereitstellen einer Dateifreigabe
Stellen Sie zuerst mit dem Befehl „net use“ die Dateifreigabe bereit.

#### <a name="open-mounted-share-in-explorer"></a>Öffnen der bereitgestellten Freigabe im Explorer
Navigieren Sie zum Datei-Explorer, und suchen Sie nach der bereitgestellten Freigabe.

![./media/storage-snapshots-list-browse/snapshot-windows-mount.png](./media/storage-snapshots-list-browse/snapshot-windows-mount.png)

#### <a name="list-previous-versions"></a>Liste der vorherigen Versionen
 Navigieren Sie zu dem Element oder übergeordneten Element, das wiederhergestellt werden soll. Doppelklicken Sie darauf, um zum gewünschten Verzeichnis zu navigieren. Klicken Sie mit der rechten Maustaste darauf, und wählen Sie im Menü „Eigenschaften“ aus.

![./media/storage-snapshots-list-browse/snapshot-windows-previous-versions.png](./media/storage-snapshots-list-browse/snapshot-windows-previous-versions.png)

Wählen Sie **Vorherige Versionen** aus, um die Liste der Freigabemomentaufnahmen für dieses Verzeichnis anzuzeigen. Je nach Netzwerkgeschwindigkeit und Anzahl der Freigabemomentaufnahmen kann es einige Sekunden dauern, bis die Liste geladen wird.

 ![./media/storage-snapshots-list-browse/snapshot-windows-list.png](./media/storage-snapshots-list-browse/snapshot-windows-list.png)

Sie können **Öffnen** auswählen, um zu einer bestimmten Momentaufnahme zu navigieren. 

 ![./media/storage-snapshots-list-browse/snapshot-browse-windows.png](./media/storage-snapshots-list-browse/snapshot-browse-windows.png)

#### <a name="restore-from-a-previous-version"></a>Wiederherstellen von einer vorherigen Version
Klicken Sie auf **Wiederherstellen**, um den Inhalt des gesamten Verzeichnisses rekursiv zum Erstellungszeitpunkt der Freigabe am ursprünglichen Speicherort zu kopieren.
 ![./media/storage-snapshots-list-browse/snapshot-windows-previous-versions.png](./media/storage-snapshots-list-browse/snapshot-windows-restore.png)

### <a name="file-share-snapshot-operations-in-azure-cli-20"></a>Vorgänge zu Dateifreigabemomentaufnahmen in der Azure CLI 2.0
Mit der Azure CLI 2.0 können Sie dieselben Aktionen ausführen, z.B. das Auflisten von Freigabemomentaufnahmen, das Durchsuchen des Inhalts von Freigabemomentaufnahmen, das Wiederherstellen oder Herunterladen von Dateien aus Freigabemomentaufnahmen oder das Löschen von Freigabemomentaufnahmen.

#### <a name="list-share-snapshots"></a>Auflisten von Freigabemomentaufnahmen

Mit [`az storage share list`](/cli/azure/storage/share?view=azure-cli-latest#az_storage_share_list), das `--include-snapshots` enthält, können Sie Freigabemomentaufnahmen einer bestimmten Freigabe auflisten.

```azurecli-interactive 
az storage share list --include-snapshots
```

#### <a name="sample-output"></a>Beispielausgabe
Der Befehl gibt eine Liste der Freigabemomentaufnahmen zusammen mit ihren jeweiligen Eigenschaften aus.

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

#### <a name="browse-share-snapshots"></a>Durchsuchen von Freigabemomentaufnahmen
Sie können mit [`az storage file list`](/cli/azure/storage/share?view=azure-cli-latest#az_storage_share_list) auch eine bestimmte Freigabemomentaufnahme durchsuchen, um den jeweiligen Inhalt anzuzeigen. Hierfür müssen der Freigabename `--share-name` und der Zeitstempel angegeben werden, nach denen in `--snapshot '2017-10-04T19:45:18.0000000Z'` gesucht werden soll.

```azurecli-interactive 
az storage file list --share-name sharesnapshotdefs --snapshot '2017-10-04T19:45:18.0000000Z' -otable
```

#### <a name="sample-output"></a>Beispielausgabe

Sie werden erkennen, dass der Inhalt der Freigabemomentaufnahme mit dem Inhalt der Freigabe zum Zeitpunkt, an dem diese Freigabemomentaufnahme erstellt wurde, identisch ist.

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
#### <a name="restore-from-share-snapshots"></a>Wiederherstellen von Freigabemomentaufnahmen

Sie können eine Datei wiederherstellen, indem Sie mit dem Befehl `az storage file download` eine Datei aus der Freigabemomentaufnahme kopieren oder herunterladen.

```azurecli-interactive 
az storage file download --path IMG_0966.JPG --share-name sharesnapshotdefs --snapshot '2017-10-04T19:45:18.0000000Z'
```

#### <a name="sample-output"></a>Beispielausgabe

Sie werden sehen, dass der Inhalt der heruntergeladenen Datei und die zugehörigen Eigenschaften mit dem Inhalt und den Eigenschaften zu dem Zeitpunkt, an dem die Freigabemomentaufnahme erstellt wurde, identisch sind.

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

## <a name="delete-azure-files-share-snapshot"></a>Löschen von Azure-Dateifreigabemomentaufnahmen

Sie können Dateifreigabemomentaufnahmen mithilfe des Azure-Portals, PowerShell, der CLI, der REST-API oder einem beliebigen Speicher-SDK löschen. In den folgenden Abschnitten erfahren Sie, wie über das Azure-Portal, die CLI und PowerShell Freigabemomentaufnahmen gelöscht werden.

Mithilfe eines beliebigen Vergleichstools können Sie die Freigabemomentaufnahmen durchsuchen und Unterschiede zwischen den beiden Freigabemomentaufnahmen anzeigen, um festzustellen, welche Freigabemomentaufnahme gelöscht werden soll. 

Eine Freigabe, die eine Freigabemomentaufnahme enthält, kann nicht gelöscht werden. Sie müssen zuerst alle zugehörigen Freigabemomentaufnahmen löschen, um die Freigabe löschen zu können.

### <a name="delete-share-snapshot-using-portal"></a>Löschen einer Freigabemomentaufnahme mit dem Portal  
Um einen oder mehrere Freigabemomentaufnahmen zu löschen, können Sie im Portal zum Blatt Ihrer Dateifreigabe navigieren und die Schaltfläche `delete` auswählen.

>   ![./media/storage-snapshots-delete/portal-snapshots-delete.png](./media/storage-snapshots-delete/portal-snapshots-delete.png)


### <a name="delete-a-share-snapshot-using-azure-cli-20"></a>Löschen einer Freigabemomentaufnahme mithilfe der Azure CLI 2.0
Mit dem Befehl `[az storage share delete]` können Sie eine Freigabemomentaufnahme löschen, indem Sie den Parameter `--snapshot '2017-10-04T23:28:35.0000000Z' ` mit dem Zeitstempel der Freigabemomentaufnahme angeben:

```azurecli-interactive
az storage share delete -n <share name> --snapshot '2017-10-04T23:28:35.0000000Z' 
```

Beispielausgabe
```json
{
  "deleted": true
}
```

### <a name="delete-a-share-snapshot-using-powershell"></a>Löschen einer Freigabemomentaufnahme mit PowerShell
Mit dem Befehl `Remove-AzureStorageShare -Share` können Sie eine Freigabemomentaufnahme erstellen:

```powershell
$connectionstring="DefaultEndpointsProtocol=http;FileEndpoint=http:<Storage Account Name>.file.core.windows.net /;AccountName=:<Storage Account Name>;AccountKey=:<Storage Account Key>"
$sharename=":<file share name>"

$ctx = New-AzureStorageContext -ConnectionString $connectionstring

##create snapshot
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