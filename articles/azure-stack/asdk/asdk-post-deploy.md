---
title: Konfigurationen nach der Bereitstellung für das Azure Stack Development Kit (ASDK) | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die empfohlenen Konfigurationsänderungen, die nach der Installation des Azure Stack Development Kits (ASDK) vorgenommen werden müssen.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 5c379e0bc6789950b05f75731937ed9bc1b44565
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54391205"
---
# <a name="post-asdk-installation-configuration-tasks"></a>Konfigurationsaufgaben nach der Installation des ASDK

Nach dem [Installieren des Azure Stack Development Kits (ASDK)](asdk-install.md) müssen Sie einige nach der Installation empfohlene Konfigurationsänderungen vornehmen, während Sie auf dem ASDK-Hostcomputer als „AzureStack\AzureStackAdmin“ angemeldet sind. 

## <a name="install-azure-stack-powershell"></a>Installieren von Azure Stack-PowerShell

Für die Verwendung von Azure Stack sind mit Azure Stack kompatible Azure PowerShell-Module erforderlich.

PowerShell-Befehle für Azure Stack werden über den PowerShell-Katalog installiert. Öffnen Sie zum Registrieren des PSGallery-Repositorys eine PowerShell-Sitzung mit erhöhten Rechten, und führen Sie den folgenden Befehl aus:

``` Powershell
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

Sie können API-Versionsprofile verwenden, um mit Azure Stack kompatible AzureRM-Module anzugeben.  API-Versionsprofile bieten eine Möglichkeit, Versionsunterschiede zwischen Azure und Azure Stack zu verwalten. Ein API-Versionsprofil ist eine Gruppe von AzureRM-PowerShell-Modulen mit spezifischen API-Versionen. Das über den PowerShell-Katalog verfügbare Modul **AzureRM.Bootstrapper** stellt PowerShell-Cmdlets bereit, die für die Arbeit mit API-Versionsprofilen erforderlich sind.

Sie können das aktuelle Azure Stack PowerShell-Modul mit oder ohne Internetverbindung mit dem ASDK-Hostcomputer installieren:

> [!IMPORTANT]
> Vor der Installation der erforderlichen Version [deinstallieren Sie unbedingt alle vorhandenen Azure PowerShell-Module](../azure-stack-powershell-install.md#3-uninstall-existing-versions-of-the-azure-stack-powershell-modules).

- **Mit Internetverbindung** auf dem ASDK-Hostcomputer. Führen Sie zum Installieren dieser Module in Ihrer Development Kit-Installation das folgende PowerShell-Skript aus:

  - Azure Stack 1811 oder höher:

    ``` PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet. 
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    # Install Azure Stack Module Version 1.6.0.
    Install-Module -Name AzureStack -RequiredVersion 1.6.0
    ```

  - Azure Stack 1808 oder höher:

    ``` PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet. 
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    # Install Azure Stack Module Version 1.5.0.
    Install-Module -Name AzureStack -RequiredVersion 1.5.0
    ```

  - Azure Stack 1807 oder früher:

    ``` PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet. 
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2017-03-09-profile -Force
    
    # Install Azure Stack Module Version 1.4.0.
    Install-Module -Name AzureStack -RequiredVersion 1.4.0
    ```

  - Azure Stack 1803 oder früher:

    ``` PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet. 
      Install-Module -Name AzureRm.BootStrapper

      # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
      Use-AzureRmProfile -Profile 2017-03-09-profile -Force

      # Install Azure Stack Module Version 1.2.11
      Install-Module -Name AzureStack -RequiredVersion 1.2.11 
    ```

  War die Installation erfolgreich, werden die AzureRM- und AzureStack-Module in der Ausgabe angezeigt.

- **Ohne Internetverbindung** auf dem ASDK-Hostcomputer. In einem Szenario ohne Internetverbindung müssen Sie zuerst mithilfe der folgenden PowerShell-Befehle die PowerShell-Module auf einen Computer mit Internetverbindung herunterladen:

  ```PowerShell
  $Path = "<Path that is used to save the packages>"

  # AzureRM for 1808 requires 2.3.0, for prior versions use 1.2.11
  Save-Package `
    -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
  
  # AzureStack requires 1.5.0 for version 1808, 1.4.0 for versions after 1803, and 1.2.11 for versions before 1803
  Save-Package `
    -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.5.0
  ```

  Kopieren Sie danach die heruntergeladenen Pakete auf den ASDK-Computer, registrieren Sie den Speicherort als Standardrepository, und installieren Sie die Module AzureRM und AzureStack aus diesem Repository:

    ```PowerShell  
    $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
    $RepoName = "MyNuGetSource"

    Register-PSRepository -Name $RepoName -SourceLocation $SourceLocation -InstallationPolicy Trusted

    Install-Module AzureRM -Repository $RepoName

    Install-Module AzureStack -Repository $RepoName
    ```

## <a name="download-the-azure-stack-tools"></a>Herunterladen der Azure Stack-Tools

[AzureStack-Tools](https://github.com/Azure/AzureStack-Tools) ist ein GitHub-Repository, das PowerShell-Module zum Verwalten und Bereitstellen von Ressourcen für Azure Stack hostet. Um diese Tools zu erhalten, klonen Sie das GitHub-Repository, oder laden Sie den Ordner „AzureStack-Tools“ herunter. Führen Sie dazu das folgende Skript aus:

  ```PowerShell
  # Change directory to the root directory.
  cd \

  # Enforce usage of TLSv1.2 to download the Azure Stack tools archive from GitHub
  [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
  invoke-webrequest `
    https://github.com/Azure/AzureStack-Tools/archive/master.zip `
    -OutFile master.zip

  # Expand the downloaded files.
  expand-archive master.zip -DestinationPath . -Force

  # Change to the tools directory.
  cd AzureStack-Tools-master
  ```

## <a name="validate-the-asdk-installation"></a>Überprüfen der ASDK-Installation

Um sicherzustellen, dass Ihre ASDK-Bereitstellung erfolgreich durchgeführt wurde, können Sie folgende Schritte durchführen und das Cmdlet „Test-AzureStack“ verwenden:

1. Melden Sie sich auf dem ASDK-Hostcomputer als „AzureStack\AzureStackAdmin“ an.
2. Starten Sie PowerShell als Administrator (nicht PowerShell ISE).
3. Führen Sie diesen Befehl aus: `Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint`
4. Führen Sie diesen Befehl aus: `Test-AzureStack`

Die Tests dauern einige Minuten. Wenn die Installation erfolgreich war, sieht die Ausgabe in etwa wie folgt aus:

![Test-AzureStack](media/asdk-post-deploy/test-azurestack.png)

Wenn ein Fehler aufgetreten ist, führen Sie die Schritte zur Problembehandlung aus, um Hilfe zu erhalten.

## <a name="reset-the-password-expiration-policy"></a>Zurücksetzen der Kennwortablaufrichtlinie 

Führen Sie nach dem Bereitstellen des ASDK die unten angegebenen Schritte aus, um sicherzustellen, dass das Kennwort für den Development Kit-Host nicht vor dem Ende des Evaluierungszeitraums abläuft.

### <a name="to-change-the-password-expiration-policy-from-powershell"></a>So ändern Sie die Kennwortablaufrichtlinie in Powershell

Führen Sie diesen Befehl über eine PowerShell-Konsole mit erhöhten Rechten aus:

```powershell
Set-ADDefaultDomainPasswordPolicy -MaxPasswordAge 180.00:00:00 -Identity azurestack.local
```

### <a name="to-change-the-password-expiration-policy-manually"></a>So ändern Sie die Kennwortablaufrichtlinie manuell

1. Öffnen Sie auf dem Development Kit-Host die **Gruppenrichtlinien-Verwaltungskonsole** (GPMC.MMC), und navigieren Sie zu **Gruppenrichtlinienverwaltung** – **Gesamtstruktur: azurestack.local** – **Domänen** – **azurestack.local**.
2. Klicken Sie mit der rechten Maustaste auf **Standarddomänenrichtlinie**, und klicken Sie auf **Bearbeiten**.
3. Navigieren Sie im Gruppenrichtlinienverwaltungs-Editor zu **Computerkonfiguration** – **Richtlinien** – **Windows-Einstellungen** – **Sicherheitseinstellungen** – **Kontorichtlinien** – **Kennwortrichtlinie**.
4. Doppelklicken Sie im rechten Bereich auf **Maximales Kennwortalter**.
5. Ändern Sie im Dialogfeld **Maximales Kennwortalter – Eigenschaften** den Wert **Kennwort läuft ab in** zu **180**, und klicken Sie auf **OK**.

![Gruppenrichtlinien-Verwaltungskonsole](media/asdk-post-deploy/gpmc.png)

## <a name="enable-multi-tenancy"></a>Aktivieren der Mehrinstanzenfähigkeit

Für Bereitstellungen mithilfe von Azure AD müssen Sie [Mehrinstanzenfähigkeit](../azure-stack-enable-multitenancy.md#enable-multi-tenancy) für die ASDK-Installation aktivieren.

> [!NOTE]  
> Wenn Administrator- oder Benutzerkonten aus anderen Domänen als derjenigen, die zur Registrierung von Azure Stack verwendet haben, für die Anmeldung an einem Azure Stack-Portal verwendet werden, muss der Domänenname, mit dem Azure Stack registriert wird, an die Portal-URL angefügt werden. Wenn beispielsweise Azure Stack mit „fabrikam.onmicrosoft.com“ registriert wurde und das Benutzerkonto für die Anmeldung admin@contoso.com ist, lautet die URL für die Anmeldung am Benutzerportal wie folgt: https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.

## <a name="next-steps"></a>Nächste Schritte

[Registrieren des ASDK bei Azure](asdk-register.md)
