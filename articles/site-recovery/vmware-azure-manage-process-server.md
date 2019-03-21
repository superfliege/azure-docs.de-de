---
title: Verwalten eines Prozessservers für die Notfallwiederherstellung von VMware-VMs und physischen Servern in Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie einen Prozessserver für die Notfallwiederherstellung von VMware-VMs und physischen Servern in Azure mithilfe von Azure Site Recovery verwalten.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: ramamill
ms.openlocfilehash: ba80c8ce57495eaa46e915cb0c472eb4aabcee57
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57863626"
---
# <a name="manage-process-servers"></a>Verwalten von Prozessservern

Standardmäßig wird der Prozessserver, der bei der Replikation von VMware-VMs oder physischen Servern nach Azure verwendet wird, auf dem lokalen Computer des Konfigurationsservers installiert. Es gibt eine Reihe von Instanzen, in denen Sie einen separaten Prozessserver einrichten müssen:

- Bei umfangreichen Bereitstellungen benötigen Sie möglicherweise zusätzliche lokale Prozessserver zur Skalierung der Kapazität.
- Für das Failback benötigen Sie eine temporäre Prozessservereinrichtung in Azure. Sie können diese VM nach Abschluss des Failbacks löschen. 

In diesem Artikel werden typische Verwaltungstasks für diese zusätzlichen Prozessserver beschrieben.

## <a name="upgrade-a-process-server"></a>Durchführen eines Upgrades für einen Prozessserver

Führen Sie für Failbackzwecke ein Upgrade für einen lokal oder in Azure ausgeführten Prozessserver durch:

[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

> [!NOTE]
>   Wenn Sie mithilfe des Azure-Katalogimage einen Prozessserver in Azure für Failbackzwecke erstellen, wird in der Regel die neueste verfügbare Version ausgeführt. Die Site Recovery-Teams veröffentlichen in regelmäßigen Abständen Fehlerbehebungen und Verbesserungen, und es wird empfohlen, Prozessserver aktuell zu halten.

## <a name="balance-the-load-on-process-server"></a>Lastenausgleich auf Prozessservern

Gehen Sie wie folgt vor, um einen Lastenausgleich zwischen zwei Prozessservern vorzunehmen:

1. Navigieren Sie zu **Recovery Services-Tresor** > **Verwalten** > **Site Recovery-Infrastruktur** > **Für VMware und physische Computer** > **Konfigurationsserver**.
2. Klicken Sie auf den Konfigurationsserver, bei dem die Prozessserver registriert sind.
3. Eine Liste der bei den Konfigurationsservern registrierten Prozessserver steht auf der Seite zur Verfügung.
4. Klicken Sie auf den Prozessserver, auf dem Sie die Workload ändern möchten.

    ![Lastausgleich](media/vmware-azure-manage-process-server/LoadBalance.png)

5. Sie können je nach Bedarf wie unten beschrieben eine der Optionen **Lastenausgleich** oder **Wechseln** verwenden.

### <a name="load-balance"></a>Lastenausgleich

Über diese Option können Sie virtuelle Computer auswählen und auf einen anderen Prozessserver übertragen.

1. Klicken Sie auf **Lastenausgleich**, und wählen Sie in der Dropdownliste den Zielprozessserver aus. Klicken Sie auf **OK**

    ![Laden des Prozessservers](media/vmware-azure-manage-process-server/LoadPS.PNG)

2. Klicken Sie auf **Computer auswählen**, und wählen Sie die virtuellen Computer aus, die vom aktuellen Prozessserver auf den Zielprozessserver verschoben werden sollen. Für jeden virtuellen Computer werden Details zur durchschnittlichen Datenänderung angezeigt.
3. Klicken Sie auf **OK**. Den Fortschritt des Auftrags können Sie unter **Recovery Services-Tresor** > **Überwachung** > **Site Recovery-Aufträge** überwachen.
4. Es dauert 15 Minuten, bis die Änderungen nach erfolgreichem Abschluss dieses Vorgangs angezeigt werden. Um sie unmittelbar anzuzeigen, können Sie [den Konfigurationsserver aktualisieren](vmware-azure-manage-configuration-server.md#refresh-configuration-server).

### <a name="switch"></a>Switch

Über diese Option wird die gesamte für einen Prozessserver geschützte Workload auf einen anderen Prozessserver verschoben.

1. Klicken Sie auf **Wechseln**, wählen Sie den Zielprozessserver aus, und klicken Sie auf **OK**.

    ![Switch](media/vmware-azure-manage-process-server/Switch.PNG)

2. Den Fortschritt des Auftrags können Sie unter **Recovery Services-Tresor** > **Überwachung** > **Site Recovery-Aufträge** überwachen.
3. Es dauert 15 Minuten, bis die Änderungen nach erfolgreichem Abschluss dieses Vorgangs angezeigt werden. Um sie unmittelbar anzuzeigen, können Sie [den Konfigurationsserver aktualisieren](vmware-azure-manage-configuration-server.md#refresh-configuration-server).

## <a name="reregister-a-process-server"></a>Erneutes Registrieren eines Prozessservers

Wenn Sie erneut einen lokal oder in Azure ausgeführten Prozessserver mit dem Konfigurationsserver registrieren müssen, führen Sie die folgenden Schritte durch:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

Nachdem Sie die Einstellungen gespeichert haben, führen Sie folgende Schritte durch:

1. Öffnen Sie als Administrator eine Eingabeaufforderung auf dem Prozessserver.
2. Navigieren Sie zum Ordner **%PROGRAMDATA%\ASR\Agent**, und führen Sie den folgenden Befehl aus:

    ```
    cdpcli.exe --registermt
    net stop obengine
    net start obengine
    ```

## <a name="modify-proxy-settings-for-an-on-premises-process-server"></a>Ändern von Proxyeinstellungen für einen lokalen Prozessserver

Wenden Sie das nachfolgende Verfahren an, wenn der Prozessserver anhand eines Proxys eine Verbindung mit Site Recovery in Azure herstellt und Sie die vorliegenden Proxyeinstellungen ändern müssen.

1. Melden Sie sich beim Computer des Prozessservers an. 
2. Öffnen Sie als Administrator ein PowerShell-Eingabefenster, und führen Sie den folgenden Befehl aus:
   ```powershell
   $pwd = ConvertTo-SecureString -String MyProxyUserPassword
   Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
   net stop obengine
   net start obengine
   ```
2. Navigieren Sie zum Ordner **%PROGRAMDATA%\ASR\Agent**, und führen Sie den folgenden Befehl aus:
   ```
   cmd
   cdpcli.exe --registermt

   net stop obengine

   net start obengine

   exit
   ```


## <a name="remove-a-process-server"></a>Entfernen eines Prozessservers

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="manage-anti-virus-software-on-process-servers"></a>Verwalten von Virenschutzsoftware auf Prozessservern

Wenn Virenschutzsoftware auf einem eigenständigen Prozessserver oder Master-Zielserver aktiv ist, schließen Sie die folgenden Ordner von Virenschutzvorgängen aus:


- C:\Programme\Microsoft Azure Recovery Services Agent
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site Recovery
- Installationsverzeichnis des Prozessservers, Beispiel: C:\Programme (x86)\Microsoft Azure Site Recovery

