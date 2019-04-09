---
title: SaaS Fulfillment API V2 – Azure Marketplace | Microsoft-Dokumentation
description: Erläutert, wie Sie ein SaaS-Angebot im Azure Marketplace mit den zugehörigen Fulfillment-APIs der Version 2 erstellen.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 02/27/2019
ms.author: pbutlerm
ms.openlocfilehash: 81213d1f7cfeb7ea10cdadfb124047ecb76aa7d4
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2019
ms.locfileid: "58352084"
---
# <a name="saas-fulfillment-api"></a>SaaS-Fulfillment-API

In diesem Artikel wird ausführlich die API behandelt, mit der unabhängige Softwarehersteller (ISVs) ihre SaaS-Anwendungen in Azure Marketplace integrieren können. Mit dieser API können ISVs für ihre Anwendungen alle Handelskanäle nutzen: direkt, über Partner (Reseller) und über den Vertrieb.  Diese API ist eine Voraussetzung für die Auflistung transaktionsfähiger SaaS-Angebote im Azure Marketplace.


## <a name="managing-the-saas-subscription-lifecycle"></a>Verwalten des Lebenszyklus von SaaS-Abonnements

Der SaaS-Dienst von Microsoft verwaltet den gesamten Lebenszyklus ab dem Erwerb für ein SaaS-Abonnement und nutzt die Fulfillment-API als Mechanismus, um die eigentliche Erfüllung, Plan- bzw. Tarifänderungen und das Löschen des Abonnements beim ISV zu steuern. Die Abrechnung basiert auf dem Status des SaaS-Abonnements, das Microsoft verwaltet. Das folgende Diagramm zeigt die Zustände und die Vorgänge, durch die Änderungen des jeweiligen Status gesteuert werden.

![Zustände im Lebenszyklus von SaaS-Abonnements](./media/saas-subscription-lifecycle-api-v2.png)


### <a name="states-of-a-saas-subscription"></a>Zustände eines SaaS-Abonnements

Die folgende Tabelle enthält die Bereitstellungszustände für ein SaaS-Abonnement, einschließlich einer Beschreibung und eines Sequenzdiagramms für den jeweiligen Status (sofern zutreffend). 

#### <a name="provisioning"></a>Bereitstellung

Wenn ein Kunde einen Kauf initiiert, empfängt der ISV diese Informationen in einem AuthCode auf einer für Kunden interaktiven Website mithilfe eines URL-Parameters. Der AuthCode kann überprüft und ausgetauscht werden, um die erforderlichen Details für die Bereitstellung zu erhalten.  Wenn der SaaS-Dienst die Bereitstellung abschließt, sendet er einen Aktivierungsaufruf, um zu signalisieren, dass die Erfüllung abgeschlossen ist und dem Kunden eine Rechnung gestellt werden kann.  Das folgende Diagramm zeigt die Abfolge der API-Aufrufe für ein Bereitstellungsszenario.  

![API-Aufrufe zum Bereitstellen eines SaaS-Diensts.](./media/saas-post-provisioning-api-v2-calls.png)

#### <a name="provisioned"></a>Bereitgestellt

Dieser Status ist der stabile Zustand eines bereitgestellten Diensts.

#### <a name="provisioning-for-update"></a>Bereitstellung für Aktualisierung
(von Marketplace) 

Dieser Status gibt an, dass eine Aktualisierung für einen vorhandenen Dienst ansteht. Eine derartige Aktualisierung kann durch den Kunden entweder im Marketplace oder über den SaaS-Dienst (nur bei „direkt“ für Kundentransaktionen) initiiert werden. Das folgende Diagramm zeigt die Aktionen für eine Aktualisierung, wenn diese im Marketplace initiiert wird.

![API-Aufrufe, wenn die Aktualisierung im Marketplace initiiert wird.](./media/saas-update-api-v2-calls-from-marketplace-a.png)

#### <a name="provisioning-for-update"></a>Bereitstellung für Aktualisierung  
(von SaaS-Dienst) 

Das folgende Diagramm zeigt die Aktionen für eine Aktualisierung, wenn diese von einem SaaS-Dienst initiiert wird. Der Webhookaufruf wird durch eine durch den SaaS-Dienst initiierte Aktualisierung des Abonnements ersetzt. 

![API-Aufrufe, wenn die Aktualisierung vom SaaS-Dienst initiiert wird.](./media/saas-update-api-v2-calls-from-saas-service-a.png) 

#### <a name="suspended"></a>Ausgesetzt

Dieser Status gibt an, dass die Zahlung des Kunden noch nicht empfangen wurde. Gemäß der Richtlinie gewähren wird dem Kunden eine Karenzzeit, bevor das Abonnement nicht erfüllt wird. Wenn sich ein Abonnement in diesem Zustand befindet, gibt es folgende Möglichkeiten: 

- Als unabhängiger Softwarehersteller können Sie den Zugriff des Benutzers auf den Dienst herabstufen oder sperren. 
- Das Abonnement muss in einem wiederherstellbaren Zustand gespeichert werden, damit die vollständige Funktionalität ohne Verlust von Daten oder Einstellungen wiederhergestellt werden kann. 
- Sie müssen über die Fulfillment-API eine Reaktivierungsanforderung für dieses Abonnement oder, am Ende der Karenzzeit, eine Anforderung zum Aufheben der Bereitstellung erhalten. 

#### <a name="unsubscribed"></a>Abonnement gekündigt 

Abonnements erreichen diesen Status infolge einer expliziten Kundenanforderung oder bei Nichtbezahlung der fälligen Beträge. Der ISV erwartet, dass die Kundendaten für eine Wiederherstellung auf Anforderung für mindestens x Tage gespeichert und dann gelöscht werden. 

## <a name="api-reference"></a>API-Referenz

In diesem Abschnitt werden die SaaS-*Abonnement-API* und die -*Vorgangs-API* dokumentiert.


### <a name="subscription-api"></a>Abonnement-API

Die Abonnement-API unterstützt die folgenden HTTPS-Vorgänge: **GET**, **POST**, **PATCH** und **DELETE**.

#### <a name="list-subscriptions"></a>Auflisten von Abonnements

Listet alle SaaS-Abonnements für einen Herausgeber auf.

**GET:<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=<ApiVersion>`**

*Abfrageparameter:*

|             |                   |
|  --------   |  ---------------  |
| ApiVersion  |  Die Version des für diese Anforderung zu verwendenden Vorgangs.  |

*Anforderungsheader:*

|                    |                   |
|  ---------------   |  ---------------  |
| Content-Typ       |  `application/json`  |
| x-ms-requestid     |  Eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
| x-ms-correlationid |  Eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
| authorization      |  JWT-Bearertoken (JSON Web Token)  |

*Antwortcodes:*

Code: 200<br>
Anhand des Authentifizierungstokens werden der Herausgeber und die entsprechenden Abonnements für alle Angebote des Herausgebers abgerufen.<br> Antwortnutzlast:<br>

```json
{
  "subscriptions": [
      {
          "id": "",
          "name": "CloudEndure for Production use",
          "publisherId": "cloudendure",
          "offerId": "ce-dr-tier2",
          "planId": "silver",
          "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "cc906b16-1991-4b6d-a5a4-34c66a5202d7"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "0396833b-87bf-4f31-b81c-c67f88973512"
          },
          "allowedCustomerOperations": [
              "Read" // Possible Values: Read, Update, Delete.
          ], // Indicates operations allowed on the SaaS subscription. For CSP initiated purchases, this will always be Read.
          "sessionMode": "None", // Possible Values: None, DryRun (Dry Run indicates all transactions run as Test-Mode in the commerce stack)
          "status": "Subscribed" // Indicates the status of the operation. [Provisioning, Subscribed, Suspended, Unsubscribed]
      }
  ],
  "continuationToken": ""
}
```

Code: 403 <br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt, ist ungültig, oder die Anforderung versucht, auf einen Kauf zugreifen, der nicht zum aktuellen Benutzer gehört. 

Code: 500 Interner Serverfehler

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="get-subscription"></a>Abrufen eines Abonnements

Ruft das angegebene SaaS-Abonnement ab. Verwenden Sie diesen Aufruf zum Abrufen von Lizenzinformationen und Plan-/Tarifinformationen.

**GET:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId?api-version=<ApiVersion>`**

*Abfrageparameter:*

|                    |                   |
|  ---------------   |  ---------------  |
| subscriptionId     |   Eindeutige ID des SaaS-Abonnements, die nach dem Auflösen des Tokens über die Auflösungs-API abgerufen wird.   |
|  ApiVersion        |   Version des für diese Anforderung zu verwendenden Vorgangs.   |

*Anforderungsheader:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Typ      |  `application/json`  |
|  x-ms-requestid    |  Eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  x-ms-correlationid |  Eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  authorization     |  JWT-Bearertoken (JSON Web Token)  |

*Antwortcodes:*

Code: 200<br>
Ruft Saas-Abonnement von ID ab<br> Antwortnutzlast:<br>

```json
Response Body:
{ 
        "id":"",
        "name":"CloudEndure for Production use",
        "publisherId": "cloudendure",
        "offerId": "ce-dr-tier2",
        "planId": "silver",
        "quantity": "10"",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "cc906b16-1991-4b6d-a5a4-34c66a5202d7"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "0396833b-87bf-4f31-b81c-c67f88973512"
          },
        "allowedCustomerOperations": ["Read"], // Indicates operations allowed on the SaaS subscription. For CSP initiated purchases, this will always be Read.
        "sessionMode": "None", // Dry Run indicates all transactions run as Test-Mode in the commerce stack
        "status": "Subscribed", // Indicates the status of the operation.
}
```

Code: 404<br>
Nicht gefunden<br> 

Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt, ist ungültig, oder die Anforderung versucht, auf einen Kauf zugreifen, der nicht zum aktuellen Benutzer gehört.

Code: 500<br>
Interner Serverfehler<br>

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }  
```

#### <a name="list-available-plans"></a>Auflisten verfügbarer Pläne

Verwenden Sie den Aufruf „listAvailablePlans“, um herauszufinden, ob es private/öffentliche Angebote für den aktuellen Benutzer gibt.

**GET:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/listAvailablePlans?api-version=<ApiVersion>`**

*Abfrageparameter:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |   Version des für diese Anforderung zu verwendenden Vorgangs.  |

*Anforderungsheader:*

|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Typ     |  `application/json` |
|   x-ms-requestid   |   Eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  x-ms-correlationid  | Eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  authorization     |  JWT-Bearertoken (JSON Web Token) |

*Antwortcodes:*

Code: 200<br>
Abrufen einer Liste der verfügbaren Pläne für einen Kunden.<br>

```json
Response Body:
[{
    "planId": "silver",
    "displayName": "Silver",
    "isPrivate": false
},
{
    "planId": "silver-private",
    "displayName": "Silver-private",
    "isPrivate": true
}]
```

Code: 404<br>
Nicht gefunden<br> 

Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt, ist ungültig, oder die Anforderung versucht, auf einen Kauf zugreifen, der nicht zum aktuellen Benutzer gehört. <br> 

Code: 500<br>
Interner Serverfehler<br>

```json
{ 
    "error": { 
      "code": "UnexpectedError", 
      "message": "An unexpected error has occurred." 
    } 
```

#### <a name="resolve-a-subscription"></a>Auflösen eines Abonnements 

Der Auflösungsendpunkt ermöglicht Benutzern das Auflösen eines Marketplace-Tokens in eine permanente Ressourcen-ID. Die Ressourcen-ID ist der eindeutige Bezeichner des SAAS-Abonnements.  Wenn ein Benutzer zur Website eines ISV weitergeleitet wird, enthält die URL in den Abfrageparametern ein Token. Es wird erwartet, dass der ISV dieses Token verwendet und eine Anforderung zu dessen Auflösung stellt. Die Antwort enthält die eindeutige ID des SAAS-Abonnements, den Namen, die Angebots-ID und den Plan für die Ressource. Dieses Token ist nur eine Stunde gültig. 

**POST:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=<ApiVersion>`**

*Abfrageparameter:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Version des für diese Anforderung zu verwendenden Vorgangs.  |

*Anforderungsheader:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Typ      | `application/json` |
|  x-ms-requestid    |  Eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  x-ms-correlationid |  Eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  authorization     |  JWT-Bearertoken (JSON Web Token)  |
|  x-ms-marketplace-token  |  Tokenabfrageparameter in der URL, wenn der Benutzer von Azure zur Website des SaaS-ISV umgeleitet wird. *Hinweis:* Führen Sie eine URL-Decodierung des Tokenwerts aus dem Browser durch, bevor Sie ihn verwenden. |

*Antwortcodes:*

Code: 200<br>
Löst das nicht transparente Token in ein SaaS-Abonnement auf.<br>

```json
Response body:
{
    "subscriptionId": "cd9c6a3a-7576-49f2-b27e-1e5136e57f45",  
    "subscriptionName": "My Saas application",
    "offerId": "ce-dr-tier2",
    "planId": "silver",
    "quantity": "20",
    "operationId": " be750acb-00aa-4a02-86bc-476cbe66d7fa"  
}
```

Code: 404<br>
Nicht gefunden

Code: 400<br>
Ungültige Anforderung – Fehler bei der Überprüfung

Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt, ist ungültig, oder die Anforderung versucht, auf einen Kauf zugreifen, der nicht zum aktuellen Benutzer gehört.

Code: 500<br>
Interner Serverfehler

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="activate-a-subscription"></a>Aktivieren eines Abonnements

**POST:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/activate?api-version=<ApiVersion>`**

*Abfrageparameter:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Version des für diese Anforderung zu verwendenden Vorgangs.  |
| subscriptionId     | Eindeutige ID des SaaS-Abonnements, die nach dem Auflösen des Tokens über die Auflösungs-API abgerufen wird.  |

*Anforderungsheader:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Typ      | `application/json`  |
|  x-ms-requestid    | Eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  x-ms-correlationid  | Eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser korreliert alle Ereignisse des Clientvorgangs mit Ereignissen auf der Serverseite. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  authorization     |  JWT-Bearertoken (JSON Web Token) |

*Anforderung:*

```json
{
    "planId": "gold",
    "quantity": ""
}
```

*Antwortcodes:*

Code: 202<br>
Aktiviert das Abonnement.<br>

Code: 404<br>
Nicht gefunden

Code: 400<br>
Ungültige Anforderung – Fehler bei der Überprüfung

Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt, ist ungültig, oder die Anforderung versucht, auf einen Kauf zugreifen, der nicht zum aktuellen Benutzer gehört.

Code: 500<br>
Interner Serverfehler

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="update-a-subscription"></a>Aktualisieren eines Abonnements

Aktualisieren oder ändern Sie einen Abonnementplan mit den bereitgestellten Werten.

**PATCH:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`**

*Abfrageparameter:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Die Version des für diese Anforderung zu verwendenden Vorgangs.  |
| subscriptionId     | Eindeutige ID des SaaS-Abonnements, die nach dem Auflösen des Tokens über die Auflösungs-API abgerufen wird.  |

*Anforderungsheader:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Typ      | `application/json` |
|  x-ms-requestid    |   Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  x-ms-correlationid  |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.    |
| authorization      |  JWT-Bearertoken (JSON Web Token)  |

*Anforderungsnutzlast:*

```json
Request Body:
{
    "planId": "gold",
    "quantity": ""
}
```

*Anforderungsheader:*

|                    |                   |
|  ---------------   |  ---------------  |
| Operation-Location | Link zu einer Ressource, um den Vorgangsstatus abzurufen.   |

*Antwortcodes:*

Code: 202<br>
ISV initiiert einen Änderungsplan oder eine Mengenänderung. <br>

Code: 404<br>
Nicht gefunden

Code: 400<br>
Ungültige Anforderung – Fehler bei der Überprüfung.

>[!Note]
>Es kann jeweils nur ein Plan oder eine Menge geändert werden, nicht beides gleichzeitig. Es wird ein Abonnement bearbeitet, bei dem **Aktualisieren** nicht zu den `allowedCustomerOperations` gehört.

Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt, ist ungültig, oder die Anforderung versucht, auf einen Kauf zugreifen, der nicht zum aktuellen Benutzer gehört.

Code: 500<br>
Interner Serverfehler

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="delete-a-subscription"></a>Löschen eines Abonnements

Kündigen und löschen Sie das angegebene Abonnement.

**DELETE:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId> ?api-version=<ApiVersion>`**

*Abfrageparameter:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Die Version des für diese Anforderung zu verwendenden Vorgangs.  |
| subscriptionId     | Eindeutige ID des SaaS-Abonnements, die nach dem Auflösen des Tokens über die Auflösungs-API abgerufen wird.  |

*Anforderungsheader:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Typ     |  `application/json` |
|  x-ms-requestid    |   Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.   |
|  x-ms-correlationid  |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.   |
|  authorization     |  JWT-Bearertoken (JSON Web Token)   |

*Antwortcodes:*

Code: 200<br>
Vom ISV initiierter Aufruf, um die Kündigung eines SaaS-Abonnements anzugeben.<br>

Code: 404<br>
Nicht gefunden

Code: 400<br>
Es wird ein Abonnement gelöscht, bei dem **DELETE** nicht zu den `allowedCustomerOperations` gehört.

Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt, ist ungültig, oder die Anforderung versucht, auf einen Kauf zugreifen, der nicht zum aktuellen Benutzer gehört.

Code: 500<br>
Interner Serverfehler

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```


### <a name="operations-api"></a>Vorgangs-API

Die Vorgangs-API unterstützt die folgenden PATCH- und GET-Vorgänge.


#### <a name="update-a-subscription"></a>Aktualisieren eines Abonnements

Aktualisieren eines Abonnements mit den bereitgestellten Werten.

**PATCH:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operation/<operationId>?api-version=<ApiVersion>`**

*Abfrageparameter:*

|                    |                   |
|  ---------------   |  ---------------  |
|   ApiVersion       |  Die Version des für diese Anforderung zu verwendenden Vorgangs.  |
| subscriptionId     | Eindeutige ID des SaaS-Abonnements, die nach dem Auflösen des Tokens über die Auflösungs-API abgerufen wird.  |
|  operationId       | Der Vorgang, der abgeschlossen wird. |

*Anforderungsheader:*

|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Typ     | `application/json`   |
|   x-ms-requestid   |   Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  x-ms-correlationid |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  authorization     |  JWT-Bearertoken (JSON Web Token)  |

*Anforderungsnutzlast:*

```json
{
    "planId": "",
    "quantity": "",
    "status": "Success"    // Allowed Values: Success/Failure. Indicates the status of the operation.
}
```

*Antwortcodes:*

Code: 200<br> Aufruf, um über den ISV-seitigen Abschluss eines Vorgangs zu informieren. Dies kann z. B. eine Änderung der Lizenzen/Pläne sein.

Code: 404<br>
Nicht gefunden

Code: 400<br>
Ungültige Anforderung – Fehler bei der Überprüfung

Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt, ist ungültig, oder die Anforderung versucht, auf einen Kauf zugreifen, der nicht zum aktuellen Benutzer gehört.

Code: 409<br>
Konflikt. Beispiel: Eine neuere Transaktion wurde bereits erfüllt.

Code: 500<br> Interner Serverfehler

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

#### <a name="list-outstanding-operations"></a>Auflisten ausstehender Vorgänge 

Listet die ausstehenden Vorgänge für den aktuellen Benutzer auf. 

**GET:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations?api-version=<ApiVersion>`**

*Abfrageparameter:*

|             |        |
|  ---------------   |  ---------------  |
|    ApiVersion                |   Die Version des für diese Anforderung zu verwendenden Vorgangs.                |
| subscriptionId     | Eindeutige ID des SaaS-Abonnements, die nach dem Auflösen des Tokens über die Auflösungs-API abgerufen wird.  |

*Anforderungsheader:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Typ     |  `application/json` |
|  x-ms-requestid    |  Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  x-ms-correlationid |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  authorization     |  JWT-Bearertoken (JSON Web Token)  |

*Antwortcodes:*

Code: 200<br> Ruft die Liste der ausstehenden Vorgänge für ein Abonnement ab.<br>
Antwortnutzlast:

```json
[{
    "id": "be750acb-00aa-4a02-86bc-476cbe66d7fa",  
    "activityId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "subscriptionId": "cd9c6a3a-7576-49f2-b27e-1e5136e57f45",
    "offerId": "ce-dr-tier2",
    "publisherId": "cloudendure",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",  
    "status": "NotStarted"  
}]
```

Code: 404<br>
Nicht gefunden

Code: 400<br>
Ungültige Anforderung – Fehler bei der Überprüfung

Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt, ist ungültig, oder die Anforderung versucht, auf einen Kauf zugreifen, der nicht zum aktuellen Benutzer gehört.

Code: 500<br>
Interner Serverfehler

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

#### <a name="get-operation-status"></a>Abrufen des Vorgangsstatus

Ermöglicht dem Benutzer das Nachverfolgen des Status eines ausgelösten asynchronen Vorgangs (Abonnieren/Kündigen/Plan ändern).

**GET:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`**

*Abfrageparameter:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Die Version des für diese Anforderung zu verwendenden Vorgangs.  |

*Anforderungsheader:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Typ      |  ` application/json`   |
|  x-ms-requestid    |   Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  x-ms-correlationid |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  authorization     | JWT-Bearertoken (JSON Web Token)  |

*Antwortcodes:* Code: 200<br> Ruft die Liste aller ausstehenden SaaS-Vorgänge ab.<br>
Antwortnutzlast:

```json
Response body:
[{
    "id  ": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "activityId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "subscriptionId":"cd9c6a3a-7576-49f2-b27e-1e5136e57f45",
    "offerId": "ce-dr-tier2",
    "publisherId": "cloudendure",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",
    "status": "NotStarted"
}]

```

Code: 404<br>
Nicht gefunden

Code: 400<br>
Ungültige Anforderung – Fehler bei der Überprüfung

Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt, ist ungültig, oder die Anforderung versucht, auf einen Kauf zugreifen, der nicht zum aktuellen Benutzer gehört.
 
Code: 500<br> Interner Serverfehler

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

## <a name="webhook-on-the-saas-service"></a>Webhook für den SaaS-Dienst

Der Herausgeber muss in diesen SaaS-Dienst einen Webhook implementieren, um proaktiv Benutzer über Änderungen beim Dienst zu benachrichtigen. Die API muss nicht authentifiziert werden und wird vom Microsoft-SaaS-Dienst aufgerufen. Der SaaS-Dienst muss die Vorgangs-API zur Überprüfung und Autorisierung aufrufen, bevor er auf die Webhookbenachrichtigung reagieren kann.

```json
{
    "operationId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "activityId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "subscriptionId":"cd9c6a3a-7576-49f2-b27e-1e5136e57f45",
    "offerId": "ce-dr-tier2",
    "publisherId": "cloudendure",
    "planId": "silver",
    "quantity": "20"  ,
    "action": "Activate",   // Activate/Delete/Suspend/Reinstate/Change[new]  
    "timeStamp": "2018-12-01T00:00:00"
}

```

<!-- Review following, might not be needed when this publishes -->


## <a name="mock-api"></a>Modell-API

Für erste Entwicklungsschritte, besonders zum Erstellen von Prototypen und zum Testen von Projekten, können Sie unsere Modell-APIs verwenden. 

Host-Endpunkt: https://marketplaceapi.microsoft.com/api API-Version: 2018-09-15 Keine Authentifizierung erforderlich Beispiel-URI: https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2018-09-15

Alle API-Aufrufe in diesem Artikel können an den Endpunkt des Modellhosts gerichtet werden. Erwartungsgemäß werden Modelldaten als Antwort zurückgegeben.


## <a name="next-steps"></a>Nächste Schritte

Entwicklern steht außerdem das programmgesteuerte Abrufen und Bearbeiten von Workloads, Angeboten und Herausgeberprofilen mit den [Cloud-Partnerportal-REST-APIs](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) zur Verfügung.
