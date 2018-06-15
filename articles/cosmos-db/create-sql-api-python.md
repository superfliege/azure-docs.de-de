---
title: 'Azure Cosmos DB: Erstellen einer App mit Python und der SQL-API | Microsoft-Dokumentation'
description: Hier finden Sie ein Python-Codebeispiel, mit dem Sie eine Verbindung mit der SQL-API von Azure Cosmos DB herstellen und diese API abfragen können.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.custom: quick start connect, mvc, devcenter
ms.devlang: python
ms.topic: quickstart
ms.date: 04/13/2018
ms.author: sngun
ms.openlocfilehash: fa81639866c9b2e9cfffee3d502eac8e18f86ec1
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34795304"
---
# <a name="azure-cosmos-db-build-a-sql-api-app-with-python-and-the-azure-portal"></a>Azure Cosmos DB: Erstellen einer SQL-API-App mit Python und dem Azure-Portal

Azure Cosmos DB ist der global verteilte Microsoft-Datenbankdienst mit mehreren Modellen. Sie können schnell Dokument-, Schlüssel/Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets die Vorteile der globalen Verteilung und der horizontalen Skalierung nutzen, die Azure Cosmos DB zugrunde liegen. 

In diesem Schnellstart wird veranschaulicht, wie Sie mithilfe des Azure-Portals ein [SQL-API](sql-api-introduction.md)-Konto, eine Dokumentendatenbank und eine Sammlung für Azure Cosmos DB erstellen. Anschließend erstellen Sie eine auf der [SQL-Python-API](sql-api-sdk-python.md) basierende Konsolen-App und führen sie aus.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)][!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

## <a name="prerequisites"></a>Voraussetzungen

* [Python 3.6](https://www.python.org/downloads/) mit „\<Installationsort\>\Python36“ und „\<Installationsort>\Python36\Scripts“ (hinzugefügt zu Ihrem PFAD) 
* [Visual Studio Code](https://code.visualstudio.com/)
* [Python-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python#overview)

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Hinzufügen einer Sammlung

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="add-sample-data"></a>Hinzufügen von Beispieldaten

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>Abfragen Ihrer Daten

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Klonen Sie zunächst eine SQL-API-App aus GitHub, legen Sie die Verbindungszeichenfolge fest, und führen Sie sie aus. Sie werden feststellen, wie einfach Sie programmgesteuert mit Daten arbeiten können. 

1. Öffnen Sie eine Eingabeaufforderung, erstellen Sie einen neuen Ordner namens „git-samples“, und schließen Sie die Eingabeaufforderung.

    ```bash
    md "C:\git-samples"
    ```

2. Öffnen Sie ein Git-Terminalfenster (z. B. git bash), und verwenden Sie den Befehl `cd`, um in den neuen Ordner zu gelangen und dort die Beispiel-App zu installieren.

    ```bash
    cd "C:\git-samples"
    ```

3. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. Dieser Befehl erstellt eine Kopie der Beispiel-App auf Ihrem Computer. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-python-getting-started.git
    ```  
    
## <a name="review-the-code"></a>Überprüfen des Codes

Dieser Schritt ist optional. Wenn Sie erfahren möchten, wie die Datenbankressourcen im Code erstellt werden, können Sie sich die folgenden Codeausschnitte ansehen. Andernfalls können Sie mit [Aktualisieren der Verbindungszeichenfolge](#update-your-connection-string) fortfahren. 

Die folgenden Codeausschnitte stammen alle aus der Datei „DocumentDBGetStarted.py“.

* Der DocumentClient wird initialisiert.

    ```python
    # Initialize the Python client
    client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})
    ```

* Es wird eine neue Datenbank erstellt.

    ```python
    # Create a database
    db = client.CreateDatabase({ 'id': config['SQL_DATABASE'] })
    ```

* Es wird eine neue Sammlung erstellt.

    ```python
    # Create collection options
    options = {
        'offerEnableRUPerMinuteThroughput': True,
        'offerVersion': "V2",
        'offerThroughput': 400
    }

    # Create a collection
    collection = client.CreateCollection(db['_self'], { 'id': config['SQL_COLLECTION'] }, options)
    ```

* Einige Dokumente werden erstellt.

    ```python
    # Create some documents
    document1 = client.CreateDocument(collection['_self'],
        { 
            'id': 'server1',
            'Web Site': 0,
            'Cloud Service': 0,
            'Virtual Machine': 0,
            'name': 'some' 
        })
    ```

* Eine Abfrage wird mit SQL ausgeführt.

    ```python
    # Query them in SQL
    query = { 'query': 'SELECT * FROM server s' }    
            
    options = {} 
    options['enableCrossPartitionQuery'] = True
    options['maxItemCount'] = 2

    result_iterable = client.QueryDocuments(collection['_self'], query, options)
    results = list(result_iterable);

    print(results)
    ```

## <a name="update-your-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen der Verbindungszeichenfolge abzurufen und in die App zu kopieren.

1. Klicken Sie im [Azure-Portal](http://portal.azure.com/) unter Ihrem Azure Cosmos DB-Konto im Navigationsbereich auf der linken Seite auf **Schlüssel**. Im nächsten Schritt kopieren Sie den **URI** und den **Primärschlüssel** mithilfe der Kopierschaltflächen auf der rechten Bildschirmseite in die Datei „DocumentDBGetStarted.py“.

    ![Anzeigen und Kopieren eines Zugriffsschlüssels im Azure-Portal auf dem Blatt „Schlüssel“](./media/create-sql-api-dotnet/keys.png)

2. Öffnen Sie die Datei „C:\git-samples\azure-cosmos-db-documentdb-python-getting-startedDocumentDBGetStarted.py“ in Visual Studio Code. 

3. Kopieren Sie mithilfe der Kopierschaltfläche den Wert für **URI** aus dem Portal, und legen Sie ihn in „DocumentDBGetStarted.py“ als Wert des Endpunktschlüssels (**endpoint**) fest. 

    `'ENDPOINT': 'https://FILLME.documents.azure.com',`

4. Kopieren Sie anschließend den Wert für **PRIMÄRSCHLÜSSEL** aus dem Portal, und legen Sie ihn in „DocumentDBGetStarted.py“ als Wert von **config.MASTERKEY** fest. Sie haben die App nun mit allen erforderlichen Informationen für die Kommunikation mit Azure Cosmos DB aktualisiert. 

    `'MASTERKEY': 'FILLME',`

5. Speichern Sie die Datei „DocumentDBGetStarted.py“.
    
## <a name="run-the-app"></a>Ausführen der App

1. Klicken Sie in Visual Studio Code auf **Ansicht**>**Befehlspalette**. 

2. Geben Sie an der Eingabeaufforderung **Python: Select Interpreter** ein, und wählen Sie die gewünschte Python-Version aus.

    Die Fußzeile in Visual Studio Code wird mit dem ausgewählten Interpreter aktualisiert. 

3. Klicken Sie auf **Ansicht** > **Integriertes Terminal**, um das in Visual Studio Code integrierte Terminal zu öffnen.

4. Vergewissern Sie sich im Fenster des integrierten Terminals, dass Sie sich im Ordner „azure-cosmos-db-documentdb-python-getting-started“ befinden. Wechseln Sie andernfalls mithilfe des folgenden Befehls zum Beispielordner: 

    ```
    cd "C:\git-samples\azure-cosmos-db-documentdb-python-getting-started"`
    ```

5. Führen Sie den folgenden Befehl aus, um das Paket „pydocumentdb“ zu installieren: 

    ```
    pip3 install pydocumentdb
    ```

    Sollte beim Installieren von „pydocumentdb“ eine Fehlermeldung mit dem Hinweis angezeigt werden, dass der Zugriff verweigert wurde, müssen [Visual Studio Code als Administrator ausführen](https://stackoverflow.com/questions/37700536/visual-studio-code-terminal-how-to-run-a-command-with-administrator-rights).

6. Führen Sie den folgenden Befehl aus, um das Beispiel auszuführen und neue Dokumente in Azure Cosmos DB zu erstellen und zu speichern:

    ```
    python DocumentDBGetStarted.py
    ```

7. Vergewissern Sie sich, dass die neuen Dokumente erstellt und gespeichert wurden. Klicken Sie hierzu im Azure-Portal auf **Daten-Explorer**, erweitern Sie **coll**, erweitern Sie **Dokumente**, und wählen Sie anschließend das Dokument **server1** aus. Der Inhalt des Dokuments „server1“ entspricht dem Inhalt, der im Fenster des integrierten Terminals zurückgegeben wurde. 

    ![Anzeigen der neuen Dokumente im Azure-Portal](./media/create-sql-api-python/azure-cosmos-db-confirm-documents.png)

## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie ein Azure Cosmos DB-Konto erstellen, eine Sammlung mit dem Daten-Explorer erstellen und eine Web-App ausführen. Jetzt können Sie zusätzliche Daten in Ihr Cosmos DB-Konto importieren. 

> [!div class="nextstepaction"]
> [Importieren von Daten in Azure Cosmos DB für die SQL-API](import-data.md)


