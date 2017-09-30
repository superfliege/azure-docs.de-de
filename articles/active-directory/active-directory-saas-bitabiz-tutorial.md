---
title: 'Tutorial: Azure Active Directory-Integration mit BitaBIZ | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und BitaBIZ konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1a51e677-c62b-4aee-9c61-56926aaaa899
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: a29f1e7b39b7f35073aa5aa6c6bd964ffaa6ffd0
ms.openlocfilehash: dca0a460224436d3886cf9a9c354ce662f99ae84
ms.contentlocale: de-de
ms.lasthandoff: 09/21/2017

---
# <a name="tutorial-azure-active-directory-integration-with-bitabiz"></a>Tutorial: Azure Active Directory-Integration mit BitaBIZ

In diesem Tutorial erfahren Sie, wie Sie BitaBIZ in Azure Active Directory (Azure AD) integrieren.

Die Integration von BitaBIZ in Azure AD hat folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf BitaBIZ haben soll.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei BitaBIZ anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration mit BitaBIZ benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges BitaBIZ-Abonnement

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von BitaBIZ über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-bitabiz-from-the-gallery"></a>Hinzufügen von BitaBIZ über den Katalog
Zum Konfigurieren der Integration von BitaBIZ in Azure AD müssen Sie BitaBIZ aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um BitaBIZ über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld die Zeichenfolge **BitaBIZ** ein, wählen Sie im Ergebnisbereich die Option **BitaBIZ** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![BitaBIZ in der Ergebnisliste](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei BitaBIZ mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in BitaBIZ als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in BitaBIZ muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in BitaBIZ den Wert für **Benutzername** aus Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei BitaBIZ müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines BitaBIZ-Testbenutzers](#create-a-bitabiz-test-user)**, um in BitaBIZ eine Entsprechung von Britta Simon zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren es in Ihrer BitaBIZ-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei BitaBIZ die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **BitaBIZ** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für BitaBIZ** die folgenden Schritte aus, wenn Sie die Anwendung im IDP-initiierten Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für BitaBIZ](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_url.png)

    Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://www.bitabiz.com/<instanceId>`

    > [!NOTE] 
    > Der Wert in der obigen URL dient lediglich zur Veranschaulichung. Ersetzen Sie ihn durch den tatsächlichen Bezeichner. Dies wird später in diesem Tutorial beschrieben.

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für BitaBIZ](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** diese URL ein: `https://www.bitabiz.com/dashboard`

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-bitabiz-tutorial/tutorial_general_400.png)
    
7. Klicken Sie im Abschnitt **BitaBIZ-Konfiguration** auf **BitaBIZ konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![BitaBIZ-Konfiguration](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_configure.png) 

8. Melden Sie sich in einem anderen Webbrowserfenster als Administrator bei Ihrem BitaBIZ-Mandanten an.

9. Klicken Sie auf **SETUP ADMIN** (ADMINISTRATOR EINRICHTEN).

    ![BitaBIZ-Konfiguration](./media/active-directory-saas-bitabiz-tutorial/settings1.png)

10. Klicken Sie im Abschnitt **Add value** (Aufwerten) auf **Microsoft integrations** (Microsoft-Integrationen).

    ![BitaBIZ-Konfiguration](./media/active-directory-saas-bitabiz-tutorial/settings2.png)

11. Scrollen Sie zum Abschnitt **Microsoft Azure AD (Enable single sign on)** (Microsoft Azure AD (einmaliges Anmelden aktivieren)), und führen Sie die folgenden Schritte aus:

    ![BitaBIZ-Konfiguration](./media/active-directory-saas-bitabiz-tutorial/settings3.png)

    a. Kopieren Sie den Wert aus dem Textfeld **Entity ID (”Identifier” in Azure AD)** (Entitäts-ID ("Bezeichner" in Azure AD)), und fügen Sie ihn im Azure-Portal im Abschnitt **Domäne und URLs für BitaBIZ** in das Textfeld **Bezeichner** ein. 
    
    b. Fügen Sie in das Textfeld **Azure AD Single Sign-On Service URL** (Azure AD-Dienst-URL für einmaliges Anmelden) die **SAML-Dienst-URL für einmaliges Anmelden** ein, die Sie aus dem Azure-Portal kopiert haben.
    
    c. Fügen Sie in das Textfeld **Azure AD SAML Entity ID** (Azure AD-SAML-Entitäts-ID) die **SAML-Entitäts-ID** ein, die Sie aus dem Azure-Portal kopiert haben.

    d. Öffnen Sie das heruntergeladene **Zertifikat (Base64)** in Editor, kopieren Sie den Inhalt in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Azure AD Signing Certificate (Base64 encoded)** (Azure AD-Signaturzertifikat (Base64-codiert) ein.

    e. Fügen Sie Ihren geschäftlichen E-Mail-Domänennamen (mycompany.com) in das Textfeld **Domain name** (Domänenname) ein, um SSO den Benutzern in Ihrem Unternehmen mit dieser E-Mail-Domäne zuzuweisen (OPTIONAL).
    
    f. Aktivieren Sie das Kontrollkästchen **SSO enabled** (SSO aktiviert) für das BitaBIZ-Konto.
    
    g. Klicken Sie auf **Save Azure AD configuration** (Azure AD-Konfiguration speichern), um die SSO-Konfiguration zu speichern und zu aktivieren.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-bitabiz-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-bitabiz-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-bitabiz-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-bitabiz-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-bitabiz-test-user"></a>Erstellen eines BitaBIZ-Testbenutzers

Damit sich Azure AD-Benutzer bei BitaBIZ anmelden können, müssen sie in BitaBIZ bereitgestellt werden.  
Im Fall von BitaBIZ muss die Bereitstellung manuell durchgeführt werden.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrer BitaBIZ-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf **SETUP ADMIN** (ADMINISTRATOR EINRICHTEN).

    ![Hinzufügen eines Benutzers in BitaBIZ](./media/active-directory-saas-bitabiz-tutorial/settings1.png)

3. Klicken Sie im Abschnitt **Organization** (Organisation) auf **Add users** (Benutzer hinzufügen).

    ![Hinzufügen eines Benutzers in BitaBIZ](./media/active-directory-saas-bitabiz-tutorial/user1.png)

4. Klicken Sie auf **Add new employee** (Neuen Mitarbeiter hinzufügen).

    ![Hinzufügen eines Benutzers in BitaBIZ](./media/active-directory-saas-bitabiz-tutorial/user2.png)

5. Führen Sie auf der Dialogfeldseite **Add new employee** (Neuen Mitarbeiter hinzufügen) die folgenden Schritte aus:

    ![Hinzufügen eines Benutzers in BitaBIZ](./media/active-directory-saas-bitabiz-tutorial/user3.png)

    a. Geben Sie im Textfeld **First Name** (Vorname) den Vornamen des Benutzers ein (beispielsweise „Britta“).

    b. Geben Sie im Textfeld **Last Name** (Nachname) den Nachnamen des Benutzers ein (beispielsweise „Simon“).

    c. Geben Sie im Textfeld **E-Mail-Adresse** die E-Mail-Adresse des Benutzers, z.B. Brittasimon@contoso.com, ein.

    d. Wählen Sie unter **Date of employment** (Einstellungsdatum) ein Datum aus.

    e. Für den Benutzer können optional noch weitere Benutzerattribute eingerichtet werden. Ausführlichere Informationen finden Sie in der [Dokumentation für die Mitarbeitereinrichtung](https://help.bitabiz.dk/manage-or-set-up-your-account/on-boarding-employees/new-employee).    
    
    f. Klicken Sie auf **Save employee** (Mitarbeiter speichern).
    
    > [!NOTE]
    > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail und folgt einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
    
### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf BitaBIZ gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu BitaBIZ durchzuführen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste die Option **BitaBIZ** aus.

    ![BitaBIZ-Link in der Anwendungsliste](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „BitaBIZ“ klicken, sollten Sie automatisch bei Ihrer BitaBIZ-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_203.png


