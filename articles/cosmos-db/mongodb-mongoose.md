---
title: Verwenden des Mongoose-Frameworks mit Azure Cosmos DB | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Node.js Mongoose-App mit Azure Cosmos DB verbinden.
services: cosmos-db
author: slyons
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 01/08/2018
ms.author: sclyon
ms.openlocfilehash: 8cfa53a1792d8e01c05aad8e4a1a0b5239a092c1
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857391"
---
# <a name="azure-cosmos-db-using-the-mongoose-framework-with-azure-cosmos-db"></a>Azure Cosmos DB: Verwenden des Mongoose-Frameworks mit Azure Cosmos DB

In diesem Tutorial wird veranschaulicht, wie das [Mongoose-Framework](http://mongoosejs.com/) verwendet werden kann, wenn Daten in Azure Cosmos DB gespeichert werden. Für diese exemplarische Vorgehensweise wird die MongoDB-API für Azure Cosmos DB verwendet. Für diejenigen, die Mongoose noch nicht kennen: Mongoose ist ein Objektmodellierungsframework für MongoDB in Node.js und stellt eine schnörkellose schemabasierte Lösung bereit, mit der Sie Ihre Anwendungsdaten modellieren können.

Azure Cosmos DB ist ein global verteilter Datenbankdienst von Microsoft mit mehreren Modellen. Sie können schnell Dokument-, Schlüssel/Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets die Vorteile der globalen Verteilung und der horizontalen Skalierung nutzen, die Azure Cosmos DB zugrunde liegen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

[Node.js](https://nodejs.org/) Version v0.10.29 oder höher.

## <a name="create-an-azure-cosmos-db-account"></a>Erstellen eines Azure Cosmos DB-Kontos

Wir erstellen nun ein Azure Cosmos DB-Konto. Wenn Sie bereits über ein Konto verfügen, das Sie verwenden möchten, können Sie diesen Schritt überspringen und mit [Einrichten der Node.js-Anwendung](#SetupNode) fortfahren. Wenn Sie den Azure Cosmos DB-Emulator verwenden, führen Sie die Schritte unter [Azure Cosmos DB-Emulator](local-emulator.md) zum Einrichten des Emulators aus, und fahren Sie dann mit [Einrichten der Node.js-Anwendung](#SetupNode) fort.

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="set-up-your-nodejs-application"></a>Einrichten der Node.js-Anwendung

>[!Note]
> Wenn Sie sich nur den Beispielcode ansehen möchten, statt die Anwendung einzurichten, klonen Sie das [Beispiel](https://github.com/Azure-Samples/Mongoose_CosmosDB), das für dieses Tutorial verwendet wird, und erstellen Sie die Node.js Mongoose-Anwendung in Azure Cosmos DB.

1. Um eine Node.js-Anwendung in dem von Ihnen gewünschten Ordner zu erstellen, führen Sie den folgenden Befehl an einer Eingabeaufforderung für den Knoten aus.

    ```npm init```

    Beantworten Sie die Fragen. Danach ist das Projekt einsatzbereit.

1. Fügen Sie im Ordner eine neue Datei hinzu, und nennen Sie diese ```index.js```.
1. Installieren Sie die erforderlichen Pakete, indem Sie eine der ```npm install```-Optionen verwenden:
    * Mongoose:```npm install mongoose@5 --save```

    > [!Note]
    > Die Mongoose-Beispielverbindung unten basiert auf Mongoose 5+, das sich seit früheren Versionen geändert hat.
    
    * Dotenv (wenn Sie Ihre Geheimnisse aus einer Datei ENV-Datei laden möchten):```npm install dotenv --save```

    >[!Note]
    > Das ```--save```-Flag bewirkt, dass die Abhängigkeit zur Datei „package.json“ hinzugefügt wird.

1. Importieren Sie die Abhängigkeiten in Ihre „index.js“-Datei.
    ```JavaScript
    var mongoose = require('mongoose');
    var env = require('dotenv').load();    //Use the .env file to load the variables
    ```

1. Fügen Sie Ihre Cosmos DB-Verbindungszeichenfolge und Ihren Cosmos DB-Namen in der ```.env```-Datei hinzu.

    ```JavaScript
    COSMOSDB_CONNSTR=mongodb://{cosmos-user}.documents.azure.com:10255/{dbname}
    COSMODDB_USER=cosmos-user
    COSMOSDB_PASSWORD=cosmos-secret
    ```

1. Stellen Sie über das Mongoose-Framework eine Verbindung mit Azure Cosmos DB her, indem Sie den folgenden Code am Ende von „index.js“ hinzufügen.
    ```JavaScript
    mongoose.connect(process.env.COSMOSDB_CONNSTR+"?ssl=true&replicaSet=globaldb", {
      auth: {
        user: process.env.COSMODDB_USER,
        password: process.env.COSMOSDB_PASSWORD
      }
    })
    .then(() => console.log('Connection to CosmosDB successful'))
    .catch((err) => console.error(err));
    ```
    >[!Note]
    > Hier werden die Umgebungsvariablen mithilfe des npm-Pakets „dotenv“ über „process.env.{Variablenname}“ geladen.

    Sobald Sie eine Verbindung mit Azure Cosmos DB hergestellt haben, können Sie damit beginnen, Objektmodelle in Mongoose einzurichten.

## <a name="caveats-to-using-mongoose-with-azure-cosmos-db"></a>Einschränkungen für das Verwenden von Mongoose mit Azure Cosmos DB

Für jedes Modell, das Sie erstellen, erstellt Mongoose eine neue MongoDB-Sammlung unter der Hauptebene. Dies ist, entsprechend dem sammlungsbezogenen Abrechnungsmodell von Azure Cosmos DB, möglicherweise nicht die kostengünstige Methode, wenn Sie mehrere Objektmodelle haben, die dünn besetzt sind.

In dieser exemplarischen Vorgehensweise werden beide Modelle behandelt. Zuerst wird das Modell vorgestellt, in dem pro Sammlung Daten eines Typs gespeichert werden. Dies ist das faktische Verhalten für Mongoose.

Mongoose hat auch ein Konzept, das als [Diskriminatoren](http://mongoosejs.com/docs/discriminators.html) bezeichnet wird. Diskriminatoren sind ein Mechanismus zu Schemavererbung. Sie ermöglichen Ihnen, mehrere Modelle mit sich überschneidenden Schemas aufgesetzt auf derselben zugrunde liegenden MongoDB-Sammlung zu haben.

Sie können die verschiedenen Datenmodelle in derselben Sammlung speichern und dann zur Abfragezeit eine Filterklausel verwenden, um nur die benötigten Daten zu abzurufen.

### <a name="one-collection-per-object-model"></a>Eine Sammlung pro Objektmodell

Das Mongoose-Standardverhalten sieht so aus, dass jedes Mal, wenn Sie ein Objektmodell erstellen, eine MongoDB-Sammlung erstellt wird. In diesem Abschnitt wird erklärt, wie sich dies mit MongoDB für Azure Cosmos DB erreichen lässt. Diese Methode empfiehlt sich für Azure Cosmos DB, wenn Sie Objektmodelle mit großen Datenmengen haben. Dies ist das Standardausführungsmodell für Mongoose, weshalb Sie möglicherweise damit vertraut sind, wenn Sie mit Mongoose vertraut sind.

1. Öffnen Sie erneut Ihre ```index.js```-Datei.

1. Erstellen Sie die Schemadefinition für „Family“.

    ```JavaScript
    const Family = mongoose.model('Family', new mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
            gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }));
    ```

1. Erstellen Sie ein Objekt für „Family“.

    ```JavaScript
    const family = new Family({
        lastName: "Volum",
        parents: [
            { firstName: "Thomas" },
            { firstName: "Mary Kay" }
        ],
        children: [
            { firstName: "Ryan", gender: "male", grade: 8 },
            { firstName: "Patrick", gender: "male", grade: 7 }
        ],
        pets: [
            { givenName: "Blackie" }
        ],
        address: { country: "USA", state: "WA", city: "Seattle" }
    });
    ```

1. Speichern Sie das Objekt schließlich in Azure Cosmos DB. Dadurch wird eine Sammlung unter der Hauptebene erstellt.

    ```JavaScript
    family.save((err, saveFamily) => {
        console.log(JSON.stringify(saveFamily));
    });
    ```

1. Erstellen Sie nun ein weiteres Schema und Objekt. Erstellen Sie dieses Mal ein Schema für Urlaubsziele (VacationDestinations), an denen die Familien interessiert sein könnten.
    1. Erstellen Sie das Schema so wie beim letzten Mal.
    ```JavaScript
    const VacationDestinations = mongoose.model('VacationDestinations', new mongoose.Schema({
        name: String,
        country: String
    }));
    ```

    1. Erstellen Sie ein Beispielobjekt (Sie können mehrere Objekte zu diesem Schema hinzufügen), und speichern Sie es.
    ```JavaScript
    const vacaySpot = new VacationDestinations({
        name: "Honolulu",
        country: "USA"
    });

    vacaySpot.save((err, saveVacay) => {
        console.log(JSON.stringify(saveVacay));
    });
    ```

1. Nachdem Sie zum Azure-Portal gewechselt haben, sehen Sie nun zwei Sammlungen, die in Azure Cosmos DB erstellt wurden.

    ![Node.js-Tutorial – Momentaufnahme des Azure-Portals, in dem ein Azure Cosmos DB-Konto angezeigt wird, wobei mehrere Sammlungsnamen hervorgehoben sind – Node-Datenbank][mutiple-coll]

1. Lesen Sie schließlich die Daten aus Azure Cosmos DB. Da das Mongoose-Standardausführungsmodell verwendet wird, sind die Lesevorgänge mit beliebigen anderen Lesevorgängen mit Mongoose identisch.

    ```JavaScript
    Family.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family: " + JSON.stringify(fam)));
    });
    ```

### <a name="using-mongoose-discriminators-to-store-data-in-a-single-collection"></a>Verwenden von Mongoose-Diskriminatoren, um Daten in einer einzigen Sammlung zu speichern

In dieser Methode werden [Mongoose-Diskriminatoren](http://mongoosejs.com/docs/discriminators.html) verwendet, um möglichst geringe Kosten für jede Azure Cosmos DB-Sammlung zu erreichen. Diskriminatoren ermöglichen es Ihnen, einen unterscheidenden Schlüssel (discriminatorKey) zu definieren, wodurch Sie die Möglichkeit erhalten, unterschiedliche Objektmodelle zu speichern und nach diesen zu unterscheiden und zu filtern.

Sie erstellen ein Basisobjektmodell, definieren einen Differenzierungsschlüssel und fügen „Family“ und „VacationDestinations“ dem Basismodell als Erweiterung hinzu.

1. Richten Sie die Basiskonfiguration ein, und definieren Sie den Diskriminatorschlüssel.

    ```JavaScript
    const baseConfig = {
        discriminatorKey: "_type", //If you've got a lot of different data types, you could also consider setting up a secondary index here.
        collection: "alldata"   //Name of the Common Collection
    };
    ```

1. Definieren Sie als Nächstes das allgemeine Objektmodell.

    ```JavaScript
    const commonModel = mongoose.model('Common', new mongoose.Schema({}, baseConfig));
    ```

1. Definieren Sie nun das „Family“-Modell. Beachten Sie hier, dass ```commonModel.discriminator``` anstelle von ```mongoose.model``` verwendet wird. Außerdem wird die Basiskonfiguration zum Mongoose-Schema hinzugefügt. Somit ist der Diskriminatorschlüssel (discriminatorKey) hier gleich ```FamilyType```.

    ```JavaScript
    const Family_common = commonModel.discriminator('FamilyType', new     mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
           gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }, baseConfig));
    ```

1. Fügen Sie auf gleiche Weise ein weiteres Schema hinzu, und zwar diesmal für die Urlaubsziele (VacationDestinations). Hierfür ist der Diskriminatorschlüssel (discriminatorKey) gleich ```VacationDestinationsType```.

    ```JavaScript
    const Vacation_common = commonModel.discriminator('VacationDestinationsType', new mongoose.Schema({
        name: String,
        country: String
    }, baseConfig));
    ```

1. Erstellen Sie schließlich Objekte für das Modell, und speichern Sie das Modell.
    1. Fügen Sie Objekte zum „Family“-Modell hinzu.
    ```JavaScript
    const family_common = new Family_common({
        lastName: "Volum",
        parents: [
            { firstName: "Thomas" },
            { firstName: "Mary Kay" }
        ],
        children: [
            { firstName: "Ryan", gender: "male", grade: 8 },
            { firstName: "Patrick", gender: "male", grade: 7 }
        ],
        pets: [
            { givenName: "Blackie" }
        ],
        address: { country: "USA", state: "WA", city: "Seattle" }
    });

    family_common.save((err, saveFamily) => {
        console.log("Saved: " + JSON.stringify(saveFamily));
    });
    ```

    1. Fügen Sie als Nächstes Objekte zum „VacationDestinations“-Modell hinzu, und speichern Sie es.
    ```JavaScript
    const vacay_common = new Vacation_common({
        name: "Honolulu",
        country: "USA"
    });

    vacay_common.save((err, saveVacay) => {
        console.log("Saved: " + JSON.stringify(saveVacay));
    });
    ```

1. Wenn Sie nun zum Azure-Portal zurückkehren, sehen Sie, dass es nur eine Sammlung namens ```alldata``` gibt, die sowohl die „Family“- als auch die „VacationDestinations“-Daten enthält.

    ![Node.js-Tutorial – Momentaufnahme des Azure-Portals, in dem ein Azure Cosmos DB-Konto angezeigt wird, wobei der Sammlungsname hervorgehoben ist – Node-Datenbank][alldata]

1. Beachten Sie auch, dass jedes Objekt ein weiteres Attribut namens ```__type``` hat, das es Ihnen erleichtert, zwischen den beiden Objektmodellen zu unterscheiden.

1. Lesen Sie schließlich die Daten, die in Azure Cosmos DB gespeichert sind. Mongoose übernimmt das Filtern von Daten anhand des Modells. Daher müssen Sie nicht anders vorgehen, wenn Sie Daten lesen. Geben Sie einfach Ihr Modell an (in diesem Fall ```Family_common```), und Mongoose verwaltet das Filtern über „DiscriminatorKey“.

    ```JavaScript
    Family_common.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family (using discriminator): " + JSON.stringify(fam)));
    });
    ```

Wie Sie sehen, ist ein Arbeiten mit Mongoose-Diskriminatoren einfach. Somit bietet Ihnen dieses Tutorial, wenn Sie eine App haben, in der das Mongoose-Framework verwendet wird, eine Möglichkeit, Ihre Anwendung für die MongoDB-API zu Azure Cosmos DB einzurichten und auszuführen, ohne dass zu viele Änderungen erforderlich sind.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über die MongoDB-Vorgänge, -Operatoren, -Phasen, -Befehle und -Optionen, die von der Azure Cosmos DB MongoDB-API unterstützt werden, finden Sie unter [Unterstützung der MongoDB-API für Features und Syntax von MongoDB](mongodb-feature-support.md).

[alldata]: ./media/mongodb-mongoose/mongo-collections-alldata.png
[mutiple-coll]: ./media/mongodb-mongoose/mongo-mutliple-collections.png
