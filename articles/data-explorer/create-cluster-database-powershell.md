---
title: 'Schnellstart: Erstellen eines Azure Data Explorer-Clusters und einer Datenbank mithilfe von PowerShell'
description: Hier erfahren Sie, wie Sie mithilfe von PowerShell einen Azure Data Explorer-Cluster und eine Datenbank erstellen.
services: data-explorer
author: oflipman
ms.author: oflipman
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: quickstart
ms.date: 03/17/2019
ms.openlocfilehash: 650bdc5cdf99645bc2be6c8e85737dacd10a6b27
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58287362"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-powershell"></a>Erstellen eines Azure Data Explorer-Clusters und einer Datenbank mithilfe von PowerShell

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [BEFEHLSZEILENSCHNITTSTELLE (CLI)](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
>  


In dieser Schnellstartanleitung wird beschrieben, wie Sie mithilfe von PowerShell einen Azure Data Explorer-Cluster und eine Datenbank erstellen.

Sie können PowerShell-Cmdlets und -Skripts unter Windows oder Linux oder in der [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) ausführen, um [Azure Data Explorer](https://docs.microsoft.com/azure/kusto/ ) zu erstellen und zu konfigurieren.

[**Az.Kusto**](https://docs.microsoft.com/powershell/module/az.kusto/?view=azps-1.4.0#kusto ). Mit Azure PowerShell und **Az.Kusto** können Sie folgende Aufgaben ausführen:

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.

## <a name="configure-parameters"></a>Konfigurieren von Parametern

Die folgenden Schritte sind nicht erforderlich, wenn Sie Befehle in Azure Cloud Shell ausführen. Wenn Sie die Befehlszeilenschnittstelle lokal ausführen, gehen Sie wie folgt vor, um sich bei Azure anzumelden und Ihr aktuelles Abonnement festzulegen:

1. Führen Sie den folgenden Befehl aus, um sich bei Azure anzumelden:

    ```azurepowershell-interactive
    Connect-AzAccount
    ```

2. Legen Sie das Abonnement fest, in dem der Cluster erstellt werden soll.

    ```azurepowershell-interactive
     Set-AzContext -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ```
3. Installieren Sie das Az.Kusto-Modul auf Ihrem Gerät:
    
    ```azurepowershell-interactive
     Install-Module -Name Az.Kusto  
    ```

## <a name="create-the-azure-data-explorer-cluster"></a>Erstellen des Azure Data Explorer-Clusters

1. Erstellen Sie Ihren Cluster mit dem folgenden Befehl:

    ```azurepowershell-interactive
     New-AzKustoCluster -ResourceGroupName testrg -Name mykustocluster -Location 'Central US' -Sku D13_v2 -Capacity 10
    ```

   |**Einstellung** | **Empfohlener Wert** | **Feldbeschreibung**|
   |---|---|---|
   | NAME | *mykustocluster* | Der gewünschte Name Ihres Clusters.|
   | Sku | *D13_v2* | Die SKU, die für Ihren Cluster verwendet wird. |
   | ResourceGroupName | *testrg* | Der Name der Ressourcengruppe, in der der Cluster erstellt werden soll |

    Sie können auch noch weitere optionale Parameter verwenden, etwa die Kapazität des Clusters.

2. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob Ihr Cluster erfolgreich erstellt wurde:

    ```azurepowershell-interactive
    Get-AzKustoCluster -Name mykustocluster --ResourceGroupName testrg
    ```

Wenn das Ergebnis `provisioningState` mit dem Wert `Succeeded` enthält, wurde der Cluster erfolgreich erstellt.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Erstellen der Datenbank im Azure Data Explorer-Cluster

1. Erstellen Sie Ihre Datenbank mit dem folgenden Befehl:

    ```azurepowershell-interactive
    New-AzKustoDatabase -ResourceGroupName testrg -ClusterName mykustocluster -Name mykustodatabase -SoftDeletePeriod 3650:00:00:00 -HotCachePeriod 3650:00:00:00
    ```

   |**Einstellung** | **Empfohlener Wert** | **Feldbeschreibung**|
   |---|---|---|
   | ClusterName | *mykustocluster* | Der Name Ihres Clusters, in dem die Datenbank erstellt werden soll.|
   | NAME | *mykustodatabase* | Der Name Ihrer Datenbank.|
   | ResourceGroupName | *testrg* | Der Name der Ressourcengruppe, in der der Cluster erstellt werden soll |
   | SoftDeletePeriod | *3650:00:00:00* | Der Zeitraum, für den Daten für Abfragen verfügbar sein sollen. |
   | HotCachePeriod | *3650:00:00:00* | Der Zeitraum, für den Daten im Cache verfügbar sein sollen. |

2. Führen Sie den folgenden Befehl aus, um die erstellte Datenbank anzuzeigen:

    ```azurepowershell-interactive
    Get-AzKustoDatabase -ClusterName mykustocluster --ResourceGroupName testrg -Name mykustodatabase
    ```

Jetzt verfügen Sie über einen Cluster und eine Datenbank.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

* Wenn Sie unsere anderen Schnellstarts und Tutorials durchgehen möchten, behalten Sie die erstellten Ressourcen bei.
* Löschen Sie den Cluster, um Ressourcen zu bereinigen. Wenn Sie einen Cluster löschen, werden auch alle darin enthaltenen Datenbanken gelöscht. Verwenden Sie den folgenden Befehl, um Ihren Cluster zu löschen:

    ```azurepowershell-interactive
    Remove-AzKustoCluster -ResourceGroupName testrg -Name mykustocluster
    ```

## <a name="next-steps"></a>Nächste Schritte

Weitere Az.Kusto-Befehle finden Sie [**hier**](https://docs.microsoft.com/powershell/module/az.kusto/?view=azps-1.4.0#kusto ).

> [!div class="nextstepaction"]
> [Schnellstart: Erfassen von Daten mit dem .NET Standard SDK für Azure Data Explorer (Vorschauversion)](net-standard-ingest-data.md)
