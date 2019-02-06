---
title: 'Tutorial: Integration von Elium in Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Elium konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: fae344b3-5bd9-40e2-9a1d-448dcd58155f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: jeedes
ms.openlocfilehash: ac88d3accad8aa9ec523ecce5eb11b6c10eb913c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55181485"
---
# <a name="tutorial-azure-active-directory-integration-with-elium"></a>Tutorial: Integration von Elium in Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie Elium in Azure Active Directory (Azure AD) integrieren.

Die Integration von Elium in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Elium hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Elium anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Integration von Elium in Azure AD benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Elium-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Elium aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-elium-from-the-gallery"></a>Hinzufügen von Elium aus dem Katalog
Zum Konfigurieren der Integration von Elium in Azure AD müssen Sie Elium aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Elium über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld **Elium** ein, wählen Sie im Ergebnisbereich **Elium** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Elium in der Ergebnisliste](./media/elium-tutorial/tutorial_elium_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Elium mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Elium als Entsprechung eines Benutzers in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Elium muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Elium müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Erstellen eines Elium-Testbenutzers](#create-an-elium-test-user)**, um eine Entsprechung von Britta Simon in Elium zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Elium-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Elium die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Elium** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/elium-tutorial/tutorial_elium_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für Elium** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Elium](./media/elium-tutorial/tutorial_elium_url.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<platform-domain>.elium.com/login/saml2/metadata`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<platform-domain>.elium.com/login/saml2/acs`

1. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Elium](./media/elium-tutorial/tutorial_elium_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: ` https://<platform-domain>.elium.com/login/saml2/login`.
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Sie erhalten diese Werte in der **SP-Metadatendatei**, die Sie unter `https://<platform-domain>.elium.com/login/saml2/metadata` herunterladen können und die später in diesem Tutorial erläutert wird.

1. Die Elium-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten.

    ![Configure single sign-on](./media/elium-tutorial/tutorial_attribute.png)

1. Konfigurieren Sie das SAML-Tokenattribut im Abschnitt **Benutzerattribute** im Dialogfeld **Einmaliges Anmelden**, wie in der vorherigen Abbildung gezeigt, und führen Sie die folgenden Schritte durch:
           
    | Attributname | Attributwert |   
    | ---------------| ----------------|
    | E-Mail   |user.mail |
    | first_name| user.givenname |
    | last_name| user.surname|
    | job_title| user.jobtitle|
    | company| user.companyname|
    
    > [!NOTE]
    > Dies sind die Standardansprüche. **Es ist nur ein E-Mail-Anspruch erforderlich**. Auch für die JIT-Bereitstellung ist nur der E-Mail-Anspruch obligatorisch. Andere benutzerdefinierte Ansprüche können je nach Kundenplattform variieren.

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Configure single sign-on](./media/elium-tutorial/tutorial_attribute_04.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    ![Configure single sign-on](./media/elium-tutorial/tutorial_attribute_05.png)

    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.

    d. Lassen Sie das Feld für den Namespace leer.
    
    e. Klicken Sie auf **OK**. 

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/elium-tutorial/tutorial_elium_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/elium-tutorial/tutorial_general_400.png)
    
1. Melden Sie sich in einem anderen Webbrowserfenster bei Elium-Unternehmenswebsite als Administrator an.

1. Klicken Sie in der oberen rechten Ecke auf das **Benutzerprofil**, und wählen Sie **Administration** aus.

    ![Configure single sign-on](./media/elium-tutorial/user1.png)

1. Wählen Sie die Registerkarte **Security** (Sicherheit) aus.

    ![Configure single sign-on](./media/elium-tutorial/user2.png)

1. Scrollen Sie zum Abschnitt **Single Sign-On (SSO)** (Einmaliges Anmelden), und führen Sie die folgenden Schritte aus:

    ![Configure single sign-on](./media/elium-tutorial/user3.png)

    a. Kopieren Sie den Wert von **Verify that SAML2 authentication works for your account** (Überprüfen, ob die SAML2-Authentifizierung für Ihr Konto funktioniert), und fügen Sie ihn im Azure-Portal im Abschnitt **Domäne und URLs für Elium** im Textfeld **Anmelde-URL** ein.

    > [!NOTE]
    > Nach dem Konfigurieren des einmaligen Anmeldens können Sie jederzeit unter der folgenden URL auf die Standardseite für die Remoteanmeldung zugreifen: `https://<platform_domain>/login/regular/login` 

    b. Aktivieren Sie das Kontrollkästchen **Enable SAML2 federation** (SAML2-Verbund aktivieren).

    c. Aktivieren Sie das Kontrollkästchen **JIT Provisioning** (JIT-Bereitstellung).

    d. Öffnen Sie die **SP Metadata** (SP-Metadaten), indem Sie auf die Schaltfläche **Download** (Herunterladen) klicken.

    e. Suchen Sie in der Datei **SP Metadata** nach **entityID**, kopieren Sie den **entityID**-Wert, und fügen Sie ihn im Azure-Portal im Abschnitt **Domäne und URLs für Elium** in das Textfeld **Bezeichner** ein. 

    ![Configure single sign-on](./media/elium-tutorial/user4.png)

    f. Suchen Sie in der Datei **SP Metadata** nach **AssertionConsumerService**, kopieren Sie den **Location**-Wert, und fügen Sie ihn im Azure-Portal im Abschnitt **Domäne und URLs für Elium** in das Textfeld **Antwort-URL** ein.

    ![Configure single sign-on](./media/elium-tutorial/user5.png)

    g. Öffnen Sie die heruntergeladene Metadatendatei im Azure-Portal im Editor, kopieren Sie den Inhalt, und fügen Sie ihn in das Textfeld **IdP Metadata** ein.

    h. Klicken Sie auf **Speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/elium-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/elium-tutorial/create_aaduser_02.png)

1. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/elium-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/elium-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-an-elium-test-user"></a>Erstellen eines Elium-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen einer Benutzerin namens Britta Simon in Elium. Elium unterstützt die Just-In-Time-Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf Elium ein neuer Benutzer erstellt.
>[!Note]
>Wenn Sie einen Benutzer manuell erstellen müssen, wenden Sie sich an das [Elium-Supportteam](mailto:support@elium.com).

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Elium gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu Elium durchzuführen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste die Option **Elium**aus.

    ![Elium-Link in der Anwendungsliste](./media/elium-tutorial/tutorial_elium_app.png)  

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Elium-Kachel klicken, sollten Sie automatisch bei Ihrer Elium-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/elium-tutorial/tutorial_general_01.png
[2]: ./media/elium-tutorial/tutorial_general_02.png
[3]: ./media/elium-tutorial/tutorial_general_03.png
[4]: ./media/elium-tutorial/tutorial_general_04.png

[100]: ./media/elium-tutorial/tutorial_general_100.png

[200]: ./media/elium-tutorial/tutorial_general_200.png
[201]: ./media/elium-tutorial/tutorial_general_201.png
[202]: ./media/elium-tutorial/tutorial_general_202.png
[203]: ./media/elium-tutorial/tutorial_general_203.png

