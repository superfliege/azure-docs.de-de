---
title: 'Tutorial: Azure Active Directory-Integration mit KnowledgeOwl | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und KnowledgeOwl konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2ae30996-864d-4872-90bc-f770e1ea159a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: e902f5969611dd3b1074e899003abe5067857c04
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39446138"
---
# <a name="tutorial-azure-active-directory-integration-with-knowledgeowl"></a>Tutorial: Azure Active Directory-Integration mit KnowledgeOwl

In diesem Tutorial erfahren Sie, wie Sie KnowledgeOwl in Azure Active Directory (Azure AD) integrieren.

Die Integration von KnowledgeOwl in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf KnowledgeOwl hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei KnowledgeOwl anzumelden (einmaliges Anmelden; Single Sign On, SSO).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit KnowledgeOwl konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein KnowledgeOwl-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von KnowledgeOwl aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-knowledgeowl-from-the-gallery"></a>Hinzufügen von KnowledgeOwl aus dem Katalog
Zum Konfigurieren der Integration von KnowledgeOwl in Azure AD müssen Sie KnowledgeOwl aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um KnowledgeOwl aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld **KnowledgeOwl** ein, wählen Sie im Ergebnisbereich **KnowledgeOwl** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![KnowledgeOwl in der Ergebnisliste](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit KnowledgeOwl anhand eines Testbenutzers namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in KnowledgeOwl als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in KnowledgeOwl muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD mit KnowledgeOwl müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Erstellen eines KnowledgeOwl-Testbenutzers](#create-a-knowledgeowl-test-user)**, um eine Entsprechung von Britta Simon in KnowledgeOwl zu erhalten, die mit der Darstellung des Benutzers in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer KnowledgeOwl-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit KnowledgeOwl die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **KnowledgeOwl** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für KnowledgeOwl** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![Informationen zur Domäne und zu den URLs für KnowledgeOwl für das einmalige Anmelden](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_url.png)

    a. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL nach folgendem Muster ein:
    |||
    |-|-|
    | `https://app.knowledgeowl.com/sp`|
    | `https://app.knowledgeowl.com/sp/id/<unique ID>`|
    |||

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein:
    |||
    |-|-|
    | `https://subdomain.knowledgeowl.com/help/saml-login`|
    | `https://subdomain.knowledgeowl.com/docs/saml-login`|
    | `https://subdomain.knowledgeowl.com/home/saml-login`|
    | `https://privatedomain.com/help/saml-login`|
    | `https://privatedomain.com/docs/saml-login`|
    | `https://privatedomain.com/home/saml-login`|
    |||

1. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![Informationen zur Domäne und zu den URLs für KnowledgeOwl für das einmalige Anmelden](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein:
    |||
    |-|-|
    | `https://subdomain.knowledgeowl.com/help/saml-login`|
    | `https://subdomain.knowledgeowl.com/docs/saml-login`|
    | `https://subdomain.knowledgeowl.com/home/saml-login`|
    | `https://privatedomain.com/help/saml-login`|
    | `https://privatedomain.com/docs/saml-login`|
    | `https://privatedomain.com/home/saml-login`|
    |||
     
    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Sie müssen diesen Wert durch die tatsächlichen Werte für Bezeichner, Antwort-URL und Anmelde-URL ersetzen. Dies wird an späterer Stelle im Tutorial erläutert.

1. Die KnowledgeOwl-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten.

    ![Configure single sign-on](./media/knowledgeowl-tutorial/attribute.png)

1. Konfigurieren Sie das SAML-Tokenattribut im Abschnitt **Benutzerattribute** im Dialogfeld **Einmaliges Anmelden**, wie in der vorherigen Abbildung gezeigt, und führen Sie die folgenden Schritte durch:
    
    | Attributname | Attributwert | Namespace|
    | ------------------- | -------------------- | -----|
    | SSO-ID | user.mail | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|
    
    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.
    
    ![Configure single sign-on](./media/knowledgeowl-tutorial/tutorial_attribute_04.png)

    ![Configure single sign-on](./media/knowledgeowl-tutorial/tutorial_attribute_05.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.

    Geben Sie in der Liste **Namespace** den für diese Zeile angezeigten Namespacewert ein.
    
    e. Klicken Sie auf **OK**.

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Rohdaten)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/knowledgeowl-tutorial/tutorial_general_400.png)
    
1. Klicken Sie im Abschnitt **KnowledgeOwl-Konfiguration** auf **KnowledgeOwl konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![KnowledgeOwl-Konfiguration](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_configure.png)

1. Melden Sie sich in einem anderen Webbrowserfenster bei der KnowledgeOwl-Unternehmenswebsite als Administrator an.

1. Klicken Sie auf **Settings** (Einstellungen), und wählen Sie dann **Security** (Sicherheit) aus.

    ![KnowledgeOwl-Konfiguration](./media/knowledgeowl-tutorial/configure1.png)

1. Scrollen Sie nach unten bis zu **SAML SSO Integration**, und führen Sie die folgenden Schritte aus:
    
    ![KnowledgeOwl-Konfiguration](./media/knowledgeowl-tutorial/configure2.png)

    a. Wählen Sie **Enable SAML SSO** (SAML-SSO aktivieren) aus.

    b. Kopieren Sie den Wert **SP Entity ID** (SP-Entitäts-ID), und fügen Sie ihn im Azure-Portal im Abschnitt **Domänen und URLs für KnowledgeOwl** im Textfeld **Bezeichner (Entitäts-ID)** ein.

    c. Kopieren Sie den Wert **SP Login URL** (SP-Anmelde-URL), und fügen Sie ihn im Azure-Portal im Abschnitt **Domänen und URLs für KnowledgeOwl** in den Textfeldern **Anmelde-URL und Antwort-URL** ein.

    d. Fügen Sie im Textfeld **IdP entityID** (IdP-Entitäts-ID) den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.

    e. Fügen Sie im Textfeld **IdP Login URL** (IdP-Anmelde-URL) den Wert der **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.

    f. Fügen Sie im Textfeld **IdP Logout URL** (IdP-Abmelde-URL) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    g. Laden Sie das heruntergeladene Zertifikat aus dem Azure-Portal hoch, indem Sie auf **Upload IdP Certificate** (IdP-Zertifikat hochladen) klicken.

    h. Klicken Sie auf **Map SAML Attributes** (SAML-Attribute zuordnen), um Attribute zuzuordnen, und führen Sie die folgenden Schritte aus:
    
    ![KnowledgeOwl-Konfiguration](./media/knowledgeowl-tutorial/configure3.png)

    * Geben Sie `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/ssoid` im Textfeld **SSO ID** ein.
    * Geben Sie `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` im Textfeld **Username/Email** (Benutzername/E-Mail) ein.
    * Geben Sie `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` im Textfeld **First Name** (Vorname) ein.
    * Geben Sie `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` im Textfeld **Last Name** (Nachname) ein.
    * Klicken Sie unten auf der Seite auf **Speichern**.

    i. Klicken Sie unten auf der Seite auf **Speichern** .

    ![KnowledgeOwl-Konfiguration](./media/knowledgeowl-tutorial/configure4.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/knowledgeowl-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/knowledgeowl-tutorial/create_aaduser_02.png)

1. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/knowledgeowl-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/knowledgeowl-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-knowledgeowl-test-user"></a>Erstellen eines KnowledgeOwl-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in KnowledgeOwl. KnowledgeOwl unterstützt die Just-In-Time-Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf KnowledgeOwl ein neuer Benutzer erstellt.
>[!Note]
>Setzen Sie sich mit dem [KnowledgeOwl-Supportteam](mailto:support@knowledgeowl.com) in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf KnowledgeOwl gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon zu KnowledgeOwl zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **KnowledgeOwl**aus.

    ![KnowledgeOwl-Link in der Anwendungsliste](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_app.png)  

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „KnowledgeOwl“ klicken, sollten Sie automatisch in Ihrer KnowledgeOwl-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/knowledgeowl-tutorial/tutorial_general_01.png
[2]: ./media/knowledgeowl-tutorial/tutorial_general_02.png
[3]: ./media/knowledgeowl-tutorial/tutorial_general_03.png
[4]: ./media/knowledgeowl-tutorial/tutorial_general_04.png

[100]: ./media/knowledgeowl-tutorial/tutorial_general_100.png

[200]: ./media/knowledgeowl-tutorial/tutorial_general_200.png
[201]: ./media/knowledgeowl-tutorial/tutorial_general_201.png
[202]: ./media/knowledgeowl-tutorial/tutorial_general_202.png
[203]: ./media/knowledgeowl-tutorial/tutorial_general_203.png

