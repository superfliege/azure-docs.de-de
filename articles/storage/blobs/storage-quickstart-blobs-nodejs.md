---
title: Azure-Schnellstart – Hochladen, Herunterladen und Auflisten von Blobs in Azure Storage mit Node.js | Microsoft-Dokumentation
description: In diesem Schnellstart erstellen Sie ein Speicherkonto und einen Container. Anschließend verwenden Sie die Speicherclientbibliothek für Node.js, um ein Blob in Azure Storage hochzuladen, ein Blob herunterzuladen und die Blobs in einem Container aufzulisten.
services: storage
author: craigshoemaker
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 03/15/2018
ms.author: cshoe
ms.openlocfilehash: 28f9936c297b6f641810e0c7783f4d84be108286
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="quickstart-upload-download-and-list-blobs-using-nodejs"></a>Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs mit Node.js

In diesem Schnellstart erfahren Sie, wie Sie mit Node.js in einem Container mit Azure-Blobspeicher Blockblobs hochladen, herunterladen und auflisten.

Für diese Schnellstartanleitung benötigen Sie ein [Azure-Abonnement](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Herunterladen der Beispielanwendung

Die in diesem Schnellstart verwendete [Beispielanwendung](https://github.com/Azure-Samples/storage-blobs-node-quickstart.git) ist eine einfache Node.js-Konsolenanwendung. Klonen Sie zunächst das Repository auf Ihren Computer, indem Sie den folgenden Befehl verwenden:

```bash
git clone https://github.com/Azure-Samples/storage-blobs-node-quickstart.git
```

Suchen Sie zum Öffnen der Anwendung nach dem Ordner *storage-blobs-node-quickstart*, und öffnen Sie ihn in Ihrer bevorzugten Bearbeitungsumgebung.

## <a name="configure-your-storage-connection-string"></a>Konfigurieren der Speicherverbindungszeichenfolge

Vor dem Ausführen der Anwendung müssen Sie die Verbindungszeichenfolge für das Speicherkonto angeben. Das Beispielrepository enthält eine Datei mit dem Namen *.env.example*. Sie können diese Datei umbenennen, indem Sie die Erweiterung *.example* entfernen, sodass die Datei nur noch *.env* heißt. Fügen Sie in der Datei *.env* Ihren Wert für die Verbindungszeichenfolge nach dem Schlüssel *AZURE_STORAGE_CONNECTION_STRING* hinzu.

## <a name="install-required-packages"></a>Installieren erforderlicher Pakete

Führen Sie im Anwendungsverzeichnis *npm install* aus, um die erforderlichen Pakete für die Anwendung zu installieren.

```bash
npm install
```

## <a name="run-the-sample"></a>Ausführen des Beispiels
Nachdem nun die Abhängigkeiten installiert wurden, können Sie das Beispiel ausführen, indem Sie Befehle an das Skript übergeben. Zum Erstellen eines Blobcontainers führen Sie beispielsweise den folgenden Befehl aus:

```bash
node index.js --command createContainer
```

Verfügbare Befehle:

| Befehl | Beschreibung |
|---------|---------|
|*createContainer* | Erstellt den Container *test-container* (Vorgang ist auch erfolgreich, wenn der Container bereits vorhanden ist). |
|*upload*          | Lädt die Datei *example.txt* in den Container *test-container* hoch. |
|*Herunterladen*        | Lädt den Inhalt des Blobs *example* in *example.downloaded.txt* hoch. |
|*delete*          | Löscht das Blob *example*. |
|*list*            | Listet den Inhalt des Containers *test-container* in der Konsole auf. |


## <a name="understanding-the-sample-code"></a>Grundlagen des Beispielcodes
In diesem Codebeispiel werden einige Module als Schnittstellen mit dem Dateisystem und der Befehlszeile verwendet. 

```javascript
if (process.env.NODE_ENV !== 'production') {
    require('dotenv').load();
}
const path = require('path');
const args = require('yargs').argv;
const storage = require('azure-storage');
```

Die Module haben den folgenden Zweck: 

- *dotenv* lädt Umgebungsvariablen, die in der Datei *.env* definiert sind, in den aktuellen Ausführungskontext.
- *path* ist erforderlich, um den absoluten Dateipfad der Datei zu ermitteln, die in Blobspeicher hochgeladen werden soll.
- *yargs* macht eine einfache Oberfläche zum Zugreifen auf Befehlszeilenargumente verfügbar.
- *azure-storage* ist das [Azure Storage-SDK](/nodejs/api/azure-storage)-Modul für Node.js.

Als Nächstes wird eine Reihe von Variablen initialisiert:

```javascript
const blobService = storage.createBlobService();
const containerName = 'test-container';
const sourceFilePath = path.resolve('./example.txt');
const blobName = path.basename(sourceFilePath, path.extname(sourceFilePath));
```

Die Variablen werden auf die folgenden Werte festgelegt:

- *blobService* ist auf eine neue Instanz des Azure Blob-Diensts festgelegt.
- *containerName* ist auf den Namen des Containers festgelegt.
- *sourceFilePath* ist auf den absoluten Pfad der hochzuladenden Datei festgelegt.
- *blobName* wird erstellt, indem der Dateiname verwendet und die Dateierweiterung entfernt wird.

In der folgenden Implementierung sind die *blobService*-Funktionen jeweils von einem *Promise*-Element (Zusage) umschlossen, mit dem der Zugriff auf die *async*-Funktion und den *await*-Operator von JavaScript ermöglicht wird, um das Rückrufverhalten der [Azure Storage-API](/nodejs/api/azure-storage/blobservice) zu optimieren. Wenn für jede Funktion eine erfolgreiche Antwort zurückgegeben wird, wird die Zusage mit relevanten Daten und einer spezifischen Nachricht für die Aktion aufgelöst.

### <a name="create-a-blob-container"></a>Erstellen eines Blobcontainers

Die Funktion *createContainer* ruft [createContainerIfNotExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createContainerIfNotExists) auf und legt die passende Zugriffsebene für das Blob fest.

```javascript
const createContainer = () => {
    return new Promise((resolve, reject) => {
        blobService.createContainerIfNotExists(containerName, { publicAccessLevel: 'blob' }, err => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Container '${containerName}' created` });
            }
        });
    });
};
```

Der zweite Parameter (*options*) für **createContainerIfNotExists** akzeptiert einen Wert für [publicAccessLevel](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createContainerIfNotExists). Der Wert *blob* für *publicAccessLevel* gibt an, dass bestimmte Blobdaten öffentlich verfügbar gemacht werden. Diese Einstellung ist anders als die Zugriffsebene *container*, bei der die Möglichkeit gewährt wird, den Inhalt des Containers aufzulisten.

Durch die Verwendung von **createContainerIfNotExists** kann die Anwendung den Befehl mehrfach *createContainer* ausführen, ohne Fehler zurückzugeben, wenn der Container bereits vorhanden ist. In einer Produktionsumgebung rufen Sie **createContainerIfNotExists** häufig nur einmal auf, da derselbe Container für die gesamte Anwendung verwendet wird. In diesen Fällen können Sie den Container im Voraus über das Portal oder die Azure CLI erstellen.

### <a name="upload-a-blob-to-the-container"></a>Hochladen eines Blobs in den Container

Für die Funktion *upload* wird die Funktion [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createBlockBlobFromLocalFile) verwendet, um eine Datei aus dem Dateisystem in Blobspeicher hochzuladen und zu schreiben bzw. zu überschreiben. 

```javascript
const upload = () => {
    return new Promise((resolve, reject) => {
        blobService.createBlockBlobFromLocalFile(containerName, blobName, sourceFilePath, err => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Upload of '${blobName}' complete` });
            }
        });
    });
};
```
Im Kontext der Beispielanwendung wird die Datei *example.txt* in ein Blob mit dem Namen *example* im Container *test-container* hochgeladen. Weitere Ansätze zum Hochladen von Inhalt in Blobs sind die Verwendung von [text](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createBlockBlobFromText) und [streams](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createBlockBlobFromStream).

Für die Überprüfung des Dateiuploads in Ihren Blobspeicher können Sie den [Azure Storage-Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/) verwenden, um die Daten in Ihrem Konto anzuzeigen.

### <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Mit der Funktion *list* rufen Sie die [listBlobsSegmented](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createBlockBlobFromText)-Methode auf, um eine Liste mit Blobmetadaten in einem Container zurückzugeben. 

```javascript
const list = () => {
    return new Promise((resolve, reject) => {
        blobService.listBlobsSegmented(containerName, null, (err, data) => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Items in container '${containerName}':`, data: data });
            }
        });
    });
};
```

Durch das Aufrufen von *listBlobsSegmented* werden Blobmetadaten als Array mit [BlobResult](/nodejs/api/azure-storage/blobresult)-Instanzen zurückgegeben. Ergebnisse werden in Batches mit einem Inkrement von 5.000 (Segmenten) zurückgegeben. Wenn in einem Container mehr als 5.000 Blobs vorhanden sind, enthalten die Ergebnisse einen Wert für **continuationToken**. Zum Auflisten nachfolgender Segmente aus dem Blobcontainer können Sie das Fortsetzungstoken als zweites Argument zurück an **listBlobSegmented** übergeben.

### <a name="download-a-blob-from-the-container"></a>Herunterladen eines Blobs aus dem Container

In der Funktion *download* wird [getBlobToLocalFile](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_getBlobToLocalFile) verwendet, um den Inhalt des Blobs an den Speicherort des angegebenen absoluten Dateipfads herunterzuladen.

```javascript
const download = () => {
    const dowloadFilePath = sourceFilePath.replace('.txt', '.downloaded.txt');
    return new Promise((resolve, reject) => {
        blobService.getBlobToLocalFile(containerName, blobName, dowloadFilePath, err => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Download of '${blobName}' complete` });
            }
        });
    });
};
```
Mit der hier angegebenen Implementierung wird der Quelldateipfad geändert und *.downloaded.txt* an den Dateinamen angefügt. In der Praxis können Sie den Speicherort und den Dateinamen beim Auswählen eines Downloadziels ändern.

### <a name="delete-blobs-in-the-container"></a>Löschen von Blobs im Container

Die Funktion *deleteBlock* (Alias: Konsolenbefehl *delete*) ruft die Funktion [deleteBlobIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteBlobIfExists) auf. Wie der Name schon erahnen lässt, gibt diese Funktion keinen Fehler zurück, wenn das Blob bereits gelöscht wurde.

```javascript
const deleteBlock = () => {
    return new Promise((resolve, reject) => {
        blobService.deleteBlobIfExists(containerName, blobName, err => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Block blob '${blobName}' deleted` });
            }
        });
    });
};
```

### <a name="upload-and-list"></a>Hochladen und Auflisten

Einer der Vorteile der Verwendung von Zusagen besteht darin, Befehle miteinander zu verketten. Die Funktion **uploadAndList** veranschaulicht, wie einfach es ist, den Inhalt eines Blobs direkt nach dem Hochladen einer Datei aufzulisten.

```javascript
const uploadAndList = () => {
    return _module.upload().then(_module.list);
};
```

### <a name="calling-code"></a>Aufrufen von Code

Um die implementierten Funktionen für die Befehlszeile verfügbar zu machen, wird jede Funktion einem Objektliteral zugeordnet.

```javascript
const _module = {
    "createContainer": createContainer,
    "upload": upload,
    "download": download,
    "delete": deleteBlock,
    "list": list,
    "uploadAndList": uploadAndList
};
```

Da *_module* jetzt vorhanden ist, sind alle Befehle über die Befehlszeile verfügbar.

```javascript
const commandExists = () => exists = !!_module[args.command];
```

Falls ein bestimmter Befehl nicht vorhanden ist, werden die Eigenschaften von *_module* in der Konsole als Hilfetext für Benutzer gerendert. 

Die Funktion *executeCommand* ist eine *async*-Funktion, mit der der jeweilige Befehl mit dem Operator *await* aufgerufen wird und alle Meldungen für Daten in der Konsole protokolliert werden.

```javascript
const executeCommand = async () => {
    const response = await _module[args.command]();

    console.log(response.message);

    if (response.data) {
        response.data.entries.forEach(entry => {
            console.log('Name:', entry.name, ' Type:', entry.blobType)
        });
    }
};
```

Abschließend ruft der ausführende Code zuerst *commandExists* auf, um sicherzustellen, dass ein bekannter Befehl an das Skript übergeben wird. Wenn ein vorhandener Befehl ausgewählt wird, wird der Befehl ausgeführt, und alle Fehler werden auf der Konsole protokolliert.

```javascript
try {
    const cmd = args.command;

    console.log(`Executing '${cmd}'...`);

    if (commandExists()) {
        executeCommand();
    } else {
        console.log(`The '${cmd}' command does not exist. Try one of these:`);
        Object.keys(_module).forEach(key => console.log(` - ${key}`));
    }
} catch (e) {
    console.log(e);
}
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie nicht planen, die in diesem Artikel erstellten Daten oder Konten zu verwenden, kann das Löschen der Daten ratsam sein, um unerwünschte Kosten zu vermeiden. Zum Löschen des Blobs und der Container können Sie die Methoden [deleteBlobIfExists](/nodejs/api/azure-storage/blobservice?view=azure-node-latest#deleteBlobIfExists_container__blob__options__callback_) und [deleteContainerIfExists](/nodejs/api/azure-storage/blobservice?view=azure-node-latest#deleteContainerIfExists_container__options__callback_) verwenden. Außerdem können Sie das Speicherkonto [über das Portal](../common/storage-create-storage-account.md) löschen.

## <a name="resources-for-developing-nodejs-applications-with-blobs"></a>Ressourcen für die Entwicklung von Node.js-Anwendungen mit Blobs

Sehen Sie sich diese zusätzlichen Ressourcen zur Node.js-Entwicklung mit Blobspeicher an:

### <a name="binaries-and-source-code"></a>Binärdateien und Quellcode

- Zeigen Sie den [Quellcode der Node.js-Clientbibliothek](https://github.com/Azure/azure-storage-node) für Azure Storage auf GitHub an, und installieren Sie ihn.

### <a name="client-library-reference-and-samples"></a>Clientbibliothek – Referenz und Beispiele

- Weitere Informationen zur Node.js-Clientbibliothek finden Sie in der [Referenz zur Node.js-API](https://docs.microsoft.com/en-us/javascript/api/overview/azure/storage).
- Erkunden Sie die [Beispiele für Blobspeicher](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=nodejs&term=blob), die mit der Node.js-Clientbibliothek geschrieben wurden.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung wurde veranschaulicht, wie Sie den Upload einer Datei zwischen einem lokalen Datenträger und Azure-Blobspeicher mit Node.js durchführen. Weitere Informationen zum Arbeiten mit Blob Storage finden Sie in der exemplarischen Vorgehensweise zu Blob Storage.

> [!div class="nextstepaction"]
> [Gewusst wie: Blob Storage-Vorgänge](storage-nodejs-how-to-use-blob-storage.md)

Die Node.js-Referenz für Azure Storage finden Sie unter [azure-storage package](https://docs.microsoft.com/javascript/api/azure-storage/?view=azure-node-latest) (azure-storage-Paket).