---
title: Verwenden einer durch den Benutzer zugewiesenen verwalteten Dienstidentität (Managed Service Identity, MSI) zum Abrufen eines Zugriffstokens für eine VM.
description: Schrittanleitungen und Beispiele zur Verwendung einer durch den Benutzer zugewiesenen verwalteten Dienstidentität einer Azure-VM zum Abrufen eines OAuth-Zugriffstokens.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 68454d3f3880df82ca895d1c5f140ebdb6030e77
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="acquire-an-access-token-for-a-vm-user-assigned-managed-service-identity-msi"></a>Abrufen eines Zugriffstokens für eine durch den Benutzer zugewiesenen verwalteten Dienstidentität (Managed Service Identity, MSI) für eine VM

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]
Dieser Artikel enthält verschiedene Code- und Skriptbeispiele für den Tokenabruf sowie eine Anleitung zu wichtigen Themen, z.B. zur Behandlung bei Tokenablauf und HTTP-Fehlern.

## <a name="prerequisites"></a>Voraussetzungen
[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]
Wenn Sie die Azure PowerShell-Beispiele in diesem Artikel verwenden möchten, müssen Sie die neueste Version von [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) installieren.

> [!IMPORTANT]
> - Bei allen Beispielcodes und -skripts in diesem Artikel wird vorausgesetzt, dass der Client auf einem MSI-fähigen virtuellen Computer ausgeführt wird. Verwenden Sie die Funktion „Verbinden“ im Azure-Portal zum Herstellen einer Remoteverbindung mit Ihrem virtuellen Computer. Weitere Informationen zum Aktivieren einer verwalteten Dienstidentität für eine VM finden Sie unter [Konfigurieren einer durch den Benutzer zugewiesenen VM-MSI (Managed Service Identity, verwaltete Dienstidentität) mit der Azure CLI](msi-qs-configure-cli-windows-vm.md) oder in einem der verwandten Artikel (Verwendung von Portal, PowerShell, Vorlage oder Azure SDK). 

## <a name="overview"></a>Übersicht

Eine Clientanwendung kann ein [App-exklusives Zugriffstoken](~/articles/active-directory/develop/active-directory-dev-glossary.md#access-token) der verwalteten Dienstidentität für den Zugriff auf eine bestimmte Ressource anfordern. Das Token [basiert auf dem MSI-Dienstprinzipal](msi-overview.md#how-does-it-work). Daher muss sich der Client nicht selbst registrieren, um ein Zugriffstoken unter seinem eigenen Dienstprinzipal abzurufen. Das Token ist geeignet für die Nutzung als Bearertoken in [Dienst-zu-Dienst-Aufrufen, für die Clientanmeldeinformationen benötigt werden](~/articles/active-directory/active-directory-protocols-oauth-service-to-service.md).

|  |  |
| -------------- | -------------------- |
| [Abrufen eines Tokens über HTTP](#get-a-token-using-http) | Protokolldetails für den MSI-Tokenendpunkt |
| [Abrufen eines Tokens über cURL](#get-a-token-using-curl) | Beispiel zur Verwendung des MSI-REST-Endpunkts über einen Bash/cURL-Client |
| [Behandlung bei Tokenablauf](#handling-token-expiration) | Anleitung zur Behandlung abgelaufener Zugriffstoken |
| [Fehlerbehandlung](#error-handling) | Anleitung zur Behandlung von vom MSI-Tokenendpunkt zurückgegebenen HTTP-Fehlern |
| [Ressourcen-IDs für Azure-Dienste](#resource-ids-for-azure-services) | Abrufen von Ressourcen-IDs für unterstützte Azure-Dienste |

## <a name="get-a-token-using-http"></a>Abrufen eines Tokens über HTTP 

Die grundlegende Schnittstelle zum Abrufen eines Zugriffstokens basiert auf REST, sodass sie für alle auf dem virtuellen Computer ausgeführten Clientanwendungen, die HTTP-REST-Aufrufe ausführen können, zur Verfügung steht. Dies ähnelt dem Azure AD-Programmiermodell, aber der Client verwendet einen Endpunkt auf dem virtuellen Computer (und keinen Azure AD-Endpunkt).

Beispielanforderung mit dem IMDS-Endpunkt (Instance Metadata Service, Instanzmetadatendienst):

```
GET http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F&client_id=712eac09-e943-418c-9be6-9fd5c91078bl HTTP/1.1 Metadata: true
```

Beispielanforderung mit dem MSI-VM-Erweiterungsendpunkt (wird in Kürze als veraltet eingestuft):

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F&client_id=712eac09-e943-418c-9be6-9fd5c91078bl HTTP/1.1 Metadata: true
```

| Element | Beschreibung |
| ------- | ----------- |
| `GET` | Das HTTP-Verb, mit dem angegeben wird, dass Sie Daten vom Endpunkt abrufen möchten. In diesem Fall ist dies ein OAuth-Zugriffstoken. | 
| `http://169.254.169.254/metadata/identity/oauth2/token` | Der MSI-Endpunkt für den Instanzmetadatendienst. |
| `http://localhost:50342/oauth2/token` | Der MSI-Endpunkt für die VM-Erweiterung, wobei 50342 der Standardport ist (konfigurierbar). |
| `api-version`  | Ein Abfragezeichenfolgenparameter, mit dem die API-Version für den IMDS-Endpunkt angegeben wird.  |
| `resource` | Ein Abfragezeichenfolgenparameter, der den App-ID-URI der Zielressource angibt. Er wird auch im Anspruch `aud` (audience) des ausgestellten Tokens angezeigt. In diesem Beispiel wird ein Token für den Zugriff auf Azure Resource Manager angefordert, das über den App-ID-URI https://management.azure.com/ verfügt. |
| `client_id` |  Ein *optionaler* Abfragezeichenfolgenparameter, mit dem die Client-ID (auch als App-ID bezeichnet) des Dienstprinzipals angegeben wird, der eine vom Benutzer zugewiesene MSI repräsentiert. Wenn Sie einen vom System zugewiesenen MSI verwenden, ist dieser Parameter nicht erforderlich. Dieser Wert wird während der Erstellung einer vom Benutzer zugewiesenen MSI in der Eigenschaft `clientId` zurückgegeben. In diesem Beispiel wird ein Token für die Client-ID „712eac09-e943-418c-9be6-9fd5c91078bl“ angefordert. |
| `Metadata` | Ein HTTP-Anforderungsheader-Feld, das für MSI als Maßnahme gegen SSRF-Angriffe (Server Side Request Forgery) erforderlich ist. Dieser Wert muss auf „true“ (in Kleinbuchstaben) festgelegt werden.

Beispiel für eine Antwort:

```
HTTP/1.1 200 OK
Content-Type: application/json
{
  "access_token": "eyJ0eXAi...",
  "expires_in": "3599",
  "expires_on": "1506484173",
  "not_before": "1506480273",
  "resource": "https://management.azure.com/",
  "token_type": "Bearer"
  "client_id":"712eac09-e943-418c-9be6-9fd5c91078bl"
}
```

| Element | BESCHREIBUNG |
| ------- | ----------- |
| `access_token` | Das angeforderte Zugriffstoken. Beim Aufrufen einer geschützten REST-API wird das Token als „Bearertoken“ in das `Authorization`-Anforderungsheader-Feld eingebettet, damit der Aufrufer von der API authentifiziert werden kann. | 
| `expires_in` | Der Zeitraum in Sekunden, wie lange das Zugriffstoken ab dem Zeitpunkt der Ausstellung gültig ist, bevor es abläuft. Der Ausstellungszeitpunkt ist im Anspruch `iat` des Tokens zu finden. |
| `expires_on` | Der Zeitpunkt, zu dem das Zugriffstoken abläuft. Das Datum wird als Anzahl von Sekunden ab „1970-01-01T0:0:0Z UTC“ (entspricht dem Anspruch `exp` des Tokens) dargestellt. |
| `not_before` | Der Zeitpunkt, ab dem das Zugriffstoken wirksam ist und akzeptiert werden kann. Das Datum wird als Anzahl von Sekunden ab „1970-01-01T0:0:0Z UTC“ (entspricht dem Anspruch `nbf` des Tokens) dargestellt. |
| `resource` | Die Ressource, für die das Zugriffstoken angefordert wurde (Übereinstimmung mit dem Abfragezeichenfolgenparameter `resource` der Anforderung). |
| `token_type` | Der Typ des Tokens. In diesem Fall ein „Bearerzugriffstoken“, sodass die Ressource Zugriff auf den Bearer dieses Tokens gewähren kann. |
| `client_id` | Die Client-ID (auch App-ID) des Dienstprinzipals, der die vom Benutzer zugewiesene MSI repräsentiert, für die das Token angefordert wurde. |

## <a name="get-a-token-using-curl"></a>Abrufen eines Tokens über cURL

Stellen Sie sicher, dass die Client-ID (auch App-ID) für den Dienstprinzipal der durch den Benutzer zugewiesenen MSI durch den <MSI CLIENT ID>-Wert des Parameters `client_id` ersetzt wird. Dieser Wert wird während der Erstellung einer vom Benutzer zugewiesenen MSI in der Eigenschaft `clientId` zurückgegeben.
  
Beispielanforderung mit dem IMDS-Endpunkt (Instance Metadata Service, Instanzmetadatendienst):

   ```bash
   response=$(curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com/&client_id=<MSI CLIENT ID>")
   access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
   echo The MSI access token is $access_token
   ```
   
Beispielanforderung mit dem MSI-VM-Erweiterungsendpunkt (wird in Kürze als veraltet eingestuft):

   ```bash
   response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/&client_id=<MSI CLIENT ID>" -H Metadata:true -s)
   access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
   echo The MSI access token is $access_token
   ```

   Beispielantworten:

   ```bash
   user@vmLinux:~$ response=$(curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com/&client_id=9d484c98-b99d-420e-939c-z585174b63bl")
   user@vmLinux:~$ access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
   user@vmLinux:~$ echo The MSI access token is $access_token
   The MSI access token is eyJ0eXAiOiJKV1QiLCJhbGciO...
   ```

## <a name="handling-token-expiration"></a>Vorgehen bei Tokenablauf

Im MSI-Subsystem werden Token zwischengespeichert. Daher können Sie sie beliebig oft aufrufen, und ein Aufruf an Azure AD über das Netzwerk erfolgt nur in folgenden Fällen:
- Cachefehler aufgrund eines fehlenden Tokens im Cache
- Tokenablauf

Wenn Sie das Token in Ihrem Code zwischenspeichern, sollten Sie auf die Behandlung von Szenarien vorbereitet sein, bei denen die Ressource angibt, dass das Token abgelaufen ist.

## <a name="error-handling"></a>Fehlerbehandlung 

Der MSI-Endpunkt signalisiert Fehler über das Statuscodefeld des Nachrichtenheaders der HTTP-Antwort als 4xx- oder 5xx-Fehler:

| Statuscode | Fehlerursache | Fehlerbehandlung |
| ----------- | ------------ | ------------- |
| 4xx – Fehler in der Anforderung. | Mindestens einer der Anforderungsparameter war falsch. | Wiederholen Sie den Vorgang nicht.  Überprüfen Sie den Fehler, um weitere Informationen zu erhalten.  4xx-Fehler sind Fehler während der Entwurfszeit.|
| 5xx – Vorübergehender Fehler vom Dienst. | Das MSI-Subsystem oder Azure Active Directory hat einen vorübergehenden Fehler zurückgegeben. | Nach mindestens 1 Sekunde können Sie den Vorgang wiederholen.  Wenn Sie den Vorgang zu schnell oder zu oft wiederholen, gibt Azure AD möglicherweise einen Fehler zum Ratenlimit zurück (429).|

Wenn ein Fehler auftritt, enthält der entsprechende HTTP-Antworttext JSON-Code mit den Fehlerdetails:

| Element | BESCHREIBUNG |
| ------- | ----------- |
| error   | Fehler-ID |
| error_description | Ausführliche Beschreibung des Fehlers. **Fehlerbeschreibungen können sich jederzeit ändern. Schreiben Sie keinen Code, der sich basierend auf Werten in der Fehlerbeschreibung brancht.**|

### <a name="http-response-reference"></a>Referenz für HTTP-Antworten

In diesem Abschnitt sind die möglichen Fehlerantworten aufgeführt. Der Status „200 OK“ ist eine erfolgreiche Antwort, und das Zugriffstoken ist im JSON-Antworttext im Element „access_token“ enthalten.

| Statuscode | Error | Fehlerbeschreibung | Lösung |
| ----------- | ----- | ----------------- | -------- |
| 400 – Ungültige Anforderung | invalid_resource | AADSTS50001: Die Anwendung *\<URI\>* wurde im Mandanten *\<TENANT-ID\>* nicht gefunden. Dies kann auftreten, wenn die Anwendung nicht vom Administrator des Mandanten installiert wurde oder wenn sie von den Benutzern des Mandanten keine Zustimmung erhalten hat. Unter Umständen haben Sie Ihre Authentifizierung an den falschen Mandanten gesendet. | (Nur Linux) |
| 400 – Ungültige Anforderung | bad_request_102 | Erforderlicher Metadatenheader nicht angegeben | Entweder fehlt der `Metadata`-Anforderungsheader in Ihrer Anforderung, oder er ist falsch formatiert. Der Wert muss als `true` (in Kleinbuchstaben) angegeben werden. Ein Beispiel finden Sie im Abschnitt [Abrufen eines Tokens über HTTP](#get-a-token-using-http) unter „Beispiel für eine Anforderung“.|
| 401 – Nicht autorisiert | unknown_source | Unbekannte Quelle *\<URI\>* | Stellen Sie sicher, dass Ihr HTTP GET-Anforderungs-URI richtig formatiert ist. Der Teil `scheme:host/resource-path` muss als `http://169.254.169.254/metadata/identity/oath2/token` oder `http://localhost:50342/oauth2/token` angegeben werden. Ein Beispiel finden Sie im Abschnitt [Abrufen eines Tokens über HTTP](#get-a-token-using-http) unter „Beispiel für eine Anforderung“.|
|           | invalid_request | In der Anforderung fehlt ein erforderlicher Parameter, ist ein ungültiger Parameter enthalten oder ist ein Parameter mehrfach vorhanden, oder die Anforderung ist auf andere Weise fehlerhaft. |  |
|           | unauthorized_client | Der Client ist zum Anfordern eines Zugriffstokens mit dieser Methode nicht autorisiert. | Ursache ist eine Anforderung, die keinen lokalen Loopback zum Aufrufen der Erweiterung verwendet hat, oder auf einem virtuellen Computer, auf dem keine verwaltete Dienstidentität ordnungsgemäß konfiguriert ist. Hilfe zur Konfiguration des virtuellen Computers finden Sie unter [Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) über das Azure-Portal](msi-qs-configure-portal-windows-vm.md). |
|           | access_denied | Der Ressourcenbesitzer oder Autorisierungsserver hat die Anforderung verweigert. |  |
|           | unsupported_response_type | Der Autorisierungsserver unterstützt das Abrufen eines Zugriffstokens mit dieser Methode nicht. |  |
|           | invalid_scope | Der angeforderte Bereich ist ungültig, unbekannt oder falsch formatiert. |  |
| 500 Interner Serverfehler | unknown | Beim Abrufen des Tokens aus Active Directory ist ein Fehler aufgetreten. Details finden Sie in den Protokollen unter *\<Dateipfad\>*. | Überprüfen Sie, ob die MSI auf dem virtuellen Computer aktiviert wurde. Hilfe zur Konfiguration des virtuellen Computers finden Sie unter [Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) über das Azure-Portal](msi-qs-configure-portal-windows-vm.md).<br><br>Überprüfen Sie zudem, ob Ihr HTTP GET-Anforderungs-URI richtig formatiert ist. Dies gilt vor allem für den Ressourcen-URI, der in der Abfragezeichenfolge angegeben ist. Unter „Beispiel für Anforderung“ im Abschnitt [Abrufen eines Tokens über HTTP](#get-a-token-using-http) finden Sie ein Beispiel, und unter [Azure-Dienste, die die Azure AD-Authentifizierung unterstützen](msi-overview.md#azure-services-that-support-azure-ad-authentication) finden Sie eine Liste mit Diensten und den dazugehörigen Ressourcen-IDs.

## <a name="resource-ids-for-azure-services"></a>Ressourcen-IDs für Azure-Dienste

Eine Liste mit Ressourcen, die Azure AD unterstützen und mit MSI getestet wurden, und die jeweiligen zugehörigen Ressourcen-IDs finden Sie unter [Azure-Dienste, die die Azure AD-Authentifizierung unterstützen](msi-overview.md#azure-services-that-support-azure-ad-authentication).


## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Aktivieren einer verwalteten Dienstidentität auf einer Azure-VM finden Sie unter [Konfigurieren einer durch den Benutzer zugewiesenen VM-MSI (Managed Service Identity, verwaltete Dienstidentität) mit der Azure CLI](msi-qs-configure-cli-windows-vm.md).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.








