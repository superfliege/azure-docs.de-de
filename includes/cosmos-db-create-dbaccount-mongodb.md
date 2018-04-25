---
title: Includedatei
description: Includedatei
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 690c5f07a2b0f7a88e16f0b0bbbaa9ca78e37317
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
1. Melden Sie sich in einem neuen Fenster beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie im linken Menü auf **Ressource erstellen** > **Datenbanken** und anschließend unter **Azure Cosmos DB** auf **Erstellen**.
   
   ![Screenshot des Azure-Portals mit Hervorhebung von „Weitere Dienste“ und „Azure Cosmos DB“](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)

3. Geben Sie auf dem Blatt **Neues Konto** als API die Option **MongoDB** an, und legen Sie die gewünschte Konfiguration für das Azure Cosmos DB-Konto fest.
 
    * Bei der **ID** muss es sich um einen eindeutigen Namen handeln, den Sie für die Identifikation Ihres Azure Cosmos DB-Kontos verwenden möchten. Die ID darf nur Kleinbuchstaben, Ziffern und Bindestriche enthalten und muss zwischen drei und 50 Zeichen lang sein.
    * **Abonnement** ist Ihr Azure-Abonnement. Diese Angabe wird automatisch ausgefüllt.
    * **Ressourcengruppe** ist der Ressourcengruppenname für Ihr Azure Cosmos DB-Konto. Wählen Sie **Neu erstellen** aus, und geben Sie dann einen neuen Ressourcengruppenname für Ihr Konto ein. Der Einfachheit halber können Sie denselben Namen wie bei Ihrer ID verwenden.
    * **Standort** ist der geografische Ort, an dem sich Ihre Azure Cosmos DB-Instanz befindet. Wählen Sie den Standort aus, der Ihren Benutzern am nächsten ist.

    Klicken Sie dann auf **Erstellen**.

    ![Die Seite „Neues Konto“ für Azure Cosmos DB](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-create-new-account.png)

4. Die Kontoerstellung dauert einige Minuten. Warten Sie, bis das Portal die Seite **Herzlichen Glückwunsch! Ihr Azure Cosmos DB-Konto mit MongoDB-API ist jetzt bereit.** anzeigt.

    ![Der Bereich „Benachrichtigungen“ im Azure-Portal](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-account-created.png)
