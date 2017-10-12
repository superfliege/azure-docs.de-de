---
title: "Ausführen von Vorgängen für den Azure-Blobspeicher (Objektspeicher) über die Azure-Befehlszeilenschnittstelle | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie Blobs in den Azure-Blobspeicher hochladen bzw. daraus herunterladen. Außerdem erfahren Sie, wie Sie eine SAS (Shared Access Signature) erstellen, um den Zugriff auf ein Blob in Ihrem Speicherkonto zu verwalten."
services: storage
documentationcenter: na
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/15/2017
ms.author: marsma
ms.openlocfilehash: c37fc0b701b668ab6bb9213a487ec8baa33fe663
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="perform-blob-storage-operations-with-azure-cli"></a>Ausführen von Blobspeichervorgängen über die Azure-Befehlszeilenschnittstelle

Der Azure-Blobspeicher ist ein Dienst zur Speicherung großer Mengen unstrukturierter Objektdaten, beispielsweise Text- oder Binärdaten, auf die von überall auf der Welt über HTTP oder HTTPS zugegriffen werden kann. In diesem Tutorial werden grundlegende Azure-Blobspeichervorgänge wie das Hochladen, Herunterladen und Löschen von Blobs behandelt. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines Containers
> * Hochladen einer Datei (Blob) in einen Container
> * Auflisten der Blobs in einem Container
> * Herunterladen eines Blobs aus einem Container
> * Kopieren eines Blobs zwischen Speicherkonten
> * Löschen eines BLOBs
> * Anzeigen und Ändern von Blobeigenschaften und -metadaten
> * Verwalten der Sicherheit mit einer SAS (Shared Access Signature)

Für dieses Tutorial wird mindestens Version 2.0.4 der Azure-Befehlszeilenschnittstelle vorausgesetzt. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>Erstellen eines Containers

Container ähneln Verzeichnissen auf Ihrem Computer. Das bedeutet, dass Sie Gruppen von Blobs in einem Container auf die gleiche Weise organisieren können wie Dateien oder Ordner auf Ihrem Computer. Ein Speicherkonto kann über eine beliebige Anzahl von Containern verfügen. In einem Container können bis zu 500 TB an Blobdaten gespeichert werden. Das entspricht gleichzeitig der maximal zulässigen Datenmenge für ein Speicherkonto.

Erstellen Sie mit dem Befehl [az storage container create](/cli/azure/storage/container#create) einen Container zum Speichern von Blobs.

```azurecli-interactive
az storage container create --name mystoragecontainer
```

Containernamen müssen mit einem Buchstaben oder einer Zahl beginnen und dürfen nur Buchstaben, Zahlen und Bindestriche (-) enthalten. Weitere Benennungsregeln für Blobs und Container finden Sie unter [Naming and Referencing Containers, Blobs, and Metadata](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Benennen von Containern, Blobs und Metadaten und Verweisen auf diese).

## <a name="enable-public-read-access-for-a-container"></a>Aktivieren von öffentlichem Lesezugriff für einen Container

Ein neu erstellter Container ist standardmäßig privat. Das bedeutet, dass niemand ohne [SAS](#create-a-shared-access-signature-sas) oder Zugriffsschlüssel für das Speicherkonto auf den Container zugreifen kann. Dieses Verhalten können Sie über die Azure-Befehlszeilenschnittstelle ändern, indem Sie die Containerberechtigungen auf eine von drei Ebenen festlegen:

| | |
|---|---|
| `--public-access off` | Kein öffentlicher Lesezugriff (Standardeinstellung) |
| `--public-access blob` | Nur öffentlicher Lesezugriff auf Blobs |
| `--public-access container` | Öffentlicher Lesezugriff auf Blobs sowie Berechtigung zum Auflisten der Blobs im Container |

Durch Festlegen des öffentlichen Zugriffs auf `blob` oder `container` aktivieren Sie den schreibgeschützten Zugriff für alle Benutzer im Internet. Den öffentlichen Lesezugriff müssen Sie beispielsweise aktivieren, wenn Sie auf Ihrer Website als Blobs gespeicherte Bilder anzeigen möchten. Wenn Sie dagegen Lese-/Schreibzugriff aktivieren möchten, müssen Sie eine [SAS (Shared Access Signature)](#create-a-shared-access-signature-sas) verwenden.

Aktivieren Sie den öffentlichen Lesezugriff für Ihren Container mithilfe des Befehls [az storage container set-permission](/cli/azure/storage/container#create).

```azurecli-interactive
az storage container set-permission \
    --name mystoragecontainer \
    --public-access container
```

## <a name="upload-a-blob-to-a-container"></a>Hochladen eines Blobs in einen Container

Blob Storage unterstützt Block-, Anfüge- und Seitenblobs. In Azure Storage werden hauptsächlich Blockblobs gespeichert. Anfügeblobs werden verwendet, wenn einem vorhandenen Blob Daten hinzugefügt werden müssen, ohne den vorhandenen Inhalt zu ändern (etwa bei der Protokollierung). Seitenblobs liegen den VHD-Dateien virtueller IaaS-Computer zugrunde.

In diesem Beispiel wird mit dem Befehl [az storage blob upload](/cli/azure/storage/blob#upload) ein Blob in den im letzten Schritt erstellten Container hochgeladen.

```azurecli-interactive
az storage blob upload \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/path/to/local/file
```

Dabei wird das Blob erstellt, falls es nicht vorhanden ist, oder überschrieben, falls es bereits vorhanden ist. Laden Sie mehrere Dateien hoch, sodass im nächsten Schritt beim Auflisten der Blobs mehrere Einträge angezeigt werden.

## <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Sie können die Blobs im Container mit dem Befehl [az storage blob list](/cli/azure/storage/blob#list) auflisten.

```azurecli-interactive
az storage blob list \
    --container-name mystoragecontainer \
    --output table
```

Beispielausgabe:

```
Name            Blob Type      Length  Content Type              Last Modified
--------------  -----------  --------  ------------------------  -------------------------
ISSUE_TEMPLATE  BlockBlob         761  application/octet-stream  2017-04-21T18:29:50+00:00
LICENSE         BlockBlob       18653  application/octet-stream  2017-04-21T18:28:50+00:00
LICENSE-CODE    BlockBlob        1090  application/octet-stream  2017-04-21T18:29:02+00:00
README.md       BlockBlob        6700  application/octet-stream  2017-04-21T18:27:33+00:00
dir1/file1.txt  BlockBlob        6700  application/octet-stream  2017-04-21T18:32:51+00:00
```

## <a name="download-a-blob"></a>Herunterladen eines Blobs

Laden Sie mithilfe des Befehls [az storage blob download](/cli/azure/storage/blob#download) das Blob herunter, das Sie in einem der vorherigen Schritte hochgeladen haben.

```azurecli-interactive
az storage blob download \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/destination/path/for/file
```

## <a name="copy-a-blob-between-storage-accounts"></a>Kopieren eines Blobs zwischen Speicherkonten

Sie können Blobs innerhalb oder zwischen Speicherkonten und Regionen asynchron kopieren.

Im folgenden Beispiel wird veranschaulicht, wie Sie Blobs von einem Speicherkonto in ein anderes kopieren. Zunächst erstellen wir einen Container im Quellspeicherkonto, der den öffentlichen Lesezugriff für seine Blobs angibt. Als Nächstes laden wir eine Datei in den Container hoch. Schließlich kopieren wird das Blob aus dem Container in das Zielspeicherkonto.

In diesem Beispiel muss der Zielcontainer bereits im Zielspeicherkonto vorhanden sein, damit der Kopiervorgang erfolgreich durchgeführt werden kann. Darüber hinaus muss das im `--source-uri`-Argument angegebene Quellblob entweder ein Token einer Shared Access Signature (SAS) enthalten oder öffentlich zugänglich sein.

```azurecli
# Create container in source account
az storage container create \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --name sourcecontainer \
    --public-access blob

# Upload blob to container in source account
az storage blob upload \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --container-name sourcecontainer \
    --file ~/path/to/sourcefile \
    --name sourcefile

# Copy blob from source account to destination account (destcontainer must exist)
az storage blob copy start \
    --account-name destaccountname \
    --account-key destaccountkey \
    --destination-blob destfile.png \
    --destination-container destcontainer \
    --source-uri https://sourceaccountname.blob.core.windows.net/sourcecontainer/sourcefile.png
```

## <a name="delete-a-blob"></a>Löschen eines BLOBs

Löschen Sie das Blob mithilfe des Befehls [az storage blob delete](/cli/azure/storage/blob#delete) aus dem Container.

```azurecli-interactive
az storage blob delete \
    --container-name mystoragecontainer \
    --name blobName
```

## <a name="display-and-modify-blob-properties-and-metadata"></a>Anzeigen und Ändern von Blobeigenschaften und -metadaten

Jedes Blob verfügt über mehrere dienstdefinierte Eigenschaften (wie Name, Typ, Länge und Ähnliches), die Sie mithilfe des Befehls [az storage blob show](/cli/azure/storage/blob#show) anzeigen können. Mithilfe des Befehls [az storage blob metadata update](/cli/azure/storage/blob/metadata#update) können Sie ein Blob auch mit Ihren eigenen Eigenschaften und Werten konfigurieren.

In diesem Beispiel zeigen wir zunächst die dienstdefinierten Eigenschaften eines Blobs an und aktualisieren das Blob anschließend mit zwei eigenen Metadateneigenschaften. Zum Schluss zeigen wir mithilfe des Befehls [az storage blob metadata show](/cli/azure/storage/blob/metadata#show) die Metadateneigenschaften des Blobs und deren Werte an.

```azurecli-interactive
# Show properties of a blob
az storage blob show \
    --container-name mystoragecontainer \
    --name blobName \
    --output table

# Set metadata properties of a blob (replaces all existing metadata)
az storage blob metadata update \
    --container-name mystoragecontainer \
    --name blobName \
    --metadata "key1=value1" "key2=value2"

# Show metadata properties of a blob
az storage blob metadata show \
    --container-name mystoragecontainer \
    --name blobName
```

## <a name="create-a-shared-access-signature-sas"></a>Erstellen einer SAS (Shared Access Signature)

Mit SAS (Shared Access Signatures) können Sie eingeschränkten Zugriff auf Objekte in Ihrem Speicherkonto gewähren, ohne die Zugriffsschlüssel Ihres Speicherkontos offenzulegen. Um einen URI zu erzeugen, der den Zugriff auf eine private Ressource erlaubt, erstellen Sie ein SAS-Token mit den gewünschten Berechtigungen und dem gewünschten Gültigkeitszeitfenster (effektive Dauer) und fügen es als Abfragezeichenfolge der URL einer Ressource hinzu, um einen vollständigen SAS-URI dafür zu erhalten. Dadurch kann jeder Benutzer, der über diesen SAS-URI (Ressourcen-URL plus SAS-Token) verfügt, während des Gültigkeitszeitfensters des SAS-Tokens auf die Ressource zugreifen. So können Sie beispielsweise den Lesezugriff auf ein privates Blob auf fünf Minuten beschränken, damit es nur in diesem Zeitraum angezeigt werden kann.

In den folgenden Schritten deaktivieren Sie den öffentlichen Zugriff auf Ihren Container, testen den rein privaten Zugriff und generieren und testen dann einen SAS-URI.

### <a name="disable-container-public-access"></a>Deaktivieren des öffentlichen Zugriffs auf den Container

Legen Sie zunächst die Zugriffsebene des Containers auf `off` fest. Dadurch wird festgelegt, dass nur mit einer SAS oder einem Speicherkonto-Zugriffsschlüssel auf den Container oder die darin enthaltenen Blobs zugegriffen werden kann.

```azurecli-interactive
az storage container set-permission \
    --name mystoragecontainer \
    --public-access off
```

### <a name="verify-private-access"></a>Überprüfen des privaten Zugriffs

Vergewissern Sie sich, dass kein öffentlicher Lesezugriff auf die Blobs in diesem Container möglich ist, indem Sie mit dem Befehl [az storage blob url](/cli/azure/storage/blob#url) die URL für eines der dazugehörigen Blobs abrufen.

```azurecli-interactive
az storage blob url \
    --container-name mystoragecontainer \
    --name blobName \
    --output tsv
```

Navigieren Sie in einem privaten Browserfenster zu der URL des Blobs. Daraufhin tritt der Fehler `ResourceNotFound` auf, da es sich hierbei um ein privates Blob handelt und Sie keine SAS angegeben haben.

### <a name="create-a-sas-uri"></a>Erstellen eines SAS-URI

Als Nächstes erstellen wir einen SAS-URI, der den Zugriff auf das Blob erlaubt. Im folgenden Beispiel füllen wir zunächst mit [az storage blob url](/cli/azure/storage/blob#url) eine Variable mit der URL für das Blob und anschließend eine weitere Variable mit einem SAS-Token auf, das mit dem Befehl [az storage blob generate-sas](/cli/azure/storage/blob#generate-sas) generiert wurde. Abschließend geben wir den vollständigen SAS-URI für das Blob durch Verkettung der beiden Variablen (mit dem Abfragezeichenfolgen-Trennzeichen `?`) aus.

```azurecli-interactive
# Get UTC datetimes for SAS start and expiry (Example: 1994-11-05T13:15:30Z)
sas_start=`date -u +'%Y-%m-%dT%H:%M:%SZ'`
sas_expiry=`date -u +'%Y-%m-%dT%H:%M:%SZ' -d '+2 minute'`

# Get the URL for the blob
blob_url=`az storage blob url \
    --container-name mystoragecontainer \
    --name blobName \
    --output tsv`

# Obtain a SAS token granting read (r) access between the
# SAS start and expiry times
sas_token=`az storage blob generate-sas \
    --container-name mystoragecontainer \
    --name blobName \
    --start $sas_start \
    --expiry $sas_expiry \
    --permissions r \
    --output tsv`

# Display the full SAS URI for the blob
echo $blob_url?$sas_token
```

### <a name="test-the-sas-uri"></a>Testen des SAS-URI

Navigieren Sie in einem privaten Browserfenster zum vollständigen SAS-URI, den Sie im vorherigen Codeausschnitt mit dem Befehl `echo` angezeigt haben. Diesmal tritt kein Fehler auf, und Sie können das Blob anzeigen oder herunterladen.

Warten Sie, bis die URL abgelaufen ist (in diesem Beispiel zwei Minuten), und navigieren Sie in einem anderen privaten Browserfenster zu dem URI. Nun tritt der Fehler `AuthenticationFailed` auf, da das SAS-Token abgelaufen ist.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcen in Ihrer Ressourcengruppe (einschließlich des in diesem Tutorial erstellten Speicherkontos und der ggf. hochgeladenen Blobs) nicht mehr benötigen, löschen Sie die Ressourcengruppe mit dem Befehl [az group delete](/cli/azure/group#delete).

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie sich mit der grundlegenden Verwendung von Blobs in Azure Storage vertraut gemacht.

> [!div class="checklist"]
> * Erstellen eines Containers
> * Hochladen einer Datei (Blob) in einen Container
> * Auflisten der Blobs in einem Container
> * Herunterladen eines Blobs aus einem Container
> * Kopieren eines Blobs zwischen Speicherkonten
> * Löschen eines BLOBs
> * Anzeigen und Ändern von Blobeigenschaften und -metadaten
> * Verwalten der Sicherheit mit einer SAS (Shared Access Signature)

Die folgenden Ressourcen enthalten weitere Informationen zur Verwendung der Azure-Befehlszeilenschnittstelle sowie zur Verwendung der Ressourcen in Ihrem Speicherkonto:

* Azure-Befehlszeilenschnittstelle
  * [Log in with Azure CLI 2.0](/cli/azure/authenticate-azure-cli) (Anmelden mit Azure CLI 2.0): Hier finden Sie Informationen zu den verschiedenen Authentifizierungsmethoden mit der Befehlszeilenschnittstelle. Hierzu zählt unter anderem die nicht interaktive Anmeldung per [Dienstprinzipal](/cli/azure/authenticate-azure-cli#logging-in-with-a-service-principal) zum Ausführen unbeaufsichtigter Azure-CLI-Skripts.
  * [Azure CLI 2.0: Befehlsreferenz](/cli/azure/)
* Microsoft Azure-Speicher-Explorer
  * Beim [Microsoft Azure-Speicher-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) handelt es sich um eine kostenlose eigenständige App von Microsoft, über die Sie unter Windows, macOS und Linux visuell mit Azure Storage-Daten arbeiten können.
