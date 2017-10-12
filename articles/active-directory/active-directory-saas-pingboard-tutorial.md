---
title: 'Tutorial: Azure Active Directory-Integration mit Pingboard | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Pingboard konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: jeedes
ms.openlocfilehash: 67534da42ac27989a8b08cec4d6f9f9c31774264
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pingboard"></a>Tutorial: Azure Active Directory-Integration mit Pingboard

In diesem Tutorial erfahren Sie, wie Sie Pingboard in Azure Active Directory (Azure AD) integrieren.

Die Integration von Pingboard in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Pingboard hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Pingboard anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Pingboard konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Pingboard-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Pingboard aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-pingboard-from-the-gallery"></a>Hinzufügen von Pingboard aus dem Katalog
Zum Konfigurieren der Integration von Pingboard in Azure AD müssen Sie Pingboard aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Pingboard aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Unternehmensanwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Pingboard** ein, wählen Sie im Ergebnisbereich **Pingboard** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Pingboard in der Ergebnisliste](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Pingboard basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Pingboard als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Pingboard muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD Wert für den **Benutzernamen** in Pingboard zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Pingboard sind die folgenden Bausteine erforderlich:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Pingboard-Testbenutzers](#create-a-pingboard-test-user)**, um eine Entsprechung von Britta Simon in Pingboard zu erhalten, die mit der Darstellung der Benutzerin in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Pingboard-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Pingboard die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Pingboard** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2.  Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Pingboard** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Pingboard (IDP)](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_url.png)

    a. Geben Sie im Textfeld **Bezeichner** den Wert `http://app.pingboard.com/sp` ein.

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<entity-id>.pingboard.com/auth/saml/consume`

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Pingboard (SP)](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_sp_initiated01.png)

     Geben Sie im Textfeld **Anmelde-URL** die URL im folgenden Format ein: `https://<sub-domain>.pingboard.com/sign_in`.

    > [!NOTE] 
    > Beachten Sie, dass dies keine echten Werte sind. Die Werte müssen durch die tatsächliche Antwort-URL und die tatsächliche Anmelde-URL ersetzt werden. Wenden Sie sich an das [Supportteam von Pingboard](https://support.pingboard.com/), um diese Werte zu erhalten.

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Pingboard-Metadaten-XML](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-pingboard-tutorial/tutorial_general_400.png)

7. Öffnen Sie zum Konfigurieren des einmaligem Anmeldens (SSO) auf Pingboard-Seite ein neues Browserfenster, und melden Sie sich bei Ihrem Pingboard-Konto an. Zum Einrichten des einmaligen Anmeldens müssen Sie ein Pingboard-Administrator sein.

8. Wählen Sie im Menü am oberen Rand die Option **Apps > Integrations**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pingboard-tutorial/Pingboard_integration.png)

9.  Wechseln Sie auf der Seite **Integrations** zur Kachel **Azure Active Directory**, und klicken Sie darauf.

    ![Integration des einmaligen Anmeldens für Pingboard](./media/active-directory-saas-pingboard-tutorial/Pingboard_aad.png)

10. Klicken Sie im geöffneten modalen Fenster auf **Configure**.

    ![Schaltfläche für Pingboard-Konfiguration](./media/active-directory-saas-pingboard-tutorial/Pingboard_configure.png)

11. Auf der folgenden Seite wird „Azure SSO Integration is enabled“ angezeigt. Öffnen Sie die heruntergeladene Metadaten-XML-Datei in Editor, und fügen Sie den Inhalt **IDP Metadata** hinzu.

    ![Bildschirm für die SSO-Konfiguration für Pingboard](./media/active-directory-saas-pingboard-tutorial/Pingboard_sso_configure.png)

12. Die Datei wird überprüft. Wenn alles in Ordnung ist, ist einmaliges Anmelden nun aktiviert.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-pingboard-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-pingboard-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-pingboard-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Dialogfeld „Benutzer“](./media/active-directory-saas-pingboard-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-pingboard-test-user"></a>Erstellen eines Pingboard-Testbenutzers

Damit sich Azure AD-Benutzer bei Pingboard anmelden können, müssen sie in Pingboard bereitgestellt werden. Für Pingboard ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der Unternehmenswebsite von Pingboard als Administrator an.

2. Klicken Sie auf der Seite **Directory** auf die Schaltfläche **Add Employee**.

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-pingboard-tutorial/create_testuser_add.png)

3. Führen Sie auf der Dialogfeldseite **Add Employee** die folgenden Schritte aus:

    ![Invite People](./media/active-directory-saas-pingboard-tutorial/create_testuser_name.png)

    a. Geben Sie im Textfeld **Full Name** den vollständigen Namen des Benutzers, z.B. **Britta Simon**, ein.

    b. Geben Sie im Textfeld **E-Mail-Adresse** die E-Mail-Adresse des Benutzers, z.B. **brittasimon@contoso.com**, ein.

    c. Geben Sie im Textfeld **Job Title** die Position von Britta Simon ein.

    d. Wählen Sie in der Dropdownliste **Location** den Standort von Britta Simon aus.
    
    e. Klicken Sie auf **Hinzufügen**.   

4. Ein Bestätigungsfenster wird eingeblendet, in dem das Hinzufügen der Benutzerin bestätigt wird.
    
    ![Bestätigen](./media/active-directory-saas-pingboard-tutorial/create_testuser_confirm.png)
        
    > [!NOTE]
    > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail und folgt einem Link zur Bestätigung des Kontos, bevor es aktiv wird.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Pingboard gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Pingboard zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Pingboard** aus.

    ![Pingboard-Link in der Anwendungsliste](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

Wenn Sie im Zugriffsbereich auf die Kachel „Pingboard“ klicken, sollten Sie automatisch bei Ihrer Pingboard-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_203.png
