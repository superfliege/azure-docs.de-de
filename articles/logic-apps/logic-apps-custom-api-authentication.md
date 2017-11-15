---
title: "Hinzufügen der Authentifizierung zu benutzerdefinierten APIs: Azure Logic Apps | Microsoft-Dokumentation"
description: "Informationen zum Einrichten der Authentifizierung für Aufrufe Ihrer benutzerdefinierten APIs in Logik-Apps"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 2528f4318d92bbfdc1008795876f0240a5e3e4f6
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2017
---
# <a name="secure-calls-to-your-custom-apis-from-logic-apps"></a>Schützen von Aufrufen Ihrer benutzerdefinierten APIs in Logik-Apps

Um Aufrufe Ihrer APIs zu schützen, können Sie die Azure Active Directory-Authentifizierung (Azure AD) über das Azure-Portal einrichten, sodass Sie Ihren Code nicht aktualisieren müssen. Alternativ können Sie die Authentifizierung über den API-Code anfordern oder erzwingen.

## <a name="authentication-options-for-your-api"></a>Authentifizierungsoptionen für Ihre API

Sie können Aufrufe Ihrer benutzerdefinierten API auf folgende Arten sichern:

* [Keine Änderungen am Code](#no-code): Schützen Sie Ihre API mit [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) über das Azure-Portal, damit Sie Ihren Code nicht aktualisieren bzw. Ihre API nicht bereitstellen müssen.

  > [!NOTE]
  > Bei der Azure AD-Authentifizierung, die Sie im Azure-Portal aktivieren, wird standardmäßig keine fein abgestufte Autorisierung durchgeführt. Beispielsweise sperrt diese Authentifizierung Ihre API nur für einen bestimmten Mandanten und nicht für einen bestimmten Benutzer oder eine App. 

* [Aktualisieren Ihres API-Codes](#update-code): Schützen Sie Ihre API, indem Sie die [Zertifikatsauthentifizierung](#certificate), [Standardauthentifizierung](#basic), oder [Azure AD-Authentifizierung](#azure-ad-code) über Code erzwingen.

<a name="no-code"></a>

### <a name="authenticate-calls-to-your-api-without-changing-code"></a>Authentifizieren von Aufrufen Ihrer API ohne Codeänderungen

Im Folgenden sind die allgemeinen Schritte für diese Methode aufgeführt:

1. Erstellen Sie zwei Azure Active Directory-Anwendungsidentitäten (Azure AD): eine für Ihre Logik-App und eine für Ihre Web-App (oder API-App).

2. Verwenden Sie für die Authentifizierung von Aufrufen Ihrer API die Anmeldeinformationen (Client-ID und Geheimnis) für den Dienstprinzipal, welcher der Azure AD-Anwendungsidentität für Ihre Logik-App zugeordnet ist.

3. Nehmen Sie die Anwendungs-IDs in die Definition der Logik-App auf.

#### <a name="part-1-create-an-azure-ad-application-identity-for-your-logic-app"></a>Teil 1: Erstellen einer Azure AD-Anwendungsidentität für die Logik-App

Die Logik-App verwendet diese Azure AD-Anwendungsidentität zum Authentifizieren bei Azure AD. Sie müssen diese Identität für Ihr Verzeichnis nur einmal einrichten. Sie können z.B. die gleiche Identität für alle Logik-Apps verwenden, Sie können aber auch pro Logik-App eindeutige Identitäten erstellen. Sie können diese Identitäten im Azure-Portal oder über [PowerShell](#powershell) einrichten.

**Erstellen der Anwendungsidentität für die Logik-App im Azure-Portal**

1. Wählen Sie im [Azure-Portal](https://portal.azure.com "https://portal.azure.com") den Eintrag **Azure Active Directory** aus. 

2. Vergewissern Sie sich, dass Sie sich in demselben Verzeichnis befinden, in dem auch die Web-App oder die API-App enthalten ist.

   > [!TIP]
   > Klicken Sie auf Ihr Profil, und wählen Sie ein anderes Verzeichnis aus, um zwischen Verzeichnissen zu wechseln. Wählen Sie alternativ **Übersicht** > **Verzeichnis wechseln** aus.

3. Wählen Sie im Verzeichnismenü unter **Verwalten** den Eintrag **App-Registrierungen** > **Neue Anwendungsregistrierung** aus.

   > [!TIP]
   > In der Liste mit den App-Registrierungen werden standardmäßig alle Registrierungen in Ihrem Verzeichnis angezeigt. Aktivieren Sie **Meine Apps** neben dem Suchfeld, um nur Ihre App-Registrierungen anzuzeigen. 

   ![Erstellen einer neuen App-Registrierung](./media/logic-apps-custom-api-authentication/new-app-registration-azure-portal.png)

4. Benennen Sie die Anwendungsidentität, lassen Sie **Anwendungstyp** auf **Web-App/API** festgelegt, stellen Sie für **Anmelde-URL** eine eindeutige, als Domäne formatierte Zeichenfolge bereit, und wählen Sie **Erstellen** aus.

   ![Bereitstellen eines Namens und einer Anmelde-URL für die Anwendungsidentität](./media/logic-apps-custom-api-authentication/logic-app-identity-azure-portal.png)

   Die Anwendungsidentität, die Sie für Ihre Logik-App erstellt haben, wird jetzt in der Liste der App-Registrierungen angezeigt.

   ![Anwendungsidentität für die Logik-App](./media/logic-apps-custom-api-authentication/logic-app-identity-created.png)

5. Wählen Sie aus der Liste der App-Registrierungen Ihre neue Anwendungsidentität aus. Kopieren und speichern Sie die **Anwendungs-ID**, die in Teil 3 als „Client-ID“ für Ihre Logik-App verwendet werden soll.

   ![Kopieren und Speichern der Anwendungs-ID für die Logik-App](./media/logic-apps-custom-api-authentication/logic-app-application-id.png)

6. Wählen Sie **Einstellungen** oder **Alle Einstellungen** aus, wenn die Einstellungen für Ihre Anwendungsidentität nicht sichtbar sind.

7. Wählen Sie unter **API-Zugriff** den Eintrag **Schlüssel** aus. Geben Sie unter **Beschreibung** einen Namen für den Schlüssel an. Wählen Sie unter **Läuft ab** eine Dauer für den Schlüssel aus.

   Der von Ihnen erstellte Schlüssel fungiert als „Geheimnis“ bzw. Kennwort der Anwendungsidentität für die Logik-App.

   ![Erstellen des Schlüssels für die Identität der Logik-App](./media/logic-apps-custom-api-authentication/create-logic-app-identity-key-secret-password.png)

8. Wählen Sie auf der Symbolleiste **Speichern** aus. Unter **Wert** wird jetzt Ihr Schlüssel angezeigt. 
**Achten Sie darauf, dass Sie Ihren Schlüssel für eine spätere Verwendung kopieren und speichern**, da der Schlüssel beim Verlassen der Seite **Schlüssel** ausgeblendet ist.

   Wenn Sie Ihre Logik-App in Teil 3 konfigurieren, geben Sie diesen Schlüssel als „Geheimnis“ oder Kennwort an.

   ![Kopieren und speichern Sie den Schlüssel für eine spätere Verwendung](./media/logic-apps-custom-api-authentication/logic-app-copy-key-secret-password.png)

<a name="powershell"></a>

**Erstellen der Anwendungsidentität für Ihre Logik-App in PowerShell**

Sie können diese Aufgabe über den Azure Ressource Manager mit PowerShell ausführen. Führen Sie in PowerShell folgende Befehle aus:

1. `Switch-AzureMode AzureResourceManager`

2. `Add-AzureAccount`

3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://mydomain.tld" -IdentifierUris "http://mydomain.tld" -Password "identity-password"`

4. Achten Sie darauf, dass Sie die **Mandanten-ID** (GUID für Ihren Azure AD-Mandanten), die **Anwendungs-ID** und das von Ihnen verwendete Kennwort kopieren.

Weitere Informationen finden Sie unter [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md).

#### <a name="part-2-create-an-azure-ad-application-identity-for-your-web-app-or-api-app"></a>Teil 2: Erstellen einer Azure AD-Anwendungsidentität für Ihre Web-App oder API-App

Wenn Ihre Web-App oder API-App bereits bereitgestellt wurde, können Sie die Authentifizierung aktivieren und die Anwendungsidentität im Azure-Portal erstellen. Andernfalls können Sie die [Authentifizierung bei der Bereitstellung mit einer Azure Resource Manager-Vorlage aktivieren](#authen-deploy). 

**Erstellen der Anwendungsidentität und Aktivieren der Authentifizierung im Azure-Portal für bereitgestellte Apps**

1. Suchen Sie im [Azure-Portal](https://portal.azure.com "https://portal.azure.com") Ihre Web-App oder API-App und wählen diese aus. 

2. Wählen Sie unter **Einstellungen** die Option **Authentifizierung/Autorisierung** aus. Unter **App Service Authentication** können Sie die Authentifizierung **aktivieren**. Klicken Sie unter **Authentifizierungsanbieter** auf **Azure Active Directory**.

   ![Aktivieren der Authentifizierung](./media/logic-apps-custom-api-authentication/custom-web-api-app-authentication.png)

3. Nun erstellen Sie eine Anwendungsidentität für Ihre Web-App oder API-App, wie im Folgenden dargestellt. Legen Sie auf der Seite **Azure Active Directory-Einstellungen** den **Verwaltungsmodus** auf **Express** fest. Wählen Sie **Neue AD-App erstellen** aus. Geben Sie Ihrer Anwendungsidentität einen Namen, und wählen Sie **OK** aus. 

   ![Erstellen der Anwendungsidentität für Ihre Web-App oder API-App](./media/logic-apps-custom-api-authentication/custom-api-application-identity.png)

4. Wählen Sie auf der Seite **Authentifizierung/Autorisierung** die Option **Speichern** aus.

Jetzt müssen Sie die Client-ID und die Mandanten-ID für die Anwendungsidentität suchen, die Ihrer Web-App oder API-App zugeordnet ist. Sie verwenden diese IDs in Teil 3. Fahren Sie mit diesen Schritten für das Azure-Portal fort.

**Suchen der Client-ID und der Mandanten-ID der Anwendungsidentität für Ihre Web-App oder API-App im Azure-Portal**

1. Klicken Sie unter **Authentifizierungsanbieter** auf **Azure Active Directory**. 

   ![Wählen Sie „Azure Active Directory“ aus](./media/logic-apps-custom-api-authentication/custom-api-app-identity-client-id-tenant-id.png)

2. Legen Sie auf der Seite **Azure Active Directory-Einstellungen** den **Verwaltungsmodus** auf **Erweitert** fest.

3. Kopieren Sie die **Client-ID**, und speichern Sie diese GUID für die Verwendung in Teil 3.

   > [!TIP] 
   > Wenn **Client-ID** und **Aussteller-URL** nicht angezeigt werden, versuchen Sie, das Azure-Portal zu aktualisieren, und wiederholen Sie Schritt 1.

4. Kopieren und speichern Sie unter **Aussteller-URL** nur die GUID für Teil 3. Sie können diese GUID bei Bedarf auch in der Bereitstellungsvorlage Ihrer Web-App oder API-App verwenden.

   Diese GUID ist Ihre spezifische Mandanten-GUID („Mandanten-ID“) und sollte in dieser URL angezeigt werden:`https://sts.windows.net/{GUID}`

5. Schließen Sie die Seite **Azure Active Directory-Einstellungen**, ohne Ihre Änderungen zu speichern.

<a name="authen-deploy"></a>

**Aktivieren der Authentifizierung bei der Bereitstellung mit einer Azure Ressource Manager-Vorlage**

Sie müssen weiterhin eine Azure AD-Anwendungsidentität für Ihre Web-App oder API-App erstellen, die sich von der Anwendungsidentität Ihrer Logik-App unterscheidet. Führen Sie zur Erstellung der Anwendungsidentität die vorherigen Schritte in Teil 2 für das Azure-Portal aus. 

Sie können auch die Schritte in Teil 1 ausführen. Dann sollten Sie jedoch sicherstellen, dass Ihre Web-App oder API-App tatsächlich für `https://{URL}` **Anmelde-URL** und **App-ID-URI** verwendet wird. In diesen Schritten müssen Sie die Client-ID und die Mandanten-ID für die Verwendung in der Bereitstellungsvorlage Ihrer App und auch für Teil 3 speichern.

> [!NOTE]
> Wenn Sie die Azure AD-Anwendungsidentität für Ihre Web-App oder API-App erstellen, müssen Sie anstelle von PowerShell das Azure-Portal verwenden. Mit dem PowerShell-Cmdlet werden nicht die erforderlichen Berechtigungen für die Anmeldung der Benutzer bei einer Website eingerichtet.

Sobald Sie über die Client-ID und die Mandanten-ID verfügen, nehmen Sie diese IDs als untergeordnete Ressourcen Ihrer Web-App oder API-App in Ihre Bereitstellungsvorlage auf:

``` json
"resources": [ {
    "apiVersion": "2015-08-01",
    "name": "web",
    "type": "config",
    "dependsOn": ["[concat('Microsoft.Web/sites/','parameters('webAppName'))]"],
    "properties": {
        "siteAuthEnabled": true,
        "siteAuthSettings": {
            "clientId": "{client-ID}",
            "issuer": "https://sts.windows.net/{tenant-ID}/",
        }
    }
} ]
```

Für die automatische Bereitstellung einer leeren Web-App und einer Logik-App zusammen mit einer Azure Active Directory-Authentifizierung müssen Sie die [vollständige Vorlage hier anzeigen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json) oder hier auf **Bereitstellung in Azure** klicken:

[![Bereitstellen in Azure](media/logic-apps-custom-api-authentication/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

#### <a name="part-3-populate-the-authorization-section-in-your-logic-app"></a>Teil 3: Ausfüllen des Abschnitts für die Autorisierung in Ihrer Logik-App

In der obigen Vorlage ist der Abschnitt für die Autorisierung bereits eingerichtet, aber wenn Sie die Logik-App direkt erstellen, müssen Sie den gesamten Abschnitt für die Autorisierung aufnehmen.

Öffnen Sie die Definition Ihrer Logik-App in der Codeansicht, wechseln Sie zum Abschnitt **HTTP-Aktion**, suchen Sie den Abschnitt **Autorisierung**, und fügen Sie folgende Zeile ein:

`{"tenant": "{tenant-ID}", "audience": "{client-ID-from-Part-2-web-app-or-API app}", "clientId": "{client-ID-from-Part-1-logic-app}", "secret": "{key-from-Part-1-logic-app}", "type": "ActiveDirectoryOAuth" }`

| Element | Erforderlich | Beschreibung | 
| ------- | -------- | ----------- | 
| tenant | Ja | Die GUID für den Azure AD-Mandanten | 
| audience | Ja | Die GUID für die Zielressource, auf die Sie zugreifen möchten. Dies ist die Client-ID der Anwendungsidentität für Ihre Web-App oder API-App | 
| clientId | Ja | Die GUID für den Client, der darauf zugreifen möchte. Dies ist die Client-ID der Anwendungsidentität für Ihre Logik-App | 
| secret | Ja | Der Schlüssel oder das Kennwort der Anwendungsidentität für den Client, der das Zugriffstoken anfordert | 
| Typ | Ja | Der Authentifizierungstyp. Für die ActiveDirectoryOAuth-Authentifizierung lautet der Wert `ActiveDirectoryOAuth`. | 
|||| 

Beispiel:

``` json
{
   "actions": {
      "some-action": {
         "conditions": [],
         "inputs": {
            "method": "post",
            "uri": "https://your-api-azurewebsites.net/api/your-method",
            "authentication": {
               "tenant": "tenant-ID",
               "audience": "client-ID-from-azure-ad-app-for-web-app-or-api-app",
               "clientId": "client-ID-from-azure-ad-app-for-logic-app",
               "secret": "key-from-azure-ad-app-for-logic-app",
               "type": "ActiveDirectoryOAuth"
            }
         },
      }
   }
}
```

<a name="update-code"></a>

### <a name="secure-api-calls-through-code"></a>Sichere API-Aufrufe über Code

<a name="certificate"></a>

#### <a name="certificate-authentication"></a>Zertifikatauthentifizierung

Sie können Clientzertifikate verwenden, um die eingehenden Anforderungen von Ihrer Logik-App für Ihre Web-App oder API-App zu überprüfen. Weitere Informationen zum Einrichten des Codes finden Sie unter [Konfigurieren der gegenseitigen TLS-Authentifizierung](../app-service/app-service-web-configure-tls-mutual-auth.md).

Schließen Sie im Abschnitt **Autorisierung** folgende Zeile ein: 

`{"type": "clientcertificate", "password": "password", "pfx": "long-pfx-key"}`

| Element | Erforderlich | Beschreibung | 
| ------- | -------- | ----------- | 
| Typ | Ja | Der Authentifizierungstyp. Für SSL-Clientzertifikate muss der Wert `ClientCertificate` lauten. | 
| password | Ja | Das Kennwort für den Zugriff auf das Clientzertifikat (PFX-Datei) | 
| pfx | Ja | Der base64-codierte Inhalt des Clientzertifikats (PFX-Datei) | 
|||| 

<a name="basic"></a>

#### <a name="basic-authentication"></a>Standardauthentifizierung

Sie können die Standardauthentifizierung, z.B. einen Benutzernamen und ein Kennwort, verwenden, um eingehende Anforderungen von der Logik-App für Ihre Web-App oder API-App zu überprüfen. Die Standardauthentifizierung ist ein gängiges Muster, und Sie können diese Authentifizierung in jeder Sprache verwenden, mit der Ihre Web-App oder API-App erstellt wird.

Schließen Sie im Abschnitt **Autorisierung** folgende Zeile ein:

`{"type": "basic", "username": "username", "password": "password"}`.

| Element | Erforderlich | Beschreibung | 
| ------- | -------- | ----------- | 
| Typ | Ja | Der Authentifizierungstyp, den Sie verwenden möchten. Für die Standardauthentifizierung muss der Wert `Basic` lauten. | 
| username | Ja | Der Benutzername, den Sie für die Authentifizierung verwenden möchten | 
| password | Ja | Das Kennwort, das Sie für die Authentifizierung verwenden möchten | 
|||| 

<a name="azure-ad-code"></a>

#### <a name="azure-active-directory-authentication-through-code"></a>Azure Active Directory-Authentifizierung über Code

Bei der Azure AD-Authentifizierung, die Sie im Azure-Portal aktivieren, wird standardmäßig keine fein abgestufte Autorisierung durchgeführt. Beispielsweise sperrt diese Authentifizierung Ihre API nur für einen bestimmten Mandanten und nicht für einen bestimmten Benutzer oder eine App. 

Extrahieren Sie den Header, der das JSON Web Token (JWT) enthält, um den API-Zugriff auf die Logik-App über Code zu beschränken. Überprüfen Sie die Identität des Aufrufers, und lehnen Sie Anforderungen ab, die nicht übereinstimmen.

<!-- Going further, to implement this authentication entirely in your own code, 
and not use the Azure portal, learn how to 
[authenticate with on-premises Active Directory in your Azure app](../app-service/app-service-authentication-overview.md).

To create an application identity for your logic app and use that identity to call your API, 
you must follow the previous steps. -->

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen und Aufrufen benutzerdefinierter APIs in Logik-App-Workflows](../logic-apps/logic-apps-custom-api-host-deploy-call.md)