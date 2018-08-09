---
title: 'Tutorial: Azure Active Directory-Integration in Zoho One| Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und Zoho One konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bbc3038c-0d8b-45dd-9645-368bd3d01a0f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: jeedes
ms.openlocfilehash: 81e86df270a7286426363c26a0e8a87b99082428
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438272"
---
# <a name="tutorial-azure-active-directory-integration-with-zoho-one"></a>Tutorial: Azure Active Directory-Integration in Zoho One

In diesem Tutorial erfahren Sie, wie Sie Zoho One in Azure Active Directory (Azure AD) integrieren.

Die Integration von Zoho One in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Zoho One hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Zoho One anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in Zoho One konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Zoho One-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Zoho One aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-zoho-one-from-the-gallery"></a>Hinzufügen von Zoho One aus dem Katalog
Zum Konfigurieren der Integration von Zoho One in Azure AD müssen Sie Zoho One aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**So fügen Sie Zoho One aus dem Katalog hinzu**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld **Zoho One** ein, wählen Sie im Ergebnisbereich **Zoho One** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Zoho One in der Ergebnisliste](./media/zohoone-tutorial/tutorial_zohoone_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Zoho One anhand einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Zoho One als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Zoho One-Benutzer muss eine Linkbeziehung eingerichtet werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Zoho One zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Erstellen eines Zoho One-Testbenutzers](#create-a-zoho-one-test-user)**, um eine Entsprechung für Britta Simon in Zoho One zu haben, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Zoho One-Anwendung.

**So konfigurieren Sie das einmalige Anmelden von Azure AD mit Zoho One**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Zoho One** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/zohoone-tutorial/tutorial_zohoone_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für Zoho One** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Zoho One](./media/zohoone-tutorial/tutorial_zohoone_url.png)

    a. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL ein: `one.zoho.com`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://accounts.zoho.com/samlresponse/<saml-identifier>`

    c. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**.

    d. Geben Sie im Textfeld **Relayzustand** eine URL ein: `https://one.zoho.com`

1. Sie können die Anwendung folgendermaßen im **SP-initiierten Modus** konfigurieren:

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://accounts.zoho.com/samlauthrequest/<domain_name>?serviceurl=https://one.zoho.com`.
     
    > [!NOTE] 
    > Der Wert der **Antwort-URL** und der **Anmelde-URL** entspricht nicht dem tatsächlichen Wert. Sie müssen diese Werte durch die tatsächlichen Werte für die Antwort-URL und die Anmelde-URL ersetzen. Darauf wird später im Tutorial eingegangen. 

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/zohoone-tutorial/tutorial_zohoone_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/zohoone-tutorial/tutorial_general_400.png)
    
1. Klicken Sie im Abschnitt **Zoho One-Konfiguration** auf **Zoho One konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Zoho One-Konfiguration](./media/zohoone-tutorial/tutorial_zohoone_configure.png) 

1. Melden Sie sich in einem anderen Webbrowserfenster auf der Zoho One-Unternehmenswebsite als Administrator an.

1. Klicken Sie auf der Registerkarte **Organisation** (Organisation) unter **SAML Authentication** (SAML-Authentifizierung) auf **Setup** (Einrichten).

    ![Organisation in Zoho One](./media/zohoone-tutorial/tutorial_zohoone_setup.png)

1. Führen Sie auf der Popupseite die folgenden Schritte aus:

    ![Anmelden und Abmelden in Zoho One](./media/zohoone-tutorial/tutorial_zohoone_save.png)

    a. Fügen Sie in das Textfeld **Sign-in URL** (Anmelde-URL) den Wert der **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.

    b. Fügen Sie in das Textfeld **Sign-out URL** (Abmelde-URL) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Klicken Sie auf **Browse** (Durchsuchen), um das **Zertifikat (Base64)** hochzuladen, das Sie zuvor aus dem Azure-Portal heruntergeladen haben.

    d. Klicken Sie auf **Speichern**.

1. Wenn Sie die Einrichtung der SAML-Authentifizierung gespeichert haben, kopieren Sie den Wert von **SAML-Identifier** (SAML-Bezeichner), und fügen Sie ihn im Azure-Portal im Abschnitt **Domäne und URLs für Zoho One** unter **Antwort-URL** ein.

    ![SAML in Zoho One](./media/zohoone-tutorial/tutorial_zohoone_samlidenti.png)

1. Öffnen Sie die Registerkarte **Domains** (Domänen), und klicken Sie auf **Add Domain** (Domäne hinzufügen).

    ![Domäne in Zoho One](./media/zohoone-tutorial/tutorial_zohoone_domain.png)

1. Führen Sie auf der Seite **Add Domain** (Domäne hinzufügen) die folgenden Schritte aus:

    ![Hinzufügen einer Domäne in Zoho One](./media/zohoone-tutorial/tutorial_zohoone_adddomain.png)

    a. Geben Sie im Textfeld **Domain Name** (Domänennamen) eine Domäne ein, z.B. **contoso.com**.

    b. Klicken Sie auf **Hinzufügen**.

    >[!Note]
    >Überprüfen Sie Ihre Domäne nach dem Hinzufügen mithilfe [dieser Schritte](https://www.zoho.com/one/help/admin-guide/domain-verification.html). Wenn Ihre Domäne überprüft wurde, geben Sie Ihren Domänennamen im Azure-Portal im Abschnitt **Domäne und URLs für Zoho One** unter **Anmelde-URL** ein.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/zohoone-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/zohoone-tutorial/create_aaduser_02.png)

1. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/zohoone-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/zohoone-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-zoho-one-test-user"></a>Erstellen eines Zoho One-Testbenutzers

Damit sich Azure AD-Benutzer bei Zoho One anmelden können, müssen sie in Zoho One bereitgestellt werden. Im Fall von Zoho One muss die Bereitstellung manuell ausgeführt werden.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei Zoho One als Sicherheitsadministrator an.

1. Klicken Sie auf der Registerkarte **Users** (Benutzer) auf das **Benutzerlogo**.

    ![Benutzer in Zoho One](./media/zohoone-tutorial/tutorial_zohoone_users.png)

1. Führen Sie auf der Seite **Benutzer hinzufügen** die folgenden Schritte aus:

    ![Hinzufügen eines Benutzers in Zoho One](./media/zohoone-tutorial/tutorial_zohoone_adduser.png)
    
    a. Geben Sie im Textfeld **Name** (Name) den Namen des Benutzers ein, z.B. **Britta Simon**.
    
    b. Geben Sie im Textfeld **Email Address** (E-Mail-Adresse) die E-Mail-Adresse des Benutzers ein, z.B. **brittasimon@contoso.com**.

    >[!Note]
    >Wählen Sie aus der Domänenliste Ihre überprüfte Domäne aus.

    c. Klicken Sie auf **Hinzufügen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt gewähren Sie Britta Simon Zugriff auf Zoho One, damit sie das einmalige Anmelden von Azure verwenden kann.

![Zuweisen der Benutzerrolle][200] 

**So weisen Sie Britta Simon Zoho One zu**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **Zoho One** aus.

    ![Zoho One-Link in der Anwendungsliste](./media/zohoone-tutorial/tutorial_zohoone_app.png)  

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Zoho One“ klicken, werden Sie automatisch bei Ihrer Zoho One-Anwendung angemeldet.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/zohoone-tutorial/tutorial_general_01.png
[2]: ./media/zohoone-tutorial/tutorial_general_02.png
[3]: ./media/zohoone-tutorial/tutorial_general_03.png
[4]: ./media/zohoone-tutorial/tutorial_general_04.png

[100]: ./media/zohoone-tutorial/tutorial_general_100.png

[200]: ./media/zohoone-tutorial/tutorial_general_200.png
[201]: ./media/zohoone-tutorial/tutorial_general_201.png
[202]: ./media/zohoone-tutorial/tutorial_general_202.png
[203]: ./media/zohoone-tutorial/tutorial_general_203.png

