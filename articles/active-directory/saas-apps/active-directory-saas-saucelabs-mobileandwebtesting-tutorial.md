---
title: 'Tutorial: Azure Active Directory-Integration in Sauce Labs (Testplattform für mobile Apps und Webanwendungen) | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Sauce Labs für das Testen von mobilen Apps und Webanwendungen konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3142d947-70e5-4345-8a30-b92d8715fac9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: jeedes
ms.openlocfilehash: 2c7c0283178e35fb8065bca3c6e380445ab43ae5
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240450"
---
# <a name="tutorial-azure-active-directory-integration-with-sauce-labs---mobile-and-web-testing"></a>Tutorial: Azure Active Directory-Integration in Sauce Labs – für das Testen von mobilen Apps und Webanwendungen

In diesem Tutorial erfahren Sie, wie Sie Sauce Lab in Azure Active Directory (Azure AD) integrieren.

Die Integration von Sauce Labs in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf die Testplattform Sauce Labs zugreifen kann.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Sauce Labs anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in Sauce Labs zu konfigurieren, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Sauce Labs-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen der Testplattform Sauce Labs aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-sauce-labs---mobile-and-web-testing-from-the-gallery"></a>Hinzufügen der Testplattform Sauce Labs aus dem Katalog
Zum Konfigurieren der Integration von Sauce Labs in Azure AD müssen Sie Sauce Labs aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Sauce Labs aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie in das Suchfeld **Sauce Labs** ein, wählen Sie im Ergebnisbereich **Sauce Labs** aus, und klicken Sie auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Testplattform Sauce Labs in der Ergebnisliste](./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Sauce Labs mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Sauce Labs als Pendant für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Sauce Labs muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Sauce Labs müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Sauce Labs-Testbenutzers](#create-a-sauce-labs---mobile-and-web-testing-test-user)**, um ein Pendant für Britta Simon in Sauce Labs zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Sauce Labs-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Sauce Labs die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Sauce Labs** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_samlbase.png)

3. Im Abschnitt **Domäne und URLs für Sauce Lab** muss der Benutzer keine Schritte ausführen, da die App bereits in Azure integriert ist.

    ![SSO-Informationen zur Domäne und zu den URLs für Sauce Labs](./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_url.png)

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_certificate.png)

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/tutorial_general_400.png)

6. Melden Sie sich in einem anderen Webbrowserfenster bei der Sauce Labs-Unternehmenswebsite als Administrator an.

7. Klicken Sie auf das **Benutzersymbol**, und wählen Sie die Registerkarte **Team Management** (Teamverwaltung) aus.

    ![Configure single sign-on](./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/configure1.png)

8. Geben Sie in das Textfeld Ihren **Domänennamen** ein.

    ![Configure single sign-on](./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/configure2.png)

9. Klicken Sie auf die Registerkarte **Configure** (Konfigurieren).

    ![Configure single sign-on](./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/configure3.png)

10. Führen Sie im Abschnitt **Configure Single Sign On** (Einmaliges Anmelden konfigurieren) die folgenden Schritte aus:

    ![Configure single sign-on](./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/configure4.png)

    a. Klicken Sie auf **Browse** (Durchsuchen), und laden Sie die aus Azure AD heruntergeladene Metadatendatei hoch.

    b. Aktivieren Sie das Kontrollkästchen **ALLOW JUST-IN-TIME PROVISIONING** (Just-in-Time-Bereitstellung zulassen).

    c. Klicken Sie auf **Speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
  
### <a name="create-a-sauce-labs---mobile-and-web-testing-test-user"></a>Erstellen eines Sauce Labs-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Sauce Labs. Sauce Labs unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf Sauce Labs ein neuer Benutzer erstellt.
>[!Note]
>Wenn Sie einen Benutzer manuell erstellen möchten, wenden Sie sich an das [Sauce Labs-Supportteam](mailto:support@saucelabs.com).

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf Sauce Labs gewähren.

![Zuweisen der Benutzerrolle][200]

**Führen Sie die folgenden Schritte aus, um Britta Simon Sauce Labs zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Sauce Labs** aus.

    ![Sauce Labs-Link in der Anwendungsliste](./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Sauce Labs-Kachel klicken, werden Sie in der Regel automatisch in Ihrer Sauce Labs-Anwendung angemeldet.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-saucelabs-mobileandwebtesting-tutorial/tutorial_general_203.png
