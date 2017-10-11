---
title: Schema-1-Vorschau August 2015 - aktualisiert Azure-Logik-Apps | Microsoft Docs
description: "Erstellen von JSON-Definitionen für Azure-Logik-Apps mit Schema Version 2015-08-01-Vorschau"
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 0d03a4d4-e8a8-4c81-aed5-bfd2a28c7f0c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 05/31/2016
ms.author: LADocs; stepsic
ms.openlocfilehash: 35d7a56d5607dcc18a4407c65b92962d3d0dcd1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a>Schema-Updates für Azure-Logik-Apps - 1. August 2015-Vorschau

Diese neuen Schema und API-Version für Azure-Logik-Apps enthält wichtige Verbesserungen, die Logik-apps stellen, zuverlässiger und einfacher zu verwenden:

*   Die **APIApp** Aktionstyp wird aktualisiert, um ein neues [ **APIConnection** ](#api-connections) Aktionstyp.
*   **Wiederholen Sie die** umbenannt in [ **Foreach**](#foreach).
*   Die [ **HTTP-Listener** API-App](#http-listener) ist nicht mehr erforderlich.
*   Aufrufen des untergeordneten Workflows verwendet eine [neues Schema](#child-workflows).

<a name="api-connections"></a>
## <a name="move-to-api-connections"></a>Verschieben Sie in der API-Verbindungen

Die wichtigste Änderung besteht, dass Sie nicht mehr auf API-Apps in Ihrem Azure-Abonnement bereitstellen, damit Sie APIs verwenden können. Hier sind die Verwendungsmöglichkeiten Sie APIs verwenden können:

* Verwaltete APIs
* Ihre benutzerdefinierte Web-APIs

Jeder Methode ist etwas anders behandelt werden, da ihre Verwaltungs- und hosting Modelle unterscheiden. Ein Vorteil dieses Modells ist, dass Sie nicht mehr begrenzt sind, Ressourcen, die bereitgestellt werden in Ihrem Azure-Ressourcengruppe. 

### <a name="managed-apis"></a>Verwaltete APIs

Einige APIs in Ihrem Namen, z. B. Office 365, Salesforce, Twitter und FTP wird von Microsoft verwaltet. Sie können einige verwaltete APIs als-ist sein können, z. B. Bing übersetzen zu können, während andere Konfiguration erforderlich ist. Diese Konfiguration wird aufgerufen, eine *Verbindung*.

Wenn Sie Office 365 verwenden, müssen Sie z. B. eine Verbindung erstellen, die Ihr Office 365-Anmeldung Token enthält. Dieses Token sicher gespeichert und aktualisiert, sodass Ihre Logik-app immer die Office 365-APIs aufrufen kann. Wenn Sie mit Ihrem SQL oder FTP-Server verbinden möchten, müssen Sie auch eine Verbindung erstellen, die die Verbindungszeichenfolge ist. 

Diese Aktionen werden in dieser Definition aufgerufen `APIConnection`. Hier ist ein Beispiel für eine Verbindung, die Aufrufe von Office 365, um eine e-Mail zu senden:

```
{
    "actions": {
        "Send_Email": {
            "type": "ApiConnection",
            "inputs": {
                "host": {
                    "api": {
                        "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
                    },
                    "connection": {
                        "name": "@parameters('$connections')['shared_office365']['connectionId']"
                    }
                },
                "method": "post",
                "body": {
                    "Subject": "Reminder",
                    "Body": "Don't forget!",
                    "To": "me@contoso.com"
                },
                "path": "/Mail"
            }
        }
    }
}
```

Die `host` Objekt ist Teil von Eingaben, die nur für API-Verbindungen und wartungsplanlizenzen Teile enthält: `api` und `connection`.

Die `api` hat die Common Language Runtime, die URL der, in dem die API verwaltet gehostet wird. Sehen Sie die verfügbaren verwalteten APIs, die durch den Aufruf `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

Wenn Sie eine API verwenden, wird die API kann oder möglicherweise keine *Verbindungsparameter* definiert. Wenn die API nicht unterstützt, keine *Verbindung* ist erforderlich. Wenn die API der Fall ist, müssen Sie eine Verbindung erstellen. Die erstellte Verbindung hat den Namen, den Sie auswählen. Verweisen Sie dann den Namen in der `connection` Objekt innerhalb der `host` Objekt. Rufen Sie zum Erstellen einer Verbindungs in einer Ressourcengruppe:

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

Mit dem folgenden Text:

```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues": {
        "accountName": "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location": "{Logic app's location}"
}
```

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a>Bereitstellen von verwalteten APIs in einer Azure-Ressourcen-Manager-Vorlage

Als interaktive Anmeldung nicht erforderlich ist, können Sie eine vollständige Anwendung in einer Azure-Ressourcen-Manager-Vorlage erstellen.
Wenn Anmeldung erforderlich ist, Sie können alles, was mit der Azure Resource Manager-Vorlage einrichten, aber weiterhin bestehen, besuchen das Portal, um die Verbindungen zu autorisieren. 

```
    "resources": [{
        "apiVersion": "2015-08-01-preview",
        "name": "azureblob",
        "type": "Microsoft.Web/connections",
        "location": "[resourceGroup().location]",
        "properties": {
            "api": {
                "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
            },
            "parameterValues": {
                "accountName": "[parameters('storageAccountName')]",
                "accessKey": "[parameters('storageAccountKey')]"
            }
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": ["[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "queries": {
                                "folderPath": "[concat('/', parameters('containerName'))]",
                                "name": "helloworld.txt"
                            },
                            "body": "@decodeDataUri('data:, Hello+world!')",
                            "path": "/datasets/default/files"
                        },
                        "conditions": []
                    }
                },
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
                }
            },
            "parameters": {
                "$connections": {
                    "value": {
                        "azureblob": {
                            "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                            "connectionName": "azureblob",
                            "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                        }

                    }
                }
            }
        }
    }]
```

In diesem Beispiel sehen Sie, dass die Verbindungen nur Ressourcen sind, die in der Ressourcengruppe vorhanden. Verweisen auf die verwalteten APIs, die in Ihrem Abonnement zur Verfügung.

### <a name="your-custom-web-apis"></a>Ihre benutzerdefinierte Web-APIs

Wenn Sie eine eigene API, nicht von Microsoft verwaltet zu verwenden, verwenden Sie die integrierte **HTTP** Aktion aus, um die Methoden aufrufen. Für eine optimale Erfahrung sollten Sie einen Swagger-Endpunkt für Ihre API verfügbar machen. Dieser Endpunkt ermöglicht die Logik-App-Designer zum Rendern der Eingaben und Ausgaben für Ihre API. Ohne Swagger kann der Designer als nicht transparenter JSON-Objekte nur der Eingaben und Ausgaben anzeigen.

Hier ist ein Beispiel mit den neuen `metadata.apiDefinitionUrl` Eigenschaft:

```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata": {
              "apiDefinitionUrl": "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method": "GET"
            }
        }
    }
}
```

Wenn Sie Ihre Web-API in Azure App Service hosten, wird Ihre Web-API automatisch in der Liste der verfügbaren Aktionen im Designer angezeigt. Wenn dies nicht der Fall ist, müssen Sie direkt in der URL einfügen. Der Swagger-Endpunkt muss nicht authentifiziert werden, um in der Logik-App-Designer verwendet werden, obwohl Sie die API mit den Methoden sichern können, die Swagger unterstützt.

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a>Aufrufen von bereitgestellte API-apps mit 2015-08-01-Vorschau

Wenn Sie zuvor eine API-App bereitgestellt haben, können Sie die app mit Aufrufen der **HTTP** Aktion.

Bei Verwendung von Dropbox beim Auflisten der Dateien, beispielsweise Ihre **2014-12-01-Vorschau** Schemadefinition-Version möglicherweise etwa:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
                }
            }
        }
    },
    "actions": {
        "dropboxconnector": {
            "type": "ApiApp",
            "inputs": {
                "apiVersion": "2015-01-14",
                "host": {
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Während der Parameter-Abschnitt der Definition der Logik-app unverändert bleibt, können Sie die entsprechende HTTP-Aktion, wie in diesem Beispiel erstellen:

```
{
    "actions": {
        "dropboxconnector": {
            "type": "Http",
            "metadata": {
              "apiDefinitionUrl": "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
            },
            "inputs": {
                "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
                "method": "POST",
                "body": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Über diese Eigenschaften einzeln-durchlaufen:

| Action-Eigenschaft | Beschreibung |
| --- | --- |
| `type` |`Http`Statt`APIapp` |
| `metadata.apiDefinitionUrl` |Um diese Aktion in der Logik-App-Designer zu verwenden, schließen Sie die Metadaten-Endpunkt, der aus erstellt wird:`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` |Erstellt von:`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` |Immer`POST` |
| `inputs.body` |Identisch mit der API-App-Parameter |
| `inputs.authentication` |Identisch mit der API-App-Authentifizierung |

Dieser Ansatz sollte für alle API-App-Aktionen verwendet werden. Beachten Sie jedoch, dass diese frühere API-Apps nicht mehr unterstützt werden. Daher sollten Sie in den beiden anderen zuvor genannten Optionen, eine verwaltete API oder Hosten Ihrer benutzerdefinierten Web-API verschieben.

<a name="foreach"></a>
## <a name="renamed-repeat-to-foreach"></a>"Wiederholen" umbenannt in "Foreach"

Für die vorherige Version des Datenbankschemas, erhalten wir viel Kundenfeedback, **wiederholen** war verwirrend und wurde nicht ordnungsgemäß zu erfassen, **wiederholen** wurde tatsächlich eine für jede Schleife. Daher haben wir umbenannt `repeat` auf `foreach`. Beispielsweise würde zuvor Sie schreiben:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "repeat": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{repeatItem()}"
            }
        }
    }
}
```

Nun möchten Sie Folgendes schreiben:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "foreach": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{item()}"
            }
        }
    }
}
```

Die Funktion `@repeatItem()` wurde zuvor für das aktuelle Element durchlaufenen verweisen verwendet. Diese Funktion ist nun vereinfacht, sodass Sie `@item()`. 

### <a name="reference-outputs-from-foreach"></a>Referenz-Ausgaben aus "Foreach"

Zur Vereinfachung der Ausgaben von `foreach` Aktionen werden nicht in ein Objekt namens umschlossen `repeatItems`. Während die Ausgaben des vorherigen `repeat` Beispiel wäre:

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

Jetzt werden diese Ausgaben aus:

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

Zuvor in den Text der Aktion abgerufen werden soll, wenn Sie diese Ausgaben zu verweisen:

```
{
    "actions": {
        "secondAction": {
            "type": "Http",
            "repeat": "@outputs('pingBing').repeatItems",
            "inputs": {
                "method": "POST",
                "uri": "http://www.example.com",
                "body": "@repeatItem().outputs.body"
            }
        }
    }
}
```

Jetzt können Sie stattdessen folgendermaßen vor:

```
{
    "actions": {
        "secondAction": {
            "type": "Http",
            "foreach": "@outputs('pingBing')",
            "inputs": {
                "method": "POST",
                "uri": "http://www.example.com",
                "body": "@item().outputs.body"
            }
        }
    }
}
```

Mit diesen Änderungen, die Funktionen `@repeatItem()`, `@repeatBody()`, und `@repeatOutputs()` werden entfernt.

<a name="http-listener"></a>
## <a name="native-http-listener"></a>Systemeigene HTTP-listener

Die HTTP-Listener-Funktionen werden jetzt erstellt. Sie müssen also nicht mehr zum Bereitstellen einer HTTP-Listener-API-App. Finden Sie unter [die vollständigen Details zur Verwendung Ihrer Logik-app-Endpunkt hier aufrufbar zu machen](../logic-apps/logic-apps-http-endpoint.md). 

Mit diesen Änderungen wir entfernt die `@accessKeys()` -Funktion, die wir mit ersetzt die `@listCallbackURL()` -Funktion zum Abrufen des Endpunkts bei Bedarf. Darüber hinaus müssen Sie mindestens ein Trigger in der Logik-app jetzt definieren. Wenn Sie möchten `/run` den Workflow, benötigen Sie eine dieser Trigger: `manual`, `apiConnectionWebhook`, oder `httpWebhook`.

<a name="child-workflows"></a>
## <a name="call-child-workflows"></a>Aufrufen von untergeordneten workflows

Aufrufen des untergeordneten Workflows erforderlich zuvor, möchten den Workflow, das Zugriffstoken abrufen und Einfügen von Token in der Definition der Logik-app, in denen diese untergeordneten Workflows aufgerufen werden soll. Mit dem neuen Schema generiert das Logic Apps-Modul automatisch eine SAS, zur Laufzeit für den untergeordneten Workflow, daher ist es nicht vertrauliche Daten in der Definition einfügen. Hier ist ein Beispiel:

```
"mynestedwf": {
    "type": "workflow",
    "inputs": {
        "host": {
            "id": "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName": "myendpointtrigger"
        },
        "queries": {
            "extrafield": "specialValue"
        },
        "headers": {
            "x-ms-date": "@utcnow()",
            "Content-type": "application/json"
        },
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        }
    },
    "conditions": []
}
```

Eine zweite Verbesserung ist, dass wir die untergeordneten Workflows vollen Zugriff auf die eingehende Anforderung erteilen. Das bedeutet, dass Sie im Parameter übergeben können die *Abfragen* Abschnitt und in der *Header* -Objekt, sodass Sie vollständig auf den gesamten Nachrichtentext definieren können.

Schließlich sind Änderungen des untergeordneten Workflows erforderliche. Während Sie zuvor direkt untergeordneten Workflow aufrufen konnte, müssen Sie jetzt einen Trigger Endpunkt des Workflows für das übergeordnete Element aufrufen definieren. Im Allgemeinen fügen Sie einen Trigger, der hat `manual` geben, und klicken Sie dann, dass Trigger in der Definition des übergeordneten verwenden. Beachten Sie die `host` speziell Eigenschaft verfügt über eine `triggerName` , da deren Trigger immer angegeben werden muss, die Sie aufrufen.

## <a name="other-changes"></a>Sonstige Änderungen

### <a name="new-queries-property"></a>Neue "Abfragen"-Eigenschaft

Alle Aktionstypen unterstützen jetzt eine neue Eingabe aufgerufen `queries`. Diese Eingabe kann strukturiertes Objekt, anstatt dass müssen manuell die Zeichenfolge zusammenstellen sein.

### <a name="renamed-parse-function-to-json"></a>Umbenannte "parse()" Funktion "json()"

Wir sind hinzufügen Weitere-Inhaltstypen bald, damit es umbenannt der `parse()` -Funktion `json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>In Kürze: Enterprise Integration-APIs

Wir haben keine verwaltete Versionen noch Enterprise Integration-APIs wie AS2. In der Zwischenzeit können Sie Ihre vorhandenen bereitgestellten BizTalk-APIs über den HTTP-Aktion. Weitere Informationen finden Sie unter "Using Ihre bereits bereitgestellte API-apps" in der [Integrationsstrategie](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/). 
