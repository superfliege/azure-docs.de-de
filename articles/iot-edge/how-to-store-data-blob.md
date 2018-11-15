---
title: Azure Blob Storage auf Azure IoT Edge-Geräten | Microsoft-Dokumentation
description: Stellen Sie ein Azure Blob Storage-Modul für Ihr IoT Edge-Gerät bereit, um Daten am Edge zu speichern.
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 10/03/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: fa88ff46b4fb93d55aa0087cca0e6184f3e087a0
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567280"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>Speichern von Daten am Edge mit Azure Blob Storage in IoT Edge (Vorschau)

Mit Azure Blob Storage in IoT Edge erhalten Sie eine [Blockblob](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs)-Speicherlösung im Edgebereich. Ein Blob Storage-Modul auf Ihrem IoT Edge-Gerät verhält sich wie ein Azure-Blockblob-Dienst, aber die Blockblobs werden lokal auf Ihrem IoT Edge-Gerät gespeichert. Sie können mit den gleichen Azure Storage SDK-Methoden oder Blockblob-API-Aufrufen auf Ihre Blobs zugreifen, mit denen Sie bereits arbeiten. 

Szenarien, in denen Daten wie Videos, Bilder, Finanzdaten, Krankenhausdaten oder beliebige andere lokal gespeicherte Daten verwendet werden, die lokal verarbeitet oder in die Cloud übertragen werden sollen, sind gute Beispiele für die Nutzung dieses Moduls.

Dieser Artikel enthält Anweisungen zum Bereitstellen von Azure Blob Storage für einen IoT Edge-Container, der einen Blobdienst auf Ihrem IoT Edge-Gerät ausführt. 

>[!NOTE]
>Azure Blob Storage in IoT Edge ist als [öffentliche Vorschau](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) verfügbar. 

## <a name="prerequisites"></a>Voraussetzungen

Ein Azure IoT Edge-Gerät:

* Sie können Ihren Entwicklungscomputer oder einen virtuellen Computer als Edge-Gerät verwenden, indem Sie die Schritte ausführen, die in der Schnellstartanleitung für [Linux](quickstart-linux.md)- oder [Windows](quickstart.md)-Geräte beschrieben sind.
* Azure Blob Storage im IoT Edge-Modul unterstützt die folgenden Gerätekonfigurationen:

   | Betriebssystem | Architecture |
   | ---------------- | ------------ |
   | Ubuntu Server 16.04 | AMD64 |
   | Ubuntu Server 18.04 | AMD64 |
   | Windows 10 IoT Core (Oktober-Update) | AMD64 |
   | Windows 10 Enterprise (Oktober-Update) | AMD64 |
   | Windows Server 2019 | AMD64 |
   | Raspbian Stretch | ARM32 |

Cloudressourcen:

* Ein [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) mit Standardtarif in Azure. 


## <a name="deploy-blob-storage-to-your-device"></a>Bereitstellen von Blob Storage auf Ihrem Gerät

Es gibt mehrere Möglichkeiten, um Module auf einem IoT Edge-Gerät bereitzustellen. Alle sind für Azure Blob Storage auf IoT Edge-Modulen geeignet. Die beiden einfachsten Methoden stellen die Verwendung des Azure-Portals oder von Visual Studio Code-Vorlagen dar. 

### <a name="azure-portal"></a>Azure-Portal

#### <a name="find-the-module"></a>Suchen nach dem Modul

Wählen Sie eine von zwei Möglichkeiten, um nach dem Blob Storage-Modul zu suchen:

1. Suchen Sie im Azure-Portal nach „Azure Blob Storage auf IoT Edge“. **Wählen** Sie anschließend den Suchergebniseintrag aus.
2. Navigieren Sie im Azure-Portal zum Marketplace, und klicken Sie auf „Internet der Dinge“. Wählen Sie im Abschnitt „IoT Edge-Module“ die Option „Azure Blob Storage auf IoT Edge“. Klicken Sie anschließend auf **Erstellen**.

#### <a name="steps-to-deploy"></a>Schritte zum Bereitstellen

**Zielgeräte für IoT Edge-Modul**

1. Wählen Sie das „Abonnement“ aus, unter dem Ihre IoT Hub-Instanz bereitgestellt wurde.
2. Wählen Sie unter „IoT Hub“ Ihre IoT Hub-Instanz aus.
3. Geben Sie den „IoT Edge-Gerätenamen“ des Geräts an, auf dem Sie dieses Modul bereitstellen möchten. Sie können „Gerät suchen“ verwenden, um Ihr Gerät zu ermitteln.
4. Klicken Sie auf **Create**.

**Festlegen von Modulen**

1. Im Abschnitt „Module hinzufügen“ unter „Bereitstellungsmodule“ sehen Sie, dass das Modul bereits aufgeführt ist und der Name mit „AzureBlobStorageonIoTEdge“ beginnt. 
2. **Wählen** Sie das Blob Storage-Modul in der Liste „Bereitstellungsmodule“ aus. Die Seitenleiste „ Benutzerdefinierte IoT Edge-Module“ wird geöffnet.
3. **Name**: Hier können Sie den Namen des Moduls ändern.
4. **Image-URI**: Ersetzen Sie den URI durch **mcr.microsoft.com/azure-blob-storage:latest**.
5. **Optionen für Containererstellung**: Bearbeiten Sie den unten angegebenen JSON-Code mit Ihren Werten, und fügen Sie ihn anstelle des JSON-Codes auf der Portal-Seite ein:
   
   ```json
   {
       "Env":[
           "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
           "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
       ],
       "HostConfig":{
           "Binds":[
               "<storage directory bind>"
           ],
           "PortBindings":{
               "11002/tcp":[{"HostPort":"11002"}]
           }
       }
   }
   ```   
   
    * Aktualisieren Sie `<your storage account name>`. Kontonamen sollten drei bis 24 Zeichen bestehend aus Kleinbuchstaben und Zahlen enthalten.
    * Aktualisieren Sie `<your storage account key>` mit einem 64-Byte-Base64-Schlüssel. Sie können einen Schlüssel mit Tools wie [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64) generieren. Mit diesen Anmeldeinformationen greifen Sie über andere Module auf Blob Storage zu.
    * Aktualisieren Sie `<storage directory bind>`. Dies richtet sich jeweils nach dem Containerbetriebssystem. Geben Sie den Namen eines [Volumes](https://docs.docker.com/storage/volumes/) oder den absoluten Pfad zu einem Verzeichnis auf Ihrem IoT Edge-Gerät an, auf bzw. unter dem das Blobmodul Daten speichern soll.  

       * Linux-Container: **\<Speicherpfad>:/blobroot**. Beispiel: „/srv/containerdata:/blobroot“. Alternativ: „Ihr-Volume:/blobroot“. 
       * Windows-Container: **\<Speicherpfad>:C:/BlobRoot**. Beispiel: „C:/ContainerData:C:/BlobRoot“. Alternativ: „Ihr-Volume:C:/blobroot“.
   
   > [!CAUTION]
   > Ändern Sie nicht die Werte „/blobroot“ (Linux) bzw. „C:/BlobRoot“ (Windows) für **\<Storage directory bind>** (Speicherverzeichnisbindung).

    ![Aktualisieren von Modulwerten](./media/how-to-store-data-blob/edit-module.png)

6. **Speichern** Sie die Werte unter „Benutzerdefinierte IoT Edge-Module“.
7. Klicken Sie im Abschnitt „Module festlegen“ auf **Weiter**.
8. Klicken Sie im Abschnitt „Routen angeben“ auf **Weiter**.
9. Klicken Sie nach der Überprüfung im Abschnitt „Bereitstellung überprüfen“ auf **Übermitteln**.
10. Vergewissern Sie sich auf Ihrer IoT Hub-Instanz, dass das Blob Storage-Modul vom Gerät ausgeführt wird. 

### <a name="visual-studio-code-templates"></a>Visual Studio Code-Vorlagen

Azure IoT Edge bietet Vorlagen in Visual Studio Code, mit denen Sie Edgelösungen entwickeln können. Für die Durchführung dieser Schritte muss [Visual Studio Code](https://code.visualstudio.com/) auf Ihrem Entwicklungscomputer installiert und mit der [Azure IoT Edge-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) konfiguriert sein.

Führen Sie die folgenden Schritte durch, um eine neue IoT Edge-Lösung mit einem Blob Storage-Modul zu erstellen, und konfigurieren Sie das Bereitstellungsmanifest. 

1. Wählen Sie **Ansicht** > **Befehlspalette** aus. 

2. Geben Sie in der Befehlspalette den Befehl **Azure IoT Edge: New IoT Edge Solution** ein, und führen Sie ihn aus. 

3. Befolgen Sie die Anweisungen zum Erstellen einer neuen Projektmappe: 

   1. **Ordner auswählen**: Navigieren Sie zu dem Ordner, in dem die neue Projektmappe erstellt werden soll.  
   
   2. **Projektmappennamen angeben**: Geben Sie einen Namen für Ihre Projektmappe ein, oder übernehmen Sie den Standardnamen.
   
   3. **Modulvorlage auswählen**: Wählen Sie die Option **Vorhandenes Modul (vollständige Image-URL angeben)** aus.
   
   4. **Modulnamen angeben**: Geben Sie einen aussagekräftigen Namen für Ihr Modul an, z.B. **azureBlobStorage**.
   
   5. **Docker-Image für das Modul angeben**: Geben Sie den Image-URI an: **mcr.microsoft.com/azure-blob-storage:latest**.

Visual Studio Code verwendet die angegebenen Informationen, erstellt eine IoT Edge-Projektmappe, und lädt diese in einem neuen Fenster. 

Die Lösungsvorlage erstellt eine Bereitstellungsmanifestvorlage, die Ihr Blob Storage-Modulimage enthält, allerdings müssen Sie die Erstellungsoptionen des Moduls konfigurieren. 

1. Öffnen Sie **deployment.template.json** in Ihrem neuen Projektmappenarbeitsbereich, und machen Sie den Abschnitt **Module** ausfindig. 

2. Löschen Sie das Modul **tempSensor**, da er für diese Bereitstellung nicht benötigt wird. 

3. Kopieren Sie den folgenden Code, und fügen Sie ihn in das Feld **createOptions** Ihres Blob Storage-Moduls ein: 

   ```json
   {\"Env\": [\"LOCAL_STORAGE_ACCOUNT_NAME=$STORAGE_ACCOUNT_NAME\",\" LOCAL_STORAGE_ACCOUNT_KEY=$STORAGE_ACCOUNT_KEY\"],\"HostConfig\": {\"Binds\": [\"<storage directory bind>\"],\"PortBindings\": {\"11002/tcp\": [{\"HostPort\":\"11002\"}]}}}
   ```

   ![Aktualisieren der Modulerstellungsoptionen](./media/how-to-store-data-blob/create-options.png)

4. Aktualisieren Sie `<storage directory bind>` in den JSON-Erstellungsoptionen abhängig von Ihrem Containerbetriebssystem. Geben Sie den Namen eines [Volumes](https://docs.docker.com/storage/volumes/) oder den absoluten Pfad zu einem Verzeichnis auf Ihrem IoT Edge-Gerät an, auf bzw. unter dem das Blobmodul Daten speichern soll.  

   * Linux-Container: **\<Speicherpfad>:/blobroot**. Beispiel: „/srv/containerdata:/blobroot“. Alternativ: „Ihr-Volume:/blobroot“.
   * Windows-Container: **\<Speicherpfad>:C:/BlobRoot**. Beispiel: „C:/ContainerData:C:/BlobRoot“. Alternativ: „Ihr-Volume:C:/blobroot“.
   
   > [!CAUTION]
   > Ändern Sie nicht die Werte „/blobroot“ (Linux) bzw. „C:/BlobRoot“ (Windows) für **\<Storage directory bind>** (Speicherverzeichnisbindung).

5. Speichern Sie **deployment.template.json**.

6. Öffnen Sie **.env** in Ihrem Projektmappenarbeitsbereich. 

7. Sie müssen keine Containerregistrierungswerte für das Blob Storage-Image eingeben, da diese öffentlich verfügbar sind. Fügen Sie stattdessen zwei neue Umgebungsvariablen hinzu: 

   ```env
   STORAGE_ACCOUNT_NAME=
   STORAGE_ACCOUNT_KEY=
   ```

8. Geben Sie einen Wert für `STORAGE_ACCOUNT_NAME` an. Kontonamen sollten drei bis 24 Zeichen lang sein und nur Kleinbuchstaben und Zahlen enthalten. Geben Sie außerdem einen Base64-Schlüssel mit 64 Byte für `STORAGE_ACCOUNT_KEY` an. Sie können einen Schlüssel mit Tools wie [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64) generieren. Mit diesen Anmeldeinformationen greifen Sie über andere Module auf Blob Storage zu. 

9. Speichern Sie **.env**. 

10. Klicken Sie mit der rechten Maustaste auf **deployment.template.json**, und klicken Sie dann auf **IoT Edge-Bereitstellungsmanifest generieren**. 

Visual Studio Code erstellt anhand der Information, die Sie in „deployment.template.json“ und „.env“ angegeben haben, eine neue Bereitstellungsmanifestdatei. Das Bereitstellungsmanifest wird in einem neuen **config**-Ordner in Ihrem Projektmappenarbeitsbereich erstellt. Sobald Sie über diese Datei verfügen, können Sie die Schritte unter [Bereitstellen von Azure IoT Edge-Modulen über Visual Studio Code](how-to-deploy-modules-vscode.md) oder [Bereitstellen von Azure IoT Edge-Modulen mit der Azure CLI](how-to-deploy-modules-cli.md) durchführen.

## <a name="connect-to-your-blob-storage-module"></a>Herstellen einer Verbindung mit Ihrem Blob Storage-Modul

Sie können mit dem Kontonamen und dem Kontoschlüssel, die Sie für Ihr Modul konfiguriert haben, auf Blob Storage auf Ihrem IoT Edge-Gerät zugreifen. 

Geben Sie Ihr IoT Edge-Gerät als Blobendpunkt für Speicheranforderungen an, die Sie vornehmen. Mithilfe der IoT Edge-Geräteinformationen und dem Kontonamen, den Sie konfiguriert haben, können Sie eine [Verbindungszeichenfolge für einen bestimmten Speicherendpunkt erstellen](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint). 

1. Für Module, die auf demselben Edgegerät bereitgestellt werden, auf dem „Azure Blob Storage auf IoT Edge“ ausgeführt wird, lautet der Blobendpunkt wie folgt: `http://<Module Name>:11002/<account name>`. 
2. Für Module, die auf einem anderen Edgegerät als auf dem Edgegerät mit Ausführung von „Azure Blob Storage auf IoT Edge“ bereitgestellt werden, lautet der Blobendpunkt wie folgt (je nach Ihrem Setup): `http://<device IP >:11002/<account name>`, `http://<IoT Edge device hostname>:11002/<account name>` oder `http://<FQDN>:11002/<account name>`.

## <a name="logs"></a>Protokolle

Sie finden die Protokolle im Container unter: 
* Für Linux: /blobroot/logs/platformblob.log

## <a name="deploy-multiple-instances"></a>Bereitstellen mehrerer Instanzen

Wenn Sie mehrere Instanzen von Azure Blob Storage in IoT Edge bereitstellen möchten, müssen Sie nur den HostPort ändern, an den das Modul gebunden wird. Die Blob Storage-Module machen immer Port 11002 im Container verfügbar, aber Sie können deklarieren, an welchen Port dieser auf dem Host gebunden wird. 

Bearbeiten Sie die Optionen zur Modulerstellung, um den HostPort-Wert zu ändern:

```json
\"PortBindings\": {\"11002/tcp\": [{\"HostPort\":\"<port number>\"}]}
```

Wenn Sie zusätzliche Blob Storage-Module verbinden, ändern Sie den Endpunkt dahingehend, dass er auf den aktualisierten Hostport zeigt. 

### <a name="try-it-out"></a>Ausprobieren

Die Dokumentation zu Azure Blob Storage enthält Schnellstartanleitungen, in denen Beispielcode in mehreren Sprachen bereitgestellt wird. Sie können diese Beispiele zum Testen von Azure Blob Storage in IoT Edge herunterladen, indem Sie den Blobendpunkt dahingehend ändern, dass er auf Ihr Blob Storage-Modul zeigt.

Die folgenden Schnellstartanleitungen verwenden Sprachen, die auch von IoT Edge unterstützt werden, sodass Sie sie als IoT Edge-Module zusammen mit dem Blob Storage-Modul bereitstellen können:

* [.NET](../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Java](../storage/blobs/storage-quickstart-blobs-java.md)
* [Python](../storage/blobs/storage-quickstart-blobs-python.md)
* [Node.js](../storage/blobs/storage-quickstart-blobs-nodejs.md)

## <a name="supported-storage-operations"></a>Unterstützte Speichervorgänge

Blob Storage-Module in IoT Edge verwenden die gleichen Azure Storage SDKs und entsprechen der Version „2018-03-28“ der Azure Storage-API für Blockblobendpunkte. Spätere Releases hängen von den Kundenanforderungen ab. 

Nicht alle Azure Blob Storage-Vorgänge werden von Azure Blob Storage in IoT Edge unterstützt. In den folgenden Abschnitten wird aufgeführt, welche Vorgänge unterstützt und welche nicht unterstützt werden. 

### <a name="account"></a>Konto

Unterstützt: 
* Auflisten von Containern

Nicht unterstützt: 
* Abrufen und Festlegen von Blobdiensteigenschaften
* Preflightüberprüfung von Blobanforderungen
* Abrufen von Statistiken zum Blobdienst
* Abrufen von Kontoinformationen

### <a name="containers"></a>Container

Unterstützt: 
* Erstellen und Löschen von Containern
* Abrufen von Containereigenschaften und -metadaten
* Auflisten von Blobs

Nicht unterstützt: 
* Abrufen und Festlegen von Container-ACLs
* Abrufen der Lease von Containern
* Festlegen von Containermetadaten

### <a name="blobs"></a>Blobs (in englischer Sprache)

Unterstützt: 
* Festlegen, Abrufen und Löschen von Blobs
* Abrufen und Festlegen von Blobeigenschaften
* Abrufen und Festlegen von Blobmetadaten

Nicht unterstützt: 
* Abrufen der Lease von Blobs
* Erstellen einer Momentaufnahme für Blobs
* Kopieren von Blobs und Abbrechen eines Blobkopiervorgangs
* Wiederherstellen von Blobs
* Festlegen des Blobtarifs

### <a name="block-blobs"></a>Blockblobs

Unterstützt: 
* Festlegen von Blöcken: Der Block muss eine Größe von maximal 4 MB haben.
* Festlegen und Abrufen von Blocklisten

Nicht unterstützt:
* Festlegen von Blocks über die URL

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md).

