---
title: Verwalten von Apache Hadoop-Clustern mit der klassischen Azure CLI – Azure HDInsight
description: Erfahren Sie, wie Sie mit der klassischen Azure CLI Apache Hadoop-Cluster in Azure HDInsight verwalten.
services: hdinsight
ms.reviewer: jasonh
author: tylerfox
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: tyfox
ms.openlocfilehash: 4de4674d8a4c2b573df12648739971e460531636
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52495104"
---
# <a name="manage-apache-hadoop-clusters-in-hdinsight-using-the-azure-classic-cli"></a>Verwalten von Apache Hadoop-Clustern in HDInsight mit der klassischen Azure CLI
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Hier erfahren Sie, wie Sie [Apache Hadoop](https://hadoop.apache.org/)-Cluster in Azure HDInsight mit der [klassischen Azure CLI](../cli-install-nodejs.md) verwalten. Die klassische CLI ist in Node.js implementiert. Sie kann auf allen Plattformen verwendet werden, die Node.js unterstützen, inklusive Windows, Mac und Linux.

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Artikel beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Die klassische Azure CLI:** Informationen zur Installation und Konfiguration finden Sie unter [Install and configure the Azure Classic CLI (Installieren und Konfigurieren der klassischen Azure CLI)](../cli-install-nodejs.md).
* Stellen Sie über den folgenden Befehl **eine Verbindung mit Azure** her:

    ```cli
    azure login
    ```
  
    Weitere Informationen zur Authentifizierung mit einem Geschäfts-, Schul- oder Unikonto finden Sie unter [Connect to an Azure subscription from the Azure Classic CLI (Herstellen einer Verbindung mit einem Azure-Abonnement über die klassische Azure CLI)](/cli/azure/authenticate-azure-cli).
* **Wechseln Sie in den Azure-Ressourcen-Manager-Modus:**
  
    ```cli
    azure config mode arm
    ```

Um Hilfe zu erhalten, verwenden Sie die Option **-h** .  Beispiel: 

```cli
azure hdinsight cluster create -h
```

## <a name="create-clusters-with-the-cli"></a>Erstellen von Clustern mit der Befehlszeilenschnittstelle
Weitere Informationen finden Sie unter [Create clusters in HDInsight using the Azure Classic CLI (Erstellen von Clustern in HDInsight mit der klassischen Azure CLI)](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

## <a name="list-and-show-cluster-details"></a>Auflisten und Anzeigen von Clusterdetails
Mit den folgenden Befehlen können Sie Clusterdetails auflisten und anzeigen:

```cli
azure hdinsight cluster list
azure hdinsight cluster show <Cluster Name>
```

![Befehlszeilenansicht der Clusterliste][image-cli-clusterlisting]

## <a name="delete-clusters"></a>Löschen von Clustern
Mit dem folgenden Befehl können Sie ein Cluster löschen:

```cli
azure hdinsight cluster delete <Cluster Name>
```

Sie können einen Cluster auch löschen, indem Sie die Ressourcengruppe löschen, die den Cluster enthält. Beachten Sie, dass damit alle Ressourcen in der Gruppe, einschließlich des Standardspeicherkontos, gelöscht werden.

```cli
azure group delete <Resource Group Name>
```

## <a name="scale-clusters"></a>Skalieren von Clustern
So ändern Sie die Apache Hadoop-Clustergröße:

```cli
azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>
```


## <a name="enabledisable-http-access-for-a-cluster"></a>Aktivieren/Deaktivieren des HTTP-Zugriffs für einen Cluster

```cli
azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
azure hdinsight cluster disable-http-access [options] <Cluster Name>
```

## <a name="next-steps"></a>Nächste Schritte
Sie sind nun in der Lage, verschiedene Verwaltungsaufgaben für HDInsight-Cluster auszuführen. Weitere Informationen finden Sie in den folgenden Artikeln:

* [Verwalten von HDInsight mit dem Azure-Portal][hdinsight-admin-portal]
* [Verwalten von HDInsight mit Azure PowerShell][hdinsight-admin-powershell]
* [Erste Schritte mit Azure HDInsight][hdinsight-get-started]
* [How to use the Azure Classic CLI (Verwenden der klassischen Azure CLI)][azure-command-line-tools]

[azure-command-line-tools]: ../cli-install-nodejs.md
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/command-line-list-of-clusters.png "Auflisten und Anzeigen von Clustern"
