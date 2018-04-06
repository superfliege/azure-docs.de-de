---
title: Startdiagnose für virtuelle Linux-Computer in Azure | Microsoft-Dokumentation
description: Übersicht über die beiden Debugfeatures für virtuelle Linux-Computer in Azure
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: Deland-Han
manager: timlt
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
ms.openlocfilehash: bf8e1b338012898ed3de3f443cf492b6890af796
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
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
1. Wählen Sie beim Erstellen eines neuen virtuellen Computers über das Vorschauportal in der Dropdownliste „Bereitstellungsmodell“ die Option **Azure Resource Manager** aus:
 
    ![Ressourcen-Manager](./media/boot-diagnostics/screenshot3.jpg)

2. Konfigurieren Sie die Überwachungsoption für die Auswahl des Speicherkontos, in dem diese Diagnosedateien abgelegt werden sollen.
 
    ![Erstellen eines virtuellen Computers](./media/boot-diagnostics/screenshot4.jpg)

3. Wenn Sie aus einer Azure Resource Manager-Vorlage bereitstellen, navigieren Sie zur Ressource des virtuellen Computers und fügen den Diagnoseprofilabschnitt an. Denken Sie daran, den API-Versionsheader „2015-06-15“ zu verwenden.

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

## <a name="update-an-existing-virtual-machine"></a>Aktualisieren eines vorhandenen virtuellen Computers

Zum Aktivieren der Startdiagnose über das Portal können Sie auch einen vorhandenen virtuellen Computer über das Portal aktualisieren. Wählen Sie die Option „Startdiagnose“, und speichern Sie. Starten Sie den virtuellen Computer neu, damit die Einstellungen übernommen werden.

![Aktualisieren eines vorhandenen virtuellen Computers](./media/boot-diagnostics/screenshot5.png)

## <a name="next-steps"></a>Nächste Schritte

Wenn bei Verwendung der VM-Startdiagnose die Fehlermeldung „Fehler beim Abrufen der Inhalte des Protokolls“ angezeigt wird, lesen Sie [Fehlermeldung „Fehler beim Abrufen der Inhalte des Protokolls“ in VM-Startdiagnose](https://support.microsoft.com/help/4094480/failed-to-get-contents-of-the-log-error-in-vm-boot-diagnostics-in-azur).