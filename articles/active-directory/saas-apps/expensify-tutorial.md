---
title: 'Tutorial: Azure Active Directory-Integration mit Expensify | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Expensify konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 1e761484-7a2f-4321-91f4-6d5d0b69344e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2018
ms.author: jeedes
ms.openlocfilehash: 5d61ac27eb5a6e4f546d8c6db66b84f2c204b507
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52840468"
---
# <a name="tutorial-azure-active-directory-integration-with-expensify"></a>Tutorial: Azure Active Directory-Integration mit Expensify

In diesem Tutorial erfahren Sie, wie Sie Expensify in Azure Active Directory (Azure AD) integrieren.

Die Integration von Expensify in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Expensify hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Expensify anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Expensify konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Expensify-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Expensify aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-expensify-from-the-gallery"></a>Hinzufügen von Expensify aus dem Katalog

Zum Konfigurieren der Integration von Expensify in Azure AD müssen Sie Expensify aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Expensify aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![image](./media/expensify-tutorial/selectazuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![image](./media/expensify-tutorial/a_select_app.png)
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![image](./media/expensify-tutorial/a_new_app.png)

4. Geben Sie im Suchfeld **Expensify** ein, wählen Sie im Ergebnisbereich **Expensify** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![image](./media/expensify-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden für Azure AD mit Expensify basierend auf einem Testbenutzer mit dem Namen „Britta Simon“.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Expensify als Gegenbenutzer zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Expensify muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Expensify den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Expensify müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Erstellen eines Expensify-Testbenutzers](#create-an-expensify-test-user)**, um ein Pendant von Britta Simon in Expensify zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Expensify-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Expensify die folgenden Schritte aus:**

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Expensify** die Option **Einmaliges Anmelden** aus.

    ![image](./media/expensify-tutorial/b1_b2_select_sso.png)

2. Klicken Sie oben im Bildschirm auf **Modus für einmaliges Anmelden ändern**, um den Modus **SAML** auszuwählen.

      ![image](./media/expensify-tutorial/b1_b2_saml_ssso.png)

3. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![image](./media/expensify-tutorial/b1_b2_saml_sso.png)

4. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![image](./media/expensify-tutorial/b1-domains_and_urlsedit.png)

5. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    a. Geben Sie im Textfeld **Anmelde-URL** die URL folgendermaßen ein: `https://www.expensify.com/authentication/saml/login`

    b. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://www.<companyname>.expensify.com`

    ![image](./media/expensify-tutorial/b1-domains_and_urls.png)

    > [!NOTE] 
    > Ersetzen Sie den Abschnitt <companyname> der Bezeichner-URL durch die Domäne Ihres Unternehmens. Siehe obiges Beispiel von `https://contoso.expensify.com`. In Expensify ist dies der Name Ihrer Domäne wie unter **Einstellungen > Domänensteuerung** angezeigt.

    ![Expensify-Domäneninformationen](./media/expensify-tutorial/tutorial_expensify_domain.png)

6. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![image](./media/expensify-tutorial/certificatebase64.png)

7. Zum Aktivieren von SSO in Expensify müssen Sie in der Anwendung zuerst die **Domänensteuerung** aktivieren. Sie können die Domänensteuerung in der Anwendung mittels der [hier](https://help.expensify.com/domain-control) aufgelisteten Schritte aktivieren. Um weitere Unterstützung zu erhalten, wenden Sie sich an das [Clientsupportteam von Expensify](mailto:help@expensify.com). Führen Sie nach dem Aktivieren der Domänensteuerung die folgenden Schritte aus:
   
    ![Configure single sign-on](./media/expensify-tutorial/tutorial_expensify_51.png)
    
    a. Melden Sie sich bei der Expensify-Anwendung an.
    
    b. Klicken Sie im linken Bereich auf **Einstellungen**, und navigieren Sie zu **SAML**.
    
    c. Schalten Sie die Option **SAML-Anmeldung** zu **Aktiviert** um.
    
    d. Öffnen Sie die aus Azure AD heruntergeladenen Verbundmetadaten im Editor, kopieren Sie den Inhalt, und fügen Sie ihn in das Textfeld **Identitätsanbietermetadaten** ein.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers 

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![image](./media/expensify-tutorial/d_users_and_groups.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![image](./media/expensify-tutorial/d_adduser.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![image](./media/expensify-tutorial/d_userproperties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-an-expensify-test-user"></a>Erstellen eines Expensify-Testbenutzers

In diesem Abschnitt erstellen Sie in Expensify einen Benutzer mit dem Namen „Britta Simon“. Wenden Sie sich an das [Clientsupportteam von Expensify](mailto:help@expensify.com), um die Benutzer der Expensify-Plattform hinzuzufügen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Expensify gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![image](./media/expensify-tutorial/d_all_applications.png)

2. Wählen Sie in der Anwendungsliste **Expensify**aus.

    ![image](./media/expensify-tutorial/d_all_proapplications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![image](./media/expensify-tutorial/d_leftpaneusers.png)

4. Wählen Sie die Schaltfläche **Hinzufügen** und dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![image](./media/expensify-tutorial/d_assign_user.png)

4. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

5. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Expensify“ klicken, sollten Sie automatisch bei Ihrer Expensify-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)




