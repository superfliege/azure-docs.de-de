---
title: Verwenden von Robomongo für Azure Cosmos DB | Microsoft-Dokumentation
description: 'Erfahren Sie, wie Sie Azure Cosmos DB mit Robomongo verwenden: API für MongoDB-Konto.'
keywords: Robomongo
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: sngun
ms.openlocfilehash: b6d64d7d7b30d4175fb8c8bf6c98127465427d4e
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34795029"
---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Verwenden von Robomongo mit Azure Cosmos DB: API für MongoDB-Konto
Zur Verbindung mit Azure Cosmos DB: API für MongoDB-Konto mithilfe von Robomongo müssen Sie Folgendes durchführen:

* Herunterladen und Installieren von [Robomongo](https://robomongo.org/)
* Über Informationen zur [Verbindungszeichenfolge](connect-mongodb-account.md) für Azure Cosmos DB: API für MongoDB verfügen

## <a name="connect-using-robomongo"></a>Herstellen einer Verbindung mithilfe von Robomongo
Führen Sie die folgenden Schritte aus, um den MongoDB-Verbindungen von Robomongo Ihr Azure Cosmos DB: API für MongoDB-Konto hinzuzufügen.

1. Rufen Sie anhand der [hier](connect-mongodb-account.md) bereitgestellten Anweisungen die Verbindungsinformationen für Ihr Azure Cosmos DB: API für MongoDB-Konto ab.

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
* Untersuchen von Azure Cosmos DB: [Beispiele](mongodb-samples.md) für die API für MongoDB
