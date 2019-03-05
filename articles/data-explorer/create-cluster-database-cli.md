---
title: 'Schnellstart: Erstellen eines Azure Data Explorer-Clusters und einer Datenbank über die Befehlszeilenschnittstelle'
description: Hier erfahren Sie, wie Sie über die Azure-Befehlszeilenschnittstelle einen Azure Data Explorer-Cluster und eine Datenbank erstellen.
services: data-explorer
author: radennis
ms.author: radennis
ms.reviewer: orspod
ms.service: data-explorer
ms.topic: quickstart
ms.date: 2/4/2019
ms.openlocfilehash: 357f0efcf7300545d10113c92702d9fed4aad049
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2019
ms.locfileid: "56958014"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-the-cli"></a>Erstellen eines Azure Data Explorer-Clusters und einer Datenbank über die Befehlszeilenschnittstelle

In dieser Schnellstartanleitung wird beschrieben, wie Sie über die Azure-Befehlszeilenschnittstelle (Azure CLI) einen Azure Data Explorer-Cluster und eine Datenbank erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diese Schnellstartanleitung mindestens die Azure CLI-Version 2.0.4 verwenden. Führen Sie `az --version` aus, um Ihre Version zu überprüfen. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="configure-the-cli-parameters"></a>Konfigurieren der CLI-Parameter

Die folgenden Schritte sind nicht erforderlich, wenn Sie Befehle in Azure Cloud Shell ausführen. Wenn Sie die Befehlszeilenschnittstelle lokal ausführen, gehen Sie wie folgt vor, um sich bei Azure anzumelden und Ihr aktuelles Abonnement festzulegen:

1. Führen Sie den folgenden Befehl aus, um sich bei Azure anzumelden:

    ```azurecli-interactive
    az login
    ```

2. Legen Sie das Abonnement fest, in dem der Cluster erstellt werden soll. Ersetzen Sie `MyAzureSub` durch den Namen des gewünschten Azure-Abonnements:

    ```azurecli-interactive
    az account set --subscription MyAzureSub
    ```

## <a name="create-the-azure-data-explorer-cluster"></a>Erstellen des Azure Data Explorer-Clusters

1. Erstellen Sie Ihren Cluster mit dem folgenden Befehl:

    ```azurecli-interactive
    az kusto cluster create --name azureclitest --sku D11_v2 --resource-group testrg
    ```

   |**Einstellung** | **Empfohlener Wert** | **Feldbeschreibung**|
   |---|---|---|
   | name | *azureclitest* | Der gewünschte Name Ihres Clusters.|
   | sku | *D13_v2* | Die SKU, die für Ihren Cluster verwendet wird. |
   | resource-group | *testrg* | Der Name der Ressourcengruppe, in der der Cluster erstellt werden soll |

    Sie können auch noch weitere optionale Parameter verwenden, etwa die Kapazität des Clusters.

2. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob Ihr Cluster erfolgreich erstellt wurde:

    ```azurecli-interactive
    az kusto cluster show --name azureclitest --resource-group testrg
    ```

Wenn das Ergebnis `provisioningState` mit dem Wert `Succeeded` enthält, wurde der Cluster erfolgreich erstellt.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Erstellen der Datenbank im Azure Data Explorer-Cluster

1. Erstellen Sie Ihre Datenbank mit dem folgenden Befehl:

    ```azurecli-interactive
    az kusto database create --cluster-name azureclitest --name clidatabase --resource-group testrg --soft-delete-period 3650:00:00:00 --hot-cache-period 3650:00:00:00
    ```

   |**Einstellung** | **Empfohlener Wert** | **Feldbeschreibung**|
   |---|---|---|
   | cluster-name | *azureclitest* | Der Name Ihres Clusters, in dem die Datenbank erstellt werden soll.|
   | name | *clidatabase* | Der Name Ihrer Datenbank.|
   | resource-group | *testrg* | Der Name der Ressourcengruppe, in der der Cluster erstellt werden soll |
   | soft-delete-period | *3650:00:00:00* | Der Zeitraum, für den Daten für Abfragen verfügbar sein sollen. |
   | hot-cache-period | *3650:00:00:00* | Der Zeitraum, für den Daten im Cache verfügbar sein sollen. |

2. Führen Sie den folgenden Befehl aus, um die erstellte Datenbank anzuzeigen:

    ```azurecli-interactive
    az kusto database show --name clidatabase --resource-group testrg --cluster-name azureclitest
    ```

Jetzt verfügen Sie über einen Cluster und eine Datenbank.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

* Wenn Sie unsere anderen Schnellstarts und Tutorials durchgehen möchten, behalten Sie die erstellten Ressourcen bei.
* Löschen Sie den Cluster, um Ressourcen zu bereinigen. Wenn Sie einen Cluster löschen, werden auch alle darin enthaltenen Datenbanken gelöscht. Verwenden Sie den folgenden Befehl, um Ihren Cluster zu löschen:

    ```azurecli-interactive
    az kusto cluster delete --name azureclitest --resource-group testrg
    ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Erfassen von Daten mit der Azure Data Explorer-Bibliothek für Python](python-ingest-data.md)
