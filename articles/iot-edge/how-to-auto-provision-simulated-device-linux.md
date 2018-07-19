---
title: Automatische Bereitstellung eines Geräts mit Azure IoT Edge unter Verwendung von DPS (Linux) | Microsoft-Dokumentation
description: Verwenden eines simulierten TPM auf einem virtuellen Linux-Computer, um die Gerätebereitstellung für Azure IoT Edge zu testen
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 9609aab6c70bc0c2755de142023bd26e7417987a
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2018
ms.locfileid: "37347703"
---
# <a name="create-and-provision-an-edge-device-with-a-virtual-tpm-on-a-linux-virtual-machine"></a>Erstellen und Bereitstellen eines Edge-Geräts mit einem virtuellen TPM auf einem virtuellen Linux-Computer

Azure IoT Edge-Geräte können genau wie nicht Edge-fähige Geräte mit dem [Device Provisioning-Dienst](../iot-dps/index.yml) automatisch bereitgestellt werden. Wenn Sie mit der automatischen Bereitstellung nicht vertraut sind, lesen Sie die Informationen unter [Konzepte für die automatische Bereitstellung](../iot-dps/concepts-auto-provisioning.md), bevor Sie fortfahren. 

Dieser Artikel beschreibt, wie Sie anhand der folgenden Schritte die automatische Bereitstellung auf einem simulierten Edge-Gerät testen: 

* Erstellen Sie einen virtuellen Linux-Computer (VM) in Hyper-V mit einem simulierten Trusted Platform Module (TPM) für die Hardwaresicherheit.
* Erstellen Sie eine neue Instanz für den IoT Hub Device Provisioning-Dienst (DPS).
* Erstellen Sie eine individuelle Registrierung für das Gerät
* Installieren Sie die IoT Edge-Runtime, und verbinden Sie das Gerät mit einem IoT Hub

Die Schritte in diesem Artikel sind für Testzwecke vorgesehen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Windows-Entwicklungscomputer mit [aktiviertem Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v). In diesem Artikel wird das Betriebssystem Windows 10 verwendet, auf dem ein Ubuntu Server-VM ausgeführt wird. 
* Aktiver IoT Hub. 

## <a name="create-a-linux-virtual-machine-with-a-virtual-tpm"></a>Erstellen eines virtuellen Linux-Computers mit virtuellem TPM

In diesem Abschnitt erstellen Sie einen neuen virtuellen Linux-Computer auf Hyper-V, der über ein simuliertes TPM verfügt, mit dem Sie testen können, wie die automatische Bereitstellung mit IoT Edge funktioniert. 

### <a name="create-a-virtual-switch"></a>Erstellen eines virtuellen Switches

Mit einem virtuellen Switch können Sie Ihren virtuellen Computer mit einem physischen Netzwerk verbinden.

1. Öffnen Sie Hyper-V auf Ihrem Windows-Computer. 

2. Klicken Sie im Menü **Aktionen** auf **Manager für virtuelle Switches**. 

3. Wählen Sie einen **externen** virtuellen Switch, und klicken Sie anschließend auf **Virtuellen Switch erstellen**. 

4. Geben Sie Ihrem neuen virtuellen Switch einen Namen, z.B. **EdgeSwitch**. Stellen Sie sicher, dass der Verbindungstyp auf **Externes Netzwerk**, festgelegt ist, und klicken Sie anschließend auf **OK**.

5. Ein Popup warnt Sie, dass die Netzwerkkonnektivität möglicherweise unterbrochen wird. Wählen Sie **Yes** (Ja), um fortzufahren. 

Wenn beim Erstellen des neuen Switches Fehler angezeigt werden, stellen Sie sicher, dass keine anderen Switches den Ethernet-Adapter verwenden und dass keine anderen Switches den gleichen Namen verwenden. 

### <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

1. Laden Sie eine für Ihren virtuellen Computer zu verwendende Festplatten-Imagedatei herunter, und speichern Sie sie lokal. Beispiel: [Ubuntu-Server](https://www.ubuntu.com/download/server) 

2. Öffnen Sie Hyper-V erneut. Klicken Sie im Menü **Aktionen** auf **Neu** > **Virtueller Computer**.

3. Geben Sie im **Assistenten für neue virtuelle Computer** die folgenden speziellen Konfigurationen ein:

   1. **Generation angeben**: Klicken Sie auf **Generation 2**.
   2. **Konfigurieren der Netzwerkeinstellungen**: Legen Sie den Wert der **Verbindung** auf den virtuellen Switch fest, den Sie im vorherigen Abschnitt erstellt haben. 
   3. **Installationsoptionen**: Klicken Sie auf **Betriebssystem von einer startbaren Imagedatei installieren**, und navigieren Sie zu der Datenträger-Imagedatei, die Sie lokal gespeichert haben.

Die Erstellung des neuen virtuellen Computers kann einige Minuten dauern. 

### <a name="enable-virtual-tpm"></a>Virtuelles TPM aktivieren

1. Nachdem der virtuelle Computer erstellt wurde, öffnen Sie seine Einstellungen. 
2. Navigieren zu **Sicherheit**. 
3. Deaktivieren Sie **Sicheren Start aktivieren**.
4. Aktivieren Sie **Trusted Platform Module aktivieren**. 
5. Klicken Sie auf **OK**.  

### <a name="start-the-virtual-machine-and-collect-tpm-data"></a>Starten des virtuellen Computers und Erfassen von TPM-Daten

Erstellen Sie auf dem virtuellen Computer ein C-SDK-Tool, mit dem Sie die **Registrierungs-ID** und den **Endorsement Key** des Geräts abrufen können. 

1. Starten und verbinden Sie den virtuellen Computer, um den Installationsvorgang abzuschließen. 

2. Führen Sie auf dem virtuellen Computer die Schritte aus [Einrichten einer Linux-Entwicklungsumgebung](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux), um das Azure IoT-Geräte-SDK für C zu installieren und zu erstellen. 

3. Führen Sie die folgenden Befehle aus, um ein C-SDK-Tool zu erstellen, das die Bereitstellungsinformationen Ihres Geräts abruft. 

   ```bash
   cd azure-iot-sdk-c/cmake
   cmake -Duse_prov_client:BOOL=ON ..
   cd provisioning_client/tools/tpm_device_provision
   make
   sudo ./tpm_device_provision
   ```

3. Kopieren Sie die Werte für die **Registrierungs-ID** und den **Endorsement Key**. Sie können diese Werte verwenden, um eine individuelle Registrierung für Ihr Gerät im DPS zu erstellen. 

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>Einrichten des IoT Hub Device Provisioning Service

Erstellen Sie eine neue Instanz des IoT Hub Device Provisioning Service in Azure, und verknüpfen Sie sie mit Ihrem IoT-Hub. Befolgen Sie die Anweisungen unter [Einrichten des IoT Hub Device Provisioning Service](../iot-dps/quick-setup-auto-provision.md).

Nachdem Sie den Device Provisioning-Dienst ausgeführt haben, kopieren Sie den Wert von **ID-Bereich** von der Seite „Übersicht“. Sie können diesen Wert verwenden, wenn Sie IoT Edge-Runtime konfigurieren. 

## <a name="create-a-dps-enrollment"></a>Erstellen einer DPS-Registrierung

Rufen Sie die Bereitstellungsinformationen von Ihrem virtuellen Computer ab, und verwenden Sie diese, um eine individuelle Registrierung im Device Provisioning-Dienst zu erstellen. 

Wenn Sie eine Registrierung im DPS erstellen, haben Sie die Möglichkeit zum Deklarieren von **Anfänglicher Status von Gerätezwilling**. Im Gerätezwilling können Sie Tags zum Gruppieren von Geräten nach jeder beliebigen Metrik, z.B. Region, Umgebung, Speicherort oder Geräte, festlegen, die Sie in Ihrer Projektmappe benötigen. Diese Tags werden zum Erstellen von [automatischen Bereitstellungen](how-to-deploy-monitor.md) verwendet. 


1. Im [Azure-Portal](https://portal.azure.com) navigieren Sie zu Ihrer Instanz des IoT Hub Device Provisioning Service. 

2. Klicken Sie in **Einstellungen** auf **Registrierungen verwalten**. 

3. Klicken Sie auf **Individuelle Registrierung hinzufügen**, und führen Sie dann die folgenden Schritte aus, um die Registrierung zu konfigurieren:  

   1. Klicken Sie unter **Mechanismus** auf die Option **TPM**. 
   2. Fügen Sie den **Endorsement Key** und die **Registrierungs-ID** ein, die Sie von Ihrem virtuellen Computer kopiert haben.
   3. Klicken Sie auf **Aktivieren**, um anzugeben, dass dieser virtuelle Computer ein IoT Edge-Gerät ist. 
   4. Wählen Sie den verknüpften **IoT-Hub**, mit dem Sie Ihr Gerät verbinden möchten. 
   5. Stellen Sie nach Wunsch eine ID für Ihr Gerät bereit. Sie können mithilfe von Geräte-IDs ein einzelnes Gerät als Ziel für die Modulbereitstellung festlegen. 
   6. Fügen Sie nach Wunsche einen Tagwert zu **Anfänglicher Status von Gerätezwilling** hinzu. Sie können mithilfe von Tags Gruppen von Geräten als Ziel für die Modulbereitstellung festlegen. 
   7. Wählen Sie **Speichern**aus. 


## <a name="install-the-iot-edge-runtime"></a>Installieren der IoT Edge-Runtime

Die IoT Edge-Runtime wird auf allen IoT Edge-Geräten bereitgestellt. Die Komponenten werden in Containern ausgeführt, und Sie können weitere Container auf dem Gerät bereitstellen, um Code im Edge-Bereich auszuführen. Installieren Sie die IoT Edge-Runtime auf Ihrem virtuellen Computer. 

Sie müssen Ihren DPS-**ID-Bereich** und die Geräte-**Registrierungs-ID** kennen, bevor Sie mit dem Artikel beginnen, der Ihrem Gerätetyp entspricht. Wenn Sie den Ubuntu-Beispielserver installiert haben, verwenden Sie die **x64**-Anweisungen. Stellen Sie sicher, dass Sie die IoT Edge-Runtime für die automatische und nicht für die manuelle Bereitstellung konfigurieren. 

* [Linux (x64)](how-to-install-iot-edge-linux.md)
* [Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md)

## <a name="give-iot-edge-access-to-the-tpm"></a>Gewähren von IoT Edge-Zugriff für das TPM

Damit die IoT Edge-Runtime Ihr Gerät automatisch bereitstellt, benötigt es Zugriff auf das TPM. 

Erteilen Sie den TPM-Zugriff anhand der folgenden Schritte. Alternativ können Sie den Zugriff erteilen, indem Sie die Systemeinstellungen außer Kraft setzen, sodass der *iotedge*-Dienst als Root ausgeführt werden kann. 

1. Suchen Sie den Pfad zum TPM-Hardwaremodul auf Ihrem Gerät, und speichern Sie ihn als lokale Variable. 

   ```bash
   tpm=$(sudo find /sys -name dev -print | fgrep tpm | sed 's/.\{4\}$//')
   ```

2. Erstellen Sie eine neue Regel, damit die IoT Edge-Runtime Zugriff auf tpm0 erhält. 

   ```bash
   sudo touch /etc/udev/rules.d/tpmaccess.rules
   ```

3. Öffnen Sie die Regeldatei. 

   ```bash
   sudo nano /etc/udev/rules.d/tpmaccess.rules
   ```

4. Kopieren Sie die folgenden Informationen, in die Regeldatei. 

   ```input 
   # allow iotedge access to tpm0
   KERNEL=="tpm0", SUBSYSTEM=="tpm", GROUP="iotedge", MODE="0660"
   ```

5. Speichern und beenden Sie die Datei. 

6. Lösen Sie das Udev-System aus, um die neue Regel ausgewertet. 

   ```bash
   /bin/udevadm trigger $tpm
   ```

7. Vergewissern Sie sich, dass die Regel erfolgreich angewendet wurde.

   ```bash
   ls -l /dev/tpm0
   ```

   Die erfolgreiche Ausgabe sieht wie folgt aus:

   ```output
   crw------- 1 root root 10, 224 Jun 28 22:34 /dev/tpm0
   ```

8. Öffnen Sie die IoT Edge-Runtime-Außerkraftsetzungsdatei. 

   ```bash
   sudo systemctl edit iotedge.service
   ```

9. Fügen Sie den folgenden Code hinzu, um eine TPM-Umgebungsvariable festzulegen.

   ```input
   [Service]
   Environment=IOTEDGE_USE_TPM_DEVICE=ON
   ```

9. Überprüfen Sie, ob die Außerkraftsetzung erfolgreich war.

   ```bash
   sudo systemctl cat iotedge.service
   ```

   Die erfolgreiche Ausgabe zeigt die **Iotedge**-Standarddienstvariablen und anschließend die Umgebungsvariable an, die Sie in **override.conf** festgelegt haben. 

12. Laden Sie die Einstellungen erneut.

   ```bash
   sudo systemctl daemon-reload
   ```

## <a name="restart-the-iot-edge-runtime"></a>Neustart der IoT Edge-Runtime

Starten Sie die IoT Edge-Runtime erneut, damit alle am Gerät vorgenommenen Konfigurationsänderungen erfasst werden. 

   ```bash
   sudo systemctl restart iotedge
   ```

Überprüfen Sie, ob die IoT Edge-Runtime ausgeführt wird. 

   ```bash
   sudo systemctl status iotedge
   ```

Wenn Bereitstellungsfehler angezeigt werden, sind die Konfigurationsänderungen möglicherweise noch nicht wirksam. Starten Sie den IoT Edge-Daemon erneut. 

   ```bash
   sudo systemctl daemon-reload
   ```
   
Oder starten Sie Ihren virtuellen Computer erneut, um festzustellen, ob die Änderungen bei einem sauberen Start wirksam werden. 

## <a name="verify-successful-installation"></a>Bestätigen einer erfolgreichen Installation

Wenn die Runtime erfolgreich gestartet wurde, können Sie zum IoT Hub wechseln und sehen, dass Ihr neues Gerät automatisch bereitgestellt wurde und bereits für die Ausführung von IoT Edge-Modulen ist. 

Überprüfen Sie den Status des IoT Daemons.

```cmd/sh
systemctl status iotedge
```

Untersuchen Sie die Daemonprotokolle.

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

Führen Sie ausgeführte Module auf.

```cmd/sh
iotedge list
```


## <a name="next-steps"></a>Nächste Schritte

Der Registrierungsprozess des Device Provisioning-Diensts ermöglicht Ihnen, die Geräte-ID und die Tags von Gerätezwillingen beim Bereitstellen des neuen Geräts zu sehen. Sie können diese Werte verwenden, um einzelne Geräte oder Gruppen von Geräten über die automatische Geräteverwaltung als Ziel festzulegen. Weitere Informationen finden Sie in [Bedarfsgerechtes Bereitstellen und Überwachen von IoT Edge-Modulen mithilfe des Azure-Portals](how-to-deploy-monitor.md) oder [Verwenden der Azure-Befehlszeilenschnittstelle](how-to-deploy-monitor-cli.md)
