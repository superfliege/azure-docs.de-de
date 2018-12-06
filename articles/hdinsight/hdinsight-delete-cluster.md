---
title: 'Gewusst wie: Löschen eines HDInsight-Clusters – Azure'
description: Informationen zu den verschiedenen Möglichkeiten, einen HDInsight-Cluster zu löschen.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 707112f755e6846622d2fde9de466551276925b8
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52494364"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-classic-cli"></a>Löschen eines HDInsight-Clusters mithilfe von Browser, PowerShell oder der klassischen Azure CLI

Die Abrechnung für einen HDInsight-Cluster beginnt, sobald der Cluster erstellt wurde, und endet mit dem Löschen des Clusters. Die Gebühren werden anteilig nach Minuten erhoben. Daher sollten Sie Ihren Cluster immer löschen, wenn Sie ihn nicht mehr verwenden. In diesem Dokument erfahren Sie, wie Sie einen Cluster mit dem [Azure-Portal](https://portal.azure.com), mit [Azure PowerShell](https://docs.microsoft.com/powershell/azure/) und der klassischen Azure CLI löschen.

> [!IMPORTANT]
> Das Löschen eines HDInsight-Clusters führt nicht zum Löschen der zugeordneten Azure Storage-Konten oder Data Lake Store-Instanz. Sie können die in diesen Diensten gespeicherten Daten künftig wiederverwenden.

## <a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich im [Azure-Portal](https://portal.azure.com) an, und wählen Sie Ihren HDInsight-Cluster aus. Wenn Ihr HDInsight-Cluster nicht mit dem Dashboard verknüpft ist, können Sie ihn anhand seines Namens über das Suchfeld suchen.
   
    ![Portalsuche](./media/hdinsight-delete-cluster/navbar.png)

2. Wählen Sie aus den Clustereinstellungen das Symbol **Löschen** aus. Wenn Sie dazu aufgefordert werden, wählen Sie **Ja**, um den Cluster zu löschen.
   
    ![Löschen-Symbol](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a>Azure PowerShell

Verwenden Sie an einer PowerShell-Eingabeaufforderung den folgenden Befehl zum Löschen des Clusters:

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres HDInsight-Clusters.

## <a name="azure-classic-cli"></a>Die klassische Azure CLI

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

Verwenden Sie an einer Eingabeaufforderung den folgenden Befehl zum Löschen des Clusters:

    azure hdinsight cluster delete CLUSTERNAME

Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres HDInsight-Clusters.
