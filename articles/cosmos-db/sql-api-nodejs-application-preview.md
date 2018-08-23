---
title: Erstellen einer Node.js-Web-App für Azure Cosmos DB | Microsoft-Dokumentation
description: In diesem Node.js-Tutorial wird beschrieben, wie Sie mit Microsoft Azure Cosmos DB Daten aus einer in Azure-Websites gehosteten Node.js Express-Webanwendung speichern und darauf zugreifen.
keywords: Anwendungsentwicklung, Datenbanktutorial, Node.js erlernen, Node.js-Tutorial
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 07/31/2018
ms.author: sngun
ms.openlocfilehash: 43c5b8beef523684a405f1396ad0a9d862c123e8
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2018
ms.locfileid: "41917578"
---
# <a name="_Toc395783175"></a>Erstellen einer Node.js-Webanwendung mithilfe von Azure Cosmos DB und des Node.js SDK (Vorschauversion)
> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Node.js: v2.0 (Vorschau)](sql-api-nodejs-application-preview.md)
> * [Java](sql-api-java-application.md)
> * [Python](sql-api-python-application.md)
> 
> 

In diesem Node.js-Tutorial erfahren Sie, wie Sie mithilfe eines SQL-API-Kontos für Azure Cosmos DB Daten aus einer in Azure Websites gehosteten Node.js Express-Anwendung speichern und abrufen. In diesem Tutorial erstellen Sie eine einfache webbasierte Anwendung (To-Do-App), mit der Sie Aufgaben erstellen, abrufen und ausführen können. Die Aufgaben werden als JSON-Dokumente in Azure Cosmos DB gespeichert. Die folgende Abbildung zeigt einen Screenshot der To-Do-Anwendung:

![Screenshot der in diesem Node.js-Lernprogramm erstellten Anwendung "My Todo List"](./media/sql-api-nodejs-application/cosmos-db-node-js-mytodo.png)

In diesem Tutorial wird veranschaulicht, wie Sie ein SQL-API-Konto für Azure Cosmos DB im Azure-Portal erstellen. Anschließend erstellen Sie eine auf dem Node.js SDK basierende Web-App und führen sie aus, um eine Datenbank und einen Container zu erstellen und dann Elemente zum Container hinzuzufügen. In diesem Tutorial wird Version 2.0 des Node.js SDK verwendet (derzeit in der Vorschauphase).

Sie können auch das vollständige Beispiel von [GitHub][GitHub] abrufen. Anweisungen zum Ausführen der App finden Sie in der [Infodatei](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md).

## <a name="_Toc395783176"></a>Voraussetzungen

Bevor Sie diesen Artikel durcharbeiten, sollten Sie sicherstellen, dass Folgendes vorhanden ist:

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js][Node.js], Version 6.10 oder höher
* [Express Generator](http://www.expressjs.com/starter/generator.html) (Installation über `npm install express-generator -g`)
* [Git][Git].

## <a name="_Toc395637761"></a>Schritt 1: Erstellen eines Azure Cosmos DB-Datenbankkontos
Wir beginnen, indem wir ein Azure Cosmos DB-Konto erstellen. Falls Sie bereits ein Konto besitzen oder den Azure Cosmos DB-Emulator für dieses Tutorial verwenden, können Sie mit [Schritt 2: Erstellen einer neuen Node.js-Anwendung](#_Toc395783178) fortfahren.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [cosmos-db-keys](../../includes/cosmos-db-keys.md)]

## <a name="_Toc395783178"></a>Schritt 2: Erstellen einer neuen Node.js-Anwendung
Nun erfahren Sie, wie Sie ein einfaches "Hello World"-Node.js-Projekt mithilfe des [Express](http://expressjs.com/) -Frameworks erstellen.

1. Öffnen Sie Ihr bevorzugtes Terminal (beispielsweise die Node.js-Eingabeaufforderung).
2. Navigieren Sie zu dem Verzeichnis, in dem Sie die neue Anwendung speichern möchten.
3. Verwenden Sie den Express Generator, um eine neue Anwendung namens **todo**zu erstellen.

   ```bash
   express todo
   ```
4. Öffnen Sie das neue **todo** -Verzeichnis, und installieren Sie alle Abhängigkeiten.

   ```bash
   cd todo
   npm install
   ```
5. Führen Sie die neue Anwendung aus.

   ```bash
   npm start
   ```

6. Sie können die neue Anwendung anzeigen, indem Sie in Ihrem Browser zu „[http://localhost:3000](http://localhost:3000)“ navigieren.
   
    ![Kennenlernen von Node.js - Screenshot der "Hello World"-Anwendung in einem Browserfenster](./media/sql-api-nodejs-application/cosmos-db-node-js-express.png)

 Beenden Sie die Anwendung durch Drücken von STRG+C im Terminalfenster, und klicken Sie auf **J**, um den Batchauftrag zu beenden.

## <a name="_Toc395783179"></a>Schritt 3: Installieren der erforderlichen Module

Die Datei **package.json** ist eine der im Stammverzeichnis des Projekts erstellten Dateien. Diese Datei enthält eine Liste zusätzlicher Module, die für Ihre Node.js-Anwendung erforderlich sind. Wenn Sie später diese Anwendung in Azure Websites bereitstellen, wird anhand dieser Datei bestimmt, welche Module in Azure installiert werden müssen, um Ihre Anwendung zu unterstützen. Für dieses Tutorial müssen zwei weitere Pakete installiert werden.

1. Öffnen Sie das Terminal, und installieren Sie das **async**-Modul über npm.

   ```bash
   npm install async --save
   ```

2. Installieren Sie das **@azure/cosmos**-Modul über npm. 

   ```bash
   npm install @azure/cosmos
   ```

## <a name="_Toc395783180"></a>Schritt 4: Verwenden des Azure Cosmos DB-Diensts in einer Node-Anwendung
Sie haben die Ersteinrichtung und -konfiguration abgeschlossen und schreiben als Nächstes Code, der von der To-Do-Anwendung für die Kommunikation mit Azure Cosmos DB benötigt wird.

### <a name="create-the-model"></a>Erstellen des Modells
1. Erstellen Sie im Stamm des Projektverzeichnisses ein neues Verzeichnis namens **models**.  

2. Erstellen Sie im Verzeichnis **models** eine neue Datei namens **taskDao.js**. Diese Datei enthält Code, der zum Erstellen der Datenbank und des Containers erforderlich ist. Darüber hinaus werden in der Datei Methoden zum Lesen, Aktualisieren, Erstellen und Suchen von Aufgaben in Azure Cosmos DB definiert. 

3. Kopieren Sie den folgenden Code in die Datei **taskDao.js**.

   ```nodejs
   // @ts-check
   const CosmosClient = require("@azure/cosmos").CosmosClient;
   const debug = require("debug")("todo:taskDao");
   class TaskDao {
     /**
      * Manages reading, adding, and updating Tasks in Cosmos DB
      * @param {CosmosClient} cosmosClient
      * @param {string} databaseId
      * @param {string} containerId
      */
     constructor(cosmosClient, databaseId, containerId) {
       this.client = cosmosClient;
       this.databaseId = databaseId;
       this.collectionId = containerId;

       this.database = null;
       this.container = null;
     }

     async init() {
       debug("Setting up the database...");
       const dbResponse = await this.client.databases.createIfNotExists({
         id: this.databaseId
       });
       this.database = dbResponse.database;
       debug("Setting up the database...done!");
       debug("Setting up the container...");
       const coResponse = await this.database.containers.createIfNotExists({
         id: this.collectionId
       });
       this.container = coResponse.container;
       debug("Setting up the container...done!");
     }

     async find(querySpec) {
       debug("Querying for items from the database");
       if (!this.container) {
         throw new Error("Collection is not initialized.");
       }
       const { result: results } = await this.container.items
        .query(querySpec)
        .toArray();
      return results;
    }

    async addItem(item) {
      debug("Adding an item to the database");
      item.date = Date.now();
      item.completed = false;
      const { body: doc } = await this.container.items.create(item);
      return doc;
    }

    async updateItem(itemId) {
      debug("Update an item in the database");
      const doc = await this.getItem(itemId);
      doc.completed = true;

      const { body: replaced } = await this.container.item(itemId).replace(doc);
      return replaced;
    }

    async getItem(itemId) {
      debug("Getting an item from the database");
      const { body } = await this.container.item(itemId).read();
      return body;
    }
  }

   module.exports = TaskDao;
   ```
4. Speichern und schließen Sie die Datei **taskDao.js** .  

### <a name="create-the-controller"></a>Erstellen des Controllers

1. Erstellen Sie im Verzeichnis **routes** des Projekts eine neue Datei namens **tasklist.js**.  

2. Fügen Sie **tasklist.js**den folgenden Code hinzu. Hierdurch werden die Module „CosmosClient“ und „async“ geladen, die von **tasklist.js** verwendet werden. Weiterhin wird hierdurch die Klasse **TaskList** definiert, die als Instanz des zuvor definierten **TaskDao**-Objekts übergeben wird:
   
   ```nodejs
   const TaskDao = require("../models/TaskDao");

   class TaskList {
     /**
      * Handles the various APIs for displaying and managing tasks
      * @param {TaskDao} taskDao
     */
    constructor(taskDao) {
    this.taskDao = taskDao;
    }
    async showTasks(req, res) {
      const querySpec = {
        query: "SELECT * FROM root r WHERE r.completed=@completed",
        parameters: [
          {
            name: "@completed",
            value: false
          }
        ]
      };

      const items = await this.taskDao.find(querySpec);
      res.render("index", {
        title: "My ToDo List ",
        tasks: items
      });
    }

    async addTask(req, res) {
      const item = req.body;

      await this.taskDao.addItem(item);
      res.redirect("/");
    }

    async completeTask(req, res) {
      const completedTasks = Object.keys(req.body);
      const tasks = [];

      completedTasks.forEach(task => {
        tasks.push(this.taskDao.updateItem(task));
      });

      await Promise.all(tasks);

      res.redirect("/");
    }
  }

  module.exports = TaskList;
   ```

3. Speichern und schließen Sie die Datei **tasklist.js** .

### <a name="add-configjs"></a>Fügen Sie config.js hinzu.

1. Erstellen Sie im Stamm des Projektverzeichnisses eine neue Datei namens **config.js**. 

2. Fügen Sie der Datei **config.cs** den folgenden Code hinzu. Mit diesem Code werden die für Ihre Anwendung erforderlichen Konfigurationseinstellungen und Werte definiert.
   
   ```nodejs
   const config = {};

   config.host = process.env.HOST || "[the endpoint URI of your Azure Cosmos DB account]";
   config.authKey =
     process.env.AUTH_KEY || "[the PRIMARY KEY value of your Azure Cosmos DB account";
   config.databaseId = "ToDoList";
   config.containerId = "Items";

   if (config.host.includes("https://localhost:")) {
     console.log("Local environment detected");
     console.log("WARNING: Disabled checking of self-signed certs. Do not have this code in production.");
     process.env.NODE_TLS_REJECT_UNAUTHORIZED = "0";
     console.log(`Go to http://localhost:${process.env.PORT || '3000'} to try the sample.`);
   }

   module.exports = config;
   ```

3. Aktualisieren Sie in der Datei **config.js** die Werte für „HOST“ und „AUTH_KEY“ mit den Werten auf der Seite „Schlüssel“ Ihres Azure Cosmos DB-Kontos im [Microsoft Azure-Portal](https://portal.azure.com). 

4. Speichern und schließen Sie die Datei **config.js** .

### <a name="modify-appjs"></a>Ändern von app.js
1. Öffnen Sie im Projektverzeichnis die Datei **app.js** . Diese Datei wurde bereits während der Erstellung der Express-Webanwendung erstellt.  

2. Fügen Sie der Datei **app.js** den folgenden Code hinzu. Dieser Code definiert die zu verwendende Konfigurationsdatei und liest Werte aus dieser Datei für einige Variablen, die wir später verwenden. 
   
   ```nodejs
   const CosmosClient = require("@azure/cosmos").CosmosClient;
   const config = require("./config");
   const TaskList = require("./routes/tasklist");
   const TaskDao = require("./models/taskDao");

   const express = require("express");
   const path = require("path");
   const logger = require("morgan");
   const cookieParser = require("cookie-parser");
   const bodyParser = require("body-parser");

   const app = express();

   // view engine setup
   app.set("views", path.join(__dirname, "views"));
   app.set("view engine", "jade");

   // uncomment after placing your favicon in /public
   //app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')));
   app.use(logger("dev"));
   app.use(bodyParser.json());
   app.use(bodyParser.urlencoded({ extended: false }));
   app.use(cookieParser());
   app.use(express.static(path.join(__dirname, "public")));

   //Todo App:
   const cosmosClient = new CosmosClient({
     endpoint: config.host,
     auth: {
       masterKey: config.authKey
     }
   });
   const taskDao = new TaskDao(cosmosClient, config.databaseId, config.containerId);
   const taskList = new TaskList(taskDao);
   taskDao
     .init(err => {
       console.error(err);
     })
     .catch(err => {
       console.error(err);
       console.error("Shutting down because there was an error settinig up the database.");
       process.exit(1);
     });

   app.get("/", (req, res, next) => taskList.showTasks(req, res).catch(next));
   app.post("/addtask", (req, res, next) => taskList.addTask(req, res).catch(next));
   app.post("/completetask", (req, res, next) => taskList.completeTask(req, res).catch(next));
   app.set("view engine", "jade");

   // catch 404 and forward to error handler
   app.use(function(req, res, next) {
     const err = new Error("Not Found");
     err.status = 404;
     next(err);
   });

   // error handler
   app.use(function(err, req, res, next) {
     // set locals, only providing error in development
     res.locals.message = err.message;
     res.locals.error = req.app.get("env") === "development" ? err : {};

     // render the error page
     res.status(err.status || 500);
     res.render("error");
   });

   module.exports = app;
   ```

3. Speichern und schließen Sie abschließend die Datei **app.js**. Wir sind fast fertig.

## <a name="_Toc395783181"></a>Schritt 5: Erstellen einer Benutzeroberfläche
Jetzt konzentrieren wir uns auf die Erstellung der Benutzeroberfläche, um den Benutzern die eigentliche Interaktion mit unserer Anwendung zu ermöglichen. Die von uns erstellte Express-Anwendung verwendet **Jade** als Anzeige-Engine. Weitere Informationen zu Jade finden Sie unter „[http://jade-lang.com/](http://jade-lang.com/)“.

1. Die Datei **layout.jade** im Verzeichnis **views** dient als globale Vorlage für andere **.jade**-Dateien. In diesem Schritt werden Sie sie modifizieren, um [Twitter Bootstrap](https://github.com/twbs/bootstrap)zu verwenden, ein Toolkit zum mühelosen Gestalten ansprechender Websites.  

2. Öffnen Sie die Datei **layout.jade**, die sich im Ordner **views** befindet, und ersetzen Sie die Inhalte durch Folgendes:

   ```html
   doctype html
   html
     head
       title= title
       link(rel='stylesheet', href='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css')
       link(rel='stylesheet', href='/stylesheets/style.css')
     body
       nav.navbar.navbar-inverse.navbar-fixed-top
         div.navbar-header
           a.navbar-brand(href='#') My Tasks
       block content
       script(src='//ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js')
       script(src='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js')
   ```

    Dadurch wird der **Jade**-Engine mitgeteilt, dass für unsere Anwendung einige HTML-Elemente dargestellt werden müssen. Die Engine erstellt dann einen **Block** mit der Bezeichnung **content**, in dem wir das Layout für unsere Inhaltsseiten bereitstellen können.

    Speichern und schließen Sie die Datei **layout.jade** .

3. Öffnen Sie nun die Datei **index.jade**, d. h. die Ansicht, die von der Anwendung verwendet wird, und ersetzen Sie den Inhalt der Datei durch Folgendes:

   ```html
   extends layout
   block content
        h1 #{title}
        br
        
        form(action="/completetask", method="post")
         table.table.table-striped.table-bordered
            tr
              td Name
              td Category
              td Date
              td Complete
            if (typeof tasks === "undefined")
              tr
                td
            else
              each task in tasks
                tr
                  td #{task.name}
                  td #{task.category}
                  - var date  = new Date(task.date);
                  - var day   = date.getDate();
                  - var month = date.getMonth() + 1;
                  - var year  = date.getFullYear();
                  td #{month + "/" + day + "/" + year}
                  td
                   if(task.completed) 
                    input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
                   else
                    input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
          button.btn.btn-primary(type="submit") Update tasks
        hr
        form.well(action="/addtask", method="post")
          label Item Name:
          input(name="name", type="textbox")
          label Item Category:
          input(name="category", type="textbox")
          br
          button.btn(type="submit") Add item
   ```

Dadurch wird das Layout erweitert und Inhalt für den Platzhalter **content** bereitgestellt, den wir zuvor in der Datei **layout.jade** gesehen haben.
   
In diesem Layout haben wir zwei HTML-Formulare erstellt.

Das erste Formular enthält eine Tabelle für unsere Daten sowie eine Schaltfläche, mit der die Elemente durch Übermittlung an die **/completeTask**-Methode des Controllers aktualisiert werden können.
    
Das zweite Formular enthält zwei Eingabefelder und eine Schaltfläche, mit der Sie ein neues Element durch Übermittlung an die **/addtask** -Methode des Controllers erstellen können.

Dies sollte jetzt alles sein, damit unsere Anwendung funktioniert.

## <a name="_Toc395783181"></a>Schritt 6: Lokales Ausführen der Anwendung
1. Um die Anwendung auf dem lokalen Computer zu testen, führen Sie `npm start` über das Terminal aus, um Ihre Anwendung zu starten, und aktualisieren Sie anschließend die Browserseite [http://localhost:3000](http://localhost:3000). Die Seite sollte nun wie folgt aussehen:
   
    ![Screenshot der Anwendung "Meine Aufgabenliste" in einem Browserfenster](./media/sql-api-nodejs-application/cosmos-db-node-js-localhost.png)

    > [!TIP]
    > Sollte eine Fehlermeldung mit einem Hinweis auf den Einzug in der Datei „layout.jade“ oder „index.jade“ ausgegeben werden, vergewissern Sie sich, dass die ersten beiden Zeilen der beiden Dateien ohne Leerzeichen linksbündig ausgerichtet sind. Sollten sich vor den ersten beiden Zeilen Leerzeichen befinden, entfernen Sie sie, speichern Sie beide Dateien, und aktualisieren Sie anschließend Ihr Browserfenster. 

2. Geben Sie unter Verwendung der Felder „Element“, „Elementname“ und „Kategorie“ eine neue Aufgabe ein, und klicken Sie anschließend auf **Element hinzufügen**. Dadurch wird in Azure Cosmos DB ein Dokument mit diesen Eigenschaften erstellt. 
3. Die Seite sollte nun das neu erstellte Element in der Aufgabenliste anzeigen.
   
    ![Screenshot der Anwendung mit einem neuen Element in der Aufgabenliste](./media/sql-api-nodejs-application/cosmos-db-node-js-added-task.png)
4. Zum Abschließen einer Aufgabe aktivieren Sie einfach das Kontrollkästchen in der entsprechenden Spalte, und klicken Sie dann auf **Aufgaben aktualisieren**. Dadurch wird das bereits erstellte Dokument aktualisiert und aus der Ansicht entfernt.

5. Drücken Sie zum Beenden der Anwendung im Terminalfenster STRG+C, und klicken Sie dann auf **J**, um den Batchauftrag zu beenden.

## <a name="_Toc395783182"></a>Schritt 7: Bereitstellen Ihres Anwendungsentwicklungsprojekts auf Azure Websites
1. Falls noch nicht geschehen, aktivieren Sie ein Git-Repository für Ihre Azure-Website. Eine Anleitung hierzu finden Sie im Thema [Lokale Git-Bereitstellung in Azure App Service](../app-service/app-service-deploy-local-git.md) .
2. Fügen Sie Ihre Azure-Website als "Git Remote" hinzu.
   
        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
3. Stellen Sie sie mittels Push auf Remote bereit.
   
        git push azure master
4. Dann schließt Git die Veröffentlichung Ihrer Webanwendung in wenigen Sekunden ab und startet einen Browser, in dem das Ergebnis Ihrer Arbeit in Azure ausgeführt wird.

    Glückwunsch! Sie haben Ihre erste Node.js-Express-Webanwendung mit Azure Cosmos DB erstellt und auf Azure-Websites veröffentlicht.

    Die vollständige Anwendung für dieses Tutorial kann von [GitHub][GitHub] heruntergeladen und als Referenz herangezogen werden.

## <a name="_Toc395637775"></a>Nächste Schritte

* Möchten Sie Azure Cosmos DB nutzen, um Skalierungs- und Leistungstests durchzuführen? Weitere Informationen finden Sie unter [Leistungs- und Skalierungstests mit Azure Cosmos DB](performance-testing.md).
* Informieren Sie sich über das [Überwachen eines Azure Cosmos DB-Kontos](monitor-accounts.md).
* Fragen Sie unser Beispieldataset im [Query Playground](https://www.documentdb.com/sql/demo)ab.
* Sehen Sie sich die [Dokumentation für Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) an.

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-todo-app

