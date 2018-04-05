---
title: Erstellen eines Kontos für die Azure Cosmos DB-MongoDB-API
description: Hier erfahren Sie, wie Sie über das Azure-Portal ein Konto für die Azure Cosmos DB-MongoDB-API erstellen.
services: cosmos-db
author: mimig1
ms.service: cosmos-db
ms.topic: include
ms.date: 03/20/2018
ms.author: mimig
ms.custom: include file
ms.openlocfilehash: 02ea0e011642313b885bc48ec48104fa2789da81
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
1. Melden Sie sich in einem neuen Fenster beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie im linken Menü auf **Ressource erstellen** > **Datenbanken** und anschließend unter **Azure Cosmos DB** auf **Erstellen**.
   
   ![Screenshot des Azure-Portals mit Hervorhebung von „Weitere Dienste“ und „Azure Cosmos DB“](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)

3. Geben Sie auf dem Blatt **Neues Konto** als API die Option **MongoDB** an, und legen Sie die gewünschte Konfiguration für das Azure Cosmos DB-Konto fest.
 
    ![Screenshot des Blatts „Neue Azure Cosmos DB“](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-2.png)

    * Bei der **ID** muss es sich um einen eindeutigen Namen handeln, den Sie für die Identifikation Ihres Azure Cosmos DB-Kontos verwenden möchten. Die ID darf nur Kleinbuchstaben, Ziffern und Bindestriche enthalten und muss zwischen drei und 50 Zeichen lang sein.
    * **Abonnement** ist Ihr Azure-Abonnement. Diese Angabe wird automatisch ausgefüllt.
    * **Ressourcengruppe** ist der Ressourcengruppenname für Ihr Azure Cosmos DB-Konto.
    * **Standort** ist der geografische Ort, an dem sich Ihre Azure Cosmos DB-Instanz befindet. Wählen Sie den Standort aus, der Ihren Benutzern am nächsten ist.

4. Klicken Sie auf **Erstellen** , um das Konto zu erstellen.
5. Klicken Sie in der Symbolleiste auf **Benachrichtigungen**, um den Bereitstellungsprozess zu überwachen.

    ![Benachrichtigung „Bereitstellung gestartet“](./media/cosmos-db-create-dbaccount-mongodb/azure-documentdb-nosql-notification.png)

6.  Wenn die Bereitstellung abgeschlossen wurde, öffnen Sie das neue Konto über die Kachel „Alle Ressourcen“. 

    ![Azure Cosmos DB-Konto auf der Kachel „Alle Ressourcen“](./media/cosmos-db-create-dbaccount-mongodb/azure-documentdb-all-resources.png)
