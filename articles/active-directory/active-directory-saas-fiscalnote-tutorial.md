---
title: 'Tutorial: Azure Active Directory-Integration von FiscalNote | Microsoft Docs'
description: Erfahren Sie, wie Sie Einmaliges Anmelden (Single Sign-On, SSO) zwischen Azure Active Directory und FiscalNote konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 55274f26-be7e-4514-964c-7186ecb55c4a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: 2e0d0f2d6fc428dbeb20ce5e056a8d6ed9a0ac97
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="tutorial-azure-active-directory-integration-with-fiscalnote"></a>Tutorial: Azure Active Directory-Integration von FiscalNote

In diesem Tutorial erfahren Sie, wie Sie FiscalNote in Azure Active Directory (Azure AD) integrieren.

Die Integration von FiscalNote in Azure AD bietet Ihnen die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf FiscalNote hat.
- Sie können es Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei FiscalNote anzumelden („Einmaliges Anmelden“).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration von FiscalNote konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein FiscalNote-Abonnement, für das Einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von FiscalNote aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-fiscalnote-from-the-gallery"></a>Hinzufügen von FiscalNote aus dem Katalog
Zum Konfigurieren der Integration von FiscalNote in Azure AD müssen Sie FiscalNote aus dem Katalog zu Ihrer Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um FiscalNote aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld den Namen **FiscalNote** ein, wählen Sie im Ergebnisbereich das Element **FiscalNote** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![FiscalNote in der Ergebnisliste](./media/active-directory-saas-fiscalnote-tutorial/tutorial_fiscalnote_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei FiscalNote mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in FiscalNote als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in FiscalNote muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens mit Azure AD bei FiscalNote müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines FiscalNote-Testbenutzers](#create-a-fiscalnote-test-user)**, um eine Entsprechung von Britta Simon in FiscalNote zu erhalten, die mit der Darstellung des Benutzers in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer FiscalNote-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens mit Azure AD bei FiscalNote die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **FiscalNote** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-fiscalnote-tutorial/tutorial_fiscalnote_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für FiscalNote** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für FiscalNote](./media/active-directory-saas-fiscalnote-tutorial/tutorial_fiscalnote_url.png)
    
    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<InstanceName>.fiscalnote.com/login?client=<ClientID>&redirect_uri=https://app.fiscalnote.com/saml-login.html&audience=https://api.fiscalnote.com/&connection=<CONNECTION_NAME>&response_type=id_token%20token`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `urn:auth0:fiscalnote:<CONNECTIONNAME>`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Kundensupportteam von FiscalNote](mailto:support@fiscalnote.com), um diese Werte zu erhalten.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Rohdaten)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-fiscalnote-tutorial/tutorial_fiscalnote_certificate.png)

5. Die FiscalNote-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-fiscalnote-tutorial/tutorial_attribute.png)

6. Konfigurieren Sie das SAML-Tokenattribut im Abschnitt **Benutzerattribute** im Dialogfeld **Einmaliges Anmelden**, wie in der vorherigen Abbildung gezeigt, und führen Sie die folgenden Schritte durch:
           
    | Attributname | Attributwert |
    | ---------------| ----------------|
    | name | user.userprincipalname|
    | givenName| user.givenname|
    | familyName| user.surname|
    | E-Mail| user.mail|
    
    a. Entfernen Sie vorhandene Attribute, und fügen Sie neue Attribute hinzu. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Attribut hinzufügen** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-fiscalnote-tutorial/tutorial_attribute_04.png)
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-fiscalnote-tutorial/tutorial_attribute_05.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.

    d. Lassen Sie das Feld für den Namespace leer.
    
    e. Klicken Sie auf **OK**.

7. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_400.png)

8. Klicken Sie im Abschnitt **FiscalNote-Konfiguration** auf **FiscalNote konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![FiscalNote-Konfiguration](./media/active-directory-saas-fiscalnote-tutorial/tutorial_fiscalnote_configure.png) 

9. Zum Konfigurieren des einmaligen Anmeldens bei **FiscalNote** müssen Sie das heruntergeladene **Zertifikat (Rohdaten), Abmelde-URL, SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** an das [FiscalNote-Supportteam](mailto:support@fiscalnote.com) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-fiscalnote-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-fiscalnote-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-fiscalnote-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-fiscalnote-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-fiscalnote-test-user"></a>Erstellen eines FiscalNote-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in FiscalNote. FiscalNote unterstützt die Just-In-Time-Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf FiscalNote ein neuer Benutzer erstellt.
>[!Note]
>Wenden Sie sich an das [Supportteam von FiscalNote](mailto:support@fiscalnote.com), wenn Sie einen Benutzer manuell erstellen müssen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf FiscalNote gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um FiscalNote Britta Simon als Benutzer zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **FiscalNote** aus.

    ![FiscalNote-Link in der Anwendungsliste](./media/active-directory-saas-fiscalnote-tutorial/tutorial_fiscalnote_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „FiscalNote“ klicken, sollten Sie automatisch bei Ihrer FiscalNote-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_203.png

