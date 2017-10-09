---
title: "Übersicht über Azure Cloud Shell (Vorschau) | Microsoft-Dokumentation"
description: "Übersicht über Azure Cloud Shell."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: juluk
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 44d2ac2fd35621ab8cd8d7584744139ee3bab5c2
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="overview-of-azure-cloud-shell-preview"></a>Übersicht über Azure Cloud Shell (Vorschau)
Azure Cloud Shell ist eine interaktive, über den Browser zugängliche Shell für die Verwaltung von Azure-Ressourcen.
Es bietet Ihnen die Flexibilität, die Shell-Benutzeroberfläche auszuwählen, die sich am besten für Ihre Arbeitsweise eignet.
Linux-Benutzer können Bash-Benutzeroberflächen verwenden, während Windows-Benutzer PowerShell nutzen können.

![Bash in Cloud Shell](media/overview/overview-bash-pic.png)

![PowerShell in Cloud Shell](media/overview/overview-ps-pic.png)

## <a name="features"></a>Features
### <a name="browser-based-shell-experience"></a>Browserbasierte Shelloberfläche
Cloud Shell ermöglicht den Zugriff auf eine browserbasierte Befehlszeilenumgebung, die extra auf Azure-Verwaltungsaufgaben ausgerichtet ist.
Nutzen Sie Cloud Shell, um unabhängig von einem lokalen Computer in einer Weise zu arbeiten, die Ihnen nur die Cloud bieten kann.

### <a name="choice-of-preferred-shell-experience"></a>Auswahl bevorzugter Shell-Benutzeroberfläche
Azure Cloud Shell bietet Ihnen die Flexibilität, die Shell-Benutzeroberfläche auszuwählen, die sich am besten für Ihre Arbeitsweise eignet.
Linux-Benutzer können Bash-Benutzeroberflächen verwenden, während Windows-Benutzer PowerShell nutzen können.

### <a name="pre-configured-azure-workstation"></a>Vorkonfigurierte Azure-Arbeitsstation
In Cloud Shell sind gängige Befehlszeilentools und Sprachunterstützung vorinstalliert, damit Sie schneller arbeiten können.

Zeigen Sie die Toolliste für [Bash-Benutzeroberflächen](features.md#tools) und [PowerShell-Benutzerberflächen](features-powershell.md#tools) an.

### <a name="automatic-authentication"></a>Automatische Authentifizierung
Cloud Shell führt in jeder Sitzung eine sichere Authentifizierung durch, sodass Sie über die Azure CLI 2.0 sofort auf Ihre Ressourcen zugreifen können.

### <a name="connect-your-azure-file-storage"></a>Herstellen einer Verbindung mit Ihrem Azure-Dateispeicher
Cloud Shell-Computer sind temporär und erfordern folglich das Einbinden einer Azure-Dateifreigabe als `clouddrive`, um Ihr $Home-Verzeichnis dauerhaft bereitzustellen.
Beim ersten Start werden Sie von Cloud Shell darauf hingewiesen, dass eine Ressourcengruppe, ein Speicherkonto und eine Dateifreigabe in Ihrem Namen erstellt werden. Dieser Schritt ist nur einmal erforderlich. Diese Komponenten werden für alle Sitzungen automatisch angefügt. Es kann eine einzelne Dateifreigabe zugeordnet werden, die sowohl von Bash als auch von PowerShell in Cloud-Shell verwendet wird.

#### <a name="create-new-storage"></a>Erstellen von neuem Speicher
![](media/overview/basic-storage.png)

Ein lokal redundantes Speicherkonto (Locally Redundant Storage, LRS) und eine Azure-Dateifreigabe können in Ihrem Namen erstellt werden. Die Azure-Dateifreigabe wird für Bash- und PowerShell-Umgebungen verwendet, wenn Sie sich für die Verwendung beider Optionen entscheiden. Es gelten die üblichen Speicherkosten.

Drei Ressourcen werden in Ihrem Namen erstellt:
1. Eine Ressourcengruppe namens `cloud-shell-storage-<region>`
2. Ein Speicherkonto namens `cs<uniqueGuid>`
3. Eine Dateifreigabe namens `cs-<user>-<domain>-com-<uniqueGuid>`

> [!Note]
> Bash in Cloud Shell erstellt zudem ein 5 GB-Datenträgerimage, um `$Home` beizubehalten. Alle Dateien in Ihrem $Home-Verzeichnis, wie etwa SSH-Schlüssel, werden beständig in Ihrem Datenträgerimage gespeichert, das in der eingebundenen Dateitreigabe gespeichert ist. Wenden Sie beim Speichern von Dateien in Ihrem $Home-Verzeichnis und der eingebundenen Dateifreigabe bewährte Methoden an.

#### <a name="use-existing-resources"></a>Verwenden vorhandener Ressourcen
![](media/overview/advanced-storage.png)

Es wird eine erweiterte Option bereitgestellt, um vorhandene Ressourcen zu Cloud Shell zuzuordnen.
Klicken Sie bei der Aufforderung zur Speichereinrichtung auf „Erweiterte Einstellungen anzeigen“, um zusätzliche Optionen anzuzeigen.
Dropdownlisten werden für Ihre zugewiesene Cloud Shell-Region und die lokal/global redundanten Speicherkonten gefiltert.

[Weitere Informationen zu Cloud Shell-Speicher, dem Aktualisieren von Dateifreigaben und dem Upload/Download von Dateien.](persisting-shell-storage.md)

## <a name="concepts"></a>Konzepte
* Cloud Shell wird auf einem temporären Computer ausgeführt, der pro Sitzung und pro Benutzer bereitgestellt wird.
* Nach 20 Minuten ohne interaktive Aktivitäten tritt für Cloud Shell ein Timeout ein.
* Der Zugriff auf Cloud Shell ist nur möglich, wenn eine Dateifreigabe angefügt ist.
* Cloud Shell verwendet dieselbe Dateifreigabe für Bash und PowerShell.
* Cloud Shell wird ein Computer pro Benutzerkonto zugewiesen.
* Berechtigungen werden auf einen normalen Linux-Benutzer (Bash) festgelegt.

Weitere Informationen erhalten Sie unter [Bash in Cloud Shell](features.md) und [PowerShell in Cloud Shell](features-powershell.md).

## <a name="examples"></a>Beispiele
* Verwenden von Skripts zum Automatisieren von Azure-Verwaltungsaufgaben
* Gleichzeitiges Verwalten von Azure-Ressourcen über das Azure-Portal und Azure-Befehlszeilentools
* Testen der Azure-CLI 2.0 oder von Azure PowerShell-Cmdlets

Testen Sie diese Beispiele in Schnellstarts für [Bash in Cloud Shell](quickstart.md) und [PowerShell in Cloud Shell](quickstart-powershell.md).

## <a name="pricing"></a>Preise
Der Computer, auf dem Cloud Shell gehostet wird, ist kostenlos. Als Voraussetzung muss eine Azure-Dateifreigabe eingebunden sein. Es gelten die üblichen Speicherkosten.

## <a name="supported-browsers"></a>Unterstützte Browser
Cloud Shell wird für Chrome, Edge und Safari empfohlen.
Cloud Shell wird zwar für Chrome, Firefox, Safari, Internet Explorer und Edge unterstützt, unterliegt jedoch bestimmten Browsereinstellungen.
