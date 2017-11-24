---
title: "Verwenden der verwalteten Dienstidentität eines virtuellen Azure-Computers für Anmeldung und Tokenabruf"
description: "Enthält Schritt-für-Schritt-Anleitungen zur Verwendung des MSI-Dienstprinzipals eines virtuellen Azure-Computers für die Anmeldung und zum Abrufen eines Zugriffstokens."
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
ms.date: 10/17/2017
ms.author: bryanla
ms.openlocfilehash: 168b2ab3676d3f3e2830966f850e14adbe579f85
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2017
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-sign-in-and-token-acquisition"></a>Verwenden der verwalteten Dienstidentität eines virtuellen Azure-Computers für Anmeldung und Tokenabruf 
[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)] Nachdem Sie die verwaltete Dienstidentität (Managed Service Identity, MSI) auf einem virtuellen Azure-Computer aktiviert haben, können Sie sie für die Anmeldung und zum Anfordern eines Zugriffstokens verwenden. In diesem Artikel werden verschiedene Möglichkeiten beschrieben, wie Sie einen MSI-[Dienstprinzipal](develop/active-directory-dev-glossary.md#service-principal-object) für die Anmeldung nutzen und ein [App-exklusives Zugriffstoken](develop/active-directory-dev-glossary.md#access-token) abrufen, um auf andere Ressourcen zuzugreifen, z.B.:

- Im Hintergrund durchgeführte/unbeaufsichtigte Anmeldung per PowerShell oder Azure CLI
- Tokenabruf für verschiedene Clients, z.B. .NET, PowerShell, Bash/CURL, REST
- Anmeldung per Azure SDK, z.B. .NET, Java, Node.js, Python, Ruby

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

Wenn Sie die PowerShell-Beispiele in diesem Artikel verwenden möchten, sollten Sie [Azure PowerShell Version 4.3.1](https://www.powershellgallery.com/packages/AzureRM) oder höher installieren. Falls Sie die Azure CLI-Beispiele in diesem Artikel verwenden möchten, haben Sie drei Optionen:
- Verwenden Sie [Azure Cloud Shell](../cloud-shell/overview.md) über das Azure-Portal.
- Verwenden Sie die eingebettete Azure Cloud Shell, indem Sie in der oberen rechten Ecke der Azure CLI-Codeblocks auf die Schaltfläche „Ausprobieren“ klicken.
- [Installieren Sie die neueste Version von CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 oder höher), wenn Sie es vorziehen, die CLI an einer lokalen Eingabeaufforderung zu nutzen. 


> [!IMPORTANT]
> - Bei allen Beispielcodes und -skripts in diesem Artikel wird vorausgesetzt, dass der Client auf einem MSI-fähigen virtuellen Computer ausgeführt wird. Weitere Informationen zur Aktivierung von MSI auf einer VM finden Sie unter [Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) über das Azure-Portal](msi-qs-configure-portal-windows-vm.md) oder in einem der verwandten Artikel (PowerShell, CLI, Vorlage oder Azure SDK). 
> - Zur Vermeidung von Autorisierungsfehlern (403/AuthorizationFailed) in den Code-/Skriptbeispielen muss die Identität der VM den Zugriff „Leser“ auf den VM-Bereich erhalten, um Azure Resource Manager-Vorgänge auf der VM zu ermöglichen. Weitere Informationen finden Sie unter [Zuweisen des MSI-Zugriffs (Managed Service Identity, verwaltete Dienstidentität) auf eine Ressource über das Azure-Portal](msi-howto-assign-access-portal.md).
> - Verwenden Sie vor dem Fortfahren mit einem der folgenden Abschnitte im Azure-Portal das Feature „Verbinden“, um eine Remoteverbindung mit Ihrer MSI-fähigen VM herzustellen.

## <a name="how-to-sign-in-from-powershell-or-cli-using-msi"></a>Anmelden über die PowerShell oder CLI per MSI

Bisher war Folgendes erforderlich, um ein Skript unter seiner eigenen Identität auszuführen:
- Registrieren des Skripts als vertrauliche bzw. Webclientanwendung bei Azure AD
- Anmelden mit den Anmeldeinformationen der Anwendung (Client-ID/Geheimnis)

Bei Verwendung von MSI muss Ihr Skriptclient nicht mehr bei Azure AD registriert werden, und es müssen auch keine Anmeldeinformationen angegeben werden. In den folgenden Beispielen wird veranschaulicht, wie Sie den MSI-Dienstprinzipal einer VM für die Anmeldung verwenden.

### <a name="azure-powershell"></a>Azure PowerShell

Mit diesem Skript wird Folgendes veranschaulicht:

- Abrufen eines MSI-Zugriffstokens für eine Azure-VM
- Verwenden des Zugriffstokens zum Anmelden an Azure AD unter dem entsprechenden MSI-Dienstprinzipal
- Verwenden des MSI-Dienstprinzipals zum Durchführen eines Azure Resource Manager-Aufrufs, um die ID des Dienstprinzipals abzurufen

```powershell
# Get an access token from MSI
$response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token `
                              -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
$content =$response.Content | ConvertFrom-Json
$access_token = $content.access_token

# Use the access token to sign in under the MSI service principal
Login-AzureRmAccount -AccessToken $access_token -AccountId “CLIENT”

# The MSI service principal is now signed in for this session.
# Next, a call to Azure Resource Manager is made to get the service principal ID for the VM's MSI. 
$spID = (Get-AzureRMVM -ResourceGroupName <RESOURCE-GROUP> -Name <VM-NAME>).identity.principalid
echo "The MSI service principal ID is $spID"
```
   
### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Mit diesem Skript wird Folgendes veranschaulicht:

- Anmelden an Azure AD unter dem MSI-Dienstprinzipal der VM
- Verwenden des MSI-Dienstprinzipals zum Durchführen eines Azure Resource Manager-Aufrufs, um die ID des Dienstprinzipals abzurufen

```azurecli-interactive
az login --msi
spID=$(az resource list -n <VM-NAME> --query [*].identity.principalId --out tsv)
echo The MSI service principal ID is $spID
```

## <a name="how-to-acquire-an-access-token-from-msi"></a>Abrufen eines Zugriffstokens per MSI

Bei Nutzung von MSI muss Ihre Clientanwendung bzw. das Clientskript nicht mehr bei Azure AD registriert werden, und es müssen auch nicht die Client-ID und das Geheimnis angegeben werden, um ein Zugriffstoken abzurufen. Ihr Client kann ein Zugriffstoken einfach vom lokalen MSI-Endpunkt abfragen, ohne Anmeldeinformationen für die Anwendung anzugeben. Das App-exklusive Zugriffstoken wird für den MSI-Dienstprinzipal ausgestellt und ist geeignet für die Nutzung als Bearertoken in [Dienst-zu-Dienst-Aufrufen, für die Clientanmeldeinformationen benötigt werden](active-directory-protocols-oauth-service-to-service.md).

In den Beispielen unten wird gezeigt, wie Sie die MSI einer VM für den Tokenabruf verwenden.

### <a name="net"></a>.NET

> [!IMPORTANT]
> Die Azure AD Authentication Library (ADAL) ist nicht in MSI integriert. Senden Sie eine Anforderung an den lokalen MSI-Endpunkt einer VM, um per MSI ein Zugriffstoken zu erhalten. Weitere Informationen finden Sie unter [FAQs und bekannte Probleme mit der verwalteten Dienstidentität (Managed Service Identity, MSI) für Azure Active Directory](msi-known-issues.md#frequently-asked-questions-faqs).

```csharp
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

### <a name="azure-powershell-token"></a>Azure PowerShell

```powershell
# Get an access token from MSI
$response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token `
                              -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
$content =$response.Content | ConvertFrom-Json
$access_token = $content.access_token
echo "The MSI access token is $access_token"
```

### <a name="bashcurl"></a>Bash/CURL

```bash
response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

### <a name="rest"></a>REST

Beispiel für eine Anforderung:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F HTTP/1.1
Metadata: true
```

| Element | Beschreibung |
| ------- | ----------- |
| `GET` | Das HTTP-Verb, mit dem angegeben wird, dass Sie Daten vom Endpunkt abrufen möchten. In diesem Fall ist dies ein OAuth-Zugriffstoken. | 
| `http://localhost:50342/oauth2/token` | Der MSI-Endpunkt, bei dem 50342 der Standardport ist und der konfiguriert werden kann. |
| `resource` | Ein Abfragezeichenfolgenparameter, der den App-ID-URI der Zielressource angibt. Er wird auch im Anspruch `aud` (audience) des ausgestellten Tokens angezeigt. In diesem Beispiel fordern wir ein Token für den Zugriff auf Azure Resource Manager an, der über den App-ID-URI „https://management.azure.com/“ verfügt. |
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
| `access_token` | Das angeforderte Zugriffstoken. Beim Aufrufen einer REST-API wird das Token als „Bearertoken“ in das `Authorization`-Anforderungsheader-Feld eingebettet, damit der Aufrufer von der API authentifiziert werden kann. | 
| `refresh_token` | Wird von MSI nicht verwendet. |
| `expires_in` | Der Zeitraum in Sekunden, wie lange das Zugriffstoken ab dem Zeitpunkt der Ausstellung gültig ist, bevor es abläuft. Der Ausstellungszeitpunkt ist im Anspruch `iat` des Tokens zu finden. |
| `expires_on` | Der Zeitpunkt, zu dem das Zugriffstoken abläuft. Das Datum wird als Anzahl von Sekunden ab „1970-01-01T0:0:0Z UTC“ (entspricht dem Anspruch `exp` des Tokens) dargestellt. |
| `not_before` | Der Zeitpunkt, ab dem das Zugriffstoken wirksam ist und akzeptiert werden kann. Das Datum wird als Anzahl von Sekunden ab „1970-01-01T0:0:0Z UTC“ (entspricht dem Anspruch `nbf` des Tokens) dargestellt. |
| `resource` | Die Ressource, für die das Zugriffstoken angefordert wurde (Übereinstimmung mit dem Abfragezeichenfolgenparameter `resource` der Anforderung). |
| `token_type` | Der Typ des Tokens. In diesem Fall ein „Bearerzugriffstoken“, sodass die Ressource Zugriff auf den Bearer dieses Tokens gewähren kann. |

## <a name="how-to-use-msi-with-azure-sdk-libraries"></a>Verwenden von MSI mit Azure SDK-Bibliotheken

Azure unterstützt mit einer Reihe von [Azure SDKs](https://azure.microsoft.com/downloads) mehrere Programmierplattformen. Mehrere davon wurden aktualisiert, um die Anmeldung per MSI zu unterstützen, und enthalten entsprechende Beispiele, um die Nutzung zu demonstrieren. Diese Liste wird jeweils aktualisiert, wenn weitere Unterstützung hinzugefügt wird:

| SDK | Beispiel |
| --- | ------ | 
| .NET | [Bereitstellen einer ARM-Vorlage von einer Windows-VM mit verwalteter Dienstidentität](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core | [Aufrufen von Azure-Diensten von einer Linux-VM mit verwalteter Dienstidentität](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js| [Manage resources using Managed Service Identity](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) (Verwalten von Ressourcen per verwalteter Dienstidentität) |
| Python | [Use MSI to authenticate simply from inside a VM](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) (Verwenden von MSI zum einfachen Authentifizieren über eine VM) |
| Ruby   | [Manage resources from an MSI-enabled VM](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) (Verwalten von Ressourcen über eine MSI-fähige VM) | 

## <a name="additional-information"></a>Zusätzliche Informationen

### <a name="token-expiration"></a>Tokenablauf

Im lokalen MSI-Subsystem werden Token zwischengespeichert. Daher können Sie sie beliebig oft aufrufen, und ein Aufruf an Azure AD über das Netzwerk erfolgt nur in folgenden Fällen:
- Cachefehler aufgrund eines fehlenden Tokens im Cache
- Tokenablauf

Wenn Sie das Token in Ihrem Code zwischenspeichern, sollten Sie auf die Behandlung von Szenarien vorbereitet sein, bei denen die Ressource angibt, dass das Token abgelaufen ist.

### <a name="resource-ids-for-azure-services"></a>Ressourcen-IDs für Azure-Dienste

Eine Liste mit Diensten, die MSI unterstützen, und den dazugehörigen Ressourcen-IDs finden Sie unter [Azure-Dienste, die die Azure AD-Authentifizierung unterstützen](msi-overview.md#azure-services-that-support-azure-ad-authentication).

## <a name="troubleshooting"></a>Problembehandlung

### <a name="sign-in-or-token-acquisition-fails"></a>Fehler bei Anmeldung oder Tokenabruf

Überprüfen Sie, ob die MSI richtig aktiviert wurde. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zurück zum virtuellen Azure-Computer, und gehen Sie wie folgt vor:

- Überprüfen Sie auf der Seite „Konfiguration“, ob für die MSI-Aktivierung „Ja“ ausgewählt ist.
- Vergewissern Sie sich auf der Seite „Erweiterungen“, dass die MSI-Erweiterung erfolgreich bereitgestellt wurde.

Wenn einer dieser Punkte nicht zutrifft, müssen Sie die MSI möglicherweise erneut für die Ressource bereitstellen oder den Fehler bei der Bereitstellung beheben.

### <a name="http-request-error-responses"></a>Antworten auf HTTP-Anforderungsfehler

- *bad_request_102: Required metadata header not specified* (bad_request_102: Erforderlicher Metadatenheader nicht angegeben)

  Entweder fehlt der `Metadata`-Anforderungsheader in Ihrer Anforderung, oder er ist falsch formatiert. Der Wert muss als `true` (in Kleinbuchstaben) angegeben werden. Ein Beispiel finden Sie im [vorherigen REST-Abschnitt](#rest) unter „Beispiel für eine Anforderung“.

- *unknown: Failed to retrieve token from the Active directory. For details see logs in \<file path\>* (Unbekannt: Fehler beim Abrufen des Tokens aus Active Directory. Details in den Protokollen unter <Dateipfad>)

  Überprüfen Sie, ob Ihr HTTP GET-Anforderungs-URI richtig formatiert ist. Dies gilt vor allem für den Ressourcen-URI, der in der Abfragezeichenfolge angegeben ist. Unter „Beispiel für Anforderung“ im [vorherigen REST-Abschnitt](#rest) finden Sie ein Beispiel, und unter [Azure-Dienste, die die Azure AD-Authentifizierung unterstützen](msi-overview.md#azure-services-that-support-azure-ad-authentication) finden Sie eine Liste mit Diensten und den dazugehörigen Ressourcen-IDs.

- *unknown_source: Unknown Source \<URI\>* (unknown_source: Unbekannte Quelle <URI>)

  Stellen Sie sicher, dass Ihr HTTP GET-Anforderungs-URI richtig formatiert ist. Der Teil `scheme:host/resource-path` muss als `http://localhost:50342/oauth2/token` angegeben werden. Ein Beispiel finden Sie im [vorherigen REST-Abschnitt](#rest) unter „Beispiel für eine Anforderung“.

## <a name="related-content"></a>Verwandte Inhalte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](msi-overview.md).
- Informationen zum Aktivieren von MSI auf einem virtuellen Azure-Computer finden Sie unter [Konfigurieren einer Azure-VM-MSI (Managed Service Identity, verwaltete Dienstidentität) mithilfe von PowerShell](msi-qs-configure-powershell-windows-vm.md).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.

