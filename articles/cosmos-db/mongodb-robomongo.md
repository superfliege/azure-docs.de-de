---
title: Verwenden von Robomongo für Azure Cosmos DB
description: 'Erfahren Sie, wie Sie Robomongo mit einem Azure Cosmos DB: API für MongoDB-Konto verwenden.'
keywords: Robomongo
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: sngun
ms.openlocfilehash: 78f0158c9a80a60717b81b4788531c7efd979111
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52863804"
---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Verwenden von Robomongo mit einem Azure Cosmos DB: API für MongoDB-Konto verwenden.
Zum Herstellen einer Verbindung mit einem Azure Cosmos DB: API für MongoDB-Konto mithilfe von Robomongo ist Folgendes erforderlich:

* Herunterladen und Installieren von [Robomongo](https://robomongo.org/)
* Verfügbare Informationen zur [Verbindungszeichenfolge](connect-mongodb-account.md) für Ihr Azure Cosmos DB: API für MongoDB-Konto

## <a name="connect-using-robomongo"></a>Herstellen einer Verbindung mithilfe von Robomongo
Führen Sie die folgenden Schritte aus, um den MongoDB-Verbindungen von Robomongo Ihr Azure Cosmos DB: API für MongoDB-Konto hinzuzufügen.

1. Rufen Sie anhand der [hier](connect-mongodb-account.md) angegebenen Anweisungen die Verbindungsinformationen für Ihr DocumentDB: API für MongoDB-Konto ab.

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
* Erkunden Sie [Beispiele](mongodb-samples.md) für Azure Cosmos DB: API für MongoDB.
