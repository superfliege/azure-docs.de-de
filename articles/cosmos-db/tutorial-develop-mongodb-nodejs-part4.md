---
title: 'MongoDB-, Angular- und Node-Tutorial für Azure: Teil 4'
description: Teil 4 der Tutorialreihe zur Erstellung einer MongoDB-App mit Angular und Note in Azure Cosmos DB unter Verwendung der gleichen APIs wie für MongoDB
services: cosmos-db
author: johnpapa
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 09/05/2017
ms.author: jopapa
ms.custom: mvc
ms.openlocfilehash: 21fd80156bf81b5ab985459bb43ca167fbe2d579
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52865980"
---
# <a name="create-a-mongodb-app-with-angular-and-azure-cosmos-db---part-4-create-an-azure-cosmos-db-account-using-the-azure-cli"></a>Erstellen einer MongoDB-App mit Angular und Azure Cosmos DB – Teil 4: Erstellen eines Azure Cosmos DB-Kontos mithilfe der Azure-Befehlszeilenschnittstelle

In diesem mehrteiligen Tutorial erfahren Sie, wie Sie eine neue, in Node.js geschriebene [MongoDB-API](mongodb-introduction.md)-App mit Express, Angular Ihrer Azure Cosmos DB-Datenbank erstellen.

Teil 4 des Tutorials baut auf [Teil 3](tutorial-develop-mongodb-nodejs-part3.md) auf und umfasst folgende Aufgaben:

> [!div class="checklist"]
> * Erstellen einer Azure-Ressourcengruppe mithilfe der Azure-Befehlszeilenschnittstelle
> * Erstellen eines Azure Cosmos DB-Kontos mithilfe der Azure CLI

## <a name="video-walkthrough"></a>Exemplarische Vorgehensweise per Video

> [!VIDEO https://www.youtube.com/embed/hfUM-AbOh94]

## <a name="prerequisites"></a>Voraussetzungen

Für diesen Teil des Tutorials wird vorausgesetzt, dass Sie die Schritte aus [Teil 3](tutorial-develop-mongodb-nodejs-part3.md) des Tutorials ausgeführt haben. 

In diesem Tutorial können Sie entweder Azure Cloud Shell (in Ihrem Internetbrowser) oder die lokal installierte [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) verwenden.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Log in to Azure](../../includes/login-to-azure.md)]

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)]

> [!TIP]
> In diesem Tutorial wird die Anwendungserstellung Schritt für Schritt erläutert. Für den Fall, dass Sie das fertige Projekt herunterladen möchten, steht die fertige Anwendung auf GitHub im [Repository „angular-cosmosdb“](https://github.com/Azure-Samples/angular-cosmosdb) zur Verfügung.

## <a name="create-an-azure-cosmos-db-account"></a>Erstellen eines Azure Cosmos DB-Kontos

Erstellen Sie mithilfe des Befehls [`az cosmosdb create`](/cli/azure/cosmosdb#az-cosmosdb-create) ein Azure Cosmos DB-Konto.

```azurecli-interactive
az cosmosdb create --name <cosmosdb-name> --resource-group myResourceGroup --kind MongoDB
```

* Geben Sie für `<cosmosdb-name>` Ihren eigenen Azure Cosmos DB-Kontonamen an. Der Name muss für alle Azure Cosmos DB-Kontonamen in Azure eindeutig sein.
* Die Einstellung `--kind MongoDB` ermöglicht Azure Cosmos DB die Verwendung von MongoDB-Clientverbindungen.

Die Ausführung des Befehls dauert unter Umständen etwas. Nach Abschluss des Befehls werden im Terminalfenster Informationen zur neuen Datenbank angezeigt. 

Gehen Sie nach der Erstellung des Azure Cosmos DB-Kontos wie folgt vor:
1. Öffnen Sie ein neues Browserfenster, und navigieren Sie zu [https://portal.azure.com](https://portal.azure.com).
1. Klicken Sie auf der linken Leiste auf das Azure Cosmos DB-Logo ![Azure Cosmos DB-Symbol im Azure-Portal](./media/tutorial-develop-mongodb-nodejs-part4/azure-cosmos-db-icon.png) . Daraufhin werden alle verfügbaren Azure Cosmos DB-Instanzen angezeigt.
1. Klicken Sie auf das soeben erstellte Azure Cosmos DB-Konto, wählen Sie die Registerkarte **Übersicht** aus, und scrollen Sie nach unten, um die Karte mit dem Standort der Datenbank anzuzeigen. 

    ![Neues Azure Cosmos DB-Konto im Azure-Portal](./media/tutorial-develop-mongodb-nodejs-part4/azure-cosmos-db-angular-portal.png)

4. Scrollen Sie im linken Navigationsbereich nach unten, und klicken Sie auf die Registerkarte **Daten global replizieren**. Daraufhin erscheint eine Karte mit den verschiedenen Regionen, die als Ziele für die Replikation zur Verfügung stehen. So können Sie beispielsweise auf „Australien, Südosten“ oder „Australien, Osten“ klicken und Ihre Daten in Australien replizieren. Weitere Informationen zur globalen Replikation finden Sie unter [Wie werden Daten mit Azure Cosmos DB global verteilt?](distribute-data-globally.md). Der Einfachheit halber verwenden wir vorerst nur eine einzelne Instanz. Bei Bedarf wissen Sie nun aber, wie die Replikation funktioniert.

    ![Neues Azure Cosmos DB-Konto im Azure-Portal](./media/tutorial-develop-mongodb-nodejs-part4/azure-cosmos-db-replicate-portal.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Teil des Tutorials haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Erstellen einer Azure-Ressourcengruppe mithilfe der Azure-Befehlszeilenschnittstelle
> * Erstellen eines Azure Cosmos DB-Kontos mithilfe der Azure-Befehlszeilenschnittstelle

Im nächsten Teil des Tutorials wird Azure Cosmos DB unter Verwendung von Mongoose mit Ihrer App verbunden.

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit Azure Cosmos DB mithilfe von Mongoose](tutorial-develop-mongodb-nodejs-part5.md)
