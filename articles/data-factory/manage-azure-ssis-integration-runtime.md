---
title: Neukonfigurieren der Azure-SSIS-Integrationslaufzeit | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Azure-SSIS-Integrationslaufzeit in Azure Data Factory neu konfigurieren, nachdem Sie sie bereits bereitgestellt haben.
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
ms.openlocfilehash: c1743a0d06f911122ed0aba586aec837f81c578c
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2018
---
# <a name="manage-an-azure-ssis-integration-runtime"></a>Verwalten einer Azure-SSIS-Integrationslaufzeit
Der Artikel [Erstellen einer Azure-SSIS Integration Runtime in Azure Data Factory](create-azure-ssis-integration-runtime.md) erläutert die Erstellung einer Azure-SSIS-Integrationslaufzeit (IR) mithilfe von Azure Data Factory. Dieser Artikel enthält Informationen zum Neukonfigurieren einer vorhandenen Azure-SSIS-Integrationslaufzeit.  

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 (GA) des Data Factory-Diensts verwenden, helfen Ihnen die Informationen unter [Dokumentation zur Version 1 von Data Factory](v1/data-factory-introduction.md) weiter.

Nach dem Bereitstellen und Starten einer Instanz der Azure-SSIS-Integrationslaufzeit können Sie sie durch Ausführen einer Reihe von `Stop` - `Set` - `Start`-PowerShell-Cmdlets neu konfigurieren. Mit dem folgenden PowerShell-Skript wird beispielsweise die Anzahl von Knoten, die für die Instanz der Azure-SSIS-Integrationslaufzeit zugewiesen wurden, in „5“ geändert.

## <a name="reconfigure-an-azure-ssis-ir"></a>Neukonfigurieren einer Azure-SSIS-Integrationslaufzeit (IR)

1. Beenden Sie zuerst die Azure-SSIS-Integrationslaufzeit mit dem Cmdlet [Stop-AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/stop-azurermdatafactoryv2integrationruntime?view=azurermps-4.4.1). Dieser Befehl gibt alle Knoten frei und beendet die Abrechnung.

    ```powershell
    Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName 
    ```
2. Konfigurieren Sie als Nächstes die Azure-SSIS-Integrationslaufzeit mit dem Cmdlet [Set-AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2integrationruntime?view=azurermps-4.4.1) neu. Der folgende Beispielbefehl skaliert eine Azure-SSIS-Integrationslaufzeit horizontal auf fünf Knoten.

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -NodeCount 5
    ```  
3. Starten Sie anschließend die Azure-SSIS-Integrationslaufzeit mit dem Cmdlet [Start-AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/start-azurermdatafactoryv2integrationruntime?view=azurermps-4.4.1). Durch diesen Befehl werden alle Knoten für die Ausführung von SSIS-Paketen zugeordnet.   

    ```powershell
    Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName
    ```

## <a name="delete-an-azure-ssis-ir"></a>Löschen einer Azure-SSIS-Integrationslaufzeit
1. Listen Sie zuerst alle vorhandenen Azure-SSIS-Integrationslaufzeit-Instanzen unter Ihrer Data Factory auf.

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName -Status
    ```
2. Beenden Sie als Nächstes alle vorhandenen Azure-SSIS-Integrationslaufzeit-Instanzen in Ihrer Data Factory.

    ```powershell
    Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    ```
3. Entfernen Sie anschließend nacheinander einzeln alle vorhandenen Azure-SSIS-Integrationslaufzeit-Instanzen in Ihrer Data Factory.

    ```powershell
    Remove-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    ```
4. Entfernen Sie abschließend Ihre Data Factory.

    ```powershell
    Remove-AzureRmDataFactoryV2 -Name $DataFactoryName -ResourceGroupName $ResourceGroupName -Force
    ```
5. Entfernen Sie die Ressourcengruppe, falls Sie eine neue Ressourcengruppe erstellt haben.

    ```powershell
    Remove-AzureRmResourceGroup -Name $ResourceGroupName -Force 
    ```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Azure-SSIS-Laufzeit finden Sie in den folgenden Themen: 

- [Azure-SSIS-Integrationslaufzeit](concepts-integration-runtime.md#azure-ssis-integration-runtime): Dieser Artikel enthält konzeptionelle Informationen zu Integrationslaufzeiten, die im Allgemeinen die Azure-SSIS-Integrationslaufzeit einschließen. 
- [Tutorial: Bereitstellen von SSIS-Paketen in Azure](tutorial-deploy-ssis-packages-azure.md): Dieser Artikel enthält schrittweise Anweisungen zum Erstellen einer Azure-SSIS-Integrationslaufzeit und verwendet eine Azure SQL-Datenbank zum Hosten des SSIS-Katalogs. 
- [Gewusst wie: Erstellen einer Azure-SSIS-Integrationslaufzeit](create-azure-ssis-integration-runtime.md): In diesem Artikel wird das Tutorial vertieft und er enthält Anweisungen zur Verwendung der verwalteten Azure SQL-Instanz (private Vorschau) und zum Verknüpfen der Integrationslaufzeit mit einem VNet. 
- [Verknüpfen einer Azure-SSIS-Integrationslaufzeit mit einem VNet](join-azure-ssis-integration-runtime-virtual-network.md): Dieser Artikel enthält grundlegende Informationen zum Verknüpfen einer Azure-SSIS-Integrationslaufzeit mit einem Azure Virtual Network (VNet). Darüber hinaus enthält er Schritte zur Verwendung des Azure-Portals zum Konfigurieren des VNet, damit die Azure-SSIS-Integrationslaufzeit dem VNet beitreten kann. 
- [Überwachen einer Azure-SSIS-Integrationslaufzeit](monitor-integration-runtime.md#azure-ssis-integration-runtime): In diesem Artikel wird das Abrufen von Informationen zu einer Azure-SSIS-Integrationslaufzeit veranschaulicht, und er enthält Beschreibungen der Status in den zurückgegebenen Informationen. 
 
