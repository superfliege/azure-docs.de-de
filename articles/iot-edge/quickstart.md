---
title: Schnellstartanleitung für Azure IoT Edge + Windows | Microsoft-Dokumentation
description: Ausprobieren von Azure IoT Edge durch Ausführen von Analysen für ein simuliertes Edgegerät
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/24/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 11b2fccf3c02555f50f48252f2cd9968c9ec90d7
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38632883"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>Schnellstartanleitung: Bereitstellen des ersten IoT Edge-Moduls aus dem Azure-Portal auf einem Windows-Gerät – Vorschau

Bei dieser Schnellstartanleitung verwenden Sie die Azure IoT Edge-Cloudschnittstelle zur Remotebereitstellung von vorgefertigtem Code auf einem IoT Edge-Gerät. Für diese Aufgabe verwenden Sie zunächst Ihr Windows-Gerät, um ein IoT Edge-Gerät zu simulieren. Anschließend können Sie darauf ein Modul bereitstellen.

In dieser Schnellstartanleitung wird Folgendes vermittelt:

1. Erstellen Sie einen IoT Hub.
2. Registrieren eines IoT Edge-Geräts für Ihren IoT Hub
3. Installieren und Starten der IoT Edge-Runtime auf Ihrem Gerät
4. Durchführen der Remotebereitstellung eines Moduls für ein IoT Edge-Gerät und Senden von Telemetriedaten an den IoT Hub

![Aufbau des Tutorials][2]

Das Modul, das Sie in dieser Schnellstartanleitung bereitstellen, ist ein simulierter Sensor, mit dem Daten zu Temperatur, Luftfeuchtigkeit und Luftdruck generiert werden. Die anderen Tutorials zu Azure IoT Edge bauen darauf auf und erläutern die Bereitstellung von Modulen, mit denen die simulierten Daten analysiert werden, um geschäftsrelevante Erkenntnisse zu gewinnen. 

>[!NOTE]
>Die IoT Edge-Runtime unter Windows befindet sich in der [öffentlichen Vorschauphase](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Wenn Sie über kein aktives Azure-Abonnement verfügen, können Sie ein [kostenloses Konto][lnk-account] erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

In dieser Schnellstartanleitung wird Ihr Windows-Computer bzw. virtueller Computer in ein IoT Edge-Gerät verwandelt. Wenn Sie Windows auf einem virtuellen Computer ausführen, aktivieren Sie die [geschachtelte Virtualisierung][lnk-nested], und weisen Sie mindestens 2 GB Speicher zu. 

Der Computer, den Sie für ein IoT Edge-Gerät verwenden, muss folgende Voraussetzungen erfüllen:

1. Stellen Sie sicher, dass Sie eine unterstützte Windows-Version verwenden:
   * Windows 10 oder höher
   * Windows Server 2016 oder höher
2. Installieren Sie [Docker für Windows][lnk-docker], und stellen Sie sicher, dass dieses Programm ausgeführt wird.
3. Konfigurieren Sie Docker für die Verwendung von [Linux-Containern](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Sie verwenden die Azure CLI für viele Schritte in dieser Schnellstartanleitung, und Azure IoT verfügt über eine Erweiterung zur Aktivierung der zusätzlichen Funktionalität. 

Fügen Sie die Azure IoT-Erweiterung der Cloud Shell-Instanz hinzu.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

Beginnen Sie mit der Schnellstartanleitung, indem Sie Ihre IoT Hub-Instanz im Azure-Portal erstellen.
![Erstellen des IoT Hubs][3]

Der kostenlose IoT Hub kann für diesen Schnellstart verwendet werden. Wenn Sie den IoT Hub schon einmal genutzt und bereits einen kostenlosen Hub erstellt haben, können Sie diesen IoT Hub verwenden. Jedes Abonnement kann nur über einen kostenlosen IoT Hub verfügen. 

1. Erstellen Sie in Azure Cloud Shell eine Ressourcengruppe. Mit dem folgenden Code wird eine Ressourcengruppe mit dem Namen **TestResources** in der Region **West US** erstellt. Wenn Sie alle Ressourcen für die Schnellstarts und Tutorials in einer Gruppe anordnen, können Sie sie zusammen verwalten. 

   ```azurecli-interactive
   az group create --name TestResources --location westus
   ```

1. Erstellen Sie einen IoT Hub in Ihrer neuen Ressourcengruppe. Mit dem folgenden Code wird ein kostenloser **F1**-Hub in der Ressourcengruppe **TestResources** erstellt. Ersetzen Sie *{hub_name}* durch einen eindeutigen Namen für Ihren IoT Hub.

   ```azurecli-interactive
   az iot hub create --resource-group TestResources --name {hub_name} --sku F1 
   ```

## <a name="register-an-iot-edge-device"></a>Registrieren eines IoT Edge-Geräts

Registrieren Sie ein IoT Edge-Gerät bei Ihrem neu erstellten IoT Hub.
![Registrieren eines Geräts][4]

Erstellen Sie eine Geräteidentität für das simulierte Gerät, sodass es mit dem IoT Hub kommunizieren kann. Da IoT Edge-Geräte sich von typischen IoT-Geräten unterscheiden und auf unterschiedliche Weise verwaltet werden können, deklarieren Sie dieses Gerät von Anfang an als IoT Edge-Gerät. 

1. Geben Sie in Azure Cloud Shell den folgenden Befehl ein, um in Ihrem Hub ein Gerät mit dem Namen **myEdgeDevice** zu erstellen.

   ```azurecli-interactive
   az iot hub device-identity create --device-id myEdgeDevice --hub-name {hub_name} --edge-enabled
   ```

1. Rufen Sie die Verbindungszeichenfolge für Ihr Gerät ab, über die Ihr physisches Gerät mit seiner Identität auf dem IoT Hub verknüpft wird. 

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

1. Kopieren Sie die Verbindungszeichenfolge, und speichern Sie sie. Sie verwenden diesen Wert, um im nächsten Abschnitt die IoT Edge-Runtime zu konfigurieren. 

## <a name="install-and-start-the-iot-edge-runtime"></a>Installieren und Starten der IoT Edge-Runtime

Installieren und starten Sie die Azure IoT Edge-Runtime auf Ihrem IoT Edge-Gerät. 
![Registrieren eines Geräts][5]

Die IoT Edge-Runtime wird auf allen IoT Edge-Geräten bereitgestellt. Sie besteht aus drei Komponenten. Der **Daemon für die IoT Edge-Sicherheit** wird jedes Mal gestartet, wenn ein Edge-Gerät gestartet wird, und startet den IoT Edge-Agent, um einen Bootstrapvorgang durchzuführen. Der **IoT Edge-Agent** erleichtert die Bereitstellung und Überwachung von Modulen auf dem IoT Edge-Gerät, einschließlich des IoT Edge-Hubs. Der **IoT Edge-Hub** verwaltet die Kommunikation zwischen Modulen auf dem IoT Edge-Gerät sowie zwischen dem Gerät und IoT Hub. 

>[!NOTE]
>Die Installationsschritte in diesem Abschnitt müssen derzeit manuell ausgeführt werden, während ein Installationsskript entwickelt wird. 

In den Anleitungen in diesem Abschnitt wird die IoT Edge-Runtime mit Linux-Containern konfiguriert. Informationen zur Verwendung von Windows-Containern finden Sie unter [Install Azure IoT Edge runtime on Windows to use with Windows containers (Installieren der Azure IoT Edge-Runtime unter Windows zur Verwendung von Windows-Containern)](how-to-install-iot-edge-windows-with-windows.md).

### <a name="download-and-install-the-iot-edge-service"></a>Herunterladen und Installieren des IoT Edge-Diensts

1. Führen Sie PowerShell auf Ihrem IoT Edge-Gerät als Administrator aus.

2. Laden Sie das IoT Edge-Dienstpaket herunter.

  ```powershell
  Invoke-WebRequest https://aka.ms/iotedged-windows-latest -o .\iotedged-windows.zip
  Expand-Archive .\iotedged-windows.zip C:\ProgramData\iotedge -f
  Move-Item c:\ProgramData\iotedge\iotedged-windows\* C:\ProgramData\iotedge\ -Force
  rmdir C:\ProgramData\iotedge\iotedged-windows
  $env:Path += ";C:\ProgramData\iotedge"
  SETX /M PATH "$env:Path"
  ```

3. Installieren Sie die vcruntime.

  ```powershell
  Invoke-WebRequest -useb https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe -o vc_redist.exe
  .\vc_redist.exe /quiet /norestart
  ```

4. Erstellen und starten Sie den IoT Edge-Dienst.

   ```powershell
   New-Service -Name "iotedge" -BinaryPathName "C:\ProgramData\iotedge\iotedged.exe -c C:\ProgramData\iotedge\config.yaml"
   Start-Service iotedge
   ```

5. Fügen Sie Firewallausnahmen für die Ports hinzu, die vom IoT Edge-Dienst verwendet werden.

   ```powershell
   New-NetFirewallRule -DisplayName "iotedged allow inbound 15580,15581" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 15580-15581 -Program "C:\programdata\iotedge\iotedged.exe" -InterfaceType Any
   ```

6. Erstellen Sie die neue Datei **iotedge.reg**, und öffnen Sie sie mit einem Text-Editor. 

7. Fügen Sie den folgenden Inhalt hinzu, und speichern Sie die Datei: 

   ```input
   Windows Registry Editor Version 5.00
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
   "CustomSource"=dword:00000001
   "EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
   "TypesSupported"=dword:00000007
   ```

8. Navigieren Sie im Datei-Explorer zu Ihrer Datei, und doppelklicken Sie darauf, um die Änderungen in die Windows-Registrierung zu importieren. 

### <a name="configure-the-iot-edge-runtime"></a>Konfigurieren der IoT Edge-Laufzeit 

Konfigurieren Sie die Runtime mit der Verbindungszeichenfolge Ihres IoT Edge-Geräts, die Sie beim Registrieren eines neuen Geräts kopiert haben. Konfigurieren Sie anschließend das Netzwerk für die Runtime. 

1. Öffnen Sie die IoT Edge-Konfigurationsdatei, die sich unter `C:\ProgramData\iotedge\config.yaml` befindet. Diese Datei ist geschützt. Führen Sie einen Text-Editor (z.B. den Editor von Windows) als Administrator aus, und öffnen Sie die Datei in diesem Editor. 

2. Suchen Sie nach dem Abschnitt **provisioning**, und aktualisieren Sie den Wert von **device_connection_string** mit der Zeichenfolge, die Sie aus Ihren IoT Edge-Gerätedetails kopiert haben. 

3. Rufen Sie in Ihrem PowerShell-Administratorfenster den Hostnamen für Ihr IoT Edge-Gerät ab, und kopieren Sie die Ausgabe. 

   ```powershell
   hostname
   ```

4. Suchen Sie in der Konfigurationsdatei nach dem Abschnitt **Edge device hostname**. Aktualisieren Sie den Wert von **hostname** mit dem Hostnamen, den Sie aus PowerShell kopiert haben.

3. Rufen Sie in Ihrem PowerShell-Administratorfenster die IP-Adresse für Ihr IoT Edge-Gerät ab. 

   ```powershell
   ipconfig
   ```

4. Kopieren Sie den Wert für die **IPv4-Adresse** im Abschnitt **vEthernet (DockerNAT)** der Ausgabe. 

5. Erstellen Sie eine Umgebungsvariable mit dem Namen **IOTEDGE_HOST**, und ersetzen Sie *\<ip_address\>* durch die IP-Adresse für Ihr IoT Edge-Gerät. 

  ```powershell
  [Environment]::SetEnvironmentVariable("IOTEDGE_HOST", "http://<ip_address>:15580")
  ```
  
  Behalten Sie die Umgebungsvariable nach einem Neustart bei.

  ```powershell
  SETX /M IOTEDGE_HOST "http://<ip_address>:15580"
  ```


6. Suchen Sie in der Datei `config.yaml` nach dem Abschnitt **Connect settings**. Aktualisieren Sie die Werte **management_uri** und **workload_uri** mit Ihrer IP-Adresse und den Ports, die Sie im vorherigen Abschnitt geöffnet haben. Ersetzen Sie **\<GATEWAY_ADDRESS\>** mit Ihrer IP-Adresse. 

   ```yaml
   connect: 
     management_uri: "http://<GATEWAY_ADDRESS>:15580"
     workload_uri: "http://<GATEWAY_ADDRESS>:15581"
   ```

7. Suchen Sie nach dem Abschnitt **Listen settings**, und fügen Sie für **management_uri** und **workload_uri** jeweils die gleichen Werte hinzu. 

   ```yaml
   listen:
     management_uri: "http://<GATEWAY_ADDRESS>:15580"
     workload_uri: "http://<GATEWAY_ADDRESS>:15581"
   ```

8. Suchen Sie nach dem Abschnitt **Moby Container Runtime settings**, und überprüfen Sie, ob der Wert für **network** auf `nat` festgelegt ist.

9. Speichern Sie die Konfigurationsdatei. 

10. Starten Sie den IoT Edge-Dienst in PowerShell neu.

   ```powershell
   Stop-Service iotedge -NoWait
   sleep 5
   Start-Service iotedge
   ```

### <a name="view-the-iot-edge-runtime-status"></a>Anzeigen des Status der IoT Edge-Runtime

Vergewissern Sie sich, dass die Runtime erfolgreich installiert und konfiguriert wurde.

1. Überprüfen Sie den Status des IoT Edge-Diensts.

   ```powershell
   Get-Service iotedge
   ```

2. Sollte eine Problembehandlung für den Dienst erforderlich sein, rufen Sie die Dienstprotokolle ab. 

   ```powershell
   # Displays logs from today, newest at the bottom.

   Get-WinEvent -ea SilentlyContinue `
    -FilterHashtable @{ProviderName= "iotedged";
      LogName = "application"; StartTime = [datetime]::Today} |
    select TimeCreated, Message |
    sort-object @{Expression="TimeCreated";Descending=$false}
   ```

3. Zeigen Sie alle Module an, die auf Ihrem IoT Edge-Gerät ausgeführt werden. Da der Dienst gerade zum ersten Mal gestartet wurde, sollte nur das Modul **edgeAgent** ausgeführt werden. Das Modul „edgeAgent“ wird standardmäßig ausgeführt und unterstützt Sie beim Installieren und Starten von zusätzlichen Modulen, die Sie auf Ihrem Gerät bereitstellen. 

   ```powershell
   iotedge list
   ```

   ![Anzeigen eines Moduls auf Ihrem Gerät](./media/quickstart/iotedge-list-1.png)

## <a name="deploy-a-module"></a>Bereitstellen eines Moduls

Verwalten Sie Ihr Azure IoT Edge-Gerät über die Cloud, um ein Modul bereitzustellen, das Telemetriedaten an die IoT Hub-Instanz sendet.
![Registrieren eines Geräts][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Anzeigen generierter Daten

In diesem Schnellstart haben Sie ein neues IoT Edge-Gerät erstellt und die IoT Edge-Runtime darauf installiert. Anschließend haben Sie das Azure-Portal verwendet, um mit einem Pushvorgang ein IoT Edge-Modul zur Ausführung auf dem Gerät zu übertragen, ohne Änderungen an dem Gerät selbst vornehmen zu müssen. In diesem Fall erstellt das mit einem Pushvorgang übertragene Modul Umgebungsdaten, die Sie für die Tutorials verwenden können. 

Vergewissern Sie sich, dass das über die Cloud bereitgestellte Modul auf dem IoT Edge-Gerät ausgeführt wird. 

```powershell
iotedge list
```

   ![Anzeigen von drei Modulen auf dem Gerät](./media/quickstart/iotedge-list-2.png)

Zeigen Sie die vom tempSensor-Modul an die Cloud gesendeten Nachrichten an. 

```powershell
iotedge logs tempSensor -f
```

  ![Anzeigen der Daten von Ihrem Modul](./media/quickstart/iotedge-logs.png)

Sie können die Nachrichten, die von Ihrer IoT Hub-Instanz empfangen werden, auch anzeigen, indem Sie das [Tool „IoT Hub-Explorer“][lnk-iothub-explorer] oder die [Azure IoT-Toolkit-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) verwenden. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können das simulierte Gerät, das Sie in dieser Schnellstartanleitung konfiguriert haben, zum Testen der IoT Edge-Tutorials verwenden. Gehen Sie wie folgt vor, wenn Sie für das Modul „tempSensor“ das Senden von Daten an Ihre IoT Hub-Instanz beenden möchten: Verwenden Sie den folgenden Befehl, um den IoT Edge-Dienst zu beenden und die auf Ihrem Gerät erstellten Container zu löschen. Denken Sie daran, den Dienst zu starten, wenn Sie Ihren Computer wieder als IoT Edge-Gerät verwenden möchten. 

   ```powershell
   Stop-Service iotedge -NoWait
   docker rm -f $(docker ps -aq)
   ```

Falls Sie die erstellten Azure-Ressourcen nicht mehr benötigen, können Sie den folgenden Befehl verwenden, um die von Ihnen erstellte Ressourcengruppe und alle dazugehörigen Ressourcen zu löschen:

   ```azurecli-interactive
   az group delete --name TestResources
   ```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein neues IoT Edge-Gerät erstellt und die Azure IoT Edge-Cloudschnittstelle zum Bereitstellen von Code auf dem Gerät verwendet. Sie verfügen nun über ein Testgerät, das Rohdaten zu seiner Umgebung generiert. 

Fahren Sie mit einem der anderen Tutorials fort, um zu erfahren, wie Azure IoT Edge Ihnen beim Umwandeln dieser Daten in geschäftliche Erkenntnisse auf Edge-Ebene behilflich sein kann.

> [!div class="nextstepaction"]
> [Filtern von Sensordaten mit einer Azure-Funktion](tutorial-deploy-function.md)


<!-- Images -->
[1]: ./media/quickstart/cloud-shell.png
[2]: ./media/quickstart/install-edge-full.png
[3]: ./media/quickstart/create-iot-hub.png
[4]: ./media/quickstart/register-device.png
[5]: ./media/quickstart/start-runtime.png
[6]: ./media/quickstart/deploy-module.png


<!-- Links -->
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
