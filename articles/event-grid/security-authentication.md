---
title: "Azure Event Grid – Sicherheit und Authentifizierung"
description: "Beschreibt Azure Event Grid und die zugehörigen Konzepte."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 09/18/2017
ms.author: babanisa
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: e2f48b6e72072ce6bf019b3adc138ae83c162f25
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---
# <a name="event-grid-security-and-authentication"></a>Event Grid – Sicherheit und Authentifizierung 

Azure Event Grid verfügt über drei Authentifizierungsarten:

* Ereignisabonnements
* Ereignisveröffentlichung
* Webhook-Ereignisbereitstellung

## <a name="webhook-event-delivery"></a>Webhook-Ereignisbereitstellung

Ein Webhook ist eine von vielen Möglichkeiten zum Empfangen von Ereignissen in Echtzeit von Azure Event Grid. Jedes Mal, wenn ein neues Ereignis bereitgestellt werden kann, fügt der Event Grid-Webhook eine HTTP-Anforderung mit dem Ereignis im Hauptteil an den konfigurierten HTTP-Endpunkt ein.

Wenn Sie Ihren eigenen Webhook-Endpunkt bei Event Grid registrieren, wird Ihnen eine POST-Anforderung mit einem einfachen Validierungscode gesendet, damit Sie den Besitz des Endpunkts nachweisen können. Ihre App muss durch Rückgabe desselben Validierungscodes reagieren. Event Grid übermittelt keine Ereignisse an Webhook-Endpunkte, welche die Überprüfung nicht erfolgreich abgeschlossen haben.

### <a name="validation-details"></a>Überprüfungsdetails

* Zum Zeitpunkt der Erstellung/Aktualisierung des Ereignisabonnements sendet Event Grid ein Ereignis „SubscriptionValidationEvent“ an den Zielendpunkt.
* Das Ereignis enthält den Headerwert „Aeg-Event-Type: SubscriptionValidation“.
* Der Hauptteil des Ereignisses weist dasselbe Schema wie andere Event Grid-Ereignisse auf.
* Die Ereignisdaten enthalten die Eigenschaft „validationCode“ mit einer zufällig generierten Zeichenfolge. Beispiel: „validationCode: acb13…“.

Ein Beispiel für „SubscriptionValidationEvent“ finden Sie im folgenden Beispiel:

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2017-08-06T22:09:30.740323Z"
}]
```

Senden Sie wie im folgenden Beispiel gezeigt den Validierungscode zurück an die Eigenschaft „validationResponse“, um den Besitz des Endpunkts nachzuweisen:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

Abschließend ist es wichtig zu beachten, dass Azure Event Grid nur HTTPS-Webhook-Endpunkte unterstützt.

## <a name="event-subscription"></a>Ereignisabonnement

Um ein Ereignis zu abonnieren, benötigen Sie die Berechtigung **Microsoft.EventGrid/EventSubscriptions/Write** für die angeforderte Ressource. Sie benötigen diese Berechtigung, da Sie ein neues Abonnement im Gültigkeitsbereich der Ressource schreiben. Die angeforderte Ressource variiert abhängig davon, ob Sie ein Systemthema oder ein benutzerdefiniertes Thema abonnieren. In diesem Abschnitt werden beide Typen beschrieben.

### <a name="system-topics-azure-service-publishers"></a>Systemthemen (Azure-Dienstherausgeber)

Für Systemthemen benötigen Sie die Berechtigung zum Schreiben eines neuen Abonnements im Gültigkeitsbereich der Ressource, die das Ereignis veröffentlicht. Das Format der Ressource ist: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`.

Beispiel: Um ein Ereignis im Speicherkonto **myacct** zu abonnieren, benötigen Sie die Berechtigung Microsoft.EventGrid/EventSubscriptions/Write für: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`.

### <a name="custom-topics"></a>Benutzerdefinierte Themen

Für benutzerdefinierte Themen benötigen Sie die Berechtigung zum Schreiben eines neuen Abonnements im Gültigkeitsbereich des Event Grid-Themas. Das Format der Ressource ist: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`.

Beispiel: Um das benutzerdefinierte Thema **mytopic** zu abonnieren, benötigen Sie die Berechtigung Microsoft.EventGrid/EventSubscriptions/Write für: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`.

## <a name="topic-publishing"></a>Themenveröffentlichung

Themen verwenden Authentifizierungen über Shared Access Signatures (SAS) oder Schlüssel. Wir empfehlen SAS, aber die Schlüsselauthentifizierung ist einfacher zu programmieren und mit vielen vorhandenen Webhookherausgebern kompatibel. 

Sie schließen den Authentifizierungswert in den HTTP-Header ein. Verwenden Sie für SAS den Wert **aeg-sas-token** als Headerwert. Verwenden Sie für die Schlüsselauthentifizierung den Wert **aeg-sas-key** als Headerwert.

### <a name="key-authentication"></a>Schlüsselauthentifizierung

Die Schlüsselauthentifizierung ist die einfachste Form der Authentifizierung. Verwenden Sie das Format: `aeg-sas-key: <your key>`.

Beispielsweise übergeben Sie einen Schlüssel mit:

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>SAS-Token

SAS-Token für Event Grid schließen die Ressource, eine Ablaufzeit und eine Signatur ein. Das Format des SAS-Tokens ist: `r={resource}&e={expiration}&s={signature}`.

Die Ressource ist der Pfad für das Thema, an das Sie Ereignisse senden. Beispielsweise lautet ein gültiger Ressourcenpfad: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`.

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

Azure Event Grid bietet die Möglichkeit, den Umfang zu steuern, in dem unterschiedliche Benutzer Zugriff auf verschiedene Verwaltungsvorgänge erhalten, z.B. Auflisten und Erstellen von Ereignisabonnements und Generieren von Schlüsseln. Event Grid nutzt die rollenbasierte Zugriffsüberprüfung (Role Based Access Check, RBAC) von Azure.

### <a name="operation-types"></a>Vorgangstypen

Azure Event Grid unterstützt die folgenden Aktionen:

* Microsoft.EventGrid/*/read
* Microsoft.EventGrid/*/write
* Microsoft.EventGrid/*/delete
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action
* Microsoft.EventGrid/topics/listKeys/action
* Microsoft.EventGrid/topics/regenerateKey/action

Die letzten drei Vorgänge geben potenziell geheime Informationen zurück, die aus normalen Lesevorgängen herausgefiltert werden. Es ist eine bewährte Methode, den Zugriff auf diese Vorgänge einzuschränken. Benutzerdefinierte Rollen können mit [Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md), der [Azure-Befehlszeilenschnittstelle (CLI)](../active-directory/role-based-access-control-manage-access-azure-cli.md) und der [REST-API](../active-directory/role-based-access-control-manage-access-rest.md) erstellt werden.

### <a name="enforcing-role-based-access-check-rbac"></a>Erzwingen der rollenbasierten Zugriffsüberprüfung (Role Based Access Check, RBAC)

Gehen Sie folgendermaßen vor, um RBAC für verschiedene Benutzer zu erzwingen:

#### <a name="create-a-custom-role-definition-file-json"></a>Erstellen einer benutzerdefinierten Rollendefinitionsdatei (JSON)

Im Folgenden finden Sie Beispiele für Event Grid-Rollendefinitionen, die Benutzern das Durchführen unterschiedlicher Gruppen von Aktionen genehmigen.

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

#### <a name="create-and-assign-custom-role-with-azure-cli"></a>Erstellen und Zuweisen einer benutzerdefinierten Rolle mit Azure CLI

So erstellen Sie eine benutzerdefinierte Rolle:

```azurecli
az role definition create --role-definition @<file path>
```

Verwenden Sie Folgendes zum Zuweisen der Rolle zu einem Benutzer:

```azurecli
az role assignment create --assignee <user name> --role "<name of role>"
```

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).

