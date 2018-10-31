---
title: 'Tutorial: Azure Active Directory-Integration mit GitHub | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und GitHub konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8761f5ca-c57c-4a7e-bf14-ac0421bd3b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeedes
ms.openlocfilehash: dbd4634c575fd4f1886d3e7714ef9ddabbde0f8a
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341156"
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>Tutorial: Azure Active Directory-Integration mit GitHub

In diesem Tutorial erfahren Sie, wie Sie GitHub in Azure Active Directory (Azure AD) integrieren.

Die Integration von GitHub in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf GitHub hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei GitHub anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit GitHub konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein GitHub-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von GitHub aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-github-from-the-gallery"></a>Hinzufügen von GitHub aus dem Katalog
Zum Konfigurieren der Integration von GitHub in Azure AD müssen Sie GitHub aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um GitHub aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![image](./media/github-tutorial/selectazuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![image](./media/github-tutorial/a_select_app.png)
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![image](./media/github-tutorial/a_new_app.png)

4. Geben Sie im Suchfeld **GitHub** ein, wählen Sie im Ergebnisbereich **GitHub** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![image](./media/github-tutorial/tutorial_github_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei GitHub mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in GitHub als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in GitHub muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei GitHub müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines GitHub-Testbenutzers](#create-a-github-test-user)**, um eine Entsprechung für Britta Simon in GitHub zu haben, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer GitHub-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in GitHub die folgenden Schritte aus:**

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **GitHub** auf **Einmaliges Anmelden**.

    ![image](./media/github-tutorial/b1_b2_select_sso.png)

2. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![image](./media/github-tutorial/b1_b2_saml_sso.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![image](./media/github-tutorial/b1-domains_and_urlsedit.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![image](./media/github-tutorial/tutorial_github_url.png) 

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://github.com/orgs/<entity-id>/sso`

    b. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL nach folgendem Muster ein: `https://github.com/orgs/<entity-id>`

    > [!NOTE]
    > Hinweis: Hierbei handelt es sich um Beispielwerte. Die Werte müssen durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner ersetzt werden. Hier empfehlen wir Ihnen, den eindeutigen Wert der Zeichenfolge im Bezeichner zu verwenden. Navigieren Sie zum Abschnitt „GitHub Admin“ (GitHub-Verwaltung), um diese Werte abzurufen.

5. Die GitHub-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Klicken Sie auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Benutzerattribute** zu öffnen.

    ![image](./media/github-tutorial/i3-attribute.png)

6. Konfigurieren Sie im Dialogfeld **Benutzerattribute** im Abschnitt **Benutzeransprüche** das SAML-Tokenattribut wie in der obigen Abbildung gezeigt, und führen Sie die folgenden Schritte aus:
    
    a. Klicken Sie auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    ![image](./media/github-tutorial/i2-attribute.png)

    ![image](./media/github-tutorial/i4-attribute.png)

    b. Wählen Sie in der Liste **Quellattribut** den Attributwert aus.

    c. Klicken Sie auf **Speichern**.
 
7. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das **Zertifikat (Base64)** herunterzuladen, und speichern Sie es auf Ihrem Computer.

    ![image](./media/github-tutorial/tutorial_github_certficate.png)

8. Kopieren Sie im Abschnitt **GitHub einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

    ![image](./media/github-tutorial/d1_samlsonfigure.png) 

9. Melden Sie sich in einem anderen Webbrowserfenster bei der GitHub-Organisationswebsite als Administrator an.

10. Navigieren Sie zu **Einstellungen**, und klicken Sie auf **Sicherheit**.

    ![Einstellungen](./media/github-tutorial/tutorial_github_config_github_03.png)

11. Aktivieren Sie das Kontrollkästchen **SAML-Authentifizierung aktivieren**, um die Felder für die Konfiguration des einmaligen Anmeldens anzuzeigen. Verwenden Sie anschließend den Wert für die SSO-URL, um die URL für das einmalige Anmelden in der Azure AD-Konfiguration zu aktualisieren.

    ![Einstellungen](./media/github-tutorial/tutorial_github_config_github_13.png)

12. Konfigurieren Sie die folgenden Felder:

    ![Einstellungen](./media/github-tutorial/tutorial_github_config_github_051.png)

    a. Fügen Sie im Textfeld **Sign-On URL** (Anmelde-URL) den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    b. Fügen Sie in das Textfeld **Issuer** (Aussteller) den **Azure AD-Bezeichner** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Öffnen Sie das aus dem Azure-Portal heruntergeladene Zertifikat im Editor, und fügen Sie es in das Textfeld **Public Certificate** (Öffentliches Zertifikat) ein.

    d. Klicken Sie auf das Symbol **Bearbeiten**, um die **Signaturmethode** und die **Digestmethode** von **RSA-SHA1** und **SHA1** in **RSA-SHA256** und **SHA256** wie unten gezeigt zu ändern.

    ![image](./media/github-tutorial/tutorial_github_sha.png) 
    
13. Klicken Sie auf **Test SAML configuration** (SAML-Konfiguration testen), um sich zu vergewissern, dass beim einmaligen Anmelden keine Validierungsfehler oder anderen Fehler auftreten.

    ![Einstellungen](./media/github-tutorial/tutorial_github_config_github_06.png)

14. Klicken Sie unten auf der Seite auf **Speichern**.

> [!NOTE]
> Einmaliges Anmelden auf GitHub führt die Authentifizierung für eine bestimmte Organisation in GitHub aus und ersetzt nicht die Authentifizierung von GitHub selbst. Wenn die GitHub.com-Sitzung des Benutzers abgelaufen ist, werden Sie während des SSO-Prozesses möglicherweise aufgefordert, sich mit der ID/Passwort von GitHub zu authentifizieren.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![image](./media/github-tutorial/d_users_and_groups.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![image](./media/github-tutorial/d_adduser.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![image](./media/github-tutorial/d_userproperties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-github-test-user"></a>Erstellen eines GitHub-Testbenutzers

In diesem Abschnitt wird in GitHub eine Benutzerin namens Britta Simon erstellt. GitHub unterstützt die automatische Benutzerbereitstellung, die standardmäßig aktiviert ist. Weitere Details zur Konfiguration der automatischen Benutzerbereitstellung finden Sie [hier](github-provisioning-tutorial.md).

**Wenn Sie einen Benutzer manuell erstellen möchten, führen Sie die folgenden Schritte aus:**

1. Melden Sie sich bei der GitHub-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf **People**.

    ![Personen](./media/github-tutorial/tutorial_github_config_github_08.png "Personen")

3. Klicken Sie auf **Invite member** (Mitglied einladen).

    ![Benutzer einladen](./media/github-tutorial/tutorial_github_config_github_09.png "Benutzer einladen")

4. Führen Sie auf der Dialogfeldseite **Invite member** (Mitglied einladen) die folgenden Schritte aus:

    a. Geben Sie im Textfeld **E-Mail** die E-Mail-Adresse des Kontos von Britta Simon ein.

    ![Personen einladen](./media/github-tutorial/tutorial_github_config_github_10.png "Personen einladen")

    b. Klicken Sie auf **Einladung senden**.

    ![Personen einladen](./media/github-tutorial/tutorial_github_config_github_11.png "Personen einladen")

    > [!NOTE]
    > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail und folgt einem Link zur Bestätigung des Kontos, bevor es aktiv wird.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt gewähren Sie Britta Simon Zugriff auf GitHub, damit sie das einmalige Anmelden von Azure verwenden kann.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![image](./media/github-tutorial/d_all_applications.png)

2. Wählen Sie in der Anwendungsliste **GitHub** aus.

    ![image](./media/github-tutorial/tutorial_github_app.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![image](./media/github-tutorial/d_leftpaneusers.png)

4. Wählen Sie die Schaltfläche **Hinzufügen** und dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![image](./media/github-tutorial/d_assign_user.png)

4. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

5. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die GitHub-Kachel klicken, werden Sie automatisch bei Ihrer GitHub-Anwendung angemeldet.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


