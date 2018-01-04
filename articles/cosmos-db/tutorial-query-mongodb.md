---
title: "Azure Cosmos DB: Wie werden Abfragen mit der MongoDB-API durchgeführt? | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mit der MongoDB-API für Azure Cosmos DB Abfragen durchführen."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 1818476a95ddf373701ad93860b02ea4c2ad761d
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2017
---
# <a name="azure-cosmos-db-how-to-query-with-api-for-mongodb"></a>Azure Cosmos DB: Wie werden Abfragen mit der API für MongoDB durchgeführt?

Die [API für MongoDB](mongodb-introduction.md) von Azure Cosmos DB unterstützt [MongoDB-Shellabfragen](https://docs.mongodb.com/manual/tutorial/query-documents/). 

In diesem Artikel werden die folgenden Aufgaben behandelt: 

> [!div class="checklist"]
> * Abfragen von Daten mit MongoDB

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
    
    db.familes.find( { id: “WakefieldFamily” }, { children: true } )

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

Die nächste Abfrage gibt alle Familien zurück, die registriert sind. 

**Abfrage**
    
    db.families.find( { "isRegistered" : true })
**Ergebnisse** Kein Dokument wird zurückgegeben. 

## <a id="examplequery4"></a> Beispielabfrage 4

Die nächste Abfrage gibt alle Familien zurück, die nicht registriert sind. 

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

Die nächste Abfrage gibt alle Familien zurück, die nicht registriert sind und im Staat NY leben. 

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

Es werden keine Ergebnisse zurückgegeben, da nicht mehr als 2 Kinder vorhanden sind. Nur wenn der Parameter 2 ist, ist diese Abfrage erfolgreich und gibt das vollständige Dokument zurück.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Sie haben gelernt, wie Sie Abfragen mithilfe von MongoDB durchführen. 

Sie können jetzt mit dem nächsten Tutorial fortfahren, um zu erfahren, wie Sie Ihre Daten global verteilen.

> [!div class="nextstepaction"]
> [Globales Verteilen Ihrer Daten](tutorial-global-distribution-sql-api.md)

