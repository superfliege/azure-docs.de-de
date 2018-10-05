---
title: 'Tutorial: Azure Active Directory-Integration mit dmarcian | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und dmarcian konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a04b9383-3a60-4d54-9412-123daaddff3b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2018
ms.author: jeedes
ms.openlocfilehash: 0f8878505280371bf6046c1d1f0d7fc1275dd496
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47039886"
---
# <a name="tutorial-azure-active-directory-integration-with-dmarcian"></a>Tutorial: Azure Active Directory-Integration mit dmarcian

In diesem Tutorial erfahren Sie, wie Sie dmarcian in Azure Active Directory (Azure AD) integrieren.

Die Integration von dmarcian in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf dmarcian hat.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei dmarcian anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit dmarcian konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein dmarcian-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von dmarcian aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-dmarcian-from-the-gallery"></a>Hinzufügen von dmarcian aus dem Katalog
Zum Konfigurieren der Integration von dmarcian in Azure AD müssen Sie dmarcian über den Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um dmarcian über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Unternehmensanwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **dmarcian** ein, wählen Sie im Ergebnisbereich **dmarcian** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![dmarcian in der Ergebnisliste](./media/dmarcian-tutorial/tutorial_dmarcian_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei dmarcian mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in dmarcian als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in dmarcian muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei dmarcian müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines dmarcian-Testbenutzers](#create-a-dmarcian-test-user)**, um ein Pendant von Britta Simon in dmarcian zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer dmarcian-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit dmarcian die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **dmarcian** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/dmarcian-tutorial/tutorial_dmarcian_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für dmarcian** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten** Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für dmarcian](./media/dmarcian-tutorial/tutorial_dmarcian_url.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein:
    | |
    | -- |
    | `https://us.dmarcian.com/sso/saml/<ACCOUNT_ID>/sp.xml` |
    | `https://dmarcian-eu.com/sso/saml/<ACCOUNT_ID>/sp.xml` |
    | `https://dmarcian-ap.com/sso/saml/<ACCOUNT_ID>/sp.xml` |

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein:
    | |
    |--|
    | `https://us.dmarcian.com/login/<ACCOUNT_ID>/handle/` |
    | `https://dmarcian-eu.com/login/<ACCOUNT_ID>/handle/` |
    | `https://dmarcian-ap.com/login/<ACCOUNT_ID>/handle/` |

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für dmarcian](./media/dmarcian-tutorial/tutorial_dmarcian_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein:
    | |
    |--|
    | `https://us.dmarcian.com/login/<ACCOUNT_ID>` |
    | `https://dmarcian-eu.com/login/<ACCOUNT_ID>` |
    | `https://dmarciam-ap.com/login/<ACCOUNT_ID>` |
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte durch die tatsächlichen Werte für Bezeichner, Antwort-URL und Anmelde-URL ersetzen. Darauf wird später im Tutorial eingegangen. 

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf die Schaltfläche „Kopieren“, um die **Verbundmetadaten-URL der App** zu kopieren und in Editor einzufügen.

    ![Downloadlink für das Zertifikat](./media/dmarcian-tutorial/tutorial_dmarcian_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/dmarcian-tutorial/tutorial_general_400.png)
    
7. Melden Sie sich in einem anderen Webbrowserfenster als Sicherheitsadministrator bei dmarcian an.

8. Klicken Sie oben rechts auf **Profile** (Profil), und navigieren Sie zu **Preferences** (Einstellungen).

    ![Einstellungen ](./media/dmarcian-tutorial/tutorial_dmarcian_pref.png)

9. Scrollen Sie nach unten, und klicken Sie auf den Abschnitt **Single Sign-On** (Einmaliges Anmelden) und dann auf **Configure** (Konfigurieren).

    ![Einmaliges Anmelden ](./media/dmarcian-tutorial/tutorial_dmarcian_sso.png)

10. Legen Sie auf der Seite **SAML Single Sign-On** (Einmaliges Anmelden für SAML) den Wert für **Status** auf **Enabled** (Aktiviert) fest, und führen Sie die folgenden Schritte aus:

    ![Authentifizierung ](./media/dmarcian-tutorial/tutorial_dmarcian_auth.png)

    * Klicken Sie im Abschnitt **Add dmarcian to your Identity Provider** (dmarcian zum Identitätsanbieter hinzufügen) auf **Copy** (Kopieren), um die **Assertionsverbraucherdienst-URL** für Ihre Instanz zu kopieren, und fügen Sie sie im Azure-Portal im Abschnitt **Domäne und URLs für dmarcian** ins Textfeld **Antwort-URL** ein.

    * Klicken Sie im Abschnitt **Add dmarcian to your Identity Provider** (dmarcian zum Identitätsanbieter hinzufügen) auf **Copy** (Kopieren), um die **Entität-ID** für Ihre Instanz zu kopieren, und fügen Sie sie im Azure-Portal im Abschnitt **Domäne und URLs für dmarcian** ins Textfeld **Bezeichner** ein.

    * Fügen Sie im Abschnitt **Set up Authentication** (Authentifizierung einrichten) im Textfeld **Identity Provider Metadata** (Metadaten des Identitätsanbieters) die **App-Verbundmetadaten-URL** ein, die Sie aus dem Azure-Portal kopiert haben.

    * Fügen Sie im Abschnitt **Set up Authentication** (Authentifizierung einrichten) im Textfeld **Attribute Statements** (Attributanweisungen) die URL `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` ein.

    * Kopieren Sie im Abschnitt **Set up Login URL** (Anmelde-URL einrichten) die **Anmelde-URL** für Ihre Instanz, und fügen Sie sie im Azure-Portal im Abschnitt **Domäne und URLs für dmarcian** ins Textfeld **Anmelde-URL** ein.

        > [!Note]
        > Sie können die **Anmelde-URL** gemäß Ihrer Organisation anpassen.

    * Klicken Sie auf **Speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/dmarcian-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/dmarcian-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/dmarcian-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/dmarcian-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-dmarcian-test-user"></a>Erstellen eines dmarcian-Testbenutzers

Damit sich Azure AD-Benutzer bei dmarcian anmelden können, müssen sie in dmarcian bereitgestellt werden. Im Fall von dmarcian muss die Bereitstellung manuell ausgeführt werden.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei dmarcian als Sicherheitsadministrator an.

2. Klicken Sie oben rechts auf **Profile** (Profil), und navigieren Sie zu **Manage Users** (Benutzer verwalten).

    ![Benutzer ](./media/dmarcian-tutorial/tutorial_dmarcian_user.png)

3. Klicken Sie rechts im Abschnitt **SSO Users** (SSO-Benutzer) auf **Add New User** (Neuen Benutzer hinzufügen).

    ![Benutzer hinzufügen ](./media/dmarcian-tutorial/tutorial_dmarcian_addnewuser.png)

4. Führen Sie im Popup **Add New User** (Neuen Benutzer hinzufügen) die folgenden Schritte aus:

    ![Neuer Benutzer ](./media/dmarcian-tutorial/tutorial_dmarcian_save.png)

    a. Geben Sie im Textfeld **New User Email** (E-Mail-Adresse des neuen Benutzers) die E-Mail-Adresse des Benutzers ein, etwa **brittasimon@contoso.com**.

    b. Wenn Sie dem Benutzer Administratorrechte gewähren möchten, aktivieren Sie **Make User an Admin** (Benutzer als Administrator festlegen).

    c. Klicken Sie auf **Benutzer hinzufügen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf dmarcian gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon zu dmarcian zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **dmarcian** aus.

    ![dmarcian-Link in der Anwendungsliste](./media/dmarcian-tutorial/tutorial_dmarcian_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „dmarcian“ klicken, sollten Sie automatisch bei Ihrer dmarcian-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/dmarcian-tutorial/tutorial_general_01.png
[2]: ./media/dmarcian-tutorial/tutorial_general_02.png
[3]: ./media/dmarcian-tutorial/tutorial_general_03.png
[4]: ./media/dmarcian-tutorial/tutorial_general_04.png

[100]: ./media/dmarcian-tutorial/tutorial_general_100.png

[200]: ./media/dmarcian-tutorial/tutorial_general_200.png
[201]: ./media/dmarcian-tutorial/tutorial_general_201.png
[202]: ./media/dmarcian-tutorial/tutorial_general_202.png
[203]: ./media/dmarcian-tutorial/tutorial_general_203.png

