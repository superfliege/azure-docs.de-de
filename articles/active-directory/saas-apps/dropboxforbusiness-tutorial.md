---
title: 'Tutorial: Azure Active Directory-Integration mit Dropbox für Unternehmen | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Dropbox für Unternehmen konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/20/2018
ms.author: jeedes
ms.openlocfilehash: eadf6724891d348c2ea3654bcf19ef0d74078049
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2018
ms.locfileid: "42145992"
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Tutorial: Azure Active Directory-Integration mit Dropbox für Unternehmen

In diesem Tutorial erfahren Sie, wie Sie Dropbox für Unternehmen in Azure Active Directory (Azure AD) integrieren.

Die Integration von Dropbox für Unternehmen in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD den Zugriff auf Dropbox für Unternehmen steuern.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Dropbox für Unternehmen anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Dropbox für Unternehmen konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Abonnement von Dropbox für Unternehmen, mit dem das einmalige Anmelden möglich ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.
Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Dropbox für Unternehmen aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-dropbox-for-business-from-the-gallery"></a>Hinzufügen von Dropbox für Unternehmen aus dem Katalog

Zum Konfigurieren der Integration von Dropbox für Unternehmen in Azure AD müssen Sie Dropbox für Unternehmen aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Dropbox für Unternehmen aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Dropbox für Unternehmen** ein, wählen Sie im Ergebnisbereich **Dropbox für Unternehmen** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Dropbox für Unternehmen in der Ergebnisliste](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Dropbox für Unternehmen mit einem Testbenutzer namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Dropbox für Unternehmen als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Dropbox für Unternehmen muss eine Linkbeziehung hergestellt werden.

Weisen Sie in Dropbox für Unternehmen den Wert für **Benutzername** aus Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Dropbox für Unternehmen müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Dropbox für Unternehmen-Testbenutzers](#create-a-dropbox-for-business-test-user)**, um in Dropbox für Unternehmen eine Entsprechung von Britta Simon zu erhalten, die mit ihrer Benutzerdarstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Dropbox für Unternehmen-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Dropbox für Unternehmen die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Dropbox für Unternehmen** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Dropbox für Unternehmen** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Dropbox für Unternehmen](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url1.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://www.dropbox.com/sso/<id>`.

    b. Geben Sie im Textfeld **Bezeichner** einen Wert ein: `Dropbox`

    > [!NOTE]
    > Der hier angegebene Wert für die Anmelde-URL ist kein echter Wert. Er muss mit der tatsächlichen Anmelde-URL aktualisiert werden. Dies wird weiter unten in diesem Tutorial beschrieben.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/dropboxforbusiness-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Dropbox für Unternehmen-Konfiguration** auf **Dropbox für Unternehmen konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Konfiguration von Dropbox für Unternehmen](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

7. Navigieren Sie zu Ihrem Dropbox für Unternehmen-Mandanten, und melden Sie sich bei diesem Mandanten an, um das einmalige Anmelden aufseiten von **Dropbox für Unternehmen** zu konfigurieren.

    ![Einmaliges Anmelden konfigurieren](./media/dropboxforbusiness-tutorial/ic769509.png "Einmaliges Anmelden konfigurieren")

8. Klicken Sie auf das **Benutzersymbol**, und wählen Sie die Registerkarte **Einstellungen** aus.

    ![Einmaliges Anmelden konfigurieren](./media/dropboxforbusiness-tutorial/configure1.png "Einmaliges Anmelden konfigurieren")

9. Klicken Sie im Navigationsbereich auf der linken Seite auf **Verwaltungskonsole**.

    ![Einmaliges Anmelden konfigurieren](./media/dropboxforbusiness-tutorial/configure2.png "Einmaliges Anmelden konfigurieren")

10. Klicken Sie in der **Verwaltungskonsole** im linken Navigationsbereich auf **Einstellungen**.

    ![Einmaliges Anmelden konfigurieren](./media/dropboxforbusiness-tutorial/configure3.png "Einmaliges Anmelden konfigurieren")

11. Wählen Sie im Abschnitt **Authentifizierung** die Option **Einmaliges Anmelden** aus.

    ![Einmaliges Anmelden konfigurieren](./media/dropboxforbusiness-tutorial/configure4.png "Einmaliges Anmelden konfigurieren")

12. Führen Sie im Abschnitt **Einmaliges Anmelden** die folgenden Schritte aus:  

    ![Einmaliges Anmelden konfigurieren](./media/dropboxforbusiness-tutorial/configure5.png "Einmaliges Anmelden konfigurieren")

    a. Wählen Sie in der Dropdownliste für **Einmaliges Anmelden** die Option **Erforderlich** aus.

    b. Klicken Sie auf **Anmelde-URL hinzufügen**, und fügen Sie den Wert der **SAML-Dienst-URL für einmaliges Anmelden**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Anmelde-URL des Identitätsanbieters** ein. Anschließend klicken Sie auf **Fertig**.

    ![Einmaliges Anmelden konfigurieren](./media/dropboxforbusiness-tutorial/configure6.png "Einmaliges Anmelden konfigurieren")

    c. Klicken Sie auf **Zertifikat hochladen**, und navigieren Sie dann zu Ihrer **Base64-codierten Zertifikatsdatei**, die Sie aus dem Azure-Portal heruntergeladen haben.

    d. Klicken Sie auf **Link kopieren**, und fügen Sie den kopierten Wert in das Textfeld **Anmelde-URL** im Abschnitt **Domäne und URLs für Dropbox für Unternehmen** im Azure-Portal ein.

    e. Klicken Sie auf **Speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/dropboxforbusiness-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/dropboxforbusiness-tutorial/create_aaduser_02.png)

1. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/dropboxforbusiness-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/dropboxforbusiness-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-dropbox-for-business-test-user"></a>Erstellen eines Dropbox für Unternehmen-Testbenutzers

In diesem Abschnitt wird ein Benutzer mit dem Namen Britta Simon in Dropbox für Unternehmen erstellt. Dropbox für Unternehmen unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Falls ein Benutzer nicht bereits in Dropbox für Unternehmen vorhanden ist, wird beim Versuch, auf Dropbox für Unternehmen zuzugreifen, ein neuer Benutzer erstellt.

>[!Note]
>Wenn Sie einen Benutzer manuell erstellen müssen, wenden Sie sich an das [Kundensupportteam von Dropbox für Unternehmen](https://www.dropbox.com/business/contact). 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Dropbox für Unternehmen gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Dropbox für Unternehmen zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **Dropbox für Unternehmen** aus.

    ![Der Link „Dropbox für Unternehmen“ in der Anwendungsliste](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png)  

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Dropbox für Unternehmen“ klicken, sollten Sie zur Anmeldeseite Ihrer Dropbox für Unternehmen-Anwendung gelangen.
 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/dropboxforbusiness-tutorial/tutorial_general_203.png

