---
title: Installieren von PowerShell für Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie PowerShell für Azure Stack installieren.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 07/10/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: e2785b0beeab042d4b1ad9a9eb5f545dbb58b8b9
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38487500"
---
# <a name="install-powershell-for-azure-stack"></a>Installieren von PowerShell für Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Für die Verwendung von Azure Stack sind mit Azure Stack kompatible Azure PowerShell-Module erforderlich. In diesem Handbuch werden die Schritte erläutert, die zum Installieren von PowerShell für Azure Stack erforderlich sind.

Dieser Artikel enthält ausführliche Anweisungen zur Installation von PowerShell für Azure Stack.

> [!Note]  
> Für die folgenden Schritte ist PowerShell 5.0 erforderlich. Führen Sie zum Überprüfen Ihrer Version „$PSVersionTable.PSVersion“ aus, und vergleichen Sie die **Hauptversion**.

PowerShell-Befehle für Azure Stack werden über den PowerShell-Katalog installiert. Sie können das folgende Verfahren verwenden, um zu überprüfen, ob PSGallery als Repository registriert ist, eine PowerShell-Sitzung mit erhöhten Rechten öffnen und den folgenden Befehl ausführen:

```PowerShell  
Get-PSRepository -Name "PSGallery"
```

Öffnen Sie eine PowerShell-Sitzung mit erhöhten Rechten, und führen Sie den folgenden Befehl aus, falls das Repository nicht registriert ist:

```PowerShell  
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```
> [!Note]  
> Für diesen Schritt ist Internetzugriff erforderlich. 

## <a name="uninstall-existing-versions-of-the-azure-stack-powershell-modules"></a>Deinstallieren vorhandener Versionen der Azure Stack-PowerShell-Module

Deinstallieren Sie vor der Installation der erforderlichen Version unbedingt alle zuvor installierten AzureRM-PowerShell-Module von Azure Stack. Verwenden Sie zum Deinstallieren eine folgenden Methoden:

 - Schließen Sie alle aktiven AzureRM-PowerShell-Module, und führen Sie den folgenden Befehl aus, um die vorhandenen PowerShell-Module zu deinstallieren:

  ```PowerShell
    Uninstall-Module AzureRM.AzureStackAdmin -Force
    Uninstall-Module AzureRM.AzureStackStorage -Force
    Uninstall-Module -Name AzureStack -Force
  ```

 - Löschen Sie aus den Ordnern `C:\Program Files\WindowsPowerShell\Modules` und `C:\Users\AzureStackAdmin\Documents\WindowsPowerShell\Modules` alle Ordner, die mit „Azure“ beginnen. Beim Löschen dieser Ordner werden alle vorhandenen PowerShell-Module entfernt.

In den folgenden Abschnitten werden die Schritte beschrieben, die zum Installieren von PowerShell für Azure Stack erforderlich sind. PowerShell kann in Azure Stack-Instanzen installiert werden, die in verbundenen, partiell verbundenen oder nicht verbundenen Szenarien betrieben werden.

## <a name="install-the-azure-stack-powershell-modules-in-a-connected-scenario-with-internet-connectivity"></a>Installieren der Azure Stack-PowerShell-Module in einem verbundenen Szenario (mit Internetverbindung)

Mit Azure Stack kompatible AzureRM-Module werden über API-Versionsprofile installiert. Für Azure Stack ist das API-Versionsprofil **2017-03-09-profile** erforderlich, das durch Installation des AzureRM.Bootstrapper-Moduls zur Verfügung gestellt wird. Informationen zu API-Versionsprofilen und den von ihnen bereitgestellten Cmdlets finden Sie unter [Manage API version profiles in Azure Stack](user/azure-stack-version-profiles.md) (Verwalten von API-Versionsprofilen in Azure Stack). Zusätzlich zu den AzureRM-Modulen müssen die Azure Stack-spezifischen PowerShell-Module installiert werden. Führen Sie zum Installieren dieser Module auf der Entwicklungsarbeitsstation das folgende PowerShell-Skript aus:

  ```PowerShell  
# Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet 
Install-Module -Name AzureRm.BootStrapper 

# Install and import the API Version Profile required by Azure Stack into the current PowerShell session. 
Use-AzureRmProfile -Profile 2017-03-09-profile -Force 

# Install Module Version 1.3.0 if Azure Stack is running 1804 at a minimum 
Install-Module -Name AzureStack -RequiredVersion 1.3.0 

# Install Module Version 1.2.11 if Azure Stack is running a lower version than 1804 
Install-Module -Name AzureStack -RequiredVersion 1.2.11 
  ```

Führen Sie den folgenden Befehl aus, um die Installation zu überprüfen:

```PowerShell  
Get-Module -ListAvailable | where-Object {$_.Name -like "Azs*"}
```

War die Installation erfolgreich, werden die AzureRM- und AzureStack-Module in der Ausgabe angezeigt.

## <a name="install-the-azure-stack-powershell-modules-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity"></a>Installieren der Azure Stack-PowerShell-Module in einem nicht verbundenen oder partiell verbundenen Szenario (mit eingeschränkter Internetverbindung)

In einem nicht verbundenen Szenario müssen Sie zuerst die PowerShell-Module auf einen Computer mit Internetverbindung herunterladen und sie dann für die Installation in das Azure Stack Development Kit übertragen.

> [!IMPORTANT]  
> Das Release des PowerShell-Moduls Azure Stack 1.3.0 enthält eine Liste mit wichtigen Änderungen. Informationen zum Upgrade von Version 1.2.11 finden Sie im [Migrationshandbuch](https://aka.ms/azspowershellmigration).

1. Melden Sie sich bei einem Computer mit Internetverbindung an, und laden Sie mithilfe des folgenden Skripts die AzureRM- und AzureStack-Pakete auf den lokalen Computer herunter:

   ```PowerShell  
   $Path = "<Path that is used to save the packages>"

   Save-Package `
     -ProviderName NuGet `
     -Source https://www.powershellgallery.com/api/v2 `
     -Name AzureRM `
     -Path $Path `
     -Force `
     -RequiredVersion 1.2.11

   Save-Package `
     -ProviderName NuGet `
     -Source https://www.powershellgallery.com/api/v2 `
     -Name AzureStack `
     -Path $Path `
     -Force `
     -RequiredVersion 1.3.0 
   ```

  > [!Important]  
  > Ändern Sie den Parameter **requiredversion** in `1.2.11`, wenn Sie Azure Stack nicht mit Update 1804 oder höher ausführen. 

2. Kopieren Sie die heruntergeladenen Pakete auf ein USB-Gerät.

3. Melden Sie sich an der Arbeitsstation an, und kopieren Sie die Pakete vom USB-Gerät an einen Speicherort auf der Arbeitsstation.

4. Nun müssen Sie diesen Speicherort als Standardrepository registrieren und die AzureRM- und AzureStack-Module aus diesem Repository installieren:

   ```PowerShell
   $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
   $RepoName = "MyNuGetSource"

   Register-PSRepository `
     -Name $RepoName `
     -SourceLocation $SourceLocation `
     -InstallationPolicy Trusted

   Install-Module AzureRM `
     -Repository $RepoName

   Install-Module AzureStack `
     -Repository $RepoName 
   ```

## <a name="configure-powershell-to-use-a-proxy-server"></a>Konfigurieren von PowerShell für die Verwendung eines Proxyservers

In Szenarien, für die ein Proxyserver für den Zugriff auf das Internet erforderlich ist, müssen Sie zuerst PowerShell für die Verwendung eines vorhandenen Proxyservers konfigurieren.

1. Öffnen Sie eine PowerShell-Eingabeaufforderung mit erhöhten Rechten.
2. Führen Sie die folgenden Befehle aus:

````PowerShell  
  #To use Windows credentials for proxy authentication
  [System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials

  #Alternatively, to prompt for separate credentials that can be used for #proxy authentication

  [System.Net.WebRequest]::DefaultWebProxy.Credentials = Get-Credential
````

## <a name="next-steps"></a>Nächste Schritte

 - [Herunterladen von Azure Stack-Tools von GitHub](azure-stack-powershell-download.md)
 - [Konfigurieren der PowerShell-Umgebung des Azure Stack-Benutzers](user/azure-stack-powershell-configure-user.md)  
 - [Konfigurieren der PowerShell-Umgebung des Azure Stack-Betreibers](azure-stack-powershell-configure-admin.md) 
 - [Verwalten von API-Versionsprofilen in Azure Stack](user/azure-stack-version-profiles.md)  
