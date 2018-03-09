---
title: "Erstellen von Schleifen und Bereichen bzw. Auflösen von Datenbatches in Workflows – Azure Logic Apps | Microsoft-Dokumentation"
description: "Erstellen Sie Schleifen zum Durchlaufen von Daten, gruppieren Sie Aktionen in Bereiche, oder lösen Sie Datenbatches auf, um mehr Workflows in Azure Logic Apps zu starten."
services: logic-apps
documentationcenter: .net,nodejs,java
author: ecfan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2016
ms.author: estfan; LADocs
ms.openlocfilehash: 24a19ef7d93b4b0d006dfb0aed38f88d3d821d99
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2018
---
# <a name="logic-apps-loops-scopes-and-debatching"></a>Schleifen, Bereiche und Auflösen von Batches in Logik-Apps
  
Logic Apps bietet eine Reihe von Verfahren zum Arbeiten mit Arrays, Sammlungen, Batches und Schleifen innerhalb eines Workflows.
  
## <a name="foreach-loop-and-arrays"></a>ForEach-Schleife und -Arrays
  
Logic Apps erlaubt Schleifen über einen Satz von Daten und das Ausführen von Aktionen für jedes Element.  Eine Schleife für eine Sammlung kann mithilfe der Aktion `foreach` ausgeführt werden.  Im Designer können Sie eine foreach-Schleife hinzufügen.  Nachdem Sie das Array ausgewählt haben, das Sie durchlaufen möchten, können Sie mit dem Hinzufügen von Aktionen beginnen.  Sie können mehrere Aktionen für jede foreach-Schleife hinzufügen.  Innerhalb der Schleife können Sie dann angeben, was bei den einzelnen Werten des Arrays passieren soll.

  In diesem Beispiel wird für jede E-Mail-Adresse, die „microsoft.com“ enthält, eine E-Mail gesendet. Bei Verwendung der Codeansicht können Sie eine foreach-Schleife wie im folgenden Beispiel angeben:

``` json
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')"
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                },
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                },
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  Eine `foreach`-Aktion kann Arrays mit Tausenden von Entitäten durchlaufen.  Die Iterationen werden standardmäßig parallel ausgeführt.  Ausführliche Informationen zu Array- und Parallelitätslimits finden Sie unter [Logic Apps-Grenzwerte und -Konfiguration](logic-apps-limits-and-config.md).

### <a name="sequential-foreach-loops"></a>Sequenzielle ForEach-Schleifen

Zum Aktivieren der sequenziellen Ausführung einer ForEach-Schleife muss der Vorgang `Sequential` hinzugefügt werden.

``` json
"forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "operationOptions": "Sequential",
        "..."
}
```
  
## <a name="until-loop"></a>Until-Schleife
  
  Sie können eine Aktion oder eine Reihe von Aktionen ausführen, bis eine Bedingung erfüllt ist.  Das gängigste Szenario für die Verwendung einer until-Schleife ist das Aufrufen eines Endpunkts, bis die gewünschte Antwort zurückgegeben wird.  Sie können im Designer eine until-Schleife hinzufügen.  Sie können nach dem Hinzufügen von Aktionen innerhalb der Schleife die Beendigungsbedingung sowie die Schleifenlimits festlegen.
  
  In diesem Beispiel wird ein HTTP-Endpunkt aufgerufen, bis der Antworttext den Wert „Completed“ hat.  Der Vorgang wird abgeschlossen, wenn eine der folgenden Bedingungen erfüllt ist: 
  
  * die HTTP-Antwort den Status „Completed“ aufweist
  * Der Vorgang wurde eine Stunde lang ausgeführt.
  * sie 100-mal durchlaufen wurde.
  
  Bei Verwendung der Codeansicht können Sie eine until-Schleife wie im folgenden Beispiel angeben:
  
  ``` json
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed')",
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a>SplitOn und Auflösen von Batches

Mitunter empfängt ein Trigger ein Array von Elementen, die Sie aus dem Batch lösen möchten, um einen Workflow pro Element zu starten.  Dieses Auflösen eines Batchs kann über den Befehl `spliton` erreicht werden.  Wenn Ihr Trigger-Swagger eine Nutzlast angibt, bei der es sich um ein Array handelt, wird ein Element vom Typ `spliton` hinzugefügt. Der Befehl `spliton` startet eine Ausführung pro Element im Array.  SplitOn kann nur einem Trigger hinzugefügt werden, der manuell konfiguriert oder überschrieben werden kann. Sie können `spliton` nicht zusammen mit einer Implementierung des synchronisierten Antwortmusters verwenden.  Jeder aufgerufene Workflow, der neben `spliton` auch über eine `response`-Aktion verfügt, wird asynchron ausgeführt und sendet sofort eine `202 Accepted`-Antwort.  

  Dieses Beispiel empfängt ein Array von Elementen und führt eine zeilenweise Batchauflösung durch. SplitOn kann in der Codeansicht wie im folgenden Beispiel angegeben werden:

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequencey": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a>Bereiche

Es ist möglich, eine Reihe von Aktionen zusammen in einem Bereich zu gruppieren.  Bereiche sind besonders für die Implementierung der Ausnahmenbehandlung hilfreich.  Sie können im Designer einen neuen Bereich hinzufügen und in diesem mit dem Hinzufügen von Aktionen beginnen.  Sie können Bereiche in der Codeansicht wie im folgenden Beispiel definieren:


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```
