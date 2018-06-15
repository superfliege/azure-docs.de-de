---
title: Erstellen einer Node.js-Web-App für Azure Cosmos DB | Microsoft-Dokumentation
description: In diesem Node.js-Tutorial wird beschrieben, wie Sie mit Microsoft Azure Cosmos DB Daten aus einer in Azure-Websites gehosteten Node.js Express-Webanwendung speichern und darauf zugreifen.
keywords: Anwendungsentwicklung, Datenbanktutorial, node.js erlernen, node.js-Tutorial
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 03/23/2018
ms.author: sngun
ms.openlocfilehash: d18e6dd9464ef103157a8532215fa797ab282437
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34797480"
---
# <a name="_Toc395783175"></a>Erstellen einer Node.js-Webanwendung mithilfe von Azure Cosmos DB
> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Java](sql-api-java-application.md)
> * [Python](sql-api-python-application.md)
> 
> 

In diesem Node.js-Tutorial erfahren Sie, wie Sie mithilfe von Azure Cosmos DB und der SQL-API Daten aus einer in Azure Websites gehosteten Node.js Express-Anwendung speichern und abrufen. Sie erstellen eine einfache webbasierte Anwendung zur Aufgabenverwaltung (eine To-Do-Anwendung), mit der Sie Aufgaben erstellen, abrufen und durchführen können. Die Aufgaben werden als JSON-Dokumente in Azure Cosmos DB gespeichert. Dieses Lernprogramm führt Sie durch die einzelnen Schritte zur Erstellung und Bereitstellung der App und erläutert die Vorgänge in den einzelnen Ausschnitten.

![Screenshot der in diesem Node.js-Lernprogramm erstellten Anwendung "My Todo List"](./media/sql-api-nodejs-application/cosmos-db-node-js-mytodo.png)

Sie haben keine Zeit, um das Lernprogramm abzuschließen, und möchten nur die vollständige Lösung abrufen? Kein Problem, Sie können die vollständige Beispiellösung von [GitHub][GitHub] abrufen. Anweisungen zum Ausführen der App finden Sie in der [Infodatei](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md).

## <a name="_Toc395783176"></a>Voraussetzungen
> [!TIP]
> In diesem Node.js-Lernprogramm wird davon ausgegangen, dass Sie bereits Erfahrung mit Node.js und Azure-Websites haben.
> 
> 

Bevor Sie diesen Artikel durcharbeiten, sollten Sie sicherstellen, dass Folgendes vorhanden ist:

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js][Node.js] Version v0.10.29 oder höher. Es wird empfohlen, Node.js 6.10 oder höher zu verwenden.
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
   
        express todo
4. Öffnen Sie das neue **todo** -Verzeichnis, und installieren Sie alle Abhängigkeiten.
   
        cd todo
        npm install
5. Führen Sie die neue Anwendung aus.
   
        npm start
6. Sie können die neue Anwendung anzeigen, indem Sie in Ihrem Browser zu „[http://localhost:3000](http://localhost:3000)“ navigieren.
   
    ![Kennenlernen von Node.js - Screenshot der "Hello World"-Anwendung in einem Browserfenster](./media/sql-api-nodejs-application/cosmos-db-node-js-express.png)

    Drücken Sie zum Beenden der Anwendung im Terminalfenster STRG+C, und klicken Sie dann auf **J** (nur bei Windows-Computern), um den Batchauftrag zu beenden.

## <a name="_Toc395783179"></a>Schritt 3: Installieren zusätzlicher Module
Die Datei **package.json** ist eine der im Stammverzeichnis des Projekts erstellten Dateien. Diese Datei enthält eine Liste zusätzlicher Module, die für Ihre Node.js-Anwendung erforderlich sind. Wenn Sie später diese Anwendung in Azure Websites bereitstellen, wird anhand dieser Datei bestimmt, welche Module in Azure installiert werden müssen, um Ihre Anwendung zu unterstützen. Für dieses Lernprogramm müssen zwei weitere Pakete installiert werden.

1. Wechseln Sie zum Terminal, und installieren Sie das **async** -Modul über npm.
   
        npm install async --save
2. Installieren Sie das **documentdb** -Modul über npm. Dieses Modul ist für die Azure Cosmos DB-Funktionen zuständig.
   
        npm install documentdb --save

## <a name="_Toc395783180"></a>Schritt 4: Verwenden des Azure Cosmos DB-Diensts in einer Knotenanwendung
Dies ist für die anfängliche Einrichtung und Konfiguration erforderlich. Jetzt möchten wir zur eigentlichen Aufgabe übergehen, also zum Erstellen von Code mithilfe von Azure Cosmos DB.

### <a name="create-the-model"></a>Erstellen des Modells
1. Erstellen Sie unter dem Projektverzeichnis in dem Verzeichnis mit der Datei „package.json“ ein neues Verzeichnis namens **models**.
2. Erstellen Sie im Verzeichnis **models** eine neue Datei namens **task-model.js**. Diese Datei enthält das Modell für die von Ihrer Anwendung erstellten Aufgaben.
3. Erstellen Sie im gleichen Verzeichnis **models** eine weitere neue Datei namens **cosmosdb-manager.js**. Diese Datei enthält einigen nützlichen, wiederverwendbaren Code, den wir in unserer Anwendung verwenden werden. 
4. Kopieren Sie den folgenden Code in **cosmosdb-manager.js**.
    ```nodejs
    let DocumentDBClient = require('documentdb').DocumentClient;

    module.exports = {
    getOrCreateDatabase: (client, databaseId, callback) => {
        let querySpec = {
        query: 'SELECT * FROM root r WHERE r.id = @id',
        parameters: [{ name: '@id', value: databaseId }]
        };

        client.queryDatabases(querySpec).toArray((err, results) => {
        if (err) {
            callback(err);
        } else {
            if (results.length === 0) {
            let databaseSpec = { id: databaseId };
            client.createDatabase(databaseSpec, (err, created) => {
                callback(null, created);
            });
            } else {
            callback(null, results[0]);
            }
        }
        });
    },

    getOrCreateCollection: (client, databaseLink, collectionId, callback) => {
        let querySpec = {
        query: 'SELECT * FROM root r WHERE r.id=@id',
        parameters: [{ name: '@id', value: collectionId }]
        };

        client.queryCollections(databaseLink, querySpec).toArray((err, results) => {
        if (err) {
            callback(err);
        } else {
            if (results.length === 0) {
            let collectionSpec = { id: collectionId };
            client.createCollection(databaseLink, collectionSpec, (err, created) => {
                callback(null, created);
            });
            } else {
            callback(null, results[0]);
            }
        }
        });
    }
    };
    ```
5. Speichern und schließen Sie die Datei **cosmosdb-manager.js**.
6. Fügen Sie am Anfang der Datei **task-model.js** den folgenden Code ein, um auf die oben erstellten Dateien **DocumentDBClient** und **cosmosdb-manager.js** zu verweisen: 

    ```nodejs
    let DocumentDBClient = require('documentdb').DocumentClient;
    let docdbUtils = require('./cosmosdb-manager.js');
    ```
7. Anschließend fügen Sie Code hinzu, um das Task-Objekt zu definieren und zu exportieren. Dieser ist verantwortlich für das Initialisieren des Task-Objekts und für das Einrichten der Datenbank und der Dokumentenauflistung, die wir verwenden werden.  

    ```nodejs
    function TaskModel(documentDBClient, databaseId, collectionId) {
      this.client = documentDBClient;
      this.databaseId = databaseId;
      this.collectionId = collectionId;
   
      this.database = null;
      this.collection = null;
    }
   
    module.exports = TaskModel;
    ```
8. Fügen Sie als Nächstes den folgenden Code hinzu, um zusätzliche Methoden für das Task-Objekt zu definieren, die Interaktionen mit in Azure Cosmos DB gespeicherten Daten ermöglichen.

    ```nodejs
    let DocumentDBClient = require('documentdb').DocumentClient;
    let docdbUtils = require('./cosmosdb-manager');

    function TaskModel(documentDBClient, databaseId, collectionId) {
    this.client = documentDBClient;
    this.databaseId = databaseId;
    this.collectionId = collectionId;

    this.database = null;
    this.collection = null;
    }

    TaskModel.prototype = {
    init: function(callback) {
        let self = this;

        docdbUtils.getOrCreateDatabase(self.client, self.databaseId, function(err, db) {
        if (err) {
            callback(err);
        } else {
            self.database = db;
            docdbUtils.getOrCreateCollection(self.client, self.database._self, self.collectionId, function(err, coll) {
            if (err) {
                callback(err);
            } else {
                self.collection = coll;
            }
            });
        }
        });
    },

    find: function(querySpec, callback) {
        let self = this;

        self.client.queryDocuments(self.collection._self, querySpec).toArray(function(err, results) {
        if (err) {
            callback(err);
        } else {
            callback(null, results);
        }
        });
    },

    addItem: function(item, callback) {
        let self = this;

        item.date = Date.now();
        item.completed = false;

        self.client.createDocument(self.collection._self, item, function(err, doc) {
        if (err) {
            callback(err);
        } else {
            callback(null, doc);
        }
        });
    },

    updateItem: function(itemId, callback) {
        let self = this;

        self.getItem(itemId, function(err, doc) {
        if (err) {
            callback(err);
        } else {
            doc.completed = true;

            self.client.replaceDocument(doc._self, doc, function(err, replaced) {
            if (err) {
                callback(err);
            } else {
                callback(null, replaced);
            }
            });
        }
        });
    },

    getItem: function(itemId, callback) {
        let self = this;
        let querySpec = {
        query: 'SELECT * FROM root r WHERE r.id = @id',
        parameters: [{ name: '@id', value: itemId }]
        };

        self.client.queryDocuments(self.collection._self, querySpec).toArray(function(err, results) {
        if (err) {
            callback(err);
        } else {
            callback(null, results[0]);
        }
        });
    }
    };

    module.exports = TaskModel;
    ```
9. Speichern und schließen Sie die Datei **task-model.js**. 

### <a name="create-the-controller"></a>Erstellen des Controllers
1. Erstellen Sie im Verzeichnis **routes** des Projekts eine neue Datei namens **tasklist.js**. 
2. Fügen Sie **tasklist.js**den folgenden Code hinzu. Hierdurch werden die Module "DocumentDBClient" und "async" geladen, die von **tasklist.js**verwendet werden. Darüber hinaus wird hierdurch die Funktion **TaskList** definiert, an die eine Instanz des zuvor definierten **Task**-Objekts übergeben wird:
   
    ```nodejs
    let DocumentDBClient = require('documentdb').DocumentClient;
    let async = require('async');

    function TaskList(taskModel) {
    this.taskModel = taskModel;
    }

    module.exports = TaskList;
    ```
3. Fügen Sie außerdem zur Datei **tasklist.js** die Methoden **showTasks, addTask** und **completeTasks** hinzu:
   
   ```nodejs
    TaskList.prototype = {
    showTasks: function(req, res) {
        let self = this;

        let querySpec = {
        query: 'SELECT * FROM root r WHERE r.completed=@completed',
        parameters: [
            {
            name: '@completed',
            value: false
            }
        ]
        };

        self.taskModel.find(querySpec, function(err, items) {
        if (err) {
            throw err;
        }

        res.render('index', {
            title: 'My ToDo List ',
            tasks: items
        });
        });
    },

    addTask: function(req, res) {
        let self = this;
        let item = req.body;

        self.taskModel.addItem(item, function(err) {
        if (err) {
            throw err;
        }

        res.redirect('/');
        });
    },

    completeTask: function(req, res) {
        let self = this;
        let completedTasks = Object.keys(req.body);

        async.forEach(
        completedTasks,
        function taskIterator(completedTask, callback) {
            self.taskModel.updateItem(completedTask, function(err) {
            if (err) {
                callback(err);
            } else {
                callback(null);
            }
            });
        },
        function goHome(err) {
            if (err) {
            throw err;
            } else {
            res.redirect('/');
            }
        }
        );
    }
    };
    ```        
4. Speichern und schließen Sie die Datei **tasklist.js** .

### <a name="add-configjs"></a>Fügen Sie config.js hinzu.
1. Erstellen Sie im Projektverzeichnis eine neue Datei namens **config.js**.
2. Fügen Sie **config.js**Folgendes hinzu. Dadurch werden die für Ihre Anwendung erforderlichen Konfigurationseinstellungen und Werte definiert.
   
    ```nodejs
    let config = {}
   
    config.host = process.env.HOST || "[the URI value from the Azure Cosmos DB Keys page on http://portal.azure.com]";
    config.authKey = process.env.AUTH_KEY || "[the PRIMARY KEY value from the Azure Cosmos DB Keys page on http://portal.azure.com]";
    config.databaseId = "ToDoList";
    config.collectionId = "Items";
   
    module.exports = config;
    ```
3. Aktualisieren Sie in der Datei **config.js** die Werte für „HOST“ und „AUTH_KEY“ mit den Werten auf der Seite „Schlüssel“ Ihres Azure Cosmos DB-Kontos im [Microsoft Azure-Portal](https://portal.azure.com).
4. Speichern und schließen Sie die Datei **config.js** .

### <a name="modify-appjs"></a>Ändern von app.js
1. Öffnen Sie im Projektverzeichnis die Datei **app.js** . Diese Datei wurde bereits während der Erstellung der Express-Webanwendung erstellt.
2. Fügen Sie am Anfang von **app.js** den folgenden Code hinzu:
   
    ```nodejs
    var DocumentDBClient = require('documentdb').DocumentClient;
    var config = require('./config');
    var TaskList = require('./routes/tasklist');
    var TaskModel = require('./models/taskModel');
    ```
3. Dieser Code definiert die zu verwendende Konfigurationsdatei und liest Werte aus dieser Datei für einige Variablen, die wir später verwenden.
4. Ersetzen Sie die folgenden beiden Zeilen in der Datei **app.js** :
   
    ```nodejs
    app.use('/', index);
    app.use('/users', users); 
    ```
   
    durch den folgenden Codeausschnitt:
   
    ```nodejs
    let docDbClient = new DocumentDBClient(config.host, {
        masterKey: config.authKey
    });
    let taskModel = new TaskModel(docDbClient, config.databaseId, config.collectionId);
    let taskList = new TaskList(taskModel);
    taskModel.init();
   
    app.get('/', taskList.showTasks.bind(taskList));
    app.post('/addtask', taskList.addTask.bind(taskList));
    app.post('/completetask', taskList.completeTask.bind(taskList));
    app.set('view engine', 'jade');
    ```
5. Diese Zeilen definieren eine neue Instanz unseres **TaskModel**-Objekts mit einer neuen Verbindung zu Azure Cosmos DB (unter Verwendung der aus **config.js** gelesenen Werte). Sie initialisieren das Task-Objekt und binden dann Formularaktionen an Methoden auf unserem **TaskList**-Controller. 
6. Speichern und schließen Sie abschließend die Datei **app.js**. Wir sind fast fertig.

## <a name="_Toc395783181"></a>Schritt 5: Erstellen einer Benutzeroberfläche
Jetzt konzentrieren wir uns auf die Erstellung der Benutzeroberfläche, um den Benutzern die eigentliche Interaktion mit unserer Anwendung zu ermöglichen. Die von uns erstellte Express-Anwendung verwendet **Jade** als Anzeige-Engine. Weitere Informationen zu Jade finden Sie unter „[http://jade-lang.com/](http://jade-lang.com/)“.

1. Die Datei **layout.jade** im Verzeichnis **views** dient als globale Vorlage für andere **.jade**-Dateien. In diesem Schritt werden Sie sie modifizieren, um [Twitter Bootstrap](https://github.com/twbs/bootstrap)zu verwenden, ein Toolkit zum mühelosen Gestalten ansprechender Websites. 
2. Öffnen Sie die Datei **layout.jade**, die sich im Ordner **views** befindet, und ersetzen Sie die Inhalte durch Folgendes:

    ```
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
                       input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
             button.btn.btn-primary(type="submit") Update tasks
           hr
           form.well(action="/addtask", method="post")
             .form-group
               label(for="name") Item Name:
               input.form-control(name="name", type="textbox")
             .form-group
               label(for="category") Item Category:
               input.form-control(name="category", type="textbox")
             br
             button.btn(type="submit") Add item
   

Dadurch wird das Layout erweitert und Inhalt für den Platzhalter **content** bereitgestellt, den wir zuvor in der Datei **layout.jade** gesehen haben.
   
In diesem Layout haben wir zwei HTML-Formulare erstellt.

Das erste Formular enthält eine Tabelle für unsere Daten sowie eine Schaltfläche, mit der die Elemente durch Übermittlung an die **/completetask** -Methode des Controllers aktualisiert werden können.
    
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
[GitHub]: https://github.com/Azure-Samples/documentdb-node-todo-app

