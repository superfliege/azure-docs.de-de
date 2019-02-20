---
title: 'Tutorial: Azure Active Directory-Integration mit Leapsome | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Leapsome konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cb523e97-add8-4289-b106-927bf1a02188
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 37549cc76e1490b0758de8e296523b0e70c98dbf
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56191230"
---
# <a name="tutorial-azure-active-directory-integration-with-leapsome"></a>Tutorial: Azure Active Directory-Integration mit Leapsome

In diesem Tutorial erfahren Sie, wie Sie Leapsome in Azure Active Directory (Azure AD) integrieren.

Die Integration von Leapsome in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf Leapsome Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Leapsome anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Leapsome konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Leapsome-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Leapsome aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-leapsome-from-the-gallery"></a>Hinzufügen von Leapsome aus dem Katalog
Zum Konfigurieren der Integration von Leapsome in Azure AD müssen Sie Leapsome aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Leapsome aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld **Leapsome** ein, wählen Sie im Ergebnisbereich **Leapsome** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Leapsome in der Ergebnisliste](./media/leapsome-tutorial/tutorial_leapsome_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Leapsome mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Leapsome als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Leapsome muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Leapsome müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Erstellen eines Leapsome-Testbenutzers](#create-a-leapsome-test-user)**, um ein Pendant von Britta Simon in Leapsome zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Leapsome-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Leapsome die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Leapsome** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/leapsome-tutorial/tutorial_leapsome_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für Leapsome** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Leapsome](./media/leapsome-tutorial/tutorial_leapsome_url.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL ein: `https://www.leapsome.com`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://www.leapsome.com/api/users/auth/saml/<CLIENTID>/assert`

1. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Leapsome](./media/leapsome-tutorial/tutorial_leapsome_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://www.leapsome.com/api/users/auth/saml/<CLIENTID>/login`.
     
    > [!NOTE] 
    > Der Wert der „Antwort-URL“ und der „Anmelde-URL“ entspricht nicht dem tatsächlichen Wert. Sie aktualisieren ihn mit den tatsächlichen Werten. Dies wird später in diesem Tutorial beschrieben.

1. Die Leapsome-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Im folgenden Screenshot wird ein Beispiel gezeigt.
    
    ![Configure single sign-on](./media/leapsome-tutorial/tutorial_Leapsome_attribute.png)

1. Konfigurieren Sie das SAML-Tokenattribut im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute**, wie im obigen Bild gezeigt, und führen Sie die folgenden Schritte aus:
    
    | Attributname | Attributwert | Namespace |
    | ---------------| --------------- | --------- |   
    | firstname | user.givenname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | lastname | user.surname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | title | user.jobtitle | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | picture | URL zum Bild des Mitarbeiters | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |

    > [!Note]
    > Bei dem Wert des Bildattributs handelt es sich nicht um einen echten Wert. Aktualisieren Sie diesen Wert mit der tatsächlichen Bild-URL. Wenden Sie sich an das  [Supportteam für den Leapsome-Client](mailto:support@leapsome.com), um diesen Wert zu erhalten.
    
    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Configure single sign-on](./media/leapsome-tutorial/tutorial_attribute_04.png)

    ![Configure single sign-on](./media/leapsome-tutorial/tutorial_attribute_05.png)
    
    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.

    d. Geben Sie im Textfeld **Namespace** den für die betreffende Zeile angezeigten Namespace-URI ein.
    
    e. Klicken Sie auf **OK**.

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/leapsome-tutorial/tutorial_leapsome_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/leapsome-tutorial/tutorial_general_400.png)
    
1. Klicken Sie im Abschnitt **Leapsome-Konfiguration** auf **Leapsome konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Leapsome-Konfiguration](./media/leapsome-tutorial/tutorial_leapsome_configure.png)

1. Melden Sie sich in einem anderen Webbrowserfenster bei Leapsome als Sicherheitsadministrator an.

1. Klicken Sie oben rechts auf das Logo für die Einstellungen und anschließend auf **Administratoreinstellungen**. 

    ![Leapsomesatz](./media/leapsome-tutorial/tutorial_leapsome_admin.png)

1. Klicken Sie in der linken Menüleiste auf **Einmaliges Anmelden (SSO)**, und führen Sie auf der Seite **SAML-basierten einmaliges Anmelden (SSO)** die folgenden Schritte aus:
    
    ![Leapsome-SAML](./media/leapsome-tutorial/tutorial_leapsome_samlsettings.png)

    a. Wählen Sie **SAML-basiertes einmaliges Anmelden aktivieren** aus.

    b. Kopieren Sie den Wert **Anmelde-URL (verweisen Sie Ihre Benutzer hierher, um Anmeldung zu starten)**, und fügen Sie ihn in das Textfeld **Anmelde-URL** im Abschnitt **Leapsome-Domänen und URLs** im Azure-Portal ein.

    c. Kopieren Sie den Wert **Antwort-URL (erhält die Antwort von Ihrem Identitätsanbieter)**, und fügen Sie ihn in das Textfeld **Antwort-URL** im Abschnitt **Leapsome-Domänen und URLs** im Azure-Portal ein.

    d. Fügen Sie im Textfeld **SSO-Anmelde-URL (von Ihrem Identitätsanbieter bereitgestellt)** den Wert von **URL für den SAML-SSO-Dienst** ein, den Sie aus dem Azure-Portal kopiert haben.

    e. Kopieren Sie das aus dem Azure-Portal heruntergeladene Zertifikat ohne die Kommentare – BEGIN CERTIFICATE und END CERTIFICATE – und fügen Sie es in das Textfeld **Zertifikat (von Ihrem Identitätsanbieter bereitgestellt)** ein.

    f. Klicken Sie auf **SSO-EINSTELLUNGEN AKTUALISIEREN**.
    
### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/leapsome-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/leapsome-tutorial/create_aaduser_02.png)

1. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/leapsome-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/leapsome-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-leapsome-test-user"></a>Erstellen eines Leapsome-Testbenutzers

In diesem Abschnitt erstellen Sie in Leapsome einen Benutzer namens Britta Simon. Lassen Sie sich beim Hinzufügen der Benutzer oder der Domäne, die auf der Leapsome-Plattform in die Whitelist aufgenommen werden soll, vom  [Leapsome Client-Supportteam](mailto:support@leapsome.com)  unterstützen. Wird die Domäne vom Team hinzugefügt, werden Benutzer automatisch auf der Leapsome-Plattform bereitgestellt. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können. 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie den Zugriff auf Leapsome gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Leapsome zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **Leapsome** aus.

    ![Leapsome-Link in der Anwendungsliste](./media/leapsome-tutorial/tutorial_leapsome_app.png)  

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Leapsome“ klicken, sollten Sie automatisch bei Ihrer Leapsome-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/leapsome-tutorial/tutorial_general_01.png
[2]: ./media/leapsome-tutorial/tutorial_general_02.png
[3]: ./media/leapsome-tutorial/tutorial_general_03.png
[4]: ./media/leapsome-tutorial/tutorial_general_04.png

[100]: ./media/leapsome-tutorial/tutorial_general_100.png

[200]: ./media/leapsome-tutorial/tutorial_general_200.png
[201]: ./media/leapsome-tutorial/tutorial_general_201.png
[202]: ./media/leapsome-tutorial/tutorial_general_202.png
[203]: ./media/leapsome-tutorial/tutorial_general_203.png

