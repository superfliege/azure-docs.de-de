---
title: Azure Event Grid – Sicherheit und Authentifizierung
description: Beschreibt Azure Event Grid und die zugehörigen Konzepte.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: babanisa
ms.openlocfilehash: a9bffe148339bfac89796405b771e9c2816eb0de
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741520"
---
# <a name="event-grid-security-and-authentication"></a>Event Grid – Sicherheit und Authentifizierung 

Azure Event Grid verfügt über drei Authentifizierungsarten:

* Webhook-Ereignisbereitstellung
* Ereignisabonnements
* Veröffentlichung benutzerdefinierter Themen

## <a name="webhook-event-delivery"></a>Webhook-Ereignisbereitstellung

Ein Webhook ist eine der vielen Möglichkeiten, um Ereignisse aus Azure Event Grid zu empfangen. Wenn ein neues Ereignis bereit ist, sendet der EventGrid-Dienst per POST-Vorgang eine HTTP-Anforderung an den konfigurierten Endpunkt, wobei das Ereignis im Anforderungstext enthalten ist.

Wie viele andere Dienste, die Webhooks unterstützen, müssen Sie bei EventGrid nachweisen, das Sie im „Besitz“ Ihres Webhookendpunkts sind. Vorher wird mit dem Bereitstellen von Ereignissen für diesen Endpunkt nicht begonnen. Mit dieser Anforderung soll verhindert werden, dass ein nicht informierter Endpunkt zum Zielendpunkt für die Ereignisbereitstellung von EventGrid wird. Wenn Sie einen der drei unten angegebenen Azure-Dienste verwenden, wird diese Überprüfung aber automatisch von der Azure-Infrastruktur durchgeführt:

* Azure Logic Apps
* Azure Automation
* Azure Functions für EventGrid-Trigger

Falls Sie einen anderen Typ von Endpunkt nutzen, z.B. eine auf einem HTTP-Trigger basierende Azure-Funktion, muss Ihr Endpunktcode an einem Überprüfungshandshake mit EventGrid beteiligt sein. EventGrid unterstützt zwei unterschiedliche Modelle für Überprüfungshandshakes:

1. **ValidationCode-basierter Handshake**: Zum Zeitpunkt der Erstellung eines Ereignisabonnements sendet EventGrid per POST-Vorgang ein „Abonnementüberprüfungsereignis“ an Ihren Endpunkt. Das Schema dieses Ereignisses ähnelt den anderen Ereignissen vom Typ EventGridEvent, und der Datenteil dieses Ereignisses enthält eine `validationCode`-Eigenschaft. Nachdem Ihre Anwendung bestätigt hat, dass die Überprüfungsanforderung für ein erwartetes Ereignisabonnement bestimmt ist, muss Ihr Anwendungscode antworten, indem ein „Echo“ des Überprüfungscodes zurück an EventGrid gesendet wird. Dieser Handshakemechanismus wird in allen EventGrid-Versionen unterstützt.

2. **ValidationURL-basierter Handshake (manueller Handshake)**: In bestimmten Fällen können Sie ggf. nicht den Quellcode des Endpunkts steuern, um den ValidationCode-basierten Handshake zu implementieren. Wenn Sie beispielsweise einen Drittanbieterdienst nutzen (z.B. [Zapier](https://zapier.com) oder [IFTTT](https://ifttt.com/)), können Sie unter Umständen nicht programmgesteuert mit dem Überprüfungscode antworten. Ab Version 2018-05-01-preview unterstützt EventGrid jetzt einen manuellen Überprüfungshandshake. Wenn Sie ein Ereignisabonnement mit einem SDK oder Tool erstellen, für die diese neue API-Version (2018-05-01-preview oder höher) verwendet wird, sendet EventGrid im Datenteil des Abonnementüberprüfungsereignisses eine `validationUrl`-Eigenschaft. Senden Sie zum Durchführen des Handshakes einfach eine GET-Anforderung für diese URL, und zwar entweder über einen REST-Client oder Ihren Webbrowser. Die für die Überprüfung angegebene URL gilt nur für ungefähr 10 Minuten. Während dieser Zeit lautet der Bereitstellungsstatus des Ereignisabonnements `AwaitingManualAction`. Wenn Sie die manuelle Überprüfung nicht innerhalb von 10 Minuten abschließen, wird der Bereitstellungsstatus auf `Failed` eingestellt. Sie müssen das Ereignisabonnement erneut erstellen, bevor Sie mit der manuellen Überprüfung beginnen.

Dieser Mechanismus der manuellen Überprüfung befindet sich in der Vorschauphase. Sie müssen die [Event Grid-Erweiterung](/cli/azure/azure-cli-extensions-list) für die [Azure CLI](/cli/azure/install-azure-cli) installieren, um es zu verwenden. Diese können Sie mit `az extension add --name eventgrid` installieren. Stellen Sie bei Verwendung der REST-API sicher, dass Sie `api-version=2018-05-01-preview` verwenden.

### <a name="validation-details"></a>Überprüfungsdetails

* Zum Zeitpunkt der Erstellung/Aktualisierung des Ereignisabonnements sendet Event Grid ein Abonnementüberprüfungsereignis an den Zielendpunkt. 
* Das Ereignis enthält den Headerwert „aeg-event-type: SubscriptionValidation“.
* Der Hauptteil des Ereignisses weist dasselbe Schema wie andere Event Grid-Ereignisse auf.
* Die eventType-Eigenschaft des Ereignisses lautet `Microsoft.EventGrid.SubscriptionValidationEvent`.
* Die Dateneigenschaft des Ereignisses enthält eine `validationCode`-Eigenschaft mit einer zufällig generierten Zeichenfolge. Beispiel: „validationCode: acb13…“.
* Wenn Sie die API-Version „2018-05-01-preview“ verwenden, enthalten die Ereignisdaten auch eine `validationUrl`-Eigenschaft mit einer URL für das manuelle Überprüfen des Abonnements.
* Das Array enthält ausschließlich das Validierungsereignis. Andere Ereignisse werden in einer separaten Anforderung gesendet, nachdem Sie den Validierungscode zurückgegeben haben.
* Die EventGrid-DataPlane-SDKs verfügen über Klassen, die den Daten des Abonnementüberprüfungsereignisses und der Abonnementüberprüfungsantwort entsprechen.

Ein Beispiel für „SubscriptionValidationEvent“ finden Sie im folgenden Beispiel:

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6",
    "validationUrl": "https://rp-eastus2.eventgrid.azure.net:553/eventsubscriptions/estest/validate?id=B2E34264-7D71-453A-B5FB-B62D0FDC85EE&t=2018-04-26T20:30:54.4538837Z&apiVersion=2018-05-01-preview&token=1BNqCxBBSSE9OnNSfZM4%2b5H9zDegKMY6uJ%2fO2DFRkwQ%3d"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2018-01-25T22:12:19.4556811Z",
  "metadataVersion": "1",
  "dataVersion": "1"
}]
```

Senden Sie wie im folgenden Beispiel gezeigt den Validierungscode zurück an die Eigenschaft „validationResponse“, um den Besitz des Endpunkts nachzuweisen:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

Alternativ können Sie das Abonnement manuell überprüfen, indem Sie eine GET-Anforderung an die Überprüfungs-URL senden. Das Ereignisabonnement bleibt im Status „Ausstehend“, bis es überprüft wurde.

Unter https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/blob/master/EventGridConsumer/EventGridConsumer/Function1.cs ist ein C#-Beispiel verfügbar, in dem veranschaulicht wird, wie Sie den Handshake für die Abonnementüberprüfung behandeln.

### <a name="checklist"></a>Checkliste

Wenn während der Erstellung des Ereignisabonnements eine Fehlermeldung der Art „Fehler beim Versuch, den angegebenen Endpunkt https://your-endpoint-here zu überprüfen. Ausführlichere Informationen finden Sie unter https://aka.ms/esvalidation.“ angezeigt wird, ist dies ein Hinweis darauf, dass für den Überprüfungshandshake ein Fehler aufgetreten ist. Überprüfen Sie Folgendes, um diesen Fehler zu beheben:

* Verfügen Sie über die Kontrolle über den Anwendungscode auf dem Zielendpunkt? Haben Sie beispielsweise beim Schreiben einer Azure Function, die auf einem HTTP-Trigger basiert, Zugriff auf den Anwendungscode, um Änderungen daran vorzunehmen?
* Wenn Sie Zugriff auf den Anwendungscode haben, sollten Sie den ValidationCode-basierten Handshakemechanismus wie im obigen Beispiel implementieren.

* Wenn Sie keinen Zugriff auf den Anwendungscode haben (z.B. bei Nutzung eines Drittanbieterdiensts, der Webhooks unterstützt), können Sie den manuellen Handshakemechanismus verwenden. Vergewissern Sie sich, dass Sie die API-Version „2018-05-01-preview“ oder eine spätere Version verwenden (installieren Sie die eventgrid-Erweiterung für die Azure-Befehlszeilenschnittstelle), um den validationUrl-Wert im Überprüfungsereignis zu erhalten. Rufen Sie den Wert der `validationUrl`-Eigenschaft ab, und navigieren Sie im Webbrowser zu dieser URL, um den Handshake für die manuelle Validierung durchzuführen. Wenn die Überprüfung erfolgreich durchgeführt wurde, sollte im Webbrowser eine Meldung mit einem entsprechenden Hinweis angezeigt werden. Sie sehen dann, dass „provisioningState“ für das Ereignisabonnement auf „Succeeded“ (Erfolgreich) festgelegt ist. 

### <a name="event-delivery-security"></a>Sicherheit für die Ereignisbereitstellung

Sie können Ihren Webhookendpunkt sichern, indem Sie der Webhook-URL beim Erstellen eines Ereignisabonnements Abfrageparameter hinzufügen. Legen Sie einen dieser Abfrageparameter als Geheimnis fest, z. B. als ein [Zugriffstoken](https://en.wikipedia.org/wiki/Access_token). Der Webhook kann es verwenden, um zu erkennen, dass das Ereignis von Event Grid stammt und gültige Berechtigungen aufweist. Event Grid nimmt diese Abfrageparameter in jede Ereignisbereitstellung an den Webhook auf.

Wenn Sie das Ereignisabonnement bearbeiten, werden die Abfrageparameter nur angezeigt und zurückgegeben, wenn der Parameter [--include-full-endpoint-url](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-show) in Azure [CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) verwendet wird.

Abschließend ist es wichtig zu beachten, dass Azure Event Grid nur HTTPS-Webhook-Endpunkte unterstützt.

## <a name="event-subscription"></a>Ereignisabonnement

Um ein Ereignis abonnieren zu können, müssen Sie nachweisen, dass Sie Zugriff auf die Ereignisquelle und den Ereignishandler haben. Wie Sie nachweisen, dass Sie einen Webhook besitzen, wurde im vorherigen Abschnitt beschrieben. Wenn Sie einen Ereignishandler verwenden, der kein Webhook ist (z.B. einen Event Hub oder Warteschlangenspeicher), benötigen Sie Schreibzugriff auf diese Ressource. Durch diese Berechtigungsüberprüfung wird verhindert, dass ein nicht autorisierter Benutzer Ereignisse an Ihre Ressource sendet.

Sie benötigen die Berechtigung **Microsoft.EventGrid/EventSubscriptions/Write** für die Ressource, die als Ereignisquelle verwendet wird. Sie benötigen diese Berechtigung, da Sie ein neues Abonnement im Gültigkeitsbereich der Ressource schreiben. Die angeforderte Ressource variiert abhängig davon, ob Sie ein Systemthema oder ein benutzerdefiniertes Thema abonnieren. In diesem Abschnitt werden beide Typen beschrieben.

### <a name="system-topics-azure-service-publishers"></a>Systemthemen (Azure-Dienstherausgeber)

Für Systemthemen benötigen Sie die Berechtigung zum Schreiben eines neuen Abonnements im Gültigkeitsbereich der Ressource, die das Ereignis veröffentlicht. Das Format der Ressource ist: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`.

Beispiel: Um ein Ereignis im Speicherkonto **myacct** zu abonnieren, benötigen Sie die Berechtigung Microsoft.EventGrid/EventSubscriptions/Write für: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`.

### <a name="custom-topics"></a>Benutzerdefinierte Themen

Für benutzerdefinierte Themen benötigen Sie die Berechtigung zum Schreiben eines neuen Ereignisabonnements im Gültigkeitsbereich des Event Grid-Themas. Das Format der Ressource ist: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`.

Beispiel: Um das benutzerdefinierte Thema **mytopic** zu abonnieren, benötigen Sie die Berechtigung Microsoft.EventGrid/EventSubscriptions/Write für: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`.

## <a name="custom-topic-publishing"></a>Veröffentlichung benutzerdefinierter Themen

Benutzerdefinierte Themen verwenden Authentifizierungen über Shared Access Signatures (SAS) oder Schlüssel. Wir empfehlen SAS, aber die Schlüsselauthentifizierung ist einfacher zu programmieren und mit vielen vorhandenen Webhookherausgebern kompatibel. 

Sie schließen den Authentifizierungswert in den HTTP-Header ein. Verwenden Sie für SAS den Wert **aeg-sas-token** als Headerwert. Verwenden Sie für die Schlüsselauthentifizierung den Wert **aeg-sas-key** als Headerwert.

### <a name="key-authentication"></a>Schlüsselauthentifizierung

Die Schlüsselauthentifizierung ist die einfachste Form der Authentifizierung. Verwenden Sie das Format: `aeg-sas-key: <your key>`.

Beispielsweise übergeben Sie einen Schlüssel mit:

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>SAS-Token

SAS-Token für Event Grid schließen die Ressource, eine Ablaufzeit und eine Signatur ein. Das Format des SAS-Tokens ist: `r={resource}&e={expiration}&s={signature}`.

Die Ressource ist der Pfad für das Event Grid-Thema, an das Sie Ereignisse senden. Beispielsweise lautet ein gültiger Ressourcenpfad: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`.

Sie generieren die Signatur aus einem Schlüssel.

Beispielsweise ist ein gültiger **aeg-sas-token**-Wert:

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
```

Das folgende Beispiel erstellt ein SAS-Token für die Verwendung mit Event Grid:

```cs
static string BuildSharedAccessSignature(string resource, DateTime expirationUtc, string key)
{
    const char Resource = 'r';
    const char Expiration = 'e';
    const char Signature = 's';

    string encodedResource = HttpUtility.UrlEncode(resource);
    var culture = CultureInfo.CreateSpecificCulture("en-US");
    var encodedExpirationUtc = HttpUtility.UrlEncode(expirationUtc.ToString(culture));

    string unsignedSas = $"{Resource}={encodedResource}&{Expiration}={encodedExpirationUtc}";
    using (var hmac = new HMACSHA256(Convert.FromBase64String(key)))
    {
        string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(unsignedSas)));
        string encodedSignature = HttpUtility.UrlEncode(signature);
        string signedSas = $"{unsignedSas}&{Signature}={encodedSignature}";

        return signedSas;
    }
}
```

## <a name="management-access-control"></a>Verwaltungszugriffssteuerung

Azure Event Grid bietet die Möglichkeit, den Umfang zu steuern, in dem unterschiedliche Benutzer Zugriff auf verschiedene Verwaltungsvorgänge erhalten, z.B. Auflisten und Erstellen von Ereignisabonnements und Generieren von Schlüsseln. Event Grid nutzt die rollenbasierte Zugriffsüberprüfung (Role Based Access Control, RBAC) von Azure.

### <a name="operation-types"></a>Vorgangstypen

Event Grid unterstützt die folgenden Aktionen:

* Microsoft.EventGrid/*/read
* Microsoft.EventGrid/*/write
* Microsoft.EventGrid/*/delete
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action
* Microsoft.EventGrid/topics/listKeys/action
* Microsoft.EventGrid/topics/regenerateKey/action

Die letzten drei Vorgänge geben potenziell geheime Informationen zurück, die aus normalen Lesevorgängen herausgefiltert werden. Es empfiehlt sich, den Zugriff auf diese Vorgänge einzuschränken. 

### <a name="built-in-roles"></a>Integrierte Rollen

Event Grid stellt zwei integrierte Rollen zum Verwalten von Ereignisabonnements bereit. Diese Rollen sind `EventSubscription Contributor (Preview)` und `EventSubscription Reader (Preview)`. Sie sind bei der Implementierung von Ereignisdomänen wichtig. Weitere Informationen zu den zulässigen Aktionen finden Sie unter [Ereignisdomäne: Zugriffsverwaltung](event-domains.md#access-management).

Sie können [diese Rollen einem Benutzer oder Gruppen zuweisen](../role-based-access-control/quickstart-assign-role-user-portal.md).

### <a name="custom-roles"></a>Benutzerdefinierte Rollen

Wenn Sie Berechtigungen angeben müssen, die sich von den integrierten Rollen unterscheiden, können Sie benutzerdefinierte Rollen erstellen.

Im Folgenden finden Sie Beispiele für Event Grid-Rollendefinitionen, die Benutzern das Durchführen unterschiedlicher Aktionen ermöglichen.

**EventGridReadOnlyRole.json:** Zulassen nur von Lesevorgängen.

```json
{
  "Name": "Event grid read only role",
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856",
  "IsCustom": true,
  "Description": "Event grid read only role",
  "Actions": [
    "Microsoft.EventGrid/*/read"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription Id>"
  ]
}
```

**EventGridNoDeleteListKeysRole.json:** Zulassen eingeschränkter Post-Aktionen, nicht aber von Löschaktionen.

```json
{
  "Name": "Event grid No Delete Listkeys role",
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174",
  "IsCustom": true,
  "Description": "Event grid No Delete Listkeys role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action"
  ],
  "NotActions": [
    "Microsoft.EventGrid/*/delete"
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

**EventGridContributorRole.json:** Zulassen aller Event Grid-Aktionen.

```json
{
  "Name": "Event grid contributor role",
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514",
  "IsCustom": true,
  "Description": "Event grid contributor role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/*/delete",
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

Benutzerdefinierte Rollen können mit [PowerShell](../role-based-access-control/custom-roles-powershell.md), der [Azure CLI](../role-based-access-control/custom-roles-cli.md) oder der [REST-API](../role-based-access-control/custom-roles-rest.md) erstellt werden.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
