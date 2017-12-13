---
title: "Verwenden der verwalteten Dienstidentität eines virtuellen Azure-Computers zum Abrufen eines Zugriffstokens"
description: "Schrittanweisungen und Beispiele zur Verwendung der verwalteten Dienstidentität eines virtuellen Azure-Computers zum Abrufen eines OAuth-Zugriffstokens."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: bryanla
ms.openlocfilehash: 20ab8c9ceeb28c92b02a50ab03c9a7fa3fb7c7b7
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2017
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-token-acquisition"></a>Verwenden der verwalteten Dienstidentität (Managed Service Identity, MSI) eines virtuellen Azure-Computers für den Tokenabruf 

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)] Dieser Artikel enthält verschiedene Code- und Skriptbeispiele für den Tokenabruf sowie eine Anleitung zu wichtigen Themen, z.B. zur Behandlung bei Tokenablauf und HTTP-Fehlern.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

Wenn Sie die Azure PowerShell-Beispiele in diesem Artikel verwenden möchten, müssen Sie die neueste Version von [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) installieren.


> [!IMPORTANT]
> - Bei allen Beispielcodes und -skripts in diesem Artikel wird vorausgesetzt, dass der Client auf einem MSI-fähigen virtuellen Computer ausgeführt wird. Verwenden Sie die Funktion „Verbinden“ im Azure-Portal zum Herstellen einer Remoteverbindung mit Ihrem virtuellen Computer. Weitere Informationen zur Aktivierung von MSI auf einer VM finden Sie unter [Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) über das Azure-Portal](msi-qs-configure-portal-windows-vm.md) oder in einem der verwandten Artikel (PowerShell, CLI, Vorlage oder Azure SDK). 

## <a name="overview"></a>Übersicht

Eine Clientanwendung kann ein [App-exklusives Zugriffstoken](develop/active-directory-dev-glossary.md#access-token) der verwalteten Dienstidentität für den Zugriff auf eine bestimmte Ressource anfordern. Das Token [basiert auf dem MSI-Dienstprinzipal](msi-overview.md#how-does-it-work). Daher muss sich der Client nicht selbst registrieren, um ein Zugriffstoken unter seinem eigenen Dienstprinzipal abzurufen. Das Token ist geeignet für die Nutzung als Bearertoken in [Dienst-zu-Dienst-Aufrufen, für die Clientanmeldeinformationen benötigt werden](active-directory-protocols-oauth-service-to-service.md).

|  |  |
| -------------- | -------------------- |
| [Abrufen eines Tokens über HTTP](#get-a-token-using-http) | Protokolldetails für den MSI-Tokenendpunkt |
| [Abrufen eines Tokens über C#](#get-a-token-using-c) | Beispiel zur Verwendung des MSI-REST-Endpunkts über einen C#-Client |
| [Abrufen eines Tokens über Go](#get-a-token-using-go) | Beispiel zur Verwendung des MSI-REST-Endpunkts über einen Go-Client |
| [Abrufen eines Tokens über Azure PowerShell](#get-a-token-using-azure-powershell) | Beispiel zur Verwendung des MSI-REST-Endpunkts über einen PowerShell-Client |
| [Abrufen eines Tokens über cURL](#get-a-token-using-curl) | Beispiel zur Verwendung des MSI-REST-Endpunkts über einen Bash/cURL-Client |
| [Behandlung bei Tokenablauf](#handling-token-expiration) | Anleitung zur Behandlung abgelaufener Zugriffstoken |
| [Fehlerbehandlung](#error-handling) | Anleitung zur Behandlung von vom MSI-Tokenendpunkt zurückgegebenen HTTP-Fehlern |
| [Ressourcen-IDs für Azure-Dienste](#resource-ids-for-azure-services) | Abrufen von Ressourcen-IDs für unterstützte Azure-Dienste |

## <a name="get-a-token-using-http"></a>Abrufen eines Tokens über HTTP 

Die grundlegende Schnittstelle zum Abrufen eines Zugriffstokens basiert auf REST, sodass sie für alle auf dem virtuellen Computer ausgeführten Clientanwendungen, die HTTP-REST-Aufrufe ausführen können, zur Verfügung steht. Dies ähnelt dem Azure AD-Programmiermodell, mit der Ausnahme, dass der Client einen Localhost-Endpunkt auf dem virtuellen Computer verwendet (und keinen Azure AD-Endpunkt).

Beispiel für eine Anforderung:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F HTTP/1.1
Metadata: true
```

| Element | Beschreibung |
| ------- | ----------- |
| `GET` | Das HTTP-Verb, mit dem angegeben wird, dass Sie Daten vom Endpunkt abrufen möchten. In diesem Fall ist dies ein OAuth-Zugriffstoken. | 
| `http://localhost:50342/oauth2/token` | Der MSI-Endpunkt, bei dem 50342 der Standardport ist und der konfiguriert werden kann. |
| `resource` | Ein Abfragezeichenfolgenparameter, der den App-ID-URI der Zielressource angibt. Er wird auch im Anspruch `aud` (audience) des ausgestellten Tokens angezeigt. In diesem Beispiel wird ein Token für den Zugriff auf Azure Resource Manager angefordert, das über den App-ID-URI „https://management.azure.com/“ verfügt. |
| `Metadata` | Ein HTTP-Anforderungsheader-Feld, das für MSI als Maßnahme gegen SSRF-Angriffe (Server Side Request Forgery) erforderlich ist. Dieser Wert muss auf „true“ (in Kleinbuchstaben) festgelegt werden.

Beispiel für eine Antwort:

```
HTTP/1.1 200 OK
Content-Type: application/json
{
  "access_token": "eyJ0eXAi...",
  "refresh_token": "",
  "expires_in": "3599",
  "expires_on": "1506484173",
  "not_before": "1506480273",
  "resource": "https://management.azure.com/",
  "token_type": "Bearer"
}
```

| Element | Beschreibung |
| ------- | ----------- |
| `access_token` | Das angeforderte Zugriffstoken. Beim Aufrufen einer geschützten REST-API wird das Token als „Bearertoken“ in das `Authorization`-Anforderungsheader-Feld eingebettet, damit der Aufrufer von der API authentifiziert werden kann. | 
| `refresh_token` | Wird von MSI nicht verwendet. |
| `expires_in` | Der Zeitraum in Sekunden, wie lange das Zugriffstoken ab dem Zeitpunkt der Ausstellung gültig ist, bevor es abläuft. Der Ausstellungszeitpunkt ist im Anspruch `iat` des Tokens zu finden. |
| `expires_on` | Der Zeitpunkt, zu dem das Zugriffstoken abläuft. Das Datum wird als Anzahl von Sekunden ab „1970-01-01T0:0:0Z UTC“ (entspricht dem Anspruch `exp` des Tokens) dargestellt. |
| `not_before` | Der Zeitpunkt, ab dem das Zugriffstoken wirksam ist und akzeptiert werden kann. Das Datum wird als Anzahl von Sekunden ab „1970-01-01T0:0:0Z UTC“ (entspricht dem Anspruch `nbf` des Tokens) dargestellt. |
| `resource` | Die Ressource, für die das Zugriffstoken angefordert wurde (Übereinstimmung mit dem Abfragezeichenfolgenparameter `resource` der Anforderung). |
| `token_type` | Der Typ des Tokens. In diesem Fall ein „Bearerzugriffstoken“, sodass die Ressource Zugriff auf den Bearer dieses Tokens gewähren kann. |

## <a name="get-a-token-using-c"></a>Abrufen eines Tokens über C#

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net;
using System.Web.Script.Serialization; 

// Build request to acquire MSI token
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://localhost:50342/oauth2/token?resource=https://management.azure.com/");
request.Headers["Metadata"] = "true";
request.Method = "GET";

try
{
    // Call /token endpoint
    HttpWebResponse response = (HttpWebResponse)request.GetResponse();

    // Pipe response Stream to a StreamReader, and extract access token
    StreamReader streamResponse = new StreamReader(response.GetResponseStream()); 
    string stringResponse = streamResponse.ReadToEnd();
    JavaScriptSerializer j = new JavaScriptSerializer();
    Dictionary<string, string> list = (Dictionary<string, string>) j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
    string accessToken = list["access_token"];
}
catch (Exception e)
{
    string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
}

```

## <a name="get-a-token-using-go"></a>Abrufen eines Tokens über Go

```
package main

import (
  "fmt"
  "io/ioutil"
  "net/http"
  "net/url"
  "encoding/json"
)

type responseJson struct {
  AccessToken string `json:"access_token"`
  RefreshToken string `json:"refresh_token"`
  ExpiresIn string `json:"expires_in"`
  ExpiresOn string `json:"expires_on"`
  NotBefore string `json:"not_before"`
  Resource string `json:"resource"`
  TokenType string `json:"token_type"`
}

func main() {
    
    // Create HTTP request for MSI token to access Azure Resource Manager
    var msi_endpoint *url.URL
    msi_endpoint, err := url.Parse("http://localhost:50342/oauth2/token")
    if err != nil {
      fmt.Println("Error creating URL: ", err)
      return 
    }
    msi_parameters := url.Values{}
    msi_parameters.Add("resource", "https://management.azure.com/")
    msi_endpoint.RawQuery = msi_parameters.Encode()
    req, err := http.NewRequest("GET", msi_endpoint.String(), nil)
    if err != nil {
      fmt.Println("Error creating HTTP request: ", err)
      return 
    }
    req.Header.Add("Metadata", "true")

    // Call MSI /token endpoint
    client := &http.Client{}
    resp, err := client.Do(req) 
    if err != nil{
      fmt.Println("Error calling token endpoint: ", err)
      return
    }

    // Pull out response body
    responseBytes,err := ioutil.ReadAll(resp.Body)
    defer resp.Body.Close()
    if err != nil {
      fmt.Println("Error reading response body : ", err)
      return
    }

    // Unmarshall response body into struct
    var r responseJson
    err = json.Unmarshal(responseBytes, &r)
    if err != nil {
      fmt.Println("Error unmarshalling the response:", err)
      return
    }

    // Print HTTP response and marshalled response body elements to console
    fmt.Println("Response status:", resp.Status)
    fmt.Println("access_token: ", r.AccessToken)
    fmt.Println("refresh_token: ", r.RefreshToken)
    fmt.Println("expires_in: ", r.ExpiresIn)
    fmt.Println("expires_on: ", r.ExpiresOn)
    fmt.Println("not_before: ", r.NotBefore)
    fmt.Println("resource: ", r.Resource)
    fmt.Println("token_type: ", r.TokenType)
}
```

## <a name="get-a-token-using-azure-powershell"></a>Abrufen eines Tokens über Azure PowerShell

Im folgenden Beispiel wird veranschaulicht, wie Sie den MSI-REST-Endpunkt über einen PowerShell-Client für Folgendes verwenden:

1. Abrufen eines Zugriffstokens
2. Verwenden des Zugriffstokens, um eine Azure Resource Manager-REST-API aufzurufen und Informationen zum virtuellen Computer abzurufen Achten Sie darauf, dass Sie `<SUBSCRIPTION-ID>`, `<RESOURCE-GROUP>` und `<VM-NAME>` jeweils durch Ihre Abonnement-ID, den Namen der Ressourcengruppe und den Namen des virtuellen Computers ersetzen.

```azurepowershell
# Get an access token for the MSI
$response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token `
                              -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
$content =$response.Content | ConvertFrom-Json
$access_token = $content.access_token
echo "The MSI access token is $access_token"

# Use the access token to get resource information for the VM
$vmInfoRest = (Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Compute/virtualMachines/<VM-NAME>?api-version=2017-12-01 -Method GET -ContentType "application/json" -Headers @{ Authorization ="Bearer $access_token"}).content
echo "JSON returned from call to get VM info:"
echo $vmInfoRest

```

## <a name="get-a-token-using-curl"></a>Abrufen eines Tokens über cURL

```bash
response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

## <a name="handling-token-expiration"></a>Vorgehen bei Tokenablauf

Im lokalen MSI-Subsystem werden Token zwischengespeichert. Daher können Sie sie beliebig oft aufrufen, und ein Aufruf an Azure AD über das Netzwerk erfolgt nur in folgenden Fällen:
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

| Element | Beschreibung |
| ------- | ----------- |
| error   | Fehler-ID |
| error_description | Ausführliche Beschreibung des Fehlers. **Fehlerbeschreibungen können sich jederzeit ändern. Schreiben Sie keinen Code, der sich basierend auf Werten in der Fehlerbeschreibung brancht.**|

### <a name="http-response-reference"></a>Referenz für HTTP-Antworten

In diesem Abschnitt sind die möglichen Fehlerantworten aufgeführt. Der Status „200 OK“ ist eine erfolgreiche Antwort, und das Zugriffstoken ist im JSON-Antworttext im Element „access_token“ enthalten.

| Statuscode | Error | Fehlerbeschreibung | Lösung |
| ----------- | ----- | ----------------- | -------- |
| 400 – Ungültige Anforderung | invalid_resource | AADSTS50001: Die Anwendung *\<URI\>* wurde im Mandanten *\<TENANT-ID\>* nicht gefunden. Dies kann auftreten, wenn die Anwendung nicht vom Administrator des Mandanten installiert wurde oder wenn sie von den Benutzern des Mandanten keine Zustimmung erhalten hat. Unter Umständen haben Sie Ihre Authentifizierung an den falschen Mandanten gesendet. | (Nur Linux) |
| 400 – Ungültige Anforderung | bad_request_102 | Erforderlicher Metadatenheader nicht angegeben | Entweder fehlt der `Metadata`-Anforderungsheader in Ihrer Anforderung, oder er ist falsch formatiert. Der Wert muss als `true` (in Kleinbuchstaben) angegeben werden. Ein Beispiel finden Sie im [vorherigen REST-Abschnitt](#rest) unter „Beispiel für eine Anforderung“.|
| 401 – Nicht autorisiert | unknown_source | Unbekannte Quelle *\<URI\>* | Stellen Sie sicher, dass Ihr HTTP GET-Anforderungs-URI richtig formatiert ist. Der Teil `scheme:host/resource-path` muss als `http://localhost:50342/oauth2/token` angegeben werden. Ein Beispiel finden Sie im [vorherigen REST-Abschnitt](#rest) unter „Beispiel für eine Anforderung“.|
|           | invalid_request | In der Anforderung fehlt ein erforderlicher Parameter, ist ein ungültiger Parameter enthalten oder ist ein Parameter mehrfach vorhanden, oder die Anforderung ist auf andere Weise fehlerhaft. |  |
|           | unauthorized_client | Der Client ist zum Anfordern eines Zugriffstokens mit dieser Methode nicht autorisiert. | Ursache ist eine Anforderung, die keinen lokalen Loopback zum Aufrufen der Erweiterung verwendet hat, oder auf einem virtuellen Computer, auf dem keine verwaltete Dienstidentität ordnungsgemäß konfiguriert ist. Hilfe zur Konfiguration des virtuellen Computers finden Sie unter [Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) über das Azure-Portal](msi-qs-configure-portal-windows-vm.md). |
|           | access_denied | Der Ressourcenbesitzer oder Autorisierungsserver hat die Anforderung verweigert. |  |
|           | unsupported_response_type | Der Autorisierungsserver unterstützt das Abrufen eines Zugriffstokens mit dieser Methode nicht. |  |
|           | invalid_scope | Der angeforderte Bereich ist ungültig, unbekannt oder falsch formatiert. |  |
| 500 Interner Serverfehler | unknown | Beim Abrufen des Tokens aus Active Directory ist ein Fehler aufgetreten. Details finden Sie in den Protokollen unter *\<Dateipfad\>*. | Überprüfen Sie, ob die MSI auf dem virtuellen Computer aktiviert wurde. Hilfe zur Konfiguration des virtuellen Computers finden Sie unter [Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) über das Azure-Portal](msi-qs-configure-portal-windows-vm.md).<br><br>Überprüfen Sie zudem, ob Ihr HTTP GET-Anforderungs-URI richtig formatiert ist. Dies gilt vor allem für den Ressourcen-URI, der in der Abfragezeichenfolge angegeben ist. Unter „Beispiel für Anforderung“ im [vorherigen REST-Abschnitt](#rest) finden Sie ein Beispiel, und unter [Azure-Dienste, die die Azure AD-Authentifizierung unterstützen](msi-overview.md#azure-services-that-support-azure-ad-authentication) finden Sie eine Liste mit Diensten und den dazugehörigen Ressourcen-IDs.

## <a name="resource-ids-for-azure-services"></a>Ressourcen-IDs für Azure-Dienste

Eine Liste mit Ressourcen, die Azure AD unterstützen und mit MSI getestet wurden, und die jeweiligen zugehörigen Ressourcen-IDs finden Sie unter [Azure-Dienste, die die Azure AD-Authentifizierung unterstützen](msi-overview.md#azure-services-that-support-azure-ad-authentication).


## <a name="related-content"></a>Verwandte Inhalte

- Informationen zum Aktivieren von MSI auf einem virtuellen Azure-Computer finden Sie unter [Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) über das Azure-Portal](msi-qs-configure-portal-windows-vm.md).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.








