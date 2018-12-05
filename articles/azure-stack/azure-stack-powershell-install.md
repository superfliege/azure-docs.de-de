---
title: Installieren von PowerShell für Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie PowerShell für Azure Stack installieren.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 09/18/2018
ms.author: sethm
ms.reviewer: thoroet
ms.openlocfilehash: 2e31b9f88857d84bd0b507ccd1622279e72aa575
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2018
ms.locfileid: "52282718"
---
# <a name="install-powershell-for-azure-stack"></a>Installieren von PowerShell für Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Um mit Ihrer Cloud arbeiten zu können, müssen Sie Azure Stack-kompatible PowerShell-Module installieren. Die Kompatibilität wird über die Funktion *API profiles* (API-Profile) aktiviert.

API-Profile bieten eine Möglichkeit, Versionsunterschiede zwischen Azure und Azure Stack zu verwalten. Ein API-Versionsprofil ist eine Gruppe von Azure Resource Manager-PowerShell-Modulen mit spezifischen API-Versionen. Jede Cloudplattform verfügt über eine Gruppe von unterstützten API-Versionsprofilen. Beispielsweise unterstützt Azure Stack eine Profilversion mit einem bestimmten Datum, z.B. **2018-03-01-hybrid**, und Azure unterstützt das **neueste** API-Versionsprofil. Wenn Sie ein Profil installieren, werden die Azure Resource Manager-PowerShell-Module installiert, die dem angegebenen Profil entsprechen.

Sie können mit Azure Stack kompatible PowerShell-Module in einem Szenario mit Internetverbindung, mit teilweiser Internetverbindung oder ohne Internetverbindung installieren. Dieser Artikel enthält detaillierte schrittweise Anleitungen zum Installieren von PowerShell für Azure Stack für diese Szenarien.

## <a name="1-verify-your-prerequisites"></a>1. Überprüfen der Voraussetzungen

Bevor Sie mit Azure Stack und PowerShell beginnen, müssen folgende Voraussetzungen erfüllt sein:

- **PowerShell Version 5.0**: Führen Sie zum Überprüfen Ihrer Version **$PSVersionTable.PSVersion** aus, und vergleichen Sie die **Hauptversion**. Wenn Sie nicht über PowerShell 5.0 verfügen, befolgen Sie die Anweisungen unter [Installieren von Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).

  > [!Note]
  > Für PowerShell 5.0 muss ein Windows-Computer verwendet werden.

- **Ausführen von PowerShell in einer Eingabeaufforderung mit erhöhten Rechten**: Sie müssen PowerShell mit Administratorrechten ausführen.

- **PowerShell-Katalogzugriff**: Sie benötigen Zugriff auf den [PowerShell-Katalog](https://www.powershellgallery.com). Der Katalog ist das zentrale Repository für PowerShell-Inhalte. Das **PowerShellGet**-Modul enthält Cmdlets zum Ermitteln, Installieren, Aktualisieren und Veröffentlichen von PowerShell-Artefakten, z.B. Modulen, DSC-Ressourcen, Rollenfunktionen und Skripts aus dem PowerShell-Katalog und anderen privaten Repositorys. Bei Verwendung von PowerShell in einem Szenario ohne Internetverbindung müssen Sie Ressourcen über einen Computer mit Internetverbindung abrufen und an einem Speicherort speichern, auf den Sie von Ihrem Computer ohne Internetverbindung aus zugreifen können.


## <a name="2-validate-the-powershell-gallery-accessibility"></a>2. Überprüfen der Verfügbarkeit des PowerShell-Katalogs

Überprüfen Sie, ob PSGallery als Repository registriert ist.

> [!Note]
> Für diesen Schritt ist Internetzugriff erforderlich.

Öffnen Sie eine PowerShell-Eingabeaufforderung mit erhöhten Rechten, und führen Sie die folgenden Cmdlets aus:

````PowerShell
Import-Module -Name PowerShellGet -ErrorAction Stop
Import-Module -Name PackageManagement -ErrorAction Stop
Get-PSRepository -Name "PSGallery"
````

Öffnen Sie eine PowerShell-Sitzung mit erhöhten Rechten, und führen Sie den folgenden Befehl aus, falls das Repository nicht registriert ist:

```PowerShell
Register-PsRepository -Default
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

## <a name="3-uninstall-existing-versions-of-the-azure-stack-powershell-modules"></a>3. Deinstallieren vorhandener Versionen der Azure Stack-PowerShell-Module

Deinstallieren Sie vor der Installation der erforderlichen Version unbedingt alle zuvor installierten AzureRM-PowerShell-Module von Azure Stack. Verwenden Sie zum Deinstallieren eine folgenden Methoden:

1. Schließen Sie alle aktiven PowerShell-Sitzungen, und führen Sie die folgenden Cmdlets aus, um die vorhandenen AzureRM-PowerShell-Module zu deinstallieren:

    ````PowerShell
    Uninstall-Module -Name AzureRM.AzureStackAdmin -Force 
    Uninstall-Module -Name AzureRM.AzureStackStorage -Force 
    Uninstall-Module -Name AzureStack -Force -Verbose
    Uninstall-Module -Name AzureRM -Force -Verbose
    Uninstall-Module -Name Azure.Storage -Force -Verbose
    Get-Module -Name Azs.* -ListAvailable | Uninstall-Module -Force -Verbose
    Get-Module -Name AzureRM.* -ListAvailable | Uninstall-Module -Force -Verbose
    ````
    Wenn eine Fehlermeldung wie „Das Modul wird bereits verwendet“ auftritt, schließen Sie die PowerShell-Sitzungen, die die Module verwenden, und führen Sie das obige Skript erneut aus.

2. Löschen Sie aus den Ordnern `C:\Program Files\WindowsPowerShell\Modules` und `C:\Users\{yourusername}\Documents\WindowsPowerShell\Modules` alle Ordner, die mit `Azure` beginnen. Beim Löschen dieser Ordner werden alle vorhandenen PowerShell-Module entfernt.

## <a name="4-connected-install-powershell-for-azure-stack-with-internet-connectivity"></a>4. Verbunden: Installieren von PowerShell für Azure Stack mit Internetverbindung

Azure Stack erfordert das API-Versionsprofil **2018-03-01-hybrid** für die Azure Stack-Version 1808. Das Profil wird durch die Installation des **AzureRM.Bootstrapper**-Moduls verfügbar. Zusätzlich zu den AzureRM-Modulen müssen die Azure Stack-spezifischen PowerShell-Module installiert werden. Das API-Versionsprofil und die Azure Stack-PowerShell-Module, die Sie benötigen, hängen von der Version von Azure Stack ab, die Sie ausführen.

Führen Sie zum Installieren dieser Module auf der Entwicklungsarbeitsstation das folgende PowerShell-Skript aus:

  - Azure Stack 1808 oder höher.

    ```PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    Install-Module -Name AzureStack -RequiredVersion 1.5.0
    ```

> [!Note]
> Um ein Upgrade von Azure PowerShell vom **2017-03-09-Profil** auf **2018-03-01-hybrid** auszuführen, lesen Sie den [Migrationsleitfaden](https://github.com/bganapa/azure-powershell/blob/migration-guide/documentation/migration-guides/Stack/migration-guide.2.3.0.md).

  - Azure Stack 1807 oder früher.

    ```PowerShell
    Install-Module -Name AzureRm.BootStrapper
    Use-AzureRmProfile -Profile 2017-03-09-profile -Force
    Install-Module -Name AzureStack -RequiredVersion 1.4.0
    ```

  - Azure Stack 1804 oder früher.

    ```PowerShell
    Install-Module -Name AzureRm.BootStrapper
    Use-AzureRmProfile -Profile 2017-03-09-profile -Force
    Install-Module -Name AzureStack -RequiredVersion 1.2.11
    ```

Führen Sie den folgenden Befehl aus, um die Installation zu überprüfen:

```PowerShell
Get-Module -Name "Azure*" -ListAvailable
Get-Module -Name "Azs*" -ListAvailable
```

War die Installation erfolgreich, werden die AzureRM- und AzureStack-Module in der Ausgabe angezeigt.

## <a name="5-disconnected-install-powershell-without-an-internet-connection"></a>5. Nicht verbunden: Installieren von PowerShell ohne Internetverbindung

In einem nicht verbundenen Szenario müssen Sie zuerst die PowerShell-Module auf einen Computer mit Internetverbindung herunterladen und sie dann für die Installation in das Azure Stack Development Kit übertragen.

Melden Sie sich bei einem Computer mit Internetverbindung an, und verwenden Sie die folgenden Skripts, um die Pakete „Azure Resource Manager“ und „AzureStack“ abhängig von Ihrer Version von Azure Stack herunterzuladen:

  - Azure Stack 1808 oder höher.

    ````PowerShell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

      $Path = "<Path that is used to save the packages>"
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.5.0
    ````

  - Azure Stack 1807 oder früher.

    > [!Note]
    Informationen zum Upgrade von Version 1.2.11 finden Sie im [Migrationshandbuch](https://aka.ms/azspowershellmigration).

    ````PowerShell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 1.2.11
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.4.0
    ````

  - Azure Stack 1804 oder früher.

    ````PowerShell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 1.2.11
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.3.0
    ````

2. Kopieren Sie die heruntergeladenen Pakete auf ein USB-Gerät.

3. Melden Sie sich an der Arbeitsstation an, und kopieren Sie die Pakete vom USB-Gerät an einen Speicherort auf der Arbeitsstation.

4. Nun registrieren Sie diesen Speicherort als Standardrepository und installieren die AzureRM- und AzureStack-Module aus diesem Repository:

   ```PowerShell
   #requires -Version 5
   #requires -RunAsAdministrator
   #requires -Module PowerShellGet
   #requires -Module PackageManagement

   $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
   $RepoName = "MyNuGetSource"

   Register-PSRepository -Name $RepoName -SourceLocation $SourceLocation  -InstallationPolicy Trusted

   Install-Module -Name AzureRM -Repository $RepoName

   Install-Module -Name AzureStack -Repository $RepoName
   ```

## <a name="6-configure-powershell-to-use-a-proxy-server"></a>6. Konfigurieren von PowerShell für die Verwendung eines Proxyservers

In Szenarien, für die ein Proxyserver für den Zugriff auf das Internet erforderlich ist, müssen Sie zuerst PowerShell für die Verwendung eines vorhandenen Proxyservers konfigurieren:

1. Öffnen Sie eine PowerShell-Eingabeaufforderung mit erhöhten Rechten.
2. Führen Sie die folgenden Befehle aus:

   ```PowerShell
   #To use Windows credentials for proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials

   #Alternatively, to prompt for separate credentials that can be used for #proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = Get-Credential
   ```

## <a name="next-steps"></a>Nächste Schritte

 - [Herunterladen von Azure Stack-Tools von GitHub](azure-stack-powershell-download.md)
 - [Konfigurieren der PowerShell-Umgebung des Azure Stack-Benutzers](user/azure-stack-powershell-configure-user.md)
 - [Konfigurieren der PowerShell-Umgebung des Azure Stack-Betreibers](azure-stack-powershell-configure-admin.md)
 - [Verwalten von API-Versionsprofilen in Azure Stack](user/azure-stack-version-profiles.md)
