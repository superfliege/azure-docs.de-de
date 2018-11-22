---
title: Node.js-Tutorial für die SQL-API für Azure Cosmos DB | Microsoft-Dokumentation
description: Ein Node.js-Tutorial, in dem veranschaulicht wird, wie Sie die SQL-API zum Herstellen einer Verbindung mit und zum Abfragen von Azure Cosmos DB verwenden
services: cosmos-db
author: deborahc
editor: monicar
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: dech
ms.openlocfilehash: 08fe85ae88b1f53941d490a6c22684d223da24a8
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52160952"
---
# <a name="tutorial-build-a-nodejs-console-app-with-javascript-sdk-to-manage-azure-cosmos-db-sql-api-data"></a>Tutorial: Erstellen einer Node.js-Konsolen-App mit dem JavaScript SDK zum Verwalten von Daten der Azure Cosmos DB-SQL-API

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

In diesem Tutorial wird gezeigt, wie Sie eine Node.js-Konsolenanwendung zum Erstellen und Abfragen von Azure Cosmos DB-Ressourcen erstellen.

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Erstellen eines Azure Cosmos DB-Kontos und Herstellen der Verbindung
> * Einrichten der Anwendung
> * Erstellen einer Datenbank
> * Erstellen eines Containers
> * Hinzufügen von Elementen zum Container
> * Ausführen von CRUD-Vorgängen für die Elemente, den Container und die Datenbank

Sie haben keine Zeit, um die Anwendung zu erstellen? Keine Sorge! Die vollständige Lösung ist auf [GitHub](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started )verfügbar. Im Abschnitt [Abrufen der vollständigen Lösung](#GetSolution) dieses Artikels finden Sie eine Kurzanleitung.

## <a name="prerequisites"></a>Voraussetzungen 

Stellen Sie sicher, dass Sie über die folgenden Ressourcen verfügen:

* Ein aktives Azure-Konto. Wenn Sie kein Konto haben, können Sie sich für eine [kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/)registrieren. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js](https://nodejs.org/) Version v6.0.0 oder höher

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Schritt 1: Erstellen eines Azure Cosmos DB-Kontos

Wir erstellen nun ein Azure Cosmos DB-Konto. Wenn Sie bereits über ein Konto verfügen, das Sie verwenden möchten, können Sie diesen Schritt überspringen und mit [Einrichten der Node.js-Anwendung](#SetupNode) fortfahren. Wenn Sie den Azure Cosmos DB-Emulator verwenden, führen Sie die Schritte unter [Azure Cosmos DB-Emulator](local-emulator.md) zum Einrichten des Emulators aus, und fahren Sie dann mit [Einrichten der Node.js-Anwendung](#SetupNode) fort. 

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupNode"></a>Schritt 2: Einrichten der Node.js-Anwendung

1. Öffnen Sie den bevorzugten Terminaldienst.
2. Suchen Sie nach dem Ordner oder Verzeichnis, in dem Sie die Node.js-Anwendung speichern möchten.
3. Erstellen Sie mit den folgenden Befehlen zwei leere JavaScript-Dateien:

   * Windows:
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```

   * Linux/OS X:
     * ```touch app.js```
     * ```touch config.js```

4. Installieren Sie das @azure/cosmos-Modul über npm. Verwenden Sie den folgenden Befehl:
   * ```npm install @azure/cosmos --save```

## <a id="Config"></a>Schritt 3: Festlegen der Konfigurationen der App

1. Öffnen Sie ```config.js``` in einem Text-Editor Ihrer Wahl.

1. Kopieren Sie den folgenden Codeausschnitt, und fügen Sie ihn ein. Legen Sie die Eigenschaften ```config.endpoint``` und ```config.primaryKey``` auf den Endpunkt-URI und den Primärschlüssel Ihrer Azure Cosmos DB-Instanz fest. Beide Konfigurationen finden Sie im [Azure-Portal](https://portal.azure.com).

   ![Screenshot: Abrufen von Schlüsseln über das Azure-Portal][keys]

   ```nodejs
   // ADD THIS PART TO YOUR CODE
   var config = {}

   config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
   config.primaryKey = "~your primary key here~";
   ``` 

1. Fügen Sie die kopierten Daten für ```database```, ```container```, und ```items``` an der Stelle in Ihr ```config```-Objekt ein, an der die Eigenschaften ```config.endpoint``` und ```config.primaryKey``` festgelegt werden. Wenn Sie bereits über Daten verfügen, die Sie in der Datenbank speichern möchten, können Sie das [Datenmigrationstool](import-data.md) von Azure Cosmos DB verwenden, anstatt die Daten hier zu definieren.

   ```nodejs
   var config = {}

   config.endpoint = "~your Azure Cosmos DB account endpoint uri here~";
   config.primaryKey = "~your primary key here~";

   config.database = {
      "id": "FamilyDatabase"
   };

   config.container = {
     "id": "FamilyContainer"
   };

   config.items = {
      "Andersen": {
          "id": "Anderson.1",
          "lastName": "Andersen",
          "parents": [{
            "firstName": "Thomas"
        }, {
                "firstName": "Mary Kay"
            }],
        "children": [{
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [{
                "givenName": "Fluffy"
            }]
        }],
        "address": {
            "state": "WA",
            "county": "King",
            "city": "Seattle"
        }
    },
    "Wakefield": {
        "id": "Wakefield.7",
        "parents": [{
            "familyName": "Wakefield",
            "firstName": "Robin"
        }, {
                "familyName": "Miller",
                "firstName": "Ben"
            }],
        "children": [{
            "familyName": "Merriam",
            "firstName": "Jesse",
            "gender": "female",
            "grade": 8,
            "pets": [{
                "givenName": "Goofy"
            }, {
                    "givenName": "Shadow"
                }]
        }, {
                "familyName": "Miller",
                "firstName": "Lisa",
                "gender": "female",
                "grade": 1
            }],
        "address": {
            "state": "NY",
            "county": "Manhattan",
            "city": "NY"
        },
        "isRegistered": false
      }
   };
   ```

   Hinweis: Wenn Sie mit der vorherigen Version des JavaScript SDK vertraut sind, kennen Sie unter Umständen bereits die Begriffe „Sammlung“ und „Dokument“. Azure Cosmos DB unterstützt [mehrere API-Modelle](https://docs.microsoft.com/azure/cosmos-db/introduction#key-capabilities). Daher werden in Version 2.0 und höheren Versionen des JavaScript SDK die generischen Begriffe „Container“ und „Element“ verwendet. Ein Container kann eine Sammlung, ein Graph oder eine Tabelle sein. Ein Element kann ein Dokument, ein Edge/Vertex oder eine Zeile sein und stellt den Inhalt eines Containers dar. 

1. Exportieren Sie abschließend das ```config```-Objekt, um in der Datei ```app.js``` darauf verweisen zu können.

   ```nodejs
        },
        "isRegistered": false
       }
   };

   // ADD THIS PART TO YOUR CODE
   module.exports = config;
   ```

## <a id="Connect"></a>Schritt 4: Herstellen einer Verbindung mit einem Azure Cosmos DB-Konto

1. Öffnen Sie die leere Datei ```app.js``` im Text-Editor. Fügen Sie den folgenden Code ein, um das ```@azure/cosmos```-Modul und das neu erstellte ```config```-Modul zu importieren.

   ```nodejs
   // ADD THIS PART TO YOUR CODE
   const CosmosClient = require('@azure/cosmos').CosmosClient;

   const config = require('./config');
   const url = require('url');
   ```

1. Fügen Sie den kopierten Code ein, um unter Verwendung der zuvor gespeicherten Eigenschaften ```config.endpoint``` und ```config.primaryKey``` ein neues CosmosClient-Element zu erstellen.

   ```nodejs
   const url = require('url');

   // ADD THIS PART TO YOUR CODE
   const endpoint = config.endpoint;
   const masterKey = config.primaryKey;

   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });
   ```

Nachdem Sie nun über den Code zum Initialisieren des Azure Cosmos DB-Clients verfügen, beschäftigen wir uns im nächsten Schritt mit der Verwendung von Azure Cosmos DB-Ressourcen.

## <a name="step-5-create-a-database"></a>Schritt 5: Erstellen einer Datenbank

1. Kopieren Sie den folgenden Code, und fügen Sie ihn ein, um die Datenbank-ID und die Container-ID festzulegen. Anhand dieser IDs ermittelt der Azure Cosmos DB-Client die richtige Datenbank und den richtigen Container.

   ```nodejs
   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

   // ADD THIS PART TO YOUR CODE
   const HttpStatusCodes = { NOTFOUND: 404 };

   const databaseId = config.database.id;
   const containerId = config.container.id;
   ```

   Eine Datenbank kann mithilfe der Funktion [createIfNotExists](/javascript/api/%40azure/cosmos/databases) oder [create](/javascript/api/%40azure/cosmos/databases) der **Databases**-Klasse erstellt werden. Eine Datenbank ist ein logischer Container für Elemente, die auf Container aufgeteilt sind. 

2. Fügen Sie in der Datei „app.js“ die Funktionen **createDatabase** und **readDatabase** unterhalb der Definitionen von ```databaseId``` und ```containerId``` ein. Mit der Funktion **createDatabase** wird eine neue Datenbank mit der ID ```FamilyDatabase``` erstellt, angegeben über das Objekt ```config``` (sofern noch nicht vorhanden). Die Funktion **readDatabase** liest die Definition der Datenbank, um sicherzustellen, dass die Datenbank vorhanden ist.

   ```nodejs
   /**
    * Create the database if it does not exist
    */
   async function createDatabase() {
       const { database } = await client.databases.createIfNotExists({ id: databaseId });
       console.log(`Created database:\n${database.id}\n`);
   }

   /**
   * Read the database definition
   */
   async function readDatabase() {
      const { body: databaseDefinition } = await client.database(databaseId).read();
      console.log(`Reading database:\n${databaseDefinition.id}\n`);
   }
   ```

3. Fügen Sie den folgenden kopierten Code an der Stelle ein, an der die Funktionen **createDatabase** und **readDatabase** festgelegt werden. Dadurch wird die **exit**-Hilfsfunktion hinzugefügt, die die Beendigungsmeldung ausgibt. 

   ```nodejs
   // ADD THIS PART TO YOUR CODE
   function exit(message) {
      console.log(message);
      console.log('Press any key to exit');
      process.stdin.setRawMode(true);
      process.stdin.resume();
      process.stdin.on('data', process.exit.bind(process, 0));
   };
   ```

4. Fügen Sie den folgenden kopierten Code an der Stelle ein, an der die **exit**-Funktion festgelegt wird, um die Funktionen **createDatabase** und **readDatabase** aufzurufen.

   ```nodejs
   createDatabase()
     .then(() => readDatabase())
     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error \${JSON.stringify(error)}`) });
   ```

   Der Code in ```app.js``` sollte nun wie folgt aussehen:

   ```nodejs
   const CosmosClient = require('@azure/cosmos').CosmosClient;

   const config = require('./config');
   const url = require('url');

   const endpoint = config.endpoint;
   const masterKey = config.primaryKey;

   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

   const HttpStatusCodes = { NOTFOUND: 404 };

   const databaseId = config.database.id;
   const containerId = config.container.id;

    /**
    * Create the database if it does not exist
    */
    async function createDatabase() {
     const { database } = await client.databases.createIfNotExists({ id: databaseId });
     console.log(`Created database:\n${database.id}\n`);
   }

   /**
   * Read the database definition
   */
   async function readDatabase() {
     const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
   }

   /**
   * Exit the app with a prompt
   * @param {message} message - The message to display
   */
   function exit(message) {
     console.log(message);
     console.log('Press any key to exit');
     process.stdin.setRawMode(true);
     process.stdin.resume();
     process.stdin.on('data', process.exit.bind(process, 0));
   }

   createDatabase()
     .then(() => readDatabase())
     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error) }`) });
   ```

5. Suchen Sie in Ihrem Terminal nach der Datei ```app.js```, und führen Sie den folgenden Befehl aus: 

   ```bash 
   node app.js
   ```

Glückwunsch! Sie haben erfolgreich eine Azure Cosmos DB-Datenbank erstellt.

## <a id="CreateContainer"></a>Schritt 6: Erstellen eines Containers

> [!WARNING]
> Durch Aufrufen der Funktion **createContainer** wird ein neuer Container erstellt. Dies wirkt sich auf den Preis aus. Weitere Informationen finden Sie auf unserer [Preisseite](https://azure.microsoft.com/pricing/details/cosmos-db/).

Ein Container kann mithilfe der Funktion [createIfNotExists](/javascript/api/%40azure/cosmos/containers) oder [create](/javascript/api/%40azure/cosmos/containers) der **Containers**-Klasse erstellt werden.  Ein Container besteht aus Elementen (JSON-Dokumente im Falle der SQL-API) und zugeordneter JavaScript-Anwendungslogik.

1. Fügen Sie in der Datei „app.js“ die kopierten Funktionen **createContainer** und **readContainer** unterhalb der Funktion **readDatabase** ein. Mit der Funktion **createContainer** wird ein neuer Container mit ```containerId``` erstellt, angegeben über das Objekt ```config``` (sofern noch nicht vorhanden). Die Funktion **readContainer** liest die Containerdefinition, um zu überprüfen, ob der Container vorhanden ist.

   ```nodejs
   /**
   * Create the container if it does not exist
   */
   async function createContainer() {
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    console.log(`Created container:\n${config.container.id}\n`);
   }

   /**
   * Read the container definition
   */
   async function readContainer() {
     const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
     console.log(`Reading container:\n${containerDefinition.id}\n`);
   }
   ```

1. Fügen Sie den kopierten Code unterhalb des Aufrufs von **readDatabase** ein, um die Funktionen **createContainer** und **readContainer** auszuführen.

   ```nodejs
   createDatabase()
     .then(() => readDatabase())

     // ADD THIS PART TO YOUR CODE
     .then(() => createContainer())
     .then(() => readContainer())
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

   Der Code in ```app.js``` sollte nun wie folgt aussehen:

   ```nodejs
   const CosmosClient = require('@azure/cosmos').CosmosClient;

   const config = require('./config');
   const url = require('url');

   const endpoint = config.endpoint;
   const masterKey = config.primaryKey;

   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

   const HttpStatusCodes = { NOTFOUND: 404 };

   const databaseId = config.database.id;
   const containerId = config.container.id;

   /**
   * Create the database if it does not exist
   */
   async function createDatabase() {
     const { database } = await client.databases.createIfNotExists({ id: databaseId });
     console.log(`Created database:\n${database.id}\n`);
   }

   /**
   * Read the database definition
   */
   async function readDatabase() {
     const { body: databaseDefinition } = await client.database(databaseId).read();
     console.log(`Reading database:\n${databaseDefinition.id}\n`);
   }

   /**
   * Create the container if it does not exist
   */
   async function createContainer() {
     const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
     console.log(`Created container:\n${config.container.id}\n`);
   }

   /**
   * Read the container definition
   */
   async function readContainer() {
     const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
     console.log(`Reading container:\n${containerDefinition.id}\n`);
   }

   /**
   * Exit the app with a prompt
   * @param {message} message - The message to display
   */
   function exit(message) {
     console.log(message);
     console.log('Press any key to exit');
     process.stdin.setRawMode(true);
     process.stdin.resume();
     process.stdin.on('data', process.exit.bind(process, 0));
   }

   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())
     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. Suchen Sie in Ihrem Terminal nach der Datei ```app.js```, und führen Sie den folgenden Befehl aus: 

   ```bash 
   node app.js
   ```

Glückwunsch! Sie haben erfolgreich einen Azure Cosmos DB-Container erstellt.

## <a id="CreateItem"></a>Schritt 7: Erstellen eines Elements

Ein Element kann mithilfe der [create](/javascript/api/%40azure/cosmos/items)-Funktion der **Items**-Klasse erstellt werden. Bei Verwendung der SQL-API werden Elemente als Dokumente projiziert. Dabei handelt es sich um benutzerdefinierten (beliebigen) JSON-Inhalt. Sie können nun ein Element in Azure Cosmos DB einfügen.

1. Fügen Sie die kopierte **createFamilyItem**-Funktion unterhalb der **readContainer**-Funktion ein. Die **createFamilyItem**-Funktion erstellt die Elemente mit den JSON-Daten, die im ```config```-Objekt gespeichert sind. Vor der Erstellung vergewissern wir uns, dass noch kein Element mit der gleichen ID vorhanden ist.

   ```nodejs
   /**
   * Create family item if it does not exist
   */
  async function createFamilyItem(itemBody) {
     try {
         // read the item to see if it exists
         const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).read();
         console.log(`Item with family id ${itemBody.id} already exists\n`);
     }
     catch (error) {
        // create the family item if it does not exist
        if (error.code === HttpStatusCodes.NOTFOUND) {
            const { item } = await client.database(databaseId).container(containerId).items.create(itemBody);
            console.log(`Created family item with id:\n${itemBody.id}\n`);
        } else {
            throw error;
        }
     }
   };
   ```

1. Fügen Sie den kopierten Code unterhalb des Aufrufs von **readContainer** ein, um die **createFamilyItem**-Funktion auszuführen.

   ```nodejs
   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())

     // ADD THIS PART TO YOUR CODE
     .then(() => createFamilyItem(config.items.Andersen))
     .then(() => createFamilyItem(config.items.Wakefield))
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. Suchen Sie in Ihrem Terminal nach der Datei ```app.js```, und führen Sie den folgenden Befehl aus: 

   ```bash 
   node app.js
   ```

Glückwunsch! Sie haben erfolgreich ein Azure Cosmos DB-Element erstellt.


## <a id="Query"></a>Schritt 8: Abfragen von Azure Cosmos DB-Ressourcen
Azure Cosmos DB unterstützt [umfassende Abfragen](how-to-sql-query.md) der in jedem Container gespeicherten JSON-Dokumente. Der folgende Beispielcode zeigt eine Abfrage, die Sie für die Dokumente in Ihrem Container ausführen können.

1. Fügen Sie in der Datei „app.js“ die kopierte **queryContainer**-Funktion unterhalb der **createFamilyItem**-Funktion ein. Azure Cosmos DB unterstützt SQL-ähnliche Abfragen, wie unten dargestellt. Weitere Informationen zum Erstellen von komplexen Abfragen finden Sie unter [Query Playground](https://www.documentdb.com/sql/demo) (in englischer Sprache) und in der [Abfragedokumentation](how-to-sql-query.md).

   ```nodejs
   /**
   * Query the container using SQL
    */
   async function queryContainer() {
     console.log(`Querying container:\n${config.container.id}`);

     // query to return all children in a family
     const querySpec = {
        query: "SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName",
        parameters: [
            {
                name: "@lastName",
                value: "Andersen"
            }
        ]
    };

    const { result: results } = await client.database(databaseId).container(containerId).items.query(querySpec).toArray();
    for (var queryResult of results) {
        let resultString = JSON.stringify(queryResult);
        console.log(`\tQuery returned ${resultString}\n`);
    }
   };
   ```

1. Fügen Sie den kopierten Code unterhalb der Aufrufe von **createFamilyItem** ein, um die **queryContainer**-Funktion auszuführen.

   ```nodejs
   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())
     .then(() => createFamilyItem(config.items.Andersen))
     .then(() => createFamilyItem(config.items.Wakefield))

     // ADD THIS PART TO YOUR CODE
     .then(() => queryContainer())
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. Suchen Sie in Ihrem Terminal nach der Datei ```app.js```, und führen Sie den folgenden Befehl aus:

   ```bash 
   node app.js
   ```

Glückwunsch! Sie haben erfolgreich Azure Cosmos DB-Elemente abgefragt.

## <a id="ReplaceItem"></a>Schritt 9: Ersetzen eines Elements
Azure Cosmos DB unterstützt das Ersetzen von Elementinhalten.

1. Fügen Sie in der Datei „app.js“ die kopierte **replaceFamilyItem**-Funktion unterhalb der **queryContainer**-Funktion ein. Beachten Sie, dass die Eigenschaft „grade“ eines untergeordneten Elements von „5“ in „6“ geändert wurde.

   ```nodejs
   // ADD THIS PART TO YOUR CODE
   /**
   * Replace the item by ID.
   */
   async function replaceFamilyItem(itemBody) {
      console.log(`Replacing item:\n${itemBody.id}\n`);
      // Change property 'grade'
      itemBody.children[0].grade = 6;
      const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).replace(itemBody);
   };
   ```

1. Fügen Sie den kopierten Code unterhalb des Aufrufs von **queryContainer** ein, um die **replaceFamilyItem**-Funktion auszuführen. Fügen Sie außerdem den Code zum erneuten Aufrufen von **queryContainer** hinzu, um sich zu vergewissern, dass das Element erfolgreich geändert wurde.

   ```nodejs
   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())
     .then(() => createFamilyItem(config.items.Andersen))
     .then(() => createFamilyItem(config.items.Wakefield))
     .then(() => queryContainer())

     // ADD THIS PART TO YOUR CODE
     .then(() => replaceFamilyItem(config.items.Andersen))
     .then(() => queryContainer())
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. Suchen Sie in Ihrem Terminal nach der Datei ```app.js```, und führen Sie den folgenden Befehl aus:

   ```bash 
   node app.js
   ```

Glückwunsch! Sie haben erfolgreich ein Azure Cosmos DB-Element ersetzt.

## <a id="DeleteItem"></a>Schritt 10: Löschen eines Elements

Azure Cosmos DB unterstützt das Löschen von JSON-Elementen.

1. Fügen Sie die kopierte **deleteFamilyItem**-Funktion unterhalb der **replaceFamilyItem**-Funktion ein.

   ```nodejs
  /**
  * Delete the item by ID.
  */
   async function deleteFamilyItem(itemBody) {
      await client.database(databaseId).container(containerId).item(itemBody.id).delete(itemBody);
      console.log(`Deleted item:\n${itemBody.id}\n`);
   };
   ```

1. Fügen Sie den kopierten Code unterhalb des zweiten Aufrufs von **queryContainer** ein, um die **deleteFamilyItem**-Funktion auszuführen.

   ```nodejs
   createDatabase()
      .then(() => readDatabase())
      .then(() => createContainer())
      .then(() => readContainer())
      .then(() => createFamilyItem(config.items.Andersen))
      .then(() => createFamilyItem(config.items.Wakefield))
      .then(() => queryContainer
      ())
      .then(() => replaceFamilyItem(config.items.Andersen))
      .then(() => queryContainer())

    // ADD THIS PART TO YOUR CODE
      .then(() => deleteFamilyItem(config.items.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. Suchen Sie in Ihrem Terminal nach der Datei ```app.js```, und führen Sie den folgenden Befehl aus: 

   ```bash 
   node app.js
   ```

Glückwunsch! Sie haben erfolgreich ein Azure Cosmos DB-Element gelöscht.

## <a id="DeleteDatabase"></a>Schritt 11: Löschen der Datenbank

Das Löschen der erstellten Datenbank entfernt die Datenbank und alle untergeordneten Ressourcen (Container, Elemente usw.).

1. Kopieren Sie die **cleanup**-Funktion, und fügen Sie sie unterhalb der **deleteFamilyItem**-Funktion ein, um die Datenbank und alle untergeordneten Ressourcen zu entfernen.

   ```nodejs
   /**
   * Cleanup the database and container on completion
   */
   async function cleanup() {
     await client.database(databaseId).delete();
   }
   ```

1. Fügen Sie den kopierten Code unterhalb des Aufrufs von **deleteFamilyItem** ein, um die **cleanup**-Funktion auszuführen.

   ```nodejs
   createDatabase()
      .then(() => readDatabase())
      .then(() => createContainer())
      .then(() => readContainer())
      .then(() => createFamilyItem(config.items.Andersen))
      .then(() => createFamilyItem(config.items.Wakefield))
      .then(() => queryContainer())
      .then(() => replaceFamilyItem(config.items.Andersen))
      .then(() => queryContainer())
      .then(() => deleteFamilyItem(config.items.Andersen))

      // ADD THIS PART TO YOUR CODE
      .then(() => cleanup())
      // ENDS HERE

      .then(() => { exit(`Completed successfully`); })
      .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

## <a id="Run"></a>Schritt 12: Ausführen der gesamten Node.js-Anwendung

Der Code sollte insgesamt wie folgt aussehen:

```nodejs
const CosmosClient = require('@azure/cosmos').CosmosClient;

const config = require('./config');
const url = require('url');

const endpoint = config.endpoint;
const masterKey = config.primaryKey;

const HttpStatusCodes = { NOTFOUND: 404 };

const databaseId = config.database.id;
const containerId = config.container.id;

const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

/**
 * Create the database if it does not exist
 */
async function createDatabase() {
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    console.log(`Created database:\n${database.id}\n`);
}

/**
 * Read the database definition
 */
async function readDatabase() {
    const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
}

/**
 * Create the container if it does not exist
 */
async function createContainer() {
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    console.log(`Created container:\n${config.container.id}\n`);
}

/**
 * Read the container definition
 */
async function readContainer() {
    const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
    console.log(`Reading container:\n${containerDefinition.id}\n`);
}

/**
 * Create family item if it does not exist
 */
async function createFamilyItem(itemBody) {
    try {
        // read the item to see if it exists
        const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).read();
        console.log(`Item with family id ${itemBody.id} already exists\n`);
    }
    catch (error) {
        // create the family item if it does not exist
        if (error.code === HttpStatusCodes.NOTFOUND) {
            const { item } = await client.database(databaseId).container(containerId).items.create(itemBody);
            console.log(`Created family item with id:\n${itemBody.id}\n`);
        } else {
            throw error;
        }
    }
};

/**
 * Query the container using SQL
 */
async function queryContainer() {
    console.log(`Querying container:\n${config.container.id}`);

    // query to return all children in a family
    const querySpec = {
        query: "SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName",
        parameters: [
            {
                name: "@lastName",
                value: "Andersen"
            }
        ]
    };

    const { result: results } = await client.database(databaseId).container(containerId).items.query(querySpec).toArray();
    for (var queryResult of results) {
        let resultString = JSON.stringify(queryResult);
        console.log(`\tQuery returned ${resultString}\n`);
    }
};

/**
 * Replace the item by ID.
 */
async function replaceFamilyItem(itemBody) {
    console.log(`Replacing item:\n${itemBody.id}\n`);
    // Change property 'grade'
    itemBody.children[0].grade = 6;
    const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).replace(itemBody);
};

/**
 * Delete the item by ID.
 */
async function deleteFamilyItem(itemBody) {
    await client.database(databaseId).container(containerId).item(itemBody.id).delete(itemBody);
    console.log(`Deleted item:\n${itemBody.id}\n`);
};

/**
 * Cleanup the database and container on completion
 */
async function cleanup() {
    await client.database(databaseId).delete();
}

/**
 * Exit the app with a prompt
 * @param {message} message - The message to display
 */
function exit(message) {
    console.log(message);
    console.log('Press any key to exit');
    process.stdin.setRawMode(true);
    process.stdin.resume();
    process.stdin.on('data', process.exit.bind(process, 0));
}

createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))
    .then(() => queryContainer())
    .then(() => replaceFamilyItem(config.items.Andersen))
    .then(() => queryContainer())
    .then(() => deleteFamilyItem(config.items.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

Suchen Sie in Ihrem Terminal nach der Datei ```app.js```, und führen Sie den folgenden Befehl aus: 

```bash 
node app.js
```

Die Ausgabe der GetStarted-Anwendung sollte angezeigt werden. Die Ausgabe sollte dem folgenden Beispieltext entsprechen.

   ```
    Created database:
    FamilyDatabase

    Reading database:
    FamilyDatabase

    Created container:
    FamilyContainer

    Reading container:
    FamilyContainer

    Created family item with id:
    Anderson.1

    Created family item with id:
    Wakefield.7

    Querying container:
    FamilyContainer
            Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing item:
    Anderson.1

    Querying container:
    FamilyContainer
            Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleted item:
    Anderson.1

    Completed successfully
    Press any key to exit
   ```

Glückwunsch! Sie haben das Node.js-Tutorial abgeschlossen und Ihre erste Azure Cosmos DB-Konsolenanwendung erstellt.

## <a id="GetSolution"></a>Abrufen der vollständigen Projektmappe für das Node.js-Tutorial

Wenn Sie keine Zeit hatten, die Schritte in diesem Tutorial auszuführen, oder nur den Code herunterladen möchten, finden Sie ihn auf [GitHub](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started ).

Zum Ausführen der Lösung für die ersten Schritte, die den gesamten Code dieses Artikels enthält, ist Folgendes erforderlich:

* Ein [Azure Cosmos DB-Konto][create-account]
* Lösung [Erste Schritte](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started) (erhältlich auf GitHub)

Installieren Sie das **@azure/cosmos**-Modul über npm. Verwenden Sie den folgenden Befehl:

* ```npm install @azure/cosmos --save```

Aktualisieren Sie dann in der Datei ```config.js``` die Werte für „config.endpoint“ und „config.primaryKey“, wie unter [Schritt 3: Festlegen der Konfigurationen der App](#Config) beschrieben. 

Suchen Sie dann auf Ihrem Terminal nach der Datei ```app.js```, und führen Sie den folgenden Befehl aus: 

```bash 
node app.js
```

Das war's auch schon. 

## <a name="next-steps"></a>Nächste Schritte

* Benötigen Sie ein komplexeres Node.js-Beispiel? Sie finden eines unter [Erstellen einer Node.js-Webanwendung mithilfe von Azure Cosmos DB](sql-api-nodejs-application.md).
* Informieren Sie sich über das [Überwachen eines Azure Cosmos DB-Kontos](monitor-accounts.md).
* Fragen Sie unser Beispieldataset im [Query Playground](https://www.documentdb.com/sql/demo)ab.

[create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-nodejs-get-started/node-js-tutorial-keys.png