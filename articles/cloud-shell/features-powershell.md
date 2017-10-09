---
title: Features von PowerShell in Azure Cloud Shell (Vorschau) | Microsoft-Dokumentation
description: "Übersicht über die Features von PowerShell in Azure Cloud Shell"
services: Azure
documentationcenter: 
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: damaerte
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 9eacb57b5a00ff11739695ed3311be0c638ba25f
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="features--tools-for-powershell-in-azure-cloud-shell"></a>Features und Tools für PowerShell in Azure Cloud Shell

[!include [features-introblock](../../includes/cloud-shell-features-introblock.md)]

> [!TIP]
> Features und Tools für [Bash](features.md) sind ebenfalls verfügbar.

PowerShell in Cloud Shell wird unter `Windows Server 2016` ausgeführt.

## <a name="features"></a>Features

### <a name="secure-automatic-authentication"></a>Sichern der automatischen Authentifizierung

PowerShell in Cloud Shell authentifiziert Zugriff auf Konten für Azure PowerShell sicher und automatisch.

### <a name="files-persistence-across-sessions"></a>Sitzungsübergreifende Dateienpersistenz

Damit Sie Dateien sitzungsübergreifend beibehalten können, wird Ihnen beim ersten Start von Cloud Shell das Anfügen einer Azure-Dateifreigabe gezeigt.
Anschließend fügt Cloud Shell Ihren Speicher (als `$home\clouddrive` eingebunden) automatisch für alle zukünftigen Sitzungen an.
Da durch jede Anforderung für Cloud Shell ein temporärer Computer zugewiesen wird, werden Dateien außerhalb von `$home\clouddrive` und der Computerstatus nicht sitzungsübergreifend beibehalten.

[Erfahren Sie mehr über das Anfügen von Azure-Dateifreigaben in Cloud Shell.](persisting-shell-storage-powershell.md)

### <a name="azure-drive-azure"></a>Azure-Laufwerk (Azure:)

PowerShell in Cloud Shell startet auf dem Azure-Laufwerk (`Azure:`).
Das Azure-Laufwerk ermöglicht die einfache Ermittlung und Navigation von Azure-Ressourcen wie Computing, Netzwerk, Speicher usw., ähnlich der Navigation im Dateisystem.
Sie können weiterhin die vertrauten [Azure PowerShell-Cmdlets](https://docs.microsoft.com/en-us/powershell/azure) zur Verwaltung dieser Ressourcen verwenden.
Alle an den Azure-Ressourcen vorgenommenen Änderungen, die entweder direkt im Azure-Portal oder über Azure PowerShell-Cmdlets durchgeführt wurden, werden sofort auf dem Azure-Laufwerk reflektiert.

![](media/features-powershell/azure-drive.png)

#### <a name="contextual-awareness"></a>Kontextbezogene Informationen

- **Ressourcengruppenbereiche**: Wenn er im Kontext eines Ressourcengruppenpfads auf dem Azure-Laufwerk (`Azure:`) auftritt, wird der Ressourcengruppenname automatisch an die Azure PowerShell-Cmdlets übergeben.

    ![](media/features-powershell/resource-group-autocomplete.png)

- **Get-AzureRmCommand**: Dieses Cmdlet gibt die Liste der Befehle im Kontext des Speicherorts auf dem Azure-Laufwerk (`Azure:`) zurück. Wenn sich der Benutzer z. B. unter `Azure:\<subscription name>\StorageAccounts` befindet, werden nur speicherbezogene Befehle angezeigt.

    ![](media/features-powershell/get-azurermcommand.png)

### <a name="rich-powershell-script-editing"></a>Erweiterte PowerShell-Skriptbearbeitung

Wenn Sie PowerShell-Dateien (`.ps1,.psm1,.psd1`) mit VIM bearbeiten, erhalten Sie automatisch die Syntaxhervorhebung und IntelliSense-Unterstützung.
Die IntelliSense-Unterstützung wird über ein VIM-Plug-In implementiert, das mit einer lokalen Instanz der [PowerShell Editor Services](https://github.com/PowerShell/PowerShellEditorServices) interagiert.

> [!TIP]
> Verwenden Sie `TAB`, um die Vervollständigung (IntelliSense) von Befehlsnamen, Parameternamen und Parameterwerten (falls zutreffend) zu erhalten.

![](media/features-powershell/powershell-editing-vim.png)

### <a name="extensible-model"></a>Erweiterbares Modell

Mit [PowerShellGet](https://docs.microsoft.com/powershell/module/powershellget) können Sie problemlos benutzerdefinierte Module und Skripts aus dem [PowerShell-Katalog](https://www.powershellgallery.com) installieren (und aktualisieren).
Nach der Installation werden Ihre Module automatisch sitzungsübergreifend in Cloud Shell beibehalten.

> [!TIP]
> Vom Benutzer installierte Module werden im Ordner `$Home\CloudDrive\.pscloudshell\WindowsPowerShell` gespeichert. Ein diesem Ordner zugeordneter symbolischer Link wird im Ordner „Dokumente“ des Benutzers erstellt (`$home\Documents\WindowsPowerShell`).

![](media/features-powershell/powershellget-module.png)

### <a name="management-of-guest-vms"></a>Verwaltung von Gast-VMs

Mithilfe zweier integrierter Befehle, `Enter-AzureRmVM` und `Invoke-AzureRmVMCommand`, können Sie Ihre virtuellen Azure-Computer remote verwalten.
Diese Befehle basieren auf dem PowerShell-Remoting und erfordern die PowerShell-Konnektivität mit den virtuellen Azure-Computern.

## <a name="tools"></a>Tools

|**Kategorie**    |**Name**                                 |
|----------------|-----------------------------------------|
|Azure-Tools     |[Azure PowerShell (4.3.1)](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1) |
|Text-Editoren    |Vim<br> Nano                             |
|Paket-Manager |PowerShellGet<br> PackageManagement<br> npm<br> pip |
|Quellcodeverwaltung  |Git                                      |
|Datenbanken       |[SqlServer-Modul](https://www.powershellgallery.com/packages/SqlServer)<br> [SQLCMD-Hilfsprogramm](https://docs.microsoft.com/sql/tools/sqlcmd-utility)      |
|Testtools      |Pester                                   |

## <a name="language-support"></a>Sprachunterstützung

|**Sprache**|**Version**|
|------------|-----------|
|.NET        |4.6        |
|Node.js     |6.10       |
|PowerShell  |5.1 und [6.0 (Beta)](https://github.com/PowerShell/powershell/releases)       |
|Python      |2.7        |

## <a name="next-steps"></a>Nächste Schritte

[Schnellstart mit PowerShell in Cloud Shell](quickstart-powershell.md) <br>
[Informationen zu Azure PowerShell](https://docs.microsoft.com/powershell/azure/)

