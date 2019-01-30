---
title: Integrieren von Anwendungen in Azure Active Directory
description: Erfahren Sie, wie eine Anwendung in Azure Active Directory (Azure AD) aktualisiert wird.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: lenalepa, sureshja
ms.openlocfilehash: 9a89768a5cf02cc8d4cdce670bdfb5b90f504bdf
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54447461"
---
# <a name="quickstart-update-an-application-in-azure-active-directory"></a>Schnellstart: Aktualisieren einer Anwendung in Azure Active Directory

Unternehmensentwickler und SaaS-Anbieter (Software-as-a-Service), die Anwendungen in Azure Active Directory (Azure AD) registriert haben, müssen möglicherweise Ihre Anwendungen so konfigurieren, dass sie auf andere Ressourcen wie Web-APIs zugreifen, diese in anderen Unternehmen verfügbar machen und vieles mehr.

In diesem Schnellstart erfahren Sie, wie Sie Ihre Anwendung konfigurieren oder aktualisieren können, um die Anforderungen und Bedürfnisse Ihrer und anderer Unternehmen zu erfüllen.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zunächst sicher, dass Sie die folgenden Schritte ausgeführt haben:

* Sie haben die Übersicht über das [Zustimmungsframework von Azure AD](consent-framework.md) gelesen und verstanden. Damit sollten Sie vertraut sein, wenn Sie Anwendungen erstellen, die von anderen Benutzern oder Anwendungen verwendet werden müssen.
* Sie verfügen über einen Azure AD-Mandanten, bei dem Anwendungen registriert wurden.
  * Wenn Sie über noch keinen Mandanten verfügen, [erfahren Sie hier, wie Sie einen erhalten](quickstart-create-new-tenant.md).
  * Wenn in Ihrem Mandanten keine Apps registriert sind, [lernen Sie, wie Sie Azure AD eine Anwendung hinzufügen](quickstart-v1-integrate-apps-with-azure-ad.md) können.

## <a name="configure-a-client-application-to-access-web-apis"></a>Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs

Damit eine Webanwendung oder vertrauliche Clientanwendung an einem Flow zur Autorisierungsgenehmigung teilnehmen kann, bei dem eine Authentifizierung (und das Abrufen eines Zugriffstokens) erforderlich ist, benötigt sie sichere Anmeldeinformationen. Die Standardauthentifizierungsmethode im Azure-Portal ist „Client-ID + geheimer Schlüssel“. In diesem Abschnitt werden die Konfigurationsschritte behandelt, die erforderlich sind, um den geheimen Schlüssel mit den Anmeldeinformationen Ihres Clients bereitzustellen.

Bevor ein Client Zugriff auf eine Web-API erhält, die durch eine Ressourcenanwendung (z.B. die Microsoft Graph-API) bereitgestellt wird, stellt das Zustimmungsframework außerdem sicher, dass dem Client die erforderlichen Berechtigungen basierend auf den angeforderten Berechtigungen erteilt werden. Standardmäßig können alle Anwendungen Berechtigungen aus **Azure Active Directory** (Graph-API) und dem klassischen Azure-Bereitstellungsmodell auswählen. Die [Graph-API-Berechtigung „Anmelden und Benutzerprofil lesen“](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes#PermissionScopeDetails) ist standardmäßig ebenfalls ausgewählt. Wenn Ihr Client unter einem Mandanten registriert wird, der über Konten mit Abonnements von Office 365 verfügt, können auch Web-APIs und Berechtigungen für SharePoint und Exchange Online ausgewählt werden. Sie können für jede gewünschte Web-API zwischen zwei Arten von Berechtigungen wählen:

- Anwendungsberechtigungen: Ihre Clientanwendung muss direkt als sie selbst (also ohne Benutzerkontext) auf die Web-API zugreifen. Für diese Art von Berechtigung ist die Zustimmung des Administrators erforderlich, sie steht nicht für systemeigene Clientanwendungen zur Verfügung.
- Delegierte Berechtigungen: Ihre Clientanwendung muss als angemeldeter Benutzer auf die Web-API zugreifen. Der Zugriff ist aber durch die ausgewählte Berechtigung eingeschränkt. Diese Art von Berechtigung kann von einem Benutzer erteilt werden, sofern für die Berechtigung nicht die Zustimmung durch einen Administrator erforderlich ist.

  > [!NOTE]
  > Durch Hinzufügen einer delegierten Berechtigung zu einer Anwendung erteilen Sie den Benutzern im Mandanten nicht automatisch Ihre Zustimmung. Benutzer müssen den zusätzlichen delegierten Berechtigungen weiterhin zur Laufzeit manuell zustimmen, sofern der Administrator nicht im Namen aller Benutzer die Zustimmung erteilt hat.

### <a name="add-application-credentials-or-permissions-to-access-web-apis"></a>Hinzufügen von Anmeldeinformationen für Anwendungen oder Zugriffsberechtigungen für Web-APIs

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wenn Ihr Konto Zugriff auf mehrere Mandanten ermöglicht, wählen Sie oben rechts Ihr Konto aus und legen Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
3. Wählen Sie im linken Navigationsbereich den **Azure Active Directory**-Dienst und dann **App-Registrierungen** aus. Suchen Sie anschließend nach der Anwendung, die Sie konfigurieren möchten, und wählen Sie diese aus.

   ![Aktualisieren der Registrierung einer Anwendung](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration.png)

4. Sie gelangen auf die Hauptseite der Anwendungsregistrierung, die die Seite **Einstellungen** für die Anwendung enthält. So fügen Sie Anmeldeinformationen für Ihre Webanwendung hinzu:
  1. Wählen Sie auf der Seite **Einstellungen** den Abschnitt **Schlüssel** aus.
  2. So fügen Sie ein Zertifikat hinzu:
    - Klicken Sie auf **Öffentlichen Schlüssel hochladen**.
    - Wählen Sie die Datei aus, die Sie hochladen möchten. Die Datei muss einen der folgenden Dateitypen aufweisen: CER, PEM, CRT.
  - So fügen Sie ein Kennwort hinzu:
    - Fügen Sie eine Beschreibung für Ihren Schlüssel hinzu.
    - Wählen Sie eine Dauer aus.
    - Wählen Sie **Speichern** aus. Die Spalte ganz rechts enthält nach dem Speichern der Konfigurationsänderungen den Schlüsselwert. **Kopieren Sie den Schlüssel** zur Verwendung im Code Ihrer Clientanwendung, da er nach dem Verlassen dieser Seite nicht mehr zugänglich ist.

5. So fügen Sie Berechtigungen für den Zugriff auf Ressourcen-APIs von Ihrem Client hinzu
  1. Wählen Sie auf der Seite **Einstellungen** zunächst den Abschnitt **Erforderliche Berechtigungen** und dann **Hinzufügen** aus.
  1. Wählen Sie **API auswählen** aus, um den Typ der Ressourcen für die Auswahl anzugeben.
  1. Durchsuchen Sie die Liste der verfügbaren APIs, oder verwenden Sie das Suchfeld, um die verfügbaren Ressourcenanwendungen in Ihrem Verzeichnis auszuwählen, die eine Web-API verfügbar machen. Wählen Sie die Ressource aus, an der Sie interessiert sind, und klicken Sie dann auf **Auswählen**.
  1. Wählen Sie auf der Seite **Zugriff aktivieren** die Anwendungsberechtigungen bzw. die delegierten Berechtigungen aus, die Ihre Anwendung für den Zugriff auf die API benötigt.
   
  ![Aktualisieren der Registrierung einer Anwendung – API für Berechtigungen](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration-settings-permissions-api.png)

  ![Aktualisieren der Registrierung einer Anwendung – Berechtigungen](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration-settings-permissions-perms.png)

6. Wählen Sie abschließend auf der Seite **Zugriff aktivieren** die Schaltfläche **Auswählen** und dann auf der Seite **API-Zugriff hinzufügen** die Schaltfläche **Fertig** aus. Sie gelangen zurück auf die Seite **Erforderliche Berechtigungen**, auf der Sie sehen, dass die neue Ressource der Liste mit den APIs hinzugefügt wurde.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Konfigurieren einer Ressourcenanwendung zum Bereitstellen von Web-APIs

Sie können eine Web-API entwickeln und sie Clientanwendungen zur Verfügung stellen, indem Sie [Zugriffsbereiche](developer-glossary.md#scopes) und [Rollen](developer-glossary.md#roles) verfügbar machen. Eine ordnungsgemäß konfigurierte Web-API wird auf die gleiche Weise wie andere Microsoft-Web-APIs (einschließlich der Graph-API und der Office 365-APIs) zur Verfügung gestellt. Zugriffsbereiche und Rollen werden durch Ihr [Anwendungsmanifest](developer-glossary.md#application-manifest) verfügbar gemacht. Hierbei handelt es sich um eine JSON-Datei, die die Identitätskonfiguration Ihrer Anwendung repräsentiert.

Im folgenden Abschnitt erfahren Sie, wie Sie Zugriffsbereiche verfügbar machen, indem Sie das Manifest der Ressourcenanwendung bearbeiten.

### <a name="add-access-scopes-to-your-resource-application"></a>Hinzufügen von Zugriffsbereichen zu Ihrer Ressourcenanwendung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wenn Ihr Konto Zugriff auf mehrere Mandanten ermöglicht, wählen Sie oben rechts Ihr Konto aus und legen Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
3. Wählen Sie im linken Navigationsbereich **Azure Active Directory > App-Registrierungen** aus. Suchen Sie anschließend nach der Anwendung, die Sie konfigurieren möchten, und wählen Sie diese aus.

   ![Aktualisieren der Registrierung einer Anwendung](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration.png)

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
  > Der Wert `id` muss programmgesteuert oder mit einem Tool zum Generieren von GUIDs (beispielsweise [guidgen](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx)) generiert werden. Die `id` stellt einen eindeutigen Bezeichner für den Bereich dar, der von der Web-API bereitgestellt wird. Nachdem ein Client richtig mit Berechtigungen zum Zugreifen auf Ihre Web-API konfiguriert wurde, wird dafür von Azure AD ein OAuth 2.0-Zugriffstoken ausgestellt. Wenn der Client die Web-API aufruft, wird das Zugriffstoken präsentiert, für das der Bereichsanspruch (scp) auf die Berechtigungen festgelegt ist, die in der dazugehörigen Anwendungsregistrierung angefordert werden.
  >
  > Sie können später bei Bedarf zusätzliche Bereiche verfügbar machen. Berücksichtigen Sie, dass Ihre Web-API verschiedene Berechtigungen verfügbar machen kann, die einer Vielzahl von unterschiedlichen Funktionen zugeordnet sind. Ihre Ressource kann den Zugriff auf die Web-API zur Laufzeit steuern, indem die Bereichsansprüche (`scp`) im erhaltenen OAuth 2.0-Zugriffstoken ausgewertet werden.

6. Klicken Sie abschließend auf **Speichern**. Ihre Web-API ist jetzt so konfiguriert, dass sie von anderen Anwendungen im Verzeichnis verwendet werden kann.

  ![Aktualisieren der Registrierung einer Anwendung](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration-manifest.png)

### <a name="verify-the-web-api-is-exposed-to-other-applications-in-your-tenant"></a>Überprüfen, ob die Web-API für andere Anwendungen in Ihrem Mandanten verfügbar gemacht wird

1. Wechseln Sie zurück zu Ihrem Azure AD-Mandanten, wählen Sie **App-Registrierungen** erneut aus, und suchen Sie nach der Clientanwendung, die Sie konfigurieren möchten. Wählen Sie diese Clientanwendung aus.

   ![Aktualisieren der Registrierung einer Anwendung](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration.png)

2. Wiederholen Sie Schritt 5 wie unter [Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs](#configure-a-client-application-to-access-web-apis). Suchen Sie im Schritt **API auswählen** nach Ihrer Ressource, indem Sie ihren Anwendungsnamen im Suchfeld eingeben und auf **Auswählen** klicken.

3. Auf der Seite **Zugriff aktivieren** sollte der neue Bereich angezeigt werden, der für Clientberechtigungsanforderungen verfügbar ist.

  ![Neue Berechtigungen werden angezeigt](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration-settings-permissions-perms-newscopes.png)

### <a name="more-on-the-application-manifest"></a>Weitere Informationen zum Anwendungsmanifest

Das Anwendungsmanifest dient als Mechanismus zum Aktualisieren der Anwendungsentität, die alle Attribute der Identitätskonfiguration einer Azure AD-Anwendung einschließlich der erörterten API-Zugriffsbereiche definiert. Weitere Informationen zur Anwendungsentität und zum dazugehörigen Schema finden Sie in der [Dokumentation zur Anwendungsentität der Graph-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity). Der Artikel enthält umfassende Referenzinformationen zu den Anwendungsentitätselementen, die verwendet werden, um Berechtigungen für Ihre API anzugeben, z.B.:  

- Das appRoles-Mitglied, bei es sich um eine Sammlung von [AppRole](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type)-Entitäten handelt, die zum Definieren der [Anwendungsberechtigungen](developer-glossary.md#permissions) für eine Web-API verwendet werden. 
- Das oauth2Permissions-Mitglied, bei dem es sich um eine Sammlung von [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type)-Entitäten handelt, die zum Definieren der [delegierten Berechtigungen](developer-glossary.md#permissions) für eine Web-API verwendet werden.

Weitere Informationen zu Anwendungsmanifestkonzepten im Allgemeinen finden Sie unter [Azure AD App-Manifest](reference-app-manifest.md).

## <a name="accessing-the-azure-ad-graph-and-office-365-via-microsoft-graph-apis"></a>Zugreifen auf Azure AD-Graph-API und Office 365 über Microsoft Graph-APIs  

Wie zuvor bereits erwähnt, können Sie zusätzlich zum Verfügbarmachen von/Zugreifen auf APIs für Ihre eigenen Anwendungen Ihre Clientanwendung für den Zugriff auf APIs aktualisieren, die von Microsoft-Ressourcen verfügbar gemacht werden. Die Microsoft Graph-API, die im Portal in der Liste mit den Ressourcen/APIs als „Microsoft Graph“ bezeichnet wird, ist für alle Anwendungen verfügbar, die mit Azure AD registriert werden. Wenn Sie Ihre Clientanwendung in einem Mandanten mit Konten registrieren, für die die Registrierung für ein Office 365-Abonnement durchgeführt wurde, können Sie auch auf die Bereiche zugreifen, die von den verschiedenen Office 365-Ressourcen verfügbar gemacht werden.

Eine vollständige Erläuterung der Bereiche, die von der Microsoft Graph-API verfügbar gemacht werden, finden Sie im Artikel [Microsoft Graph-Berechtigungsreferenz](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference).

> [!NOTE]
> Aufgrund einer aktuellen Einschränkung können systemeigene Clientanwendungen die Azure AD Graph-API nur aufrufen, wenn sie die Berechtigung "Zugriff auf das Verzeichnis Ihrer Organisation" verwenden. Diese Einschränkung gilt nicht für Webanwendungen.

## <a name="configuring-multi-tenant-applications"></a>Konfigurieren von mehrinstanzenfähigen Anwendungen

Wenn Sie eine Anwendung in Azure AD registrieren, möchten Sie ggf. erreichen, dass auf Ihre Anwendung nur durch Benutzer in Ihrer Organisation zugegriffen werden kann. Es kann auch sein, dass es nur für Benutzer in externen Organisationen möglich sein soll, auf Ihre Anwendung zuzugreifen. Diese zwei Anwendungstypen werden als Einzelinstanzanwendung bzw. als mehrinstanzenfähige Anwendung bezeichnet. In diesem Abschnitt wird beschrieben, wie Sie die Konfiguration einer Einzelinstanzanwendung so ändern, dass Sie eine mehrinstanzenfähige Anwendung erhalten.

Es ist wichtig, die Unterschiede zwischen einer Einzelinstanzanwendung und einer mehrinstanzenfähigen Anwendung zu verstehen:  

- Eine Einzelinstanzanwendung ist für die Verwendung in einem einzelnen Unternehmen gedacht. Es handelt sich dabei zumeist um eine Branchenanwendung (Line-of-Business, LOB), die von einem Unternehmensentwickler geschrieben wurde. Auf eine Anwendung mit nur einem Mandanten kann nur von Benutzern mit Konten unter demselben Mandanten wie die Anwendungsregistrierung zugegriffen werden. Daher ist die Bereitstellung in nur einem Verzeichnis erforderlich.
- Eine mehrinstanzenfähige Anwendung ist für die Verwendung in vielen Organisationen vorgesehen. Sie wird als SaaS-Webanwendung (Software-as-a-Service) bezeichnet, die in der Regel von einem unabhängigen Softwarehersteller (ISV, Independent Software Vendor) geschrieben wurde. Mehrinstanzenfähige Anwendungen müssen unter jedem Mandanten bereitgestellt werden, auf den Benutzer Zugriff benötigen. Für andere Mandanten als der Mandant mit der Anwendungsregistrierung ist die Zustimmung durch Benutzer oder Administratoren erforderlich, um sie zu registrieren. Beachten Sie, dass native Clientanwendungen standardmäßig mehrinstanzenfähig sind, da sie auf dem Gerät des Ressourcenbesitzers installiert sind. Weitere Informationen zum Zustimmungs-Framework finden Sie oben in der [Übersicht über das Zustimmungs-Framework](#overview-of-the-consent-framework).

Um eine Anwendung mehrinstanzenfähig zu machen, sind sowohl Änderungen der Anwendungsregistrierung als auch Änderungen der Webanwendung selbst erforderlich. Beides wird in den folgenden Abschnitten beschrieben.

### <a name="changing-the-application-registration-to-support-multi-tenant"></a>Ändern der Anwendungsregistrierung zur Unterstützung der Mehrinstanzenfähigkeit

Wenn Sie eine Anwendung schreiben, die Sie Ihren Kunden oder Partnern außerhalb Ihrer Organisation zur Verfügung stellen möchten, müssen Sie die Anwendungsdefinition im Azure-Portal aktualisieren.

> [!IMPORTANT]
> Für Azure AD muss der App-ID-URI von mehrinstanzenfähigen Anwendungen global eindeutig sein. Der App-ID-URI ist eine der Methoden, mit der eine Anwendung in Protokollmeldungen identifiziert wird. Bei einer Anwendung mit einem einzigen Mandanten muss der App-ID-URI nur in diesem Mandanten eindeutig sein. Bei einer mehrinstanzenfähigen Anwendung muss er global eindeutig sein, damit Azure AD die Anwendung in allen Mandanten finden kann.
> Globale Eindeutigkeit wird durch die Anforderung erzwungen, dass der App-ID-URI einen Hostnamen aufweisen muss, der mit einer überprüften Domäne des Azure AD-Mandanten übereinstimmt. Wenn der Name Ihres Mandanten beispielsweise „contoso.onmicrosoft.com“ lautet, wäre https://contoso.onmicrosoft.com/myapp ein gültiger App-ID-URI. Wenn Ihr Mandant eine verifizierte Domäne von „contoso.com“ hat, wäre auch https://contoso.com/myapp ein gültiger App-ID-URI. Wenn der App-ID-URI nicht diesem Muster folgt, schlägt das Festlegen einer Anwendung als mehrinstanzenfähig fehl.

Gehen Sie wie folgt vor, um externen Benutzern den Zugriff auf Ihre Anwendung zu ermöglichen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wenn Sie unter Ihrem Konto mehrere Zugriffsmöglichkeiten haben, können Sie oben rechts auf Ihr Konto klicken und Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten festlegen.
3. Klicken Sie im linken Navigationsbereich auf den **Azure Active Directory**-Dienst und dann auf **App-Registrierungen**. Suchen Sie anschließend nach der Anwendung, die Sie konfigurieren möchten, und klicken Sie darauf. Sie gelangen auf die Hauptseite der Anwendungsregistrierung, und die Seite **Einstellungen** für die Anwendung wird geöffnet.
4. Klicken Sie auf der Seite **Einstellungen** auf **Eigenschaften**, und schieben Sie den Regler für **Mehrinstanzenfähig** auf **Ja**.

Nachdem Sie die Änderungen vorgenommen haben, können Benutzer und Administratoren in anderen Organisationen es ihren Benutzern ermöglichen, sich an Ihrer Anwendung anzumelden. So kann Ihre Anwendung auf Ressourcen zugreifen, die durch den entsprechenden Mandanten geschützt sind.

### <a name="changing-the-application-to-support-multi-tenant"></a>Ändern der Anwendung zur Unterstützung der Mehrinstanzenfähigkeit

Die Unterstützung für mehrinstanzenfähige Anwendungen ist stark vom Zustimmungs-Framework von Azure AD abhängig. Die Zustimmung (Consent) ist das Verfahren, mit dem ein Benutzer eines anderen Mandanten der Anwendung Zugriff auf die Ressourcen gewähren kann, die durch den Mandanten des Benutzers geschützt sind. Dieser Prozess wird als „Zustimmung des Benutzers“ bezeichnet.

Ihre Webanwendung kann auch folgende Optionen anbieten:

- Die Möglichkeit „Mein Unternehmen registrieren“ für Administratoren. Dieser Prozess wird als „Zustimmung des Administrators“ bezeichnet und ermöglicht einem Administrator das Gewähren der Zustimmung im Namen *aller Benutzer* der Organisation. Nur ein Benutzer, der sich mit einem Konto authentifiziert, das zur globalen Administratorrolle gehört, kann die Zustimmung des Administrators erteilen. Andere Benutzer erhalten eine Fehlermeldung.
- Eine Registrierungsbenutzeroberfläche für Benutzer. Es wird erwartet, dass für Benutzer die Schaltfläche „Registrieren“ bereitgestellt wird, mit der der Browser an den Azure AD OAuth 2.0-Endpunkt `/authorize` oder einen OpenID Connect-Endpunkt vom Typ `/userinfo` weitergeleitet wird. Diese Endpunkte ermöglichen es der Anwendung, Informationen zum neuen Benutzer durch Untersuchen des id_token-Objekts abzurufen. Nach der Registrierungsphase wird der Benutzer, ähnlich wie im Abschnitt [Übersicht über das Zustimmungs-Framework](#overview-of-the-consent-framework) gezeigt, zum Erteilen der Zustimmung aufgefordert.

Weitere Informationen zu den Anwendungsänderungen, die zur Unterstützung von mehrinstanzenfähigen Benutzeroberflächen für Zugriff und Anmeldung/Registrierung erforderlich sind, finden Sie hier:

- [Anmelden von Azure AD-Benutzern (Active Directory) mit dem mehrinstanzenfähigen Anwendungsmuster](howto-convert-app-to-be-multi-tenant.md)
- Liste mit [Codebeispielen für Mehrinstanzenfähigkeit](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant)
- [Schnellstart: Hinzufügen eines Unternehmensbrandings zur Anmeldeseite in Azure AD](../fundamentals/customize-branding.md)

## <a name="enabling-oauth-20-implicit-grant-for-single-page-applications"></a>Aktivieren der impliziten OAuth 2.0-Gewährung für Single-Page-Webanwendungen

Single-Page-Webanwendungen (SPAs) basieren in der Regel auf einem JavaScript-intensiven Front-End, das im Browser ausgeführt wird. Hierüber wird das Web-API-Back-End der Anwendung zum Ausführen der Geschäftslogik aufgerufen. Für SPAs, die in Azure AD gehostet werden, verwenden Sie die implizite OAuth 2.0-Gewährung zum Authentifizieren des Benutzers für Azure AD und rufen ein Token ab, mit dem Sie Aufrufe vom JavaScript-Client der Anwendung an die dazugehörige Back-End-Web-API schützen können.

Nachdem der Benutzer seine Zustimmung erteilt hat, können mit dem gleichen Authentifizierungsprotokoll Token zum Sichern von Aufrufen zwischen dem Client und anderen für die Anwendung konfigurierten Web-API-Ressourcen abgerufen werden. Weitere Informationen zur impliziten Gewährung der Autorisierung, damit Sie entscheiden können, ob diese sich für Ihr Anwendungsszenario eignet, finden Sie unter [Grundlegendes zum Ablauf der impliziten OAuth2-Gewährung in Azure Active Directory (AD)](v1-oauth2-implicit-grant-flow.md).

Die implizite OAuth 2.0-Gewährung ist für Anwendungen standardmäßig deaktiviert. Sie können die implizite OAuth 2.0-Gewährung für Ihre Anwendung aktivieren, indem Sie den Wert `oauth2AllowImplicitFlow` im dazugehörigen [Anwendungsmanifest](reference-app-manifest.md) festlegen.

### <a name="to-enable-oauth-20-implicit-grant"></a>So aktivieren Sie die implizite OAuth 2.0-Gewährung

> [!NOTE]
> Ausführliche Informationen zur Bearbeitung des Anwendungsmanifests finden Sie im vorherigen Abschnitt [Konfigurieren einer Ressourcenanwendung zum Bereitstellen von Web-APIs](#configuring-a-resource-application-to-expose-web-apis).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wenn Sie unter Ihrem Konto mehrere Zugriffsmöglichkeiten haben, können Sie oben rechts auf Ihr Konto klicken und Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten festlegen.
3. Klicken Sie im linken Navigationsbereich auf den **Azure Active Directory**-Dienst und dann auf **App-Registrierungen**. Suchen Sie anschließend nach der Anwendung, die Sie konfigurieren möchten, und klicken Sie darauf. Sie gelangen auf die Hauptseite der Anwendungsregistrierung, und die Seite **Einstellungen** für die Anwendung wird geöffnet.
4. Wechseln Sie auf die Seite **Manifest bearbeiten**, indem Sie auf der Registrierungsseite der Anwendung auf **Manifest** klicken. Ein webbasierter Manifest-Editor wird geöffnet, mit dem Sie das Manifest im Portal bearbeiten können (**Bearbeiten**). Suchen Sie nach dem Wert „oauth2AllowImplicitFlow“, und ändern sie ihn in „true“. Standardmäßig ist „false“ festgelegt.
   
  ```json
  "oauth2AllowImplicitFlow": true,
  ```
5. Speichern Sie das aktualisierte Manifest. Nach dem Speichern des Manifests ist Ihre Web-API für die Verwendung der impliziten OAuth 2.0-Gewährung zum Authentifizieren von Benutzern konfiguriert.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über diese anderen verwandten Schnellstarts für die App-Verwaltung für Apps, die den Azure AD v1.0-Endpunkt verwenden:
- [Hinzufügen einer Anwendung zu Azure AD](quickstart-v1-integrate-apps-with-azure-ad.md)
- [Entfernen einer Anwendung aus Azure AD](quickstart-v1-remove-azure-ad-app.md)

Um mehr über die beiden Azure AD-Objekte, die eine registrierte Anwendung darstellen, und die Beziehung zwischen ihnen zu erfahren, lesen Sie [Anwendungsobjekte und Dienstprinzipalobjekte](app-objects-and-service-principals.md).

Weitere Informationen zu den Brandingrichtlinien, die Sie bei der Entwicklung von Anwendungen mit Azure Active Directory verwenden sollten, finden Sie unter [Brandingrichtlinien für Anwendungen](howto-add-branding-in-azure-ad-apps.md).