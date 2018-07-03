---
title: Simulieren von Azure IoT Edge unter Windows | Microsoft-Dokumentation
description: Installieren der Azure IoT Edge-Laufzeit auf einem simulierten Gerät unter Windows und Bereitstellen des ersten Moduls
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 06/08/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 5863a8edbb20b2b0c231834259f1bb7b0423a8f6
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37033801"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>Schnellstartanleitung: Bereitstellen des ersten IoT Edge-Moduls aus dem Azure-Portal auf einem Windows-Gerät – Vorschau

Mit Azure IoT Edge können Sie Analysen und die Datenverarbeitung auf Ihren Geräten durchführen, statt alle Daten per Push in die Cloud zu übertragen. Die IoT Edge-Tutorials veranschaulichen, wie Sie verschiedene Modultypen bereitstellen. Zunächst benötigen Sie aber ein Testgerät. 

In dieser Schnellstartanleitung wird Folgendes vermittelt:

1. Erstellen eines IoT Hubs
2. Registrieren eines IoT Edge-Geräts
3. Starten der IoT Edge-Runtime
4. Bereitstellen eines Moduls

![Aufbau des Tutorials][2]

Das Modul, das Sie in dieser Schnellstartanleitung bereitstellen, ist ein simulierter Sensor, mit dem Daten zu Temperatur, Luftfeuchtigkeit und Luftdruck generiert werden. Die anderen Tutorials zu Azure IoT Edge bauen darauf auf und erläutern die Bereitstellung von Modulen, mit denen die simulierten Daten analysiert werden, um geschäftsrelevante Erkenntnisse zu gewinnen. 

>[!NOTE]
>Die IoT Edge-Runtime unter Windows befindet sich in der [öffentlichen Vorschauphase](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Wenn Sie über kein aktives Azure-Abonnement verfügen, können Sie ein [kostenloses Konto][lnk-account] erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

In dieser Schnellstartanleitung wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer unter Windows verwenden, um ein IoT-Gerät zu simulieren. Wenn Sie Windows auf einem virtuellen Computer ausführen, aktivieren Sie die [geschachtelte Virtualisierung][lnk-nested], und weisen Sie mindestens 2 GB Speicher zu. 

Der Computer, den Sie für ein IoT Edge-Gerät verwenden, muss folgende Voraussetzungen erfüllen:

1. Stellen Sie sicher, dass Sie eine unterstützte Windows-Version verwenden:
   * Windows 10 oder höher
   * Windows Server 2016 oder höher
2. Installieren Sie [Docker für Windows][lnk-docker], und stellen Sie sicher, dass dieses Programm ausgeführt wird.
3. Konfigurieren Sie Docker für die Verwendung von [Linux-Containern](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers).

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

Beginnen Sie mit der Schnellstartanleitung, indem Sie Ihre IoT Hub-Instanz im Azure-Portal erstellen.
![Erstellen des IoT Hubs][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Registrieren eines IoT Edge-Geräts

Registrieren Sie ein IoT Edge-Gerät bei Ihrem neu erstellten IoT Hub.
![Registrieren eines Geräts][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="install-and-start-the-iot-edge-runtime"></a>Installieren und Starten der IoT Edge-Runtime

Installieren und starten Sie die Azure IoT Edge-Runtime auf Ihrem IoT Edge-Gerät. 
![Registrieren eines Geräts][5]

Die IoT Edge-Runtime wird auf allen IoT Edge-Geräten bereitgestellt. Sie besteht aus drei Komponenten: Der **Daemon für die IoT Edge-Sicherheit** wird jedes Mal gestartet, wenn ein Edge-Gerät gestartet wird, und startet den IoT Edge-Agent, um einen Bootstrapvorgang durchzuführen. Der **IoT Edge-Agent** erleichtert die Bereitstellung und Überwachung von Modulen auf dem IoT Edge-Gerät, einschließlich des IoT Edge-Hubs. Der **IoT Edge-Hub** verwaltet die Kommunikation zwischen Modulen auf dem IoT Edge-Gerät sowie zwischen dem Gerät und IoT Hub. 

>[!NOTE]
>Die Installationsschritte in diesem Abschnitt müssen derzeit manuell ausgeführt werden, während ein Installationsskript entwickelt wird. 

In den Anleitungen in diesem Abschnitt wird die IoT Edge-Runtime mit Linux-Containern konfiguriert. Informationen zur Verwendung von Windows-Containern finden Sie unter [Install Azure IoT Edge runtime on Windows to use with Windows containers](how-to-install-iot-edge-windows-with-windows.md) (Installieren der Azure IoT Edge-Runtime unter Windows zur Verwendung von Windows-Containern).

### <a name="download-and-install-the-iot-edge-service"></a>Herunterladen und Installieren des IoT Edge-Diensts

1. Führen Sie PowerShell auf Ihrem IoT Edge-Gerät als Administrator aus.

2. Laden Sie das IoT Edge-Dienstpaket herunter.

   ```powershell
   Invoke-WebRequest https://conteng.blob.core.windows.net/iotedged/iotedge.zip -o .\iotedge.zip
   Expand-Archive .\iotedge.zip C:\ProgramData\iotedge -f
   $env:Path += ";C:\ProgramData\iotedge"
   SETX /M PATH "$env:Path"
   ```

3. Erstellen und starten Sie den IoT Edge-Dienst.

   ```powershell
   New-Service -Name "iotedge" -BinaryPathName "C:\ProgramData\iotedge\iotedged.exe -c C:\ProgramData\iotedge\config.yaml"
   Start-Service iotedge
   ```

4. Fügen Sie Firewallausnahmen für die Ports hinzu, die vom IoT Edge-Dienst verwendet werden.

   ```powershell
   New-NetFirewallRule -DisplayName "iotedged allow inbound 15580,15581" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 15580-15581 -Program "C:\programdata\iotedge\iotedged.exe" -InterfaceType Any
   ```

5. Erstellen Sie die neue Datei **iotedge.reg**, und öffnen Sie sie mit einem Text-Editor. 

6. Fügen Sie den folgenden Inhalt hinzu, und speichern Sie die Datei: 

   ```input
   Windows Registry Editor Version 5.00
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
   "CustomSource"=dword:00000001
   "EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
   "TypesSupported"=dword:00000007
   ```

7. Navigieren Sie im Datei-Explorer zu Ihrer Datei, und doppelklicken Sie darauf, um die Änderungen in die Windows-Registrierung zu importieren. 

### <a name="configure-the-iot-edge-runtime"></a>Konfigurieren der IoT Edge-Laufzeit 

Konfigurieren Sie die Runtime mit der Verbindungszeichenfolge Ihres IoT Edge-Geräts, die Sie beim Registrieren eines neuen Geräts kopiert haben. Konfigurieren Sie anschließend das Netzwerk für die Runtime. 

1. Öffnen Sie die IoT Edge-Konfigurationsdatei, die sich unter `C:\ProgramData\iotedge\config.yaml` befindet. Diese Datei ist geschützt. Führen Sie einen Text-Editor (z.B. den Editor von Windows) als Administrator aus, und öffnen Sie die Datei in diesem Editor. 

2. Suchen Sie nach dem Abschnitt **provisioning**, und aktualisieren Sie den Wert von **device_connection_string** mit der Zeichenfolge, die Sie aus Ihren IoT Edge-Gerätedetails kopiert haben. 

3. Rufen Sie in Ihrem PowerShell-Administratorfenster den Hostnamen für Ihr IoT Edge-Gerät ab, und kopieren Sie die Ausgabe. 

   ```powershell
   hostname
   ```

4. Suchen Sie in der Konfigurationsdatei nach dem Abschnitt **Edge device hostname**. Aktualisieren Sie den Wert von **hostname** mit dem Hostnamen, den Sie aus PowerShell kopiert haben.

5. Rufen Sie in Ihrem PowerShell-Administratorfenster die IP-Adresse für Ihr IoT Edge-Gerät ab. 

   ```powershell
   ipconfig
   ```

6. Kopieren Sie den Wert für die **IPv4-Adresse** im Abschnitt **vEthernet (DockerNAT)** der Ausgabe. 

7. Erstellen Sie eine Umgebungsvariable mit dem Namen **IOTEDGE_HOST**, und ersetzen Sie *\<ip_address\>* durch die IP-Adresse für Ihr IoT Edge-Gerät. 

   ```powershell
   [Environment]::SetEnvironmentVariable("IOTEDGE_HOST", "http://<ip_address>:15580")
   ```

8. Suchen Sie in der Datei `config.yaml` nach dem Abschnitt **Connect settings**. Aktualisieren Sie die Werte **management_uri** und **workload_uri** mit Ihrer IP-Adresse (anstelle von **\<GATEWAY_ADDRESS\>**) und den Ports, die Sie im vorherigen Abschnitt geöffnet haben. 

   ```yaml
   connect: 
     management_uri: "http://<GATEWAY_ADDRESS>:15580"
     workload_uri: "http://<GATEWAY_ADDRESS>:15581"
   ```

9. Suchen Sie nach dem Abschnitt **Listen settings**, und fügen Sie für **management_uri** und **workload_uri** jeweils die gleichen Werte hinzu. 

   ```yaml
   listen:
     management_uri: "http://<GATEWAY_ADDRESS>:15580"
     workload_uri: "http://<GATEWAY_ADDRESS>:15581"
   ```

10. Suchen Sie nach dem Abschnitt **Moby Container Runtime settings**. Heben Sie die Auskommentierung der Zeile **network** auf, und vergewissern Sie sich, dass der Wert auf `nat` festgelegt ist.

   ```yaml
   moby_runtime:
     uri: "npipe://./pipe/docker_engine"
     network: "nat"
   ```

11. Speichern Sie die Konfigurationsdatei. 

12. Starten Sie den IoT Edge-Dienst in PowerShell neu.

   ```powershell
   Stop-Service iotedge
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
  select TimeCreated, Message | Sort-Object -Descending
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

Wenn Sie die erstellte IoT Hub-Instanz nicht mehr benötigen, können Sie über das Azure-Portal die Ressource und alle damit verbundenen Geräte entfernen. Navigieren Sie zur Übersichtsseite Ihrer IoT Hub-Instanz, und klicken Sie auf **Löschen**. 

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein neues IoT Edge-Gerät erstellt und die Azure IoT Edge-Cloudschnittstelle zum Bereitstellen von Code auf dem Gerät verwendet. Sie verfügen nun über ein Testgerät, das Rohdaten zu seiner Umgebung generiert. 

Fahren Sie mit einem der anderen Tutorials fort, um zu erfahren, wie Azure IoT Edge Ihnen beim Umwandeln dieser Daten in geschäftliche Erkenntnisse auf Edge-Ebene behilflich sein kann.

> [!div class="nextstepaction"]
> [Filtern von Sensordaten mit einer Azure-Funktion](tutorial-deploy-function.md)

<!-- Images -->
[2]: ./media/quickstart/install-edge-full.png
[3]: ./media/quickstart/create-iot-hub.png
[4]: ./media/quickstart/register-device.png
[5]: ./media/quickstart/start-runtime.png
[6]: ./media/quickstart/deploy-module.png

<!-- Links -->
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-python]: https://www.python.org/downloads/
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-install-iotcore]: how-to-install-iot-core.md
[lnk-account]: https://azure.microsoft.com/free
