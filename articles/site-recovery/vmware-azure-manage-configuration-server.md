---
title: Verwalten des Konfigurationsservers für die VMware-Notfallwiederherstellung mit Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie einen vorhandenen Konfigurationsserver für die VMware-Notfallwiederherstellung in Azure mit Azure Site Recovery verwalten.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: df5b2ecce2a5c9d7c263ee0acc3a49b859b93f7f
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39346119"
---
# <a name="manage-the-configuration-server-for-vmware-vms"></a>Verwalten des Konfigurationsservers für virtuelle VMware-Computer

Sie richten einen lokalen Konfigurationsserver ein, wenn Sie [Azure Site Recovery](site-recovery-overview.md) für die Notfallwiederherstellung von VMware-VMs und physischen Servern in Azure verwenden. Der Konfigurationsserver koordiniert die Kommunikation zwischen der lokalen VMware-Umgebung und Azure und verwaltet die Datenreplikation. In diesem Artikel werden häufige Aufgaben zur Verwaltung des Konfigurationsservers nach dessen Bereitstellung zusammengefasst.



## <a name="modify-vmware-settings"></a>Ändern von VMware-Einstellungen

Sie können wie folgt auf den Konfigurationsserver zugreifen:
    - Melden Sie sich bei der VM an, auf der er bereitgestellt wird, und starten Sie den Azure Site Recovery Configuration Manager über die Desktopverknüpfung.
    - Alternativ können Sie remote über **https://*ConfigurationServerName*/:44315 /** auf den Konfigurationsserver zugreifen. Melden Sie sich mit Administratoranmeldeinformationen an.
   
### <a name="modify-vmware-server-settings"></a>Ändern von VMware-Servereinstellungen

1. Um dem Konfigurationsserver einen anderen VMware-Server zuzuordnen, wählen Sie nach dem Anmelden **vCenter-Server/vSphere ESXi-Server hinzufügen** aus.
2. Geben Sie die Eigenschaften ein, und wählen Sie dann **OK** aus.


### <a name="modify-credentials-for-automatic-discovery"></a>Ändern der Anmeldeinformationen für die automatische Ermittlung

1. Wenn Sie die Anmeldeinformationen für die Verbindung mit dem VMware-Server für die automatische Ermittlung von VMware-VMs ändern möchten, klicken Sie nach dem Anmelden auf **Bearbeiten**.
2. Geben Sie die neuen Anmeldeinformationen ein, und klicken Sie dann auf **OK**.

    ![Ändern von VMware](./media/vmware-azure-manage-configuration-server/modify-vmware-server.png)


## <a name="modify-credentials-for-mobility-service-installation"></a>Ändern der Anmeldeinformationen für die Installation von Mobility Service

Ändern Sie die Anmeldeinformationen für die automatische Installation von Mobility Service auf VMware-VMs, die Sie für die Replikation aktivieren möchten.

1. Wählen Sie nach der Anmeldung **VM-Anmeldeinformationen verwalten** aus.
2. Geben Sie die neuen Anmeldeinformationen ein, und klicken Sie dann auf **OK**.

    ![Ändern der Anmeldeinformationen für Mobility Service](./media/vmware-azure-manage-configuration-server/modify-mobility-credentials.png)

## <a name="modify-proxy-settings"></a>Ändern von Proxyeinstellungen

Ändern Sie die Proxyeinstellungen, die vom Konfigurationsservercomputer für den Internetzugriff in Azure verwendet werden. Ändern Sie die Einstellungen auf beiden Computern, wenn Sie zusätzlich zum Standardprozessserver, der auf dem Konfigurationsservercomputer ausgeführt wird, über einen weiteren Prozessservercomputer verfügen.

1. Wählen Sie nach der Anmeldung beim Konfigurationsserver **Konnektivität verwalten** aus.
2. Aktualisieren Sie die Proxywerte. Klicken Sie auf **Speichern**, um die Einstellungen zu aktualisieren.

## <a name="add-a-network-adapter"></a>Hinzufügen eines Netzwerkadapters

Die OVF-Vorlage (Open Virtualization Format) stellt die Konfigurationsserver-VM mit einem einzelnen Netzwerkadapter bereit.

- Sie können [der VM einen zusätzlichen Adapter hinzufügen](vmware-azure-deploy-configuration-server.md#add-an-additional-adapter), müssen diesen Schritt aber vor der Registrierung des Konfigurationsservers im Tresor durchführen.
- Nach dem Registrieren des Konfigurationsservers im Tresor fügen Sie einen Adapter in den Eigenschaften des virtuellen Computers hinzu. Dann müssen Sie den Server erneut im Tresor registrieren.


## <a name="reregister-a-configuration-server-in-the-same-vault"></a>Erneutes Registrieren eines Konfigurationsservers im selben Tresor

Sie können den Konfigurationsserver bei Bedarf im selben Tresor erneut registrieren. Wenn Sie zusätzlich zum Standardprozessserver, der auf dem Konfigurationsservercomputer ausgeführt wird, über einen weiteren Prozessservercomputer verfügen, registrieren Sie beide Computer erneut.


  1. Öffnen Sie im Tresor **Verwalten** > **Site Recovery-Infrastruktur** > **Konfigurationsserver**.
  2. Klicken Sie unter **Server** auf **Registrierungsschlüssel herunterladen**, um die Datei mit den Tresoranmeldeinformationen herunterzuladen.
  3. Melden Sie sich auf dem Konfigurationsservercomputer an.
  4. Öffnen Sie in **%ProgramData%\ASR\home\svsystems\bin** die Datei **cspsconfigtool.exe**.
  5. Klicken Sie auf der Registerkarte **Tresorregistrierung** auf **Durchsuchen**, und suchen Sie die Datei mit den Anmeldeinformationen für den Tresor, die Sie heruntergeladen haben.
  6. Geben Sie bei Bedarf die Proxyserverdetails an. Klicken Sie anschließend auf **Registrieren**.
  7. Öffnen Sie als Administrator ein PowerShell-Eingabefenster, und führen Sie den folgenden Befehl aus:
   ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
   ```

      >[!NOTE] 
      >Um das **neueste Zertifikat** per Pull vom Konfigurationsserver auf den Prozessserver für die horizontale Skalierung abzurufen, führen Sie den folgenden Befehl aus: *"<Installation Drive\Microsoft Azure Site Recovery\agent\cdpcli.exe>" --registermt*

  8. Starten Sie den OBEngine-Dienst neu, indem Sie den folgenden Befehl ausführen.
  ```
          net stop obengine
          net start obengine

## Upgrade the configuration server

You run update rollups to update the configuration server. Updates can be applied for up to N-4 versions. For example:

- If you run 9.7, 9.8, 9.9, or 9.10, you can upgrade directly to 9.11.
- If you run 9.6 or earlier and you want to upgrade to 9.11, you must first upgrade to version 9.7. before 9.11.

Links to update rollups for upgrading to all versions of the configuration server are available in the [wiki updates page](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

Upgrade the server as follows:

1. In the vault, go to **Manage** > **Site Recovery Infrastructure** > **Configuration Servers**.
2. If an update is available, a link appears in the **Agent Version** > column.
    ![Update](./media/vmware-azure-manage-configuration-server/update2.png)
3. Download the update installer file to the configuration server.

    ![Update](./media/vmware-azure-manage-configuration-server/update1.png)

4. Double-click to run the installer.
5. The installer detects the current version running on the machine. Click **Yes** to start the upgrade.
6. When the upgrade completes the server configuration validates.

    ![Update](./media/vmware-azure-manage-configuration-server/update3.png)
    
7. Click **Finish** to close the installer.

## Delete or unregister a configuration server

1. [Disable protection](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) for all VMs under the configuration server.
2. [Disassociate](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) and [delete](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) all replication policies from the configuration server.
3. [Delete](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) all vCenter servers/vSphere hosts that are associated with the configuration server.
4. In the vault, open **Site Recovery Infrastructure** > **Configuration Servers**.
5. Select the configuration server that you want to remove. Then, on the **Details** page, select **Delete**.

    ![Delete configuration server](./media/vmware-azure-manage-configuration-server/delete-configuration-server.png)
   

### Delete with PowerShell

You can optionally delete the configuration server by using PowerShell.

1. [Install](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) the Azure PowerShell module.
2. Sign in to your Azure account by using this command:
    
    `Connect-AzureRmAccount`
3. Select the vault subscription.

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Set the vault context.
    
    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault> Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. Retrieve the configuration server.

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Delete the configuration server.

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> You can use the **-Force** option in Remove-AzureRmSiteRecoveryFabric for forced deletion of the configuration server.
 
## Generate configuration server Passphrase

1. Sign in to your configuration server, and then open a command prompt window as an administrator.
2. To change the directory to the bin folder, execute the command **cd %ProgramData%\ASR\home\svsystems\bin**
3. To generate the passphrase file, execute **genpassphrase.exe -v > MobSvc.passphrase**.
4. Your passphrase will be stored in the file located at **%ProgramData%\ASR\home\svsystems\bin\MobSvc.passphrase**.

## Renew SSL certificates

The configuration server has an inbuilt web server, which orchestrates activities of the Mobility Service, process servers, and master target servers connected to it. The web server uses an SSL certificate to authenticate clients. The certificate expires after three years and can be renewed at any time.

### Check expiry

For configuration server deployments before May 2016, certificate expiry was set to one year. If you have a certificate that is going to expire, the following occurs:

- When the expiry date is two months or less, the service starts sending notifications in the portal, and by email (if you subscribed to Site Recovery notifications).
- A notification banner appears on the vault resource page. For more information, select the banner.
- If you see an **Upgrade Now** button, it indicates that some components in your environment haven't been upgraded to 9.4.xxxx.x or higher versions. Upgrade the components before you renew the certificate. You can't renew on older versions.

### Renew the certificate

1. In the vault, open **Site Recovery Infrastructure** > **Configuration Server**. Select the required configuration server.
2. The expiry date appears under **Configuration Server health**.
3. Select **Renew Certificates**. 


## Next steps

Review the tutorials for setting up disaster recovery of [VMware VMs](vmware-azure-tutorial.md) to Azure.
