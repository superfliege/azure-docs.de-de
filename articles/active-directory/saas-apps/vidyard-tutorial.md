---
title: 'Tutorial: Azure Active Directory-Integration mit Vidyard| Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und Vidyard konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bed7df23-6e13-4e7c-b4cc-53ed4804664d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2018
ms.author: jeedes
ms.openlocfilehash: 683cdc43c8d671c14142d0770cd2594ad862b7ac
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55196241"
---
# <a name="tutorial-azure-active-directory-integration-with-vidyard"></a>Tutorial: Azure Active Directory-Integration mit Vidyard

In diesem Tutorial erfahren Sie, wie Sie Vidyard in Azure Active Directory (Azure AD) integrieren.

Die Integration von Vidyard in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Vidyard hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Vidyard anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in Vidyard konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Vidyard-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Vidyard aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-vidyard-from-the-gallery"></a>Hinzufügen von Vidyard aus dem Katalog
Zum Konfigurieren der Integration von Vidyard in Azure AD müssen Sie Vidyard aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**So fügen Sie Vidyard aus dem Katalog hinzu**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld **Vidyard** ein, wählen Sie im Ergebnisbereich **Vidyard** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Vidyard in der Ergebnisliste](./media/vidyard-tutorial/tutorial_vidyard_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Vidyard anhand einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Vidyard als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Vidyard-Benutzer muss eine Linkbeziehung eingerichtet werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Vidyard zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Erstellen eines Vidyard-Testbenutzers](#create-a-vidyard-test-user)**, um eine Entsprechung für Britta Simon in Vidyard zu haben, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Vidyard-Anwendung.

**So konfigurieren Sie das einmalige Anmelden von Azure AD mit Vidyard**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Vidyard** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/vidyard-tutorial/tutorial_vidyard_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für Vidyard** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Vidyard](./media/vidyard-tutorial/tutorial_vidyard_url2.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://secure.vidyard.com/sso/saml/<unique id>/metadata`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://secure.vidyard.com/sso/saml/<unique id>/consume`

1. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Vidyard](./media/vidyard-tutorial/tutorial_vidyard_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://secure.vidyard.com/sso/saml/<unique id>/login`.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte durch die tatsächlichen Werte für Bezeichner, Antwort-URL und Anmelde-URL ersetzen. Darauf wird später im Tutorial eingegangen.

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/vidyard-tutorial/tutorial_vidyard_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/vidyard-tutorial/tutorial_general_400.png)

1. Klicken Sie im Abschnitt **Vidyard-Konfiguration** auf **Vidyard konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Vidyard-Konfiguration](./media/vidyard-tutorial/tutorial_vidyard_configure.png)

1. Melden Sie sich in einem anderen Webbrowserfenster auf der Vidyard-Unternehmenswebsite als Administrator an.

1. Wählen Sie im Vidyard-Dashboard **Group** (Gruppe)  >  **Security** (Sicherheit) aus.

    ![Vidyard-Konfiguration](./media/vidyard-tutorial/configure1.png)

1. Klicken Sie auf die Registerkarte **New Profile** (Neues Profil).

    ![Vidyard-Konfiguration](./media/vidyard-tutorial/configure2.png)

1. Führen Sie im Abschnitt **SAML Configuration** (SAML-Konfiguration) die folgenden Schritte aus:

    ![Vidyard-Konfiguration](./media/vidyard-tutorial/configure3.png)

    a. Geben Sie einen allgemeinen Profilnamen in das Textfeld **Profile Name** (Profilname) ein.

    b. Kopieren Sie den Wert in **SSO User Login Page** (Benutzeranmeldeseite für SSO), und fügen Sie diesen im Azure-Portal im Abschnitt **Domäne und URLs für Vidyard** im Textfeld **Anmelde-URL** ein.

    c. Kopieren Sie den Wert aus **ACS URL** (ACS-URL), und fügen Sie ihn im Azure-Portal im Abschnitt **Domäne und URLs für Vidyard** im Textfeld **Antwort-URL** ein.

    d. Kopieren Sie den Wert aus **Issuer/Metadata URL** (Aussteller-/Metadaten-URL), und fügen Sie diesen im Azure-Portal im Abschnitt **Domäne und URLs für Vidyard** im Textfeld **Bezeichner** ein.

    e. Öffnen Sie im Editor das Zertifikat, das Sie im Azure-Portal heruntergeladen haben, und fügen Sie den Inhalt in das Textfeld **X.509-Zertifikat** ein.

    f. Fügen Sie in das Textfeld **SAML Endpoint URL** (SAML-Endpunkt-URL) den Wert der **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.

    g. Klicken Sie auf **Confirm** (Bestätigen).

1. Wählen Sie auf der Registerkarte „Single Sign On“ (Einmaliges Anmelden) neben einem vorhandenen Profil **Assign** (Zuweisen) aus.

    ![Vidyard-Konfiguration](./media/vidyard-tutorial/configure4.png)

    > [!NOTE]
    > Wenn Sie ein Profil für das einmalige Anmelden erstellt haben, weisen sie es jeder Gruppe zu, für die Benutzer Zugriff über Azure benötigen. Wenn der Benutzer nicht in der Gruppe ist, der er zugewiesen wurde, erstellt Vidyard automatisch ein Benutzerkonto und weist die Rolle in Echtzeit zu.

1. Wählen Sie Ihre Organisationsgruppe aus, die unter **Groups Available to Assign** (Verfügbare Gruppen zum Zuweisen) angezeigt wird.

    ![Vidyard-Konfiguration](./media/vidyard-tutorial/configure5.png)

1. Die zugewiesenen Gruppen werden unter **Groups Currently Assigned** (Derzeit zugewiesene Gruppen) angezeigt. Wählen Sie für die Gruppe eine Rolle gemäß Ihrer Organisation aus, und klicken Sie auf **Confirm** (Bestätigen).

    ![Vidyard-Konfiguration](./media/vidyard-tutorial/configure6.png)

    > [!NOTE]
    > Weitere Informationen finden Sie in [diesem Dokument](https://knowledge.vidyard.com/saml-single-sign-on-authentication/saml-based-single-sign-on-sso-in-vidyard).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/vidyard-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/vidyard-tutorial/create_aaduser_02.png)

1. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/vidyard-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/vidyard-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-vidyard-test-user"></a>Erstellen eines Vidyard-Testbenutzers

In diesem Abschnitt wird in Vidyard eine Benutzerin namens Britta Simon erstellt. Vidyard unterstützt die Just-In-Time-Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf Vidyard ein neuer Benutzer erstellt.
>[!Note]
>Setzen Sie sich mit dem  [Supportteam von Vidyard](mailto:support@vidyard.com) in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt gewähren Sie Britta Simon Zugriff auf Vidyard, damit sie das einmalige Anmelden von Azure verwenden kann.

![Zuweisen der Benutzerrolle][200] 

**So weisen Sie Britta Simon Vidyard zu**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **Vidyard** aus.

    ![Vidyard-Link in der Anwendungsliste](./media/vidyard-tutorial/tutorial_vidyard_app.png)  

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Vidyard“ klicken, werden Sie automatisch bei Ihrer Vidyard-Anwendung angemeldet.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/vidyard-tutorial/tutorial_general_01.png
[2]: ./media/vidyard-tutorial/tutorial_general_02.png
[3]: ./media/vidyard-tutorial/tutorial_general_03.png
[4]: ./media/vidyard-tutorial/tutorial_general_04.png

[100]: ./media/vidyard-tutorial/tutorial_general_100.png

[200]: ./media/vidyard-tutorial/tutorial_general_200.png
[201]: ./media/vidyard-tutorial/tutorial_general_201.png
[202]: ./media/vidyard-tutorial/tutorial_general_202.png
[203]: ./media/vidyard-tutorial/tutorial_general_203.png

