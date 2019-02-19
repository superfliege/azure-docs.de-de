---
title: Tutorial zum Bereitstellen von Azure Data Box Gateway in Hyper-V | Microsoft-Dokumentation
description: Im zweiten Tutorial zum Bereitstellen von Azure Data Box Gateway geht es um die Bereitstellung eines virtuellen Geräts in Hyper-V.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 09/26/2018
ms.author: alkohli
ms.openlocfilehash: cf47919ead890f0ad0e89646dde26276ebfb1127
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56109740"
---
# <a name="tutorial-provision-azure-data-box-gateway-in-hyper-v-preview"></a>Tutorial: Bereitstellen von Azure Data Box Gateway in Hyper-V (Vorschauversion)

## <a name="overview"></a>Übersicht

In diesem Tutorial erfahren Sie, wie Sie ein Data Box Gateway auf einem Hostsystem mit Hyper-V unter Windows Server 2016, Windows Server 2012 R2 oder Windows Server 2012 bereitstellen. 

Sie benötigen Administratorrechte, um ein virtuelles Gerät bereitzustellen und zu konfigurieren. Die Bereitstellung und die anfängliche Einrichtung dauern ca. 10 Minuten.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Sicherstellen, dass der Host die Mindestanforderungen für Geräte erfüllt
> * Bereitstellen eines virtuellen Geräts in Hypervisor
> * Starten des virtuellen Geräts und Abrufen der IP-Adresse

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

> [!IMPORTANT]
> - Data Box Gateway ist in der Vorschauphase. Lesen Sie die [Azure-Vertragsbedingungen für Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bestellen und bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

Für die Bereitstellung eines virtuellen Geräts auf einem Hostsystem mit Hyper-V unter Windows Server 2016 oder Windows Server 2012 R2 gelten die folgenden Voraussetzungen.

### <a name="for-the-data-box-gateway-resource"></a>Für die Data Box Gateway-Ressource

Stellen Sie Folgendes sicher, bevor Sie beginnen:

* Sie haben alle Schritte unter [Vorbereiten des Portals für Data Box Gateway](data-box-gateway-deploy-prep.md) ausgeführt.
* Sie haben das Image des virtuellen Geräts für Hyper-V wie unter [Vorbereiten des Portals für Data Box Gateway](data-box-gateway-deploy-prep.md) beschrieben aus dem Azure-Portal heruntergeladen.

  > [!IMPORTANT]
  > Die auf dem Data Box Gateway ausgeführte Software kann nur mit der Data Box Gateway-Ressource verwendet werden.
 
### <a name="for-the-data-box-gateway-virtual-device"></a>Für das virtuelle Data Box Gateway-Gerät

Stellen Sie Folgendes sicher, bevor Sie ein Gerät bereitstellen:

* Sie haben Zugriff auf ein Hostsystem mit Hyper-V unter Windows Server 2012 R2 oder höher, das zum Bereitstellen eines Geräts verwendet werden kann.
* Das Hostsystem verfügt für die Bereitstellung des virtuellen Geräts über die folgenden Ressourcen:

  * Mindestens 4 Kerne
  * Mindestens 8 GB RAM 
  * Eine Netzwerkschnittstelle
  * Einen Betriebssystemdatenträger mit 250 GB
  * Einen virtuellen Datenträger mit 2 TB für Daten

### <a name="for-the-network-in-the-datacenter"></a>Für das Netzwerk im Datencenter

Vorbereitungen

- Lesen Sie die Netzwerkanforderungen für die Bereitstellung eines Data Box Gateway, und konfigurieren Sie das Netzwerk des Datencenters gemäß den Anforderungen. Weitere Informationen finden Sie unter [Data Box Gateway – Netzwerkanforderungen](data-box-gateway-system-requirements.md#networking-port-requirements).
- Stellen Sie sicher, dass die Internetbandbreite mindestens 20 MBit/s beträgt, um die optimale Nutzung des Geräts zu ermöglichen.


## <a name="check-the-host-system"></a>Überprüfen des Hostsystems

Zum Erstellen eines virtuellen Geräts benötigen Sie Folgendes:

* Die unter Windows Server 2016, Windows Server 2012 R2 oder Windows Server 2012 installierte Hyper-V-Rolle
* Microsoft Hyper-V Manager auf einem Microsoft Windows-Client mit einer Verbindung mit dem Host
* Stellen Sie sicher, dass die zugrunde liegende Hardware (Hostsystem), auf der Sie das virtuelle Gerät erstellen, die folgenden Ressourcen für Ihr virtuelles Gerät reservieren kann:

    * Mindestens 4 Kerne
    * Mindestens 8 GB RAM
    * Eine mit dem Netzwerk verbundene Netzwerkschnittstelle, über die Datenverkehr ins Internet weitergeleitet werden kann. 
    * Einen Betriebssystemdatenträger mit 250 GB
    * Einen virtuellen Datenträger mit 2 TB für Systemdaten

## <a name="provision-a-virtual-device-in-hypervisor"></a>Bereitstellen eines virtuellen Geräts in VMware

Führen Sie die folgenden Schritte aus, um ein Gerät im Hypervisor bereitzustellen.

1. Kopieren Sie das Image des virtuellen Geräts auf den lokalen Datenträger Ihres Windows Server-Hosts. Dies ist das VHDX-Image, das Sie über das Azure-Portal heruntergeladen haben. Notieren Sie sich den Speicherort, an den Sie das Image kopiert haben, da Sie es später noch benötigen.
2. Öffnen Sie den **Server-Manager**. Klicken Sie in der oberen rechten Ecke auf **Extras**, und wählen Sie **Hyper-V-Manager**.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image1.png)  
  
3. Klicken Sie im **Hyper-V-Manager** unter „Bereich“ mit der rechten Maustaste auf Ihren Systemknoten, um das Kontextmenü zu öffnen. Klicken Sie dann auf **Neu** > **Virtueller Computer**.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image2.png)
4. Klicken Sie auf der Seite **Vorbereitung** des Assistenten für neue virtuelle Computer auf **Weiter**.
5. Geben Sie auf der Seite **Namen und Speicherort angeben** einen **Namen** für Ihr virtuelles Gerät an. Klicken Sie auf **Weiter**.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image3.png)
6. Wählen Sie auf der Seite **Generation angeben** die Option **Generation 2** für den VHDX-Imagetyp des Geräts aus, und klicken Sie anschließend auf **Weiter**.    

   ![](./media/data-box-gateway-deploy-provision-hyperv/image4.png)
7. Geben Sie auf der Seite **Speicher zuweisen** einen **Startspeicher** von mindestens **8192 MB** an, aktivieren Sie nicht den dynamischen Arbeitsspeicher, und klicken Sie auf **Weiter**.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image5.png) 
8. Geben Sie auf der Seite **Netzwerk konfigurieren** den virtuellen Switch an, der mit dem Internet verbunden ist, und klicken Sie auf **Weiter**.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image6.png)
9. Wählen Sie auf der Seite **Virtuelle Festplatte verbinden** die Option **Vorhandene virtuelle Festplatte verwenden** aus, geben Sie den Speicherort des Images des virtuellen Geräts an, und klicken Sie auf **Weiter**.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image7.png)
10. Überprüfen Sie die **Zusammenfassung**, und klicken Sie dann auf **Fertig stellen**, um den virtuellen Computer zu erstellen.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image8.png)
11. Sie benötigen vier Kerne, um die Mindestanforderungen zu erfüllen. Um vier virtuelle Prozessoren hinzuzufügen, wählen Sie im Fenster **Hyper-V-Manager** Ihr Hostsystem aus. Suchen Sie im rechten Bereich unter der Liste **Virtuelle Computer** den virtuellen Computer, den Sie gerade erstellt haben. Klicken Sie mit der rechten Maustaste auf den ausgewählten Namen der Maschine, und wählen Sie **Einstellungen**.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image9.png)
12. Klicken Sie auf der Seite **Einstellungen** im linken Bereich auf **Prozessor**. Legen Sie im rechten Bereich die **Anzahl virtueller Prozessoren** auf 4 (oder mehr) fest. Klicken Sie auf **Anwenden**.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image10.png)
13. Um die Mindestanforderungen zu erfüllen, müssen Sie auch einen virtuellen Datenträger mit 2 TB hinzufügen. Auf der Seite **Einstellungen** :

    1. Wählen Sie im linken Bereich die Option **SCSI-Controller**.
    2. Wählen Sie im rechten Bereich die Option **Festplatte**, und klicken Sie auf **Hinzufügen**.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image11.png)
14. Wählen Sie auf der Seite **Festplatte** die Option **Virtuelle Festplatte**, und klicken Sie auf **Neu**. Der **Assistent für neue virtuelle Festplatten** wird gestartet.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image12.png)
1. Klicken Sie auf der Seite **Vorbereitung** des Assistenten für neue virtuelle Festplatten auf **Weiter**.
2. Übernehmen Sie auf der Seite **Datenträgerformat auswählen** die Standardoption **VHDX** für das Format. Klicken Sie auf **Weiter**.
   
17. Legen Sie auf der Seite **Datenträgertyp auswählen** den Datenträgertyp für die virtuelle Festplatte auf **Dynamisch erweiterbar** fest (empfohlen). Sie können auch die Option **Feste Größe** für den Datenträger auswählen, aber dies ist unter Umständen mit einer langen Wartezeit verbunden. Die Verwendung der Option **Differenzierend** ist nicht zu empfehlen. Klicken Sie auf **Weiter**. 

    ![](./media/data-box-gateway-deploy-provision-hyperv/image13.png)
18. Geben Sie auf der Seite **Namen und Speicherort angeben** einen **Namen** und einen **Speicherort** (z.B. per Durchsuchen) für den Datenträger an. Klicken Sie auf **Weiter**.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image14.png)
19. Wählen Sie auf der Seite **Datenträger konfigurieren** die Option **Neue virtuelle Festplatte ohne Inhalt erstellen** aus, und geben Sie als Größe **2 TB** (oder mehr) an. 2 TB sind die Mindestanforderung, Sie können aber auch einen größeren Datenträger bereitstellen. Beachten Sie, dass Sie den Datenträger nach der Bereitstellung nicht mehr verkleinern können.  Sie können den Datenträger jedoch erweitern, indem Sie einen Datenträger für Daten hinzufügen. Klicken Sie auf **Weiter**.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image15.png)
20. Überprüfen Sie auf der Seite **Zusammenfassung** die Details Ihres virtuellen Datenträgers, und klicken Sie zum Erstellen des Datenträgers auf **Fertig stellen**, wenn alles korrekt ist. Der Assistent wird geschlossen, und Ihrem Computer wird eine virtuelle Festplatte hinzugefügt.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image16.png)
21. Wechseln Sie zurück zur Seite **Einstellungen**. Klicken Sie auf **OK**, um die Seite **Einstellungen** zu schließen und zum Hyper-V-Manager-Fenster zurückzukehren.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image17.png)

## <a name="start-the-virtual-device-and-get-the-ip"></a>Starten des virtuellen Geräts und Abrufen der IP-Adresse
Führen Sie die folgenden Schritte aus, um Ihr virtuelles Gerät zu starten und eine Verbindung dafür herzustellen.

#### <a name="to-start-the-virtual-device"></a>So starten Sie das virtuelle Gerät
1. Starten Sie das virtuelle Gerät.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image18.png)
2. Wenn das Gerät ausgeführt wird, wählen Sie das Gerät aus, klicken Sie mit der rechten Maustaste, und wählen Sie **Verbinden**.

3. Unter Umständen müssen Sie 10 bis 15 Minuten warten, bis das Gerät bereit ist. In der Konsole wird eine Statusmeldung angezeigt, die den Fortschritt angibt. Wenn das Gerät bereit ist, wählen Sie **Aktion**. Drücken Sie `Ctrl + Alt + Delete`, um sich beim virtuellen Gerät anzumelden. Der Standardbenutzer ist *EdgeUser*, und das Standardkennwort lautet *Password1*.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image21.png)
   
6. Die Schritte 5 bis 7 gelten nur beim Starten in anderen Umgebungen als einer DHCP-Umgebung. Wenn Sie in einer DHCP-Umgebung arbeiten, können Sie diese Schritte überspringen. Wenn Sie Ihr Gerät in einer anderen Umgebung als einer DHCP-Umgebung gestartet haben, wird eine entsprechende Meldung angezeigt.
    
7. Verwenden Sie zum Konfigurieren des Netzwerks den Befehl `Get-HcsIpAddress`, um die Netzwerkschnittstellen aufzulisten, die auf dem virtuellen Gerät aktiviert sind. Wenn für das Gerät eine einzelne Netzwerkschnittstelle aktiviert ist, wird dieser Schnittstelle der Standardname `Ethernet`zugewiesen.

8. Verwenden Sie das `Set-HcsIpAddress` -Cmdlet, um das Netzwerk zu konfigurieren. Siehe folgendes Beispiel:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`
    
9. Nachdem die anfängliche Einrichtung abgeschlossen und das Gerät gestartet wurde, wird der Bannertext für das Gerät angezeigt. Notieren Sie sich die IP-Adresse und die URL, die im Bannertext für die Verwaltung des Geräts angezeigt wird. Sie verwenden diese IP-Adresse zum Herstellen der Verbindung mit der Webbenutzeroberfläche Ihres virtuellen Geräts und zum Durchführen des lokalen Setups und der Aktivierung.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image23.png)
      

Wenn Ihr Gerät die Mindestanforderungen für die Konfiguration nicht erfüllt, wird im Bannertext ein Fehler angezeigt. Sie müssen die Gerätekonfiguration ändern, damit der Computer über ausreichende Ressourcen verfügt und die Mindestanforderungen erfüllen kann. Sie können das Gerät dann neu starten und die Verbindung dafür herstellen. Die Mindestanforderungen für die Konfiguration finden Sie unter [Überprüfen des Hostsystems](#check-the-host-system).

<!--If you face any other error during the initial configuration using the local web UI, refer to the following workflows:

* Run diagnostic tests to [troubleshoot web UI setup](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Generate log package and view log files](storsimple-ova-web-ui-admin.md#generate-a-log-package).-->

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie sich mit den folgenden Themen zu Data Box Gateway befasst:

> [!div class="checklist"]
> * Sicherstellen, dass der Host die Mindestanforderungen für Geräte erfüllt
> * Bereitstellen eines virtuellen Geräts in Hypervisor
> * Starten des virtuellen Geräts und Abrufen der IP-Adresse

Im nächsten Tutorial erfahren Sie, wie Sie Ihr virtuelles Gerät verbinden, einrichten und aktivieren.

> [!div class="nextstepaction"]
> [Verbinden und Einrichten des Data Box Gateway](./data-box-gateway-deploy-connect-setup-activate.md)


