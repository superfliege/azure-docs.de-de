---
title: Tutorial zum Vorbereiten des Azure-Portals für die Bereitstellung von Data Box Gateway | Microsoft-Dokumentation
description: Im ersten Tutorial zum Bereitstellen von Azure Data Box Gateway geht es um die Vorbereitung des Azure-Portals.
services: databox-edge-gateway
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: b8aac8a6a666bf3f360ef2867ff15933ad72dbd7
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166983"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-gateway-preview"></a>Tutorial: Vorbereiten der Bereitstellung von Azure Data Box Gateway (Vorschauversion)


Dies ist das erste Tutorial einer Reihe von Bereitstellungstutorials, die zum vollständigen Bereitstellen Ihres Azure Data Box Gateway erforderlich sind. In diesem Tutorial wird beschrieben, wie Sie das Azure-Portal für die Bereitstellung Ihrer Data Box Gateway-Ressource vorbereiten. 

Sie benötigen Administratorrechte, um die Einrichtung und Konfiguration abzuschließen. Die Vorbereitung des Portals erfordert weniger als 10 Minuten.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Neue Ressource erstellen
> * Herunterladen des Images des virtuellen Geräts
> * Abrufen des Aktivierungsschlüssels

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


> [!IMPORTANT]
> - Data Box Gateway ist in der Vorschauphase. Lesen Sie die [Azure-Vertragsbedingungen für Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bestellen und bereitstellen. 

### <a name="get-started"></a>Erste Schritte

Führen Sie zum Bereitstellen Ihres Data Box Gateway die folgenden Tutorials in der angegebenen Reihenfolge aus.

| **#** | **Schritt** | **Dokumente** |
| --- | --- | --- | 
| 1. |**[Vorbereiten des Azure-Portals für Data Box Gateway](data-box-gateway-deploy-prep.md)** |Erstellen und konfigurieren Sie die Data Box Gateway-Ressource, bevor Sie ein virtuelles Data Box Gateway-Gerät bereitstellen. |
| 2. |**[Bereitstellen des Data Box Gateway in Hyper-V](data-box-gateway-deploy-provision-hyperv.md)** <br><br><br>**[Bereitstellen des Data Box Gateway in VMware](data-box-gateway-deploy-provision-vmware.md)**|Stellen Sie für Hyper-V ein virtuelles Data Box Gateway-Gerät auf einem Hostsystem mit Hyper-V unter Windows Server 2016 oder Windows Server 2012 R2 bereit, und stellen Sie eine Verbindung mit dem Gerät her. <br><br><br> Stellen Sie für VMware ein virtuelles Data Box Gateway-Gerät auf einem Hostsystem mit VMware ESXi 6.0 oder 6.5 bereit, und stellen Sie eine Verbindung mit dem Gerät her.<br></br> |
| 3. |**[Verbinden, Einrichten und Aktivieren des Data Box Gateway](data-box-gateway-deploy-connect-setup-activate.md)** |Stellen Sie eine Verbindung mit der lokalen Webbenutzeroberfläche her, führen Sie die Einrichtung durch, und aktivieren Sie das Gerät. Anschließend können Sie die SMB-Freigaben bereitstellen.  |
| 4. |**[Übertragen von Daten mit Data Box Gateway](data-box-gateway-deploy-add-shares.md)** |Fügen Sie Freigaben hinzu, und stellen Sie über SMB oder NFS eine Verbindung mit Freigaben her. |

Sie können jetzt mit der Einrichtung des Azure-Portals beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Im Folgenden finden Sie die Konfigurationsvoraussetzungen für die Data Box Gateway-Ressource, das Data Box Gateway-Gerät und das Netzwerk des Datencenters.

### <a name="for-the-data-box-gateway-resource"></a>Für die Data Box Gateway-Ressource

Stellen Sie Folgendes sicher, bevor Sie beginnen:

* Ihr Microsoft Azure-Abonnement ist für die Data Box Gateway-Ressource aktiviert.
* Sie besitzen ein Microsoft Azure-Speicherkonto mit Anmeldeinformationen für den Zugriff.

### <a name="for-the-data-box-gateway-device"></a>Für das Data Box Gateway-Gerät

Stellen Sie Folgendes sicher, bevor Sie ein virtuelles Gerät bereitstellen:

* Sie haben Zugriff auf ein Hostsystem mit Hyper-V unter Windows Server 2012 R2 oder höher oder VMware (ESXi 6.0 oder 6.5), das zum Bereitstellen eines Geräts verwendet werden kann.
* Das Hostsystem verfügt für die Bereitstellung des virtuellen Data Box-Geräts über die folgenden Ressourcen:
  
  * Mindestens 4 Kerne
  * Mindestens 8 GB RAM 
  * Eine Netzwerkschnittstelle
  * Einen Betriebssystemdatenträger mit 250 GB
  * Einen virtuellen Datenträger mit 2 TB für Systemdaten

### <a name="for-the-datacenter-network"></a>Für das Netzwerk des Rechenzentrums

Stellen Sie Folgendes sicher, bevor Sie beginnen:

* Das Netzwerk in Ihrem Datencenter ist gemäß den Netzwerkanforderungen für das Data Box Gateway-Gerät konfiguriert. Weitere Informationen finden Sie unter [Data Box Gateway – Systemanforderungen](data-box-gateway-system-requirements.md).

* Für Ihr Data Box Gateway steht jederzeit eine feste Internetbandbreite von 20 MBit/s (oder mehr) zur Verfügung. Diese Bandbreite sollte nicht gemeinsam mit anderen Anwendungen genutzt werden. Wenn die Einschränkung der maximalen Bandbreite aktiviert ist, empfehlen wir die Verwendung einer Internetbandbreite von mindestens 32 MBit/s, damit die Einschränkung funktioniert.

## <a name="create-a-new-resource"></a>Neue Ressource erstellen

Führen Sie die folgenden Schritte aus, um eine neue Data Box Gateway-Ressource zu erstellen. 

Falls Sie eine vorhandene Data Box Gateway-Ressource zum Verwalten Ihrer virtuellen Geräte haben, überspringen Sie diesen Schritt, und fahren Sie mit [Abrufen des Aktivierungsschlüssels](#get-the-activation-key) fort.

Führen Sie die folgenden Schritte im Azure-Portal aus, um eine Data Box-Ressource zu erstellen.
1. Melden Sie sich mit Ihren Microsoft Azure-Anmeldeinformationen beim Azure-Vorschauportal unter der folgenden URL an: [https://aka.ms/databox-edge](https://aka.ms/databox-edge). 

2. Wählen Sie das Abonnement aus, das Sie für die Vorschauversion von Data Box Edge verwenden möchten. Wählen Sie die Region aus, in der Sie die Data Box Edge-Ressource bereitstellen möchten. Klicken Sie im Bereich für die Option **Data Box Gateway** auf **Erstellen**.

    ![Suchen nach dem Data Box Gateway-Dienst](media/data-box-gateway-deploy-prep/data-box-gateway-edge-sku.png)

3. Geben Sie die folgenden Informationen für die neue Ressource ein, oder wählen Sie sie aus.
    
    |Einstellung  |Wert  |
    |---------|---------|
    |Ressourcenname   | Ein Anzeigename, der die Ressource identifiziert.<br>Der Name muss 2 bis 50 Zeichen lang sein und darf nur Buchstaben, Zahlen und Bindestriche enthalten.<br> Der Name muss mit einem Buchstaben oder einer Zahl beginnen und enden.        |
    |Abonnement    |Das Abonnement ist mit Ihrem Abrechnungskonto verknüpft. |
    |Ressourcengruppe  |Wählen Sie eine vorhandene Gruppe aus, oder erstellen Sie eine neue Gruppe.<br>Erfahren Sie mehr über [Azure-Ressourcengruppen](../azure-resource-manager/resource-group-overview.md).     |
    |Standort     |In diesem Release sind „USA, Osten“, „USA, Westen 2“, „Asien, Südosten“ und „Europa, Westen“ verfügbar. <br> Wählen Sie den Standort aus, der der geografischen Region, in der Ihr Gerät bereitgestellt werden soll, am nächsten ist.|
    
    ![Erstellen der Data Box Gateway-Ressource](media/data-box-gateway-deploy-prep/data-box-gateway-resource.png)
    
4. Klicken Sie auf **OK**.
 
Die Erstellung der Ressource dauert einige Minuten. Nachdem die Ressource erfolgreich erstellt wurde, werden Sie entsprechend benachrichtigt.


## <a name="download-the-virtual-device-image"></a>Herunterladen des Images des virtuellen Geräts

Nachdem die Data Box Gateway-Ressource erstellt wurde, laden Sie das entsprechende Image des virtuellen Geräts herunter, um auf Ihrem Hostsystem ein virtuelles Gerät bereitzustellen. Die Images virtueller Geräte sind betriebssystemspezifisch und können im Azure-Portal auf dem Blatt **Schnellstart** Ihrer Ressource heruntergeladen werden.

> [!IMPORTANT]
> Die auf dem Data Box Gateway ausgeführte Software kann nur mit der Data Box Gateway-Ressource verwendet werden.


Führen Sie die folgenden Schritte im [Azure-Portal](https://portal.azure.com/) aus.

1. Klicken Sie auf die Ressource, die Sie erstellt haben, und dann auf **Übersicht**. Falls Sie eine vorhandene Azure Data Box Gateway-Ressource haben, klicken Sie auf die Ressource, und wechseln Sie zu **Übersicht**.

    ![Neue Data Box Gateway-Ressource](media/data-box-gateway-deploy-prep/data-box-gateway-resource-created.png)

4. Klicken Sie in der Schnellstartanleitung auf der rechten Seite auf den Link für das Image, das Sie herunterladen möchten. Die Imagedateien sind ca. 4,8 GB groß.
   
   * [VHDX für Hyper-V unter Windows Server 2012 R2 und höher](https://aka.ms/dbe-vhdx-2012).
   * [VMDK für VMWare ESXi 6.0 oder 6.5](https://aka.ms/dbe-vmdk).

5. Laden Sie die Datei auf einen lokalen Datenträger herunter, und entzippen Sie sie. Notieren Sie den Speicherort der entzippten Datei.


## <a name="get-the-activation-key"></a>Abrufen des Aktivierungsschlüssels

Wenn die Data Box Gateway-Ressource betriebsbereit ist, müssen Sie den Aktivierungsschlüssel abrufen. Dieser Schlüssel wird verwendet, um Ihr Data Box Gateway-Gerät zu aktivieren und mit der Ressource zu verbinden.

Der Aktivierungsschlüssel wird zum Registrieren aller Data Box Gateway-Geräte verwendet, die mit Ihrer Data Box Gateway-Ressource aktiviert werden müssen. Sie können diesen Schlüssel jetzt abrufen, während Sie sich im Azure-Portal befinden.

1. Klicken Sie auf die Ressource, die Sie erstellt haben, und dann auf **Übersicht**.

    ![Neue Data Box Gateway-Ressource](media/data-box-gateway-deploy-prep/data-box-gateway-resource-created.png)

2. Klicken Sie auf **Schlüssel generieren**, um einen Aktivierungsschlüssel zu erstellen. Klicken Sie auf das Kopiersymbol, um den Schlüssel zu kopieren und zur späteren Verwendung zu speichern.

    ![Abrufen des Aktivierungsschlüssels](media/data-box-gateway-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - Der Aktivierungsschlüssel läuft drei Tage nach der Generierung ab. 
> - Generieren Sie einen neuen Schlüssel, falls der Schlüssel abgelaufen ist. Der ältere Schlüssel ist nicht mehr gültig.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie sich mit den folgenden Themen zu Data Box Gateway befasst:

> [!div class="checklist"]
> * Neue Ressource erstellen
> * Herunterladen des Images des virtuellen Geräts
> * Abrufen des Aktivierungsschlüssels

Im nächsten Tutorial erfahren Sie, wie Sie eine VM für Ihr Data Box Gateway bereitstellen. Sehen Sie sich je nach Hostbetriebssystem die folgenden ausführlichen Anweisungen an:

> [!div class="nextstepaction"]
> [Bereitstellen eines Data Box Gateway in Hyper-V](./data-box-gateway-deploy-provision-hyperv.md)

OR

> [!div class="nextstepaction"]
> [Bereitstellen eines Data Box Gateway in VMware](./data-box-gateway-deploy-provision-vmware.md)


