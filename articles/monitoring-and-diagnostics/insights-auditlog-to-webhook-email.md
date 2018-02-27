---
title: "Aufrufen eines Webhooks für eine Azure-Aktivitätsprotokollwarnung | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie Aktivitätsprotokollereignisse für benutzerdefinierte Aktionen an andere Dienste weiterleiten. So können Sie beispielsweise SMS-Nachrichten senden, Fehler protokollieren oder ein Team per Chat-/Messagingdienst benachrichtigen."
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
ms.openlocfilehash: 9872c30d123f0a7443e28dc58ee0d4e16572a390
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2018
---
# <a name="call-a-webhook-on-an-azure-activity-log-alert"></a>Aufrufen eines Webhooks für eine Azure-Aktivitätsprotokollwarnung
Mithilfe von Webhooks können Benutzer eine Azure-Warnbenachrichtigung zur Nachbearbeitung oder Ausführung benutzerdefinierter Aktionen an andere Systeme weiterleiten. Sie können einen Webhook für eine Warnung verwenden, um sie an Dienste weiterzuleiten, die SMS-Nachrichten versenden, um Fehler zu protokollieren, um ein Team per Chat-/Messagingdienst zu benachrichtigen oder um verschiedene andere Aktionen auszuführen. Sie können auch eine Aktivitätsprotokollwarnung einrichten, bei deren Aktivierung eine E-Mail gesendet wird.

In diesem Artikel erfahren Sie, wie Sie einen Webhook festlegen, der beim Auslösen einer Azure-Aktivitätsprotokollwarnung aufgerufen wird. Außerdem erfahren Sie, wie die Nutzlast für die HTTP-POST-Methode für einen Webhook aussieht. Informationen zur Einrichtung und zum Schema einer Azure-Metrikwarnung finden Sie unter [Konfigurieren eines Webhooks für eine Azure-Metrikwarnung](insights-webhooks-alerts.md). 

> [!NOTE]
> Das Feature, das das Aufrufen eines Webhooks für eine Azure-Aktivitätsprotokollwarnung unterstützt, befindet sich derzeit noch in der Vorschauphase.
>
>

Aktivitätsprotokollwarnungen können mithilfe von [Azure PowerShell-Cmdlets](insights-powershell-samples.md#create-metric-alerts), mithilfe einer [plattformübergreifenden Befehlszeilenschnittstelle](insights-cli-samples.md#work-with-alerts) oder über die [Azure Monitor-REST-APIs](https://msdn.microsoft.com/library/azure/dn933805.aspx) eingerichtet werden. Das Azure-Portal kann derzeit nicht zum Einrichten einer Aktivitätsprotokollwarnung verwendet werden.

## <a name="authenticate-the-webhook"></a>Authentifizieren des Webhooks
Der Webhook kann mithilfe einer der folgenden Methoden authentifiziert werden:

* **Tokenbasierte Autorisierung:** Der Webhook-URI wird mit einer Token-ID gespeichert. Beispiel: `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
* **Standardautorisierung:** Der Webhook-URI wird mit einem Benutzernamen und Kennwort gespeichert. Beispiel: `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Nutzlast und Schema
Der POST-Vorgang enthält für alle aktivitätsprotokollbasierten Warnungen die folgende JSON-Nutzlast und das folgende Schema. Dieses Schema ähnelt dem Schema für metrikbasierte Warnungen.

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

| Elementname | Beschreibung |
| --- | --- |
| status |Wird für Metrikwarnungen verwendet. Muss für Aktivitätsprotokollwarnungen immer auf „Aktiviert“ festgelegt werden.|
| context |Der Kontext des Ereignisses. |
| activityLog | Die Protokolleigenschaften des Ereignisses.|
| authorization |Die Eigenschaften der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) des Ereignisses. Zu diesen Eigenschaften zählen üblicherweise **action**, **role** und **scope**. |
| action | Die von der Warnung erfasste Aktion. |
| scope | Der Bereich der Warnung (also die Ressource).|
| channels | Der Vorgang. |
| claims | Eine Auflistung von Informationen im Zusammenhang mit den Ansprüchen. |
| caller |Die GUID oder der Benutzername des Benutzers, der den Vorgang, UPN-Anspruch oder SPN-Anspruch ausgeführt hat (sofern verfügbar). Kann für bestimmte Systemaufrufe NULL sein. |
| correlationId |Üblicherweise eine GUID in Zeichenfolgenformat. Ereignisse mit **correlationId** gehören zur gleichen übergeordneten Aktion. Sie besitzen üblicherweise den gleichen **correlationId**-Wert. |
| Beschreibung |Die Warnungsbeschreibung, die beim Erstellen der Warnung festgelegt wurde. |
| eventSource |Der Name des Azure-Diensts oder der Infrastruktur, der bzw. die das Ereignis generiert hat. |
| eventTimestamp |Die Zeit, zu der das Ereignis aufgetreten ist. |
| eventDataId |Der eindeutige Bezeichner des Ereignisses. |
| level |Einer der folgenden Werte: „Kritisch“, „Fehler“, „Warnung“, „Information“ oder „Ausführlich“. |
| operationName |Der Name des Vorgangs. |
| operationId |In der Regel eine gemeinsame GUID der Ereignisse. Die GUID entspricht in der Regel einem einzelnen Vorgang. |
| Ressourcen-ID |Die Ressourcen-ID der betroffenen Ressource. |
| resourceGroupName |Der Name der Ressourcengruppe für die betroffene Ressource. |
| resourceProviderName |Der Ressourcenanbieter der betroffenen Ressource. |
| status |Ein Zeichenfolgenwert, der den Status des Vorgangs angibt. Gängige Werte: „Gestartet“, „In Bearbeitung“, „Erfolgreich“, „Fehler“, „Aktiv“ und „Aufgelöst“. |
| subStatus |Enthält üblicherweise den HTTP-Statuscode des zugehörigen REST-Aufrufs. Es können auch weitere Zeichenfolgen enthalten sein, die einen Unterstatus beschreiben. Gängige subStatus-Werte: „OK“ (HTTP-Statuscode: 200), „Erstellt“ (HTTP-Statuscode: 201), „Akzeptiert“ (HTTP-Statuscode: 202), „Kein Inhalt“ (HTTP-Statuscode: 204), „Ungültige Anforderung“ (HTTP-Statuscode: 400), „Nicht gefunden“ (HTTP-Statuscode: 404), „Konflikt“ (HTTP-Statuscode: 409), „Interner Serverfehler“ (HTTP-Statuscode: 500), „Dienst nicht verfügbar“ (HTTP-Statuscode: 503) und „Gatewaytimeout“ (HTTP-Statuscode: 504). |
| subscriptionId |Die Azure-Abonnement-ID. |
| submissionTimestamp |Die Zeit, zu der das Ereignis durch den Azure-Dienst generiert wurde, der die Anforderung verarbeitet hat. |
| resourceType | Der Ressourcentyp, der das Ereignis generiert hat|
| Eigenschaften |Ein Satz von Schlüssel-Wert-Paaren mit Details zum Ereignis. Beispiel: `Dictionary<String, String>`. |

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich ausführlicher über das [Aktivitätsprotokoll](monitoring-overview-activity-logs.md).
* Erfahren Sie, wie Sie [Azure Automation-Skripts (Runbooks) für Azure-Warnungen ausführen](http://go.microsoft.com/fwlink/?LinkId=627081).
* Informieren Sie sich über das [Senden einer SMS-Nachricht über Twilio auf der Grundlage einer Azure-Warnung mithilfe einer Logik-App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Dieses Beispiel ist eigentlich für Metrikwarnungen konzipiert, kann jedoch für Aktivitätsprotokollwarnungen angepasst werden.
* Informieren Sie sich über das [Senden einer Slack-Nachricht auf der Grundlage einer Azure-Warnung mithilfe einer Logik-App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Dieses Beispiel ist eigentlich für Metrikwarnungen konzipiert, kann jedoch für Aktivitätsprotokollwarnungen angepasst werden.
* Informieren Sie sich über das [Senden einer Nachricht an eine Azure-Warteschlange auf der Grundlage einer Azure-Warnung mithilfe einer Logik-App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Dieses Beispiel ist eigentlich für Metrikwarnungen konzipiert, kann jedoch für Aktivitätsprotokollwarnungen angepasst werden.
