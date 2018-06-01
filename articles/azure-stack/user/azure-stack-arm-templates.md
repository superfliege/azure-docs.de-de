---
title: Verwenden von Azure Resource Manager-Vorlagen in Azure Stack | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Azure Resource Manager-Vorlagen in Azure Stack zum Bereitstellen von Ressourcen verwenden.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 9c4d538f77ae056163fd17aa547162a4ad3eff63
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2018
ms.locfileid: "34301677"
---
# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Verwenden von Azure-Ressourcen-Manager-Vorlagen in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Mit Azure Resource Manager-Vorlagen können Sie alle Ressourcen für Ihre Anwendung in einem einzigen, koordinierten Vorgang bereitstellen. Sie können Vorlagen auch erneut bereitstellen, um Änderungen an den Ressourcen in einer Ressourcengruppe vorzunehmen.

Diese Vorlagen können mit dem Microsoft Azure Stack-Portal, PowerShell, über die Befehlszeile und Visual Studio bereitgestellt werden.

Die folgenden Schnellstartvorlagen sind auf [GitHub](http://aka.ms/azurestackgithub) verfügbar.

## <a name="deploy-sharepoint-server-non-high-availability-deployment"></a>Bereitstellen von SharePoint Server (Bereitstellung ohne Hochverfügbarkeit)

Verwenden Sie die PowerShell DSC-Erweiterung, um eine SharePoint Server 2013-Farm zu erstellen, die folgende Ressourcen enthält:

* Ein virtuelles Netzwerk
* Drei Speicherkonten
* Zwei externe Load Balancer
* Einen virtuellen Computer (VM), der als Domänencontroller für eine neue Gesamtstruktur mit einer einzelnen Domäne konfiguriert ist
* Einen virtuellen Computer, der als eigenständiger Server für SQL Server 2014 konfiguriert ist
* Eine VM, die als SharePoint Server 2013-Farm mit einem Computer konfiguriert ist

## <a name="deploy-ad-non-high-availability-deployment"></a>Bereitstellen von AD (Bereitstellung ohne Hochverfügbarkeit)

Verwenden Sie die PowerShell DSC-Erweiterung, um einen Server als AD-Domänencontroller zu erstellen, der folgende Ressourcen enthält:

* Ein virtuelles Netzwerk
* Ein Speicherkonto
* Einen externen Load Balancer
* Einen virtuellen Computer (VM), der als Domänencontroller für eine neue Gesamtstruktur mit einer einzelnen Domäne konfiguriert ist

## <a name="deploy-adsql-non-high-availability-deployment"></a>Bereitstellen von AD/SQL (Bereitstellung ohne Hochverfügbarkeit)

Verwenden Sie die PowerShell DSC-Erweiterung, um einen eigenständigen Server für SQL Server 2014 zu erstellen, der folgende Ressourcen enthält:

* Ein virtuelles Netzwerk
* Zwei Speicherkonten
* Einen externen Load Balancer
* Einen virtuellen Computer (VM), der als Domänencontroller für eine neue Gesamtstruktur mit einer einzelnen Domäne konfiguriert ist
* Einen virtuellen Computer, der als eigenständiger Server für SQL Server 2014 konfiguriert ist

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server

Verwenden Sie die PowerShell DSC-Erweiterung, um einen vorhandenen lokalen Konfigurations-Manager (LCM) für virtuelle Computer zu konfigurieren und auf einem DSC Pull-Server im Azure Automation-Konto zu registrieren.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Erstellen eines virtuellen Computers von einem Benutzerimage

Erstellen Sie einen virtuellen Computer aus einem Benutzerimage. Diese Vorlage stellt auch ein virtuelles Netzwerk (mit DNS), eine öffentliche IP-Adresse und eine Netzwerkschnittstelle bereit.

## <a name="basic-virtual-machine"></a>Virtueller Computer im Tarif „Basic“

Stellen Sie einen virtuellen Windows-Computer bereit, der ein virtuelles Netzwerk (mit DNS), eine öffentliche IP-Adresse und eine Netzwerkschnittstelle umfasst.

## <a name="cancel-a-running-template-deployment"></a>Abbrechen der Bereitstellung einer Vorlage

Verwenden Sie zum Abbrechen der Ausführung einer Vorlagenbereitstellung das PowerShell-Cmdlet `Stop-AzureRmResourceGroupDeployment`.

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen von Vorlagen mit dem Portal](azure-stack-deploy-template-portal.md)
* [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
