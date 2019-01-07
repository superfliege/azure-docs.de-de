---
title: Autorisieren von Entwicklerkonten mithilfe von Azure Active Directory – Azure API Management | Microsoft-Dokumentation
description: Erfahren Sie, wie Benutzer mithilfe von Azure Active Directory in Azure API Management autorisiert werden.
services: api-management
documentationcenter: API Management
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: apimpm
ms.openlocfilehash: 10792c912e3028cd12eedb83b1b211bab20bc5a7
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52961058"
---
# <a name="authorize-developer-accounts-by-using-azure-active-directory-in-azure-api-management"></a>Autorisieren von Entwicklerkonten mithilfe von Azure Active Directory in Azure API Management

Dieser Artikel zeigt, wie Sie den Zugriff auf das Entwicklerportal für Benutzer aus Azure Active Directory (Azure AD) aktivieren. Außerdem erfahren Sie, wie Sie Azure AD-Benutzergruppen verwalten, indem Sie externe Gruppen hinzufügen, die die Benutzer enthalten.

## <a name="prerequisites"></a>Voraussetzungen

- Bearbeiten Sie den folgenden Schnellstart: [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)
- Importieren und veröffentlichen Sie eine Azure API Management-Instanz. Weitere Informationen finden Sie unter [Importieren und Veröffentlichen](import-and-publish.md).

[!INCLUDE [premium-dev-standard.md](../../includes/api-management-availability-premium-dev-standard.md)]

## <a name="authorize-developer-accounts-by-using-azure-ad"></a>Autorisieren von Entwicklerkonten mithilfe von Azure AD

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
1. Select ![Pfeil](./media/api-management-howto-aad/arrow.png).
1. Geben Sie im Suchfeld **api** ein.
1. Wählen Sie **API Management-Dienste** aus.
1. Wählen Sie Ihre API Management-Dienstinstanz aus.
1. Wählen Sie unter **SICHERHEIT** die Option **Identitäten** aus.

1. Wählen Sie oben **+ Hinzufügen** aus.

    Auf der rechten Seite wird der Bereich **Identitätsanbieter hinzufügen** geöffnet.
1. Wählen Sie für **Anbietertyp** die Option **Azure Active Directory** aus.

    Im Bereich werden Steuerelemente angezeigt, mit denen Sie weitere erforderliche Informationen eingeben können. Zu den Steuerelementen gehören **Client-ID** und **Clientgeheimnis**. (Sie erhalten weiter unten in diesem Artikel Informationen zu diesen Steuerelementen.)
1. Notieren Sie sich den Inhalt der **Umleitungs-URL**.
    
   ![Schritte zum Hinzufügen eines Identitätsanbieters im Azure-Portal](./media/api-management-howto-aad/api-management-with-aad001.png)  
1. Öffnen Sie in Ihrem Browser eine weitere Registerkarte. 
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
1. Select ![Pfeil](./media/api-management-howto-aad/arrow.png).
1. Geben Sie **Active** ein. Der Bereich **Azure Active Directory** wird geöffnet.
1. Wählen Sie **Azure Active Directory**.
1. Wählen Sie unter **VERWALTEN** die Option **App-Registrierungen** aus.
1. Wählen Sie **Registrierung einer neuen Anwendung** aus.

    ![Auswahl zum Erstellen einer neuen App-Registrierung](./media/api-management-howto-aad/api-management-with-aad002.png)

    Auf der rechten Seite wird der Bereich **Erstellen** angezeigt. Dort geben Sie die relevanten Informationen zur Azure AD-App ein.
1. Geben Sie einen Namen für die Anwendung ein.
1. Wählen Sie **Web-App/API** als Anwendungstyp aus.
1. Geben Sie als Anmelde-URL die Anmelde-URL Ihres Entwicklerportals ein. In diesem Beispiel ist die Anmelde-URL gleich `https://apimwithaad.portal.azure-api.net/signin`.
1. Klicken Sie auf **Erstellen**, um die Anwendung zu erstellen.
1. Um Ihre App zu finden, wählen Sie **App-Registrierungen** aus, und suchen Sie nach dem Namen.

    ![Feld für die Suche nach einer App](./media/api-management-howto-aad/find-your-app.png)
1. Nach dem Registrieren der Anwendung wechseln Sie zu **Antwort-URL** und stellen sicher, dass die **Umleitungs-URL** auf den Wert aus Schritt 9 festgelegt ist. 
1. Wenn Sie Ihre Anwendung konfigurieren möchten (z.B. wenn Sie die **App-ID-URL** ändern möchten), wählen Sie **Eigenschaften** aus.

    ![Öffnen des Eigenschaftenbereichs](./media/api-management-howto-aad/api-management-with-aad004.png)

    Wenn für diese Anwendung mehrere Azure AD-Instanzen verwendet werden, wählen Sie unter **Mehrinstanzenfähig** die Option **Ja** aus. Der Standardwert lautet **Nein**.
1. Legen Sie die Anwendungsberechtigungen fest, indem Sie **Erforderliche Berechtigungen** auswählen.
1. Wählen Sie Ihre Anwendung aus, und aktivieren Sie die Kontrollkästchen **Verzeichnisdaten lesen** und **Anmelden und Benutzerprofil lesen**.

    ![Kontrollkästchen für Berechtigungen](./media/api-management-howto-aad/api-management-with-aad005.png)

1. Wählen Sie **Berechtigungen erteilen** aus, um Anwendungsberechtigungen zu genehmigen.

    Weitere Informationen zu Anwendungsberechtigungen und delegierten Berechtigungen finden Sie unter [Zugreifen auf die Graph-API][Accessing the Graph API].
    
1. Kopieren Sie im linken Bereich den Wert für **Anwendungs-ID**.

    ![Wert „Anwendungs-ID“](./media/api-management-howto-aad/application-id.png)
1. Wechseln Sie zurück zu Ihrer API Management-Anwendung. 

    Fügen Sie den Wert **Anwendungs-ID** im Fenster **Identitätsanbieter hinzufügen** in das Feld **Client-ID** ein.
1. Wechseln Sie zurück zur Azure AD-Konfiguration, und wählen Sie **Schlüssel** aus.
1. Erstellen Sie einen neuen Schlüssel, indem Sie einen Namen und eine Dauer angeben. 
1. Wählen Sie **Speichern**aus. Der Schlüssel wird generiert.

    Kopieren Sie den Schlüssel in die Zwischenablage.

    ![Auswahl zum Erstellen eines Schlüssels](./media/api-management-howto-aad/api-management-with-aad006.png)

    > [!NOTE]
    > Notieren Sie sich diesen Schlüssel. Nachdem Sie das Fenster für die Azure AD-Konfiguration geschlossen haben, ist eine Anzeige des Schlüssels nicht mehr möglich.
    > 
    > 

1. Wechseln Sie zurück zu Ihrer API Management-Anwendung. 

    Fügen Sie den Schlüssel im Fenster **Identitätsanbieter hinzufügen** in das Textfeld **Clientgeheimnis** ein.

    > [!IMPORTANT]
    > Aktualisieren Sie unbedingt den **geheimen Clientschlüssel**, bevor der Schlüssel abläuft. 
    >  
    >

1. Das Fenster **Identitätsanbieter hinzufügen** enthält auch das Textfeld **Zulässige Mandanten**. Geben Sie die in diesem Feld die Domänen der Azure AD-Instanzen ein, für die Sie den APIs der API Management-Dienstinstanz Zugriff gewähren möchten. Sie können mehrere Domänen durch neue Zeilen, Leerzeichen oder Kommas trennen.

    Sie können im Abschnitt **Zulässige Mandanten** mehrere Domänen angeben. Bevor sich Benutzer aus einer anderen Domäne als der ursprünglichen Domäne (der Domäne, in der die Anwendung registriert wurde) anmelden können, muss ein globaler Administrator dieser anderen Domäne der Anwendung Berechtigungen für den Verzeichnisdatenzugriff erteilen. Um Berechtigungen zu erteilen, müssen Sie als globaler Administrator folgendermaßen vorgehen:
    
    a. Wechseln Sie zu `https://<URL of your developer portal>/aadadminconsent` (z. B. https://contoso.portal.azure-api.net/aadadminconsent).
    
    b. Geben Sie den Domänennamen des Azure AD-Mandanten ein, auf den Zugriff erteilt werden soll.
    
    c. Klicken Sie auf **Submit** (Senden). 
    
    Im folgenden Beispiel versucht ein globaler Administrator von miaoaad.onmicrosoft.com, Berechtigungen für dieses spezifische Entwicklerportal zu erteilen. 

1. Nachdem Sie die gewünschte Konfiguration angegeben haben, wählen Sie **Hinzufügen** aus.

    ![Schaltfläche „Hinzufügen“ im Bereich „Identitätsanbieter hinzufügen“](./media/api-management-howto-aad/api-management-with-aad007.png)

Nachdem die Änderungen gespeichert wurden, können sich Benutzer der angegebenen Azure AD-Instanz beim Entwicklerportal anmelden, indem sie die unter [Anmelden beim Entwicklerportal mit einem Azure AD-Konto](#log_in_to_dev_portal) beschriebenen Schritte ausführen.

![Eingabe des Namens eines Azure AD-Mandanten](./media/api-management-howto-aad/api-management-aad-consent.png)

Auf dem nächsten Bildschirm wird der globale Administrator aufgefordert, die Berechtigungserteilung zu bestätigen. 

![Bestätigung der Berechtigungszuweisung](./media/api-management-howto-aad/api-management-permissions-form.png)

Wenn ein nicht globaler Administrator versucht, sich anzumelden, bevor Berechtigungen durch einen globalen Administrator erteilt wurden, tritt ein Anmeldefehler auf, und ein Fehlerbildschirm wird angezeigt.

## <a name="add-an-external-azure-ad-group"></a>Hinzufügen einer externen Azure AD-Gruppe

Nachdem Sie den Zugriff für Benutzer in einer Azure AD-Instanz aktiviert haben, können Sie Azure AD-Gruppen in API Management hinzufügen. Anschließend ist es einfacher, die Zuordnung der Entwickler in der Gruppe zu den gewünschten Produkten zu verwalten.

Um eine externe Azure AD-Gruppe zu konfigurieren, muss zunächst die Azure AD-Instanz über die Registerkarte **Identitäten** konfiguriert werden – siehe hierzu die Vorgehensweise im vorherigen Abschnitt. 

Externe Azure AD-Gruppen werden über die Registerkarte **Gruppen** Ihrer API Management-Instanz hinzugefügt.

1. Wählen Sie die Registerkarte **Gruppen** .
1. Klicken Sie auf die Schaltfläche **AAD-Gruppe hinzufügen**.
   ![Schaltfläche „AAD-Gruppe hinzufügen“](./media/api-management-howto-aad/api-management-with-aad008.png)
1. Wählen Sie die Gruppe aus, die Sie hinzufügen möchten.
1. Klicken Sie auf die Schaltfläche **Auswählen**.

Nachdem Sie eine externe Azure AD-Gruppe hinzugefügt haben, können Sie ihre Eigenschaften überprüfen und konfigurieren. Wählen Sie den Namen der Gruppe auf der Registerkarte **Gruppen** aus. Von hier aus können Sie Informationen wie z.B. **Name** und **Beschreibung** der Gruppe bearbeiten.
 
Benutzer aus der konfigurierten Azure AD-Instanz können sich jetzt beim Entwicklerportal anmelden. Sie können beliebige Gruppen anzeigen und abonnieren, die für sie sichtbar sind.

## <a name="a-idlogintodevportalsign-in-to-the-developer-portal-by-using-an-azure-ad-account"></a><a id="log_in_to_dev_portal"/>Anmelden beim Entwicklerportal mit einem Azure AD-Konto

So melden Sie sich mit einem in den vorherigen Abschnitten konfigurierten Azure AD-Konto beim Entwicklerportal an

1. Öffnen Sie ein neues Browserfenster, indem Sie die Anmelde-URL aus der Active Directory-Anwendungskonfiguration verwenden, und wählen Sie **Azure Active Directory** aus.

   ![Anmeldeseite][api-management-dev-portal-signin]

1. Geben Sie die Anmeldeinformationen eines Azure AD-Benutzers ein, und wählen Sie **Anmelden** aus.

   ![Anmeldung mit Benutzername und Kennwort][api-management-aad-signin]

1. Es wird möglicherweise ein Registrierungsformular angezeigt, wenn weitere Informationen erforderlich sind. Vervollständigen Sie das Registrierungsformular, und wählen Sie **Registrieren**.

   ![Schaltfläche „Anmelden“ auf dem Registrierungsformular][api-management-complete-registration]

Sie sind jetzt beim Entwicklerportal für Ihre API Management-Dienstinstanz angemeldet.

![Entwicklerportal nach Abschluss der Registrierung][api-management-registration-complete]

[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: get-started-create-service-instance.md

[https://oauth.net/2/]: https://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: https://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Sign in to the developer portal by using an Azure AD account]: #Sign-in-to-the-developer-portal-by-using-an-Azure-AD-account
