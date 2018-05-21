---
title: 'Tutorial: Azure Active Directory-Integration mit AppNeta Performance Monitor | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und AppNeta Performance Monitor konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 643a45fb-d6fc-4b32-b721-68899f8c7d44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2018
ms.author: jeedes
ms.openlocfilehash: 0bfbb7022ad8e70d89d79297b5f5476160bd4e33
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-appneta-performance-monitor"></a>Tutorial: Azure Active Directory-Integration mit AppNeta Performance Monitor

In diesem Tutorial erfahren Sie, wie Sie AppNeta Performance Monitor in Azure Active Directory (Azure AD) integrieren.

Die Integration von AppNeta Performance Monitor in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf AppNeta Performance Monitor hat.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei AppNeta Performance Monitor anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit AppNeta Performance Monitor konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein AppNeta Performance Monitor-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von AppNeta Performance Monitor aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-appneta-performance-monitor-from-the-gallery"></a>Hinzufügen von AppNeta Performance Monitor aus dem Katalog
Um die Integration von AppNeta Performance Monitor in Azure AD zu konfigurieren, müssen Sie AppNeta Performance Monitor aus dem Katalog zu Ihrer Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um AppNeta Performance Monitor aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie in das Suchfeld **AppNeta Performance Monitor** ein, wählen Sie **AppNeta Performance Monitor** im Ergebnisbereich aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![AppNeta Performance Monitor in der Ergebnisliste](./media/active-directory-saas-appneta-tutorial/tutorial_appneta_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei AppNeta Performance Monitor mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in AppNeta Performance Monitor als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in AppNeta Performance Monitor muss eine Linkbeziehung eingerichtet werden.

Um einmaliges Anmelden von Azure AD mit AppNeta Performance Monitor zu konfigurieren und zu testen, müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines AppNeta Performance Monitor-Testbenutzers](#create-an-appneta-performance-monitor-test-user)**, um in AppNeta Performance Monitor ein Pendant zu Britta Simon zu erhalten, das mit der Azure AD-Darstellung des Benutzers verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal, und konfigurieren Sie das einmalige Anmelden in Ihrer AppNeta Performance Monitor-Anwendung.

**Um einmaliges Anmelden von Azure AD mit AppNeta Performance Monitor zu konfigurieren, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **AppNeta Performance Monitor** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-appneta-tutorial/tutorial_appneta_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für AppNeta Performance Monitor** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für AppNeta Performance Monitor](./media/active-directory-saas-appneta-tutorial/tutorial_appneta_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.pm.appneta.com`.

    b. Geben Sie im Textfeld **Bezeichner** den Wert `PingConnect` ein.

    > [!NOTE] 
    > Der Wert der Anmelde-URL entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Kundensupportteam von AppNeta Performance Monitor](mailto:support@appneta.com), um diesen Wert zu erhalten. 

5. Die AppNeta Performance Monitor-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten.

    ![Configure Single Sign-On](./media/active-directory-saas-appneta-tutorial/attribute.png)

6. Konfigurieren Sie das SAML-Tokenattribut im Abschnitt **Benutzerattribute** im Dialogfeld **Einmaliges Anmelden**, wie in der vorherigen Abbildung gezeigt, und führen Sie die folgenden Schritte durch:
           
    | Attributname | Attributwert |
    | ---------------| ----------------|
    | firstName| user.givenname|
    | lastName| user.surname|
    | E-Mail| user.userprincipalname|
    | name| user.userprincipalname|
    | groups   | user.assignedroles |
    | phone| user.telephonenumber |
    | title| user.jobtitle|

    > [!NOTE]
    > „groups“ bezieht sich auf die Sicherheitsgruppe in AppNeta, die einer „Rolle“ in Azure AD zugeordnet wird. Lesen Sie [dieses](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) Dokument, in dem erläutert wird, wie benutzerdefinierte Rollen in Azure AD erstellt werden.
        
    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Configure Single Sign-On](./media/active-directory-saas-appneta-tutorial/tutorial_attribute_04.png)
    
    ![Configure Single Sign-On](./media/active-directory-saas-appneta-tutorial/tutorial_attribute_05.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.

    d. Lassen Sie das Feld für den Namespace leer.
    
    e. Klicken Sie auf **OK**.  

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-appneta-tutorial/tutorial_appneta_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-appneta-tutorial/tutorial_general_400.png)

6. Um einmaliges Anmelden auf **AppNeta Performance Monitor**-Seite zu konfigurieren, müssen Sie die heruntergeladene **Metadata XML**-Datei an das [Supportteam von AppNeta Performance Monitor](mailto:support@appneta.com) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-appneta-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-appneta-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-appneta-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-appneta-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-an-appneta-performance-monitor-test-user"></a>Erstellen eines AppNeta Performance Monitor-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen einer Benutzerin namens Britta Simon in AppNeta Performance Monitor. AppNeta Performance Monitor unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist ein Benutzer bei dem Versuch, auf AppNeta Performance Monitor zuzugreifen, nicht vorhanden, wird ein neuer Benutzer erstellt.
>[!Note]
>Setzen Sie sich mit dem [Supportteam von AppNeta Performance Monitor](mailto:support@appneta.com) in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf AppNeta Performance Monitor gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon zu AppNeta Performance Monitor zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste der Anwendungen die Anwendung **AppNeta Performance Monitor** aus.

    ![Der Link für Systemmonitor AppNeta Link in der Liste der Anwendungen](./media/active-directory-saas-appneta-tutorial/tutorial_appneta_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „AppNeta Performance Monitor“ klicken, sollten Sie automatisch bei Ihrer AppNeta Performance Monitor-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-appneta-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-appneta-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-appneta-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-appneta-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-appneta-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-appneta-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-appneta-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-appneta-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-appneta-tutorial/tutorial_general_203.png

