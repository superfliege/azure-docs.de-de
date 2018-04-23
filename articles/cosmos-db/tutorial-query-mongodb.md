---
title: 'Azure Cosmos DB: Wie werden Abfragen mit der MongoDB-API durchgeführt? | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie mit der MongoDB-API für Azure Cosmos DB Abfragen durchführen.
services: cosmos-db
documentationcenter: ''
author: SnehaGunda
manager: kfile
ms.assetid: ''
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 03/29/2018
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: d8d524038d6483ff5da195648ee763f8faa1dad4
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="tutorial-query-azure-cosmos-db-by-using-the-mongodb-api"></a>Tutorial: Abfragen von Azure Cosmos BD mithilfe der MongoDB-API

Die [API für MongoDB](mongodb-introduction.md) von Azure Cosmos DB unterstützt [MongoDB-Shellabfragen](https://docs.mongodb.com/manual/tutorial/query-documents/). 

In diesem Artikel werden die folgenden Aufgaben behandelt: 

> [!div class="checklist"]
> * Abfragen von Daten mit MongoDB

Sehen Sie sich zur Einführung das folgende Video zu MongoDB-Abfragen mit Azure Cosmos DB-Programmleiter Andy Hoh an:

>[!VIDEO https://www.youtube.com/tVk8S7lFWMA]

## <a name="sample-document"></a>Beispieldokument

Die Abfragen in diesem Artikel verwenden das folgende Beispieldokument.

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```
## <a id="examplequery1"></a> Beispielabfrage 1 

Im Beispiel des obigen Familienbeispieldokuments gibt die folgende Abfrage die Dokumente zurück, in denen das Feld „id“ mit `WakefieldFamily` übereinstimmt.

**Abfrage**
    
    db.families.find({ id: “WakefieldFamily”})

**Ergebnisse**

    {
    "_id": "ObjectId(\"58f65e1198f3a12c7090e68c\")",
    "id": "WakefieldFamily",
    "parents": [
      {
        "familyName": "Wakefield",
        "givenName": "Robin"
      },
      {
        "familyName": "Miller",
        "givenName": "Ben"
      }
    ],
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ],
    "address": {
      "state": "NY",
      "county": "Manhattan",
      "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
    }

## <a id="examplequery2"></a>Beispielabfrage 2 

Die nächste Abfrage gibt alle Kinder in der Familie zurück. 

**Abfrage**
    
    db.families.find( { id: “WakefieldFamily” }, { children: true } )

**Ergebnisse**

    {
    "_id": "ObjectId("58f65e1198f3a12c7090e68c")",
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ]
    }


## <a id="examplequery3"></a> Beispielabfrage 3 

Die nächste Abfrage gibt alle registrierten Familien zurück. 

**Abfrage**
    
    db.families.find( { "isRegistered" : true })
**Ergebnisse** Kein Dokument wird zurückgegeben. 

## <a id="examplequery4"></a> Beispielabfrage 4

Die nächste Abfrage gibt alle nicht registrierten Familien zurück. 

**Abfrage**
    
    db.families.find( { "isRegistered" : false })
**Ergebnisse**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}

## <a id="examplequery5"></a> Beispielabfrage 5

Die nächste Abfrage gibt alle nicht registrierten Familien im Bundesstaat NY zurück. 

**Abfrage**
    
     db.families.find( { "isRegistered" : false, "address.state" : "NY" })

**Ergebnisse**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}


## <a id="examplequery6"></a> Beispielabfrage 6

Die nächste Abfrage gibt alle Familien zurück, deren Kinder in Klasse 8 gehen.

**Abfrage**
  
     db.families.find( { children : { $elemMatch: { grade : 8 }} } )

**Ergebnisse**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}

## <a id="examplequery7"></a> Beispielabfrage 7

Die nächste Abfrage gibt alle Familien zurück, wo das Kinderarray die Größe 3 hat.

**Abfrage**
  
      db.Family.find( {children: { $size:3} } )

**Ergebnisse**

Da keine Familien mit mehr als zwei Kindern vorhanden sind, werden keine Ergebnisse zurückgegeben. Nur wenn der Parameter 2 ist, ist diese Abfrage erfolgreich und gibt das vollständige Dokument zurück.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Sie haben gelernt, wie Sie Abfragen mithilfe von MongoDB durchführen. 

Sie können jetzt mit dem nächsten Tutorial fortfahren, um zu erfahren, wie Sie Ihre Daten global verteilen.

> [!div class="nextstepaction"]
> [Globales Verteilen Ihrer Daten](tutorial-global-distribution-sql-api.md)

