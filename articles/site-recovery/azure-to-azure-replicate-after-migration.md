---
title: Einrichten der Notfallwiederherstellung für Azure-VMs nach Migration in Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: Dieser Artikel beschreibt das Vorbereiten von VMs für die Einrichtung der Notfallwiederherstellung zwischen Azure-Regionen nach der Migration in Azure mithilfe von Azure Site Recovery.
services: site-recovery
author: ponatara
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: ponatara
ms.openlocfilehash: ae212bebf5a34e9a80f49a750735137d7a8814be
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51232663"
---
# <a name="set-up-disaster-recovery-for-azure-vms-after-migration-to-azure"></a>Einrichten der Notfallwiederherstellung für Azure-VMs nach der Migration in Azure 


Verwenden Sie diesen Artikel, nachdem Sie mit dem [Site Recovery](site-recovery-overview.md)-Dienst [lokale Computer in Azure-VMs migriert haben](tutorial-migrate-on-premises-to-azure.md). Dieser Artikel hilft Ihnen bei der Vorbereitung der Azure-VMs für die Einrichtung der Notfallwiederherstellung in einer sekundären Azure-Region mithilfe von Site Recovery.



## <a name="before-you-start"></a>Vorbereitung

Bevor Sie die Notfallwiederherstellung einrichten, stellen Sie sicher, dass die Migration wie erwartet abgeschlossen wurde. Um eine Migration erfolgreich abzuschließen, sollten Sie nach dem Failover die Option **Komplette Migration** für jeden Computer auswählen, den Sie migrieren möchten. 



## <a name="install-the-azure-vm-agent"></a>Installieren des Azure-VM-Agents

Der Azure-[VM-Agent](../virtual-machines/extensions/agent-windows.md) muss auf der VM installiert sein, damit Site Recovery sie replizieren kann.


1. Um den VM-Agent in VMs unter Windows zu installieren, laden Sie das [Agent-Installationsprogramm](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) herunter, und führen Sie es aus. Zum Durchführen der Installation benötigen Sie Administratorberechtigungen für die VM.
2. Um den VM-Agent in VMs mit Linux zu installieren, installieren Sie den neuesten [Linux-Agent](../virtual-machines/extensions/agent-linux.md). Zum Durchführen der Installation benötigen Sie Administratorberechtigungen. Es wird empfohlen, den Agent aus dem Repository Ihrer Distribution zu installieren. Es wird nicht empfohlen, den Linux-VM-Agent direkt von GitHub zu installieren. 


## <a name="validate-the-installation-on-windows-vms"></a>Überprüfen der Installation auf Windows-VMs

1. In der Azure-VM sollte im Ordner „C:\WindowsAzure\Packages“ die Datei „WaAppAgent.exe“ vorhanden sein.
2. Klicken Sie mit der rechten Maustaste auf die Datei, und wählen Sie in **Eigenschaften** die Registerkarte **Details** aus.
3. Im Feld **Produktversion** sollte 2.6.1198.718 oder eine höhere Version angegeben sein.



## <a name="migration-from-vmware-vms-or-physical-servers"></a>Migration von VMware-VMs oder physischen Servern

Wenn Sie lokale VMware-VMs (oder physische Server) in Azure migrieren, beachten Sie Folgendes:

- Sie müssen den Azure-VM-Agent nur dann installieren, wenn der auf dem migrierten Computer installierte Mobility-Dienst die Version 9.6 oder früher hat.
- Bei Windows-VMs mit Version 9.7.0.0.0 des Mobility-Dienstes installiert das Dienstinstallationsprogramm den neuesten verfügbaren Azure-VM-Agent. Bei der Migration erfüllen diese VMs bereits die Installationsvoraussetzungen des Agents für beliebige VM-Erweiterungen, einschließlich Site Recovery-Erweiterung.
- Sie müssen den Mobility-Dienst manuell von der Azure-VM deinstallieren, indem Sie eine der folgenden Methoden verwenden. Bevor Sie die Replikation konfigurieren, starten Sie die VM neu.
    - Unter Windows deinstallieren Sie in „Systemsteuerung“ > **Software**  den Eintrag **Microsoft Azure Site Recovery Mobility Service/Masterzielserver**. Führen Sie an einer Eingabeaufforderung mit erhöhten Rechten Folgendes aus:
        ```
        MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
        ```
    - Melden Sie sich unter Linux als Root-Benutzer an. Wechseln Sie in einem Terminal zu **/user/local/ASR**, und führen Sie den folgenden Befehl aus:
        ```
        uninstall.sh -Y
        ```


## <a name="next-steps"></a>Nächste Schritte

[Schnelles Replizieren](azure-to-azure-quickstart.md) einer Azure-VM in eine sekundäre Region
