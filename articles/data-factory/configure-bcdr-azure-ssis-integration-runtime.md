---
title: BCDR-Empfehlungen (Business Continuity & Disaster Recovery) für die Azure-SSIS-Integration Runtime | Microsoft-Dokumentation
description: In diesem Artikel werden BCDR-Empfehlungen für die Azure-SSIS-Integration Runtime beschrieben.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 07/26/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 37347df2d543116085f52fed76c692b60fac2ad6
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39295225"
---
# <a name="business-continuity-and-disaster-recovery-bcdr-recommendations-for-azure-ssis-integration-runtime"></a>BCDR-Empfehlungen (Business Continuity & Disaster Recovery) für die Azure-SSIS-Integration Runtime

Zum Zweck der Notfallwiederherstellung können Sie die Azure-SSIS-Integration Runtime in der Region beenden, in der sie derzeit ausgeführt wird, und zu einer anderen Region wechseln, um sie erneut zu starten. Dafür wird die Verwendung von [Azure-Regionspaaren](../best-practices-availability-paired-regions.md) empfohlen.

## <a name="prerequisites"></a>Voraussetzungen

- Stellen Sie sicher, dass die Notfallwiederherstellung für den Azure SQL-Datenbankserver aktiviert ist, falls beim Server zum gleichen Zeitpunkt ein Ausfall auftritt. Weitere Informationen finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](../sql-database/sql-database-business-continuity.md).

- Wenn Sie ein virtuelles Netzwerk in der aktuellen Region verwenden, müssen Sie ein anderes virtuelles Netzwerk in der neuen Region verwenden, um eine Verbindung mit der Azure-SSIS-Integration Runtime herzustellen. Weitere Informationen finden Sie unter [Verknüpfen einer Azure-SSIS-Integration Runtime mit einem virtuellen Netzwerk](join-azure-ssis-integration-runtime-virtual-network.md).

- Bei Verwendung eines benutzerdefinierten Setups müssen Sie unter Umständen einen weiteren SAS-URI für den Blobcontainer vorbereiten, in dem das benutzerdefinierte Setupskript und die zugehörigen Dateien gespeichert sind, damit er auch während eines Ausfalls zugänglich ist. Weitere Informationen finden Sie unter [Anpassen des Setups für Azure-SSIS-Integration Runtime](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="steps"></a>Schritte

Führen Sie die folgenden Schritte aus, um die Azure-SSIS-IR anzuhalten, auf eine neue Region umzustellen und wieder zu starten.

1. Beenden Sie die IR in der ursprünglichen Region.

2. Führen Sie den folgenden Befehl in PowerShell aus, um die IR zu aktualisieren:

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -Location "new region" `
                    -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                    -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                    -VNetId "new VNet" `
                    -Subnet "new subnet" `
                    -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

    Weitere Informationen zu diesem PowerShell-Befehl finden Sie unter [Erstellen der Azure-SSIS-Integration Runtime in Azure Data Factory](create-azure-ssis-integration-runtime.md).

3. Starten Sie IR erneut.

## <a name="next-steps"></a>Nächste Schritte

Ziehen Sie diese weiteren Konfigurationsoptionen für die Azure-SSIS-IR in Betracht:

- [Konfigurieren der Azure-SSIS-Integration Runtime für hohe Leistung](configure-azure-ssis-integration-runtime-performance.md)

- [Anpassen des Setups für Azure-SSIS-Integration Runtime](how-to-configure-azure-ssis-ir-custom-setup.md)

- [Bereitstellen der Enterprise-Edition für die Azure-SSIS-Integration Runtime](how-to-configure-azure-ssis-ir-enterprise-edition.md)
