---
title: Speichern von Blockblobs auf Geräten – Azure IoT Edge | Microsoft-Dokumentation
description: Stellen Sie ein Azure Blob Storage-Modul für Ihr IoT Edge-Gerät bereit, um Daten am Edge zu speichern.
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 01/04/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 9faed53540d449f8658655ff7285b38aa20bee6c
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2019
ms.locfileid: "54901818"
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

Im Azure Marketplace stehen IoT Edge-Module zur Verfügung, die direkt auf Ihren IoT Edge-Geräten bereitgestellt werden können. Hierzu zählt auch Azure Blob Storage auf IoT Edge. Führen Sie die folgenden Schritte aus, um das Modul über das Azure-Portal bereitzustellen:

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach „Azure Blob Storage auf IoT Edge“. Wählen**Sie** das Marketplace-Suchergebnis aus.

   ![Erstellen des Moduls auf der Grundlage einer Marketplace-Suche](./media/how-to-store-data-blob/marketplace-module.png)

2. Wählen Sie ein IoT Edge-Gerät aus, das dieses Modul empfangen soll. Geben Sie auf der Seite **Zielgeräte für IoT Edge-Modul** die folgenden Informationen ein:

   1. Wählen Sie das **Abonnement** aus, das den von Ihnen verwendeten IoT-Hub enthält.

   2. Wählen Sie Ihren**IoT-Hub aus**.

   3. Wenn Ihnen der **Name des IoT Edge-Geräts** bekannt ist, geben Sie ihn in das Textfeld ein. Wählen Sie andernfalls **Gerät suchen** aus, um aus einer Liste mit IoT Edge-Geräten auf Ihrem IoT-Hub ein Gerät auszuwählen. 
   
   4. Klicken Sie auf **Erstellen**.

   Jetzt, da Sie ein IoT Edge-Modul im Azure Marketplace und ein das Modul empfangendes IoT Edge-Gerät ausgewählt haben, gelangen Sie zu einem aus drei Schritten bestehenden Assistenten, der Sie bei der genauen Definition der Bereitstellung des Moduls unterstützt.

3. Im Schritt **Module hinzufügen** des Assistenten zum Festlegen von Modulen sehen Sie, dass das Modul **AzureBlobStorageonIoTEdge** bereits unter **Bereitstellungsmodule** aufgeführt ist. 

2. Wählen Sie in der Liste mit den Bereitstellungsmodulen das Blob Storage-Modul aus, um die Moduldetails zu öffnen. 

   ![Auswählen des Modulnamens, um die Moduldetails zu öffnen](./media/how-to-store-data-blob/open-module-details.png)

3. Aktualisieren Sie auf der Seite **Benutzerdefinierte IoT Edge-Module** das Modul „Azure Blob Storage auf IoT Edge“:

   1. Ändern Sie unter **Name** den Namen des Moduls, sodass dieser nur aus Kleinbuchstaben besteht. Sie können das Modul umbenennen oder `azureblobstorageoniotedge` verwenden. 

      >[!IMPORTANT]
      >Bei Modulaufrufen für Azure IoT Edge muss die Groß-/Kleinschreibung beachtet werden, und das Storage SDK verwendet standardmäßig Kleinbuchstaben. Verwenden Sie daher einen Namen in Kleinbuchstaben, um sicherzustellen, dass Ihre Verbindungen mit dem Modul „Azure Blob Storage auf IoT Edge“ nicht unterbrochen werden. 

   2. Die Standardoptionen für die Containererstellung enthalten zwar die **Portbindungen, die Ihr Container** benötigt, Sie müssen jedoch auch Informationen zu Ihrem Speicherkonto sowie eine Bindung für das Speicherverzeichnis auf Ihrem Gerät hinzufügen. Überschreiben Sie den JSON-Code im Portal mit dem folgenden JSON-Code:
    
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
   3. Aktualisieren Sie den kopierten JSON-Code mit folgenden Informationen: 

      * Ersetzen Sie `<your storage account name>` durch einen leicht zu merkenden Namen. Kontonamen sollten drei bis 24 Zeichen bestehend aus Kleinbuchstaben und Zahlen enthalten.
      * Ersetzen Sie `<your storage account key>` durch einen Base64-Schlüssel mit 64 Bytes. Sie können einen Schlüssel mit Tools wie [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64) generieren. Mit diesen Anmeldeinformationen greifen Sie über andere Module auf Blob Storage zu.
      * Ersetzen Sie `<storage directory bind>` in Abhängigkeit von Ihrem Containerbetriebssystem. Geben Sie den Namen eines [Volumes](https://docs.docker.com/storage/volumes/) oder den absoluten Pfad zu einem Verzeichnis auf Ihrem IoT Edge-Gerät an, auf bzw. unter dem das Blobmodul Daten speichern soll. Die Speicherverzeichnisbindung ordnet einen Ort auf Ihrem Gerät einem festen Ort im Modul zu. 

         * Linux-Container: **\<Speicherpfad>:/blobroot**. Beispiel: „/srv/containerdata:/blobroot“. Alternativ: „Ihr-Volume:/blobroot“. 
         * Windows-Container: **\<Speicherpfad>:C:/BlobRoot**. Beispiel: „C:/ContainerData:C:/BlobRoot“. Alternativ: „Ihr-Volume:C:/blobroot“.
   
      > [!IMPORTANT]
      > Die zweite Hälfte des Werts für die Speicherverzeichnisbindung verweist auf einen bestimmten Ort im Modul und darf nicht geändert werden. Die Speicherverzeichnisbindung muss immer mit **:/blobroot** (Linux-Container) bzw. mit **:C:/BlobRoot** (Windows-Container) enden.

      ![Aktualisieren der Modulcontainer-Erstellungsoptionen – Portal](./media/how-to-store-data-blob/edit-module.png)

   4. Wählen Sie **Speichern** aus.

4. Wählen Sie **Weiter** aus, um mit dem nächsten Schritt des Assistenten fortzufahren.
5. Wählen Sie im Schritt **Routen angeben** des Assistenten die Option **Weiter** aus.
6. Klicken Sie im Schritt **Bereitstellung überprüfen** des Assistenten auf **Senden**.
7. Nach Übermittlung der Bereitstellung wird wieder die Seite **IoT Edge** Ihres IoT-Hubs angezeigt. Wählen Sie das IoT Edge-Zielgerät für die Bereitstellung aus, um dessen Details zu öffnen. 
8. Vergewissern Sie sich anhand der Gerätedetails, dass das Blob Storage-Modul als **In Bereitstellung angegeben** und als **Vom Gerät gemeldet** aufgeführt ist. Unter Umständen dauert es einen Moment, bis das Modul auf dem Gerät gestartet und an IoT Hub gemeldet wurde. Aktualisieren Sie die Seite, um einen aktualisierten Status anzuzeigen. 

### <a name="visual-studio-code-templates"></a>Visual Studio Code-Vorlagen

Azure IoT Edge bietet Vorlagen in Visual Studio Code, mit denen Sie Edgelösungen entwickeln können. Für diese Schritte muss [Visual Studio Code](https://code.visualstudio.com/) auf Ihrem Entwicklungscomputer installiert und mit den [Azure IoT-Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) konfiguriert sein.

Führen Sie die folgenden Schritte durch, um eine neue IoT Edge-Lösung mit einem Blob Storage-Modul zu erstellen, und konfigurieren Sie das Bereitstellungsmanifest. 

1. Wählen Sie **Ansicht** > **Befehlspalette** aus. 

2. Geben Sie in der Befehlspalette den folgenden Befehl ein, und führen Sie ihn aus: **Azure IoT Edge: New IoT Edge Solution** ein, und führen Sie ihn aus. Folgen Sie den Anweisungen in der Befehlspalette, um Ihre Projektmappe zu erstellen.

   | Feld | Wert |
   | ----- | ----- |
   | Ordner auswählen | Wählen Sie den Speicherort auf Ihrem Entwicklungscomputer aus, an dem Visual Studio Code die Projektmappendateien erstellen soll. |
   | Provide a solution name (Projektmappennamen angeben) | Geben Sie für Ihre Projektmappe einen aussagekräftigen Namen ein, oder übernehmen Sie den Standardnamen **EdgeSolution**. |
   | Select module template (Modulvorlage auswählen) | Wählen Sie die Option **Vorhandenes Modul (vollständige Image-URL angeben)** aus. |
   | Provide a module name (Modulname angeben) | Geben Sie einen klein geschriebenen Namen für das Modul ein (beispielsweise **azureblobstorage**).<br><br>Es ist wichtig, für das Modul „Azure Blob Storage auf IoT Edge“ einen Namen in Kleinbuchstaben zu verwenden. IoT Edge beachtet bei Modulverweisen die Groß-/Kleinschreibung, und das Storage SDK verwendet standardmäßig Kleinbuchstaben. |
   | Provide Docker image for the module (Docker-Image für das Modul angeben) | Geben Sie den Image-URI an: **mcr.microsoft.com/azure-blob-storage:latest** |

   Visual Studio Code verwendet die angegebenen Informationen, erstellt eine IoT Edge-Projektmappe, und lädt diese in einem neuen Fenster. Die Lösungsvorlage erstellt eine Bereitstellungsmanifestvorlage, die Ihr Blob Storage-Modulimage enthält, allerdings müssen Sie die Erstellungsoptionen des Moduls konfigurieren. 

3. Öffnen Sie **deployment.template.json** in Ihrem neuen Projektmappenarbeitsbereich, und machen Sie den Abschnitt **Module** ausfindig. Nehmen Sie folgende Konfigurationsänderungen vor:

   1. Löschen Sie das Modul **tempSensor**, da er für diese Bereitstellung nicht benötigt wird. 

   2. Kopieren Sie den folgenden Code, und fügen Sie ihn in das Feld **createOptions** Ihres Blob Storage-Moduls ein: 

      ```json
      "Env": [
        "LOCAL_STORAGE_ACCOUNT_NAME=$STORAGE_ACCOUNT_NAME","LOCAL_STORAGE_ACCOUNT_KEY=$STORAGE_ACCOUNT_KEY"
      ],
      "HostConfig":{
        "Binds": ["<storage directory bind>"],
        "PortBindings":{
          "11002/tcp": [{"HostPort":"11002"}]
        }
      }
      ```

      ![Aktualisieren von Optionen zur Modulerstellung – Visual Studio Code](./media/how-to-store-data-blob/create-options.png)

4. Aktualisieren Sie `<storage directory bind>` in den JSON-Erstellungsoptionen abhängig von Ihrem Containerbetriebssystem. Geben Sie den Namen eines [Volumes](https://docs.docker.com/storage/volumes/) oder den absoluten Pfad zu einem Verzeichnis auf Ihrem IoT Edge-Gerät an, auf bzw. unter dem das Blobmodul Daten speichern soll. Die Speicherverzeichnisbindung ordnet einen Ort auf Ihrem Gerät einem festen Ort im Modul zu.  

   * Linux-Container: **\<Speicherpfad>:/blobroot**. Beispiel: „/srv/containerdata:/blobroot“. Alternativ: „Ihr-Volume:/blobroot“.
   * Windows-Container: **\<Speicherpfad>:C:/BlobRoot**. Beispiel: „C:/ContainerData:C:/BlobRoot“. Alternativ: „Ihr-Volume:C:/blobroot“.
   
   > [!IMPORTANT]
   > Die zweite Hälfte des Werts für die Speicherverzeichnisbindung verweist auf einen bestimmten Ort im Modul und darf nicht geändert werden. Die Speicherverzeichnisbindung muss immer mit **:/blobroot** (Linux-Container) bzw. mit **:C:/BlobRoot** (Windows-Container) enden.

5. Speichern Sie die Datei **deployment.template.json**.

6. Öffnen Sie die **ENV-Datei** im Arbeitsbereich Ihrer Projektmappe. 

7. Die ENV-Datei ist für den Empfang von Anmeldeinformationen für die Containerregistrierung eingerichtet. Dies wird für das Blob Storage-Image jedoch nicht benötigt, da es öffentlich verfügbar ist. Ersetzen Sie die Datei stattdessen durch zwei neue Umgebungsvariablen: 

   ```env
   STORAGE_ACCOUNT_NAME=
   STORAGE_ACCOUNT_KEY=
   ```

8. Geben Sie einen Wert für `STORAGE_ACCOUNT_NAME` an. Kontonamen müssen zwischen drei und 24 Zeichen lang sein und dürfen nur Kleinbuchstaben und Zahlen enthalten. Geben Sie für `STORAGE_ACCOUNT_KEY` einen Base64-Schlüssel mit 64 Bytes an. Sie können einen Schlüssel mit Tools wie [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64) generieren. Mit diesen Anmeldeinformationen greifen Sie über andere Module auf Blob Storage zu. 

   Verwenden Sie vor und nach den angegebenen Werten keine Leerzeichen oder Anführungszeichen. 

9. Speichern Sie die Datei mit der Erweiterung **.env**. 

10. Klicken Sie mit der rechten Maustaste auf **deployment.template.json**, und klicken Sie dann auf **IoT Edge-Bereitstellungsmanifest generieren**. 

11. Visual Studio Code erstellt anhand der Information, die Sie in „deployment.template.json“ und „.env“ angegeben haben, eine neue Bereitstellungsmanifestdatei. Das Bereitstellungsmanifest wird in einem neuen **config**-Ordner in Ihrem Projektmappenarbeitsbereich erstellt. Sobald Sie über diese Datei verfügen, können Sie die Schritte unter [Bereitstellen von Azure IoT Edge-Modulen über Visual Studio Code](how-to-deploy-modules-vscode.md) oder [Bereitstellen von Azure IoT Edge-Modulen mit der Azure CLI](how-to-deploy-modules-cli.md) durchführen.

## <a name="connect-to-your-blob-storage-module"></a>Herstellen einer Verbindung mit Ihrem Blob Storage-Modul

Sie können mit dem Kontonamen und dem Kontoschlüssel, die Sie für Ihr Modul konfiguriert haben, auf Blob Storage auf Ihrem IoT Edge-Gerät zugreifen. 

Geben Sie Ihr IoT Edge-Gerät als Blobendpunkt für Speicheranforderungen an, die Sie vornehmen. Mithilfe der IoT Edge-Geräteinformationen und dem Kontonamen, den Sie konfiguriert haben, können Sie eine [Verbindungszeichenfolge für einen bestimmten Speicherendpunkt erstellen](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint). 

1. Für Module, die auf demselben Edgegerät bereitgestellt werden, auf dem „Azure Blob Storage auf IoT Edge“ ausgeführt wird, lautet der Blobendpunkt wie folgt: `http://<module name>:11002/<account name>`. 
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

