---
title: "Azure PowerShell-Skriptbeispiel – Hinzufügen einer Netzwerksicherheitsgruppen-Regel | Microsoft-Dokumentation"
description: "Azure PowerShell-Skriptbeispiel– fügt eine Netzwerksicherheitsgruppen-Regel zum Zulassen von eingehendem Datenverkehr an einem bestimmten Port hinzu"
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 11/28/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: fd3c648ee63c45bef305658832a4d31dfdb213be
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2017
---
# <a name="add-an-inbound-network-security-group-rule"></a>Einzufügen einer eingehenden Netzwerksicherheitsgruppen-Regel

Dieses Beispielskript erstellt eine Netzwerksicherheitsgruppen-Regel zum Zulassen von eingehendem Datenverkehr an Port 8081.  Das Skript ruft die Ressource `Microsoft.Network/networkSecurityGroups` ab, in der sich der Cluster befindet, erstellt eine neue Netzwerksicherheits-Konfigurationsregel und aktualisiert die Netzwerksicherheitsgruppe. Passen Sie die Parameter nach Bedarf an.

Installieren Sie bei Bedarf Azure PowerShell anhand der Anweisungen im [Azure PowerShell-Handbuch](/powershell/azure/overview). 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-inbound-nsg-rule/add-inbound-nsg-rule.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Ruft die Ressource `Microsoft.Network/networkSecurityGroups` ab |
|[Get-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermnetworksecuritygroup)| Ruft die Netzwerksicherheitsgruppe anhand des Namens ab|
|[Add-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig)| Fügt einer Netzwerksicherheitsgruppe eine Netzwerksicherheits-Konfigurationsregel hinzu |
|[Set-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/set-azurermnetworksecuritygroup)| Legt den Zielstatus für eine Netzwerksicherheitsgruppe fest|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).
