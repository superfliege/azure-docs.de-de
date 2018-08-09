---
title: 'Tutorial: Azure Active Directory-Integration mit XaitPorter | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und XaitPorter konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d33c7cb7-0550-425b-882a-619a713a71b7
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: jeedes
ms.openlocfilehash: 12fb8e5b2b940c48de766a48f59ed0cc342b5356
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421065"
---
# <a name="tutorial-azure-active-directory-integration-with-xaitporter"></a>Tutorial: Azure Active Directory-Integration mit XaitPorter

In diesem Tutorial erfahren Sie, wie Sie XaitPorter in Azure Active Directory (Azure AD) integrieren.

Die Integration von XaitPorter in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf XaitPorter hat.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei XaitPorter anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit XaitPorter konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein XaitPorter-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von XaitPorter aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-xaitporter-from-the-gallery"></a>Hinzufügen von XaitPorter aus dem Katalog
Zum Konfigurieren der Integration von XaitPorter in Azure AD müssen Sie XaitPorter aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um XaitPorter aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld **XaitPorter** ein, wählen Sie im Ergebnisbereich **XaitPorter** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![XaitPorter in der Ergebnisliste](./media/xaitporter-tutorial/tutorial_xaitporter_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit XaitPorter anhand eines Testbenutzers mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in XaitPorter als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in XaitPorter muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in XaitPorter den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei XaitPorter müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Erstellen eines XaitPorter-Testbenutzers](#create-a-xaitporter-test-user)**, um ein Pendant von Britta Simon in XaitPorter zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer XaitPorter-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei XaitPorter die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **XaitPorter** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/xaitporter-tutorial/tutorial_xaitporter_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für XaitPorter** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für XaitPorter](./media/xaitporter-tutorial/tutorial_xaitporter_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.xaitporter.com/saml/login`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<subdomain>.xaitporter.com`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam für den XaitPorter-Client](https://www.xait.com/support/), um diese Werte zu erhalten.
     
1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf die Schaltfläche „Kopieren“, um die **Verbundmetadaten-URL der App** zu kopieren und in Editor einzufügen. 

    ![Downloadlink für das Zertifikat](./media/xaitporter-tutorial/tutorial_xaitporter_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/xaitporter-tutorial/tutorial_general_400.png)

1. Geben Sie die **IP-Adresse** oder **Verbundmetadaten-URL der App** an das [SmartRecruiters-Supportteam](https://www.smartrecruiters.com/about-us/contact-us/) weiter, damit es die Whitelist so konfigurieren kann, dass die IP-Adresse über Ihre XaitPorter-Instanz erreichbar ist. 

1. Melden Sie sich in einem anderen Webbrowserfenster bei der XaitPorter-Unternehmenswebsite als Administrator an.

1. Klicken Sie auf **Admin**.

    ![Configure single sign-on](./media/xaitporter-tutorial/user1.png)

1. Wählen Sie in der Dropdownliste **System Setup** (Systemsetup) die Option **Manage Single Sign-On** (Einmaliges Anmelden verwalten) aus.

    ![Configure single sign-on](./media/xaitporter-tutorial/user2.png)

1. Führen Sie im Abschnitt **MANAGE SINGLE SIGN-ON** (EINMALIGES ANMELDEN VERWALTEN) die folgenden Schritte aus:

    ![Configure single sign-on](./media/xaitporter-tutorial/user3.png)

    a. Aktivieren Sie **Enable Single Sign-On Authentication** (SSO-Authentifizierung aktivieren).

    b. Fügen Sie im Textfeld **Identity Provider Settings** (Identitätsanbietereinstellungen) die **Verbundmetadaten-URL der App** ein, die Sie im Azure-Portal kopiert haben, und klicken Sie auf **Fetch** (Abrufen).

    c. Aktivieren Sie **Enable Autocreation of Users** (Automatische Erstellung von Benutzern aktivieren).

    d. Klicken Sie auf **OK**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/xaitporter-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/xaitporter-tutorial/create_aaduser_02.png)

1. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/xaitporter-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/xaitporter-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-xaitporter-test-user"></a>Erstellen eines XaitPorter-Testbenutzers

In diesem Abschnitt erstellen Sie in XaitPorter einen Benutzer mit dem Namen Britta Simon. Wenden Sie sich an das [Supportteam für den XaitPorter-Client](https://www.xait.com/support/), um die Benutzer auf der XaitPorter-Plattform hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können. 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf XaitPorter gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon XaitPorter zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **XaitPorter** aus.

    ![XaitPorter-Link in der Anwendungsliste](./media/xaitporter-tutorial/tutorial_xaitporter_app.png)  

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „XaitPorter“ klicken, sollten Sie automatisch bei Ihrer XaitPorter-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/xaitporter-tutorial/tutorial_general_01.png
[2]: ./media/xaitporter-tutorial/tutorial_general_02.png
[3]: ./media/xaitporter-tutorial/tutorial_general_03.png
[4]: ./media/xaitporter-tutorial/tutorial_general_04.png

[100]: ./media/xaitporter-tutorial/tutorial_general_100.png

[200]: ./media/xaitporter-tutorial/tutorial_general_200.png
[201]: ./media/xaitporter-tutorial/tutorial_general_201.png
[202]: ./media/xaitporter-tutorial/tutorial_general_202.png
[203]: ./media/xaitporter-tutorial/tutorial_general_203.png

