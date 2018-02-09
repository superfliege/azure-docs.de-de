---
title: "Autorisieren von Entwicklerkonten mithilfe von Azure Active Directory – Azure API Management | Microsoft-Dokumentation"
description: Erfahren Sie, wie Benutzer mithilfe von Active Directory in Azure API Management autorisiert werden.
services: api-management
documentationcenter: API Management
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: apimpm
ms.openlocfilehash: c9d99d6f7c2777c8e68f5db50b402280377d88e9
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Autorisieren von Entwicklerkonten mithilfe von Active Directory in Azure API Management

Dieser Leitfaden zeigt, wie Sie den Zugriff auf das Entwicklerportal für Benutzer in Azure Active Directory aktivieren. Außerdem erfahren Sie, wie Sie Azure Active Directory-Benutzergruppen verwalten, indem Sie externe Gruppen hinzufügen, die Benutzer eines anderen Azure Active Directory-Verzeichnisses enthalten.

> [!WARNING]
> Die Integration von Azure Active Directory ist nur in den Tarifen [Developer, Standard und Premium](https://azure.microsoft.com/pricing/details/api-management/) enthalten.

## <a name="prerequisites"></a>Voraussetzungen

- Absolvieren Sie den folgenden Schnellstart: [Erstellen einer Azure API Management-Instanz](get-started-create-service-instance.md).
- Importieren und veröffentlichen Sie eine API Management-Instanz. Weitere Informationen finden Sie unter [Importieren und Veröffentlichen](import-and-publish.md).

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Autorisieren von Entwicklerkonten mithilfe von Active Directory

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Wählen Sie ![Pfeil](./media/api-management-howto-aad/arrow.png).
3. Geben Sie im Suchfeld den Suchbegriff „api“ ein.
4. Klicken Sie auf **API Management-Dienste**.
5. Wählen Sie Ihre APIM-Dienstinstanz aus.
6. Wählen Sie unter **SICHERHEIT** die Option **Identitäten** aus.

    ![Externe Identitäten](./media/api-management-howto-aad/api-management-with-aad001.png)
7. Klicken Sie oben auf **+ Hinzufügen**.

    Auf der rechten Seite wird das Fenster **Identitätsanbieter hinzufügen** geöffnet.
8. Wählen Sie für **Anbietertyp** die Option **Azure Active Directory** aus.

    Im Fenster werden Steuerelemente geöffnet, mit denen Sie weitere erforderliche Informationen eingeben können. Hierzu gehören z.B. folgende Elemente: **Client-ID**, **geheimer Clientschlüssel** (diese Informationen erhalten Sie weiter unten im Tutorial).
9. Notieren Sie sich die **Umleitungs-URL**.  
10. Öffnen Sie in Ihrem Browser eine weitere Registerkarte. 
11. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
12. Wählen Sie ![Pfeil](./media/api-management-howto-aad/arrow.png).
13. Geben Sie „active“ ein, und der Eintrag **Azure Active Directory** wird angezeigt.
14. Wählen Sie **Azure Active Directory**.
15. Wählen Sie unter **VERWALTEN** die Option **App-Registrierung** aus.

    ![App-Registrierung](./media/api-management-howto-aad/api-management-with-aad002.png)
16. Klicken Sie auf **Registrierung einer neuen Anwendung**.

    Das Fenster **Erstellen** wird auf der rechten Seite angezeigt. Dort geben Sie die relevanten Informationen der AAD-App ein.
17. Geben Sie einen Namen für die Anwendung ein.
18. Wählen Sie **Web-App/API** als Anwendungstyp aus.
19. Geben Sie als Anmelde-URL die Anmelde-URL Ihres Entwicklerportals ein. In diesem Beispiel lautet die Anmelde-URL folgendermaßen: https://apimwithaad.portal.azure-api.net/signin.
20. Klicken Sie auf **Erstellen**, um die Anwendung zu erstellen.
21. Um Ihre App zu finden, wählen Sie **App-Registrierungen** aus, und suchen Sie nach dem Namen.

    ![App-Registrierung](./media/api-management-howto-aad/find-your-app.png)
22. Nach dem Registrieren der Anwendung wechseln Sie zu **Antwort-URL** und stellen sicher, dass die „Umleitungs-URL“ auf den in Schritt 9 erhaltenen Wert festgelegt ist. 
23. Wenn Sie Ihre Anwendung konfigurieren möchten (z.B. wenn Sie die **App-ID-URL** ändern möchten), wählen Sie **Eigenschaften** aus.

    ![App-Registrierung](./media/api-management-howto-aad/api-management-with-aad004.png)

    Wenn mehrere Azure Active Directory-Verzeichnisse für diese Anwendung verwendet werden sollen, klicken Sie bei der Einstellung **Die Anwendung ist mehrinstanzenfähig** auf **Ja**. Der Standardwert lautet **Nein**.
24. Legen Sie die Anwendungsberechtigungen fest, indem Sie **Erforderliche Berechtigungen** auswählen.
25. Wählen Sie Ihre Anwendung aus, und aktivieren Sie die Berechtigungen **Verzeichnisdaten lesen** und **Anmelden und Benutzerprofil lesen**.

    ![App-Registrierung](./media/api-management-howto-aad/api-management-with-aad005.png)

    Weitere Informationen zu Anwendungen und delegierten Berechtigungen finden Sie unter [Zugreifen auf die Graph-API][Accessing the Graph API].
26. Kopieren Sie im linken Fenster den Wert für **Anwendungs-ID**.

    ![App-Registrierung](./media/api-management-howto-aad/application-id.png)
27. Wechseln Sie zurück zu Ihrer API Management-Anwendung. Hier sollte das Fenster **Identitätsanbieter hinzufügen** angezeigt werden. <br/>Fügen Sie den Wert der **Anwendungs-ID** in das Feld **Client-ID** ein.
28. Wechseln Sie zurück zur Azure Active Directory-Konfiguration, und klicken Sie auf **Schlüssel**.
29. Erstellen Sie einen neuen Schlüssel, indem Sie einen Namen und eine Dauer angeben. 
30. Klicken Sie auf **Speichern**. Der Schlüssel wird generiert.

    Kopieren Sie den Schlüssel in die Zwischenablage.

    ![App-Registrierung](./media/api-management-howto-aad/api-management-with-aad006.png)

    > [!NOTE]
    > Notieren Sie sich diesen Schlüssel. Nachdem Sie das Fenster für die Azure Active Directory-Konfiguration geschlossen haben, ist eine Anzeige des Schlüssels nicht mehr möglich.
    > 
    > 
31. Wechseln Sie zurück zu Ihrer API Management-Anwendung. <br/>Fügen Sie den Schlüssel im Fenster **Identitätsanbieter hinzufügen** in das Feld **Geheimer Clientschlüssel** ein.
32. Das Fenster **Identitätsanbieter hinzufügen** enthält das Textfeld **Zulässige Mandanten**, in dem Sie die Verzeichnisse angeben, die Zugriff auf die APIs der API Management-Dienstinstanz haben. <br/>Geben Sie die Domänen der Azure Active Directory-Instanzen an, denen Sie Zugriff erteilen möchten. Sie können mehrere Domänen durch neue Zeilen, Leerzeichen oder Kommas trennen.

    Im Abschnitt **Zulässige Mandanten** können mehrere Domänen angegeben werden. Bevor sich Benutzer aus einer anderen Domäne als der ursprünglichen Domäne (der Domäne, in der die Anwendung registriert wurde) anmelden können, muss ein globaler Administrator dieser anderen Domäne der Anwendung Berechtigungen für den Verzeichnisdatenzugriff erteilen. Zum Erteilen der Berechtigung wechselt der globale Administrator zu `https://<URL of your developer portal>/aadadminconsent` (z.B.: https://contoso.portal.azure-api.net/aadadminconsent), gibt den Domänennamen des Active Directory-Mandanten ein, für den Zugriff gewährt werden soll, und klickt auf „Absenden“. Im folgenden Beispiel versucht ein globaler Administrator von `miaoaad.onmicrosoft.com`, Berechtigungen für dieses bestimmte Entwicklerportal zu erteilen. 

33. Nachdem die gewünschte Konfiguration angegeben wurde, klicken Sie auf **Hinzufügen**.

    ![App-Registrierung](./media/api-management-howto-aad/api-management-with-aad007.png)

Sobald die Änderungen gespeichert wurden, können sich Benutzer aus dem angegebenen Azure Active Directory-Verzeichnis beim Entwicklerportal anmelden, indem sie die unter [Anmelden beim Entwicklerportal mit einem Azure Active Directory-Konto](#log_in_to_dev_portal) beschriebenen Schritte ausführen.

![Berechtigungen](./media/api-management-howto-aad/api-management-aad-consent.png)

Im nächsten Bildschirm wird der globale Administrator aufgefordert, die Erteilung der Berechtigung zu bestätigen. 

![Berechtigungen](./media/api-management-howto-aad/api-management-permissions-form.png)

Wenn ein nicht globaler Administrator versucht, sich anzumelden, bevor Berechtigungen durch einen globalen Administrator erteilt wurden, tritt ein Anmeldefehler auf, und ein Fehlerbildschirm wird angezeigt.

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Hinzufügen einer externen Azure Active Directory-Gruppe

Nachdem der Zugriff für Benutzer in einem Azure Active Directory-Verzeichnis aktiviert wurde, können Sie Azure Active Directory-Gruppen in API Management hinzufügen, um die Zuweisung der Entwickler in der Gruppe zu den gewünschten Produkten zu vereinfachen.

Um eine externe Azure Active Directory-Gruppe zu konfigurieren, muss Azure Active Directory zunächst – wie in der Vorgehensweise im vorherigen Abschnitt beschrieben – über die Registerkarte „Identitäten“ konfiguriert werden. 

Externe Azure Active Directory-Gruppen werden über die Registerkarte **Gruppen** Ihrer API Management-Instanz hinzugefügt.

![Gruppen](./media/api-management-howto-aad/api-management-with-aad008.png)

1. Wählen Sie die Registerkarte **Gruppen** .
2. Klicken Sie auf die Schaltfläche **AAD-Gruppe hinzufügen**.
3. Wählen Sie die Gruppe aus, die Sie hinzufügen möchten.
4. Klicken Sie auf die Schaltfläche **Auswählen**.

Nach dem Erstellen einer Azure Active Directory-Gruppe und dem Hinzufügen von externen Gruppen können Sie die Eigenschaften dieser Gruppen prüfen und konfigurieren, indem Sie auf der Registerkarte **Gruppen** auf den Namen der Gruppe klicken.

Auf dieser Registerkarte können Sie **Name** und **Beschreibung** der Gruppe bearbeiten.
 
Benutzer aus dem konfigurierten Azure Active Directory-Verzeichnis können sich beim Entwicklerportal anmelden und beliebige Gruppen abonnieren, die sie anzeigen können. Hierbei müssen die Anweisungen im folgenden Abschnitt befolgt werden.

## <a name="a-idlogintodevportalhow-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a><a id="log_in_to_dev_portal"/>So melden Sie sich mit einem Azure Active Directory-Konto beim Entwicklerportal an

Um sich mit einem im vorherigen Abschnitt konfigurierten Azure Active Directory-Konto beim Entwicklerportal anzumelden, öffnen Sie ein neues Browserfenster mit der **Anmelde-URL** aus der Active Directory-Anwendungskonfiguration, und klicken Sie auf **Azure Active Directory**.

![Entwicklerportal][api-management-dev-portal-signin]

Geben Sie die Anmeldeinformationen eines Benutzer in Ihrem Azure Active Directory-Verzeichnis ein, und klicken Sie auf **Anmelden**.

![Anmelden][api-management-aad-signin]

Es wird möglicherweise ein Registrierungsformular angezeigt, wenn weitere Informationen erforderlich sind. Vervollständigen Sie das Registrierungsformular, und klicken Sie auf **Registrieren**.

![Registrierung][api-management-complete-registration]

Der Benutzer ist jetzt beim Entwicklerportal für Ihre API Management-Dienstinstanz angemeldet.

![Registrierung abgeschlossen][api-management-registration-complete]

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

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account
