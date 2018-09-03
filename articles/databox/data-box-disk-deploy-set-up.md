---
title: Einrichten des Microsoft Azure Data Box-Datenträgers | Microsoft-Dokumentation
description: Verwenden Sie dieses Tutorial, um zu erfahren, wie Sie Ihren Azure Data Box-Datenträger einrichten.
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/28/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 6fcc7823a7e2f2f1e280622a1fa05d4417a71546
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "43143481"
---
# <a name="tutorial-unpack-connect-and-unlock-azure-data-box-disk"></a>Tutorial: Entpacken, Verbinden und Entsperren des Azure Data Box-Datenträgers

In diesem Tutorial wird beschrieben, wie Sie Ihren Azure Data Box-Datenträger entpacken, verbinden und entsperren.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Entpacken Ihres Data Box-Datenträgers
> * Anschließen und Entsperren des Data Box-Datenträgers

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie beginnen:

1. Sie haben [Tutorial: Bestellen eines Azure Data Box-Datenträgers (Vorschau)](data-box-disk-deploy-ordered.md) durchgearbeitet.
2. Sie haben Ihre Datenträger erhalten, und der Auftragsstatus im Portal wurde aktualisiert und in **Übermittelt** geändert.
3. Sie verfügen über einen Hostcomputer, auf dem Sie das Tool zum Entsperren von Data Box-Datenträgern installieren können. Für Ihren Hostcomputer müssen die folgenden Bedingungen erfüllt sein:
    - Es muss ein [unterstütztes Betriebssystem](data-box-disk-system-requirements.md) ausgeführt werden.
    - [Windows PowerShell 4 muss installiert sein](https://www.microsoft.com/download/details.aspx?id=40855).
    - [.NET Framework 4.5.1 muss installiert sein.](https://www.microsoft.com/download/details.aspx?id=30653)
    - [BitLocker muss aktiviert sein.](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
    - [Windows Management Framework 4 muss installiert sein.](https://www.microsoft.com/en-us/download/details.aspx?id=40855) 

## <a name="unpack-your-disks"></a>Auspacken der Datenträger

 Führen Sie die folgenden Schritte aus, um Ihre Datenträger auszupacken.

1. Die Data Box-Datenträger werden in einem kleinen Versandpaket gesendet. Öffnen Sie das Paket, und nehmen Sie den Inhalt heraus. Überprüfen Sie, ob das Paket ein bis fünf SSD-Datenträger (Solid-State Disks) und ein USB-Verbindungskabel pro Datenträger enthält. Überprüfen Sie das Paket auf Zeichen einer Manipulation oder andere erkennbare Beschädigungen. 

    ![Versandpaket für Data Box-Datenträger](media/data-box-disk-deploy-set-up/data-box-disk-ship-package1.png)

2. Öffnen Sie das Versandpaket nicht, falls es manipuliert wurde oder stark beschädigt ist. Wenden Sie sich an den Microsoft-Support, um Hilfe bei der Ermittlung zu erhalten, ob sich die Datenträger in einem einwandfreien Zustand befinden oder ob eine Ersatzsendung erforderlich ist.
3. Prüfen Sie, ob das Paket über eine durchsichtige Kunststoffhülle mit einem Versandetikett für die Rücksendung verfügt (unter dem Etikett für die Zusendung an Sie). Falls dieses Etikett nicht vorhanden oder beschädigt ist, können Sie im Azure-Portal jederzeit ein neues Etikett herunterladen und ausdrucken. 

    ![Versandetikett für Data Box-Datenträger](media/data-box-disk-deploy-set-up/data-box-disk-package-ship-label.png)

4. Heben Sie das Paket und die Schaumteile für den Schutz des Inhalts für die Rücksendung der Datenträger auf.

## <a name="connect-and-unlock-your-disks"></a>Anschließen und Entsperren der Datenträger

Führen Sie die folgenden Schritte aus, um Ihre Datenträger anzuschließen und zu entsperren.

1. Verwenden Sie das beigefügte Kabel, um den Datenträger an einen Windows-Computer anzuschließen, auf dem ein unterstütztes Betriebssystem ausgeführt wird (siehe „Voraussetzungen“). 

    ![Anschließen des Data Box-Datenträgers](media/data-box-disk-deploy-set-up/data-box-disk-connect-unlock.png)    
    
2. Navigieren Sie im Azure-Portal zu **Allgemein > Gerätedetails**. 
3. Klicken Sie auf **Download Data Box Disk unlock tool** (Tool zum Entsperren von Data Box-Datenträgern herunterladen). 

    ![Tool zum Entsperren von Data Box-Datenträgern – Download](media/data-box-disk-deploy-set-up/data-box-disk-unload1.png)     

4. Extrahieren Sie das Tool auf demselben Computer, den Sie zum Kopieren der Daten verwenden möchten.
5. Öffnen Sie ein Eingabeaufforderungsfenster, oder führen Sie Windows PowerShell auf demselben Computer als Administrator aus.
6. (Optional) Um zu überprüfen, ob der zum Entsperren des Datenträgers verwendete Computer die Anforderungen des Betriebssystems erfüllt, können Sie den Befehl für die Systemüberprüfung ausführen. Nachfolgend sehen Sie eine Beispielausgabe. 

    ```powershell
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    
    PS C:\DataBoxDiskUnlockTool\DiskUnlock> .\DataBoxDiskUnlock.exe /SystemCheck
    Successfully verified that the system can run the tool.
    PS C:\DataBoxDiskUnlockTool\DiskUnlock>
    ``` 

7. Navigieren Sie im Azure-Portal zu **Allgemein > Gerätedetails**. Verwenden Sie das Symbol „Kopieren“, um den Hauptschlüssel zu kopieren.
8. Führen Sie `DataBoxDiskUnlock.exe` aus, und geben Sie den Hauptschlüssel an. Der Laufwerkbuchstabe, der dem Datenträger zugewiesen ist, wird angezeigt. Nachfolgend sehen Sie eine Beispielausgabe.

    ```powershell
    PS C:\WINDOWS\system32> cd C:\DataBoxDiskUnlockTool\DiskUnlock
    PS C:\DataBoxDiskUnlockTool\DiskUnlock> .\DataBoxDiskUnlock.exe
    Enter the passkeys (format: passkey1;passkey2;passkey3):
    testpasskey1
    
    Following volumes are unlocked and verified.
    Volume drive letters: D:
    
    PS C:\DataBoxDiskUnlockTool\DiskUnlock>
    ```

9. Wiederholen Sie die Schritte 6 bis 8 für alle zukünftigen Datenträgeraktivierungen dieser Art. Nutzen Sie den Befehl „help“, falls Sie Hilfe zum Tool zum Entsperren von Data Box-Datenträgern benötigen.   

    ```powershell
    PS C:\DataBoxDiskUnlockTool\DiskUnlock> .\DataBoxDiskUnlock.exe /help
    USAGE:
    DataBoxUnlock /PassKeys:<passkey_list_separated_by_semicolon>
    
    Example: DataBoxUnlock /PassKeys:<your passkey>
    Example: DataBoxUnlock /SystemCheck
    Example: DataBoxUnlock /Help
    
    /PassKeys:       Get this passkey from Azure DataBox Disk order. The passkey unlocks your disks.
    /SystemCheck:    This option checks if your system meets the requirements to run the tool.
    /Help:           This option provides help on cmdlet usage and examples.
    
    PS C:\DataBoxDiskUnlockTool\DiskUnlock>
    ```  
10. Nachdem der Datenträger entsperrt wurde, können Sie den Inhalt des Datenträgers anzeigen.    

    ![Inhalt des Data Box-Datenträgers](media/data-box-disk-deploy-set-up/data-box-disk-content.png) 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zu Azure Data Box-Datenträgern erhalten, z.B.:

> [!div class="checklist"]
> * Entpacken Ihres Data Box-Datenträgers
> * Anschließen und Entsperren von Data Box-Datenträgern


Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Daten auf Ihren Data Box-Datenträger kopieren.

> [!div class="nextstepaction"]
> [Kopieren von Daten auf Ihren Data Box-Datenträger](./data-box-disk-deploy-copy-data.md)

