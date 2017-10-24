---
title: Integrieren von Anwendungen in Azure Active Directory
description: "Es wird beschrieben, wie Sie eine Anwendung in Azure AD hinzufügen, aktualisieren oder sie aus Azure Active Directory (Azure AD) entfernen."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: mbaldwin
ms.assetid: ae637be5-0b71-4b1e-b1fe-b83df3eb4845
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/04/2017
ms.author: bryanla
ms.custom: aaddev
ms.reviewer: luleon
ms.openlocfilehash: d9816f53e1d0e09f938452a022c75a3d20a43da4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="integrating-applications-with-azure-active-directory"></a>Integrieren von Anwendungen in Azure Active Directory
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Unternehmensentwickler und SaaS-Anbieter (Software-as-a-Service) können kommerzielle Clouddienste oder Branchenanwendungen entwickeln, die in Azure Active Directory (Azure AD) integriert werden können, um eine sichere Anmeldung und Autorisierung für ihre Dienste bereitzustellen. Um eine Anwendung oder einen Dienst in Azure AD zu integrieren, muss ein Entwickler die Anwendung zuerst bei Azure AD registrieren.

In diesem Artikel wird veranschaulicht, wie Sie die Registrierung einer Anwendung in Azure AD hinzufügen, aktualisieren oder entfernen. Sie lernen die verschiedenen Anwendungstypen kennen, die in Azure AD integriert werden können, und erfahren, wie Sie Ihre Anwendungen für den Zugriff auf andere Ressourcen (z.B. auf Web-APIs) konfigurieren.

Weitere Informationen zu den beiden Azure AD-Objekten, die eine registrierte Anwendung darstellen, und der Beziehung zwischen ihnen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](active-directory-application-objects.md). Weitere Informationen zu den Richtlinien zum Branding bei der Anwendungsentwicklung mit Azure Active Directory finden Sie unter [Brandingrichtlinien für integrierte Apps](active-directory-branding-guidelines.md).

## <a name="adding-an-application"></a>Hinzufügen einer Anwendung
Jede Anwendung muss zunächst in einem Azure AD-Mandanten registriert werden, um die Funktionen von Azure AD nutzen zu können. Dieser Registrierungsvorgang umfasst das Angeben von Details zu Ihrer Anwendung in Azure AD. Beispielsweise muss die URL für den Speicherort angegeben werden, die URL, an die nach der Authentifizierung eines Benutzers Antworten gesendet werden sollen, die URI zum Identifizieren der App usw.

### <a name="to-register-a-new-application-using-the-azure-portal"></a>Registrieren einer neuen Anwendung mit dem Azure-Portal
1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.
2. Wenn Sie unter Ihrem Konto mehrere Zugriffsmöglichkeiten haben, können Sie oben rechts auf Ihr Konto klicken und Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten festlegen.
3. Klicken Sie im linken Navigationsbereich auf den **Azure Active Directory**-Dienst und dann auf **App-Registrierungen** und **Registrierung einer neuen Anwendung**.

   ![Registrieren einer neuen Anwendung](./media/active-directory-integrating-applications/add-app-registration.png)

4. Geben Sie auf der angezeigten Seite **Erstellen** die Registrierungsinformationen für Ihre Anwendung ein: 

  - **Name:** Geben Sie einen aussagekräftigen Anwendungsnamen ein.
  - **Anwendungstyp:** 
    - Wählen Sie für [Clientanwendungen](active-directory-dev-glossary.md#client-application), die lokal auf dem Gerät installiert sind, die Option „Native“ (Nativ). Diese Einstellung wird für öffentliche [native OAuth-Clients](active-directory-dev-glossary.md#native-client) verwendet.
    - Wählen Sie die Option „Web-App/API“ für [Clientanwendungen](active-directory-dev-glossary.md#client-application) und [Ressourcen-/API-Anwendungen](active-directory-dev-glossary.md#resource-server), die auf einem sicheren Server installiert sind. Diese Einstellung wird für vertrauliche OAuth-[Webclients](active-directory-dev-glossary.md#web-client) und für öffentliche [Clients auf Basis von Benutzer-Agents](active-directory-dev-glossary.md#user-agent-based-client) verwendet. Außerdem kann dieselbe Anwendung sowohl einen Client als auch eine Ressource/API verfügbar machen.
  - **Anmelde-URL:** Geben Sie für Anwendungen vom Typ „Web-App/API“ die Basis-URL Ihrer App an. `http://localhost:31544` kann beispielsweise die URL für eine Web-App sein, die auf einem lokalen Computer ausgeführt wird. Benutzer können diese URL nutzen, um sich an einer Webclientanwendung anzumelden. 
  - **Umleitungs-URI:** Geben Sie für Anwendungen vom Typ „Nativ“ den URI an, der von Azure AD zum Zurückgeben von Tokenantworten verwendet wird. Geben Sie einen für Ihre Anwendung spezifischen Wert ein, z.B. `http://MyFirstAADApp`.

   ![Registrieren einer neuen Anwendung – Erstellen](./media/active-directory-integrating-applications/add-app-registration-create.png)

   Spezifische Beispiele für Webanwendungen oder native Anwendungen finden Sie in unseren [Schnellstarts](active-directory-developers-guide.md#get-started).

5. Klicken Sie auf **Erstellen**, wenn Sie fertig sind. Azure AD weist Ihrer Anwendung eine eindeutige Anwendungs-ID zu, und Sie gelangen auf die Hauptseite für die Registrierung Ihrer Anwendung. Je nachdem, ob es sich bei Ihrer Anwendung um eine Web- oder eine systemeigene Anwendung handelt, werden jeweils andere Optionen zum Hinzufügen weiterer Funktionen zu Ihrer Anwendung bereitgestellt. Der nächste Abschnitt enthält eine Übersicht zum Thema „Zustimmung“ sowie Details zur Aktivierung von zusätzlichen Konfigurationsfeatures bei Ihrer Anwendungsregistrierung (Anmeldeinformationen, Berechtigungen, Aktivierung der Anmeldung für Benutzer von anderen Mandanten).

  > [!NOTE]
  > Die neu registrierte Anwendung wird standardmäßig so konfiguriert, dass sich **nur** Benutzer desselben Mandanten an Ihrer Anwendung anmelden können.
  > 
  > 

## <a name="updating-an-application"></a>Aktualisieren einer Anwendung
Nachdem Ihre Anwendung in Azure AD registriert wurde, muss sie ggf. aktualisiert werden, um Zugriff auf Web-APIs bereitzustellen, für andere Unternehmen zur Verfügung gestellt zu werden usw.  In diesem Abschnitt werden verschiedene Möglichkeiten beschrieben, wie Sie Ihre Anwendung weiter konfigurieren können. Wir beginnen mit einer Übersicht über das Zustimmungs-Framework. Damit sollten Sie vertraut sein, wenn Sie Anwendungen erstellen, die von anderen Benutzern oder Anwendungen verwendet werden müssen.

### <a name="overview-of-the-consent-framework"></a>Übersicht über das Zustimmungs-Framework

Das Azure AD-Zustimmungs-Framework vereinfacht die Entwicklung von mehrinstanzenfähigen Webanwendungen und nativen Clientanwendungen, z.B. Anwendungen mit mehreren Ebenen. Diese Anwendungen ermöglichen die Anmeldung mit Benutzerkonten über einen Azure AD-Mandanten, der sich von dem Mandanten unterscheidet, unter dem die Anwendung registriert ist. Außerdem kann der Zugriff auf Web-APIs, z.B. die Microsoft Graph-API (zum Zugreifen auf Azure Active Directory, Intune und Office 365-Dienste), und andere APIs von Microsoft-Diensten zusätzlich zu Ihren eigenen Web-APIs erforderlich sein. Das Framework basiert darauf, dass Benutzer oder Administratoren ihre Zustimmung zur Registrierung einer Anwendung in ihrem Verzeichnis erteilen. Diese Zustimmung kann auch den Zugriff auf Verzeichnisdaten umfassen.

Wenn beispielsweise eine Webclientanwendung Kalenderinformationen über einen Benutzer aus Office 365 lesen muss, muss dieser Benutzer der Clientanwendung zuerst seine Zustimmung erteilen. Anschließend kann die Clientanwendung die Microsoft Graph-API im Namen des Benutzers aufrufen und die Kalenderinformationen nach Bedarf verwenden. Die [Microsoft Graph-API](https://graph.microsoft.io) ermöglicht den Zugriff auf Daten in Office 365 (z.B. Kalender und Nachrichten aus Exchange, Websites und Listen aus SharePoint, Dokumente aus OneDrive, OneNote-Notizbücher, Aufgaben aus dem Planer, Arbeitsmappen aus Excel usw.) sowie auf Benutzer und Gruppen von Azure AD und andere Datenobjekte aus weiteren Microsoft Cloud Services. 

Das Zustimmungs-Framework (Consent Framework) basiert auf OAuth 2.0 und seinen verschiedenen Datenflüssen, z.B. Authorization Code Grant und Client Credentials Grant. Dabei kommen öffentliche oder vertrauliche Clients zum Einsatz. Durch die Verwendung von OAuth 2.0 ermöglicht Azure AD die Entwicklung zahlreicher verschiedener Typen von Clientanwendungen, z. B. für Telefon, Tablet, Server oder Web, und ermöglicht den Zugriff auf die erforderlichen Ressourcen.

Weitere Informationen zur Verwendung des Zustimmungs-Frameworks mit OAuth2.0-Autorisierungsgewährungen finden Sie unter [Autorisieren des Zugriffs auf Webanwendungen mit OAuth 2.0 und Azure Active Directory](active-directory-protocols-oauth-code.md) und [Authentifizierungsszenarien für Azure AD](active-directory-authentication-scenarios.md). Informationen dazu, wie Sie autorisierten Zugriff auf Office 365 über Microsoft Graph erhalten, finden Sie auf der Seite zur [App-Authentifizierung mit Microsoft Graph](https://graph.microsoft.io/docs/authorization/auth_overview).

#### <a name="example-of-the-consent-experience"></a>Beispiel für die Zustimmungsbenutzeroberfläche

Die folgenden Schritte zeigen, wie das Zustimmungs-Framework in der Benutzeroberfläche für den Anwendungsentwickler und für den Benutzer umgesetzt wird.

1. Angenommen, Sie verfügen über eine Webclientanwendung, die für den Zugriff auf eine Ressource oder API bestimmte Berechtigungen anfordern muss. Im nächsten Abschnitt wird beschrieben, wie Sie diese Konfiguration durchführen. Im Wesentlichen wird aber das Azure-Portal verwendet, um zur Konfigurationszeit Berechtigungsanforderungen zu deklarieren. Wie andere Konfigurationseinstellungen auch, werden diese Teil der Azure AD-Registrierung der Anwendung:
   
  ![Berechtigungen für andere Anwendungen](./media/active-directory-integrating-applications/requiredpermissions.png)
    
2. Angenommen, die Berechtigungen für Ihre Anwendung wurden aktualisiert, die Anwendung wird ausgeführt, und ein Benutzer möchte die Anwendung zum ersten Mal verwenden. Zuerst muss die Anwendung vom `/authorize`-Endpunkt von Azure AD einen Autorisierungscode erhalten. Der Autorisierungscode kann dann genutzt werden, um ein neues Zugriffs- und Aktualisierungstoken zu beschaffen.

3. Wenn der Benutzer noch nicht authentifiziert ist, wird er vom `/authorize`-Endpunkt von Azure AD zum Anmelden aufgefordert.
   
  ![Benutzer- oder Administratoranmeldung bei Azure AD](./media/active-directory-integrating-applications/usersignin.png)

4. Nachdem der Benutzer sich angemeldet hat, ermittelt Azure AD, ob für den Benutzer eine Seite für die Zustimmungserteilung angezeigt werden muss. Das Ergebnis dieser Ermittlung ist davon abhängig, ob der Benutzer (oder der Administrator seiner Organisation) die Anwendungszustimmung bereits erteilt hat. Wenn die Zustimmung noch nicht erteilt wurde, fordert Azure AD den Benutzer zur Zustimmung auf und zeigt an, welche Berechtigungen für die Funktionalität benötigt werden. Der Berechtigungssatz, der im Dialogfeld für die Zustimmung angezeigt wird, ist identisch mit der Auswahl unter „Delegierte Berechtigungen“ im Azure-Portal.
   
  ![Oberfläche für die Benutzerzustimmung](./media/active-directory-integrating-applications/consent.png)

5. Nachdem der Benutzer seine Zustimmung erteilt hat, wird ein Autorisierungscode an Ihre Anwendung zurückgegeben, mit dem ein Zugriffs- und Aktualisierungstoken abgerufen wird. Weitere Informationen zu diesem Datenfluss finden Sie im [Abschnitt „Webanwendung zu Web-API“ im Thema „Authentifizierungsszenarios für Azure AD“](active-directory-authentication-scenarios.md#web-application-to-web-api).

6. Als Administrator können Sie auch für alle Benutzer in Ihrem Mandanten den delegierten Berechtigungen einer Anwendung zustimmen. Durch die Zustimmung eines Administrators wird verhindert, dass das Zustimmungsdialogfeld für jeden Benutzer des Mandanten angezeigt wird. Dies wird auf Ihrer Anwendungsseite im [Azure-Portal](https://portal.azure.com) durchgeführt. Klicken Sie auf der Seite **Einstellungen** für Ihre Anwendung auf **Erforderliche Berechtigungen** und dann auf die Schaltfläche **Berechtigungen erteilen**. 

  ![Erteilen von Berechtigungen für explizite Administratorzustimmung](./media/active-directory-integrating-applications/grantpermissions.png)
    
  > [!NOTE]
  > Das Gewähren der expliziten Zustimmung über die Schaltfläche **Berechtigungen erteilen** ist derzeit für Single-Page-Anwendungen (SPA) erforderlich, die „ADAL.js“ nutzen. Andernfalls tritt für die Anwendung ein Fehler auf, wenn das Zugriffstoken angefordert wird.   

### <a name="configure-a-client-application-to-access-web-apis"></a>Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs
Damit eine Webanwendung oder vertrauliche Clientanwendung an einem Flow zur Autorisierungsgenehmigung teilnehmen kann, bei dem eine Authentifizierung (und das Abrufen eines Zugriffstokens) erforderlich ist, benötigt sie sichere Anmeldeinformationen. Die Standardauthentifizierungsmethode im Azure-Portal ist „Client-ID + geheimer Schlüssel“. In diesem Abschnitt werden die erforderlichen Konfigurationsschritte zum Bereitstellen des geheimen Schlüssels für die Anmeldeinformationen des Clients behandelt.

Bevor ein Client Zugriff auf eine Web-API erhält, die durch eine Ressourcenanwendung (z.B. Microsoft Graph-API) bereitgestellt wird, stellt das Zustimmungs-Framework außerdem sicher, dass dem Client die erforderlichen Berechtigungen basierend auf den angeforderten Berechtigungen erteilt werden. Standardmäßig können alle Anwendungen unter „Windows Azure Active Directory“ (Graph-API) und „Windows Azure-Dienstverwaltungs-API“ Berechtigungen auswählen. Die [Graph-API-Berechtigung „Anmelden und Benutzerprofil lesen“](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes#PermissionScopeDetails) ist standardmäßig ebenfalls ausgewählt. Wenn Ihr Client unter einem Mandanten registriert wird, der über Konten mit Abonnements von Office 365 verfügt, können auch Web-APIs und Berechtigungen für SharePoint und Exchange Online ausgewählt werden. Sie können für jede gewünschte Web-API zwischen [zwei Arten von Berechtigungen](active-directory-dev-glossary.md#permissions) wählen:

- Anwendungsberechtigungen: Ihre Clientanwendung benötigt direkten Zugriff auf die Web-API selbst (kein Benutzerkontext). Für diese Art von Berechtigung ist die Zustimmung des Administrators erforderlich, sie steht nicht für systemeigene Clientanwendungen zur Verfügung.

- Delegierte Berechtigungen: Ihre Clientanwendung benötigt als angemeldeter Benutzer Zugriff auf die Web-API. Der Zugriff ist jedoch durch die ausgewählte Berechtigung eingeschränkt. Diese Art von Berechtigung kann von einem Benutzer erteilt werden, sofern für die Berechtigung nicht die Zustimmung durch einen Administrator erforderlich ist. 

  > [!NOTE]
  > Durch Hinzufügen einer delegierten Berechtigung zu einer Anwendung erteilen Sie den Benutzern im Mandanten nicht automatisch Ihre Zustimmung wie zuvor im klassischen Azure-Portal. Benutzer müssen den hinzugefügten delegierten Berechtigungen bei der Ausführung noch manuell zustimmen, sofern der Administrator nicht auf die Schaltfläche **Erteilen von Berechtigungen** im Abschnitt **Erforderliche Berechtigungen** der Anwendungsseite im Azure-Portal klickt. 

#### <a name="to-add-application-credentials-or-permissions-to-access-web-apis"></a>So fügen Sie Anmeldeinformationen für Anwendungen oder Zugriffsberechtigungen für Web-APIs hinzu
1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.
2. Wenn Sie unter Ihrem Konto mehrere Zugriffsmöglichkeiten haben, können Sie oben rechts auf Ihr Konto klicken und Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten festlegen.
3. Klicken Sie im linken Navigationsbereich auf den **Azure Active Directory**-Dienst und dann auf **App-Registrierungen**. Suchen Sie anschließend nach der Anwendung, die Sie konfigurieren möchten, und klicken Sie darauf.

   ![Aktualisieren der Registrierung einer Anwendung](./media/active-directory-integrating-applications/update-app-registration.png)

4. Sie gelangen auf die Hauptseite der Anwendungsregistrierung, und die Seite **Einstellungen** für die Anwendung wird geöffnet. Gehen Sie wie folgt vor, um einen geheimen Schlüssel für die Anmeldeinformationen Ihrer Webanwendung hinzuzufügen:
  - Klicken Sie auf der Seite **Einstellungen** auf den Abschnitt **Schlüssel**.  
  - Fügen Sie eine Beschreibung für Ihren Schlüssel hinzu.
  - Wählen Sie entweder eine Dauer von ein oder zwei Jahren aus.
  - Klicken Sie auf **Speichern**. Die Spalte ganz rechts enthält nach dem Speichern der Konfigurationsänderungen den Schlüsselwert. **Kopieren Sie den Schlüssel** zur Verwendung im Code Ihrer Clientanwendung, da er nach dem Verlassen dieser Seite nicht mehr zugänglich ist.

  ![Aktualisieren der Registrierung einer Anwendung – Schlüssel](./media/active-directory-integrating-applications/update-app-registration-settings-keys.png)

5. So fügen Sie Berechtigungen für den Zugriff auf Ressourcen-APIs von Ihrem Client hinzu
  - Klicken Sie auf der Seite **Einstellungen** auf den Abschnitt **Erforderliche Berechtigungen**. 
  - Klicken Sie auf die Schaltfläche **Hinzufügen** .
  - Klicken Sie auf **Hiermit wählen Sie eine API aus.**, um den Typ der Ressourcen für die Auswahl anzugeben.
  - Durchsuchen Sie die Liste der verfügbaren APIs, oder verwenden Sie das Suchfeld, um die verfügbaren Ressourcenanwendungen in Ihrem Verzeichnis auszuwählen, die eine Web-API verfügbar machen. Klicken Sie auf die Ressource, an der Sie interessiert sind, und dann auf **Wählen**.
  - Sie gelangen auf die Seite **Zugriff aktivieren**. Wählen Sie die Anwendungsberechtigungen bzw. die delegierten Berechtigungen aus, die Ihre Anwendung für den Zugriff auf die API benötigt.
   
  ![Aktualisieren der Registrierung einer Anwendung – API für Berechtigungen](./media/active-directory-integrating-applications/update-app-registration-settings-permissions-api.png)

  ![Aktualisieren der Registrierung einer Anwendung – Berechtigungen](./media/active-directory-integrating-applications/update-app-registration-settings-permissions-perms.png)

6. Klicken Sie abschließend auf der Seite **Zugriff aktivieren** auf die Schaltfläche **Auswählen** und dann auf der Seite **API-Zugriff hinzufügen** auf die Schaltfläche **Fertig**. Sie gelangen zurück auf die Seite **Erforderliche Berechtigungen**, auf der Sie sehen, dass die neue Ressource der Liste mit den APIs hinzugefügt wurde.

  > [!NOTE]
  > Wenn Sie auf die Schaltfläche **Fertig** klicken, werden basierend auf den von Ihnen konfigurierten Berechtigungen für andere Anwendungen automatisch die Berechtigungen für die Anwendung in Ihrem Verzeichnis festgelegt.  Sie können diese Anwendungsberechtigungen auf der Seite **Einstellungen** anzeigen.
  > 
  > 

### <a name="configuring-a-resource-application-to-expose-web-apis"></a>Konfigurieren einer Ressourcenanwendung zum Bereitstellen von Web-APIs

Sie können eine Web-API entwickeln und sie Clientanwendungen zur Verfügung stellen, indem Sie [Zugriffsbereiche](active-directory-dev-glossary.md#scopes) und [Rollen](active-directory-dev-glossary.md#roles) verfügbar machen. Eine ordnungsgemäß konfigurierte Web-API wird auf die gleiche Weise wie andere Microsoft-Web-APIs (einschließlich der Graph-API und der Office 365-APIs) zur Verfügung gestellt. Zugriffsbereiche und Rollen werden durch Ihr [Anwendungsmanifest](active-directory-dev-glossary.md#application-manifest) verfügbar gemacht. Hierbei handelt es sich um eine JSON-Datei, die die Identitätskonfiguration Ihrer Anwendung repräsentiert. 

Im folgenden Abschnitt erfahren Sie, wie Sie Zugriffsbereiche verfügbar machen, indem Sie das Manifest der Ressourcenanwendung bearbeiten.

#### <a name="adding-access-scopes-to-your-resource-application"></a>Hinzufügen von Zugriffsbereichen zu Ihrer Ressourcenanwendung

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.
2. Wenn Sie unter Ihrem Konto mehrere Zugriffsmöglichkeiten haben, können Sie oben rechts auf Ihr Konto klicken und Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten festlegen.

3. Klicken Sie im linken Navigationsbereich auf den **Azure Active Directory**-Dienst und dann auf **App-Registrierungen**. Suchen Sie anschließend nach der Anwendung, die Sie konfigurieren möchten, und klicken Sie darauf.

   ![Aktualisieren der Registrierung einer Anwendung](./media/active-directory-integrating-applications/update-app-registration.png)

4. Sie gelangen auf die Hauptseite der Anwendungsregistrierung, und die Seite **Einstellungen** für die Anwendung wird geöffnet. Wechseln Sie auf die Seite **Manifest bearbeiten**, indem Sie auf der Registrierungsseite der Anwendung auf **Manifest** klicken. Ein webbasierter Manifest-Editor wird geöffnet, mit dem Sie das Manifest im Portal bearbeiten können (**Bearbeiten**). Optional können Sie auf **Herunterladen** klicken und dann die Option **Hochladen** verwenden, um das Manifest wieder auf Ihre Anwendung anzuwenden.

5. In diesem Beispiel wird der neue Bereich `Employees.Read.All` auf unserer Ressource/API verfügbar gemacht, indem der Sammlung`oauth2Permissions` das folgende JSON-Element hinzugefügt wird: Der vorhandene Bereich `user_impersonation` wird während der Registrierung standardmäßig bereitgestellt. Mit `user_impersonation` kann eine Clientanwendung die Berechtigung zum Zugreifen auf die Ressource mit der Identität des angemeldeten Benutzers anfordern. Achten Sie darauf, dass Sie das Komma nach dem vorhandenen Bereichselement `user_impersonation` hinzufügen und die Eigenschaftswerte gemäß den Anforderungen Ihrer Ressource ändern. 

  ```json
  {
    "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
    "adminConsentDisplayName": "Read-only access to Employee records",
    "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
    "isEnabled": true,
    "type": "User",
    "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
    "userConsentDisplayName": "Read-only access to your Employee records",
    "value": "Employees.Read.All"
  }
  ```
  > [!NOTE]
  > Der Wert „id“ muss generiert werden, indem Sie ein GUID-Generierungstool wie [guidgen]](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx) verwenden oder programmgesteuert vorgehen. Die GUID repräsentiert einen eindeutigen Bezeichner für den Bereich, der von der Web-API bereitgestellt wird. Nachdem ein Client mit Berechtigungen zum Zugreifen auf Ihre Web-API richtig konfiguriert wurde, wird dafür von Azure AD ein OAuth 2.0-Zugriffstoken ausgestellt. Wenn der Client die Web-API aufruft, wird das Zugriffstoken präsentiert, für das der Bereichsanspruch (scp) auf die Berechtigungen festgelegt ist, die in der dazugehörigen Anwendungsregistrierung angefordert werden.
  >
  > Sie können später bei Bedarf zusätzliche Bereiche verfügbar machen. Berücksichtigen Sie, dass Ihre Web-API verschiedene Berechtigungen verfügbar machen kann, die einer Vielzahl von unterschiedlichen Funktionen zugeordnet sind. Ihre Ressource kann den Zugriff auf die Web-API zur Laufzeit steuern, indem die Bereichsansprüche (`scp`) im erhaltenen OAuth 2.0-Zugriffstoken ausgewertet werden.
  > 

6. Klicken Sie abschließend auf **Speichern**. Ihre Web-API ist jetzt so konfiguriert, dass sie von anderen Anwendungen im Verzeichnis verwendet werden kann.  

  ![Aktualisieren der Registrierung einer Anwendung](./media/active-directory-integrating-applications/update-app-registration-manifest.png)

#### <a name="verify-the-web-api-is-exposed-to-other-applications-in-your-tenant"></a>Überprüfen, ob die Web-API für andere Anwendungen in Ihrem Mandanten verfügbar gemacht wird
1. Wechseln Sie zurück zu Ihrem Azure AD-Mandanten, klicken Sie erneut auf **App-Registrierungen**, und suchen Sie nach der Clientanwendung, die Sie konfigurieren möchten. Klicken Sie auf diese Clientanwendung.

   ![Aktualisieren der Registrierung einer Anwendung](./media/active-directory-integrating-applications/update-app-registration.png)

2. Wiederholen Sie Schritt 5 wie unter [Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs](#configure-a-client-application-to-access-web-apis). Suchen Sie im Schritt **API auswählen** nach Ihrer Ressource, indem Sie ihren Anwendungsnamen im Suchfeld eingeben und auf **Auswählen** klicken. 

3. Auf der Seite **Zugriff aktivieren** sollte der neue Bereich angezeigt werden, der für Clientberechtigungsanforderungen verfügbar ist.

  ![Neue Berechtigungen werden angezeigt](./media/active-directory-integrating-applications/update-app-registration-settings-permissions-perms-newscopes.png)

#### <a name="more-on-the-application-manifest"></a>Weitere Informationen zum Anwendungsmanifest

Das Anwendungsmanifest dient als Mechanismus zum Aktualisieren der Anwendungsentität, der alle Attribute der Identitätskonfiguration einer Azure AD-Anwendung einschließlich der erörterten API-Zugriffsbereiche definiert. Weitere Informationen zur Anwendungsentität und zum dazugehörigen Schema finden Sie in der [Dokumentation zur Anwendungsentität der Graph-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity). Der Artikel enthält umfassende Referenzinformationen zu den Anwendungsentitätselementen, die verwendet werden, um Berechtigungen für Ihre API anzugeben, z.B.:  

- Das appRoles-Mitglied, bei es sich um eine Sammlung von [AppRole](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type)-Entitäten handelt, die zum Definieren der [Anwendungsberechtigungen](active-directory-dev-glossary.md#permissions) für eine Web-API verwendet werden. 
- Das oauth2Permissions-Mitglied, bei dem es sich um eine Sammlung von [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type)-Entitäten handelt, die zum Definieren der [delegierten Berechtigungen](active-directory-dev-glossary.md#permissions) für eine Web-API verwendet werden.

Weitere Informationen zu Anwendungsmanifestkonzepten im Allgemeinen finden Sie unter [Grundlegendes zum Azure Active Directory-Anwendungsmanifest](active-directory-application-manifest.md).

### <a name="accessing-the-azure-ad-graph-and-office-365-via-microsoft-graph-apis"></a>Zugreifen auf Azure AD-Graph-API und Office 365 über Microsoft Graph-APIs  

Wie zuvor bereits erwähnt, können Sie zusätzlich zum Verfügbarmachen von/Zugreifen auf APIs für Ihre eigenen Anwendungen Ihre Clientanwendung für den Zugriff auf APIs aktualisieren, die von Microsoft-Ressourcen verfügbar gemacht werden. Die Microsoft Graph-API, die im Portal in der Liste mit den Ressourcen/APIs als „Microsoft Graph“ bezeichnet wird, ist für alle Anwendungen verfügbar, die mit Azure AD registriert werden. Wenn Sie Ihre Clientanwendung in einem Mandanten mit Konten registrieren, für die die Registrierung für ein Office 365-Abonnement durchgeführt wurde, können Sie auch auf die Bereiche zugreifen, die von den verschiedenen Office 365-Ressourcen verfügbar gemacht werden.

Eine vollständige Erläuterung der Bereiche, die von der Microsoft Graph-API verfügbar gemacht werden, finden Sie im Artikel [Berechtigungsbereiche | Graph-API-Konzepte](https://graph.microsoft.io/docs/authorization/permission_scopes).

> [!NOTE]
> Aufgrund einer aktuellen Einschränkung können systemeigene Clientanwendungen die Azure AD Graph-API nur aufrufen, wenn sie die Berechtigung "Zugriff auf das Verzeichnis Ihrer Organisation" verwenden. Diese Einschränkung gilt nicht für Webanwendungen.
> 
> 

### <a name="configuring-multi-tenant-applications"></a>Konfigurieren von mehrinstanzenfähigen Anwendungen

Wenn Sie eine Anwendung in Azure AD registrieren, möchten Sie ggf. erreichen, dass auf Ihre Anwendung nur durch Benutzer in Ihrer Organisation zugegriffen werden kann. Es kann auch sein, dass es nur für Benutzer in externen Organisationen möglich sein soll, auf Ihre Anwendung zuzugreifen. Diese zwei Anwendungstypen werden als Einzelinstanzanwendung bzw. als mehrinstanzenfähige Anwendung bezeichnet. In diesem Abschnitt wird beschrieben, wie Sie die Konfiguration einer Einzelinstanzanwendung so ändern, dass Sie eine mehrinstanzenfähige Anwendung erhalten.

Es ist wichtig, die Unterschiede zwischen einer Einzelinstanzanwendung und einer mehrinstanzenfähigen Anwendung zu verstehen:  

- Eine Einzelinstanzanwendung ist für die Verwendung in einem einzelnen Unternehmen gedacht. Es handelt sich dabei zumeist um eine Branchenanwendung (Line-of-Business, LOB), die von einem Unternehmensentwickler geschrieben wurde. Auf eine Anwendung mit nur einem Mandanten kann nur von Benutzern mit Konten unter demselben Mandanten wie die Anwendungsregistrierung zugegriffen werden. Daher ist die Bereitstellung in nur einem Verzeichnis erforderlich.
- Eine mehrinstanzenfähige Anwendung ist für die Verwendung in vielen Organisationen vorgesehen. Sie wird als SaaS-Webanwendung (Software-as-a-Service) bezeichnet, die in der Regel von einem unabhängigen Softwarehersteller (ISV, Independent Software Vendor) geschrieben wurde. Mehrinstanzenfähige Anwendungen müssen unter jedem Mandanten bereitgestellt werden, auf den Benutzer Zugriff benötigen. Für andere Mandanten als der Mandant mit der Anwendungsregistrierung ist die Zustimmung durch Benutzer oder Administratoren erforderlich, um sie zu registrieren. Beachten Sie, dass native Clientanwendungen standardmäßig mehrinstanzenfähig sind, da sie auf dem Gerät des Ressourcenbesitzers installiert sind. Weitere Informationen zum Zustimmungs-Framework finden Sie oben in der [Übersicht über das Zustimmungs-Framework](#overview-of-the-consent-framework).

Um eine Anwendung mehrinstanzenfähig zu machen, sind sowohl Änderungen der Anwendungsregistrierung als auch Änderungen der Webanwendung selbst erforderlich. Beides wird in den folgenden Abschnitten beschrieben.

#### <a name="changing-the-application-registration-to-support-multi-tenant"></a>Ändern der Anwendungsregistrierung zur Unterstützung der Mehrinstanzenfähigkeit

Wenn Sie eine Anwendung schreiben, die Sie Ihren Kunden oder Partnern außerhalb Ihrer Organisation zur Verfügung stellen möchten, müssen Sie die Anwendungsdefinition im Azure-Portal aktualisieren.

> [!IMPORTANT]
> Für Azure AD muss der App-ID-URI von mehrinstanzenfähigen Anwendungen global eindeutig sein. Der App-ID-URI ist eine der Methoden, mit der eine Anwendung in Protokollmeldungen identifiziert wird. Bei einer Anwendung mit einem einzigen Mandanten muss der App-ID-URI nur in diesem Mandanten eindeutig sein. Bei einer mehrinstanzenfähigen Anwendung muss er global eindeutig sein, damit Azure AD die Anwendung in allen Mandanten finden kann. Globale Eindeutigkeit wird durch die Anforderung erzwungen, dass der App-ID-URI einen Hostnamen aufweisen muss, der mit einer überprüften Domäne des Azure AD-Mandanten übereinstimmt. Wenn der Name Ihres Mandanten beispielsweise „contoso.onmicrosoft.com“ lautet, ist „https://contoso.onmicrosoft.com/myapp“ ein gültiger App-ID-URI. Wenn Ihr Mandant über die verifizierte Domäne „contoso.com“ verfügt, wäre „https://contoso.com/myapp“ ebenfalls ein gültiger App-ID-URI. Wenn der App-ID-URI nicht diesem Muster folgt, schlägt das Festlegen einer Anwendung als mehrinstanzenfähig fehl.
> 

Gehen Sie wie folgt vor, um externen Benutzern den Zugriff auf Ihre Anwendung zu ermöglichen: 

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.
2. Wenn Sie unter Ihrem Konto mehrere Zugriffsmöglichkeiten haben, können Sie oben rechts auf Ihr Konto klicken und Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten festlegen.
3. Klicken Sie im linken Navigationsbereich auf den **Azure Active Directory**-Dienst und dann auf **App-Registrierungen**. Suchen Sie anschließend nach der Anwendung, die Sie konfigurieren möchten, und klicken Sie darauf. Sie gelangen auf die Hauptseite der Anwendungsregistrierung, und die Seite **Einstellungen** für die Anwendung wird geöffnet.
4. Klicken Sie auf der Seite **Einstellungen** auf **Eigenschaften**, und schieben Sie den Regler für **Mehrinstanzenfähig** auf **Ja**.

Nachdem Sie die Änderungen vorgenommen haben, können Benutzer und Administratoren in anderen Organisationen es ihren Benutzern ermöglichen, sich an Ihrer Anwendung anzumelden. So kann Ihre Anwendung auf Ressourcen zugreifen, die durch den entsprechenden Mandanten geschützt sind.

#### <a name="changing-the-application-to-support-multi-tenant"></a>Ändern der Anwendung zur Unterstützung der Mehrinstanzenfähigkeit

Die Unterstützung für mehrinstanzenfähige Anwendungen ist stark vom Zustimmungs-Framework von Azure AD abhängig. Die Zustimmung (Consent) ist das Verfahren, mit dem ein Benutzer eines anderen Mandanten der Anwendung Zugriff auf die Ressourcen gewähren kann, die durch den Mandanten des Benutzers geschützt sind. Dieser Prozess wird als „Zustimmung des Benutzers“ bezeichnet.

Ihre Webanwendung kann auch folgende Optionen anbieten:

- Die Möglichkeit „Mein Unternehmen registrieren“ für Administratoren. Dieser Prozess wird als „Zustimmung des Administrators“ bezeichnet und ermöglicht einem Administrator das Gewähren der Zustimmung im Namen *aller Benutzer* der Organisation. Nur ein Benutzer, der sich mit einem Konto authentifiziert, das zur globalen Administratorrolle gehört, kann die Zustimmung des Administrators erteilen. Andere Benutzer erhalten eine Fehlermeldung.

- Eine Registrierungsbenutzeroberfläche für Benutzer. Es wird erwartet, dass für Benutzer die Schaltfläche „Registrieren“ bereitgestellt wird, mit der der Browser an den Azure AD OAuth 2.0-Endpunkt `/authorize` oder einen OpenID Connect-Endpunkt vom Typ `/userinfo` weitergeleitet wird. Diese Endpunkte ermöglichen es der Anwendung, Informationen zum neuen Benutzer durch Untersuchen des id_token-Objekts abzurufen. Nach der Registrierungsphase wird der Benutzer, ähnlich wie im Abschnitt [Übersicht über das Zustimmungs-Framework](#overview-of-the-consent-framework) gezeigt, zum Erteilen der Zustimmung aufgefordert.

Weitere Informationen zu den Anwendungsänderungen, die zur Unterstützung von mehrinstanzenfähigen Benutzeroberflächen für Zugriff und Anmeldung/Registrierung erforderlich sind, finden Sie hier:

- [Anmelden von Azure AD-Benutzern (Active Directory) mit dem mehrinstanzenfähigen Anwendungsmuster](active-directory-devhowto-multi-tenant-overview.md)
- Liste mit [Codebeispielen für Mehrinstanzenfähigkeit](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant) 
- [Schnellstart: Hinzufügen eines Unternehmensbrandings zur Anmeldeseite in Azure AD](../customize-branding.md)

### <a name="enabling-oauth-20-implicit-grant-for-single-page-applications"></a>Aktivieren der impliziten OAuth 2.0-Gewährung für Single-Page-Anwendungen

Single-Page-Anwendungen (SPAs) basieren in der Regel auf einem JavaScript-intensiven Front-End, das im Browser ausgeführt wird. Hierüber wird das Web-API-Back-End der Anwendung zum Ausführen der Geschäftslogik aufgerufen. Für SPAs, die in Azure AD gehostet werden, verwenden Sie die implizite OAuth 2.0-Gewährung zum Authentifizieren des Benutzers für Azure AD und rufen ein Token ab, mit dem Sie Aufrufe vom JavaScript-Client der Anwendung an die dazugehörige Back-End-Web-API schützen können. 

Nachdem der Benutzer seine Zustimmung erteilt hat, können mit dem gleichen Authentifizierungsprotokoll Token zum Sichern von Aufrufen zwischen dem Client und anderen für die Anwendung konfigurierten Web-API-Ressourcen abgerufen werden. Weitere Informationen zur impliziten Gewährung der Autorisierung, damit Sie entscheiden können, ob diese sich für Ihr Anwendungsszenario eignet, finden Sie unter [Grundlegendes zum Ablauf der impliziten OAuth2-Gewährung in Azure Active Directory (AD)](active-directory-dev-understanding-oauth2-implicit-grant.md).

Die implizite OAuth 2.0-Gewährung ist für Anwendungen standardmäßig deaktiviert. Sie können die implizite OAuth 2.0-Gewährung für Ihre Anwendung aktivieren, indem Sie den Wert `oauth2AllowImplicitFlow` im dazugehörigen [Anwendungsmanifest](active-directory-application-manifest.md) festlegen.

#### <a name="to-enable-oauth-20-implicit-grant"></a>So aktivieren Sie die implizite OAuth 2.0-Gewährung

> [!NOTE]
> Ausführliche Informationen zur Bearbeitung des Anwendungsmanifests finden Sie im vorherigen Abschnitt [Konfigurieren einer Ressourcenanwendung zum Bereitstellen von Web-APIs](#configuring-a-resource-application-to-expose-web-apis).
>

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.
2. Wenn Sie unter Ihrem Konto mehrere Zugriffsmöglichkeiten haben, können Sie oben rechts auf Ihr Konto klicken und Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten festlegen.
3. Klicken Sie im linken Navigationsbereich auf den **Azure Active Directory**-Dienst und dann auf **App-Registrierungen**. Suchen Sie anschließend nach der Anwendung, die Sie konfigurieren möchten, und klicken Sie darauf. Sie gelangen auf die Hauptseite der Anwendungsregistrierung, und die Seite **Einstellungen** für die Anwendung wird geöffnet.
4. Wechseln Sie auf die Seite **Manifest bearbeiten**, indem Sie auf der Registrierungsseite der Anwendung auf **Manifest** klicken. Ein webbasierter Manifest-Editor wird geöffnet, mit dem Sie das Manifest im Portal bearbeiten können (**Bearbeiten**). Suchen Sie nach dem Wert „oauth2AllowImplicitFlow“, und ändern sie ihn in „true“. Standardmäßig ist „false“ festgelegt.
   
  ```json
  "oauth2AllowImplicitFlow": true,
  ```
5. Speichern Sie das aktualisierte Manifest. Nach dem Speichern des Manifests ist Ihre Web-API für die Verwendung der impliziten OAuth 2.0-Gewährung zum Authentifizieren von Benutzern konfiguriert.

## <a name="removing-an-application"></a>Entfernen einer Anwendung
In diesem Abschnitt wird die Vorgehensweise beim Entfernen der Registrierung einer Anwendung aus Ihrem Azure AD-Mandanten beschrieben.

### <a name="removing-an-application-authored-by-your-organization"></a>Entfernen einer Anwendung, die von Ihrer Organisation erstellt wurde
Anwendungen, die von Ihrer Organisation registriert wurden, werden unter dem Filter „Meine Apps“ auf der Hauptseite „App-Registrierungen“ Ihres Mandanten angezeigt. Dies sind die Anwendungen, die Sie manuell über das Azure-Portal oder programmgesteuert per PowerShell oder Graph-API registriert haben. Genauer gesagt, werden sie sowohl durch ein Anwendungs- als auch Dienstprinzipalobjekt in Ihrem Mandanten dargestellt. Weitere Informationen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](active-directory-application-objects.md).

#### <a name="to-remove-a-single-tenant-application-from-your-directory"></a>So entfernen Sie eine Einzelinstanzanwendung aus Ihrem Verzeichnis
1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.
2. Wenn Sie unter Ihrem Konto mehrere Zugriffsmöglichkeiten haben, können Sie oben rechts auf Ihr Konto klicken und Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten festlegen.
3. Klicken Sie im linken Navigationsbereich auf den **Azure Active Directory**-Dienst und dann auf **App-Registrierungen**. Suchen Sie anschließend nach der Anwendung, die Sie konfigurieren möchten, und klicken Sie darauf. Sie gelangen auf die Hauptseite der Anwendungsregistrierung, und die Seite **Einstellungen** für die Anwendung wird geöffnet.
4. Klicken Sie auf der Hauptseite der Anwendung für die Registrierung auf **Löschen**.
5. Klicken Sie in der Bestätigungsmeldung auf **Ja** .

#### <a name="to-remove-a-multi-tenant-application-from-its-home-directory"></a>So entfernen Sie eine mehrinstanzenfähige Anwendung aus ihrem Stammverzeichnis
1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.
2. Wenn Sie unter Ihrem Konto mehrere Zugriffsmöglichkeiten haben, können Sie oben rechts auf Ihr Konto klicken und Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten festlegen.
3. Klicken Sie im linken Navigationsbereich auf den **Azure Active Directory**-Dienst und dann auf **App-Registrierungen**. Suchen Sie anschließend nach der Anwendung, die Sie konfigurieren möchten, und klicken Sie darauf. Sie gelangen auf die Hauptseite der Anwendungsregistrierung, und die Seite **Einstellungen** für die Anwendung wird geöffnet.
4. Wählen Sie auf der Seite **Einstellungen** die Option **Eigenschaften**, und legen Sie **Mehrinstanzenfähig** auf **Nein** fest, um Ihre Anwendung zuerst auf die Verwendung eines einzelnen Mandanten festzulegen. Klicken Sie anschließend auf **Speichern**. Die Dienstprinzipalobjekte der Anwendung verbleiben auf den Mandanten aller Organisationen, die bereits ihre Zustimmung erteilt haben.
5. Klicken Sie auf der Hauptseite der Anwendung für die Registrierung auf **Löschen**.
6. Klicken Sie in der Bestätigungsmeldung auf **Ja** .

### <a name="removing-a-multi-tenant-application-authorized-by-another-organization"></a>Entfernen einer mehrinstanzenfähigen Anwendung, die von einer anderen Organisation autorisiert ist
Bei einer Teilmenge der Anwendungen, die auf der Hauptseite „App-Registrierungen“ Ihres Mandanten unter dem Filter „Alle Apps“ (mit Ausnahme der Registrierungen vom Typ „Meine Apps“) angezeigt werden, handelt es sich um mehrinstanzenfähige Anwendungen. In technischer Hinsicht stammen diese mehrinstanzenfähigen Anwendungen von einem anderen Mandanten und wurden während des Zustimmungsprozesses unter Ihrem Mandanten registriert. Genauer gesagt: Sie werden unter Ihrem Mandanten nur von einem Dienstprinzipalobjekt ohne entsprechendes Anwendungsobjekt repräsentiert. Weitere Informationen zu den Unterschieden zwischen Anwendungs- und Dienstprinzipalobjekten finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure AD](active-directory-application-objects.md).

Der Administrator des Unternehmens muss seinen Dienstprinzipal entfernen, um für eine mehrinstanzenfähige Anwendung den Zugriff auf Ihr Verzeichnis zu entfernen (nachdem die Zustimmung erteilt wurde). Der Administrator muss über globalen Administratorzugriff verfügen und kann über das Azure-Portal die [Azure AD-PowerShell-Cmdlets](http://go.microsoft.com/fwlink/?LinkId=294151) entfernen, um die Erteilung des Zugriffs aufzuheben.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zur Funktionsweise der Authentifizierung in Azure AD finden Sie unter [Authentifizierungsszenarien für Azure AD](active-directory-authentication-scenarios.md).
- Unter [Brandingrichtlinien für integrierte Apps](active-directory-branding-guidelines.md) finden Sie Tipps zu Darstellungsfragen für Ihre App.
- Weitere Informationen zu den Beziehungen zwischen Anwendungsobjekten und Dienstprinzipalobjekten der Anwendung finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](active-directory-application-objects.md).
- Weitere Informationen zur Rolle des App-Manifests finden Sie unter [Grundlegendes zum Azure Active Directory-Anwendungsmanifest](active-directory-application-manifest.md).
- Unter [Azure AD-Entwicklerglossar](active-directory-dev-glossary.md) finden Sie Definitionen für einige der wichtigsten Konzepte für Azure Active Directory-Entwickler (AD).
- Im [Active Directory-Entwicklerhandbuch](active-directory-developers-guide.md) finden Sie eine Übersicht über alle für Entwickler relevanten Inhalte.

