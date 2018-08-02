---
title: Übersicht über Azure Cloud Shell | Microsoft-Dokumentation
description: Übersicht über Azure Cloud Shell.
services: ''
documentationcenter: ''
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: juluk
ms.openlocfilehash: 9588bebdc827760f0e0d3e2aadccbff5f24723f1
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "39258924"
---
# <a name="overview-of-azure-cloud-shell"></a>Übersicht über Azure Cloud Shell
Azure Cloud Shell ist eine interaktive, über den Browser zugängliche Shell für die Verwaltung von Azure-Ressourcen.
Es bietet Ihnen die Flexibilität, die Shell-Benutzeroberfläche auszuwählen, die sich am besten für Ihre Arbeitsweise eignet.
Linux-Benutzer können Bash-Benutzeroberflächen verwenden, während Windows-Benutzer PowerShell nutzen können.

Versuchen Sie es über „shell.azure.com“, indem Sie unten klicken.

[![](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell starten")](https://shell.azure.com)

Verwenden Sie das Cloud Shell-Symbol, um es über das Azure-Portal zu versuchen:

![Start über das Portal](media/overview/portal-launch-icon.png)

## <a name="features"></a>Features
### <a name="browser-based-shell-experience"></a>Browserbasierte Shelloberfläche
Cloud Shell ermöglicht den Zugriff auf eine browserbasierte Befehlszeilenumgebung, die extra auf Azure-Verwaltungsaufgaben ausgerichtet ist.
Nutzen Sie Cloud Shell, um unabhängig von einem lokalen Computer in einer Weise zu arbeiten, die Ihnen nur die Cloud bieten kann.

### <a name="choice-of-preferred-shell-experience"></a>Auswahl bevorzugter Shell-Benutzeroberfläche
Linux-Benutzer können Bash in Cloud Shell verwenden. Für Windows-Benutzer steht PowerShell in Cloud Shell (Vorschauversion) über das Shell-Dropdownmenü zur Verfügung.

![Bash in Cloud Shell](media/overview/overview-bash-pic.png)

![PowerShell in Cloud Shell (Vorschauversion)](media/overview/overview-ps-pic.png)

### <a name="authenticated-and-configured-azure-workstation"></a>Authentifizierte und konfigurierte Azure-Arbeitsstation
Cloud Shell wird von Microsoft verwaltet und verfügt somit über gängige Befehlszeilentools und Sprachunterstützung. Darüber hinaus führt Cloud Shell automatisch eine sichere Authentifizierung durch, sodass Sie über die Azure CLI 2.0 oder über Azure PowerShell-Cmdlets sofort auf Ihre Ressourcen zugreifen können.

Zeigen Sie die vollständige [Liste der in Cloud Shell installierten Tools](features.md#tools) an.

### <a name="integrated-cloud-shell-editor"></a>Integrierter Cloud Shell-Editor
Cloud Shell bietet einen integrierten grafischen Text-Editor, der auf dem Open-Source-Monaco-Editor basiert. Erstellen und bearbeiten Sie Konfigurationsdateien einfach durch Ausführung von `code .` zur nahtlosen Bereitstellung über Azure CLI 2.0 oder Azure PowerShell.

[Erfahren Sie mehr über den Cloud Shell-Editor](using-cloud-shell-editor.md).

### <a name="multiple-access-points"></a>Mehrere Zugriffspunkte
Cloud Shell ist ein flexibles Tool mit verschiedenen Zugriffsmöglichkeiten:
* [portal.azure.com](https://portal.azure.com)
* [shell.azure.com](https://shell.azure.com)
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest)
* [Azure Mobile App](https://azure.microsoft.com/features/azure-portal/mobile-app/)
* [Azure-Kontoerweiterung für VS Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)

### <a name="connect-your-microsoft-azure-files-storage"></a>Herstellen einer Verbindung mit dem Microsoft Azure Files-Speicher
Cloud Shell-Computer sind temporär und erfordern die Einbindung einer Azure Files-Freigabe als `clouddrive`, um Ihre Dateien dauerhaft bereitzustellen.

Beim ersten Start werden Sie von Cloud Shell darauf hingewiesen, dass für Sie eine Ressourcengruppe, ein Speicherkonto und eine Azure Files-Freigabe erstellt werden. Dieser Schritt ist nur einmal erforderlich. Diese Komponenten werden für alle Sitzungen automatisch angefügt. Es kann eine einzelne Dateifreigabe zugeordnet werden, die sowohl von Bash als auch von PowerShell in Cloud Shell (Vorschauversion) verwendet wird.

#### <a name="create-new-storage"></a>Erstellen von neuem Speicher
![](media/overview/basic-storage.png)

Ein lokal redundantes Speicherkonto (Locally Redundant Storage, LRS) und eine Azure Files-Freigabe können in Ihrem Namen erstellt werden. Die Azure Files-Freigabe wird für Bash- und PowerShell-Umgebungen verwendet, wenn Sie sich für die Verwendung beider Optionen entscheiden. Es gelten die üblichen Speicherkosten.

Drei Ressourcen werden in Ihrem Namen erstellt:
1. Eine Ressourcengruppe namens `cloud-shell-storage-<region>`
2. Ein Speicherkonto namens `cs<uniqueGuid>`
3. Eine Dateifreigabe namens `cs-<user>-<domain>-com-<uniqueGuid>`

> [!Note]
> Bash in Cloud Shell erstellt zudem ein 5 GB-Datenträgerimage, um `$Home` beizubehalten. Alle Dateien in Ihrem $Home-Verzeichnis, wie etwa SSH-Schlüssel, werden beständig in Ihrem Datenträgerimage gespeichert, das in der eingebundenen Azure-Dateifreigabe gespeichert ist. Wenden Sie beim Speichern von Dateien in Ihrem $Home-Verzeichnis und der eingebundenen Azure-Dateifreigabe bewährte Methoden an.

#### <a name="use-existing-resources"></a>Verwenden vorhandener Ressourcen
![](media/overview/advanced-storage.png)

Es wird eine erweiterte Option bereitgestellt, um vorhandene Ressourcen zu Cloud Shell zuzuordnen.
Klicken Sie bei der Aufforderung zur Speichereinrichtung auf „Erweiterte Einstellungen anzeigen“, um zusätzliche Optionen anzuzeigen.

> [!Note]
> Dropdownlisten werden für Ihre vorab zugewiesene Cloud Shell-Region und die LRS/GRS/ZRS-Speicherkonten gefiltert.

[Informationen zu Cloud Shell-Speicher, dem Aktualisieren von Azure-Dateifreigaben und dem Upload/Download von Dateien](persisting-shell-storage.md)

## <a name="concepts"></a>Konzepte
* Cloud Shell wird auf einem temporären Host ausgeführt, der pro Sitzung und pro Benutzer bereitgestellt wird.
* Nach 20 Minuten ohne interaktive Aktivitäten tritt für Cloud Shell ein Timeout ein.
* Für Cloud Shell muss eine Azure-Dateifreigabe bereitgestellt werden.
* Cloud Shell verwendet dieselbe Azure-Dateifreigabe für Bash und PowerShell.
* Cloud Shell wird ein Computer pro Benutzerkonto zugewiesen.
* Cloud Shell stellt $Home dauerhaft unter Verwendung eines 5-GB-Images bereit, das sich in Ihrer Dateifreigabe befindet.
* Berechtigungen werden auf einen normalen Linux-Benutzer in Bash festgelegt.

Weitere Informationen finden Sie unter [Features und Tools für Bash in Azure Cloud Shell](features.md) sowie unter [Features und Tools für PowerShell in Azure Cloud Shell](features-powershell.md).

## <a name="pricing"></a>Preise
Der Computer, auf dem Cloud Shell gehostet wird, ist kostenlos. Als Voraussetzung muss eine Azure Files-Freigabe eingebunden sein. Es gelten die üblichen Speicherkosten.

## <a name="next-steps"></a>Nächste Schritte
[Schnellstart für Bash in Azure Cloud Shell](quickstart.md) <br>
[Schnellstart für PowerShell in Cloud Shell (Vorschauversion)](quickstart-powershell.md)