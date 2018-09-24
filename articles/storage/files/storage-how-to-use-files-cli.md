---
title: Verwalten von Azure-Dateifreigaben mit der Azure CLI
description: Es wird beschrieben, wie Sie Azure Files mit der Azure CLI verwalten.
services: storage
author: wmgries
ms.service: storage
ms.topic: get-started-article
ms.date: 03/26/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 07d1d1f7050437306d5c2737b9f62b5615753869
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980422"
---
# <a name="manage-azure-file-shares-using-azure-cli"></a>Verwalten von Azure-Dateifreigaben mit der Azure CLI
[Azure Files](storage-files-introduction.md) ist das benutzerfreundliche Clouddateisystem von Microsoft. Azure-Dateifreigaben können unter Windows, Linux und macOS bereitgestellt werden. In diesem Artikel werden die Grundlagen der Arbeit mit Azure-Dateifreigaben per Azure CLI Schritt für Schritt beschrieben. In diesem Artikel werden folgende Themen erläutert: 

> [!div class="checklist"]
> * Erstellen einer Ressourcengruppe und eines Speicherkontos
> * Erstellen einer Azure-Dateifreigabe 
> * Erstellen eines Verzeichnisses
> * Hochladen einer Datei 
> * Herunterladen einer Datei
> * Erstellen und Verwenden einer Freigabemomentaufnahme

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie sich für die lokale Installation und Nutzung der Azure CLI entscheiden, müssen Sie für die Schritte dieses Artikels Azure CLI Version 2.0.4 oder höher ausführen. Führen Sie den Befehl **az --version** aus, um Ihre Azure CLI-Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 

Standardmäßig wird für Azure CLI-Befehle JSON-Code (JavaScript Object Notation) zurückgegeben. JSON ist die Standardmethode zum Senden und Empfangen der Nachrichten von REST-APIs. Um die Verwendung von JSON-Antworten zu ermöglichen, wird in einigen Beispielen dieses Artikels für Azure CLI-Befehle der Parameter *query* verwendet. Für diesen Parameter wird die [JMESPath-Abfragesprache](http://jmespath.org/) zum Analysieren des JSON-Codes genutzt. Weitere Informationen zur Verwendung der Ergebnisse von Azure CLI-Befehlen per JMESPath-Abfragesprache erhalten Sie im [JMESPath-Tutorial](http://jmespath.org/tutorial.html).

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Falls Sie nicht bereits über eine Azure-Ressourcengruppe verfügen, können Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe erstellen. 

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *East US* (USA, Osten) erstellt:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-storage-account"></a>Speicherkonto erstellen
Ein Speicherkonto ist ein gemeinsam genutzter Pool mit Speicherplatz, den Sie zum Bereitstellen von Azure-Dateifreigaben oder anderen Speicherressourcen wie Blobs oder Warteschlangen verwenden können. Ein Speicherkonto kann eine unbegrenzte Anzahl von Dateifreigaben enthalten. Auf einer Freigabe kann eine unbegrenzte Anzahl von Dateien gespeichert werden, bis die Kapazitätsgrenzen des Speicherkontos erreicht sind.

Im folgenden Beispiel wird mit dem Befehl [az storage account create](/cli/azure/storage/account#create) ein Speicherkonto mit dem Namen *mystorageaccount\<Zufallszahl\>* erstellt und der Name dieses Speicherkontos dann in die Variable `$STORAGEACCT` eingefügt. Namen von Speicherkonten müssen eindeutig sein. Mit `$RANDOM` wird eine Zahl an den Speicherkontonamen angefügt, damit er eindeutig ist. 

```azurecli-interactive 
STORAGEACCT=$(az storage account create \
    --resource-group "myResourceGroup" \
    --name "mystorageacct$RANDOM" \
    --location eastus \
    --sku Standard_LRS \
    --query "name" | tr -d '"')
```

### <a name="get-the-storage-account-key"></a>Abrufen des Speicherkontoschlüssels
Mit Speicherkontoschlüsseln wird in einem Speicherkonto den Zugriff auf die Ressourcen gesteuert. Die Schlüssel werden automatisch erstellt, wenn Sie ein Speicherkonto erstellen. Sie können die Speicherkontoschlüssel für Ihr Speicherkonto mit dem Befehl [az storage account keys list](/cli/azure/storage/account/keys#list) abrufen: 

```azurecli-interactive 
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

## <a name="create-an-azure-file-share"></a>Erstellen einer Azure-Dateifreigabe
Jetzt können Sie Ihre erste Azure-Dateifreigabe erstellen. Erstellen Sie Dateifreigaben mit dem Befehl [az storage share create](/cli/azure/storage/share#create). In diesem Beispiel wird eine Azure-Dateifreigabe mit dem Namen *myshare* erstellt: 

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" 
```

> [!IMPORTANT]  
> Freigabenamen dürfen nur Kleinbuchstaben, Zahlen und einzelne Bindestriche enthalten (ein Bindestrich am Anfang ist nicht zulässig). Ausführliche Informationen zur Benennung von Dateifreigaben und Dateien finden Sie unter [Benennen und Referenzieren von Freigaben, Verzeichnissen, Dateien und Metadaten](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata).

## <a name="work-with-the-contents-of-an-azure-file-share"></a>Verwenden des Inhalts einer Azure-Dateifreigabe
Nachdem Sie nun eine Azure-Dateifreigabe erstellt haben, können Sie die Dateifreigabe per SMB unter [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) oder [macOS](storage-how-to-use-files-mac.md) bereitstellen. Alternativ hierzu können Sie mit Ihrer Azure-Dateifreigabe arbeiten, indem Sie die Azure CLI verwenden. Die Nutzung der Azure CLI hat gegenüber einer Bereitstellung der Dateifreigabe per SMB den Vorteil, dass für alle Anforderungen, die per Azure CLI gesendet werden, die Datei-REST-API genutzt wird. Sie können die Datei-REST-API verwenden, um Dateien und Verzeichnisse auf Ihrer Dateifreigabe an den folgenden Orten zu erstellen, zu ändern und zu löschen:

- Bash Azure Cloud Shell (Bereitstellung von Dateifreigaben per SMB ist nicht möglich)
- Clients, die keine SMB-Freigaben bereitstellen können (z.B. lokale Clients, für die Port 445 nicht blockiert ist)
- Serverlose Szenarien, z.B. bei [Azure Functions](../../azure-functions/functions-overview.md)

### <a name="create-a-directory"></a>Erstellen eines Verzeichnisses
Verwenden Sie den Befehl [`az storage directory create`](/cli/azure/storage/directory#az_storage_directory_create), um im Stammverzeichnis Ihrer Azure-Dateifreigabe ein neues Verzeichnis mit dem Namen *myDirectory* zu erstellen:

```azurecli-interactive
az storage directory create \
   --account-name $STORAGEACCT \
   --account-key $STORAGEKEY \
   --share-name "myshare" \
   --name "myDirectory" 
```

### <a name="upload-a-file"></a>Hochladen einer Datei
Zum Demonstrieren eines Dateiuploads mit dem Befehl [`az storage file upload`](/cli/azure/storage/file#az_storage_file_upload) erstellen Sie zuerst eine Datei zum Hochladen auf das temporäre Cloud Shell-Laufwerk. Im folgenden Beispiel erstellen Sie die Datei und laden diese anschließend hoch:

```azurecli-interactive
date > ~/clouddrive/SampleUpload.txt

az storage file upload \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --source "~/clouddrive/SampleUpload.txt" \
    --path "myDirectory/SampleUpload.txt"
```

Beim lokalen Ausführen der Azure CLI ersetzen Sie `~/clouddrive` durch einen Pfad, der auf Ihrem Computer vorhanden ist.

Nach dem Hochladen der Datei können Sie mit dem Befehl [`az storage file list`](/cli/azure/storage/file#az_storage_file_list) sicherstellen, dass die Datei auf Ihre Azure-Dateifreigabe hochgeladen wurde:

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory" \
    --output table
```

### <a name="download-a-file"></a>Herunterladen einer Datei
Sie können den Befehl [`az storage file download`](/cli/azure/storage/file#az_storage_file_download) verwenden, um eine Kopie der Datei herunterzuladen, die Sie auf das temporäre Cloud Shell-Laufwerk hochgeladen haben:

```azurecli-interactive
# Delete an existing file by the same name as SampleDownload.txt, if it exists, because you've run this example before
rm -rf ~/clouddrive/SampleDownload.txt

az storage file download \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory/SampleUpload.txt" \
    --dest "~/clouddrive/SampleDownload.txt"
```

### <a name="copy-files"></a>Kopieren von Dateien
Eine häufige Aufgabe besteht darin, Dateien von einer Dateifreigabe auf eine andere oder in oder aus einem Azure Blob-Speichercontainer zu kopieren. Erstellen Sie eine neue Freigabe, um diese Funktionalität zu demonstrieren. Kopieren Sie die Datei, die Sie auf diese neue Freigabe hochgeladen haben, indem Sie den Befehl [az storage file copy](/cli/azure/storage/file/copy) verwenden: 

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare2"

az storage directory create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare2" \
    --name "myDirectory2"

az storage file copy start \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --source-share "myshare" \
    --source-path "myDirectory/SampleUpload.txt" \
    --destination-share "myshare2" \
    --destination-path "myDirectory2/SampleCopy.txt"
```

Wenn Sie die Dateien auf der neuen Freigabe auflisten, sollte die kopierte Datei angezeigt werden:

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare2" \
    --output table
```

Der Befehl `az storage file copy start` ist für Dateiverschiebungen zwischen Azure-Dateifreigaben und Azure-Blobspeichercontainern zwar gut geeignet, aber wir empfehlen Ihnen, für größere Verschiebungen AzCopy zu verwenden. (Mit „größer“ ist hier die Anzahl oder Größe von zu verschiebenden Dateien gemeint.) Informieren Sie sich über [AzCopy für Linux](../common/storage-use-azcopy-linux.md) und [AzCopy für Windows](../common/storage-use-azcopy.md). AzCopy muss lokal installiert sein. AzCopy ist in Cloud Shell nicht verfügbar. 

## <a name="create-and-modify-share-snapshots"></a>Erstellen und Ändern von Freigabemomentaufnahmen
Eine weitere nützliche Aufgabe, die Sie mit einer Azure-Dateifreigabe durchführen können, ist die Erstellung von Freigabemomentaufnahmen. Mit einer Momentaufnahme wird eine Kopie einer Azure-Dateifreigabe für einen bestimmten Zeitpunkt erstellt. Freigabemomentaufnahmen ähneln einigen Betriebssystemtechnologien, mit denen Sie unter Umständen bereits vertraut sind:
- Momentaufnahmen vom Typ [Logical Volume Manager (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) für Linux-Systeme
- Momentaufnahmen vom Typ [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) für macOS
- [Volumeschattenkopie-Dienst (VSS)](https://docs.microsoft.com/en-us/windows/desktop/VSS/volume-shadow-copy-service-portal) für Windows-Dateisysteme wie NTFS und ReFS

Sie können eine Freigabemomentaufnahme erstellen, indem Sie den Befehl [`az storage share snapshot`](/cli/azure/storage/share#az_storage_share_snapshot) verwenden:

```azurecli-interactive
SNAPSHOT=$(az storage share snapshot \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" \
    --query "snapshot" | tr -d '"')
```

### <a name="browse-share-snapshot-contents"></a>Durchsuchen des Inhalts einer Freigabemomentaufnahme
Sie können den Inhalt einer Freigabemomentaufnahme durchsuchen, indem Sie den Zeitstempel der Freigabemomentaufnahme, die Sie mit der Variablen `$SNAPSHOT` erfasst haben, an den Befehl `az storage file list` übergeben:

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --snapshot $SNAPSHOT \
    --output table
```

### <a name="list-share-snapshots"></a>Auflisten von Freigabemomentaufnahmen
Verwenden Sie den folgenden Befehl, um eine Liste mit den Momentaufnahmen anzuzeigen, die Sie für Ihre Freigabe erstellt haben:

```azurecli-interactive
az storage share list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --include-snapshot \
    --query "[? name=='myshare' && snapshot!=null]" | tr -d '"'
```

### <a name="restore-from-a-share-snapshot"></a>Wiederherstellen von einer Freigabemomentaufnahme
Sie können eine Datei mit dem zuvor verwendeten Befehl `az storage file copy start` wiederherstellen. Löschen Sie zuerst die Datei „SampleUpload.txt“, die Sie hochgeladen haben, damit Sie sie aus der Momentaufnahme wiederherstellen können:

```azurecli-interactive
# Delete SampleUpload.txt
az storage file delete \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory/SampleUpload.txt"

# Build the source URI for a snapshot restore
URI=$(az storage account show \
    --resource-group "myResourceGroup" \
    --name $STORAGEACCT \
    --query "primaryEndpoints.file" | tr -d '"')

URI=$URI"myshare/myDirectory/SampleUpload.txt?sharesnapshot="$SNAPSHOT

# Restore SampleUpload.txt from the share snapshot
az storage file copy start \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --source-uri $URI \
    --destination-share "myshare" \
    --destination-path "myDirectory/SampleUpload.txt"
```

### <a name="delete-a-share-snapshot"></a>Löschen einer Freigabemomentaufnahme
Mit dem Befehl [`az storage share delete`](/cli/azure/storage/share#az_storage_share_delete) können Sie eine Freigabemomentaufnahme löschen. Verwenden Sie die Variable, die den `$SNAPSHOT`-Verweis auf den Parameter `--snapshot` enthält:

```azurecli-interactive
az storage share delete \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" \
    --snapshot $SNAPSHOT
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie den Vorgang abgeschlossen haben, können Sie den Befehl [`az group delete`](/cli/azure/group#delete) verwenden, um die Ressourcengruppe und alle dazugehörigen Ressourcen zu entfernen: 

```azurecli-interactive 
az group delete --name "myResourceGroup"
```

Alternativ hierzu können Sie auch Ressourcen einzeln entfernen.
- Entfernen Sie die Azure-Dateifreigaben, die Sie für diesen Artikel erstellt haben, wie folgt:

    ```azurecli-interactive
    az storage share delete \
        --account-name $STORAGEACCT \
        --account-key $STORAGEKEY \
        --name "myshare" \
        --delete-snapshots include

    az storage share delete \
        --account-name $STORAGEACCT \
        --account-key $STORAGEKEY \
        --name "myshare2" \
        --delete-snapshots include
    ```

- Entfernen Sie das Speicherkonto selbst wie folgt: (Hiermit werden implizit die von Ihnen erstellten Azure-Dateifreigaben und alle anderen ggf. erstellten Speicherressourcen, z.B. ein Azure-Blobspeichercontainer, entfernt.)

    ```azurecli-interactive
    az storage account delete \
        --resource-group "myResourceGroup" \
        --name $STORAGEACCT \
        --yes
    ```

## <a name="next-steps"></a>Nächste Schritte
- [Verwalten von Dateifreigaben mit dem Azure-Portal](storage-how-to-use-files-portal.md)
- [Verwalten von Dateifreigaben mit Azure PowerShell](storage-how-to-use-files-powershell.md)
- [Verwalten von Dateifreigaben mit Storage-Explorer](storage-how-to-use-files-storage-explorer.md)
- [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md)