---
title: Verwenden von API-Versionsprofilen mit PowerShell in Azure Stack | Microsoft-Dokumentation
description: Hier erfahren Sie mehr zur Verwendung von API-Versionsprofilen mit PowerShell in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: EBAEA4D2-098B-4B5A-A197-2CEA631A1882
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: f6fd560d6ecb9feaa3df14b288cee5d98ce55aee
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54391096"
---
# <a name="use-api-version-profiles-for-powershell-in-azure-stack"></a>Verwenden von API-Versionsprofilen für PowerShell in Azure Stack

*Anwendungsbereich: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

API-Versionsprofile bieten eine Möglichkeit, Versionsunterschiede zwischen Azure und Azure Stack zu verwalten. Ein API-Versionsprofil ist eine Gruppe von AzureRM-PowerShell-Modulen mit spezifischen API-Versionen. Jede Cloudplattform verfügt über eine Gruppe von unterstützten API-Versionsprofilen. Beispielsweise unterstützt Azure Stack eine Profilversion mit einem bestimmten Datum, z.B. **2018-03-01-hybrid**, und Azure unterstützt das **neueste** API-Versionsprofil. Wenn Sie ein Profil installieren, werden die AzureRM-PowerShell-Module installiert, die dem angegebenen Profil entsprechen.

## <a name="install-the-powershell-module-required-to-use-api-version-profiles"></a>Installieren des für die Nutzung von API-Versionsprofilen erforderlichen PowerShell-Moduls

Das über den PowerShell-Katalog verfügbare Modul **AzureRM.Bootstrapper** stellt PowerShell-Cmdlets bereit, die für die Arbeit mit API-Versionsprofilen erforderlich sind. Verwenden Sie das folgende Cmdlet, um das Modul **AzureRM.Bootstrapper** zu installieren:

```PowerShell
Install-Module -Name AzureRm.BootStrapper
```

## <a name="azure-stack-version-and-profile-versions"></a>Azure Stack-Version und Profilversionen

In der folgenden Tabelle werden die erforderliche API-Profilversion und der PowerShell-Administratormodulmoniker aufgeführt, die für die neuesten Versionen von Azure Stack verwendet werden. Wenn Sie diesen Artikel mit einer Version vor 1808 verwenden, aktualisieren Sie Ihr Versionsprofil und Ihren Moniker mit dem richtigen Wert.

| Versionsnr. | API-Versionsprofil | PS-Administratormodulmoniker |
| --- | --- | --- |
| 1811 oder höher | 2018-03-01-hybrid | 1.6.0 |
| 1808 oder höher | 2018-03-01-hybrid | 1.5.0 |
| 1804 oder höher | 2017-03-09-profile | 1.4.0 |
| Versionen vor 1804 | 2017-03-09-profile | 1.2.11 |

> [!NOTE]  
> Informationen zum Upgrade von Version 1.2.11 finden Sie im [Migrationshandbuch](https://aka.ms/azpsh130migration).

## <a name="install-a-profile"></a>Installieren eines Profils

Verwenden Sie das Cmdlet **Install-AzureRmProfile** mit dem API-Versionsprofil **2018-03-01-hybrid**, um die von Azure Stack benötigten AzureRM-Module zu installieren. Die Azure Stack-Betreibermodule werden nicht mit diesem API-Versionsprofil installiert. Sie müssen separat installiert werden, wie in Schritt 3 des Artikels [Installieren von PowerShell für Azure Stack](../azure-stack-powershell-install.md) beschrieben wird.

```PowerShell
Install-AzureRMProfile -Profile 2018-03-01-hybrid
```

## <a name="install-and-import-modules-in-a-profile"></a>Installieren und Importieren von Modulen in einem Profil

Verwenden Sie das Cmdlet **Use-AzureRmProfile**, um Module zu installieren und zu importieren, die einem API-Versionsprofil zugeordnet sind. Sie können nur ein API-Versionsprofil pro PowerShell-Sitzung importieren. Um ein weiteres API-Versionsprofil zu importieren, müssen Sie eine neue PowerShell-Sitzung öffnen. Das Cmdlet **Use-AzureRMProfile** führt die folgenden Aufgaben durch:

1. Es überprüft, ob die dem angegebenen API-Versionsprofil zugeordneten PowerShell-Module im aktuellen Bereich installiert wurden.  
2. Es lädt die Module herunter und installiert diese, falls sie noch nicht installiert wurden.
3. Es importiert die Module in die aktuelle PowerShell-Sitzung.

```PowerShell
# Installs and imports the specified API version profile into the current PowerShell session.
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Scope CurrentUser

# Installs and imports the specified API version profile into the current PowerShell session without any prompts
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Scope CurrentUser -Force
```

Um die ausgewählten AzureRM-Module aus einem API-Versionsprofil zu installieren und zu importieren, führen Sie das Cmdlet **Use-AzureRMProfile** mit dem Parameter **Module** aus:

```PowerShell
# Installs and imports the compute, storage and network modules from the specified API version profile into your current PowerShell session.
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Module AzureRM.Compute, AzureRM.Storage, AzureRM.Network
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

Verwenden Sie das Cmdlet **Update-AzureRmProfile**, um die Module in einem API-Versionsprofil auf die aktuelle Version der Module zu aktualisieren, die im PowerShell-Katalog verfügbar sind. Sie sollten das Cmdlet **Update-AzureRmProfile** immer in einer neuen PowerShell-Sitzung auszuführen, um Konflikte beim Importieren von Modulen zu vermeiden. Das Cmdlet **Update-AzureRmProfile** führt die folgenden Aufgaben durch:

1. Es überprüft, ob die aktuellen Versionen der Module im angegebenen API-Versionsprofil für den aktuellen Bereich installiert wurden.  
2. Es fordert Sie dazu auf, diese zu installieren, sofern sie noch nicht installiert wurden.  
3. Es installiert und importiert die aktualisierten Module in die aktuelle PowerShell-Sitzung.  

```PowerShell
Update-AzureRmProfile -Profile 2018-03-01-hybrid
```

<!-- To remove the previously installed versions of the modules before updating to the latest available version, use the Update-AzureRmProfile cmdlet along with the **-RemovePreviousVersions** parameter:

```PowerShell 
Update-AzureRmProfile -Profile 2018-03-01-hybrid -RemovePreviousVersions
``` -->

Dieses Cmdlet führt die folgenden Schritte durch:  

1. Es überprüft, ob die aktuellen Versionen der Module im angegebenen API-Versionsprofil für den aktuellen Bereich installiert wurden.  
2. Es entfernt die älteren Versionen der Module aus dem aktuellen API-Versionsprofil und in der aktuellen PowerShell-Sitzung.  
3. Es fordert Sie zur Installation der aktuellen Version auf.  
4. Es installiert und importiert die aktualisierten Module in die aktuelle PowerShell-Sitzung.  

## <a name="uninstall-profiles"></a>Deinstallieren von Profilen

Verwenden Sie das Cmdlet **Uninstall-AzureRmProfile**, um das angegebene API-Versionsprofil zu deinstallieren.

```PowerShell
Uninstall-AzureRmProfile -Profile  2018-03-01-hybrid
```

## <a name="next-steps"></a>Nächste Schritte

* [Installieren von PowerShell für Azure Stack](azure-stack-powershell-install.md)
* [Konfigurieren der PowerShell-Umgebung des Azure Stack-Benutzers](azure-stack-powershell-configure-user.md)  
