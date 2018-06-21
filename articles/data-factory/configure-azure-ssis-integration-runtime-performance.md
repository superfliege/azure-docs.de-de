---
title: Konfigurieren von Leistung für die Azure-SSIS Integration Runtime | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Eigenschaften von Azure-SSIS Integration Runtime für hohe Leistung konfigurieren.
services: data-factory
ms.date: 01/10/2018
ms.topic: conceptual
ms.service: data-factory
ms.workload: data-services
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: e0bbd504b47a3b599022049c984c6dd59a6d0a0b
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266769"
---
# <a name="configure-the-azure-ssis-integration-runtime-for-high-performance"></a>Konfigurieren von Azure-SSIS Integration Runtime für hohe Leistung

In diesem Artikel erfahren Sie, wie Sie eine Instanz von Azure-SSIS Integration Runtime (IR) für hohe Leistung konfigurieren. Mit Azure-SSIS IR können Sie SSIS-Pakete (SQL Server Integration Services) in Azure bereitstellen und ausführen. Weitere Informationen zu Azure-SSIS IR finden Sie im Artikel zu [Integration Runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime). Informationen zum Bereitstellen und Ausführen von SSIS-Paketen in Azure finden Sie unter [Lift and shift SQL Server Integration Services workloads to the cloud](/sql/integration-services/lift-shift/ssis-azure-lift-shift-ssis-packages-overview) (Migrieren von SQL Server Integration Services-Workloads in die Cloud per Lift & Shift).

> [!IMPORTANT]
> Dieser Artikel enthält Leistungsergebnisse und Beobachtungen aus internen Tests, die von Mitgliedern des SSIS-Entwicklerteams durchgeführt wurden. Ihre Ergebnisse können davon abweichen. Führen Sie daher eigene Tests durch, bevor Sie Ihre Konfigurationseinstellungen endgültig festlegen. Diese Einstellungen können sich sowohl auf die Kosten als auch auf die Leistung auswirken.

## <a name="properties-to-configure"></a>Zu konfigurierende Eigenschaften

Der folgende Auszug aus einem Konfigurationsskript enthält die Eigenschaften, die Sie beim Erstellen einer Instanz von Azure-SSIS Integration Runtime konfigurieren können. Das vollständige PowerShell-Skript und die dazugehörige Beschreibung finden Sie unter [Bereitstellen von SQL Server Integration Services-Paketen in Azure](tutorial-deploy-ssis-packages-azure-powershell.md).

```powershell
$SubscriptionName = "<Azure subscription name>"
$ResourceGroupName = "<Azure resource group name>"
# Data factory name. Must be globally unique
$DataFactoryName = "<Data factory name>" 
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "<Specify a name for your Azure-SSIS IR>"
$AzureSSISDescription = "<Specify description for your Azure-SSIS IR"
# In public preview, only EastUS, NorthEurope, and WestEurope are supported.
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_D3_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "<Azure SQL server name>.database.windows.net"
$SSISDBServerAdminUserName = "<Azure SQL server - user name>"
$SSISDBServerAdminPassword = "<Azure SQL server - user password>"
# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (Preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "<pricing tier of your Azure SQL server. Examples: Basic, S0, S1, S2, S3, etc.>"
```

## <a name="azuressislocation"></a>AzureSSISLocation
**AzureSSISLocation** ist der Ort, an dem sich der Integration Runtime-Workerknoten befindet. Der Workerknoten gewährleistet eine konstante Verbindung mit der SSIS-Katalogdatenbank (SSIS Catalog Database, SSISDB) in einer Azure SQL-Datenbank. Legen Sie **AzureSSISLocation** auf den Ort fest, an dem sich auch der SQL-Datenbankserver befindet, der die SSISDB hostet, um die bestmögliche Integration Runtime-Effizienz zu erzielen.

## <a name="azuressisnodesize"></a>AzureSSISNodeSize
Die öffentliche Vorschauversion von Azure Data Factory v2, die auch Azure-SSIS IR enthält, unterstützt folgende Optionen:
-   Standard\_A4\_v2
-   Standard\_A8\_v2
-   Standard\_D1\_v2
-   Standard\_D2\_v2
-   Standard\_D3\_v2
-   Standard\_D4\_v2

Gemäß den inoffiziellen internen Tests des SSIS-Entwicklerteams scheint die D-Serie besser für die SSIS-Paketausführung geeignet zu sein als die A-Serie.

-   Die D-Serie bietet ein besseres Preis-Leistungs-Verhältnis als die A-Serie.
-   Bei der D-Serie wird für den gleichen Preis ein höherer Durchsatz erzielt als bei der A-Serie.

### <a name="configure-for-execution-speed"></a>Konfigurieren für Ausführungsgeschwindigkeit
Wenn Sie nicht viele Pakete ausführen müssen und die Pakete schnell ausgeführt werden sollen, wählen Sie anhand der Informationen im folgenden Diagramm einen geeigneten VM-Typ für Ihr Szenario aus.

Diese Daten stellen eine einzelne Paketausführung auf einem einzelnen Workerknoten dar. Das Paket lädt zehn Millionen Datensätze mit Vor- und Nachnamespalten aus Azure Blob Storage herunter, erstellt eine Spalte für den vollständigen Namen und schreibt Datensätze, bei denen der vollständige Name mehr als 20 Zeichen umfasst, in Azure Blob Storage.

![SSIS Integration Runtime: Paketausführungsgeschwindigkeit](media/configure-azure-ssis-integration-runtime-performance/ssisir-execution-speed.png)

### <a name="configure-for-overall-throughput"></a>Konfigurieren für allgemeinen Durchsatz

Wenn Sie viele Pakete ausführen müssen und für Sie der allgemeine Durchsatz im Vordergrund steht, wählen Sie anhand der Informationen im folgenden Diagramm einen geeigneten VM-Typ für Ihr Szenario aus.

![SSIS Integration Runtime: Maximaler allgemeiner Durchsatz](media/configure-azure-ssis-integration-runtime-performance/ssisir-overall-throughput.png)

## <a name="azuressisnodenumber"></a>AzureSSISNodeNumber

**AzureSSISNodeNumber** passt die Integration Runtime-Skalierbarkeit an. Der Integration Runtime-Durchsatz ist proportional zu **AzureSSISNodeNumber**. Legen Sie **AzureSSISNodeNumber** zunächst auf einen niedrigen Wert fest, überwachen Sie den Integration Runtime-Durchsatz, und passen Sie den Wert anschließend für Ihr Szenario an. Informationen zum Ändern der Konfiguration der Workerknotenanzahl finden Sie unter [Verwalten einer Azure-SSIS-Integrationslaufzeit](manage-azure-ssis-integration-runtime.md).

## <a name="azuressismaxparallelexecutionspernode"></a>AzureSSISMaxParallelExecutionsPerNode

Wenn Sie bereits einen leistungsstarken Workerknoten für die Paketausführung verwenden, lässt sich der allgemeine Integration Runtime-Durchsatz ggf. durch Erhöhen von **AzureSSISMaxParallelExecutionsPerNode** steigern. Für Standard_D1_v2-Knoten werden 1 bis 4 parallele Ausführungen pro Knoten unterstützt. Für alle anderen Arten von Knoten werden 1 bis 8 parallele Ausführungen pro Knoten unterstützt.
Einen geeigneten ungefähren Wert können Sie auf der Grundlage der Kosten für Ihr Paket und der folgenden Konfigurationen für Workerknoten ermitteln. Weitere Informationen finden Sie unter [Universelle VM-Größen](../virtual-machines/windows/sizes-general.md).

| Größe             | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Maximaler Durchsatz (temporärer Speicher): IOPS/Lesen (MBit/s)/Schreiben (MBit/s) | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbps) |
|------------------|------|-------------|------------------------|------------------------------------------------------------|-----------------------------------|------------------------------------------------|
| Standard\_D1\_v2 | 1    | 3,5         | 50                     | 3000/46/23                                             | 2/2 x 500                         | 2/750                                        |
| Standard\_D2\_v2 | 2    | 7           | 100                    | 6000/93/46                                             | 4/4 x 500                         | 2/1500                                       |
| Standard\_D3\_v2 | 4    | 14          | 200                    | 12000/187/93                                           | 8/8 x 500                         | 4/3000                                       |
| Standard\_D4\_v2 | 8    | 28          | 400                    | 24000/375/187                                          | 16/16 x 500                       | 8/6000                                       |
| Standard\_A4\_v2 | 4    | 8           | 40                     | 4000/80/40                                             | 8/8 x 500                         | 4/1000                                       |
| Standard\_A8\_v2 | 8    | 16          | 80                     | 8000/160/80                                            | 16/16 x 500                       | 8/2000                                       |

Richtlinien für das Festlegen des passenden Werts für die Eigenschaft **AzureSSISMaxParallelExecutionsPerNode**: 

1. Verwenden Sie zunächst einen niedrigen Wert.
2. Erhöhen Sie ihn geringfügig, um zu überprüfen, ob sich der Durchsatz insgesamt verbessert.
3. Erhöhen Sie den Wert nicht weiter, wenn der allgemeine Durchsatz den Maximalwert erreicht.

## <a name="ssisdbpricingtier"></a>SSISDBPricingTier

**SSISDBPricingTier** ist der Tarif für die SSIS-Katalogdatenbank (SSIS Catalog Database, SSISDB) in einer Azure SQL-Datenbank. Diese Einstellung beeinflusst die maximale Anzahl von Workern in der IR-Instanz, die Geschwindigkeit beim Hinzufügen einer Paketausführung zur Warteschlange und die Geschwindigkeit beim Laden des Ausführungsprotokolls.

-   Wenn die Geschwindigkeit beim Hinzufügen einer Paketausführung zur Warteschlange und beim Laden des Ausführungsprotokolls für Sie nicht relevant ist, können Sie den niedrigsten Datenbanktarif verwenden. Azure SQL-Datenbank mit Basic-Tarif unterstützt acht Worker in einer Integration Runtime-Instanz.

-   Wählen Sie bei Verwendung von mehr als acht Workern oder mehr als 50 Kernen eine leistungsfähigere Datenbank als Basic. Andernfalls wird die Datenbank zum Engpass der Integration Runtime-Instanz und beeinträchtigt die allgemeine Leistung.

Der Datenbanktarif kann auch auf der Grundlage der im Azure-Portal verfügbaren Informationen zur Nutzung von [Datenbanktransaktionseinheiten](../sql-database/sql-database-what-is-a-dtu.md) (Database Transaction Units, DTUs) angepasst werden.

## <a name="design-for-high-performance"></a>Entwerfen für hohe Leistung
Das Entwerfen eines SSIS-Pakets für die Ausführung in Azure unterscheidet sich vom Entwerfen eines Pakets für die lokale Ausführung. Unabhängige Aufgaben werden hier nicht im gleichen Paket zusammengefasst, sondern auf mehrere Pakete aufgeteilt, um eine effizientere Ausführung in Azure-SSIS IR zu erreichen. Erstellen Sie für jedes Paket eine Paketausführung, damit nicht auf den Abschluss der jeweils anderen Paketausführungen gewartet werden muss. Dieser Ansatz macht sich die Skalierbarkeit von Azure-SSIS Integration Runtime zunutze und verbessert den allgemeinen Durchsatz.

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich ausführlicher über Azure-SSIS Integration Runtime: [Azure-SSIS-Integrationslaufzeit](concepts-integration-runtime.md#azure-ssis-integration-runtime)
