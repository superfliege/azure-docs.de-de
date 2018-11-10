---
title: Startdiagnose für virtuelle Computer in Azure | Microsoft-Dokumentation
description: Übersicht über die beiden Debugfeatures für virtuelle Computer in Azure
services: virtual-machines
author: Deland-Han
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: delhan
ms.openlocfilehash: 9341458336e4c95b84590eadbc86073e7dbf09a0
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50419553"
---
# <a name="how-to-use-boot-diagnostics-to-troubleshoot-virtual-machines-in-azure"></a>Es wird beschrieben, wie Sie die Startdiagnose zum Beheben von Problemen mit virtuellen Computern in Azure verwenden.

Zwei Debuggingfunktionen werden ab sofort in Azure unterstützt: die Konsolenausgabe und die Screenshotunterstützung für das Bereitstellungsmodell des Ressourcen-Managers von Azure Virtual Machines. 

Wenn Sie Ihr eigenes Image in Azure verwenden oder sogar eines der Plattformimages starten, kann ein virtueller Computer aus zahlreichen Gründen in einen nicht startbaren Zustand geraten. Diese Funktionen ermöglichen Ihnen ein einfaches Diagnostizieren und Wiederherstellten Ihrer virtuellen Computer nach Startfehlern.

Für virtuelle Linux-Computer können Sie die Ausgabe des Konsolenprotokolls problemlos über das Portal anzeigen. Azure ermöglicht Ihnen sowohl für virtuelle Windows- als auch Linux-Computer auch das Anzeigen eines Screenshots des virtuellen Computers im Hypervisor. Beide Funktionen werden für virtuelle Azure-Computer in allen Regionen unterstützt. Hinweis: Es kann bis zu 10 Minuten dauern, bis Screenshots und Ausgaben in Ihrem Speicherkonto angezeigt werden.

Sie können die Option **Startdiagnose** aktivieren, um das Protokoll und den Screenshot anzuzeigen.

![Ressourcen-Manager](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)

## <a name="common-boot-errors"></a>Häufige Startfehler

- [0xC000000E](https://support.microsoft.com/help/4010129)
- [0xC000000F](https://support.microsoft.com/help/4010130)
- [0xC0000011](https://support.microsoft.com/help/4010134)
- [0xC0000034](https://support.microsoft.com/help/4010140)
- [0xC0000098](https://support.microsoft.com/help/4010137)
- [0xC00000BA](https://support.microsoft.com/help/4010136)
- [0xC000014C](https://support.microsoft.com/help/4010141)
- [0xC0000221](https://support.microsoft.com/help/4010132)
- [0xC0000225](https://support.microsoft.com/help/4010138)
- [0xC0000359](https://support.microsoft.com/help/4010135)
- [0xC0000605](https://support.microsoft.com/help/4010131)
- [Betriebssystem nicht gefunden](https://support.microsoft.com/help/4010142)
- [Startfehler oder INACCESSIBLE_BOOT_DEVICE](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Aktivieren der Diagnose auf einem virtuellen Computer
1. Wählen Sie beim Erstellen eines neuen virtuellen Computers über das Azure-Portal in der Dropdownliste „Bereitstellungsmodell“ die Option **Azure Resource Manager** aus:
 
    ![Ressourcen-Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. Aktivieren Sie unter **Einstellungen** die **Startdiagnose**, und wählen Sie dann ein Speicherkonto aus, in dem Sie diese Diagnosedateien ablegen möchten.
 
    ![Erstellen eines virtuellen Computers](./media/virtual-machines-common-boot-diagnostics/create-storage-account.png)

    > [!NOTE]
    > Das Feature „Startdiagnose“ unterstützt keine Storage Premium-Konten. Wenn Sie ein Premium-Speicherkontos für die Startdiagnose verwenden, erhalten Sie beim Starten der VM möglicherweise den Fehler „StorageAccountTypeNotSupported“.
    >
    > 

3. Beim Bereitstellen aus einer Azure Resource Manager-Vorlage navigieren Sie zur VM-Ressource und fügen den Diagnoseprofilabschnitt an. Denken Sie daran, den API-Versionsheader „2015-06-15“ zu verwenden.

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. Das Diagnoseprofil ermöglicht Ihnen die Auswahl des Speicherkontos, in dem diese Protokolle abgelegt werden sollen.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('https://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

Informationen zum Bereitstellen eines virtuellen Beispielcomputers mit aktivierter Startdiagnose finden Sie in unserem Repository.

## <a name="enable-boot-diagnostics-on-existing-virtual-machine"></a>Aktivieren der Startdiagnose auf vorhandenen virtuellen Computern 

Zum Aktivieren von Startdiagnoseeinstellungen auf vorhandenen virtuellen Computern führen Sie diese Schritte aus:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und wählen Sie dann den virtuellen Computer aus.
2. Wählen Sie unter **Support + Problembehandlung** die Option **Startdiagnose** > **Einstellungen** aus, ändern Sie den Status in **Ein**, und wählen Sie dann ein Speicherkonto aus. 
4. Stellen Sie sicher, dass die Option „Startdiagnose“ ausgewählt ist, und speichern Sie dann die Änderung.

    ![Aktualisieren eines vorhandenen virtuellen Computers](./media/virtual-machines-common-boot-diagnostics/enable-for-existing-vm.png)

3. Starten Sie den virtuellen Computer neu, damit die Einstellungen übernommen werden.


