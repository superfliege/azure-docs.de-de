---
title: Verwalten von API-Versionsprofilen in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie mehr zu API-Versionsprofilen in Azure Stack.
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: sngun
ms.openlocfilehash: f986abebaf9117b040c149c10f6b2358bbdc0f44
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2017
---
# <a name="manage-api-version-profiles-in-azure-stack"></a>Verwalten von API-Versionsprofilen in Azure Stack

Die API-Funktion von Azure App Service-Versionsprofilen bietet eine Möglichkeit, Versionsunterschiede zwischen Azure und Azure Stack zu verwalten. Ein API-Versionsprofil ist eine Gruppe von AzureRM-PowerShell-Modulen mit spezifischen API-Versionen. Jede Cloudplattform verfügt über eine Gruppe von unterstützten API-Versionsprofilen. Beispielsweise unterstützt Azure Stack eine Profilversion mit einem bestimmten Datum, z.B. **2017-03-09-profile**, und Azure unterstützt das *aktuelle* API-Versionsprofil. Wenn Sie ein Profil installieren, werden die AzureRM-PowerShell-Module installiert, die dem angegebenen Profil entsprechen.

## <a name="install-the-powershell-module-required-to-use-api-version-profiles"></a>Installieren des für die Nutzung von API-Versionsprofilen erforderlichen PowerShell-Moduls

Das über den PowerShell-Katalog verfügbare Modul **AzureRM.Bootstrapper** stellt PowerShell-Cmdlets bereit, die für die Arbeit mit API-Versionsprofilen erforderlich sind. Verwenden Sie das folgende Cmdlet, um das Modul **AzureRM.Bootstrapper** zu installieren:

```PowerShell
Install-Module -Name AzureRm.BootStrapper
```
Das Modul **AzureRM.Bootstrapper** befindet sich in der Vorschauversion, weshalb sich die Details und Funktionen ändern können. Um die aktuelle Version dieses Moduls aus dem PowerShell-Katalog herunterzuladen und zu installieren, führen Sie das folgende Cmdlet aus:

```PowerShell
Update-Module -Name "AzureRm.BootStrapper"
```

## <a name="install-a-profile"></a>Installieren eines Profils

Verwenden Sie das Cmdlet **Install-AzureRmProfile** mit dem API-Versionsprofil **2017-03-09-profile**, um die von Azure Stack benötigten AzureRM-Module zu installieren. 

>[!NOTE]
>Die Azure Stack-Cloudadministratormodule werden nicht mit diesem API-Versionsprofil installiert. Die Administratormodule sollten separat installiert werden, wie in Schritt 3 des Artikels [Installieren von PowerShell für Azure Stack](azure-stack-powershell-install.md) beschrieben wird.

```PowerShell 
Install-AzureRMProfile -Profile 2017-03-09-profile
```
## <a name="install-and-import-modules-in-a-profile"></a>Installieren und Importieren von Modulen in einem Profil

Verwenden Sie das Cmdlet **Use-AzureRmProfile**, um Module zu installieren und zu importieren, die einem API-Versionsprofil zugeordnet sind. Sie können nur ein API-Versionsprofil pro PowerShell-Sitzung importieren. Um ein weiteres API-Versionsprofil zu importieren, müssen Sie eine neue PowerShell-Sitzung öffnen. Das Cmdlet **Use-AzureRMProfile** führt die folgenden Aufgaben durch:  
1. Es überprüft, ob die dem angegebenen API-Versionsprofil zugeordneten PowerShell-Module im aktuellen Bereich installiert wurden.  
2. Es lädt die Module herunter und installiert diese, falls sie noch nicht installiert wurden.   
3. Es importiert die Module in die aktuelle PowerShell-Sitzung. 

```PowerShell
# Installs and imports the specified API version profile into the current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser

# Installs and imports the specified API version profile into the current PowerShell session without any prompts.
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser -Force
```

Um die ausgewählten AzureRM-Module aus einem API-Versionsprofil zu installieren und zu importieren, führen Sie das Cmdlet **Use-AzureRMProfile** mit dem Parameter *Module* aus:

```PowerShell
# Installs and imports the Compute, Storage, and Network modules from the specified API version profile into your current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Module AzureRM.Compute, AzureRM.Storage, AzureRM.Network
```

## <a name="get-the-installed-profiles"></a>Abrufen der installierten Profile

Verwenden Sie das Cmdlet **Get-AzureRmProfile**, um die Liste der verfügbaren API-Versionsprofile abzurufen: 

```PowerShell
# Lists all API version profiles provided by the AzureRM.BootStrapper module.
Get-AzureRmProfile -ListAvailable 

# Lists the API version profiles that are installed on your machine.
Get-AzureRmProfile
```
## <a name="update-profiles"></a>Aktualisieren von Profilen

Verwenden Sie das Cmdlet **Update-AzureRmProfile**, um die Module in einem API-Versionsprofil auf die aktuelle Version der Module zu aktualisieren, die im PowerShell-Katalog verfügbar sind. Es wird empfohlen, das Cmdlet **Update-AzureRmProfile** in einer neuen PowerShell-Sitzung auszuführen, um Konflikte beim Importieren von Modulen zu vermeiden. Das Cmdlet **Update-AzureRmProfile** führt die folgenden Aufgaben durch:

1. Es überprüft, ob die aktuellen Versionen der Module im angegebenen API-Versionsprofil für den aktuellen Bereich installiert wurden.  
2. Es fordert Sie dazu auf, die Module zu installieren, sofern sie noch nicht installiert wurden.  
3. Es installiert und importiert die aktualisierten Module in die aktuelle PowerShell-Sitzung.  

```PowerShell
Update-AzureRmProfile -Profile 2017-03-09-profile
```

Um die zuvor installierten Versionen der Module vor dem Aktualisieren auf die aktuell verfügbare Version zu entfernen, verwenden Sie das Cmdlet **Update-AzureRmProfile** zusammen mit dem Parameter *-RemovePreviousVersions*:

```PowerShell 
Update-AzureRmProfile -Profile 2017-03-09-profile -RemovePreviousVersions
```

Dieses Cmdlet führt die folgenden Schritte durch:  

1. Es überprüft, ob die aktuellen Versionen der Module im angegebenen API-Versionsprofil für den aktuellen Bereich installiert wurden.  
2. Es entfernt die älteren Versionen der Module aus dem aktuellen API-Versionsprofil und in der aktuellen PowerShell-Sitzung.  
3. Es fordert Sie zur Installation der aktuellen Version der Module auf.  
4. Es installiert und importiert die aktualisierten Module in die aktuelle PowerShell-Sitzung.  
 
## <a name="uninstall-profiles"></a>Deinstallieren von Profilen

Verwenden Sie das Cmdlet **Uninstall-AzureRmProfile**, um das angegebene API-Versionsprofil zu deinstallieren:

```PowerShell 
Uninstall-AzureRmProfile -Profile 2017-03-09-profile
```

## <a name="next-steps"></a>Nächste Schritte
* [Installieren von PowerShell für Azure Stack](azure-stack-powershell-install.md)
* [Konfigurieren der PowerShell-Umgebung des Azure Stack-Benutzers](azure-stack-powershell-configure-user.md)  
