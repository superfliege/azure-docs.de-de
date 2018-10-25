---
title: Senden benutzerdefinierter Metriken für eine Azure-Ressource an den Azure Monitor-Metrikenspeicher mithilfe einer REST-API
description: Senden benutzerdefinierter Metriken für eine Azure-Ressource an den Azure Monitor-Metrikenspeicher mithilfe einer REST-API
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: d36697e6b5765ecf35ed9b3add45cff6c33823a5
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49958217"
---
# <a name="send-custom-metrics-for-an-azure-resource-to-the-azure-monitor-metric-store-using-a-rest-api"></a>Senden benutzerdefinierter Metriken für eine Azure-Ressource an den Azure Monitor-Metrikenspeicher mithilfe einer REST-API

In diesem Artikel erfahren Sie, wie Sie benutzerdefinierte Metriken für Azure-Ressourcen über eine REST-API an den Metrikenspeicher von Azure Monitor senden können.  Sobald sich die Metriken in Azure Monitor befinden, können Sie alle Schritte mit ihnen ausführen, die Sie auch mit Standardmetriken durchführen können, z. B. Diagrammerstellung, Benachrichtigung, Weiterleitung an andere externe Tools usw.  

>[!NOTE] 
>Die REST-API gestattet nur das Senden von benutzerdefinierten Metriken für Azure-Ressourcen. Sie können mit [Application Insights](../application-insights/app-insights-api-custom-events-metrics.md) Metriken für Ressourcen in verschiedenen Umgebungen oder lokal senden.    


## <a name="create-and-authorize-a-service-principal-to-emit-metrics"></a>Erstellen und Autorisieren eines Dienstprinzipals zur Ausgabe von Metriken 

Erstellen Sie einen Dienstprinzipal in Ihrem Azure Active Directory-Mandanten unter Verwendung der Anweisungen unter [Erstellen eines Dienstprinzipals](../active-directory/develop/howto-create-service-principal-portal.md). 

Beachten Sie Folgendes, während Sie diesen Prozess durchlaufen: 

- Sie können eine beliebige URL für die Anmelde-URL eingeben.  
- Erstellen Sie einen neuen geheimen Client für diese App.  
- Speichern Sie den Schlüssel und die Client-ID für die Verwendung in späteren Schritten.  

Erteilen Sie der App, die in Schritt 1 „Herausgeber von Überwachungsmetriken“ erstellt wurde, Berechtigungen für die Ressource, für die Sie Metriken ausgeben möchten. Wenn Sie planen, die App zu verwenden, um benutzerdefinierte Metriken für viele Ressourcen auszugeben, können Sie diese Berechtigungen auf der Ebene der Ressourcengruppe oder des Abonnements vergeben. 

## <a name="get-an-authorization-token"></a>Abrufen eines Autorisierungstokens
Öffnen Sie eine Eingabeaufforderung, und führen Sie den folgenden Befehl aus:
```shell
curl -X POST https://login.microsoftonline.com/<yourtenantid>/oauth2/token -F "grant_type=client_credentials" -F "client_id=<insert clientId from earlier step> " -F "client_secret=<insert client secret from earlier step>" -F "resource=https://monitoring.azure.com/"
```
Speichern Sie das Zugriffstoken von der Antwort.

![Access Token](./media/metrics-store-custom-rest-api/accesstoken.png)

## <a name="emit-the-metric-via-the-rest-api"></a>Ausgeben der Metrik über die REST-API 

1. Fügen Sie den folgende JSON-Code in eine Datei ein, und speichern Sie sie als „custommetric.json“ auf Ihrem lokalen Computer. Aktualisieren Sie den Zeitparameter in der JSON-Datei. 
    
    ```json
    { 
        "time": "2018-09-13T16:34:20”, 
        "data": { 
            "baseData": { 
                "metric": "QueueDepth", 
                "namespace": "QueueProcessing", 
                "dimNames": [ 
                  "QueueName", 
                  "MessageType" 
                ], 
                "series": [ 
                  { 
                    "dimValues": [ 
                      "ImagesToProcess", 
                      "JPEG" 
                    ], 
                    "min": 3, 
                    "max": 20, 
                    "sum": 28, 
                    "count": 3 
                  } 
                ] 
            } 
        } 
    } 
    ``` 

1. Geben Sie im Eingabeaufforderungsfenster die Metrikdaten ein. 
    - Azure Region: Muss mit der Bereitstellungsregion der Ressource übereinstimmen, für die Sie Metriken ausgeben. 
    - Ressourcen-ID: Ressourcen-ID der Azure-Ressource, mit der Sie die Metrik verfolgen.  
    - Zugriffstoken: Fügen Sie das zuvor erhaltene Token ein.

    ```Shell 
    curl -X POST curl -X POST https://<azureRegion>.monitoring.azure.com/<resourceId> /metrics -H "Content-Type: application/json" -H "Authorization: Bearer <AccessToken>" -d @custommetric.json 
    ```
1. Ändern Sie den Zeitstempel und die Werte in der JSON-Datei. 
1. Wiederholen Sie die beiden vorherigen Schritte einige Male, damit Sie Daten für mehrere Minuten erhalten.

## <a name="troubleshooting"></a>Problembehandlung 
Wenn Sie in einem Teil des Prozesses einen Fehler erhalten, sollten Sie Folgendes berücksichtigen:

1. Sie können keine Metriken für ein Abonnement oder eine Ressourcengruppe als Ihre Azure-Ressource ausgeben. 
1. Sie können keine Metrik in den Speicher einfügen, die älter als 20 Minuten ist. Der Metrikspeicher ist für die Benachrichtigung und Diagrammerstellung in Echtzeit optimiert. 
2. Die Anzahl der Dimensionsnamen sollte mit den Werten übereinstimmen und umgekehrt. Überprüfen Sie die Werte. 
2. Möglicherweise geben Sie Metriken für Regionen aus, die keine benutzerdefinierten Metriken unterstützen. Weitere Informationen finden Sie unter [unterstützte benutzerdefinierte metrische Bereiche (Vorschau)](metrics-custom-overview.md#supported-regions). 



## <a name="view-your-metrics"></a>Anzeigen Ihrer Metriken 

1. Anmelden beim Azure-Portal 

1. Klicken Sie im linken Menü auf **Überwachen**. 

1. Klicken Sie auf der Seite „Überwachen“ auf **Metriken**. 

   ![Access Token](./media/metrics-store-custom-rest-api/metrics.png) 

1. Ändern Sie den Aggregationszeitraum in **Letzte 30 Minuten**.  

1. Wählen Sie in der Dropdownliste *Ressource* die Ressource aus, für die Sie die Metrik ausgegeben haben.  

1. Wählen Sie in der Dropdownliste *Namespaces* die Option **QueueProcessing** aus. 

1. Wählen Sie in der Dropdownliste *Metriken* die Option **QueueDepth** aus.  

 
## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [benutzerdefinierte Metriken](metrics-custom-overview.md).