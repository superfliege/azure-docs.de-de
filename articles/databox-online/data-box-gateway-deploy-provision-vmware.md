---
title: Tutorial zum Bereitstellen von Azure Data Box Gateway in VMware | Microsoft-Dokumentation
description: Im zweiten Tutorial zum Bereitstellen von Azure Data Box Gateway geht es um die Bereitstellung eines virtuellen Geräts in VMware.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 10/01/2018
ms.author: alkohli
ms.openlocfilehash: ea4203c45f482b990122a966fc2ec13b3fb41c84
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167153"
---
# <a name="tutorial-provision-azure-data-box-gateway-in-vmware-preview"></a>Tutorial: Bereitstellen von Azure Data Box Gateway in VMware (Vorschauversion)

## <a name="overview"></a>Übersicht

In diesem Tutorial wird beschrieben, wie Sie ein Data Box Gateway auf einem Hostsystem mit VMware ESXi 6.0 oder 6.5 bereitstellen. 

Sie benötigen Administratorrechte, um ein virtuelles Gerät bereitzustellen und zu verbinden. Die Bereitstellung und die anfängliche Einrichtung dauern ca. 10 Minuten.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Sicherstellen, dass der Host die Mindestanforderungen für Geräte erfüllt
> * Bereitstellen eines virtuellen Geräts in VMware
> * Starten des virtuellen Geräts und Abrufen der IP-Adresse

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

> [!IMPORTANT]
> - Data Box Gateway ist in der Vorschauphase. Lesen Sie die [Azure-Vertragsbedingungen für Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bestellen und bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

Für die Bereitstellung eines virtuellen Geräts auf einem Hostsystem mit VMware ESXi 6.0 oder 6.5 gelten die folgenden Voraussetzungen.

### <a name="for-the-data-box-gateway-resource"></a>Für die Data Box Gateway-Ressource

Stellen Sie Folgendes sicher, bevor Sie beginnen:

* Sie haben alle Schritte unter [Vorbereiten des Portals für Data Box Gateway](data-box-gateway-deploy-prep.md) ausgeführt.
* Sie haben das Image des virtuellen Geräts für VMware wie unter [Vorbereiten des Portals für Data Box Gateway](data-box-gateway-deploy-prep.md) beschrieben aus dem Azure-Portal heruntergeladen.

  > [!IMPORTANT]
  > Die auf dem Data Box Gateway ausgeführte Software kann nur mit der Data Box Gateway-Ressource verwendet werden.

### <a name="for-the-data-box-gateway-virtual-device"></a>Für das virtuelle Data Box Gateway-Gerät

Stellen Sie Folgendes sicher, bevor Sie ein virtuelles Gerät bereitstellen:

* Sie haben Zugriff auf ein Hostsystem mit VMware (ESXi 6.0 oder 6.5), das zum Bereitstellen eines Geräts verwendet werden kann.
* Das Hostsystem verfügt für die Bereitstellung des virtuellen Geräts über die folgenden Ressourcen:

  * Mindestens 4 Kerne
  * Mindestens 8 GB RAM
  * Eine Netzwerkschnittstelle
  * Einen Betriebssystemdatenträger mit 250 GB
  * Einen virtuellen Datenträger mit 2 TB für Systemdaten

### <a name="for-the-network-in-datacenter"></a>Für das Netzwerk im Rechenzentrum

Vorbereitungen

- Lesen Sie die Netzwerkanforderungen für die Bereitstellung eines Data Box Gateway, und konfigurieren Sie das Netzwerk des Datencenters gemäß den Anforderungen. Weitere Informationen finden Sie unter [Data Box Gateway – Netzwerkanforderungen](data-box-gateway-system-requirements.md#networking-requirements).
- Stellen Sie sicher, dass die Internetbandbreite mindestens 20 MBit/s beträgt, um die optimale Nutzung des Geräts zu ermöglichen.

## <a name="check-the-host-system"></a>Überprüfen des Hostsystems

Zum Erstellen eines virtuellen Geräts benötigen Sie Folgendes:

* Zugriff auf ein Hostsystem mit VMware ESXi Server 6.0 oder 6.5. Das Hostsystem muss die folgenden Ressourcen für Ihr virtuelles Gerät reservieren können:
 
  * Mindestens 4 Kerne
  * Mindestens 8 GB RAM 
  * Eine mit dem Netzwerk verbundene Netzwerkschnittstelle, über die Datenverkehr ins Internet weitergeleitet werden kann. 
  * Einen Betriebssystemdatenträger mit 250 GB
  * Einen virtuellen Datenträger mit 2 TB für Daten
* VMware vSphere-Client auf Ihrem System zum Verwalten des ESXi-Hosts


## <a name="provision-a-virtual-device-in-hypervisor"></a>Bereitstellen eines virtuellen Geräts in Hypervisor

Führen Sie die folgenden Schritte aus, um ein virtuelles Gerät im Hypervisor bereitzustellen.

1. Kopieren Sie das Image mit dem virtuellen Gerät auf Ihr System. Dies ist das virtuelle Image (zwei Dateien), das Sie über das Azure-Portal heruntergeladen haben. Notieren Sie sich den Speicherort, an den Sie das Image kopiert haben, da Sie es später noch benötigen.

2. Melden Sie sich mit dem vSphere-Webclient beim ESXi-Server an. Sie benötigen Administratorrechte, um einen virtuellen Computer zu erstellen.

   ![](./media/data-box-gateway-deploy-provision-vmware/image1.png)
  
3. Laden Sie das VMDK auf den ESXi-Server hoch. Klicken Sie im Navigatorbereich auf **Storage** (Speicher).

   ![](./media/data-box-gateway-deploy-provision-vmware/image2.png)

4. Wählen Sie im rechten Bereich unter **Datenspeicher**den Datenspeicher aus, in den Sie das VMDK hochladen möchten. 

    - Der Datenspeicher muss vom Typ VMFS5 sein. 
    - Der Datenspeicher muss zudem über genügend freien Speicherplatz für die Datenträger für das Betriebssystem und die Daten verfügen.
   
5. Klicken Sie mit der rechten Maustaste, und wählen Sie **Datenspeicher durchsuchen** aus.

   ![](./media/data-box-gateway-deploy-provision-vmware/image3.png)

6. Das Fenster **Datenspeicher-Browser** wird angezeigt.

   ![](./media/data-box-gateway-deploy-provision-vmware/image4.png)

7. Klicken Sie auf der Symbolleiste auf **Create directory** (Verzeichnis erstellen), um einen neuen Ordner zu erstellen. Geben Sie den Ordnernamen an, und notieren Sie ihn sich. Sie verwenden diesen Ordnernamen später beim Erstellen einer virtuellen Maschine (empfohlene bewährte Methode). Klicken Sie auf **Create directory** (Verzeichnis erstellen).

   ![](./media/data-box-gateway-deploy-provision-vmware/image5.png)

8. Der neue Ordner wird im linken Bereich des **Datenspeicher-Browsers** angezeigt. Klicken Sie auf das Symbol **Upload**, und wählen Sie **Upload File** (Datei hochladen) aus.

    ![](./media/data-box-gateway-deploy-provision-vmware/image6.png)

9. Rufen Sie das Verzeichnis auf, in das Sie die VMDK-Dateien heruntergeladen haben. Es enthält zwei Dateien. Wählen Sie die Datei aus, die Sie hochladen wollen.

    ![](./media/data-box-gateway-deploy-provision-vmware/image7.png)

10. Klicken Sie auf **Öffnen**. Der Upload der VMDK-Datei in den angegebenen Datenspeicher wird gestartet. Das Hochladen der Datei kann mehrere Minuten dauern.
11. Sobald der Upload abgeschlossen ist, können Sie die Datei im Datenspeicher in dem von Ihnen erstellten Ordner sehen. Laden Sie nun die zweite VMDK-Datei in den gleichen Datenspeicher hoch. Nachdem die beiden Dateien hochgeladen wurden, werden sie in einer einzelnen Datei zusammengeführt. Sie sehen dann nur eine Datei im Verzeichnis.

    ![](./media/data-box-gateway-deploy-provision-vmware/image8.png)

12. Wechseln Sie zurück zum vSphere-Clientfenster. Klicken Sie im Navigatorbereich auf **Virtual Machines** (VMs). Klicken Sie im rechten Bereich auf **Create/Register VM** (VM erstellen/registrieren).

    ![](./media/data-box-gateway-deploy-provision-vmware/image9.png)

13. Ein Fenster **New Virtual Machine** (Neue VM) wird angezeigt. Wählen Sie unter „Select creation type“ (Erstellungstyp auswählen) die Option **Create a new virtual machine** (Neue VM erstellen) aus, und klicken Sie auf **Next** (Weiter).
    ![](./media/data-box-gateway-deploy-provision-vmware/image10.png)

14. Geben Sie auf der Seite **Select a name and guest OS** (Namen und Gastbetriebssystem auswählen) den **Namen** Ihrer VM an. Dieser Name sollte mit dem Ordnernamen übereinstimmen, den Sie in Schritt 7 angegeben haben (empfohlene bewährte Methode). Wählen Sie für **Guest OS family** (Gastbetriebssystemfamilie) Windows und für **Guest OS version** (Gastbetriebssystemversion) Microsoft Windows Server 2016 (64-Bit) aus. Klicken Sie auf **Weiter**.

    ![](./media/data-box-gateway-deploy-provision-vmware/image11.png)

15. Wählen Sie auf der Seite **Select storage** (Speicher auswählen) einen Datenspeicher aus, den Sie zum Bereitstellen der VM verwenden möchten. Klicken Sie auf **Weiter**.

    ![](./media/data-box-gateway-deploy-provision-vmware/image12.png)
16. Legen Sie auf der Seite **Customize settings** (Einstellungen anpassen) **CPU** auf 4, **Memory** (Arbeitsspeicher) auf 8.192 MB (oder mehr) und **Hard disk 1** (Festplatte 1) auf 2 TB (oder mehr) fest. Wählen Sie die **SCSI-Festplatte** aus, die Sie hinzufügen möchten. In diesem Fall wurde „LSI Logic SAS“ ausgewählt. **Statische IDE-Datenträger werden nicht unterstützt.** **Hard disk 1** (Festplatte 1) ist der virtuelle Datenträger. Beachten Sie, dass Sie den Datenträger nach der Bereitstellung nicht mehr verkleinern können.

    ![](./media/data-box-gateway-deploy-provision-vmware/image13.png)

    Klicken Sie auf der gleichen Seite auf **Add hard disk** (Festplatte hinzufügen), und wählen Sie dann **Existing hard disk** (Vorhandene Festplatte) aus. Wählen Sie die VMDK-Datei im Datenspeicher aus. Dadurch wird ein Betriebssystemdatenträger hinzugefügt. 

     ![](./media/data-box-gateway-deploy-provision-vmware/image14.png)

    Scrollen Sie nach unten, bis Sie den Eintrag **New hard disk** (Neue Festplatte) sehen, und erweitern Sie ihn, um die Einstellungen anzuzeigen. Legen Sie **Virtual Device Node** (Virtueller Geräteknoten) auf **IDE controller 0** (IDE-Controller 0) fest. Klicken Sie auf **Weiter**.

     ![](./media/data-box-gateway-deploy-provision-vmware/image15.png)

27. Prüfen Sie auf der Seite **Für Fertigstellung bereit** alle Einstellungen, die der neuen virtuellen Maschine zugeordnet sind. Vergewissern Sie sich, dass für die CPU 4, für den Arbeitsspeicher 8.192 MB, für die Netzwerkschnittstelle 1 und für Festplatte 2 der IDE-Controller 0 angezeigt werden. Klicken Sie auf **Fertig stellen**. 
   
    ![](./media/data-box-gateway-deploy-provision-vmware/image16.png)
    ![](./media/data-box-gateway-deploy-provision-vmware/image17.png)

Ihre virtuelle Maschine wird nun bereitgestellt. Es wird eine Benachrichtigung mit dem Hinweis angezeigt, dass die neue VM der Liste der VMs hinzugefügt wird. 

![](./media/data-box-gateway-deploy-provision-vmware/image17.png)

Der nächste Schritt umfasst das Inbetriebnehmen der Maschine und das Abrufen der IP-Adresse.

> [!NOTE]
> Wir empfehlen, keine VMware-Tools auf dem virtuellen Gerät zu installieren (wie oben beschrieben wird). Die Installation von VMware-Tools führt zu einer nicht unterstützten Konfiguration.

## <a name="start-the-virtual-device-and-get-the-ip"></a>Starten des virtuellen Geräts und Abrufen der IP-Adresse

Führen Sie die folgenden Schritte aus, um Ihr virtuelles Gerät zu starten und eine Verbindung dafür herzustellen.

#### <a name="to-start-the-virtual-device"></a>So starten Sie das virtuelle Gerät
1. Starten Sie das virtuelle Gerät. Wählen Sie im rechten Bereich Ihr Gerät in der Liste der VMs aus, und klicken Sie mit der rechten Maustaste, um das Kontextmenü anzuzeigen. Wählen Sie **Ein/Aus** und dann **Einschalten**. Die virtuelle Maschine sollte nun eingeschaltet werden. Der Status wird im unteren Bereich des Webclients angezeigt.

    ![](./media/data-box-gateway-deploy-provision-vmware/image19.png)

2. Wählen Sie erneut Ihre VM aus. Klicken Sie mit der rechten Maustaste, wählen Sie **Console** (Konsole) und dann **Open in a new window** (In einem neuen Fenster öffnen) aus.

    ![](./media/data-box-gateway-deploy-provision-vmware/image20.png)

3. Die Konsole der VM wird in einem neuen Fenster geöffnet. 

    ![](./media/data-box-gateway-deploy-provision-vmware/image21.png)

4. Sobald das Gerät ausgeführt wird, klicken Sie auf die Registerkarte oben im mittleren Bereich des Konsolenfensters. Klicken Sie auf **Guest OS > Send keys > Ctrl+Alt+Delete** (Gastbetriebssystem > Tasteneingaben senden > STRG+ALT+ENTF). Hierdurch wird die VM entsperrt.

   ![](./media/data-box-gateway-deploy-provision-vmware/image22.png)

5. Geben Sie das Kennwort ein, um sich bei der VM anzumelden. Das Standardkennwort lautet „Password1“.

   ![](./media/data-box-gateway-deploy-provision-vmware/image23.png)

6. Die Schritte 5 bis 7 gelten nur beim Starten in anderen Umgebungen als einer DHCP-Umgebung. Wenn Sie in einer DHCP-Umgebung arbeiten, überspringen Sie diese Schritte, und fahren Sie mit Schritt 8 fort. Falls Sie Ihr Gerät in einer anderen Umgebung als einer DHCP-Umgebung gestartet haben, wird eine entsprechende Meldung angezeigt. **Verwenden Sie das Set-HcsIPAddress-Cmdlet zum Konfigurieren des Netzwerks**. 
   
7. Führen Sie zum Konfigurieren des Netzwerks an der Eingabeaufforderung den Befehl `Get-HcsIpAddress` aus, um die Netzwerkschnittstellen aufzulisten, die auf dem virtuellen Gerät aktiviert sind. Wenn für das Gerät eine einzelne Netzwerkschnittstelle aktiviert ist, wird dieser Schnittstelle der Standardname `Ethernet`zugewiesen.

8. Verwenden Sie das `Set-HcsIpAddress` -Cmdlet, um das Netzwerk zu konfigurieren. Unten ist ein Beispiel angegeben:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

9. Nachdem die anfängliche Einrichtung abgeschlossen und das Gerät gestartet wurde, wird der Bannertext für das Gerät angezeigt. Notieren Sie sich die IP-Adresse und die URL, die im Bannertext für die Verwaltung des Geräts angezeigt wird. Sie verwenden diese IP-Adresse zum Herstellen der Verbindung mit der Webbenutzeroberfläche Ihres virtuellen Geräts und zum Durchführen des lokalen Setups und der Aktivierung.

   ![](./media/data-box-gateway-deploy-provision-vmware/image24.png)

Wenn Ihr Gerät die Mindestanforderungen für die Konfiguration nicht erfüllt, wird im Bannertext ein Fehler angezeigt (siehe unten). Sie müssen die Gerätekonfiguration ändern, damit sie über ausreichende Ressourcen zum Erfüllen der Mindestanforderungen verfügt. Sie können das Gerät dann neu starten und die Verbindung dafür herstellen. Die Mindestanforderungen für die Konfiguration finden Sie unter [Überprüfen des Hostsystems](#check-the-host-system).

<!---If you face any other error during the initial configuration using the local web UI, refer to the following workflows:

* Run diagnostic tests to [troubleshoot web UI setup](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Generate log package and view log files](storsimple-ova-web-ui-admin.md#generate-a-log-package).-->

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie sich mit den folgenden Themen zu Data Box Gateway befasst:

> [!div class="checklist"]
> * Sicherstellen, dass der Host die Mindestanforderungen für Geräte erfüllt
> * Bereitstellen eines virtuellen Geräts in VMware
> * Starten des virtuellen Geräts und Abrufen der IP-Adresse

Im nächsten Tutorial erfahren Sie, wie Sie Ihr virtuelles Gerät verbinden, einrichten und aktivieren.

* [Einrichten und Herstellen einer Verbindung mit Freigaben auf dem Data Box Gateway](data-box-gateway-deploy-connect-setup-activate.md)

