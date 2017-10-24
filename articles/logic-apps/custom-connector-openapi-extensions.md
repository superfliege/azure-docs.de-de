---
title: "OpenApi-Erweiterungen für benutzerdefinierte Connectors – Azure Logic Apps | Microsoft-Dokumentation"
description: Erweitern der OpenAPI Ihres benutzerdefinierten Connectors mit erweiterten Funktionen
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: e8565019bd4631043485224c73c2fe60e633951c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="extend-your-custom-connectors-openapi-with-advanced-functionality"></a>Erweitern der OpenAPI Ihres benutzerdefinierten Connectors mit erweiterten Funktionen

Um benutzerdefinierte Connectors für Azure Logic Apps, Microsoft Flow oder Microsoft PowerApps zu erstellen, müssen Sie eine OpenAPI-Definitionsdatei bereitstellen. Diese Datei ist ein sprachunabhängiges, maschinenlesbares Dokument, das die Vorgänge und Parameter Ihrer API beschreibt. Neben der Standardfunktionalität von OpenAPI können Sie auch diese OpenAPI-Erweiterungen einbinden, wenn Sie benutzerdefinierte Connectors für Logic Apps und Flow erstellen:

* [`summary`](#summary)
* [`x-ms-summary`](#x-ms-summary)
* [`description`](#description)
* [`x-ms-visibility`](#x-ms-visibility)
* [`x-ms-dynamic-values`](#x-ms-dynamic-values)
* [`x-ms-dynamic-schema`](#x-ms-dynamic-schema)

Es folgen weitere Details zu diesen Erweiterungen:

<a name="summary"></a>

## <a name="summary"></a>summary

Gibt den Titel für die Aktion (bzw. Vorgang) an. </br>
Gilt für: Vorgänge </br>
Empfohlene: Verwenden Sie für `summary` das Format *Ersten Buchstaben im Satz großschreiben*. </br>
Beispiel: "Wenn ein Ereignis hinzugefügt dem Kalender wird" oder "E-Mail senden"

![„summary“ für jeden Vorgang](./media/custom-connector-openapi-extensions/summary.png)

``` json
"actions" {
  "Send_an_email": {
    /// Other action properties here...
    "summary": "Send an email",
    /// Other action properties here...
  }
},
```

<a name="x-ms-summary"></a>

## <a name="x-ms-summary"></a>x-ms-summary

Gibt den Titel für eine Entität an. </br>
Gilt für: Parameter, Antwortschema </br>
Empfohlene: Verwenden Sie für `x-ms-summary` das Format *Große Anfangsbuchstaben*. </br>
Beispiel: Kalender-ID, Betreff, Beschreibung des Ereignisses usw.

!["x-ms-summary" für jede Entität](./media/custom-connector-openapi-extensions/x-ms-summary.png)

``` json
"actions" {
  "Send_an_email": {
    /// Other action properties here...
    "parameters": [ 
      {
        /// Other parameters here...
        "x-ms-summary": "Subject",
        /// Other parameters here...
      }
    ]
  }
},
```

<a name="description"></a>

## <a name="description"></a>Beschreibung

Gibt eine ausführliche Erläuterung der Funktionsweise des Vorgangs oder des Formats und der Funktion einer Entität an. </br>
Gilt für: Vorgänge, Parameter, Antwortschema </br>
Empfohlene: Verwenden Sie für `description` das Format *Ersten Buchstaben im Satz großschreiben*. </br>
Beispiel: „Dieser Vorgang wird ausgelöst, wenn dem Kalender ein neues Ereignis hinzugefügt wird“, „Geben Sie den Betreff der E-Mail an“ usw.

![„description“ für jeden Vorgang oder jede Entität](./media/custom-connector-openapi-extensions/description.png)

``` json
"actions" {
  "Send_an_email": {
     "description": "Specify the subject of the mail",
     /// Other action properties here...
  }
},
```

<a name="visibility"></a>

## <a name="x-ms-visibility"></a>x-ms-visibility

Gibt die Sichtbarkeit einer Entität für Benutzer an. </br>
Mögliche Werte: `important`, `advanced` und `internal` </br>
Gilt für: Vorgänge, Parameter, Schemas

* Vorgänge und Parameter des Typs `important` werden dem Benutzer stets zuerst angezeigt.
* Vorgänge und Parameter des Typs `advanced` sind in einem zusätzlichen Menü versteckt.
* Vorgänge und Parameter des Typs `internal` werden dem Benutzer nicht angezeigt.

> [!NOTE] 
> Sie **müssen** Standardwerte für Parameter des Typs `internal` und `required` angeben.

Beispiel: In den Menüs **Mehr anzeigen** und **Erweiterte Optionen anzeigen** sind Vorgänge und Parameter des Typs `advanced` ausgeblendet.

![„x-ms-visibility“ zum Anzeigen oder Ausblenden von Vorgängen und Parametern](./media/custom-connector-openapi-extensions/x-ms-visibility.png)

``` json
"actions" {
  "Send_an_email": {
     /// Other action properties here...
     "parameters": [
         {
           "name": "Subject",
           "type": "string",
           "description": "Specify the subject of the mail",
           "x-ms-summary": "Subject",
           "x-ms-visibility": "important",
           /// Other parameter properties here...
         }
     ]
     /// Other action properties here...
  }
},
```

<a name="x-ms-dynamic-values"></a>

## <a name="x-ms-dynamic-values"></a>x-ms-dynamic-values

Zeigt eine für den Benutzer ausgefüllte Liste an, in der er die Eingabeparameter für einen Vorgang auswählen kann. </br>
Gilt für: Parameter </br>
Vorgehensweise: Hinzufügen des `x-ms-dynamic-values`-Objekts zur Definition des Parameters. Siehe beispielsweise dieses [OpenAPI-Beispiel](https://procsi.blob.core.windows.net/blog-images/sampleDynamicSwagger.json).

![„x-ms-dynamic-values“ für das Anzeigen von Listen](./media/custom-connector-openapi-extensions/x-ms-dynamic-values.png)

### <a name="properties-for-x-ms-dynamic-values"></a>Eigenschaften für „x-ms-dynamic-values“

| Name | Erforderlich oder optional | Beschreibung | 
| ---- | -------------------- | ----------- | 
| **operationID** | Erforderlich | Der zum Auffüllen der Liste aufzurufende Vorgang | 
| **value-path** | Erforderlich | Eine Pfadzeichenfolge im Objekt innerhalb von `value-collection`, die auf den Parameterwert verweist. </br>Wenn `value-collection` nicht angegeben ist, wird die Antwort als Array ausgewertet. | 
| **value-title** | Optional | Eine Pfadzeichenfolge im Objekt innerhalb von `value-collection`, die auf die Beschreibung des Werts verweist. </br>Wenn `value-collection` nicht angegeben ist, wird die Antwort als Array ausgewertet. | 
| **value-collection** | Optional | Eine Pfadzeichenfolge, die als Array von Objekten in der Antwortnutzlast ausgewertet wird. | 
| **parameters** | Optional | Ein Objekt, dessen Eigenschaften die Eingabeparameter zum Aufrufen eines Vorgangs mit dynamischen Werten angeben. | 
|||| 

Hier ist ein Beispiel, das die Eigenschaften in `x-ms-dynamic-values` zeigt:

``` json
"x-ms-dynamic-values": {
  "operationId": "PopulateDropdown",
  "value-path": "name",
  "value-title": "properties/displayName",
  "value-collection": "value",
  "parameters": {
     "staticParameter": "<value>",
     "dynamicParameter": {
        "parameter": "<value-to-pass-to-dynamicParameter>"
     }
  }
}
```

## <a name="example-all-the-openapi-extensions-up-to-this-point"></a>Beispiel: Alle OpenAPI-Erweiterungen bis zu diesem Punkt

``` json
"/api/lists/{listID-dynamic}": {
    "get": {
        "description": "Get items from a single list - uses dynamic values and outputs dynamic schema",
        "summary": "Gets items from the selected list",
        "operationID": "GetListItems",
        "parameters": [
           {
             "name": "listID-dynamic",
             "type": "string",
             "in": "path",
             "description": "Select the list from where you want outputs",
             "required": true,
             "x-ms-summary": "Select List",
             "x-ms-dynamic-values": {
                "operationID": "GetLists",
                "value-path": "id",
                "value-title": "name"
             }
           }
        ]
    }
}
```

<a name="x-ms-dynamic schema"></a>

## <a name="x-ms-dynamic-schema"></a>x-ms-dynamic-schema

Gibt an, dass das Schema für den aktuellen Parameter oder die Antwort dynamisch ist. Dieses Objekt kann einen Vorgang aufrufen, der durch den Wert dieses Felds definiert ist, das Schema dynamisch ermittelt und die entsprechende Benutzeroberfläche anzeigt, um Benutzereingaben zu sammeln oder verfügbare Felder anzuzeigen. 

Gilt für: Parameter, Antwort

Vorgehensweise: Hinzufügen des `x-ms-dynamic-schema`-Objekts zur Definition eines Anforderungsparameters oder eines Antworttexts. Siehe beispielsweise dieses [OpenAPI-Beispiel](https://procsi.blob.core.windows.net/blog-images/sampleDynamicSwagger.json).

Dieses Beispiel zeigt, wie sich das Eingabeformular basierend auf dem Eintrag ändert, den der Benutzer in der Dropdownliste auswählt:

![„x-ms-dynamic-schema“ für dynamische Parameter](./media/custom-connector-openapi-extensions/x-ms-dynamic-schema.png)

Dieses Beispiel zeigt, wie sich die Ausgabe basierend auf dem Eintrag ändert, den der Benutzer in der Dropdownliste auswählt. In dieser Version wählt der Benutzer „Cars“ aus:

![„x-ms-dynamic-schema-response“ für das ausgewählte Element „Cars“](./media/custom-connector-openapi-extensions/x-ms-dynamic-schema-output1.png)

In dieser Version wählt der Benutzer „Food“ aus:

![„x-ms-dynamic-schema-response“ für das ausgewählte Element „Food“](./media/custom-connector-openapi-extensions/x-ms-dynamic-schema-output2.png)

### <a name="properties-for-x-ms-dynamic-schema"></a>Eigenschaften für „x-ms-dynamic-schema“

| Name | Erforderlich oder optional | Beschreibung | 
| ---- | -------------------- | ----------- | 
| **operationID** | Erforderlich | Der zum Abrufen des Schemas aufzurufende Vorgang | 
| **parameters** | Erforderlich | Ein Objekt, dessen Eigenschaften die Eingabeparameter zum Aufrufen eines Vorgangs mit dynamischem Schema angeben. | 
| **value-path** |Optional | Eine Pfadzeichenfolge, die auf die Eigenschaft verweist, die das Schema aufweist. </br>Falls nicht angegeben, wird davon ausgegangen, dass die Antwort das Schema in den Eigenschaften des Stammobjekts enthält. | 
|||| 

Hier ist ein Beispiel für einen dynamischen Parameter:

``` json
{
  "name": "dynamicListSchema",
  "in": "body",
  "description": "Dynamic schema for items in the selected list",
  "schema": {
    "type": "object",
    "x-ms-dynamic-schema": {
        "operationID": "GetListSchema",
        "parameters": {
          "listID": {
            "parameter": "listID-dynamic"
          }
        },
        "value-path": "items"
    }
  }
}
```

Hier ist ein Beispiel für eine dynamische Antwort:

``` json
"DynamicResponseGetListSchema": {
   "type": "object",
   "x-ms-dynamic-schema": {
      "operationID": "GetListSchema",
      "parameters": {
         "listID": {
            "parameter": "listID-dynamic"
         }
      },
      "value-path": "items"
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

* [Beschreiben Ihrer benutzerdefinierten APIs und Connectors](../logic-apps/custom-connector-api-postman-collection.md)
* [Logic Apps: Registrieren Ihres Connectors](../logic-apps/logic-apps-custom-connector-register.md)
* [Flow: Registrieren Ihres Connectors](https://ms.flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector)