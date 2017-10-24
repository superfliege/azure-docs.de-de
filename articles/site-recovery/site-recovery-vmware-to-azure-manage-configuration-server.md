---
title: " Verwalten eines Konfigurationsservers in Azure Site Recovery | Microsoft Docs"
description: Dieser Artikel beschreibt das Einrichten und Verwalten eines Konfigurationsservers.
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 10/06/2017
ms.author: anoopkv
ms.openlocfilehash: e4740c96383468713976e5a98881bec13b0c1921
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="manage-a-configuration-server"></a>Verwalten eines Konfigurationsservers

Der Konfigurationsserver fungiert als Koordinator zwischen den Site Recovery-Diensten und Ihrer lokalen Infrastruktur. In diesem Artikel wird beschrieben, wie Sie den Konfigurationsserver einrichten, konfigurieren und verwalten können.

> [!NOTE]
> [Kapazitätsplanung](site-recovery-capacity-planner.md) ist ein wichtiger Schritt, um sicherzustellen, dass Sie den Konfigurationsserver mit einer Konfiguration bereitstellen, die Ihren Lastanforderungen entspricht. Lesen Sie hier mehr über [Anforderungen an die Größenanpassung für einen Konfigurationsserver](#sizing-requirements-for-a-configuration-server).


## <a name="prerequisites"></a>Voraussetzungen
Im Folgenden werden die minimalen Hardware-, Software- und Netzwerkanforderungen zum Einrichten eines Konfigurationsservers aufgeführt.
> [!IMPORTANT]
> Wenn Sie einen Konfigurationsserver für den Schutz von virtuellen VMware-Computern bereitstellen, wird empfohlen, diesen als virtuellen **Hochverfügbarkeitscomputer (HA)** bereitzustellen.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-the-configuration-server-software"></a>Herunterladen der Konfigurationsserversoftware

1. Melden Sie sich beim Azure-Portal an, und navigieren Sie zu Ihrem Recovery Services-Tresor.
2. Navigieren Sie zu **Site Recovery-Infrastruktur** > **Konfigurationsserver** (unter „Für VMware und physische Computer“).

  ![Seite „Server hinzufügen“](./media/site-recovery-vmware-to-azure-manage-configuration-server/AddServers.png)
3. Klicken Sie auf die **+Server**-Schaltfläche.
4. Klicken Sie auf der Seite **Server hinzufügen** auf die Downloadschaltfläche, um den Registrierungsschlüssel herunterzuladen. Sie benötigen diesen Schlüssel während der Installation des Konfigurationsservers zur Registrierung beim Azure Site Recovery-Dienst.
5. Klicken Sie auf den Link **Einheitliches Setup von Microsoft Azure Site Recovery herunterladen**, um die neueste Version des Konfigurationsservers herunterzuladen.

  ![Downloadseite](./media/site-recovery-vmware-to-azure-manage-configuration-server/downloadcs.png)

  > [!TIP]
  Die neueste Version des Konfigurationsservers kann direkt von der [Microsoft Download Center-Downloadseite](http://aka.ms/unifiedsetup) heruntergeladen werden.

## <a name="installing-and-registering-a-configuration-server-from-gui"></a>Installieren und Registrieren eines Konfigurationsservers von der GUI aus
[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

## <a name="installing-and-registering-a-configuration-server-using-command-line"></a>Installieren und Registrieren eines Konfigurationsservers über die Befehlszeile

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
  ```

### <a name="sample-usage"></a>Beispielverwendung
  ```
  MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
  cd C:\Temp\Extracted
  UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "CS" /InstallLocation "D:\" /MySQLCredsFilePath "C:\Temp\MySQLCredentialsfile.txt" /VaultCredsFilePath "C:\Temp\MyVault.vaultcredentials" /EnvType "VMWare"
  ```


### <a name="configuration-server-installer-command-line-arguments"></a>Befehlszeilenargumente für das Konfigurationsserver-Installationsprogramm.
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-mysql-credentials-file"></a>Erstellen einer MySql-Anmeldeinformationendatei
Der MySQLCredsFilePath-Parameter nimmt eine Datei als Eingabe an. Erstellen Sie die Datei im folgenden Format, und übergeben Sie sie als Eingabe an den Parameter MySQLCredsFilePath.
```
[MySQLCredentials]
MySQLRootPassword = "Password>"
MySQLUserPassword = "Password"
```
### <a name="create-a-proxy-settings-configuration-file"></a>Erstellen einer Konfigurationsdatei für Proxyeinstellungen
Der ProxySettingsFilePath-Parameter nimmt eine Datei als Eingabe an. Erstellen Sie die Datei im folgenden Format, und übergeben Sie sie als Eingabe an den Parameter ProxySettingsFilePath.

```
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-configuration-server"></a>Ändern der Proxyeinstellungen des Konfigurationsservers
1. Melden Sie sich am Konfigurationsserver an.
2. Starten Sie die Datei „cspsconfigtool.exe“ über die Verknüpfung auf Ihrem Desktop.
3. Klicken Sie auf die Registerkarte **Tresorregistrierung**.
4. Laden Sie eine neue Tresorregistrierungsdatei vom Portal herunter, und geben Sie sie als Eingabe für das Tool an.

  ![register-configuration-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
5. Geben Sie die Details des neuen Proxyservers an, und klicken Sie auf die Schaltfläche **Registrieren**.
6. Öffnen Sie ein PowerShell-Befehlsfenster mit Administratorrechten.
7. Führen Sie den folgenden Befehl aus
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```

  >[!WARNING]
  Wenn Sie Prozessserver für horizontales Hochskalieren mit diesem Konfigurationsserver verbunden haben, müssen Sie [die Proxyeinstellungen auf allen Prozessservern für horizontales Hochskalieren](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) in Ihrer Bereitstellung korrigieren.

## <a name="modify-user-accounts-and-passwords"></a>Ändern von Benutzerkonten und Kennwörtern

„CSPSConfigTool.exe“ wird zur Verwaltung von Benutzerkonten verwendet, die für die **automatische Ermittlung von virtuellen VMware-Computern** und zum Durchführen der **Pushinstallation des Mobility Service auf geschützten Computern verwendet werden. 

1. Melden Sie sich am Konfigurationsserver an.
2. Starten Sie „CSPSConfigtool.exe“, indem Sie auf dem Desktop auf die Verknüpfung klicken.
3. Klicken Sie auf die Registerkarte **Konten verwalten**.
4. Wählen Sie das Konto aus, für das das Kennwort geändert werden muss, und klicken Sie auf die Schaltfläche **Bearbeiten**.
5. Geben Sie das neue Kennwort ein, und klicken Sie auf **OK**.


## <a name="re-register-a-configuration-server-with-the-same-recovery-services-vault"></a>Erneutes Registrieren eines Konfigurationsservers beim gleichen Recovery Services-Tresor
  1. Melden Sie sich am Konfigurationsserver an.
  2. Starten Sie die Datei „cspsconfigtool.exe“ über die Verknüpfung auf Ihrem Desktop.
  3. Klicken Sie auf die Registerkarte **Tresorregistrierung**.
  4. Laden Sie eine neue Registrierungsdatei vom Portal herunter und geben Sie sie als Eingabe für das Tool an.
        ![register-configuration-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
  5. Geben Sie die Details des Proxyservers an, und klicken Sie auf die Schaltfläche **Registrieren**.  
  6. Öffnen Sie ein PowerShell-Befehlsfenster mit Administratorrechten.
  7. Führen Sie den folgenden Befehl aus

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

  >[!WARNING]
  Wenn mit diesem Konfigurationsserver Prozessserver für horizontales Hochskalieren verbunden sind, müssen Sie in Ihrer Bereitstellung [alle Prozessserver für horizontales Hochskalieren erneut registrieren](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server).

## <a name="registering-a-configuration-server-with-a-different-recovery-services-vault"></a>Registrieren eines Konfigurationsservers bei einem anderen Recovery Services-Tresor

> [!WARNING]
> In den nachfolgenden Schritten wird die Zuordnung der Konfiguration zum aktuellen Tresor getrennt und die Replikation aller geschützten virtuellen Computer unter dem Konfigurationsserver beendet.

1. Melden Sie sich am Konfigurationsserver an.
2. Führen Sie an einer Eingabeaufforderung mit Administratorrechten folgenden Befehl aus:

    ```
    reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
    net stop dra
    ```
3. Starten Sie die Datei „cspsconfigtool.exe“ über die Verknüpfung auf Ihrem Desktop.
4. Klicken Sie auf die Registerkarte **Tresorregistrierung**.
5. Laden Sie eine neue Registrierungsdatei vom Portal herunter und geben Sie sie als Eingabe für das Tool an.

    ![register-configuration-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
6. Geben Sie die Details des Proxyservers an, und klicken Sie auf die Schaltfläche **Registrieren**.  
7. Öffnen Sie ein PowerShell-Befehlsfenster mit Administratorrechten.
8. Führen Sie den folgenden Befehl aus
    ```
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="upgrading-a-configuration-server"></a>Durchführen eines Upgrades für einen Konfigurationsserver

> [!WARNING]
> Updates werden nur bis zur N-4. Version unterstützt. Wenn die neueste Version auf dem Markt z. B. 9.11 ist, können Sie aus Version 9.10, 9.9, 9.8 oder 9.7 ein direktes Update auf 9.11 ausführen. Wenn Sie jedoch Version 9.6 oder niedriger verwenden, müssen Sie mindestens auf Version 9.7 aktualisieren, bevor Sie die neuesten Updates auf Ihren Konfigurationsserver anwenden können. Downloadlinks für frühere Versionen finden Sie unter [Updates für den Azure Site Recovery-Dienst](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

1. Laden Sie den Update-Installer auf Ihren Konfigurationsserver herunter.
2. Starten Sie den Installer, indem Sie auf diesen doppelklicken.
3. Der Installer erkennt die Version der Site Recovery-Komponenten, die auf Computer vorhanden sind und fordert zu einer Bestätigung auf. 
4. Klicken Sie auf die Schaltfläche „OK“, um die Angabe zu bestätigen, und fahren Sie dann mit dem Upgrade fort.


## <a name="delete-or-unregister-a-configuration-server"></a>Löschen oder Aufheben der Registrierung eines Konfigurationsservers

> [!WARNING]
> Überprüfen Sie Folgendes vor der Außerbetriebnahme Ihres Konfigurationsservers.
> 1. [Deaktivieren Sie den Schutz](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) für alle virtuellen Computer unter diesem Konfigurationsserver.
> 2. [Heben Sie die Zuordnung von allen Replikationsrichtlinien zum Konfigurationsserver auf](site-recovery-setup-replication-settings-vmware.md#dissociate-a-configuration-server-from-a-replication-policy), oder [löschen](site-recovery-setup-replication-settings-vmware.md#delete-a-replication-policy) Sie sie.
> 3. [Löschen](site-recovery-vmware-to-azure-manage-vCenter.md#delete-a-vcenter-in-azure-site-recovery) Sie alle vCenter-Server/vSphere-Hosts, die mit dem Konfigurationsserver verknüpft sind.


### <a name="delete-the-configuration-server-from-azure-portal"></a>Löschen des Konfigurationsservers im Azure-Portal
1. Navigieren Sie im Azure-Portal im Menü „Tresor“ zu **Site Recovery-Infrastruktur** > **Konfigurationsserver**.
2. Klicken Sie auf den Konfigurationsserver, den Sie außer Betrieb nehmen möchten.
3. Klicken Sie auf der Detailseite des Konfigurationsservers auf die Schaltfläche „Löschen“.

  ![delete-configuration-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.PNG)
4. Klicken Sie auf **Ja**, um das Löschen des Servers zu bestätigen.

### <a name="uninstall-the-configuration-server-software-and-its-dependencies"></a>Deinstallieren von Software und Abhängigkeiten des Konfigurationsservers
  > [!TIP]
  Wenn Sie den Konfigurationsserver erneut mit Azure Site Recovery verwenden möchten, können Sie direkt mit Schritt 4 fortfahren.

1. Melden Sie sich als Administrator beim Konfigurationsserver an.
2. Öffnen Sie „Systemsteuerung > Programme > Programme deinstallieren“.
3. Deinstallieren Sie die Programme in der folgenden Reihenfolge:
  * Microsoft Azure Recovery Services-Agent
  * Microsoft Azure Site Recovery Mobility Service/Masterzielserver
  * Microsoft Azure Site Recovery-Anbieter
  * Microsoft Azure Site Recovery-Konfigurationsserver/Prozessserver
  * Microsoft Azure Site Recovery-Konfigurationsserverabhängigkeiten
  * MySQL Server 5.5
4. Führen Sie an einer Eingabeaufforderung mit Administratorrechten folgenden Befehl aus:
  ```
  reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
  ```

## <a name="delete-or-unregister-a-configuration-server-powershell"></a>Löschen oder Aufheben der Registrierung eines Konfigurationsservers (PowerShell)

1. [Installieren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) Sie das Azure PowerShell-Modul.
2. Melden Sie sich mithilfe des folgenden Befehls bei Ihrem Azure-Konto an:
    
    `Login-AzureRmAccount`
3. Wählen Sie das Abonnement aus, unter dem der Tresor vorhanden ist.

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Richten Sie jetzt den Kontext für Ihren Tresor ein.
    
    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. Rufen Sie Ihren ausgewählten Konfigurationsserver ab.

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Löschen Sie den Konfigurationsserver.

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> Die **-Force**-Option im Cmdlet „Remove-AzureRmSiteRecoveryFabric“ kann verwendet werden, um das Entfernen bzw. Löschen des Konfigurationsservers zu erzwingen.

## <a name="renew-configuration-server-secure-socket-layerssl-certificates"></a>Erneuern von SSL-Zertifikaten (Secure Socket Layer) für den Konfigurationsserver
Der Konfigurationsserver verfügt über einen integrierten Webserver, der die Aktivitäten von Mobility Service, Prozessserver und Masterzielserver, die mit dem Konfigurationsserver verbunden sind, orchestriert. Der Webserver des Konfigurationsservers verwendet ein SSL-Zertifikat zum Authentifizieren seiner Clients. Dieses Zertifikat hat eine Ablaufzeit von drei Jahren und kann jederzeit mithilfe des folgenden Verfahrens erneuert werden:

> [!WARNING]
Der Zertifikatablauf kann nur in Version 9.4.XXXX.X oder höher ausgeführt werden. Aktualisieren Sie alle Azure Site Recovery-Komponenten (Konfigurationsserver, Prozessserver, Masterzielserver, Mobility Service), bevor Sie den Workflow zum Erneuern von Zertifikaten starten.

1. Navigieren Sie im Azure-Portal zu Ihrem Tresor > „Site Recovery-Infrastruktur > Konfigurationsserver“.
2. Klicken Sie auf den Konfigurationsserver, für den Sie das SSL-Zertifikat erneuern müssen.
3. Unter der Konfigurationsserverintegrität sehen Sie das Ablaufdatum des SSL-Zertifikats.
4. Um das Zertifikat zu erneuern, klicken Sie wie in der folgenden Abbildung gezeigt auf die Aktion **Zertifikate erneuern**:

  ![delete-configuration-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/renew-cert-page.png)

### <a name="secure-socket-layer-certificate-expiry-warning"></a>Secure Socket Layer-Zertifikatsablaufwarnung

> [!NOTE]
Für alle Installationen, die vor dem Mai 2016 durchgeführt wurden, wurde die Gültigkeit des SSL-Zertifikats auf ein Jahr festgelegt. Sie haben bereits Benachrichtigungen über den Ablauf von Zertifikaten im Azure-Portal gesehen.

1. Wenn das SSL-Zertifikat des Konfigurationsservers in den nächsten zwei Monaten abläuft, beginnt der Dienst damit, die Benutzer über das Azure-Portal und per E-Mail zu benachrichtigen (Sie müssen allerdings Azure Site Recovery-Benachrichtigungen abonniert haben). Sie sehen dann ein Benachrichtigungsbanner auf der Ressourcenseite des Tresors.

  ![certificate-notification](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-renew-notification.png)
2. Klicken Sie auf das Banner, um weitere Details zum Ablauf des Zertifikats zu erfahren.

  ![certificate-details](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-expiry-details.png)

  >[!TIP]
  Möglicherweise wird anstelle der Schaltfläche **Jetzt verlängern** die Schaltfläche **Upgrade jetzt ausführen** angezeigt. Die Schaltfläche „Jetzt Upgrade durchführen“ bedeutet, dass für einige Komponenten in Ihrer Umgebung noch kein Upgrade auf 9.4.xxxx.x oder höhere Versionen durchgeführt wurde.

## <a name="revive-a-configuration-server-if-the-secure-socket-layer-ssl-certificate-expired"></a>Erneutes Aktivieren eines Konfigurationsservers bei abgelaufenem SSL-Zertifikat (Secure Socket Layer)

1. Aktualisieren Sie Ihren Konfigurationsserver auf die [neueste Version](http://aka.ms/unifiedinstaller).
2. Wenn Sie über Prozessserver für horizontales Hochskalieren, Failback-Masterzielserver und Prozessserver für Failback verfügen, aktualisieren Sie sie auf die neueste Version.
3. Aktualisieren Sie den Mobility Service auf allen geschützten virtuellen Computern auf die neueste Version.
4. Melden Sie sich beim Konfigurationsserver an, und öffnen Sie eine Eingabeaufforderung mit Administratorberechtigungen.
5. Navigieren Sie zum Ordner „%ProgramData%\ASR\home\svsystems\bin“.
6. Führen Sie „RenewCerts.exe“ aus, um das SSL-Zertifikat auf dem Konfigurationsserver zu verlängern.
7. Wenn der Vorgang erfolgreich durchgeführt wurde, sollte eine Meldung zur erfolgreichen Verlängerung des Zertifikats angezeigt werden.


## <a name="sizing-requirements-for-a-configuration-server"></a>Anforderungen an die Größenanpassung für einen Konfigurationsserver

| **CPU** | **Arbeitsspeicher** | **Größe des Cachedatenträgers** | **Datenänderungsrate** | **Geschützte Computer** |
| --- | --- | --- | --- | --- |
| 8 vCPUs (2 Sockets * 4 Kerne mit 2,5 GHz) |16 GB |300 GB |500 GB oder weniger |Weniger als 100 Computer replizieren. |
| 12 vCPUs (2 Sockets * 6 Kerne mit 2,5 GHz) |18 GB |600 GB |500 GB bis 1 TB |Zwischen 100 und 150 Computer replizieren. |
| 16 vCPUs (2 Sockets * 8 Kerne mit 2,5 GHz) |32 GB |1 TB |1 TB bis 2 TB |Zwischen 150 und 200 Computer replizieren. |

  >[!TIP]
  Wenn Ihre täglichen Datenänderungen 2 TB überschreiten, oder Sie über 200 virtuelle Computer replizieren möchten, sollten Sie zusätzliche Prozessserver für den Lastenausgleich des Replikationsdatenverkehrs bereitstellen. Erfahren Sie mehr über das Bereitstellen von Prozessservern für horizontales Hochskalieren.


## <a name="common-issues"></a>Häufige Probleme
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]
