---
title: 'Erstellen eines Skillsets (REST-API-Version: 2017-11-11-Preview) – Azure Search | Microsoft-Dokumentation'
description: Ein Skillset ist eine Sammlung kognitiver Skills, die zur Anreicherung von Dokumenten dienen.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: language-reference
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: a5277d4137ede5fe6dacf993413eefd7c9e46ad4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33786909"
---
# <a name="create-skillset-api-version2017-11-11-preview"></a>Erstellen eines Skillsets (API-Version: 2017-11-11-Preview)

**Gilt für:** api-version-2017-11-11-Preview

Ein Skillset ist eine Sammlung kognitiver Skills, die für die Verarbeitung natürlicher Sprache und andere Transformationen verwendet werden. Zu diesen Skills gehören beispielsweise das Extrahieren von benannten Entitäten oder wichtigen Sätzen und das Verteilen von Text in Blöcken auf logischen Seiten.

Um das Skillset zu verwenden, verweisen Sie in einem Azure Search-Indexer darauf, und führen Sie den Indexer dann aus, um Daten zu importieren, Transformationen und Anreicherungen auszulösen und die Ausgabefelder einem Index zuzuordnen. Ein Skillset ist eine Ressource auf hoher Ebene, funktioniert aber nur innerhalb der Indexerverarbeitung. Da es sich um eine Ressource auf hoher Ebene handelt, können Sie ein Skillset einmal erstellen und dann in mehreren Indexern darauf verweisen. 

Ein Skillset wird in Azure Search durch eine HTTP PUT- oder POST-Anforderung ausgedrückt. Bei einer PUT-Anforderung ist der Text der Anforderung ein JSON-Schema, das angibt, welche Skills aufgerufen werden. Skills werden durch Eingabe-Ausgabe-Zuordnungen miteinander verkettet, wobei die Ausgabe einer Transformation zur Eingabe der nächsten wird.

Ein Skillset muss mindestens einen Skill umfassen. Theoretisch gibt es keine Obergrenze für die Anzahl von Skills, drei bis fünf ist jedoch eine gängige Konfiguration.  


> [!NOTE]
> Die kognitive Suche befindet sich in der öffentlichen Vorschau, und die Ausführung von Skillsets wird zurzeit kostenlos angeboten. Die Preise für diese Funktion werden zu einem späteren Zeitpunkt bekannt gegeben.

```http  
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```  

## <a name="request"></a>Anforderung  
 HTTPS ist für alle Dienstanforderungen erforderlich. Die Anforderung **Skillset erstellen** kann mithilfe einer PUT-Methode konstruiert werden, wobei der Name des Skillsets Teil der URL ist. Wenn das Skillset noch nicht vorhanden ist, wird es erstellt. Wenn es bereits vorhanden ist, wird es mit der neuen Definition aktualisiert. Beachten Sie, dass Sie nur ein Skillset gleichzeitig per PUT erstellen können.  

 Der Name des Skillsets muss folgende Voraussetzungen erfüllen:

- Er darf nur aus Kleinbuchstaben bestehen.
- Er muss mit einem Buchstaben oder einer Ziffer beginnen und enden.
- Er darf keine Schrägstriche oder Punkte enthalten.
- Er muss aus weniger als 128 Zeichen bestehen. 

Das erste Zeichen eines Skillsetnamens muss ein Buchstabe oder eine Ziffer sein. Der Rest des Namens kann beliebige Buchstaben, Ziffern und Bindestriche enthalten, allerdings dürfen Bindestriche nicht direkt aufeinander folgen.  

Der Parameter **api-version** ist erforderlich. Die einzig verfügbare Version ist `2017-11-11-Preview`. Eine Liste aller Versionen finden Sie unter [API-Versionen in Azure Search](https://go.microsoft.com/fwlink/?linkid=834796). 


### <a name="request-headers"></a>Anforderungsheader  

 Die folgende Tabelle beschreibt die erforderlichen und optionalen Anforderungsheader.  

|Anforderungsheader|BESCHREIBUNG|  
|--------------------|-----------------|  
|*Content-Type:*|Erforderlich. Auf `application/json`|  
|*api-key:*|Erforderlich. `api-key` wird zum Authentifizieren der Anforderung beim Search-Dienst verwendet. Es handelt sich um einen für Ihren Dienst eindeutigen Zeichenfolgewert. Die Anforderung **Skillset erstellen** muss einen `api-key`-Header enthalten, der auf Ihren Verwaltungsschlüssel festgelegt ist (im Gegensatz zu einem Abfrageschlüssel).|  

Sie benötigen auch den Dienstnamen, um die Anforderungs-URL zu erstellen. Sie können sowohl den Dienstnamen als auch `api-key` aus dem Dashboard Ihres Diensts im Azure-Portal abrufen. Hilfe bei der Seitennavigation finden Sie unter [Erstellen eines Azure-Suchdiensts im Portal](search-create-service-portal.md).  

### <a name="request-body-syntax"></a>Syntax des Anforderungstexts  

Der Anforderungstext enthält die Skillsetdefinition, die aus mindestens einem vollständig angegebenen Skill besteht, sowie optionale Parameter für Namen und Beschreibung.  

Die Syntax für die Strukturierung der Anforderungsnutzlast ist wie folgt. Sie finden eine Beispielanforderung weiter unten in diesem Artikel sowie unter [Definieren eines Skillsets](cognitive-search-defining-skillset.md).  

```
{   
    "name" : "Required for POST, optional for PUT. Friendly name of the skillset",  
    "description" : "Optional. Anything you want, or null",  
    "Skills" : "Required. An array of skills. Each skill has an odata.type, name, input and output parameters",  
}  
```

### <a name="request-example"></a>Anforderungsbeispiel
 Das folgende Beispiel erstellt ein Skillset zur Anreicherung einer Sammlung von Finanzdokumenten.

```http
PUT https://[servicename].search.windows.net/skillsets/financedocenricher?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```

Der Anforderungstext ist ein JSON-Dokument. Dieses spezifische Skillset verwendet zwei Skills auf asynchrone Weise und verarbeitet die Inhalte (`contents`) als zwei verschiedene Transformationen. Alternativ dazu können Sie auch die Ausgabe einer Transformation als Eingabe an die nächste Transformation weiterleiten. Weitere Informationen finden Sie unter [Definieren eines Skillsets](cognitive-search-defining-skillset.md).

```json
{
  "name": "financedocenricher",
  "description": 
  "Extract sentiment from financial records, extract company names, and then find additional information about each company mentioned.",
  "skills":
  [
    {
      "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "organizations",
          "targetName": "organizations"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "score",
          "targetName": "mySentiment"
        }
      ]
    },
  ]
}
```

## <a name="response"></a>response  

 Bei einer erfolgreichen Anforderung wird der Statuscode „201 – erstellt“ angezeigt.  

 Standardmäßig enthält der Antworttext das JSON-Schema für die erstellte Skillsetdefinition. Wenn der Prefer-Anforderungsheader jedoch auf „return=minimal“ festgelegt ist, ist der Antworttext leer und der Statuscode lautet „204 – kein Inhalt“ statt „201 – erstellt“. Dies gilt unabhängig davon, ob zum Erstellen des Skillsets eine PUT- oder eine POST-Anforderung verwendet wurde.   

## <a name="see-also"></a>Weitere Informationen

+ [Übersicht über die kognitive Suche](cognitive-search-concept-intro.md)
+ [Schnellstart: Testen der kognitiven Suche](cognitive-search-quickstart-blob.md)
+ [Tutorial: Angereicherte Indizierung von Azure-Blobs](cognitive-search-tutorial-blob.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)
+ [Zuordnen von Feldern](cognitive-search-output-field-mapping.md)
+ [Definieren einer benutzerdefinierten Schnittstelle](cognitive-search-custom-skill-interface.md)
+ [Beispiel: Erstellen eines benutzerdefinierten Skills](cognitive-search-create-custom-skill-example.md)
+ [Vordefinierte Qualifikationen](cognitive-search-predefined-skills.md)