---
title: 'Tutorial: Azure Active Directory-Integration mit mindWireless | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und mindWireless konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bd00a339-27c9-4904-b66f-a95bf597ac3c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2018
ms.author: jeedes
ms.openlocfilehash: b3be6f8a0c34278b699d0eac8faa2abaaa93d114
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34349164"
---
# <a name="tutorial-azure-active-directory-integration-with-mindwireless"></a>Tutorial: Azure Active Directory-Integration mit mindWireless

In diesem Tutorial erfahren Sie, wie Sie mindWireless in Azure Active Directory (Azure AD) integrieren.

Die Integration von mindWireless in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf mindWireless hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei mindWireless anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit mindWireless konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein mindWireless-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von mindWireless aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-mindwireless-from-the-gallery"></a>Hinzufügen von mindWireless aus dem Katalog
Um die Integration von mindWireless in Azure AD zu konfigurieren, müssen Sie mindWireless aus dem Katalog zu Ihrer Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um mindWireless aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie **mindWireless** in das Suchfeld ein, wählen Sie **mindWireless** im Ergebnisbereich aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![mindWireless in der Ergebnisliste](./media/active-directory-saas-mindwireless-tutorial/tutorial_mindwireless_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie anhand einer Testbenutzerin namens Britta Simon das einmalige Anmelden von Azure AD mit mindWireless.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in mindWireless als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in mindWireless muss eine Linkbeziehung eingerichtet werden.

Um das einmalige Anmelden in Azure AD mit mindWireless zu konfigurieren und zu testen, müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines mindWireless-Testbenutzers](#create-a-mindwireless-test-user)**, um ein Pendant von Britta Simon in mindWireless zu erhalten, das mit der Azure AD-Darstellung eines Benutzers verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal, und konfigurieren Sie das einmalige Anmelden in Ihrer mindWireless-Anwendung.

**Um das einmalige Anmelden von Azure AD mit mindWireless zu testen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **mindWireless** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-mindwireless-tutorial/tutorial_mindwireless_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für mindWireless** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für mindWireless](./media/active-directory-saas-mindwireless-tutorial/tutorial_mindwireless_url.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<subdomain>.mwsmart.com/`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<subdomain>.mwsmart.com/SAML/AssertionConsumerService.aspx`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Wenden Sie sich an das [Supportteam von mindWireless](mailto:sdulloor@mindwireless.com), um diese Werte zu erhalten.

4. Die mindWireless-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen.

5. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen. Der Anspruchsname ist immer eine Kombination aus **Mitarbeiter-ID** und dem Wert, den Sie dem Attribut „user.extensionattribute1“ zugewiesen haben, das die Mitarbeiter-ID des Benutzers enthält. Hier erfolgt die Benutzerzuordnung von Azure AD zu mindWireless auf Grundlage der Mitarbeiter-ID. Je nach Anwendungseinstellungen ist jedoch auch eine Zuordnung zu einem anderen Wert möglich. Sie können zunächst beim [Supportteam von mindWireless](mailto:sdulloor@mindwireless.com) die richtige ID eines Benutzers erfragen und dann diesen Wert dem Anspruch **Mitarbeiter-ID** zuordnen.

    ![Configure Single Sign-On](./media/active-directory-saas-mindwireless-tutorial/tutorial_attribute.png)

6. Konfigurieren Sie das SAML-Tokenattribut im Abschnitt **Benutzerattribute** im Dialogfeld **Einmaliges Anmelden**, wie in der vorherigen Abbildung gezeigt, und führen Sie die folgenden Schritte durch:
    
    | Attributname | Attributwert | Namespacewert |
    | -------------- | --------------- | ----------------|
    | Mitarbeiter-ID | user.employeeid | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|
    
    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Configure Single Sign-On](./media/active-directory-saas-mindwireless-tutorial/tutorial_attribute_04.png)

    ![Configure Single Sign-On](./media/active-directory-saas-mindwireless-tutorial/tutorial_attribute_05.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.

    d. Geben Sie im Textfeld **Namespace** den für die betreffende Zeile angezeigten Namespacewert ein.
    
    e. Klicken Sie auf **OK**.
    
7. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-mindwireless-tutorial/tutorial_mindwireless_certificate.png) 

8. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-mindwireless-tutorial/tutorial_general_400.png)

9. Klicken Sie im Abschnitt **mindWireless-Konfiguration** auf **mindWireless konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![mindWireless-Konfiguration](./media/active-directory-saas-mindwireless-tutorial/tutorial_mindwireless_configure.png) 

10. Um einmaliges Anmelden für **mindWireless** zu konfigurieren, müssen Sie das heruntergeladene **Zertifikat (Base64), die SAML-Dienst-URL für einmaliges Anmelden** und die **SAML-Entitäts-ID** an das [Supportteam von mindWireless](mailto:sdulloor@mindwireless.com) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-mindwireless-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-mindwireless-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-mindwireless-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-mindwireless-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="create-a-mindwireless-test-user"></a>Erstellen eines mindWireless-Testbenutzers

In diesem Abschnitt erstellen Sie in mindWireless eine Benutzerin namens Britta Simon. Wenden Sie sich an das [Supportteam von mindWireless](mailto:sdulloor@mindwireless.com), um die Benutzer in der mindWireless-Plattform hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können. 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf mindWireless gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon zu mindWireless zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **mindWireless** aus.

    ![mindWireless-Link in der Anwendungsliste](./media/active-directory-saas-mindwireless-tutorial/tutorial_mindwireless_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „mindWireless“ klicken, sollten Sie automatisch bei Ihrer mindWireless-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mindwireless-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mindwireless-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mindwireless-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mindwireless-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mindwireless-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mindwireless-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mindwireless-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mindwireless-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mindwireless-tutorial/tutorial_general_203.png

