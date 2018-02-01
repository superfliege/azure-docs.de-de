---
title: "Aufrufen eines Webhooks für Azure-Aktivitätsprotokollwarnungen | Microsoft-Dokumentation"
description: "Leiten Sie Aktivitätsprotokollereignisse für benutzerdefinierte Aktionen an andere Dienste weiter. Senden Sie z.B. SMS, melden Sie Fehler oder benachrichtigen Sie Teams über Chat/Messagingdienst."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 64d333d1-7f37-4a00-9d16-dda6e69a113b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: johnkem
ms.openlocfilehash: 08467aed4e1601b32598fc42515d9c38b601a9d4
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="call-a-webhook-on-azure-activity-log-alerts"></a>Aufrufen eines Webhooks für Azure-Aktivitätsprotokollwarnungen
Mithilfe von Webhooks können Benutzer eine Azure-Warnbenachrichtigung zur Nachbearbeitung oder Ausführung benutzerdefinierter Aktionen an andere Systeme weiterleiten. Sie können einen Webhook für eine Warnung verwenden, um sie an Dienste weiterzuleiten, die SMS-Nachrichten versenden, Fehler protokollieren, ein Team per Chat-/Messagingdienst benachrichtigen oder beliebige andere Aktionen ausführen. In diesem Artikel wird beschrieben, wie Sie einen Webhook festlegen, der beim Auslösen einer Azure-Aktivitätsprotokollwarnung aufgerufen werden soll. Außerdem erfahren Sie, wie die Nutzlast für die HTTP-POST-Methode für einen Webhook aussieht. Informationen zur Einrichtung und zum Schema einer Azure-Metrikwarnung finden Sie auf [dieser Seite](insights-webhooks-alerts.md). Sie können auch eine Aktivitätsprotokollwarnung einrichten, bei deren Aktivierung eine E-Mail gesendet wird.

> [!NOTE]
> Dieses Feature befindet sich zurzeit in der Vorschau und wird zu einem späteren Zeitpunkt entfernt.
>
>

Aktivitätsprotokollwarnungen können mithilfe von [Azure PowerShell-Cmdlets](insights-powershell-samples.md#create-metric-alerts), mithilfe der [plattformübergreifenden Befehlszeilenschnittstelle](insights-cli-samples.md#work-with-alerts) oder mithilfe der [Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn933805.aspx) eingerichtet werden. Derzeit können Sie keine mithilfe des Azure-Portals festlegen.

## <a name="authenticating-the-webhook"></a>Authentifizieren des Webhooks
Der Webhook kann mithilfe folgender Methoden authentifiziert werden:

1. **Tokenbasierte Autorisierung:** Der Webhook-URI wird mit einer Token-ID gespeichert, z.B. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.
2. **Einfache Autorisierung:** Der Webhook-URI wird mit einem Benutzernamen und Kennwort gespeichert, z.B. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`.

## <a name="payload-schema"></a>Nutzlast und Schema
Der POST-Vorgang enthält für alle aktivitätsprotokollbasierten Warnungen die folgende JSON-Nutzlast und das folgende Schema. Dieses Schema ähnelt dem Schema metrikbasierter Warnungen.

```json
{
    "WebhookName": "Alert1515526229589",
    "RequestBody": {
        "schemaId": "Microsoft.Insights/activityLogs",
        "data": {
            "status": "Activated",
            "context": {
                "activityLog": {
                    "authorization": {
                        "action": "Microsoft.Compute/virtualMachines/deallocate/action",
                        "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/Microsoft.Compute/virtualMachines/ContosoVM1"
                    },
                    "channels": "Operation",
                    "claims": {
                        "aud": "https://management.core.windows.net/",
                        "iss": "https://sts.windows.net/00000000-0000-0000-0000-000000000000/",
                        "iat": "1234567890",
                        "nbf": "1234567890",
                        "exp": "1234567890",
                        "aio": "Y2NgYBD8ZLlhu27JU6WZsXemMIvVAAA=",
                        "appid": "00000000-0000-0000-0000-000000000000",
                        "appidacr": "2",
                        "e_exp": "262800",
                        "http://schemas.microsoft.com/identity/claims/identityprovider": "https://sts.windows.net/00000000-0000-0000-0000-000000000000/",
                        "http://schemas.microsoft.com/identity/claims/objectidentifier": "00000000-0000-0000-0000-000000000000",
                        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "00000000-0000-0000-0000-000000000000",
                        "http://schemas.microsoft.com/identity/claims/tenantid": "00000000-0000-0000-0000-000000000000",
                        "uti": "XnCk46TrDkOQXwo49Y8fAA",
                        "ver": "1.0"
                    },
                    "caller": "00000000-0000-0000-0000-000000000000",
                    "correlationId": "00000000-0000-0000-0000-000000000000",
                    "description": "",
                    "eventSource": "Administrative",
                    "eventTimestamp": "2018-01-09T20:11:25.8410967+00:00",
                    "eventDataId": "00000000-0000-0000-0000-000000000000",
                    "level": "Informational",
                    "operationName": "Microsoft.Compute/virtualMachines/deallocate/action",
                    "operationId": "00000000-0000-0000-0000-000000000000",
                    "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
                    "resourceGroupName": "ContosoVM",
                    "resourceProviderName": "Microsoft.Compute",
                    "status": "Succeeded",
                    "subStatus": "",
                    "subscriptionId": "00000000-0000-0000-0000-000000000000",
                    "submissionTimestamp": "2018-01-09T20:11:40.2986126+00:00",
                    "resourceType": "Microsoft.Compute/virtualMachines"
                }
            },
            "properties": {}
        }
    },
    "RequestHeader": {
        "Expect": "100-continue",
        "Host": "s1events.azure-automation.net",
        "User-Agent": "IcMBroadcaster/1.0",
        "X-CorrelationContext": "RkkKACgAAAACAAAAEADlBbM7x86VTrHdQ2JlmlxoAQAQALwazYvJ/INPskb8S5QzgDk=",
        "x-ms-request-id": "00000000-0000-0000-0000-000000000000"
    }
}
```

| Elementname | BESCHREIBUNG |
| --- | --- |
| status |Wird für Metrikwarnungen verwendet. Muss für Aktivitätsprotokollwarnungen immer auf „activated“ festgelegt werden. |
| context |Der Kontext des Ereignisses. |
| activityLog | Die Protokolleigenschaften des Ereignisses|
| authorization |Die RBAC-Eigenschaften des Ereignisses. Hierzu zählen üblicherweise „action“, „role“ und „scope“. |
| action | Von der Warnung erfasste Aktion |
| scope | Bereich der Warnung (d.h. Ressource)|
| channels | Vorgang |
| claims | Eine Auflistung von Informationen im Zusammenhang mit den Ansprüchen |
| caller |GUID oder Benutzername des Benutzers, der den Vorgang, UPN-Anspruch oder SPN-Anspruch ausgeführt hat (sofern verfügbar) Kann für bestimmte Systemaufrufe NULL sein. |
| correlationId |Üblicherweise eine GUID in Zeichenfolgenformat. Ereignisse mit „correlationId“ gehören zur gleichen übergeordneten Aktion und besitzen üblicherweise den gleichen correlationId-Wert. |
| Beschreibung |Die bei der Erstellung der Warnung festgelegte Warnungsbeschreibung. |
| eventSource |Der Name des Azure-Diensts oder der Infrastruktur, der bzw. die das Ereignis generiert hat. |
| eventTimestamp |Zeitpunkt, zu dem das Ereignis aufgetreten ist |
| eventDataId |Eindeutiger Bezeichner für das Ereignis. |
| level |Einer der folgenden Werte: „Kritisch“, „Fehler“, „Warnung“, „Information“ und „Ausführlich“ |
| operationName |Name des Vorgangs. |
| operationId |Üblicherweise eine gemeinsame GUID für alle Ereignisse des gleichen Vorgangs. |
| Ressourcen-ID |Ressourcen-ID der betroffenen Ressource. |
| resourceGroupName |Der Name der Ressourcengruppe für die betroffene Ressource. |
| resourceProviderName |Der Ressourcenanbieter der betroffenen Ressource. |
| status |Eine Zeichenfolge. Der Status des Vorgangs. Gängige Werte: „Started“, „In Progress“, „Succeeded“, „Failed“, „Active“, „Resolved“. |
| subStatus |Enthält üblicherweise den HTTP-Statuscode des zugehörigen REST-Aufrufs. Es können auch weitere Zeichenfolgen enthalten sein, die einen Unterstatus beschreiben. Gängige subStatus-Werte: „OK“ (HTTP-Statuscode: 200), „Erstellt“ (HTTP-Statuscode: 201), „Akzeptiert“ (HTTP-Statuscode: 202), „Kein Inhalt“ (HTTP-Statuscode: 204), „Ungültige Anforderung“ (HTTP-Statuscode: 400), „Nicht gefunden“ (HTTP-Statuscode: 404), „Konflikt“ (HTTP-Statuscode: 409), „Interner Serverfehler“ (HTTP-Statuscode: 500), „Dienst nicht verfügbar“ (HTTP-Statuscode: 503), „Gatewaytimeout“ (HTTP-Statuscode: 504) |
| subscriptionId |Die Azure-Abonnement-ID. |
| submissionTimestamp |Die Zeit, zu der das Ereignis durch den Azure-Dienst generiert wurde, der die Anforderung verarbeitet hat. |
| resourceType | Der Ressourcentyp, der das Ereignis generiert hat|
| Eigenschaften |Eine Gruppe von `<Key, Value>`-Paaren (`Dictionary<String, String>`) mit Details zum Ereignis. |

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zum Aktivitätsprotokoll](monitoring-overview-activity-logs.md)
* [Ausführen von Azure Automation-Skripts (Runbooks) für Azure-Warnungen](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Senden einer SMS über Twilio auf der Grundlage einer Azure-Warnung mithilfe einer Logik-App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Dieses Beispiel ist eigentlich für Metrikwarnungen konzipiert, kann jedoch für Aktivitätsprotokollwarnungen angepasst werden.
* [Senden einer Slack-Nachricht auf der Grundlage einer Azure-Warnung mithilfe einer Logik-App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Dieses Beispiel ist eigentlich für Metrikwarnungen konzipiert, kann jedoch für Aktivitätsprotokollwarnungen angepasst werden.
* [Senden einer Nachricht an eine Azure-Warteschlange auf der Grundlage einer Azure-Warnung mithilfe einer Logik-App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Dieses Beispiel ist eigentlich für Metrikwarnungen konzipiert, kann jedoch für Aktivitätsprotokollwarnungen angepasst werden.
