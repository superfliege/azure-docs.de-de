---
title: Herstellen einer Verbindung mit Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Verbindung mit dem ASDK herstellen.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 3cebbfa6-819a-41e3-9f1b-14ca0a2aaba3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2018
ms.author: jeffgilb
ms.openlocfilehash: 55be312046f5cdea2c1481ed435b5859ab2c2540
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50026825"
---
# <a name="connect-to-the-azure-stack-development-kit"></a>Herstellen einer Verbindung mit dem Azure Stack Development Kit

Zum Verwalten von Ressourcen müssen Sie zunächst eine Verbindung mit dem Azure Stack Development Kit (ASDK) herstellen. In diesem Artikel werden die Schritte zum Herstellen einer Verbindung mit dem ASDK beschrieben. Folgende Verbindungsoptionen stehen zur Verfügung:

* [Remotedesktopverbindung:](#connect-with-remote-desktop) Über eine Remotedesktopverbindung kann ein einzelner Benutzer schnell eine Verbindung mit dem Development Kit herstellen.
* [Virtuelles privates Netzwerk (VPN):](#connect-with-vpn) Bei Verwendung einer VPN-Verbindung können mehrere Benutzer parallel über Clients außerhalb der Azure Stack-Infrastruktur eine Verbindung herstellen. Eine VPN-Verbindung muss erst eingerichtet werden.

<a name="connect-to-azure-stack-with-remote-desktop"></a>
##  <a name="connect-to-azure-stack-by-using-remote-desktop-connection"></a>Herstellen einer Remotedesktopverbindung mit Azure Stack

Über eine Remotedesktopverbindung kann ein einzelner Benutzer Ressourcen im Betreiber- oder Benutzerportal verwalten.

1. Öffnen Sie Remotedesktopverbindung („mstc.exe“), und stellen Sie eine Verbindung mit dem Development Kit-Hostcomputer als **AzureStack\AzureStackAdmin** her. Verwenden Sie dabei das Kennwort, das Sie beim ASDK-Setup angegeben haben.  

2. Öffnen Sie auf dem Development Kit-Computer den Server-Manager („ServerManager.exe“). Wählen Sie **Lokaler Server** aus, deaktivieren Sie **Verstärkte Sicherheitskonfiguration für IE**, und schließen Sie den Server-Manager.

3. Melden Sie sich beim Verwaltungsportal als **AzureStack\CloudAdmin** an, oder verwenden Sie andere Azure Stack-Operatoranmeldeinformationen. Die Adresse des ASDK-Verwaltungsportals lautet [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).

4. Melden Sie sich beim Benutzerportal als **AzureStack\CloudAdmin** an, oder verwenden Sie andere Azure Stack-Benutzeranmeldeinformationen. Die Adresse des ASDK-Benutzerportals lautet [https://portal.local.azurestack.external](https://portal.local.azurestack.external).

> [!NOTE]
> Weitere Informationen zu den Einsatzgebieten der einzelnen Konten finden Sie unter [Grundlagen der ASDK-Verwaltung](asdk-admin-basics.md#what-account-should-i-use).

<a name="connect-to-azure-stack-with-vpn"></a>
## <a name="connect-to-azure-stack-by-using-vpn"></a>Herstellen einer VPN-Verbindung mit Azure Stack

Sie können eine VPN-Verbindung vom Typ „Geteilter Tunnel“ mit dem ASDK herstellen, um auf die Azure Stack-Portale und lokal installierte Tools wie Visual Studio und PowerShell zuzugreifen. Mit VPN-Verbindungen können mehrere Benutzer gleichzeitig auf Azure Stack-Ressourcen zugreifen, die beim ASDK gehostet werden.

VPN-Verbindungen werden in Azure AD- und AD FS-Bereitstellungen (Active Directory Federation Services, Active Directory-Verbunddienste) unterstützt.

> [!NOTE]
> Eine VPN-Verbindung bietet keine Konnektivität zu virtuellen Computern der Azure Stack-Infrastruktur.

### <a name="prerequisites"></a>Voraussetzungen
Bevor Sie eine VPN-Verbindung mit dem ASDK herstellen, stellen Sie sicher, dass Sie folgende Voraussetzungen erfüllen.

- Installieren Sie auf Ihrem lokalen Computer das [Azure Stack-kompatible Azure PowerShell-Modul](asdk-post-deploy.md#install-azure-stack-powershell).  
- Laden Sie die [Tools herunter, die für die Arbeit mit Azure Stack benötigt werden](asdk-post-deploy.md#download-the-azure-stack-tools).

### <a name="set-up-vpn-connectivity"></a>Einrichten der VPN-Konnektivität

Wenn Sie eine VPN-Verbindung mit dem ASDK herstellen möchten, öffnen Sie PowerShell als Administrator auf Ihrem lokalen Windows-Computer. Führen Sie dann das folgende Skript mit der IP-Adresse und dem Kennwort für Ihre Umgebung aus:

```PowerShell
# Change directories to the default Azure Stack tools directory
cd C:\AzureStack-Tools-master

# Configure Windows Remote Management (WinRM), if it's not already configured.
winrm quickconfig  

Set-ExecutionPolicy RemoteSigned

# Import the Connect module.
Import-Module .\Connect\AzureStack.Connect.psm1

# Add the development kit host computer’s IP address as the ASDK certificate authority (CA) to the list of trusted hosts. Make sure you update the IP address and password values for your environment.

$hostIP = "<Azure Stack host IP address>"

$Password = ConvertTo-SecureString `
  "<operator's password provided when deploying Azure Stack>" `
  -AsPlainText `
  -Force

Set-Item wsman:\localhost\Client\TrustedHosts `
  -Value $hostIP `
  -Concatenate

# Create a VPN connection entry for the local user.
Add-AzsVpnConnection `
  -ServerAddress $hostIP `
  -Password $Password

```

Wenn die Einrichtung erfolgreich war, wird **azurestack** in der Liste mit den VPN-Verbindungen angezeigt.

![Netzwerkverbindungen](media/asdk-connect/image3.png)  

### <a name="connect-to-azure-stack"></a>Herstellen einer Verbindung mit Azure Stack

Stellen Sie mithilfe einer der folgenden Methoden eine Verbindung mit der Azure Stack-Instanz her:  

* Verwenden Sie den Befehl `Connect-AzsVpn `:
    
  ```PowerShell
  Connect-AzsVpn `
    -Password $Password
  ```

  Stufen Sie den Azure Stack-Host bei entsprechender Aufforderung als vertrauenswürdig ein, und installieren Sie das Zertifikat über **AzureStackCertificateAuthority** im Zertifikatspeicher Ihres lokalen Computers. 
  
  > [!IMPORTANT]
  > Die Aufforderung wird unter Umständen durch das PowerShell-Fenster verdeckt.

* Klicken Sie auf Ihrem lokalen Computer auf **Netzwerkeinstellungen** > **VPN** > **azurestack** > **Verbinden**. Geben Sie an der Anmeldeeingabeaufforderung den Benutzernamen (**AzureStack\AzureStackAdmin**) und Ihr Kennwort ein.

### <a name="test-vpn-connectivity"></a>Testen der VPN-Konnektivität

Öffnen Sie zum Testen der Portalverbindung einen Webbrowser, und navigieren Sie zum Benutzerportal (https://portal.local.azurestack.external/) oder zum Verwaltungsportal (https://adminportal.local.azurestack.external/). Melden Sie sich an, und erstellen Sie Ressourcen.  

## <a name="next-steps"></a>Nächste Schritte

[Problembehandlung](asdk-troubleshooting.md)
