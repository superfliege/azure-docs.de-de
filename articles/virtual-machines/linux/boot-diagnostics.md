---
title: Startdiagnose für virtuelle Linux-Computer in Azure | Microsoft-Dokumentation
description: Übersicht über die beiden Debugfeatures für virtuelle Linux-Computer in Azure
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: Deland-Han
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/19/2018
ms.author: delhan
ms.openlocfilehash: 38cc806cb77af60cda10f3aeac2e5ed13b445b8c
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "33941852"
---
# <a name="how-to-use-boot-diagnostics-to-troubleshoot-linux-virtual-machines-in-azure"></a>Verwenden der Startdiagnose zum Beheben von Problemen mit virtuellen Linux-Computern in Azure

In Azure ist jetzt Unterstützung von zwei Debuggingfunktionen verfügbar: Konsolenausgaben- und Screenshotunterstützung für das Resource Manager-Bereitstellungsmodell für virtuelle Azure-Computer. 

Wenn Sie Ihr eigenes Image in Azure verwenden oder sogar eines der Plattformimages starten, kann sich ein virtueller Computer aus zahlreichen Gründen in einem nicht startfähigen Zustand befinden. Diese Funktionen ermöglichen Ihnen ein einfaches Diagnostizieren und Wiederherstellten Ihrer virtuellen Computer nach Startfehlern.

Für virtuelle Linux-Computer können Sie die Ausgabe des Konsolenprotokolls problemlos über das Portal anzeigen:

![Azure-Portal](./media/boot-diagnostics/screenshot1.png)
 
Azure ermöglicht Ihnen jedoch sowohl für virtuelle Windows- als auch Linux-Computer auch das Anzeigen eines Screenshots des virtuellen Computers im Hypervisor:

![Error](./media/boot-diagnostics/screenshot2.png)

Beide Funktionen werden für virtuelle Azure-Computer in allen Regionen unterstützt. Hinweis: Es kann bis zu 10 Minuten dauern, bis Screenshots und Ausgaben in Ihrem Speicherkonto angezeigt werden.

## <a name="common-boot-errors"></a>Häufige Startfehler

- [Dateisystemprobleme](https://support.microsoft.com/help/3213321/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck) 
- [Kernelprobleme](https://support.microsoft.com/help/4091524/how-recovery-azure-linux-vm-from-kernel-related-boot-related-issues/) 
- [FSTAB-Fehler](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Aktivieren der Diagnose auf einem virtuellen Computer
1. Wählen Sie beim Erstellen eines neuen virtuellen Computers über das Azure-Portal in der Dropdownliste „Bereitstellungsmodell“ die Option **Azure Resource Manager** aus:
 
    ![Ressourcen-Manager](./media/boot-diagnostics/screenshot3.jpg)

2. Aktivieren Sie unter **Einstellungen** die **Startdiagnose**, und wählen Sie dann ein Speicherkonto aus, in dem Sie diese Diagnosedateien ablegen möchten.
 
    ![Erstellen eines virtuellen Computers](./media/boot-diagnostics/create-storage-account.png)

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
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
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

    ![Aktualisieren eines vorhandenen virtuellen Computers](./media/boot-diagnostics/enable-for-existing-vm.png)

3. Starten Sie den virtuellen Computer neu, damit die Einstellungen übernommen werden.

## <a name="next-steps"></a>Nächste Schritte

Wenn bei Verwendung der VM-Startdiagnose die Fehlermeldung „Fehler beim Abrufen der Inhalte des Protokolls“ angezeigt wird, lesen Sie [Fehlermeldung „Fehler beim Abrufen der Inhalte des Protokolls“ in VM-Startdiagnose](https://support.microsoft.com/help/4094480/failed-to-get-contents-of-the-log-error-in-vm-boot-diagnostics-in-azur).