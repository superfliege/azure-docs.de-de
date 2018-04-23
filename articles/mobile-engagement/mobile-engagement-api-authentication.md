---
title: Authentifizieren mit Mobile Engagement-REST-APIs
description: Beschreibt die Authentifizierung mit Azure Mobile Engagement-REST-APIs
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: da82cb36-957a-4e19-a805-b44733cf6597
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: 52843c926248627e4e530612cfe9326d4302eb95
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Authentifizieren mit Mobile Engagement-REST-APIs
> [!IMPORTANT]
> Azure Mobile Engagement wird am 31.3.2018 außer Kraft gesetzt. Diese Seite wird kurz danach gelöscht.
> 

## <a name="overview"></a>Übersicht

In diesem Dokument erfahren Sie, wie Sie ein gültiges Azure AD-OAuth-Token (Active Directory) zur Authentifizierung mit den Mobile Engagement-REST-APIs beziehen.

Hierbei wird vorausgesetzt, dass Sie über ein gültiges Azure-Abonnement verfügen und mithilfe eines der [Tutorials für Entwickler](mobile-engagement-windows-store-dotnet-get-started.md) eine Mobile Engagement-App erstellt haben.

## <a name="authentication"></a>Authentifizierung

Zur Authentifizierung wird ein Microsoft Azure Active Directory-basiertes OAuth-Token verwendet. 

Für die Authentifizierung einer API-Anforderung muss jeder Anforderung ein Autorisierungsheader hinzugefügt werden. Der Autorisierungsheader hat folgendes Format:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

> [!NOTE]
> Azure Active Directory-Token sind eine Stunde gültig.
> 
> 

Es gibt mehrere Möglichkeiten, ein Token zu beziehen. Da die APIs über einen Clouddienst aufgerufen werden, verwenden Sie einen API-Schlüssel. In der Azure-Terminologie werden API-Schlüssel als Dienstprinzipalkennwort bezeichnet. Das folgende Verfahren beschreibt eine Möglichkeit zum manuellen Einrichten des Kennworts.

### <a name="one-time-setup-using-a-script"></a>Einmalige Einrichtung (per Skript)

Führen Sie die Schritte in der folgenden Anleitung aus, um die Einrichtung unter Verwendung eines PowerShell-Skripts durchzuführen. Mit einem PowerShell-Skript lässt sich die Einrichtung zwar am schnellsten durchführen, es werden jedoch die lockersten Standards verwendet. 

Sie können optional auch die Anleitung aus der [manuellen Einrichtung](mobile-engagement-api-authentication-manual.md) verwenden und die Einrichtung direkt über das Azure-Portal vornehmen. Über das Azure-Portal können Sie eine detailliertere Konfiguration durchführen.

1. Laden Sie die [neueste Version von Azure PowerShell](http://aka.ms/webpi-azps) herunter. Weitere Informationen zu den Downloadanweisungen finden Sie in [dieser Übersicht](/powershell/azure/overview).

2. Vergewissern Sie sich nach der Installation von Azure PowerShell mithilfe folgender Befehle, dass das **Azure-Modul** installiert ist:

    a. Stellen Sie sicher, dass das Azure PowerShell-Modul in der Liste der verfügbaren Module enthalten ist.

        Get-Module –ListAvailable

    ![Verfügbare Azure-Module][1]

    b. Sollte das Azure PowerShell-Modul in der obigen Liste nicht enthalten sein, führen Sie den folgenden Befehl aus:

        Import-Module Azure
3. Führen Sie den folgenden Befehl aus, um sich über PowerShell bei Azure Resource Manager anzumelden. Geben Sie den Benutzernamen und das Kennwort für Ihr Azure-Konto an: 

        Connect-AzureRmAccount
4. Falls Sie über mehrere Abonnements verfügen, führen Sie folgende Schritte aus:

    a. Rufen Sie eine Liste mit allen Ihren Abonnements ab. Kopieren Sie dann den Wert von **SubscriptionId** für das Abonnement, das Sie verwenden möchten. Vergewissern Sie sich, dass das Abonnement über die Mobile Engagement-App verfügt. Diese App wird später für die Interaktion mit den APIs verwendet. 

        Get-AzureRmSubscription

    b. Führen Sie den folgenden Befehl aus: Geben Sie den Wert von **SubscriptionId** an, um das zu verwendende Abonnement zu konfigurieren:

        Select-AzureRmSubscription –SubscriptionId <subscriptionId>
5. Kopieren Sie den Text des Skripts [New-AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) auf Ihren lokalen Computer. Speichern Sie ihn anschließend als PowerShell-Cmdlet (beispielsweise `APIAuth.ps1`), und führen Sie es aus.

         `.\APIAuth.ps1`.

6. Das Skript fordert Sie zu einer Eingabe für **principalName** auf. Geben Sie einen geeigneten Namen ein, den Sie für Ihre Active Directory-Anwendung verwenden möchten (Beispiel: APIAuth). 

7. Nach Abschluss der Skriptausführung werden die vier folgenden Werte angezeigt. Kopieren Sie diese Werte. Sie werden zur programmgesteuerten Authentifizierung mit Active Directory benötigt: 

   - **TenantId**
   - **SubscriptionId**
   - **ApplicationId**
   - **Geheimnis**

   Verwenden Sie „TenantId“ als `{TENANT_ID}`, „ApplicationId“ als `{CLIENT_ID}` und „Secret“ als `{CLIENT_SECRET}`.

   > [!NOTE]
   > Ihre Standardsicherheitsrichtlinie verhindert möglicherweise das Ausführen von PowerShell-Skripts. Führen Sie in diesem Fall den folgenden Befehl aus, um die Ausführungsrichtlinie vorübergehend so zu konfigurieren, dass die Skriptausführung zugelassen wird:
   > 
   > Set-ExecutionPolicy RemoteSigned
8. Die Gruppe von PowerShell-Cmdlets sieht wie folgt aus:
    ![PowerShell-Cmdlets][3]
9. Wechseln Sie im Azure-Portal zu Active Directory, klicken Sie auf **App-Registrierungen**, und suchen Sie nach Ihrer App, um sich zu vergewissern, dass sie vorhanden ist.
    ![Suchen nach Ihrer App][4]

### <a name="steps-to-get-a-valid-token"></a>Schritte zum Abrufen eines gültigen Tokens

1. Rufen Sie die API mit den folgenden Parametern auf. Ersetzen Sie dabei **TENANT\_ID**, **CLIENT\_ID** und **CLIENT\_SECRET**:
   
   * **Anforderungs-URL** als `https://login.microsoftonline.com/{TENANT_ID}/oauth2/token`

   * **HTTP-Header „Content-Type“** als `application/x-www-form-urlencoded`
   
   * **HTTP-Anforderungstext** als `grant_type=client\_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&resource=https%3A%2F%2Fmanagement.core.windows.net%2F`
     
    Im Folgenden finden Sie eine Beispielanforderung:
    ```
    POST /{TENANT_ID}/oauth2/token HTTP/1.1
    Host: login.microsoftonline.com
    Content-Type: application/x-www-form-urlencoded
    grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso
    urce=https%3A%2F%2Fmanagement.core.windows.net%2F
    ```
    Hier sehen Sie eine Beispielantwort:
    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Content-Length: 1234

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144
    5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}
    ```
     Dieses Beispiel enthält die URL-Codierung der POST-Parameter, in der der Wert `resource` tatsächlich `https://management.core.windows.net/` lautet. Verwenden Sie auch für `{CLIENT_SECRET}` die URL-Codierung, da der Wert Sonderzeichen enthalten kann.

     > [!NOTE]
     > Zu Testzwecken können Sie ein HTTP-Client-Tool wie [Fiddler](http://www.telerik.com/fiddler) oder die [Postman-Erweiterung](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) für Chrome verwenden. 
     > 
     > 
2. Schließen Sie in jeden API-Aufruf den Autorisierungsanforderungsheader ein:
   
        Authorization: Bearer {ACCESS_TOKEN}
   
    Falls Ihre Anforderung den Statuscode 401 zurückgibt, überprüfen Sie den Antworttext. Dort erfahren Sie möglicherweise, dass das Token abgelaufen ist. In diesem Fall rufen Sie ein neues Token ab.

## <a name="use-the-apis"></a>Verwenden der APIs
Da Sie jetzt über ein gültiges Token verfügen, sind Sie bereit für die API-Aufrufe.

1. In jeder API-Anforderung muss ein gültiges, nicht abgelaufenes Token übergeben werden. Das nicht abgelaufene Token haben Sie im vorherigen Abschnitt bezogen.

2. Fügen Sie einige Parameter in den Anforderungs-URI ein, der Ihre Anwendung identifiziert. Der Anforderungs-URI sieht wie folgender Code aus:
   
        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/
   
    Wählen Sie zum Abrufen der Parameter Ihren Anwendungsnamen aus. Klicken Sie dann auf **Dashboard**. Daraufhin wird eine Seite mit allen drei Parametern angezeigt:
   
   * **1**`{subscription-id}`
   * **2**`{app-collection}`
   * **3**`{app-resource-name}`
   * **4** Ihr Ressourcengruppenname ist **MobileEngagement**, sofern Sie keinen neuen erstellt haben. 

> [!NOTE]
> Ignorieren Sie die API-Stammadresse, da diese für die vorherigen APIs galt.
> 
> Wenn Sie die App über das Azure-Portal erstellt haben, müssen Sie den Namen der Anwendungsressource verwenden, der sich vom Namen der App unterscheidet. Wenn Sie die App im Azure-Portal erstellt haben, sollten Sie den App-Namen verwenden. (Bei Apps, die im neuen Portal erstellt werden, wird nicht zwischen Anwendungsressourcenname und App-Name unterschieden.)
> 
> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/search-app.png



