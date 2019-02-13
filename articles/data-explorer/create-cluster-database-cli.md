---
title: Erstellen eines Azure Data Explorer-Clusters und einer Datenbank mit der CLI
description: In diesem Artikel wird beschrieben, wie Sie mit der Azure-Befehlszeilenschnittstelle (Azure CLI) einen Azure Data Explorer-Cluster und eine Datenbank erstellen.
services: data-explorer
author: radennis
ms.author: radennis
ms.reviewer: orspod
ms.service: data-explorer
ms.topic: howto
ms.date: 1/31/2019
ms.openlocfilehash: a4c9156ef80f05e247b1cfef0acd56b601a2db65
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55812683"
---
# <a name="create-an-azure-data-explorer-cluster-and-a-database-using-cli"></a>Erstellen eines Azure Data Explorer-Clusters und einer Datenbank mit der CLI

In diesem Artikel wird beschrieben, wie Sie mit der Azure-Befehlszeilenschnittstelle (Azure CLI) einen Azure Data Explorer-Cluster und eine Datenbank erstellen.

## <a name="whats-the-difference-between-the-management-plane-and-data-plane-apis"></a>Was ist der Unterschied zwischen den Verwaltungsebenen- und Datenebenen-APIs?

Es gibt zwei verschiedene API-Bibliotheken, Verwaltungs- und Datenebenen-APIs.
Die Verwaltungs-APIs werden zum Verwalten der Ressourcen verwendet, beispielsweise zum Erstellen eines Clusters, zum Erstellen einer Datenbank, zum Löschen einer Datenverbindung, zum Ändern der Anzahl von Instanzen usw. Die Datenebenen-APIs werden verwendet, um mit den Daten zu interagieren, Abfragen auszuführen, Daten zu erfassen usw.

## <a name="configure-the-cli-parameters"></a>Konfigurieren der CLI-Parameter

Anmelden am Konto

```Bash
az login
```

Legen Sie das Abonnement fest, in dem der Cluster erstellt werden soll.

```Bash
az account set --subscription "your_subscription"
```

## <a name="create-the-azure-data-explorer-cluster"></a>Erstellen des Azure Data Explorer-Clusters

Erstellen Sie Ihren Cluster mit dem folgenden Befehl.

```Bash
az kusto cluster create --name azureclitest --sku D11_v2 --resource-group testrg
```

Geben Sie die folgenden Werte an:

   |**Einstellung** | **Empfohlener Wert** | **Feldbeschreibung**|
   |---|---|---|
   | name | *azureclitest* | Der gewünschte Name Ihres Clusters.|
   | sku | *D13_v2* | Die SKU, die für Ihren Cluster verwendet wird. |
   | resource-group | *testrg* | Der Name der Ressourcengruppe, in der der Cluster erstellt werden soll. |

Wenn Sie möchten, können Sie auch noch weitere optionale Parameter verwenden, z. B. die Kapazität des Clusters usw.

Um zu überprüfen, ob Ihr Cluster erfolgreich erstellt wurde, können Sie den folgenden Befehl ausführen:

```Bash
az kusto cluster show --name azureclitest --resource-group testrg
```

Wenn das Ergebnis „provisioningState“ mit dem Wert „Succeeded“ enthält, bedeutet dies, dass der Cluster erfolgreich erstellt wurde.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Erstellen der Datenbank im Azure Data Explorer-Cluster

Erstellen Sie Ihre Datenbank mit dem folgenden Befehl.

```Bash
az kusto database create --cluster-name azureclitest --name clidatabase --resource-group testrg --soft-delete-period 3650:00:00:00 --hot-cache-period 3650:00:00:00
```

Geben Sie die folgenden Werte an:

   |**Einstellung** | **Empfohlener Wert** | **Feldbeschreibung**|
   |---|---|---|
   | cluster-name | *azureclitest* | Der Name Ihres Clusters, in dem die Datenbank erstellt werden soll.|
   | name | *clidatabase* | Der gewünschte Name Ihrer Datenbank.|
   | resource-group | *testrg* | Der Name der Ressourcengruppe, in der der Cluster erstellt werden soll. |
   | soft-delete-period | *3650:00:00:00* | Der Zeitraum, in dem die Daten beibehalten werden sollen, damit sie für Abfragen verfügbar sind. |
   | hot-cache-period | *3650:00:00:00* | Der Zeitraum, in dem die Daten im Cache gespeichert werden sollen. |

Durch Ausführen des folgenden Befehls können Sie die von Ihnen erstellte Datenbank anzeigen.

```Bash
az kusto database show --name clidatabase --resource-group testrg --cluster-name azureclitest
```

Das war's auch schon. Jetzt verfügen Sie über einen Cluster und eine Datenbank.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie unsere anderen Schnellstarts und Tutorials durchgehen möchten, behalten Sie die erstellten Ressourcen bei.

Wenn Sie einen Cluster löschen, werden auch alle darin enthaltenen Datenbanken gelöscht. Führen Sie den folgenden Befehl aus, um Ihren Cluster zu löschen.

```Bash
az kusto cluster delete --name azureclitest --resource-group testrg
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Erfassen von Daten aus Event Hub in Azure Data Explorer](ingest-data-event-hub.md)
