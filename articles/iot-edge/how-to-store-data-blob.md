---
title: Speichern von Blockblobs auf Geräten – Azure IoT Edge | Microsoft-Dokumentation
description: Stellen Sie ein Azure Blob Storage-Modul für Ihr IoT Edge-Gerät bereit, um Daten am Edge zu speichern.
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 03/07/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 0fc34c913453abd174009213233a54e30b9346d3
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57881383"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>Speichern von Daten am Edge mit Azure Blob Storage in IoT Edge (Vorschau)

Mit Azure Blob Storage in IoT Edge erhalten Sie eine [Blockblob](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs)-Speicherlösung im Edgebereich. Ein Blob Storage-Modul auf Ihrem IoT Edge-Gerät verhält sich wie ein Azure-Blockblob-Dienst, aber die Blockblobs werden lokal auf Ihrem IoT Edge-Gerät gespeichert. Sie können mit den gleichen Azure Storage SDK-Methoden oder Blockblob-API-Aufrufen auf Ihre Blobs zugreifen, mit denen Sie bereits arbeiten. 

Dieses Modul verfügt standardmäßig über die Features **automatisches Tiering** und **automatisches Ablaufen**.

> [!NOTE]
> Die Funktionen für automatisches Tiering und automatisches Ablaufen sind derzeit nur in Linux AMD64 und Linux ARM32 verfügbar.

**Automatisches Tiering** ist eine konfigurierbare Funktion. Sie ermöglicht das automatische Hochladen der Daten aus Ihrem lokalen Blobspeicher in Azure und unterstützt sporadische Internetkonnektivität. Die Funktion ermöglicht Folgendes:
- Aktivieren/Deaktivieren des Tiering-Features
- Auswählen der Reihenfolge, in der die Daten in Azure kopiert werden sollen (beispielsweise „NewestFirst“ oder „OldestFirst“)
- Angeben des Azure Storage-Kontos, in das Ihre Daten hochgeladen werden sollen
- Angeben der Container, die Sie in Azure hochladen möchten. Mit diesem Modul können Sie sowohl Quell- als auch Zielcontainernamen angeben.
- Vollständiges Blob-Tiering (mithilfe des Vorgangs `Put Blob`) sowie Tiering auf Blockebene (mithilfe der Vorgänge `Put Block` und `Put Block List`)

Dieses Modul verwendet Tiering auf Blockebene, wenn Ihr Blob aus Blöcken besteht. Im Anschluss finden Sie einige gängige Szenarien:
- Wenn Ihre Anwendung einige Blöcke eines zuvor hochgeladenen Blobs aktualisiert, lädt dieses Modul nicht das gesamte Blob hoch, sondern nur die aktualisierten Blöcke.
- Falls beim Hochladen eines Blobs die Internetverbindung unterbrochen wird, lädt das Modul ebenfalls nur die restlichen Blöcke und nicht das gesamte Blob hoch, wenn die Verbindung wiederhergestellt wurde.

Im Falle einer unerwarteten Prozessbeendigung während eines Blob-Uploads (etwa durch einen Stromausfall) werden alle Blöcke, die für den Upload vorgesehen waren, erneut hochgeladen, wenn das Modul wieder online ist.

**Automatisches Ablaufen** ist eine konfigurierbare Funktion, die dafür sorgt, dass Ihre Blobs nach Ablauf der Gültigkeitsdauer (Time To Live, TTL) automatisch aus dem lokalen Speicher gelöscht werden. Der Wert wird in Minuten angegeben. Die Funktion ermöglicht Folgendes:
- Aktivieren/Deaktivieren des automatischen Ablaufens
- Angeben der Gültigkeitsdauer in Minuten

Szenarien, in denen Daten wie Videos, Bilder, Finanzdaten, Krankenhausdaten oder beliebige andere lokal gespeicherte Daten verwendet werden, die lokal verarbeitet oder in die Cloud übertragen werden sollen, sind gute Beispiele für die Nutzung dieses Moduls.

Dieser Artikel enthält Anweisungen zum Bereitstellen von Azure Blob Storage für einen IoT Edge-Container, der einen Blobdienst auf Ihrem IoT Edge-Gerät ausführt. 

>[!NOTE]
>Azure Blob Storage in IoT Edge ist als [öffentliche Vorschau](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) verfügbar. 

Das folgende Video enthält eine kurze Einführung:
> [!VIDEO https://www.youtube.com/embed/wkprcfVidyM]

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

   4. Legen Sie [automatisches Tiering und automatisches Ablaufen](#configure-auto-tiering-and-auto-expiration-via-azure-portal) in den gewünschten Eigenschaften fest. Eine Liste mit Eigenschaften und möglichen Werten für automatisches Tiering und automatisches Ablaufen finden Sie [hier](#auto-tiering-properties) bzw. [hier](#auto-expiration-properties). 

   5. Wählen Sie **Speichern** aus. 

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

5. Konfigurieren Sie [automatisches Tiering und automatisches Ablaufen](#configure-auto-tiering-and-auto-expiration-via-vscode). Eine Liste mit Eigenschaften für automatisches Tiering und automatisches Ablaufen finden Sie [hier](#auto-tiering-properties) bzw. [hier](#auto-expiration-properties).

6. Speichern Sie die Datei **deployment.template.json**.

7. Öffnen Sie die **ENV-Datei** im Arbeitsbereich Ihrer Projektmappe. 

8. Die ENV-Datei ist für den Empfang von Anmeldeinformationen für die Containerregistrierung eingerichtet. Dies wird für das Blob Storage-Image jedoch nicht benötigt, da es öffentlich verfügbar ist. Ersetzen Sie die Datei stattdessen durch zwei neue Umgebungsvariablen: 

   ```env
   STORAGE_ACCOUNT_NAME=
   STORAGE_ACCOUNT_KEY=
   ```

9. Geben Sie einen Wert für `STORAGE_ACCOUNT_NAME` an. Kontonamen müssen zwischen drei und 24 Zeichen lang sein und dürfen nur Kleinbuchstaben und Zahlen enthalten. Geben Sie für `STORAGE_ACCOUNT_KEY` einen Base64-Schlüssel mit 64 Bytes an. Sie können einen Schlüssel mit Tools wie [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64) generieren. Mit diesen Anmeldeinformationen greifen Sie über andere Module auf Blob Storage zu. 

   Verwenden Sie vor und nach den angegebenen Werten keine Leerzeichen oder Anführungszeichen. 

10. Speichern Sie die Datei mit der Erweiterung **.env**. 

11. Klicken Sie mit der rechten Maustaste auf **deployment.template.json**, und klicken Sie dann auf **IoT Edge-Bereitstellungsmanifest generieren**. 

12. Visual Studio Code erstellt anhand der Information, die Sie in „deployment.template.json“ und „.env“ angegeben haben, eine neue Bereitstellungsmanifestdatei. Das Bereitstellungsmanifest wird in einem neuen **config**-Ordner in Ihrem Projektmappenarbeitsbereich erstellt. Sobald Sie über diese Datei verfügen, können Sie die Schritte unter [Bereitstellen von Azure IoT Edge-Modulen über Visual Studio Code](how-to-deploy-modules-vscode.md) oder [Bereitstellen von Azure IoT Edge-Modulen mit der Azure CLI](how-to-deploy-modules-cli.md) durchführen.

## <a name="auto-tiering-and-auto-expiration-properties-and-configuration"></a>Eigenschaften und Konfiguration von automatischem Tiering und automatischem Ablaufen

Verwenden Sie gewünschte Eigenschaften, um Eigenschaften für automatisches Tiering und automatisches Ablaufen festzulegen. Diese können während der Bereitstellung festgelegt oder nachträglich durch Bearbeitung des Modulzwillings ohne erneute Bereitstellung geändert werden. Es empfiehlt sich, den Modulzwilling für `reported configuration` und `configurationValidation` zu überprüfen, um sicherzustellen, dass die Werte korrekt weitergegeben werden.

### <a name="auto-tiering-properties"></a>Eigenschaften für automatisches Tiering 
Der Name dieser Einstellung lautet `tieringSettings`.

| Feld | Mögliche Werte | Erklärung |
| ----- | ----- | ---- |
| tieringOn | true, false | Dieser Wert ist standardmäßig auf `false` festgelegt. Wenn Sie das Feature aktivieren möchten, legen Sie ihn auf `true` fest.|
| backlogPolicy | NewestFirst, OldestFirst | Ermöglicht das Auswählen der Reihenfolge, in der die Daten in Azure kopiert werden. Standardwert: `OldestFirst`. Die Reihenfolge richtet sich nach dem Zeitpunkt der letzten Änderung des Blobs. |
| remoteStorageConnectionString |  | `"DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>"` ist eine Verbindungszeichenfolge, die es ermöglicht, das Azure Storage-Konto anzugeben, in das Ihre Daten hochgeladen werden sollen. Geben Sie `Azure Storage Account Name`, `Azure Storage Account Key`, `End point suffix` an. Fügen Sie ein geeignetes Endpunktsuffix (EndpointSuffix) von Azure für das Ziel des Datenuploads hinzu (variiert für Azure global, Azure Government und Microsoft Azure Stack). |
| tieredContainers | `"<source container name1>": {"target": "<target container name>"}`,<br><br> `"<source container name1>": {"target": "%h-%d-%m-%c"}`, <br><br> `"<source container name1>": {"target": "%d-%c"}` | Ermöglicht das Angeben der Containernamen, die Sie in Azure hochladen möchten. Mit diesem Modul können Sie sowohl Quell- als auch Zielcontainernamen angeben. Falls Sie keinen Zielcontainernamen angeben, wird der Containername automatisch wie folgt zugewiesen: `<IoTHubName>-<IotEdgeDeviceName>-<ModuleName>-<ContainerName>`. Sie können Vorlagenzeichenfolgen für den Zielcontainernamen erstellen. Weitere Informationen finden Sie in der Spalte mit den möglichen Werten. <br>* %h -> IoT Hub-Name (drei bis 50 Zeichen) <br>* %d -> IoT-Geräte-ID (ein bis 129 Zeichen) <br>* %m -> Modulname (ein bis 64 Zeichen) <br>* %c -> Quellcontainername (drei bis 63 Zeichen) <br><br>Der Containername darf maximal 63 Zeichen lang sein. Bei automatischer Zuweisung des Containernamens gilt Folgendes: Ist der Name länger als 63 Zeichen, werden die einzelnen Bestandteile (IoTHubName, IotEdgeDeviceName, ModuleName, ContainerName) jeweils auf 15 Zeichen gekürzt. |

### <a name="auto-expiration-properties"></a>Eigenschaften für automatisches Ablaufen
Der Name dieser Einstellung lautet `ttlSettings`.

| Feld | Mögliche Werte | Erklärung |
| ----- | ----- | ---- |
| ttlOn | true, false | Dieser Wert ist standardmäßig auf `false` festgelegt. Wenn Sie das Feature aktivieren möchten, legen Sie ihn auf `true` fest.|
| timeToLiveInMinutes | `<minutes>` | Geben Sie die Gültigkeitsdauer in Minuten an. Nach Ablauf der Gültigkeitsdauer (TTL) löscht das Modul Ihre Blobs automatisch aus dem lokalen Speicher. |

### <a name="configure-auto-tiering-and-auto-expiration-via-azure-portal"></a>Konfigurieren des automatischen Tierings und des automatischen Ablaufens über das Azure-Portal

Legen Sie die gewünschten Eigenschaften fest, um automatisches Tiering und automatisches Ablaufen zu aktivieren. Verfügbare Werte:

- **Während der ersten Bereitstellung:** Kopieren Sie den JSON-Code in das Feld **Gewünschte Eigenschaften für Modulzwilling festlegen**. Konfigurieren Sie die jeweilige Eigenschaft mit einem entsprechenden Wert, speichern Sie, und fahren Sie mit der Bereitstellung fort.

   ```json
   {
     "properties.desired": {
       "ttlSettings": {
         "ttlOn": <true, false>, 
         "timeToLiveInMinutes": <timeToLiveInMinutes> 
       },
       "tieringSettings": {
         "tieringOn": <true, false>,
         "backlogPolicy": "<NewestFirst, OldestFirst>",
         "remoteStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>",
         "tieredContainers": {
           "<source container name1>": {
             "target": "<target container name1>"
           }
         }
       }
     }
   }

   ```

  ![Festlegen der Eigenschaften für automatisches Tiering und automatisches Ablaufen](./media/how-to-store-data-blob/iotedge_custom_module.png)

- **Nach Bereitstellung des Moduls über das Feature „Zwilling der Modulidentität“**: Navigieren Sie zu „Zwilling der Modulidentität“ dieses Moduls, kopieren Sie den JSON-Code unter den gewünschten Eigenschaften, konfigurieren Sie die jeweilige Eigenschaft mit einem entsprechenden Wert, und speichern Sie. Wenn Sie eine gewünschte Eigenschaft hinzufügen oder aktualisieren, vergewissern Sie sich im JSON-Code unter „Zwilling der Modulidentität“, dass die Änderungen im Abschnitt `reported configuration` berücksichtigt sind und dass im Abschnitt `configurationValidation` eine Erfolgsmeldung für die jeweilige Eigenschaft angezeigt wird.

   ```json 
    "ttlSettings": {
        "ttlOn": <true, false>, 
        "timeToLiveInMinutes": <timeToLiveInMinutes> 
    },
    "tieringSettings": {
        "tieringOn": <true, false>,
        "backlogPolicy": "<NewestFirst, OldestFirst>",
        "remoteStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>",
        "tieredContainers": {
            "<source container name1>": {
                "target": "<target container name1>"
            }
        }
    }

   ```

![tiering+ttl module_identity_twin](./media/how-to-store-data-blob/module_identity_twin.png) 

### <a name="configure-auto-tiering-and-auto-expiration-via-vscode"></a>Konfigurieren des automatischen Tierings und des automatischen Ablaufens über Visual Studio Code

- **Während der ersten Bereitstellung:** Fügen Sie „deployment.template.json“ den folgenden JSON-Code hinzu, um die gewünschten Eigenschaften für dieses Modul zu definieren. Konfigurieren Sie die jeweilige Eigenschaft mit einem entsprechenden Wert, und speichern Sie.

   ```json
   "<your azureblobstorageoniotedge module name>":{
     "properties.desired": {
       "ttlSettings": {
         "ttlOn": <true, false>, 
         "timeToLiveInMinutes": <timeToLiveInMinutes> 
       },
       "tieringSettings": {
         "tieringOn": <true, false>,
         "backlogPolicy": "<NewestFirst, OldestFirst>",
         "remoteStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>",
         "tieredContainers": {
           "<source container name1>": {
             "target": "<target container name1>"
           }
         }
       }
     }
   }

   ```

Im Anschluss sehen Sie ein Beispiel für die gewünschten Eigenschaften für dieses Modul: ![Festlegen gewünschter Eigenschaften für „azureblobstorageoniotedge“ (Visual Studio Code)](./media/how-to-store-data-blob/tiering_ttl.png)

- **Nach Bereitstellung des Moduls über „Modulzwilling“**: [Bearbeiten Sie den Modulzwilling](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin) dieses Moduls, kopieren Sie den JSON-Code unter den gewünschten Eigenschaften, konfigurieren Sie die jeweilige Eigenschaft mit einem entsprechenden Wert, und speichern Sie. Wenn Sie eine gewünschte Eigenschaft hinzufügen oder aktualisieren, vergewissern Sie sich im JSON-Code unter „Modulzwilling“, dass die Änderungen im Abschnitt `reported configuration` berücksichtigt sind und dass im Abschnitt `configurationValidation` eine Erfolgsmeldung für die jeweilige Eigenschaft angezeigt wird.

   ```json 
    "ttlSettings": {
        "ttlOn": <true, false>, 
        "timeToLiveInMinutes": <timeToLiveInMinutes> 
    },
    "tieringSettings": {
        "tieringOn": <true, false>,
        "backlogPolicy": "<NewestFirst, OldestFirst>",
        "remoteStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>",
        "tieredContainers": {
            "<source container name1>": {
                "target": "<target container name1>"
            }
        }
    }

   ```
  ## <a name="logs"></a>Protokolle

Gehen Sie wie [hier](production-checklist.md#set-up-logs-and-diagnostics) beschrieben vor, um Ihre Docker-Protokolle für IoT Edge-Module einzurichten.

## <a name="connect-to-your-blob-storage-module"></a>Herstellen einer Verbindung mit Ihrem Blob Storage-Modul

Sie können mit dem Kontonamen und dem Kontoschlüssel, die Sie für Ihr Modul konfiguriert haben, auf Blob Storage auf Ihrem IoT Edge-Gerät zugreifen. 

Geben Sie Ihr IoT Edge-Gerät als Blobendpunkt für Speicheranforderungen an, die Sie vornehmen. Mithilfe der IoT Edge-Geräteinformationen und dem Kontonamen, den Sie konfiguriert haben, können Sie eine [Verbindungszeichenfolge für einen bestimmten Speicherendpunkt erstellen](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint). 

1. Bei Modulen, die auf dem gleichen Edgegerät bereitgestellt werden, auf dem auch „Azure Blob Storage auf IoT Edge“ ausgeführt wird, lautet der Blobendpunkt wie folgt: `http://<module name>:11002/<account name>`. 
2. Bei Modulen, die nicht auf dem Edgegerät bereitgestellt wurden, auf dem „Azure Blob Storage auf IoT Edge“ ausgeführt wird, lautet der Blobendpunkt abhängig von Ihrem Setup wie folgt: `http://<device IP >:11002/<account name>`, `http://<IoT Edge device hostname>:11002/<account name>` oder `http://<FQDN>:11002/<account name>`.

## <a name="deploy-multiple-instances"></a>Bereitstellen mehrerer Instanzen

Wenn Sie mehrere Instanzen von Azure Blob Storage in IoT Edge bereitstellen möchten, müssen Sie unterschiedliche Speicherpfade angeben und den Hostport ändern, an den das Modul gebunden wird. Die Blob Storage-Module machen immer Port 11002 im Container verfügbar, aber Sie können deklarieren, an welchen Port dieser auf dem Host gebunden wird. 

Bearbeiten Sie die Optionen zur Modulerstellung, um den HostPort-Wert zu ändern:

```json
\"PortBindings\": {\"11002/tcp\": [{\"HostPort\":\"<port number>\"}]}
```

Wenn Sie zusätzliche Blob Storage-Module verbinden, ändern Sie den Endpunkt dahingehend, dass er auf den aktualisierten Hostport zeigt. 

## <a name="try-it-out"></a>Ausprobieren

### <a name="azure-blob-storage-quickstart-samples"></a>Schnellstartbeispiele für Azure Blob Storage
Die Dokumentation zu Azure Blob Storage enthält Schnellstartanleitungen, in denen Beispielcode in mehreren Sprachen bereitgestellt wird. Sie können diese Beispiele zum Testen von Azure Blob Storage in IoT Edge herunterladen, indem Sie den Blobendpunkt dahingehend ändern, dass er auf Ihr Blob Storage-Modul zeigt. Gehen Sie wie [hier](#connect-to-your-blob-storage-module) beschrieben vor, um eine Verbindung mit Ihrem Blob Storage-Modul herzustellen.

Die folgenden Schnellstartanleitungen verwenden Sprachen, die auch von IoT Edge unterstützt werden, sodass Sie sie als IoT Edge-Module zusammen mit dem Blob Storage-Modul bereitstellen können:

* [.NET](../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Java](../storage/blobs/storage-quickstart-blobs-java.md)
* [Python](../storage/blobs/storage-quickstart-blobs-python.md)
* [Node.js](../storage/blobs/storage-quickstart-blobs-nodejs.md) 

### <a name="azure-storage-explorer"></a>Azure Storage-Explorer
Sie können auch den [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) verwenden, um eine Verbindung mit Ihrem lokalen Speicherkonto herzustellen. Wir haben zu Testzwecken die [frühere Version 1.5.0](https://github.com/Microsoft/AzureStorageExplorer/releases/tag/v1.5.0) des Azure-Explorers verwendet.
> [!NOTE]
> Bei den folgenden Schritten treten unter Umständen Fehler auf. Ignorieren Sie diese, und aktualisieren Sie die Ansicht. 

1. Laden Sie den Azure Storage-Explorer herunter, und installieren Sie ihn.
2. Stellen Sie mithilfe einer Verbindungszeichenfolge eine Verbindung mit Azure Storage her.
3. Geben Sie die Verbindungszeichenfolge an: `DefaultEndpointsProtocol=http;BlobEndpoint=http://<host device name>:11002/<your local account name>;AccountName=<your local account name>;AccountKey=<your local account key>;`
4. Führen Sie die Schritte für die Verbindungsherstellung aus.
5. Erstellen Sie einen Container innerhalb Ihres lokalen Speicherkontos.
6. Beginnen Sie damit, Dateien als Blockblobs hochzuladen.
   > [!NOTE]
   > Deaktivieren Sie das Kontrollkästchen für das Hochladen als Seitenblobs. Dieses Modul unterstützt keine Seitenblobs. Diese Aufforderung wird unter anderem beim Hochladen von ISO-, VHD- und VHDX-Dateien sowie beim Hochladen großer Dateien angezeigt.

7. Auf Wunsch können Sie eine Verbindung mit Ihren Azure-Speicherkonten herstellen, in die Sie die Daten hochladen. Dadurch erhalten Sie eine zentrale Ansicht, die sowohl Ihr lokales Speicherkonto als auch Ihr Azure-Speicherkonto enthält.

## <a name="supported-storage-operations"></a>Unterstützte Speichervorgänge

Blob Storage-Module in IoT Edge verwenden die gleichen Azure Storage SDKs und entsprechen der Version „2017-04-17“ der Azure Storage-API für Blockblobendpunkte. Spätere Releases hängen von den Kundenanforderungen ab.

Nicht alle Azure Blob Storage-Vorgänge werden von Azure Blob Storage in IoT Edge unterstützt. Der folgende Abschnitt enthält eine Liste der unterstützten und nicht unterstützten Vorgänge:

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
* Abrufen und Festlegen von Container-ACLs
* Festlegen von Containermetadaten

Nicht unterstützt:
* Abrufen der Lease von Containern

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
* Festlegen von Blocks
* Festlegen und Abrufen von Blocklisten

Nicht unterstützt:
* Festlegen von Blocks über die URL

## <a name="feedback"></a>Feedback:
Ihr Feedback ist uns sehr wichtig und trägt dazu bei, den Nutzen und die Benutzerfreundlichkeit dieses Modul und seiner Features zu verbessern. Übermitteln Sie uns Ihr Feedback, und lassen Sie uns wissen, was wir verbessern können.

Sie erreichen uns unter absiotfeedback@microsoft.com. 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md).

