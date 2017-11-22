---
title: "Schnellstartanleitung: Table-API mit Node.js – Azure Cosmos DB | Microsoft-Dokumentation"
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe der Table-API von Azure Cosmos DB eine Anwendung mit dem Azure-Portal und Node.js erstellen.
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: node
ms.topic: quickstart
ms.date: 11/15/2017
ms.author: arramac
ms.openlocfilehash: 99f3ddb165fa548ca1d65676bb1f945632c72dd3
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="quickstart-build-a-table-api-app-with-nodejs-and-azure-cosmos-db"></a>Schnellstartanleitung: Erstellen einer Table-API-App mit Node.js und Azure Cosmos DB

In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Node.js und der [Table-API](table-introduction.md) von Azure Cosmos DB eine App erstellen, indem Sie ein Beispiel von GitHub klonen. Außerdem wird in dieser Schnellstartanleitung gezeigt, wie Sie ein Azure Cosmos DB-Konto erstellen und mithilfe des Daten-Explorers Tabellen und Entitäten im webbasierten Azure-Portal erstellen.

Azure Cosmos DB ist der global verteilte Microsoft-Datenbankdienst mit mehreren Modellen. Sie können schnell Datenbanken mit Dokumenten, Schlüsseln/Werten, breiten Spalten und Graphen erstellen und abfragen und dabei stets die Vorteile der globalen Verteilung und der horizontalen Skalierung nutzen, die Azure Cosmos DB zugrunde liegen. 

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Außerdem haben Sie folgende Möglichkeiten:

* [Node.js](https://nodejs.org/en/) Version v0.10.29 oder höher
* [Git](http://git-scm.com/)

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Hinzufügen einer Tabelle

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>Hinzufügen von Beispieldaten

Sie können nun mithilfe des Daten-Explorers Daten zu einer neuen Tabelle hinzufügen.

1. Erweitern Sie im Daten-Explorer **sample-database**, klicken Sie auf **Entitäten**, und klicken Sie anschließend auf **Entität hinzufügen**.

   ![Erstellen neuer Entitäten im Daten-Explorer im Azure-Portal](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
2. Geben Sie nun Daten in die Wertfelder „PartitionKey“ und „RowKey“ ein, und klicken Sie auf **Entität hinzufügen**.

   ![Festlegen des Partitions- und Zeilenschlüssels für eine neue Entität](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-entity.png)
  
    Jetzt können Sie weitere Entitäten in Ihre Tabelle hinzufügen, diese Entitäten bearbeiten und Ihre Daten im Daten-Explorer abfragen. Im Daten-Explorer können Sie zudem Ihren Durchsatz skalieren und der Tabelle gespeicherte Prozeduren, benutzerdefinierte Funktionen und Trigger hinzufügen.

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Klonen Sie jetzt eine Tabellen-App aus GitHub, legen Sie die Verbindungszeichenfolge fest und führen Sie diese aus. Sie werden feststellen, wie einfach Sie programmgesteuert mit Daten arbeiten können. 

1. Öffnen Sie ein Terminalfenster von Git (z. B. Git Bash), und verwenden Sie den Befehl `cd`, um den Ordner zu ändern, in den die Beispiel-App gespeichert wird. 

    ```bash
    cd "C:\git-samples"
    ```

2. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. Dieser Befehl erstellt eine Kopie der Beispiel-App auf Ihrem Computer. 

    ```bash
    git clone https://github.com/Azure-Samples/storage-table-node-getting-started.git
    ```

3. Öffnen Sie anschließend die Projektmappendatei in Visual Studio. 

## <a name="update-your-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen der Verbindungszeichenfolge abzurufen und in die App zu kopieren. Dadurch kann Ihre App mit Ihrer gehosteten Datenbank kommunizieren. 

1. Klicken Sie im [Azure-Portal](http://portal.azure.com/) auf **Verbindungszeichenfolge**. 

    ![Anzeigen und Kopieren der erforderlichen Verbindungszeichenfolgeninformationen aus dem Bereich „Verbindungszeichenfolge“](./media/create-table-nodejs/connection-string.png)

2. Öffnen Sie die Datei „app.config“, und kopieren Sie die erforderlichen Verbindungszeichenfolgeneigenschaften in die Konfigurationsdatei.

3. Speichern Sie die Datei „app.config“.

Sie haben die App nun mit allen erforderlichen Informationen für die Kommunikation mit Azure Cosmos DB aktualisiert. 

## <a name="run-the-app"></a>Ausführen der App

1. Wechseln Sie im Terminalfenster von Git mithilfe von `cd` zum Ordner „storage-table-java-getting-started“.

    ```git
    cd "C:\git-samples\
storage-table-node-getting-started"
    ```

2. Führen Sie im Terminalfenster von Git die folgenden Befehle aus, um die Java-Anwendung zu starten.

    ```git
    node ./tableSample.js 
    ```

    Im Konsolenfenster werden die Tabellendaten angezeigt, die der neuen Tabellendatenbank in Azure Cosmos DB hinzugefügt werden.

    Jetzt können Sie zum Daten-Explorer zurückkehren, um diese neue Daten anzuzeigen, abzufragen, anzupassen und mit ihnen zu arbeiten. 

## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie ein Azure Cosmos DB-Konto erstellen, eine Tabelle mit dem Daten-Explorer erstellen und eine App ausführen.  Jetzt können Sie Ihre Daten mit der Table-API abfragen.  

> [!div class="nextstepaction"]
> [Importieren von Daten für die Verwendung mit der Table-API von Azure Cosmos DB](table-import.md)
