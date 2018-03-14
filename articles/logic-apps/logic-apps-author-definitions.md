---
title: "Erstellen von Workflowdefinitionen für Logik-Apps per JSON-Code – Azure Logic Apps | Microsoft-Dokumentation"
description: "Hinzufügen von Parametern, Verarbeiten von Zeichenfolgen, Erstellen von Parameterzuordnungen und Abrufen von Daten mit Date-Funktionen"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 01/31/2018
ms.author: LADocs; estfan
ms.openlocfilehash: d05f7e34cbe670db6733c199e3420c810c304a84
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2018
---
# <a name="build-on-your-logic-app-definition-with-json"></a>Erstellen Ihrer Workflowdefinitionen für Logik-Apps per JSON-Code

Um erweiterte Aufgaben mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md) auszuführen, können Sie die Codeansicht zum Bearbeiten Ihrer Logik-App-Definition verwenden, die einfache, deklarative JSON-Sprache verwendet. Falls Sie dies noch nicht getan haben, sollten Sie sich zuerst darüber informieren, wie Sie [Ihre erste Logik-App erstellen](../logic-apps/quickstart-create-first-logic-app-workflow.md). Sehen Sie sich auch die [vollständige Referenz zur Definitionssprache für Workflows](http://aka.ms/logicappsdocs) an.

> [!NOTE]
> Einige Funktionen von Azure Logic Apps, z.B. Parameter, stehen nur zur Verfügung, wenn Sie in der Codeansicht für die Definition der Logik-App arbeiten. Parameter erleichtern Ihnen das Wiederverwenden von Werten in der gesamten Logik-App. Wenn Sie z.B. dieselbe E-Mail-Adresse in verschiedenen Aktionen verwenden möchten, definieren Sie diese E-Mail-Adresse als Parameter.

## <a name="view-and-edit-your-logic-app-definitions-in-json"></a>Anzeigen und Bearbeiten Ihrer Logik-App-Definition in JSON

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com "Azure-Portal") an.

2. Wählen Sie im Menü auf der linken Seite die Option **Weitere Dienste** aus. Wählen Sie unter **Enterprise Integration** die Option **Logik-Apps**. Speichern Sie Ihre Logik-App.

3. Wählen Sie im Logik-App-Menü unter **Entwicklungstools** die Option **Logik-App-Codeansicht** aus.

   Das Codeansichtsfenster wird geöffnet und zeigt die Definition der Logik-App an.

## <a name="parameters"></a>Parameter

Mit Parametern können Sie Werte in der gesamten Logik-App wiederverwenden, und sie eignen sich gut zum Ersetzen von Werten, die Sie vielleicht häufig ändern müssen. Wenn Sie z.B. eine E-Mail-Adresse haben, die Sie an mehreren Stellen verwenden möchten, sollten Sie die E-Mail-Adresse als Parameter definieren. 

Parameter sind auch nützlich, wenn Sie Parameter in verschiedenen Umgebungen überschreiben müssen. Hier erfahren Sie mehr über [Parameter für die Bereitstellung](#deployment-parameters) und die [REST-API für Azure Logic Apps](https://docs.microsoft.com/rest/api/logic).

> [!NOTE]
> Parameter sind nur in der Codeansicht verfügbar.

In der [ersten Beispiel-Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) haben Sie einen Workflow erstellt, der E-Mail-Nachrichten sendet, wenn im RSS-Feed einer Website neue Daten angezeigt werden. Die URL des Feeds ist hartcodiert, damit dieses Beispiel zeigt, wie Sie den Abfragewert mit einem Parameter ersetzen, damit Sie die URL des Feeds leichter ändern können.

1. Suchen Sie in der Codeansicht nach dem `parameters : {}`-Objekt, und fügen Sie ein `currentFeedUrl`-Objekt hinzu:

   ``` json
     "currentFeedUrl" : {
      "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
   }
   ```

2. Ersetzen Sie in der `When_a_feed-item_is_published`-Aktion im `queries`-Abschnitt den Abfragewert durch `"feedUrl": "#@{parameters('currentFeedUrl')}"`. 

   **Vorher**
   ``` json
   }
      "queries": {
          "feedUrl": "https://s.ch9.ms/Feeds/RSS"
       }
   },   
   ```

   **Nachher**
   ``` json
   }
      "queries": {
          "feedUrl": "#@{parameters('currentFeedUrl')}"
       }
   },   
   ```

   Sie können auch die `concat`-Funktion verwenden, um zwei oder mehr Zeichenfolgen zu verknüpfen. 
   Beispielsweise funktioniert `"@concat('#',parameters('currentFeedUrl'))"` genauso wie das vorherige Beispiel.

3.  Wenn Sie fertig sind, wählen Sie **Speichern** aus. 

Jetzt können Sie den RSS-Feed der Website ändern, indem Sie eine andere URL über das `currentFeedURL`-Objekt übergeben.

<a name="deployment-parameters"></a>

## <a name="deployment-parameters-for-different-environments"></a>Bereitstellungsparameter für unterschiedliche Umgebungen

In der Regel besitzen Bereitstellungslebenszyklen Umgebungen für Entwicklungs-, Staging- und Produktionsumgebungen. In all diesen Umgebungen können Sie beispielsweise die gleiche Logik-App-Definition, aber unterschiedliche Datenbanken verwenden. Ebenso können Sie die gleiche Definition übergreifend in verschiedenen Regionen verwenden, um Hochverfügbarkeit zu erzielen. Die einzelnen Logik-App-Instanzen sollten aber die Datenbank der betreffenden Region verwenden. 

> [!NOTE] 
> Dieses Szenario unterscheidet sich von der Verwendung von Parametern zur *Laufzeit*, wo Sie stattdessen die `trigger()`-Funktion verwenden sollten.

Hier ist eine grundlegende Definition:

``` json
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "request": {
          "type": "request",
          "kind": "http"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```
In der tatsächlichen `PUT`-Anforderung für die Logik-App können Sie den Parameter `uri` angeben. In den einzelnen Umgebungen können Sie einen anderen Wert für den `connection`-Parameter angeben. Da kein Standardwert mehr vorhanden ist, ist für die Nutzlast der Logik-App dieser Parameter erforderlich:

``` json
{
    "properties": {},
        "definition": {
          /// Use the definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

Weitere Informationen finden Sie in der [Dokumentation zur REST-API für Azure Logic Apps](https://docs.microsoft.com/rest/api/logic/).

## <a name="process-strings-with-functions"></a>Verarbeiten von Zeichenfolgen mit Funktionen

Logic Apps verfügt über verschiedene Funktionen zum Arbeiten mit Zeichenfolgen. Nehmen Sie z.B. an, dass Sie einen Firmennamen aus einem Auftrag einem anderen System übergeben möchten. Allerdings sind Sie sich nicht über die ordnungsgemäße Behandlung für die Zeichencodierung sicher. Sie könnten eine base64-Codierung für diese Zeichenfolge ausführen, aber um Escapezeichen in der URL zu vermeiden, können Sie stattdessen mehrere Zeichen ersetzen. Außerdem müssen Sie nur eine Teilzeichenfolge des Firmennamens verwenden, da die ersten fünf Zeichen nicht verwendet werden. 

``` json
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1",
        "companyName": "NAME=Contoso"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "request": {
      "type": "Request",
      "kind": "Http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').companyName,5,sub(length(parameters('order').companyName), 5) )),'+','-') ,'/' ,'_' )}"
      }
    }
  },
  "outputs": {}
}
```

In diesem Beispiel werden die Schritte beschrieben, in denen diese Zeichenfolge von innen nach außen verarbeitet wird:

``` 
"uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').companyName,5,sub(length(parameters('order').companyName), 5) )),'+','-') ,'/' ,'_' )}"
```

1. Rufen Sie die [`length()`](../logic-apps/logic-apps-workflow-definition-language.md) des Firmennamens ab, um die Gesamtzahl der Zeichen zu erhalten.

2. Um eine kürzere Zeichenfolge zu erhalten, subtrahieren Sie `5`.

3. Nun erhalten Sie [`substring()`](../logic-apps/logic-apps-workflow-definition-language.md). Beginnen Sie bei Index `5`, und fahren Sie mit dem Rest der Zeichenfolge fort.

4. Wandeln Sie diese Teilzeichenfolge in eine [`base64()`](../logic-apps/logic-apps-workflow-definition-language.md)-Zeichenfolge um.

5. Ersetzen Sie mit [`replace()`](../logic-apps/logic-apps-workflow-definition-language.md) alle `+`-Zeichen durch `-`-Zeichen.

6. Ersetzen Sie schließlich mit [`replace()`](../logic-apps/logic-apps-workflow-definition-language.md) alle `/`-Zeichen durch `_`-Zeichen.

## <a name="map-list-items-to-property-values-then-use-maps-as-parameters"></a>Zuordnen von Listenelementen zu Eigenschaftswerten und Verwenden von Zuordnungen als Parameter

Um basierend auf einem Eigenschaftswert unterschiedliche Ergebnisse zu erzielen, können Sie eine Zuordnung erstellen, die für jeden Eigenschaftswert nach der Übereinstimmung mit einem Ergebnis sucht, und dann diese Zuordnung als Parameter verwenden. 

Dieser Workflow definiert z.B. einige Kategorien als Parameter und eine Zuordnung, die für jede dieser Kategorien nach der Übereinstimmung mit einer bestimmten URL sucht. Der Workflow ruft zunächst eine Liste von Artikeln auf. Anschließend verwendet der Workflow die Zuordnung, um für jeden Artikel die URL zu finden, die mit der Kategorie übereinstimmt.

*   Mit der Funktion [`intersection()`](../logic-apps/logic-apps-workflow-definition-language.md) wird überprüft, ob die Kategorie einer bekannten definierten Kategorie entspricht.

*   Nach Eingang einer übereinstimmenden Kategorie ruft das Beispiel das Element mithilfe eckiger Klammern aus der Zuordnung ab: `parameters[...]`

``` json
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "specialCategories": {
      "defaultValue": [
        "science",
        "google",
        "microsoft",
        "robots",
        "NSA"
      ],
      "type": "Array"
    },
    "destinationMap": {
      "defaultValue": {
        "science": "http://www.nasa.gov",
        "microsoft": "https://www.microsoft.com/en-us/default.aspx",
        "google": "https://www.google.com",
        "robots": "https://en.wikipedia.org/wiki/Robot",
        "NSA": "https://www.nsa.gov/"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "Request",
      "kind": "http"
    }
  },
  "actions": {
    "getArticles": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
      }
    },
    "forEachArticle": {
      "type": "foreach",
      "foreach": "@body('getArticles').responseData.feed.entries",
      "actions": {
        "ifGreater": {
          "type": "if",
          "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)",
          "actions": {
            "getSpecialPage": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
              }
            }
          }
        }
      },
      "runAfter": {
        "getArticles": [
          "Succeeded"
        ]
      }
    }
  }
}
```

## <a name="get-data-with-date-functions"></a>Abrufen von Daten mit Date-Funktionen

Zum Abrufen von Daten aus einer Datenquelle, die nicht nativ *Trigger* unterstützt, können Sie stattdessen Date-Funktionen zum Arbeiten mit Uhrzeiten und Datumsangaben verwenden. Dieser Ausdruck ermittelt z.B., wie lange die Schritte dieses Workflows von innen nach außen dauern:

``` json
"expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
```

1. Extrahieren Sie von der `order`-Aktion aus die `startTime`. 
2. Rufen Sie die aktuelle Uhrzeit mit `utcNow()` ab.
3. Subtrahieren Sie eine Sekunde:

   [`addseconds(..., -1)`](../logic-apps/logic-apps-workflow-definition-language.md) 

   Sie können auch andere Zeiteinheiten verwenden, z.B. `minutes` oder `hours`. 

3. Jetzt können Sie diese beiden Werte vergleichen. 

   Wenn der erste Wert kleiner als der zweite Wert ist, bedeutet dies, dass mehr als eine Sekunde verstrichen ist, seit der Auftrag erteilt wurde.

Zum Formatieren von Datumsangaben können Sie Zeichenfolgenformatierer verwenden. Zum Abrufen von RFC1123 verwenden Sie beispielsweise [`utcnow('r')`](../logic-apps/logic-apps-workflow-definition-language.md). Erfahren Sie mehr über die [Formatierung von Datumsangaben](../logic-apps/logic-apps-workflow-definition-language.md).

``` json
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder-id"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{parameters('order').id}"
      }
    },
    "ifTimingWarning": {
      "type": "If",
      "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
      "actions": {
        "timingWarning": {
          "type": "Http",
          "inputs": {
            "method": "GET",
            "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
          }
        }
      },
      "runAfter": {
        "order": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```


## <a name="next-steps"></a>Nächste Schritte

* [Ausführen von Schritten basierend auf einer Bedingung (Bedingungsanweisungen)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Ausführen von Schritten basierend auf verschiedenen Werten (switch-Anweisungen)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Ausführen und Wiederholen von Schritten (Schleifen)](../logic-apps/logic-apps-control-flow-loops.md)
* [Ausführen oder Zusammenführen paralleler Schritte (Verzweigungen)](../logic-apps/logic-apps-control-flow-branches.md)
* [Ausführen von Schritten basierend auf gruppierten Aktionsstatus (Bereiche)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
* Erfahren Sie mehr über die [Definitionssprache für Workflows für Azure Logic Apps](../logic-apps/logic-apps-workflow-definition-language.md).
* Erfahren Sie mehr über die [Workflowaktionen und -trigger für Azure Logic Apps](../logic-apps/logic-apps-workflow-actions-triggers.md).