---
title: Verwalten von Azure-Dateifreigaben mit der Azure CLI
description: Es wird beschrieben, wie Sie Azure Files über die Azure CLI verwalten.
services: storage
documentationcenter: na
author: wmgries
manager: jeconnoc
editor: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/26/2018
ms.author: wgries
ms.openlocfilehash: 066a43b553be18a5a0bc889fff441824df301b98
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2018
---
# <a name="managing-azure-file-shares-using-the-azure-cli"></a>Verwalten von Azure-Dateifreigaben mit der Azure CLI
[Azure Files](storage-files-introduction.md) ist das benutzerfreundliche Clouddateisystem von Microsoft. Azure-Dateifreigaben können unter Windows, Linux und macOS bereitgestellt werden. In dieser Anleitung werden die Grundlagen der Arbeit mit Azure-Dateifreigaben mit der Azure CLI Schritt für Schritt beschrieben. In diesem Artikel werden folgende Themen erläutert: 

> [!div class="checklist"]
> * Erstellen einer Ressourcengruppe und eines Speicherkontos
> * Erstellen einer Azure-Dateifreigabe 
> * Erstellen eines Verzeichnisses
> * Hochladen einer Datei 
> * Herunterladen einer Datei
> * Erstellen und Verwenden einer Freigabemomentaufnahme

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Azure CLI lokal installieren und verwenden möchten, müssen Sie für diesen Leitfaden mindestens Version 2.0.4 der Azure CLI ausführen. Führen Sie **az --version** aus, um die Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 

Die Azure CLI-Befehle geben standardmäßig JSON-Code (JavaScript Object Notation) zurück. Dies ist die übliche Vorgehensweise zum Senden und Empfangen von Nachrichten für REST-APIs. Um die Verarbeitung der JSON-Antworten zu ermöglichen, wird in einigen Beispielen dieses Leitfadens der Abfrageparameter für Azure CLI-Befehle verwendet. Dieser Parameter nutzt die [JMESPath-Abfragesprache](http://jmespath.org/) zum Analysieren des JSON-Codes. Weitere Informationen dazu, wie Sie die Ergebnisse der Azure CLI-Befehle ändern, erhalten Sie im [JMESPath-Tutorial](http://jmespath.org/tutorial.html).

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Falls Sie nicht bereits über eine Azure-Ressourcengruppe verfügen, können Sie mit dem Befehl [az group create](/cli/azure/group#create) eine neue erstellen. 

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *East US* (USA, Osten) erstellt.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-storage-account"></a>Speicherkonto erstellen
Ein Speicherkonto ist ein gemeinsam genutzter Pool mit Speicherplatz, den Sie zum Bereitstellen von Azure-Dateifreigaben oder anderen Speicherressourcen wie Blobs oder Warteschlangen verwenden können. Ein Speicherkonto kann eine unbegrenzte Anzahl von Dateifreigaben enthalten, und eine Freigabe kann eine unbegrenzte Anzahl von Dateien speichern, bis die Kapazitätsgrenze des Speicherkontos erreicht ist.

In diesem Beispiel wird mit dem Befehl [az storage account create](/cli/azure/storage/account#create) ein Speicherkonto mit dem Namen `mystorageaccount<random number>` erstellt und der Name dieses Speicherkontos dann in die Variable `$STORAGEACCT` eingefügt. Speicherkontonamen müssen eindeutig sein. Bei Verwendung von `$RANDOM` wird eine Zahl an das Ende des Speicherkontonamens angefügt, damit er eindeutig ist. 

```azurecli-interactive 
STORAGEACCT=$(az storage account create \
    --resource-group "myResourceGroup" \
    --name "mystorageacct$RANDOM" \
    --location eastus \
    --sku Standard_LRS \
    --query "name" | tr -d '"')
```

### <a name="get-the-storage-account-key"></a>Abrufen des Speicherkontoschlüssels
Speicherkontoschlüssel werden verwendet, um in einem Speicherkonto den Zugriff auf die Ressourcen zu steuern. Sie werden automatisch erstellt, wenn Sie ein Speicherkonto erstellen. Sie können die Speicherkontoschlüssel für das Speicherkonto mit dem Befehl [az storage account keys list](/cli/azure/storage/account/keys#list) abrufen. 

```azurecli-interactive 
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

## <a name="create-an-azure-file-share"></a>Erstellen einer Azure-Dateifreigabe
Jetzt können Sie Ihre erste Azure-Dateifreigabe erstellen. Sie können Dateifreigaben mit dem Befehl [az storage share create](/cli/azure/storage/share#create) erstellen. In diesem Beispiel wird eine Azure-Dateifreigabe mit dem Namen `myshare` erstellt. 

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" 
```

> [!Important]  
> Freigabenamen dürfen nur Kleinbuchstaben, Zahlen und einzelne Bindestriche enthalten und dürfen nicht mit einem Bindestrich beginnen. Ausführliche Informationen zur Benennung von Dateifreigaben und Dateien finden Sie unter [Benennen und Referenzieren von Freigaben, Verzeichnissen, Dateien und Metadaten](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata).

## <a name="manipulating-the-contents-of-the-azure-file-share"></a>Bearbeiten des Inhalts der Azure-Dateifreigabe
Nachdem Sie nun eine Azure-Dateifreigabe erstellt haben, können Sie die Dateifreigabe mit SMB unter [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) oder [macOS](storage-how-to-use-files-mac.md) bereitstellen. Alternativ hierzu können Sie Ihre Azure-Dateifreigabe mit der Azure CLI ändern. Dies ist vorteilhafter als das Bereitstellen der Dateifreigabe mit SMB, da alle mit der Azure CLI gesendeten Anforderungen mit der Datei-REST-API durchgeführt werden, sodass Sie Dateien und Verzeichnisse auf Ihrer Dateifreigabe wie folgt erstellen, ändern und löschen können:

- Bash Cloud Shell (Bereitstellung von Dateifreigaben per SMB ist nicht möglich).
- Clients, die keine SMB-Freigaben bereitstellen können, z.B. lokale Clients, für die die Blockierung von Port 445 nicht aufgehoben wurde.
- Serverlose Szenarien, z.B. wie bei [Azure Functions](../../azure-functions/functions-overview.md). 

### <a name="create-directory"></a>Erstellen eines Verzeichnisses
Verwenden Sie den Befehl [`az storage directory create`](/cli/azure/storage/directory#az_storage_directory_create), um im Stammverzeichnis Ihrer Azure-Dateifreigabe ein neues Verzeichnis mit dem Namen *myDirectory* zu erstellen.

```azurecli-interactive
az storage directory create \
   --account-name $STORAGEACCT \
   --account-key $STORAGEKEY \
   --share-name "myshare" \
   --name "myDirectory" 
```

### <a name="upload-a-file"></a>Hochladen einer Datei
Um das Hochladen einer Datei mit dem Befehl [`az storage file upload`](/cli/azure/storage/file#az_storage_file_upload) demonstrieren zu können, müssen wir zuerst auf Ihrem temporären Laufwerk der Azure CLI Cloud Shell eine Datei erstellen, die hochgeladen werden kann. Im folgenden Beispiel erstellen wir die Datei und laden sie anschließend hoch.

```azurecli-interactive
date > ~/clouddrive/SampleUpload.txt

az storage file upload \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --source "~/clouddrive/SampleUpload.txt" \
    --path "myDirectory/SampleUpload.txt"
```

Beim lokalen Ausführen der Azure CLI sollten Sie `~/clouddrive` durch einen Pfad ersetzen, der auf Ihrem Computer vorhanden ist.

Nach dem Hochladen der Datei können Sie den Befehl [`az storage file list`](/cli/azure/storage/file#az_storage_file_list) zum Überprüfen verwenden, um sicherzustellen, dass die Datei auf Ihre Azure-Dateifreigabe hochgeladen wurde.

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory" \
    --output table
```

### <a name="download-a-file"></a>Herunterladen einer Datei
Sie können den Befehl [`az storage file download`](/cli/azure/storage/file#az_storage_file_download) verwenden, um eine Kopie der Datei herunterzuladen, die Sie auf das temporäre Laufwerk Ihrer Cloud Shell hochgeladen haben.

```azurecli-interactive
# Delete an existing file by the same name as SampleDownload.txt, if it exists because you've run this example before
rm -rf ~/clouddrive/SampleDownload.txt

az storage file download \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory/SampleUpload.txt" \
    --dest "~/clouddrive/SampleDownload.txt"
```

### <a name="copy-files"></a>Kopieren von Dateien
Eine häufige Aufgabe besteht darin, Dateien von einer Dateifreigabe auf eine andere oder in bzw. aus einem Azure Blob-Speichercontainer zu kopieren. Zum Demonstrieren dieser Funktionalität können Sie eine neue Freigabe erstellen und die Datei, die Sie hochgeladen haben, mit dem Befehl [az storage file copy](/cli/azure/storage/file/copy) auf diese neue Freigabe kopieren. 

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

Wenn Sie die Dateien auf der neuen Freigabe auflisten, sollte die kopierte Datei angezeigt werden.

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare2" \
    --output table
```

Der Befehl `az storage file copy start` eignet sich zwar gut für das Verschieben von Ad-hoc-Dateien zwischen Azure-Dateifreigaben und Azure Blob Storage-Containern, aber für umfangreichere Verschiebungen (in Bezug auf die Anzahl oder Größe von Dateien) empfehlen wir die Verwendung von AzCopy. Informieren Sie sich über [AzCopy für Linux](../common/storage-use-azcopy-linux.md) und [AzCopy für Windows](../common/storage-use-azcopy.md). AzCopy muss lokal installiert werden und ist in Cloud Shell nicht verfügbar. 

## <a name="create-and-modify-share-snapshots"></a>Erstellen und Ändern von Freigabemomentaufnahmen
Eine weitere nützliche Aufgabe, die Sie mit einer Azure-Dateifreigabe durchführen können, ist die Erstellung von Freigabemomentaufnahmen. Mit einer Momentaufnahme wird für eine Azure-Dateifreigabe ein bestimmter Zeitpunkt beibehalten. Freigabemomentaufnahmen ähneln Betriebssystemtechnologien, mit denen Sie unter Umständen bereits vertraut sind:
- Momentaufnahmen vom Typ [Logical Volume Manager (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) für Linux-Systeme
- Momentaufnahmen vom Typ [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) für macOS 
- [Volumeschattenkopie-Dienst (VSS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee923636) für Windows-Dateisysteme wie NTFS und ReFS

Sie können eine Freigabemomentaufnahme erstellen, indem Sie den Befehl [`az storage share snapshot`](/cli/azure/storage/share#az_storage_share_snapshot) verwenden.

```azurecli-interactive
SNAPSHOT=$(az storage share snapshot \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" \
    --query "snapshot" | tr -d '"')
```

### <a name="browse-share-snapshot-contents"></a>Durchsuchen des Inhalts einer Freigabemomentaufnahme
Sie können den Inhalt einer Freigabemomentaufnahme durchsuchen, indem Sie den Zeitstempel der Freigabemomentaufnahme, die wir mit der Variablen `$SNAPSHOT` erfasst haben, an den Befehl `az storage file list` übergeben.

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --snapshot $SNAPSHOT \
    --output table
```

### <a name="list-share-snapshots"></a>Auflisten von Freigabemomentaufnahmen
Sie können die Liste mit den Momentaufnahmen, die Sie für Ihre Freigabe erstellt haben, mit dem folgenden Befehl anzeigen.

```azurecli-interactive
az storage share list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --include-snapshot \
    --query "[? name=='myshare' && snapshot!=null]" | tr -d '"'
```

### <a name="restore-from-a-share-snapshot"></a>Wiederherstellen von einer Freigabemomentaufnahme
Sie können eine Datei wiederherstellen, indem Sie den Befehl `az storage file copy start` verwenden, den wir bereits genutzt haben. Zuerst löschen wir die hochgeladene Datei `SampleUpload.txt`, damit wir sie aus der Momentaufnahme wiederherstellen können.

```azurecli-interactive
# Delete SampleUpload.txt
az storage file delete \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory/SampleUpload.txt"

# Build the source URI for snapshot restore
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
Sie können eine Freigabemomentaufnahme löschen, indem Sie den Befehl [`az storage share delete`](/cli/azure/storage/share#az_storage_share_delete) mit der Variablen verwenden, die den Verweis `$SNAPSHOT` auf den Parameter `--snapshot` enthält.

```azurecli-interactive
az storage share delete \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" \
    --snapshot $SNAPSHOT
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie den Vorgang abgeschlossen haben, können Sie den Befehl [`az group delete`](/cli/azure/group#delete) verwenden, um die Ressourcengruppe und alle dazugehörigen Ressourcen zu entfernen. 

```azurecli-interactive 
az group delete --name "myResourceGroup"
```

Alternativ hierzu können Sie Ressourcen einzeln entfernen:
- Zum Entfernen der Azure-Dateifreigaben, die wir für diese Schnellstartanleitung erstellt haben

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

- Zum Entfernen des eigentlichen Speicherkontos (Es werden implizit die von uns erstellten Azure-Dateifreigaben sowie alle anderen Speicherressourcen entfernt, die Sie ggf. erstellt haben, z.B. ein Azure Blob Storage-Container.)

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