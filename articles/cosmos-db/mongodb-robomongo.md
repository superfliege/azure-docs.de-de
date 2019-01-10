---
title: Herstellen einer Verbindung mit Azure Cosmos DB mithilfe von Robo 3T
description: Erfahren Sie, wie eine Verbindung mit Azure Cosmos DB mithilfe von Robo 3T und der API für MongoDB von Azure Cosmos DB herstellen.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 12/26/2018
author: sivethe
ms.author: sivethe
ms.openlocfilehash: 5696c376ad64df01d7f9d43ff59c87402c334c52
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54034810"
---
# <a name="use-robo-3t-with-azure-cosmos-dbs-api-for-mongodb"></a>Verwenden von Robo 3T mit der API für MongoDB von Azure Cosmos DB

Um mithilfe von Robo 3T eine Verbindung mit einem Cosmos-Konto herzustellen, müssen Sie die folgenden Aktionen ausführen:

* Herunterladen und Installieren von [Robo 3T](https://robomongo.org/)
* Abrufen der Informationen zur Cosmos DB-[Verbindungszeichenfolge](connect-mongodb-account.md)

## <a name="connect-using-robo-3t"></a>Herstellen einer Verbindung mit Robo 3T
Führen Sie die folgenden Schritte aus, um dem Robo 3T-Verbindungs-Manager Ihr Cosmos-Konto hinzuzufügen:

1. Rufen Sie die Verbindungsinformationen für Ihr Cosmos-Konto ab, das mit der API für MongoDB von Azure Cosmos DB konfiguriert wurde. Verwenden Sie dazu die [hier](connect-mongodb-account.md) beschriebenen Anweisungen.

    ![Screenshot des Blatts „Verbindungszeichenfolge“](./media/mongodb-robomongo/connectionstringblade.png)
2. Führen Sie *Robomongo.exe* aus.

3. Klicken Sie unter **Datei** auf die Verbindungsschaltfläche, um Ihre Verbindungen zu verwalten. Klicken Sie anschließend im Fenster mit den **MongoDB-Verbindungen** auf **Erstellen**. Daraufhin öffnet sich das Fenster mit den **Verbindungseinstellungen**.

4. Wählen Sie im Fenster mit den **Verbindungseinstellungen** einen Namen aus. Ermitteln Sie anschließend den **Host** und den **Port** in Ihren Verbindungsinformationen aus Schritt 1, und geben Sie sie unter **Adresse** bzw. **Port** ein.

    ![Screenshot der Verbindungsverwaltung von Robomongo](./media/mongodb-robomongo/manageconnections.png)
5. Klicken Sie auf der Registerkarte **Authentifizierung** auf **Perform authentication** (Authentifizierung durchführen). Geben Sie anschließend die Datenbank (Standardeinstellung: *Admin*) sowie **Benutzername** und **Kennwort** ein.
Sowohl **Benutzername** als auch **Kennwort** finden Sie in den Verbindungsinformationen aus Schritt 1.

    ![Screenshot der Authentifizierungsregisterkarte von Robomongo](./media/mongodb-robomongo/authentication.png)
6. Aktivieren Sie auf der Registerkarte **SSL** das Kontrollkästchen **Use SSL protocol** (SSL-Protokoll verwenden), und ändern Sie anschließend die **Authentifizierungsmethode** in **Selbstsigniertes Zertifikat**.

    ![Screenshot der SSL-Registerkarte von Robomongo](./media/mongodb-robomongo/SSL.png)
7. Klicken Sie abschließend auf **Test**, um sich zu vergewissern, dass die Verbindung hergestellt werden kann, und klicken Sie schließlich auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [Studio 3T](mongodb-mongochef.md) mit der API für MongoDB von Azure Cosmos DB verwenden.
- Untersuchen Sie MongoDB-[Beispiele](mongodb-samples.md) mit der API für MongoDB von Azure Cosmos DB.
