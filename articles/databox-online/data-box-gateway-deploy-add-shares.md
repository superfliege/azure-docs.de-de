---
title: Übertragen von Daten mit Azure Data Box Gateway | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Freigaben auf einem Data Box Gateway-Gerät hinzufügen und eine Verbindung mit diesen Freigaben herstellen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 6220a794c10f43d562155adbad910694a233ec88
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166020"
---
# <a name="tutorial-transfer-data-with-azure-data-box-gateway-preview"></a>Tutorial: Übertragen von Daten mit Azure Data Box Gateway (Vorschauversion)


## <a name="introduction"></a>Einführung

In diesem Artikel wird beschrieben, wie Sie auf dem Data Box Gateway Freigaben hinzufügen und eine Verbindung mit diesen Freigaben herstellen. Nachdem die Freigaben hinzugefügt wurden, kann das Data Box Gateway-Gerät Daten in Azure übertragen.

Dieser Vorgang kann bis zu zehn Minuten dauern. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen einer Freigabe
> * Herstellen einer Verbindung mit einer Freigabe

> [!IMPORTANT]
> - Data Box Gateway ist in der Vorschauphase. Lesen Sie die [Azure-Vertragsbedingungen für Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bestellen und bereitstellen. 
 
## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie Ihrem Data Box Gateway Freigaben hinzufügen:

* Sie haben ein virtuelles Gerät bereitgestellt und wie unter [Bereitstellen eines Data Box Gateway in Hyper-V](data-box-gateway-deploy-provision-hyperv.md) bzw. [Bereitstellen eines Data Box Gateway in VMware](data-box-gateway-deploy-provision-vmware.md) beschrieben eine Verbindung mit dem Gerät hergestellt. 

    Das virtuelle Gerät wurde wie unter [Verbinden und Aktivieren des Azure Data Box Gateway](data-box-gateway-deploy-connect-setup-activate.md) beschrieben aktiviert und ist zum Erstellen von Freigaben und Übertragen von Daten bereit.


## <a name="add-a-share"></a>Hinzufügen einer Freigabe

Führen Sie die folgenden Schritte im [Azure-Portal](https://portal.azure.com/) aus, um eine Freigabe zu erstellen.

1. Kehren Sie zum Azure-Portal zurück. Wechseln Sie zu **Alle Ressourcen**, und suchen Sie nach Ihrer Data Box Gateway-Ressource.
    
2. Wählen Sie in der gefilterten Ressourcenliste Ihre Data Box Gateway-Ressource aus, und navigieren Sie dann zu **Übersicht**. Klicken Sie auf der Gerätebefehlsleiste auf **+ Freigabe hinzufügen**.
   
   ![Hinzufügen einer Freigabe](./media/data-box-gateway-deploy-add-shares/click-add-share.png)

4. Geben Sie unter **Freigabe hinzufügen** die Freigabeeinstellungen an. Geben Sie einen eindeutigen Namen für die Freigabe an. 

   Freigabenamen dürfen nur Zahlen, Kleinbuchstaben und Bindestriche enthalten. Der Name der Freigabe muss 3 bis 63 Zeichen lang sein und mit einem Buchstaben oder einer Zahl beginnen. Vor und nach jedem Bindestrich muss ein Zeichen stehen, das kein Bindestrich ist.
    
5. Wählen Sie einen **Typ** für die Freigabe aus. Die verfügbaren Typen sind „SMB“ und „NFS“ („SMB“ ist die Standardeinstellung). „SMB“ ist die Standardeinstellung für Windows-Clients, und „NFS“ wird für Linux-Clients verwendet. Die angezeigten Optionen für SMB- und NFS-Dateifreigaben unterscheiden sich geringfügig. 

6. Sie müssen ein Speicherkonto angeben, in dem sich die Freigabe befindet. Im Speicherkonto wird ein Container mit dem Freigabenamen erstellt, sofern er noch nicht vorhanden ist. Wenn er bereits vorhanden ist, wird der vorhandene Container verwendet. 
    
7. Wählen Sie für **Speicherdienst** die Option „Blockblob“, „Seitenblob“ oder „Dateien“ aus. Der ausgewählte Diensttyp hängt von dem Format ab, in dem die Daten in Azure gespeichert werden sollen. In diesem Fall sollen die Daten beispielsweise als Blobblöcke in Azure gespeichert werden. Daher wählen wir „Blockblob“ aus. Wenn Sie „Seitenblob“ auswählen, müssen Sie sicherstellen, dass Ihre Daten ganzzahlige Vielfache von 512 Bytes sind. Beachten Sie, dass es sich bei VHDX-Daten immer um ganzzahlige Vielfache von 512 Bytes handelt.
   
8. Dieser Schritt hängt davon ab, ob Sie eine SMB- oder NFS-Freigabe erstellen. 
     
    - **Wenn Sie eine SMB-Freigabe erstellen:** Wählen Sie im Feld „Lokaler Benutzer (alle Berechtigungen)“ die Option **Neu erstellen** oder **Vorhandene verwenden** aus. Wenn Sie einen neuen lokalen Benutzer erstellen, füllen Sie die Felder **Benutzername**, **Kennwort** und **Kennwort bestätigen** aus. Dadurch werden die Berechtigungen dem lokalen Benutzer zugewiesen. Nachdem Sie hier die Berechtigungen zugewiesen haben, können Sie den Datei-Explorer verwenden, um diese Berechtigungen zu ändern.
    
        ![Hinzufügen einer SMB-Freigabe](./media/data-box-gateway-deploy-add-shares/add-share-smb-1.png)
        
        Wenn Sie für die Freigabe das Kontrollkästchen **Allow only read operations** (Nur Lesevorgänge zulassen) aktivieren, können Sie Benutzer mit schreibgeschütztem Zugriff angeben.
        
    - **Wenn Sie eine NFS-Freigabe erstellen:** Sie müssen die IP-Adressen der zulässigen Clients angeben, die auf die Freigabe zugreifen können.

        ![Hinzufügen einer NFS-Freigabe](./media/data-box-gateway-deploy-add-shares/add-share-nfs-1.png)
   
9. Klicken Sie auf **Erstellen**, um die Freigabe zu erstellen. 
    
    Sie werden benachrichtigt, wenn die Freigabe erstellt wird. Nachdem die Freigabe mit den angegebenen Einstellungen erstellt wurde, wird das Blatt **Freigaben** aktualisiert und zeigt die neue Freigabe. 
    
    ![Aktualisierte Liste der Freigaben](./media/data-box-gateway-deploy-add-shares/updated-list-of-shares.png) 

## <a name="connect-to-the-share"></a>Herstellen einer Verbindung mit der Freigabe

Führen Sie die folgenden Schritte auf dem mit Ihrem Data Box Gateway verbundenen Windows Server-Client aus, um eine Verbindung mit den Freigaben herzustellen.


1. Öffnen Sie ein Befehlsfenster. Geben Sie an der Eingabeaufforderung Folgendes ein:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    Geben Sie das Kennwort für die Freigabe ein, wenn Sie dazu aufgefordert werden. Hier sehen Sie eine Beispielausgabe des Befehls.

    ```powershell
    Microsoft Windows [Version 18.8.16299.192) 
    (c) 2817 microsoft Corporation. All rights reserved . 
    
    C: \Users\GatewayUser>net use \\10.10.10.60\newtestuser /u:Tota11yNewUser 
    Enter the password for 'TotallyNewUser' to connect to '10.10.10.60' • 
    The command completed successfully. 
    
    C: \Users\GatewayUser>
    ```   


2. Drücken Sie WINDOWS-TASTE+R. Geben Sie im Fenster **Ausführen** die `\\<device IP address>` an. Klicken Sie auf **OK**. Der Datei-Explorer wird geöffnet. Die von Ihnen erstellten Freigaben sollten jetzt als Ordner angezeigt werden. Wählen Sie eine Freigabe (einen Ordner) aus, und doppelklicken Sie darauf, um den Inhalt anzuzeigen.
 
    ![Herstellen einer Verbindung mit einer SMB-Freigabe](./media/data-box-gateway-deploy-add-shares/connect-to-share2.png)-->

    Die Daten werden bei ihrer Generierung in diese Freigaben geschrieben, und das Gerät pusht die Daten in die Cloud.

### <a name="connect-to-an-nfs-share"></a>Herstellen einer Verbindung mit einer NFS-Freigabe

Führen Sie die folgenden Schritte auf dem mit Ihrem Data Box Edge verbundenen Linux-Client aus.

1. Stellen Sie sicher, dass der NFSv4-Client auf dem Client installiert ist. Verwenden Sie zum Installieren des NFS-Clients den folgenden Befehl:

   `sudo apt-get install nfs-common`

    Weitere Informationen finden Sie unter [Install NFSv4 client](https://help.ubuntu.com/community/SettingUpNFSHowTo#NFSv4_client) (Installieren des NFSv4-Clients).

2. Führen Sie nach der Installation des NFS-Clients den folgenden Befehl aus, um die NFS-Freigabe einzubinden, die Sie auf Ihrem Data Box Gateway-Gerät erstellt haben:

   `sudo mount <device IP>:/<NFS share on device> /home/username/<Folder on local Linux computer>`

    Vergewissern Sie sich vor dem Einrichten der Bereitstellungen, dass die Verzeichnisse, die als Bereitstellungspunkte auf dem lokalen Computer fungieren, bereits erstellt wurden und keine Dateien oder Unterordner enthalten.

    Das folgende Beispiel zeigt, wie Sie über NFS eine Verbindung mit einer Freigabe auf dem Gateway-Gerät herstellen. Die IP-Adresse des virtuellen Geräts lautet `10.10.10.60`, die Freigabe `mylinuxshare2` wird auf der Ubuntu-VM eingebunden, und der Bereitstellungspunkt ist `/home/databoxubuntuhost/gateway`.

    `sudo mount -t nfs 10.10.10.60:/mylinuxshare2 /home/databoxubuntuhost/gateway`

> [!NOTE] 
> Für die Vorschauversion gelten die folgenden Einschränkungen:
> - Das Umbenennen einer Datei, die in den Freigaben erstellt wurde, wird nicht unterstützt. 
> - Durch das Löschen einer Datei aus einer Freigabe wird der Eintrag im Speicherkonto nicht gelöscht.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie sich mit den folgenden Themen zu Data Box Gateway befasst:

> [!div class="checklist"]
> * Hinzufügen einer Freigabe
> * Herstellen einer Verbindung mit einer Freigabe


Im nächsten Tutorial erfahren Sie, wie Sie Ihr Data Box Gateway verwalten.

> [!div class="nextstepaction"]
> [Verwenden der lokalen Webbenutzeroberfläche zum Verwalten eines Data Box Gateway](http://aka.ms/dbg-docs)


