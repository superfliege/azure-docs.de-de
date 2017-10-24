---
title: "Sichern benutzerdefinierter Connectors mit Azure AD – Azure Logic Apps | Microsoft-Dokumentation"
description: "Hinzufügen von Sicherheit und Authentifizierung zu Ihrer API und dem Connector mithilfe von Azure AD (Azure Active Directory)"
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
ms.openlocfilehash: bcee842cb880002daaa3ae9d9110ee1734941b6d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="add-security-to-your-api-and-connector-with-azure-active-directory-azure-ad"></a>Hinzufügen von Sicherheit zu Ihrer API und dem Connector mithilfe von Azure AD (Azure Active Directory)

Wenn Sie Aufrufe zwischen Ihrer API und dem benutzerdefinierten Connector sichern möchten, fügen Sie die Azure AD-Authentifizierung zu Ihrer Web-API und dem Connector hinzu. Für dieses Szenario erstellen Sie zwei Azure AD-Apps (Azure Active Directory): mit einer Azure AD-App wird Ihre Web-API gesichert, während mit der anderen Azure AD-App die Registrierung Ihres Connectors gesichert und delegierter Zugriff hinzugefügt wird. 

In diesem Tutorial wird die Azure Resource Manager-API als Beispiel verwendet. Mit Azure Resource Manager können Sie die Komponenten einer Lösung verwalten, die Sie in Azure erstellt haben, wie z.B. Datenbanken, virtuelle Computer und Web-Apps. Ein benutzerdefinierter Connector für Azure Resource Manager ist möglicherweise nützlich, wenn Sie Azure-Ressourcen über Ihre Workflows verwalten möchten. Weitere Informationen finden Sie unter [Übersicht über den Azure-Ressourcen-Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Falls Sie über kein Abonnement verfügen, erstellen Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/). Registrieren Sie sich andernfalls für ein [Abonnement mit nutzungsbasierter Bezahlung](https://azure.microsoft.com/pricing/purchase-options/).

* Für dieses Tutorial die [OpenAPI-Beispieldatei für Azure Resource Manager](https://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json)

  > [!NOTE] 
  > In der OpenAPI-Beispieldatei werden nicht alle Vorgänge in Azure Resource Manager definiert, und sie enthält derzeit nur den Vorgang zum [Auflisten aller Abonnements](https://docs.microsoft.com/rest/api/resources/subscriptions#Subscriptions_List). Sie können diese OpenAPI bearbeiten oder mit dem [OpenAPI-Online-Editor](http://editor.swagger.io/) eine andere OpenAPI-Datei erstellen.
  >
  > Sie können dieses Tutorial auf alle RESTful-APIs anwenden, bei denen die Azure AD-Authentifizierung verwendet werden soll.

## <a name="1-create-your-first-azure-ad-app-for-securing-your-web-api"></a>1. Erstellen Sie Ihre erste Azure AD-App zum Sichern Ihrer Web-API

Ihre erste Azure AD-App führt die Authentifizierung durch, wenn Ihr benutzerdefinierter Connector Ihre API aufruft. In diesem Beispiel ist dies die Azure Resource Manager-API.

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an. Wenn Sie über mehrere Azure Active Directory-Mandanten verfügen, sollten Sie sicherstellen, dass Sie beim richtigen Verzeichnis angemeldet sind, indem Sie das Verzeichnis unter Ihrem Benutzernamen überprüfen. 

   ![Überprüfen Sie Ihr Verzeichnis](./media/custom-connector-azure-active-directory-authentication/check-user-directory.png)

   > [!TIP]
   > Wenn Sie Verzeichnisse ändern möchten, müssen Sie Ihren Benutzernamen und anschließend das gewünschte Verzeichnis auswählen.

2. Wählen Sie im Hauptmenü von Azure den Eintrag **Azure Active Directory** aus, um das aktuelle Verzeichnis anzuzeigen.

   ![Wählen Sie „Azure Active Directory“ aus](./media/custom-connector-azure-active-directory-authentication/azure-active-directory.png)

   > [!TIP]
   > Wenn der Eintrag **Azure Active Directory** im Hauptmenü von Azure nicht angezeigt wird, wählen Sie **Weitere Dienste** aus. Geben Sie „Azure Active Directory“ als Filter in das Feld **Filter** ein, und wählen Sie anschließend **Azure Active Directory** aus.

3. Wählen Sie im Verzeichnismenü unter **Verwalten** den Eintrag **App-Registrierungen** aus. Wählen Sie aus der Liste der registrierten Apps **+ Registrierung einer neuen Anwendung** aus.

   ![Klicken Sie auf „App-Registrierungen“ > „Registrierung einer neuen Anwendung“.](./media/custom-connector-azure-active-directory-authentication/add-app-registrations.png)

4. Geben Sie, wie in der Tabelle beschrieben, unter **Erstellen** die Details für Ihre Azure AD-App an, und wählen Sie anschließend **Erstellen** aus. 

   ![Erstellen der Azure AD-App](./media/custom-connector-azure-active-directory-authentication/create-app1-registration.png)

   | Einstellung | Empfohlener Wert | Beschreibung | 
   | ------- | --------------- | ----------- | 
   | **Name** | *web-api-app-name* | Der Name für die Azure AD-App Ihrer Web-API | 
   | **Anwendungstyp** | **Web-App/API** | Ihr App-Typ | 
   | **Anmelde-URL** | `https://login.windows.net` | | 
   |||| 

5. Wählen Sie Ihre Azure AD-App aus, wenn Sie zur Liste der **App-Registrierungen** Ihres Verzeichnisses zurückkehren.

   ![Auswählen Ihrer Azure AD-App aus der Liste](./media/custom-connector-azure-active-directory-authentication/app1-registration-created.png)

6. Wenn die Detailseite der App angezeigt wird, sollten Sie sicherstellen, dass Sie die **Anwendungs-ID *der App* an einem sicheren Ort**  kopieren und speichern. Sie benötigen diese ID später noch einmal.

   ![Speichern der „Anwendungs-ID“ für die spätere Verwendung](./media/custom-connector-azure-active-directory-authentication/application-id-app1.png)

7. Geben Sie nun eine Antwort-URL für Ihre Azure AD-App an. Wählen Sie im Menü **Einstellungen** der App den Eintrag **Antwort-URLs** aus. Geben Sie diese URL ein, und klicken Sie anschließend auf **Speichern**.

   ![Antwort-URLs](./media/custom-connector-azure-active-directory-authentication/add-reply-url1.png)

   | Einstellung | Empfohlener Wert | Beschreibung | 
   | ------- | --------------- | ----------- | 
   | **Antwort-URLs** | Geben Sie für Ihre eigene API folgende URL ein: </br>`https://{your-web-app-root-URL}/.auth/login/aad/callback` | | 
   | **Delegierte Berechtigungen** | {nicht erforderlich} | | 
   | **Clientschlüssel** | {nicht erforderlich} | | 
   |||| 

   > [!TIP]
   > Wenn das Menü **Einstellungen** vorher nicht angezeigt wurde, wählen Sie hier **Einstellungen** aus:
   >
   > ![Auswählen von „Einstellungen“](./media/custom-connector-azure-active-directory-authentication/show-app1-settings-menu.png)

## <a name="2-create-your-second-azure-ad-app-for-your-custom-connector"></a>2. Erstellen Ihrer zweiten Azure AD-App für Ihren benutzerdefinierten Connector

Mit Ihrer zweiten Azure AD-App wird die Registrierung Ihres benutzerdefinierten Connectors gesichert und delegierter Zugriff auf die Web-API hinzugefügt, die von der ersten Azure AD-App geschützt wird. 

> [!IMPORTANT]
> Stellen Sie sicher, dass beide Azure AD-Apps in demselben Verzeichnis enthalten sind.

1. Kehren Sie zur Liste der **App-Registrierungen** zurück, und wählen Sie erneut **+ Registrierung einer neuen Anwendung**.

   ![Klicken Sie auf „App-Registrierungen“ > „Registrierung einer neuen Anwendung“.](./media/custom-connector-azure-active-directory-authentication/add-app-registrations2.png)

2. Geben Sie, wie in der Tabelle beschrieben, unter **Erstellen** die Details für Ihre zweite Azure AD-App an, und wählen Sie anschließend **Erstellen** aus. 

   ![Erstellen der Azure AD-App](./media/custom-connector-azure-active-directory-authentication/create-app2-registration.png)

   | Einstellung | Empfohlener Wert | Beschreibung | 
   | ------- | --------------- | ----------- | 
   | **Name** | *Ihr-Connectorname* | Der Name für die Azure AD-App Ihres Connectors | 
   | **Anwendungstyp** | **Web-App/API** | Ihr App-Typ | 
   | **Anmelde-URL** | `https://login.windows.net` | | 
   |||| 

3. Wählen Sie Ihre zweite Azure AD-App aus, wenn Sie zur Liste der **App-Registrierungen** Ihres Verzeichnisses zurückkehren.

   ![Auswählen Ihrer zweiten Azure AD-App aus der Liste](./media/custom-connector-azure-active-directory-authentication/app2-registration-created.png)

4. Wenn die Detailseite der App angezeigt wird, sollten Sie sicherstellen, dass Sie die **Anwendungs-ID *der App auch* an einem sicheren Ort**  kopieren und speichern. Sie benötigen diese ID später noch einmal.

   ![Speichern der „Anwendungs-ID“ für die spätere Verwendung](./media/custom-connector-azure-active-directory-authentication/application-id-app2.png)

5. Geben Sie nun eine Antwort-URL für Ihre Azure AD-App an. Wählen Sie im Menü **Einstellungen** der App den Eintrag **Antwort-URLs** aus. Geben Sie diese URL ein, und klicken Sie anschließend auf **Speichern**.

   | Einstellung | Empfohlener Wert | 
   | ------- | --------------- | 
   | **Antwort-URLs** | Geben Sie für den benutzerdefinierten Connector von Azure Resource Manager folgende URL ein:`https://msmanaged-na.consent.azure-apim.net/redirect` | 
   ||| 

   > [!TIP]
   > Wenn das Menü **Einstellungen** vorher nicht angezeigt wurde, wählen Sie hier **Einstellungen** aus:
   >
   > ![Auswählen von „Einstellungen“](./media/custom-connector-azure-active-directory-authentication/show-app2-settings-menu.png)

6. Wenn Ihnen das Menü **Einstellungen** wieder angezeigt wird, wählen Sie **Erforderliche Berechtigungen** > **Hinzufügen** aus.

   ![Erforderliche Berechtigungen > Hinzufügen](./media/custom-connector-azure-active-directory-authentication/add-api-access1-select-permissions.png)

7. Wenn das Menü **API-Zugriff hinzufügen** geöffnet wird, wählen Sie **API auswählen** > 
**Windows Azure Service Management-API** > **Auswählen** aus.

   ![Auswählen einer API](./media/custom-connector-azure-active-directory-authentication/add-api-access2-select-api.png)

8. Wählen Sie im Menü **API-Zugriff hinzufügen** den Eintrag **Berechtigungen auswählen** aus. Wählen Sie unter **Delegierte Berechtigungen** den Eintrag **Als Organisationsbenutzer auf Azure Service Management zugreifen** > **Auswählen** aus.

   ![Auswählen von „Delegierte Berechtigungen“ > „Als Organisationsbenutzer auf Azure Services Management zugreifen“](./media/custom-connector-azure-active-directory-authentication/add-api-access3-select-permissions.png)

   Verwenden Sie andernfalls andere Optionen:

   | Option | Empfohlener Wert | Beschreibung | 
   | ------ | --------------- | ----------- | 
   | **Delegierte Berechtigungen** | | Auswählen von Berechtigungen für delegierten Zugriff auf Ihre Web-API | 
   |||| 

9. Wählen Sie nun im Menü **API-Zugriff hinzufügen** den Eintrag **Fertig** aus.

   ![Menü „API-Zugriff hinzufügen“ > „Fertig“](./media/custom-connector-azure-active-directory-authentication/add-api-access4-done.png)

10. Generieren Sie jetzt einen *Clientschlüssel* oder einen „geheimen Schlüssel“ für die Azure AD-App Ihres Connectors. 

    1. Wählen Sie **Schlüssel** aus, wenn Ihnen das Menü **Einstellungen** wieder angezeigt wird. 
    Geben Sie einen Namen für Ihren Schlüssel mit maximal 16 Zeichen an, wählen Sie ein Ablaufdatum aus, und klicken Sie anschließend auf **Speichern**.

       ![Erstellen eines Clientschlüssels](./media/custom-connector-azure-active-directory-authentication/add-key.png)

    2. Wenn Ihr generierter Schlüssel angezeigt wird, **sollten Sie sicherstellen, dass Sie diesen Schlüssel an einen sicheren Ort kopieren und speichern**, bevor Sie die Seite **Schlüssel** verlassen.
    
       ![Manuelles Kopieren und Speichern Ihres Schlüssels](./media/custom-connector-azure-active-directory-authentication/save-key.png)

11. Nachdem Sie Ihren Schlüssel gespeichert haben, können Sie das Menü**Einstellungen** jetzt schließen.

## <a name="3-add-azure-ad-authentication-to-your-web-api-app"></a>3. Hinzufügen einer Azure AD-Authentifizierung zu Ihrer Web-API-App

Jetzt können Sie die Authentifizierung für Ihre Web-API mit Ihrer ersten Azure AD-App aktivieren. Weitere Informationen finden Sie unter [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Azure Active Directory-Anmeldung](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach Ihrer Web-API-App, die Sie zuvor unter [Erstellen benutzerdefinierter Connectors über die Web-API](../logic-apps/custom-connector-build-web-api-app-tutorial.md) veröffentlicht haben.

2. Wählen Sie unter **Einstellungen** die Option **Authentifizierung/Autorisierung** aus. 

   1. Wählen sie unter **App Service-Authentifizierung** die Option **Ein** aus.
   2. Wählen Sie für **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist.** die Option **Mit Azure Active Directory anmelden** aus.
   3. Wählen Sie für **Authentifizierungsanbieter** die Option **Azure Active Directory** aus. 

   ![Auswählen von „Authentifizierung“/„Autorisierung“](./media/custom-connector-azure-active-directory-authentication/web-api-app-authentication.png)

3. Gehen Sie auf der Seite **Azure Active Directory-Einstellungen** wie folgt vor:

   1. Wählen Sie unter **Verwaltungsmodus** die Option **Express** aus.

   2. Wählen Sie jetzt die Azure AD-App aus, die Ihre Web-API-App für die Authentifizierung verwendet. 
   Wählen Sie **Vorhandene AD-App auswählen** > **Azure AD-App** aus.

   3. Wählen Sie unter **Azure AD-Anwendungen** die Azure AD-App aus, die Sie zuvor für Ihre Web-API-App erstellt haben. 
   
   4. Wählen Sie so lange **OK** aus, bis Ihnen wieder die Seite **Authentifizierung/Autorisierung** angezeigt wird.

   Beispiel:

   ![Auswählen der Azure AD-App, die Ihre Web-API-App darstellt](./media/custom-connector-azure-active-directory-authentication/web-api-app-select-azure-ad-app.png)

4. Wählen Sie auf der Seite **Authentifizierung/Autorisierung** die Option **Speichern** aus.

   ![Speichern von Authentifizierungseinstellungen](./media/custom-connector-azure-active-directory-authentication/web-api-app-azure-ad-app-save.png)

   Ihre Web-API-App kann jetzt Azure AD für die Authentifizierung verwenden.

## <a name="4-set-up-azure-ad-authentication-in-your-web-apis-openapi-file"></a>4. Einrichten Ihrer Azure AD-Authentifizierung in der OpenAPI-Datei Ihrer Web-API

Öffnen Sie die OpenAPI-Datei Ihrer Web-API-App, damit Sie das Objekt `securityDefintions` und die Azure AD-Authentifizierung unter der Eigenschaft `host` hinzufügen können. Im Folgenden wird ein Beispiel für die Eigenschaft `host` und das Objekt `securityDefinitions` dargestellt:

``` json
// Your OpenAPI file header appears here...

"host": "{your-web-api-app-root-url}",
"schemes": [
    "https" // Make sure this is https!
],
"securityDefinitions": {
    "AAD": {
        "type": "oauth2",
        "flow": "accessCode",
        "authorizationUrl": "https://login.windows.net/common/oauth2/authorize",
        "tokenUrl": "https://login.windows.net/common/oauth2/token",
        "scopes": {}
    }
},

// Your OpenAPI document continues here...
```
Jetzt können Sie Ihren benutzerdefinierten Connector erstellen und registrieren.

## <a name="next-steps"></a>Nächste Schritte

* [Register your connector](../logic-apps/logic-apps-custom-connector-register.md) (Registrieren Ihres Connectors)
* [Häufig gestellte Fragen zu benutzerdefinierten Connectors](../logic-apps/custom-connector-faq.md)
