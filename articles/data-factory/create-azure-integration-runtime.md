---
title: Erstellen von Azure Integration Runtime in Azure Data Factory | Microsoft-Dokumentation
description: "Informationen zum Erstellen von Integration Runtime in Azure Data Factory, die zum Kopieren von Daten und Verteilen von Transformationsaktivitäten verwendet wird."
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
ms.date: 09/15/2017
ms.author: spelluru
ms.openlocfilehash: e4e9416d75d5887de059a6fcfc66683940e3e6fe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-and-configure-azure-integration-runtime"></a>Erstellen und Konfigurieren von Azure Integration Runtime
Bei Integration Runtime (IR) handelt es sich um die Computeinfrastruktur, mit der Azure Data Factory Datenintegrationsfunktionen verschiedene Netzwerkumgebungen übergreifend bereitstellt. Weitere Informationen zu IR finden Sie unter [Integration runtime in Azure Data Factory](concepts-integration-runtime.md) (Integration Runtime in Azure Data Factory).

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (GA) ist, lesen Sie die [Dokumentation zur Version 1 von Data Factory](v1/data-factory-introduction.md).

Azure IR bietet ein vollständig verwaltetes Compute zur nativen Durchführung des Verschiebens von Daten und Disponierens von Datentransformationsaktivitäten zum Berechnen von Diensten wie HDInsight. IR wird in der Azure-Umgebung gehostet und unterstützt das Herstellen von Verbindungen mit Ressourcen in einer öffentlichen Netzwerkumgebung mit öffentlich zugänglichen Endpunkten.

Dieses Dokument ist eine Einführung zum Erstellen und Konfigurieren von Azure Integration Runtime. 

## <a name="default-azure-ir"></a>Standardmäßige Azure IR
Standardmäßig verfügt jede Data Factory im Back-End über eine Azure-IR, die Vorgänge in Clouddatenspeichern und Computediensten im öffentlichen Netzwerk unterstützt. Der Speicherort dieser Azure IR wird automatisch aufgelöst. Wenn die **connectVia**-Eigenschaft in der Definition des verknüpften Diensts nicht angegeben ist, wird die angegebene Azure IR verwendet. Sie müssen nur dann explizit eine Azure IR erstellen, wenn Sie den Speicherort der IR explizit definieren möchten, oder wenn Sie die Aktivitätsausführungen aus Verwaltungsgründen auf verschiedenen IRs virtuell gruppieren möchten. 

## <a name="create-azure-ir"></a>Erstellen der Azure IR
Integration Runtime kann mithilfe des **Set-AzureRmDataFactoryV2IntegrationRuntime**-PowerShell-Cmdlets erstellt werden. Um ein Azure IR zu erstellen, geben Sie Namen, Speicherort und Typ im Befehl an. Hier ist ein Beispielbefehl zum Erstellen einer Azure IR mit Speicherort „Europa, Westen“:

```powershell
Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName "SampleV2DataFactory1" -Name "MySampleAzureIR" -ResourceGroupName "ADFV2SampleRG" -Type Managed -Location "West Europe"
```  
Für die Azure IR muss der Typ auf **Managed** festgelegt werden. Sie müssen keine Computedetails angeben, da sie in der Cloud vollständig elastisch verwaltet wird. Geben Sie Computedetails wie Knotengröße und Knotenzahl an, wenn Sie eine Azure-SSIS IR erstellen möchten. Weitere Informationen finden Sie unter [Create an Azure-SSIS integration runtime in Azure Data Factory](create-azure-ssis-integration-runtime.md) (Erstellen einer Azure-SSIS Integration Runtime in Azure Data Factory).

Sie können eine vorhandenen Azure IR mithilfe des Set-AzureRmDataFactoryV2IntegrationRuntime-PowerShell-Cmdlets konfigurieren, um ihren Speicherort zu ändern. Weitere Informationen über den Speicherort einer Azure IR finden Sie unter [Integration Runtime in Azure Data Factory](concepts-integration-runtime.md).

## <a name="use-azure-ir"></a>Verwenden der Azure IR

Sobald eine Azure IR erstellt ist, können Sie in Ihrer Definition des verknüpften Diensts darauf verweisen. Das folgende Beispiel zeigt Ihnen, wie Sie von einem verknüpften Azure Storage-Dienst aus auf die oben erstellte Azure Integration Runtime verweisen können:  

```json
{
    "name": "MyStorageLinkedService",
    "properties": {
      "type": "AzureStorage",
      "typeProperties": {
        "connectionString": {
          "value": "DefaultEndpointsProtocol=https;AccountName=myaccountname;AccountKey=...",
          "type": "SecureString"
        }
      },
      "connectVia": {
        "referenceName": "MySampleAzureIR",
        "type": "IntegrationRuntimeReference"
      }   
    }
}

```

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Erstellen anderer Integration Runtime-Typen finden Sie in den folgenden Artikeln:

- [How to create and configure Self-hosted Integration Runtime](create-self-hosted-integration-runtime.md) (Erstellen und Konfigurieren einer selbstgehosteten Integration Runtime)
- [Create an Azure-SSIS integration runtime in Azure Data Factory](create-azure-ssis-integration-runtime.md) (Erstellen einer Azure-SSIS Integration Runtime in Azure Data Factory)
 
