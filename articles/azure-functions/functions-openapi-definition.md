---
title: Erstellen einer OpenAPI-Definition für eine Funktion mithilfe von Azure API Management
description: Erstellen Sie eine OpenAPI-Definition, die anderen Apps und Diensten das Aufrufen Ihrer Funktion in Azure ermöglicht.
services: functions
keywords: OpenAPI, Swagger, Cloud-Apps, Clouddienste,
author: ggailey777
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.topic: tutorial
ms.date: 11/26/2018
ms.author: glenga
ms.reviewer: sunayv
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 3ad304bc8f038d4009352dae72d70079828c26ba
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65141569"
---
# <a name="create-an-openapi-definition-for-a-function-with-azure-api-management"></a>Erstellen einer OpenAPI-Definition für eine Funktion mithilfe von Azure API Management

REST-APIs werden häufig mithilfe einer OpenAPI-Definition beschrieben. Diese Definition enthält Informationen zu den in einer API verfügbaren Vorgängen sowie zur Strukturierung der Anforderungs- und Antwortdaten für die API.

In diesem Tutorial erstellen Sie eine Funktion, die ermittelt, ob eine Notfallreparatur einer Windturbine kosteneffizient ist. Anschließend erstellen Sie eine OpenAPI-Definition für die Funktions-App mithilfe von [Azure API Management](../api-management/api-management-key-concepts.md), damit die Funktion von anderen Apps und Diensten aufgerufen werden kann.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Funktion in Azure
> * Generieren einer OpenAPI-Definition mit Azure API Management
> * Testen der Definition durch Aufrufen der Funktion

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

Sie müssen über eine Funktionen-App verfügen, die die Ausführung Ihrer Funktionen in Azure hostet. Sie können mit einer Funktions-App Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung, Skalierung und Freigabe von Ressourcen.

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

## <a name="create-the-function"></a>Erstellen der Funktion

In diesem Tutorial wird eine per HTTP ausgelöste Funktion verwendet, die zwei Parameter akzeptiert:

* Die geschätzte Zeit für die Durchführung einer Turbinenreparatur in Stunden.
* Die Kapazität der Turbine in Kilowatt. 

Die Funktion berechnet dann die Kosten einer Reparatur und den Umsatzerlös, der in einem Zeitraum von 24 Stunden von der Turbine generiert werden könnte. Zum Erstellen der per HTTP ausgelösten Funktion im [Azure-Portal](https://portal.azure.com).

1. Erweitern Sie die Funktionen-App, und wählen Sie die Schaltfläche **+** neben **Functions** aus. Wählen Sie **Im Portal** > **Weiter** aus.

1. Wählen Sie **Weitere Vorlagen...** und dann **Vorlagen fertigstellen und anzeigen** aus.

1. Zum Auswählen des HTTP-Triggers geben Sie `TurbineRepair` als **Namen** der Funktion ein, wählen Sie `Function` als **[Authentifizierungsebene](functions-bindings-http-webhook.md#http-auth)** aus, und wählen Sie dann **Erstellen** aus.  

    ![Erstellen der HTTP-Funktion für OpenAPI](media/functions-openapi-definition/select-http-trigger-openapi.png)

1. Ersetzen Sie den Inhalt der C#-Skriptdatei „run.csx“ durch den folgenden Code, und wählen Sie dann **Speichern** aus:

    ```csharp
    #r "Newtonsoft.Json"
    
    using System.Net;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;
    
    const double revenuePerkW = 0.12;
    const double technicianCost = 250;
    const double turbineCost = 100;
    
    public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
    {
        // Get query strings if they exist
        int tempVal;
        int? hours = Int32.TryParse(req.Query["hours"], out tempVal) ? tempVal : (int?)null;
        int? capacity = Int32.TryParse(req.Query["capacity"], out tempVal) ? tempVal : (int?)null;
    
        // Get request body
        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic data = JsonConvert.DeserializeObject(requestBody);
    
        // Use request body if a query was not sent
        capacity = capacity ?? data?.capacity;
        hours = hours ?? data?.hours;
    
        // Return bad request if capacity or hours are not passed in
        if (capacity == null || hours == null){
            return new BadRequestObjectResult("Please pass capacity and hours on the query string or in the request body");
        }
        // Formulas to calculate revenue and cost
        double? revenueOpportunity = capacity * revenuePerkW * 24;  
        double? costToFix = (hours * technicianCost) +  turbineCost;
        string repairTurbine;
    
        if (revenueOpportunity > costToFix){
            repairTurbine = "Yes";
        }
        else {
            repairTurbine = "No";
        };
    
        return (ActionResult)new OkObjectResult(new{
            message = repairTurbine,
            revenueOpportunity = "$"+ revenueOpportunity,
            costToFix = "$"+ costToFix
        });
    }
    ```

    Dieser Funktionscode gibt Folgendes zurück: eine Meldung `Yes` oder `No`, um anzugeben, ob eine Notfallreparatur kosteneffizient ist, den möglichen Umsatzerlös der Turbine und die Kosten für die Reparatur der Turbine.

1. Klicken Sie zum Testen der Funktion ganz rechts auf **Testen**, um die Registerkarte „Testen“ zu erweitern. Geben Sie den folgenden Wert für **Anforderungstext** ein, und klicken Sie dann auf **Ausführen**.

    ```json
    {
    "hours": "6",
    "capacity": "2500"
    }
    ```

    ![Testen der Funktion im Azure-Portal](media/functions-openapi-definition/test-function.png)

    Im Text der Antwort wird der folgende Wert zurückgegeben.

    ```json
    {"message":"Yes","revenueOpportunity":"$7200","costToFix":"$1600"}
    ```

Sie haben jetzt eine Funktion, die die Kosteneffizienz von Notfallreparaturen ermittelt. Als Nächstes generieren Sie eine OpenAPI-Definition für die Funktions-App.

## <a name="generate-the-openapi-definition"></a>Generieren der OpenAPI-Definition

Jetzt können Sie die OpenAPI-Definition generieren.

1. Wählen Sie die Funktions-App und dann **Plattformfeatures**, **Alle Einstellungen** aus.

    ![Testen der Funktion im Azure-Portal](media/functions-openapi-definition/select-all-settings-openapi.png)

1. Scrollen Sie nach unten, und wählen Sie dann **API Management** > **Neu erstellen** aus, um eine neue API Management-Instanz zu erstellen.

    ![Funktion verknüpfen](media/functions-openapi-definition/link-apim-openapi.png)

1. Verwenden Sie die API Management-Einstellungen, die in der Tabelle unter der Abbildung angegeben sind.

    ![Erstellen eines neuen API Management-Diensts](media/functions-openapi-definition/new-apim-service-openapi.png)

    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name** | Global eindeutiger Name | Es wird ein Name auf der Grundlage des Namens Ihrer Funktions-App generiert. |
    | **Abonnement** | Ihr Abonnement | Das Abonnement, unter dem diese neue Ressource erstellt wird. |  
    | **[Ressourcengruppe](../azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Dieselbe Ressource wie Ihre Funktions-App, die für Sie festgelegt werden sollte. |
    | **Location** | USA (Westen) | Wählen Sie den Standort USA, Westen aus. |
    | **Name der Organisation** | Contoso | Der Name der Organisation, der im Entwicklerportal und für E-Mail-Benachrichtigungen verwendet wird. |
    | **Administrator-E-Mail** | Ihre E-Mail-Adresse | Die E-Mail-Adresse, die Benachrichtigungen des Systems von API Management erhält. |
    | **Preisstufe** | Verbrauch (Vorschau) | Vollständige Preisinformationen finden Sie auf der [API Management-Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/api-management/) |
    | **Application Insights** | Ihre Instanz | Verwenden Sie die gleichen Application Insights, die von Ihrer Funktions-App verwendet wird. |

1. Wählen Sie **Erstellen** aus, um die API Management-Instanz zu erstellen, was ein paar Minuten dauern kann.

1. Wählen Sie **Application Insights aktivieren** aus, um Protokolle an den gleichen Ort wie die Funktionsanwendung zu senden, übernehmen Sie dann die verbleibenden Standardeinstellungen, und wählen Sie **API verknüpfen** aus.

1. **Azure-Funktionen importieren** wird mit hervorgehobener Funktion **Turbinenreparatur** geöffnet. Wählen Sie **Auswählen** aus, um fortzufahren.

    ![Importieren von Azure-Funktionen in API Management](media/functions-openapi-definition/import-function-openapi.png)

1. Übernehmen Sie auf der Seite **Aus Funktions-App erstellen** die Standardeinstellungen, und wählen Sie **Erstellen** aus.

    ![Aus Funktions-App erstellen](media/functions-openapi-definition/create-function-openapi.png)

Jetzt wird die API für die Funktion erstellt.

## <a name="test-the-openapi-definition"></a>Testen der OpenAPI-Definition

Bevor Sie die API-Definition verwenden, sollten Sie überprüfen, ob sie funktioniert.

1. Wählen Sie auf der Registerkarte **Test** Ihrer Funktion den **POST**-Vorgang aus.

1. Geben Sie Werte für **Stunden** und **Kapazität** ein.

```json
{
"hours": "6",
"capacity": "2500"
}
```

1. Klicken Sie auf **Senden**, und sehen Sie sich die HTTP-Antwort an.

    ![Testen der Funktions-API](media/functions-openapi-definition/test-function-api-openapi.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer Funktion in Azure
> * Generieren einer OpenAPI-Definition mit Azure API Management
> * Testen der Definition durch Aufrufen der Funktion

Fahren Sie mit dem nächsten Thema fort, um mehr über API Management zu erfahren.

> [!div class="nextstepaction"]
> [API Management](../api-management/api-management-key-concepts.md)
