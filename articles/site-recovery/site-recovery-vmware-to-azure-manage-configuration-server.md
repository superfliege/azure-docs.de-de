---
title: "Verwalten des Konfigurationsservers für die VMware-Notfallwiederherstellung mit Azure Site Recovery | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie einen vorhandenen Konfigurationsserver für die VMware-Notfallwiederherstellung in Azure mit Azure Site Recovery verwalten."
services: site-recovery
author: AnoopVasudavan
ms.service: site-recovery
ms.topic: article
ms.date: 02/18/2018
ms.author: anoopkv
ms.openlocfilehash: 80426b076481aaf2001644498da8cdce6f0311ab
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2018
---
# <a name="manage-the-configuration-server-for-vmware-vms"></a>Verwalten des Konfigurationsservers für virtuelle VMware-Computer

Sie richten einen lokalen Konfigurationsserver ein, wenn Sie [Azure Site Recovery](site-recovery-overview.md) für die Notfallwiederherstellung von VMware-VMs und physischen Servern in Azure verwenden. Der Konfigurationsserver koordiniert die Kommunikation zwischen der lokalen VMware-Umgebung und Azure und verwaltet die Datenreplikation. In diesem Artikel werden häufige Aufgaben zur Verwaltung des Konfigurationsservers nach dessen Bereitstellung zusammengefasst.


## <a name="modify-vmware-settings"></a>Ändern von VMware-Einstellungen

Ändern Sie die Einstellungen für den VMware-Server, mit dem der Konfigurationsserver eine Verbindung herstellt.

1. Melden Sie sich auf dem Computer an, auf dem der Konfigurationsserver ausgeführt wird.
2. Starten Sie den Azure Site Recovery-Konfigurations-Manager über die Desktopverknüpfung. Öffnen Sie alternativ die [diesen Link](https://configuration-server-name/IP:44315).
3. Wählen Sie **vCenter-Server/vSPhere ESXi-Server verwalten** aus, und führen Sie dann folgende Schritte aus:

    * Um dem Konfigurationsserver einen anderen VMware-Server zuzuordnen, klicken Sie auf **vCenter-Server/vSphere ESXi-Server hinzufügen**. Geben Sie die Informationen zum Server an.

    * Wenn Sie die Anmeldeinformationen für die Verbindung mit dem VMware-Server für die automatische Ermittlung von VMware-VMs ändern möchten, klicken Sie auf **Bearbeiten**. Geben Sie die neuen Anmeldeinformationen ein, und klicken Sie dann auf **OK**.

    ![Ändern von VMware](./media/site-recovery-vmware-to-azure-manage-configuration-server/modify-vmware-server.png)

## <a name="modify-credentials-for-mobility-service-installation"></a>Ändern der Anmeldeinformationen für die Installation von Mobility Service

Ändern Sie die Anmeldeinformationen für die automatische Installation von Mobility Service auf VMware-VMs, die Sie für die Replikation aktivieren möchten.

1. Melden Sie sich auf dem Computer an, auf dem der Konfigurationsserver ausgeführt wird.
2. Starten Sie den Site Recovery-Konfigurations-Manager über die Desktopverknüpfung. Öffnen Sie alternativ [diesen Link](https://configuration-server-name/IP:44315).
3. Klicken Sie auf **VM-Anmeldeinformationen verwalten**, und geben Sie die neuen Anmeldeinformationen an. Klicken Sie auf **OK**, um die Einstellungen zu aktualisieren.

    ![Ändern der Anmeldeinformationen für Mobility Service](./media/site-recovery-vmware-to-azure-manage-configuration-server/modify-mobility-credentials.png)

## <a name="modify-proxy-settings"></a>Ändern von Proxyeinstellungen

Ändern Sie die Proxyeinstellungen, die vom Konfigurationsservercomputer für den Internetzugriff in Azure verwendet werden. Ändern Sie die Einstellungen auf beiden Computern, wenn Sie zusätzlich zum Standardprozessserver, der auf dem Konfigurationsservercomputer ausgeführt wird, über einen weiteren Prozessservercomputer verfügen.

1. Melden Sie sich auf dem Computer an, auf dem der Konfigurationsserver ausgeführt wird.
2. Starten Sie den Site Recovery-Konfigurations-Manager über die Desktopverknüpfung. Öffnen Sie alternativ [diesen Link](https://configuration-server-name/IP:44315).
3. Klicken Sie auf **Konnektivität verwalten**, und aktualisieren Sie die Proxywerte. Klicken Sie auf **Speichern**, um die Einstellungen zu aktualisieren.

## <a name="add-a-network-adapter"></a>Hinzufügen eines Netzwerkadapters

Die OVF-Vorlage (Open Virtualization Format) stellt die Konfigurationsserver-VM mit einem einzelnen Netzwerkadapter bereit. Sie können [der VM einen zusätzlichen Adapter hinzufügen](how-to-deploy-configuration-server.md#add-an-additional-adapter), müssen diesen Schritt aber vor der Registrierung des Konfigurationsservers im Tresor durchführen.

Nach dem Registrieren des Konfigurationsservers im Tresor fügen Sie einen Adapter in den Eigenschaften des virtuellen Computers hinzu. Registrieren Sie anschließend den Server im Tresor.


## <a name="reregister-a-configuration-server-in-the-same-vault"></a>Erneutes Registrieren eines Konfigurationsservers im selben Tresor

Sie können den Konfigurationsserver bei Bedarf im selben Tresor erneut registrieren. Wenn Sie zusätzlich zum Standardprozessserver, der auf dem Konfigurationsservercomputer ausgeführt wird, über einen weiteren Prozessservercomputer verfügen, registrieren Sie beide Computer erneut.


  1. Öffnen Sie im Tresor **Verwalten** > **Site Recovery-Infrastruktur** > **Konfigurationsserver**.
  2. Klicken Sie unter **Server** auf **Registrierungsschlüssel herunterladen**, um die Datei mit den Tresoranmeldeinformationen herunterzuladen.
  3. Melden Sie sich auf dem Konfigurationsservercomputer an.
  4. Öffnen Sie in **%ProgramData%\ASR\home\svagent\bin** die Datei **cspsconfigtool.exe**.
  5. Klicken Sie auf der Registerkarte **Tresorregistrierung** auf **Durchsuchen**, und suchen Sie die Datei mit den Anmeldeinformationen für den Tresor, die Sie heruntergeladen haben.
  6. Geben Sie bei Bedarf die Proxyserverdetails an. Klicken Sie anschließend auf **Registrieren**.
  7. Öffnen Sie als Administrator ein PowerShell-Eingabefenster, und führen Sie den folgenden Befehl aus:

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

## <a name="upgrade-the-configuration-server"></a>Aktualisieren Sie den Konfigurationsserver

Sie führen Updaterollups aus, um den Konfigurationsserver zu aktualisieren. Updates können für maximal N-4 Versionen angewendet werden. Beispiel: 

- Wenn Sie Version 9.7, 9.8, 9.9 oder 9.10 ausführen, können Sie direkt auf 9.11 aktualisieren.
- Falls Sie Version 9.6 oder eine ältere Version ausführen und auf 9.11 aktualisieren möchten, müssen Sie zuerst das Upgrade auf Version 9.7 durchführen, bevor das Upgrade auf 9.11 möglich ist.

Links zu Updaterollups zum Aktualisieren aller Versionen des Konfigurationsservers sind auf der [Wiki-Seite zu den Updates](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx) verfügbar.

Aktualisieren Sie den Server wie folgt:

1. Laden Sie die Datei mit dem Update-Installer auf den Konfigurationsserver herunter.
2. Doppelklicken Sie auf die Datei, um das Installationsprogramm auszuführen.
3. Das Installationsprogramm erkennt die aktuelle Version, die auf dem Computer ausgeführt wird.
4. Klicken Sie auf **OK**, um den Vorgang zu bestätigen und das Upgrade durchzuführen. 


## <a name="delete-or-unregister-a-configuration-server"></a>Löschen oder Aufheben der Registrierung eines Konfigurationsservers

1. Deaktivieren Sie die Option [Schutz deaktivieren](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) für alle virtuellen Computer unter dem Konfigurationsserver.
2. [Heben Sie die Zuordnung von allen Replikationsrichtlinien zum Konfigurationsserver auf](site-recovery-setup-replication-settings-vmware.md#dissociate-a-configuration-server-from-a-replication-policy), oder [löschen](site-recovery-setup-replication-settings-vmware.md#delete-a-replication-policy) Sie sie.
3. [Löschen](site-recovery-vmware-to-azure-manage-vCenter.md#delete-a-vcenter-in-azure-site-recovery) Sie alle vCenter-Server/vSphere-Hosts, die dem Konfigurationsserver zugeordnet sind.
4. Öffnen Sie im Tresor **Site Recovery-Infrastruktur** > **Konfigurationsserver**.
5. Wählen Sie den Konfigurationsserver aus, den Sie entfernen möchten. Klicken Sie auf der Seite **Details** auf **Löschen**.

    ![Löschen von Konfigurationsservern](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.png)
   

### <a name="delete-with-powershell"></a>Löschen mit PowerShell

Optional können Sie den Konfigurationsserver mithilfe von PowerShell löschen:

1. [Installieren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) Sie das Azure PowerShell-Modul.
2. Melden Sie sich mit diesem Befehl bei Ihrem Azure-Konto an:
    
    `Login-AzureRmAccount`
3. Wählen Sie das Tresorabonnement aus:

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Legen Sie den Tresorkontext fest.
    
    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. Rufen Sie den Konfigurationsserver ab:

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Löschen Sie den Konfigurationsserver.

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> Sie können die Option **-Force** im Cmdlet Remove-AzureRmSiteRecoveryFabric verwenden, um das Löschen des Konfigurationsservers zu erzwingen.
 


## <a name="renew-ssl-certificates"></a>Erneuern von SSL-Zertifikaten

Der Konfigurationsserver verfügt über einen integrierten Webserver, der die Aktivitäten von Mobility Service, Prozessservern und Masterzielservern, die mit dem Konfigurationsserver verbunden sind, orchestriert. Der Webserver verwendet ein SSL-Zertifikat, um Clients zu authentifizieren. Das Zertifikat läuft nach drei Jahren ab und kann jederzeit erneuert werden.

### <a name="check-expiry"></a>Überprüfen des Ablaufs

Für Bereitstellungen von Konfigurationsservern vor dem Mai 2016 wurde die Zertifikatablaufzeit auf ein Jahr festgelegt. Wenn eines Ihrer Zertifikate demnächst abläuft, geschieht Folgendes:

- Wenn das Ablaufdatum höchstens zwei Monate in der Zukunft liegt, sendet der Dienst Benachrichtigungen im Portal und per E-Mail (wenn Sie Site Recovery-Benachrichtigungen abonniert haben).
- Auf der Ressourcenseite des Tresors wird ein Benachrichtigungsbanner angezeigt. Klicken Sie auf das Banner, um ausführlichere Informationen zu erhalten.
- Wenn die Schaltfläche **Jetzt Upgrade durchführen** angezeigt wird, wurde für einige Komponenten in Ihrer Umgebung noch kein Upgrade auf 9.4.xxxx.x oder höhere Versionen durchgeführt. Aktualisieren Sie die Komponenten, bevor Sie das Zertifikat erneuern. Sie können bei älteren Versionen keine Erneuerung durchführen.

### <a name="renew-the-certificate"></a>Erneuern von Zertifikaten

1. Öffnen Sie im Tresor **Site Recovery-Infrastruktur** > **Konfigurationsserver**. Wählen Sie den erforderlichen Konfigurationsserver aus.
2. Das Ablaufdatum wird unter **Integrität des Konfigurationsservers** angezeigt.
3. Klicken Sie auf **Zertifikate erneuern**. 


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die Tutorials zum Einrichten der Notfallwiederherstellung von [VMware-VMs](tutorial-vmware-to-azure.md) in Azure an.
