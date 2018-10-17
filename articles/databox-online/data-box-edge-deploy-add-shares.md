---
title: Übertragen von Daten mit Azure Data Box Edge | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Freigaben auf einem Data Box Edge-Gerät hinzufügen und eine Verbindung mit diesen Freigaben herstellen.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/08/2018
ms.author: alkohli
ms.custom: ''
Customer intent: As an IT admin, I need to understand how to add and connect to shares on Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: b5c63a255547bae03acbec771593eac97d474003
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2018
ms.locfileid: "48832830"
---
# <a name="tutorial-transfer-data-with-azure-data-box-edge-preview"></a>Tutorial: Übertragen von Daten mit Azure Data Box Edge (Vorschauversion)

In diesem Tutorial erfahren Sie, wie Sie auf dem Data Box Edge-Gerät Freigaben hinzufügen und eine Verbindung mit diesen Freigaben herstellen. Nachdem die Freigaben hinzugefügt wurden, kann das Data Box Edge-Gerät Daten an Azure übertragen.

Dieser Vorgang kann bis zu zehn Minuten dauern. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen einer Freigabe
> * Herstellen einer Verbindung mit einer Freigabe

> [!IMPORTANT]
> Data Box Edge befindet sich in der Vorschauphase. Lesen Sie die [Azure-Vertragsbedingungen für Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bestellen und bereitstellen. 
 
## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich, dass Folgendes erfüllt ist, bevor Sie Ihrem Data Box Edge-Gerät Freigaben hinzufügen:

* Sie haben Ihr physisches Gerät gemäß der Anleitung unter [Installieren von Data Box Edge](data-box-edge-deploy-install.md) installiert. 

    Das physische Gerät ist aktiviert, wie in [Verbinden, Einrichten und Aktivieren von Azure Data Box Edge](data-box-edge-deploy-connect-setup-activate.md) beschrieben. Das Gerät ist für die Erstellung von Freigaben und die Übertragung von Daten bereit.


## <a name="add-a-share"></a>Hinzufügen einer Freigabe

Gehen Sie im [Azure-Portal](https://portal.azure.com/) wie folgt vor, um eine Freigabe zu erstellen:

1. Kehren Sie zum Azure-Portal zurück. Navigieren Sie zu **Alle Ressourcen**, und suchen Sie nach Ihrer Data Box Edge-Ressource.
    
2. Wählen Sie in der gefilterten Ressourcenliste Ihre Data Box Edge-Ressource aus, und navigieren Sie zu **Übersicht**. Klicken Sie auf der Gerätebefehlsleiste auf **+ Freigabe hinzufügen**.
   
   ![Hinzufügen einer Freigabe](./media/data-box-edge-deploy-add-shares/click-add-share.png)

3. Geben Sie unter **Freigabe hinzufügen** die Freigabeeinstellungen an. Geben Sie einen eindeutigen Namen für die Freigabe an. 

   Freigabenamen dürfen nur Zahlen, Kleinbuchstaben und Bindestriche enthalten. Der Name der Freigabe muss 3 bis 63 Zeichen lang sein und mit einem Buchstaben oder einer Zahl beginnen. Vor und nach jedem Bindestrich muss ein Zeichen stehen, das kein Bindestrich ist.
    
    1. Wählen Sie einen **Typ** für die Freigabe aus. Die verfügbaren Typen sind „SMB“ und „NFS“ („SMB“ ist die Standardeinstellung). „SMB“ ist die Standardeinstellung für Windows-Clients, und „NFS“ wird für Linux-Clients verwendet. 

    2. Die angezeigten Optionen für SMB- und NFS-Dateifreigaben unterscheiden sich geringfügig. 

    3. Sie müssen ein Speicherkonto angeben, in dem sich die Freigabe befindet. Im Speicherkonto wird ein Container mit dem Freigabenamen erstellt, sofern der Container noch nicht vorhanden ist. Wenn er bereits vorhanden ist, wird der vorhandene Container verwendet. 
    
    4. Wählen Sie für **Speicherdienst** die Option „Blockblob“, „Seitenblob“ oder „Dateien“ aus. Der ausgewählte Diensttyp hängt von dem Format ab, in dem die Daten in Azure gespeichert werden sollen. In diesem Beispiel sollen die Daten als Blobblöcke in Azure vorliegen. Daher wählen wir „Blockblob“ aus. Bei Verwendung von „Seitenblob“ müssen Ihre Daten ganzzahlige Vielfache von 512 Bytes sein. VHDX-Daten sind beispielsweise immer ganzzahlige Vielfache von 512 Bytes.
   
    5. Dieser Schritt hängt davon ab, ob Sie eine SMB- oder NFS-Freigabe erstellen. 
     
        - **Wenn Sie eine SMB-Freigabe erstellen:** Wählen Sie im Feld „Lokaler Benutzer (alle Berechtigungen)“ die Option **Neu erstellen** oder **Vorhandene verwenden** aus. Wenn Sie einen neuen lokalen Benutzer erstellen, füllen Sie die Felder **Benutzername**, **Kennwort** und **Kennwort bestätigen** aus. Dadurch werden die Berechtigungen dem lokalen Benutzer zugewiesen. Nachdem Sie hier die Berechtigungen zugewiesen haben, können Sie den Datei-Explorer verwenden, um diese Berechtigungen zu ändern.

            Wenn Sie für die Freigabe das Kontrollkästchen **Allow only read operations** (Nur Lesevorgänge zulassen) aktivieren, können Sie Benutzer mit schreibgeschütztem Zugriff angeben.

            ![Hinzufügen einer SMB-Freigabe](./media/data-box-edge-deploy-add-shares/add-share-smb-1.png)
   
        - **Wenn Sie eine NFS-Freigabe erstellen:** Sie müssen die IP-Adressen der zulässigen Clients angeben, die auf die Freigabe zugreifen können.

            ![Hinzufügen einer NFS-Freigabe](./media/data-box-edge-deploy-add-shares/add-share-nfs-1.png)
   
4. Klicken Sie auf **Erstellen**, um die Freigabe zu erstellen. 
    
    Sie werden benachrichtigt, wenn die Freigabe erstellt wird. Nachdem die Freigabe mit den angegebenen Einstellungen erstellt wurde, wird das Blatt **Freigaben** aktualisiert und zeigt die neue Freigabe. 
    
    ![Aktualisierte Liste der Freigaben](./media/data-box-edge-deploy-add-shares/updated-list-of-shares.png) 

## <a name="connect-to-the-share"></a>Herstellen einer Verbindung mit der Freigabe

Nun können Sie eine Verbindung mit den Freigaben herstellen, die Sie im vorherigen Schritt erstellt haben. Die auszuführenden Schritte sind ggf. abhängig von der verwendeten Freigabe (SMB oder NFS). 

### <a name="connect-to-an-smb-share"></a>Herstellen einer Verbindung mit einer SMB-Freigabe

Führen Sie auf dem mit Ihrer Data Box Edge-Ressource verbundenen Windows Server-Client die folgenden Schritte aus, um eine Verbindung mit Freigaben herzustellen:


1. Öffnen Sie ein Befehlsfenster. Geben Sie an der Eingabeaufforderung Folgendes ein:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    Geben Sie das Kennwort für die Freigabe ein, wenn Sie dazu aufgefordert werden. Hier sehen Sie eine Beispielausgabe des Befehls.

    ```powershell
    Microsoft Windows [Version 10.0.16299.192) 
    (c) 2017 microsoft Corporation. All rights reserved . 
    
    C: \Users\DataBoxEdgeUser>net use \\10.10.10.60\newtestuser /u:Tota11yNewUser 
    Enter the password for 'TotallyNewUser' to connect to '10.10.10.60': 
    The command completed successfully. 
    
    C: \Users\DataBoxEdgeUser>
    ```   


2. Drücken Sie WINDOWS-TASTE+R. Geben Sie im Fenster **Ausführen** die `\\<device IP address>` an. Klicken Sie auf **OK**. Der Datei-Explorer wird geöffnet. Die von Ihnen erstellten Freigaben sollten jetzt als Ordner angezeigt werden. Wählen Sie eine Freigabe (einen Ordner) aus, und doppelklicken Sie darauf, um den Inhalt anzuzeigen.
 
    ![Herstellen einer Verbindung mit einer SMB-Freigabe](./media/data-box-edge-deploy-add-shares/connect-to-share2.png)

    Die Daten werden bei ihrer Generierung in diese Freigaben geschrieben, und das Gerät pusht die Daten in die Cloud.

### <a name="connect-to-an-nfs-share"></a>Herstellen einer Verbindung mit einer NFS-Freigabe

Führen Sie auf dem mit Ihrer Data Box Edge-Ressource verbundenen Linux-Client die folgenden Schritte aus:

1. Stellen Sie sicher, dass der NFSv4-Client auf dem Client installiert ist. Verwenden Sie zum Installieren des NFS-Clients den folgenden Befehl:

   `sudo apt-get install nfs-common`

    Weitere Informationen finden Sie unter [Install NFSv4 client](https://help.ubuntu.com/community/SettingUpNFSHowTo#NFSv4_client) (Installieren des NFSv4-Clients).

2. Führen Sie nach der Installation des NFS-Clients den folgenden Befehl aus, um die NFS-Freigabe einzubinden, die Sie auf Ihrem Data Box Edge-Gerät erstellt haben:

   `sudo mount <device IP>:/<NFS share on device> /home/username/<Folder on local Linux computer>`

    Vergewissern Sie sich vor dem Einbinden der Freigaben, dass die Verzeichnisse, die als Bereitstellungspunkte auf Ihrem lokalen Computer fungieren, bereits erstellt wurden. Diese Verzeichnisse dürfen keine Dateien oder Unterordner enthalten.

    Das folgende Beispiel zeigt, wie Sie über NFS eine Verbindung mit einer Freigabe auf Ihrem Data Box Edge-Gerät herstellen. Die Geräte-IP lautet `10.10.10.60`. Die Freigabe `mylinuxshare2` wird in „ubuntuVM“ eingebunden. Der Bereitstellungspunkt der Freigabe ist `/home/databoxubuntuhost/edge`.

    `sudo mount -t nfs 10.10.10.60:/mylinuxshare2 /home/databoxubuntuhost/Edge`

> [!NOTE] 
> Für die Vorschauversion gelten die folgenden Einschränkungen:
> - Das Umbenennen einer Datei, die in den Freigaben erstellt wurde, wird nicht unterstützt. 
> - Durch das Löschen einer Datei aus einer Freigabe wird der Eintrag im Speicherkonto nicht gelöscht.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden unter anderem folgende Data Box Edge-Themen behandelt:

> [!div class="checklist"]
> * Hinzufügen einer Freigabe
> * Herstellen einer Verbindung mit einer Freigabe


Im nächsten Tutorial erfahren Sie, wie Sie Ihre Daten mit Data Box Edge transformieren.

> [!div class="nextstepaction"]
> [Transformieren von Daten mit Data Box Edge](./data-box-edge-deploy-configure-compute.md)


