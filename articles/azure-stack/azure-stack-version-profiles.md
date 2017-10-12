---
title: Verwenden von API-Versionsprofilen in Azure Stack | Microsoft-Dokumentation
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
ms.date: 09/25/2017
ms.author: sngun
ms.openlocfilehash: b9a010409dc7f49333ab188b89280f3ad54816c1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="manage-api-version-profiles-in-azure-stack"></a>Verwalten von API-Versionsprofilen in Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

API-Versionsprofile bieten eine Möglichkeit, Versionsunterschiede zwischen Azure und Azure Stack zu verwalten. Ein API-Versionsprofil ist eine Gruppe von AzureRM-PowerShell-Modulen mit spezifischen API-Versionen. Jede Cloudplattform verfügt über eine Gruppe von unterstützten API-Versionsprofilen. Beispielsweise unterstützt Azure Stack eine Profilversion mit einem bestimmten Datum, z.B. **2017-03-09-profile**, und Azure unterstützt das **aktuelle** API-Versionsprofil. Wenn Sie ein Profil installieren, werden die AzureRM-PowerShell-Module installiert, die dem angegebenen Profil entsprechen.

## <a name="install-the-powershell-module-required-to-use-api-version-profiles"></a>Installieren des für die Nutzung von API-Versionsprofilen erforderlichen PowerShell-Moduls

Das über den PowerShell-Katalog verfügbare Modul **AzureRM.Bootstrapper** stellt PowerShell-Cmdlets bereit, die für die Arbeit mit API-Versionsprofilen erforderlich sind. Verwenden Sie das folgende Cmdlet, um das Modul „AzureRM.Bootstrapper“ zu installieren:

```PowerShell
Install-Module -Name AzureRm.BootStrapper
```
Das Modul „AzureRM.Bootstrapper“ befindet sich in der Vorschauphase. Details und Funktionen können sich ändern. Um die aktuelle Version dieses Moduls aus dem PowerShell-Katalog herunterzuladen und zu installieren, führen Sie das folgende Cmdlet aus:

```PowerShell
Update-Module -Name "AzureRm.BootStrapper"
```

## <a name="install-a-profile"></a>Installieren eines Profils

Verwenden Sie das Cmdlet **Install-AzureRmProfile** mit dem API-Versionsprofil **2017-03-09-profile**, um die von Azure Stack benötigten AzureRM-Module zu installieren. Beachten Sie, dass die Azure Stack-Betreibermodule nicht mit diesem API-Versionsprofil installiert werden. Sie sollten wie in Schritt 3 des Artikels [Install PowerShell for Azure Stack (Installieren von PowerShell für Azure Stack)](azure-stack-powershell-install.md) separat installiert werden.

```PowerShell 
Install-AzureRMProfile -Profile 2017-03-09-profile
```
## <a name="install-and-import-modules-in-a-profile"></a>Installieren und Importieren von Modulen in einem Profil

Verwenden Sie das Cmdlet **Use-AzureRmProfile**, um Module zu installieren und zu importieren, die einem API-Versionsprofil zugeordnet sind. Sie können nur ein API-Versionsprofil pro PowerShell-Sitzung importieren. Um ein weiteres API-Versionsprofil zu importieren, müssen Sie eine neue PowerShell-Sitzung öffnen. Das Cmdlet „Use-AzureRMProfile“ führt die folgenden Schritte durch:  
1. Es überprüft, ob die dem angegebenen API-Versionsprofil zugeordneten PowerShell-Module im aktuellen Bereich installiert wurden.  
2. Es lädt die Module herunter und installiert diese, falls sie noch nicht installiert wurden.   
3. Es importiert die Module in die aktuelle PowerShell-Sitzung. 

```PowerShell
# Installs and imports the specified API version profile into the current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser

# Installs and imports the specified API version profile into the current PowerShell session without any prompts
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser -Force
```

Um die ausgewählten AzureRM-Module aus einem API-Versionsprofil zu installieren und zu importieren, führen Sie das Cmdlet „Use-AzureRMProfile“ mit dem Parameter **Module** aus:

```PowerShell
# Installs and imports the compute, Storage and Network modules from the specified API version profile into your current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Module AzureRM.Compute, AzureRM.Storage, AzureRM.Network
```

## <a name="get-the-installed-profiles"></a>Abrufen der installierten Profile

Verwenden Sie das Cmdlet **Get-AzureRmProfile**, um die Liste der verfügbaren API-Versionsprofile abzurufen: 

```PowerShell
# lists all API version profiles provided by the AzureRM.BootStrapper module.
Get-AzureRmProfile -ListAvailable 

# lists the API version profiles which are installed on your machine
Get-AzureRmProfile
```
## <a name="update-profiles"></a>Aktualisieren von Profilen

Verwenden Sie das Cmdlet **Update-AzureRmProfile**, um die Module in einem API-Versionsprofil auf die aktuelle Version der Module zu aktualisieren, die im PowerShell-Katalog verfügbar sind. Es wird empfohlen, das Cmdlet **Update-AzureRmProfile** immer in einer neuen PowerShell-Sitzung auszuführen, um Konflikte beim Importieren von Modulen zu vermeiden. Das Cmdlet „Update-AzureRmProfile“ führt die folgenden Schritte durch:

1. Es überprüft, ob die aktuellen Versionen der Module im angegebenen API-Versionsprofil für den aktuellen Bereich installiert wurden.  
2. Es fordert Sie dazu auf, diese zu installieren, sofern sie noch nicht installiert wurden.  
3. Es installiert und importiert die aktualisierten Module in die aktuelle PowerShell-Sitzung.  

```PowerShell
Update-AzureRmProfile -Profile 2017-03-09-profile
```

Um die zuvor installierten Versionen der Module vor dem Update auf die aktuell verfügbare Version zu entfernen, verwenden Sie das Cmdlet „Update-AzureRmProfile“ zusammen mit dem Parameter **-RemovePreviousVersions**:

```PowerShell 
Update-AzureRmProfile -Profile 2017-03-09-profile -RemovePreviousVersions
```

Dieses Cmdlet führt die folgenden Schritte durch:  

1. Es überprüft, ob die aktuellen Versionen der Module im angegebenen API-Versionsprofil für den aktuellen Bereich installiert wurden.  
2. Es entfernt die älteren Versionen der Module aus dem aktuellen API-Versionsprofil und in der aktuellen PowerShell-Sitzung.  
4. Es fordert Sie zur Installation der aktuellen Version auf.  
5. Es installiert und importiert die aktualisierten Module in die aktuelle PowerShell-Sitzung.  
 
## <a name="uninstall-profiles"></a>Deinstallieren von Profilen

Verwenden Sie das Cmdlet **Uninstall-AzureRmProfile**, um das angegebene API-Versionsprofil zu deinstallieren.

```PowerShell 
Uninstall-AzureRmProfile -Profile 2017-03-09-profile
```

## <a name="next-steps"></a>Nächste Schritte
* [Installieren von PowerShell für Azure Stack](azure-stack-powershell-install.md)
* [Konfigurieren der PowerShell-Umgebung des Azure Stack-Benutzers](user/azure-stack-powershell-configure-user.md)  
