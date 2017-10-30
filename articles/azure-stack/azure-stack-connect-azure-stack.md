---
title: Herstellen einer Verbindung mit Azure Stack | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Verbindung mit Azure Stack herstellen.
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 3cebbfa6-819a-41e3-9f1b-14ca0a2aaba3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/25/2017
ms.author: sngun
ms.openlocfilehash: ba2359739b1d9cd265879227a499c94f93d8e4c6
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2017
---
# <a name="connect-to-azure-stack"></a>Herstellen einer Verbindung mit Azure Stack

*Gilt für: Azure Stack Development Kit*

Zum Verwalten von Ressourcen müssen Sie zunächst eine Verbindung mit dem Azure Stack Development Kit herstellen. Wie das geht, erfahren Sie in diesem Artikel. Folgende Verbindungsoptionen stehen zur Verfügung:

* [Remotedesktopverbindung:](#connect-with-remote-desktop) Bei einer Remotedesktopverbindung kann ein einzelner Benutzer schnell eine Verbindung über das Development Kit herstellen.
* [Virtuelles privates Netzwerk (VPN):](#connect-with-vpn) Bei Verwendung einer VPN-Verbindung können mehrere Benutzer parallel über Clients außerhalb der Azure Stack-Infrastruktur eine Verbindung herstellen. (Dies muss allerdings entsprechend eingerichtet werden.)

<a name="connect-to-azure-stack-with-remote-desktop"></a>
##  <a name="connect-to-azure-stack-by-using-remote-desktop-connection"></a>Herstellen einer Remotedesktopverbindung mit Azure Stack
Über eine Remotedesktopverbindung kann ein einzelner Benutzer Ressourcen im Betreiber- oder Benutzerportal verwalten.

1. Öffnen Sie „Remotedesktopverbindung“, und stellen Sie eine Verbindung mit dem Development Kit her. Geben Sie als Benutzername **AzureStack\AzureStackAdmin** ein. Verwenden Sie das Betreiberkennwort, das Sie beim Einrichten von Azure Stack angegeben.  

2. Öffnen Sie auf dem Development Kit-Computer den Server-Manager. Klicken Sie auf **Lokaler Server**, deaktivieren Sie das Kontrollkästchen **Verstärkte Sicherheitskonfiguration für Internet Explorer**, und schließen Sie den Server-Manager wieder.

3. Das [Benutzerportal](azure-stack-key-features.md#portal) finden Sie unter https://portal.local.azurestack.external. Melden Sie sich mit Benutzeranmeldeinformationen an. Das [Azure Stack-Betreiberportal](azure-stack-key-features.md#portal) finden Sie unter https://adminportal.local.azurestack.external/. Melden Sie sich mit den Azure AD-Anmeldeinformationen (Azure Active Directory) an, die Sie im Rahmen der Installation angegeben haben.

<a name="connect-to-azure-stack-with-vpn"></a>
## <a name="connect-to-azure-stack-by-using-vpn"></a>Herstellen einer VPN-Verbindung mit Azure Stack

Sie können eine VPN-Verbindung vom Typ „Geteilter Tunnel“ mit einem Azure Stack Development Kit herstellen. Über eine VPN-Verbindung können Sie auf das Azure Stack-Betreiberportal, auf das Benutzerportal sowie auf lokal installierte Tools wie Visual Studio und PowerShell zugreifen, um Azure Stack-Ressourcen zu verwalten. VPN-Verbindungen werden sowohl in Azure AD- als auch in AD FS-Bereitstellungen (Active Directory-Verbunddienste) unterstützt. Über VPN-Verbindungen können mehrere Clients gleichzeitig eine Verbindung mit Azure Stack herstellen. 

> [!NOTE] 
> Eine VPN-Verbindung bietet keine Konnektivität zu virtuellen Computern der Azure Stack-Infrastruktur. 

### <a name="prerequisites"></a>Voraussetzungen

1. Installieren Sie auf Ihrem lokalen Computer das [Azure Stack-kompatible Azure PowerShell-Modul](azure-stack-powershell-install.md).  
2. Laden Sie die [Tools herunter, die für die Arbeit mit Azure Stack benötigt werden](azure-stack-powershell-download.md). 

### <a name="set-up-vpn-connectivity"></a>Einrichten der VPN-Konnektivität

Wenn Sie eine VPN-Verbindung mit dem Development Kit herstellen möchten, öffnen Sie zunächst auf Ihrem lokalen Windows-basierten Computer Windows PowerShell als Administrator. Führen Sie dann das folgende Skript mit der IP-Adresse und dem Kennwort für Ihre Umgebung aus:

```PowerShell 
# Configure Windows Remote Management (WinRM), if it's not already configured.
winrm quickconfig  

Set-ExecutionPolicy RemoteSigned

# Import the Connect module.
Import-Module .\Connect\AzureStack.Connect.psm1 

# Add the development kit computer’s host IP address and certificate authority (CA) to the list of trusted hosts. Make sure you update the IP address and password values for your environment. 

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

![Netzwerkverbindungen](media/azure-stack-connect-azure-stack/image3.png)  

### <a name="connect-to-azure-stack"></a>Herstellen einer Verbindung mit Azure Stack

Stellen Sie mithilfe einer der folgenden Methoden eine Verbindung mit der Azure Stack-Instanz her:  

* Verwenden Sie den Befehl `Connect-AzsVpn `: 
    
  ```PowerShell
  Connect-AzsVpn `
    -Password $Password
  ```

  Stufen Sie den Azure Stack-Host bei entsprechender Aufforderung als vertrauenswürdig ein, und installieren Sie das Zertifikat über **AzureStackCertificateAuthority** im Zertifikatspeicher Ihres lokalen Computers. (Die Aufforderung wird unter Umständen durch das PowerShell-Fenster verdeckt.) 

* Klicken Sie auf Ihrem lokalen Computer auf **Netzwerkeinstellungen** > **VPN** > **azurestack** > **Verbinden**. Geben Sie an der Anmeldeeingabeaufforderung den Benutzernamen (**AzureStack\AzureStackAdmin**) und Ihr Kennwort ein.

### <a name="test-vpn-connectivity"></a>Testen der VPN-Konnektivität

Öffnen Sie zum Testen der Portalverbindung einen Webbrowser, und navigieren Sie entweder zum Benutzerportal (https://portal.local.azurestack.external/) oder zum Betreiberportal (https://adminportal.local.azurestack.external/). Melden Sie sich an, und erstellen Sie Ressourcen.  

## <a name="next-steps"></a>Nächste Schritte

[Verfügbarmachen von virtuellen Computern für Ihre Azure Stack-Benutzer](azure-stack-tutorial-tenant-vm.md)

