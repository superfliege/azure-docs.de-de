---
title: Azure Cloud Shell für Windows-Benutzer | Microsoft-Dokumentation
description: Anleitung für Benutzer, die mit Linux-Systemen nicht vertraut sind
services: azure
documentationcenter: ''
author: maertendMSFT
manager: hemantm
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/03/2018
ms.author: damaerte
ms.openlocfilehash: 5e318a0f64033aa0c4b306e547c11e1994afa229
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37861223"
---
# <a name="powershell-in-azure-cloud-shell-for-windows-users"></a>PowerShell in Azure Cloud Shell für Windows-Benutzer

Im Mai 2018 wurden Änderungen für PowerShell in Azure Cloud Shell [angekündigt](https://azure.microsoft.com/blog/pscloudshellrefresh/).  Als PowerShell-Oberfläche in Azure Cloud Shell wird jetzt PowerShell Core 6 unter Linux verwendet.
Mit dieser Änderung können sich einige Aspekte von PowerShell in Cloud Shell unter Umständen von dem unterscheiden, was in Windows PowerShell 5.1 erwartet wird.

## <a name="case-sensitivity"></a>Groß- und Kleinschreibung

Unter Windows wird im Dateisystem die Groß- und Kleinschreibung nicht beachtet.  Unter Linux wird im Dateisystem dagegen die Groß- und Kleinschreibung beachtet.
Das bedeutet, dass `file.txt` und `FILE.txt` zuvor als dieselbe Datei galten, jetzt jedoch als unterschiedliche Dateien gelten.
Bei einer `tab`-Vervollständigung im Dateisystem muss die Groß- und Kleinschreibung richtig verwendet werden.  Bei PowerShell-spezifischen Elementen, z.B. `tab`-Cmdlets, wird die Groß- und Kleinschreibung nicht beachtet. 

## <a name="windows-powershell-alias-vs-linux-utilities"></a>Windows PowerShell-Alias im Vergleich zu Linux-Hilfsprogrammen

Vorhandene Befehle unter Linux, z.B. `ls`, `sort` oder `sleep`, haben Vorrang vor ihren PowerShell-Aliasen.  Im Folgenden sind gängige entfernte Aliase und die entsprechenden Befehle aufgeführt:  

|Entfernter Alias   |Entsprechender Befehl   |
|---|---|
|`ls`     | `dir` <br> `Get-ChildItem` |
|`sort`   | `Sort-Object` |
|`sleep`  | `Start-Sleep` |

## <a name="persisting-home-vs-homeclouddrive"></a>Beibehalten von $home und $home\clouddrive

Für Benutzer, die Skripts und andere Dateien in ihrem Verzeichnis „clouddrive“ beibehalten haben, wird das $HOME-Verzeichnis jetzt sitzungsübergreifend beibehalten.

## <a name="powershell-profile"></a>PowerShell-Profil

Standardmäßig wird kein PowerShell-Profil erstellt.  Zum Erstellen Ihres Profils erstellen Sie unter `$HOME/.config` ein `PowerShell`-Verzeichnis.  Unter `$HOME/.config/PowerShell` können Sie Ihr Profil unter dem Namen `Microsoft.PowerShell_profile.ps1` erstellen.

## <a name="whats-new-in-powershell-core-6"></a>Neuerungen in PowerShell Core 6

Informationen zu den Neuerungen in PowerShell Core 6 finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6) und im Blogbeitrag [Getting Started with PowerShell Core](https://blogs.msdn.microsoft.com/powershell/2017/06/09/getting-started-with-powershell-core-on-windows-mac-and-linux/) (Erste Schritte mit PowerShell Core).
