---
title: Neukonfigurieren der Azure SSIS Integration Runtime | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Azure SSIS Integration Runtime in Azure Data Factory neu konfigurieren, nachdem Sie sie bereits bereitgestellt haben.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: spelluru
ms.openlocfilehash: 19a81917ade977a0d04934b77e8213ef6d9e0f12
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2017
---
# <a name="reconfigure-an-azure-ssis-integration-runtime"></a>Neukonfigurieren einer Azure SSIS Integration Runtime
Der Artikel [Erstellen einer Azure-SSIS-Integrationslaufzeit](create-azure-ssis-integration-runtime.md) erläutert die Erstellung einer Azure-SSIS-Integrationslaufzeit mithilfe von Azure Data Factory. Dieser Artikel enthält Informationen zum Neukonfigurieren einer vorhandenen Azure SSIS Integration Runtime.  

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 (GA) des Data Factory-Diensts verwenden, helfen Ihnen die Informationen unter [Dokumentation zur Version 1 von Data Factory](v1/data-factory-introduction.md) weiter.

Nach dem Bereitstellen und Starten einer Instanz der Azure-SSIS-Integrationslaufzeit können Sie sie durch Ausführen einer Reihe von `Stop` - `Set` - `Start`-PowerShell-Cmdlets neu konfigurieren. Das folgende PowerShell-Skript ändert z. B. die Anzahl der Knoten, die für die Instanz der Azure-SSIS-Integrationslaufzeit zugewiesen wurden, in „5“.

## <a name="stop-azure-ssis-ir"></a>Beenden der Azure SSIS IR
Beenden Sie zuerst die Azure SSIS Integration Runtime mit dem Cmdlet [Stop-AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/stop-azurermdatafactoryv2integrationruntime?view=azurermps-4.4.1). Dieser Befehl gibt alle Knoten frei und beendet die Abrechnung.

```powershell
Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName 
```

## <a name="reconfigure-azure-ssis-ir"></a>Neukonfigurieren einer Azure SSIS IR
Konfigurieren Sie die Azure SSIS Integration Runtime mit dem Cmdlet [Set-AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2integrationruntime?view=azurermps-4.4.1) neu. Der folgende Beispielbefehl skaliert eine Azure SSIS Integration Runtime horizontal auf fünf Knoten.

```powershell
Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -NodeCount 5
```  

## <a name="start-azure-ssis-ir"></a>Starten der Azure SSIS IR
Starten Sie dann die Azure SSIS Integration Runtime mit dem Cmdlet [Start-AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/start-azurermdatafactoryv2integrationruntime?view=azurermps-4.4.1). Durch diesen Befehl werden alle Knoten für die Ausführung von SSIS-Paketen zugeordnet.   

```powershell
Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Azure-SSIS-Laufzeit finden Sie in den folgenden Themen: 

- [Azure-SSIS-Integrationslaufzeit](concepts-integration-runtime.md#azure-ssis-integration-runtime): Dieser Artikel enthält konzeptionelle Informationen zu Integrationslaufzeiten, die im Allgemeinen die Azure-SSIS-Integrationslaufzeit einschließen. 
- [Tutorial: Bereitstellen von SSIS-Paketen in Azure](tutorial-deploy-ssis-packages-azure.md): Dieser Artikel enthält schrittweise Anweisungen zum Erstellen einer Azure-SSIS-Integrationslaufzeit und verwendet eine Azure SQL-Datenbank zum Hosten des SSIS-Katalogs. 
- [Gewusst wie: Erstellen einer Azure-SSIS-Integrationslaufzeit](create-azure-ssis-integration-runtime.md): In diesem Artikel wird das Tutorial vertieft und er enthält Anweisungen zur Verwendung der verwalteten Azure SQL-Instanz (private Vorschau) und zum Verknüpfen der Integrationslaufzeit mit einem VNet. 
- [Verknüpfen einer Azure-SSIS-Integrationslaufzeit mit einem VNet](join-azure-ssis-integration-runtime-virtual-network.md): Dieser Artikel enthält grundlegende Informationen zum Verknüpfen einer Azure-SSIS-Integrationslaufzeit mit einem Azure Virtual Network (VNet). Darüber hinaus enthält er Schritte zur Verwendung des Azure-Portals zum Konfigurieren des VNet, damit die Azure-SSIS-Integrationslaufzeit dem VNet beitreten kann. 
- [Überwachen einer Azure-SSIS-Integrationslaufzeit](monitor-integration-runtime.md#azure-ssis-integration-runtime): In diesem Artikel wird das Abrufen von Informationen zu einer Azure-SSIS-Integrationslaufzeit veranschaulicht, und er enthält Beschreibungen der Status in den zurückgegebenen Informationen. 
 
