---
title: "Schnellstartanleitung: Table-API mit .NET – Azure-Cosmos-DB | Microsoft-Dokumentation"
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe der Table-API von Azure Cosmos DB eine Anwendung mit dem Azure-Portal und .NET erstellen.
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quickstart connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 11/20/2017
ms.author: mimig
ms.openlocfilehash: e0f0a95ea086e83ef0c46145b33b348071407aa5
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2017
---
# <a name="quickstart-build-a-table-api-app-with-net-and-azure-cosmos-db"></a>Schnellstartanleitung: Erstellen einer Table-API-App mit .NET und Azure Cosmos DB 

In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Java und der [Table-API](table-introduction.md) von Azure Cosmos DB eine App erstellen, indem Sie ein Beispiel von GitHub klonen. Außerdem wird in dieser Schnellstartanleitung gezeigt, wie Sie ein Azure Cosmos DB-Konto erstellen und mithilfe des Daten-Explorers Tabellen und Entitäten im webbasierten Azure-Portal erstellen.

Azure Cosmos DB ist der global verteilte Microsoft-Datenbankdienst mit mehreren Modellen. Sie können schnell Dokument-, Schlüssel/Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets die Vorteile der globalen Verteilung und der horizontalen Skalierung nutzen, die Azure Cosmos DB zugrunde liegen. 

## <a name="prerequisites"></a>Voraussetzungen

Falls Sie Visual Studio 2017 noch nicht installiert haben, können Sie die **kostenlose** [Visual Studio 2017 Community-Edition](https://www.visualstudio.com/downloads/) herunterladen und verwenden. Aktivieren Sie beim Setup von Visual Studio die Option **Azure-Entwicklung**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

> [!IMPORTANT] 
> Zum Arbeiten mit den allgemein verfügbaren Tabellen-API-SDKs müssen Sie ein neues Tabellen-API-Konto erstellen. Tabellen-API-Konten, die während der Vorschau erstellt wurden, werden von den allgemein verfügbaren SDKs nicht unterstützt.
>

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
    git clone https://github.com/Azure-Samples/storage-table-dotnet-getting-started.git
    ```

3. Öffnen Sie anschließend in Visual Studio die Projektmappendatei „TableStorage“. 

## <a name="update-your-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen der Verbindungszeichenfolge abzurufen und in die App zu kopieren. Dadurch kann Ihre App mit Ihrer gehosteten Datenbank kommunizieren. 

1. Klicken Sie im [Azure-Portal](http://portal.azure.com/) auf **Verbindungszeichenfolge**. 

    Kopieren Sie die primäre Verbindungszeichenfolge mithilfe der Kopierschaltfläche im rechten Bildschirmbereich.

    ![Anzeigen und Kopieren der primären Verbindungszeichenfolge im Bereich „Verbindungszeichenfolge“](./media/create-table-dotnet/connection-string.png)

2. Öffnen Sie in Visual Studio die Datei „App.config“. 

3. Da der Speicheremulator in diesem Tutorial nicht verwendet wird, heben Sie die Auskommentierung von „StorageConnectionString“ in Zeile 8 auf, und kommentieren Sie „StorageConnectionString“ in Zeile 7 aus. Zeile 7 und 8 sollten jetzt wie folgt aussehen:

    ```
    <!--key="StorageConnectionString" value="UseDevelopmentStorage=true;" />-->
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]" />
    ```

4. Fügen Sie die primäre Verbindungszeichenfolge aus dem Portal in den StorageConnectionString-Wert in Zeile 8 ein. Fügen Sie die Zeichenfolge innerhalb der Anführungszeichen ein. 

    > [!IMPORTANT]
    > Wenn Ihr Endpunkt „documents.azure.com“ verwendet, bedeutet dies, dass Sie über ein Vorschaukonto verfügen und ein [neues Tabellen-API-Konto](#create-a-database-account) erstellen müssen, um mit dem allgemein verfügbaren Tabellen-API-SDK zu arbeiten. 
    > 

    Zeile 8 sollte nun in etwa wie folgt aussehen:

    ```
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=txZACN9f...==;TableEndpoint=https://<account name>.table.cosmosdb.azure.com;" />
    ```

5. Speichern Sie die Datei „App.config“.

Sie haben die App nun mit allen erforderlichen Informationen für die Kommunikation mit Azure Cosmos DB aktualisiert. 

## <a name="build-and-deploy-the-app"></a>Erstellen und Bereitstellen der App

1. Klicken Sie in Visual Studio im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **TableStorage**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**. 

2. Geben Sie im NuGet-Feld **Durchsuchen** die Zeichenfolge *Microsoft.Azure.CosmosDB.Table* ein.

3. Installieren Sie die in den Ergebnissen enthaltene Bibliothek **Microsoft.Azure.CosmosDB.Table**. Dadurch werden das Paket der Table-API von Azure Cosmos DB und dessen Abhängigkeiten installiert.

4. Öffnen Sie „BasicSamples.cs“, und fügen Sie in Zeile 30 und 52 einen Haltepunkt ein.

5. Drücken Sie STRG + F5, um die Anwendung auszuführen.

    Im Konsolenfenster werden die Tabellendaten angezeigt, die der neuen Tabellendatenbank in Azure Cosmos DB hinzugefügt werden. 
    
    Wenn ein Fehler in Bezug auf Abhängigkeiten angezeigt wird, finden Sie weitere Informationen unter [Troubleshooting](table-sdk-dotnet.md#troubleshooting) (Problembehandlung).

    Wenn der erste Haltepunkt erreicht wird, wechseln Sie zurück zum Daten-Explorer im Azure-Portal, erweitern Sie die Demo-Tabelle, und klicken Sie auf **Entitäten**. Die Registerkarte **Entitäten** auf der rechten Seite zeigt die neu hinzugefügte Entität. Die Telefonnummer für Benutzer lautet 425-555-0101.
    
6. Schließen Sie die Registerkarte „Entitäten“ im Daten-Explorer.
    
7. Führen Sie die App bis zum nächsten Haltepunkt weiter aus.

    Wenn der Haltepunkt erreicht ist, wechseln Sie zurück zum Portal und klicken erneut auf die Registerkarte „Entitäten“. Beachten Sie, dass die Telefonnummer aktualisiert wurde und jetzt 425-555-0105 lautet.

8. Im Konsolenfenster drücken Sie STRG+C, um die Ausführung der App zu beenden. 

    Im Daten-Explorer können Sie jetzt Entitäten hinzufügen oder ändern und die Daten abfragen.

## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie ein Azure Cosmos DB-Konto erstellen, eine Tabelle mit dem Daten-Explorer erstellen und eine App ausführen.  Jetzt können Sie Ihre Daten mit der Table-API abfragen.  

> [!div class="nextstepaction"]
> [Importieren von Daten in die Table-API](table-import.md)

