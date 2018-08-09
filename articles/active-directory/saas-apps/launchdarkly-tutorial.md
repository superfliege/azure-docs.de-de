---
title: 'Tutorial: Azure Active Directory-Integration mit LaunchDarkly | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und LaunchDarkly konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0e9cb37e-16bf-493b-bfc8-8d9840545a1e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: 54bf459f6acd7649f3ad1a546bef1d0429393161
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39420758"
---
# <a name="tutorial-azure-active-directory-integration-with-launchdarkly"></a>Tutorial: Azure Active Directory-Integration mit LaunchDarkly

In diesem Tutorial erfahren Sie, wie Sie LaunchDarkly in Azure Active Directory (Azure AD) integrieren.

Die Integration von LaunchDarkly in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf LaunchDarkly hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei LaunchDarkly anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit LaunchDarkly konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein LaunchDarkly-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von LaunchDarkly aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-launchdarkly-from-the-gallery"></a>Hinzufügen von LaunchDarkly aus dem Katalog
Zum Konfigurieren der Integration von LaunchDarkly in Azure AD müssen Sie LaunchDarkly aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um LaunchDarkly aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld **LaunchDarkly** ein, wählen Sie im Ergebnisbereich **LaunchDarkly** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![LaunchDarkly in der Ergebnisliste](./media/launchdarkly-tutorial/tutorial_launchdarkly_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit LaunchDarkly anhand eines Testbenutzers namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in LaunchDarkly als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in LaunchDarkly muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit LaunchDarkly müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Erstellen eines LaunchDarkly-Testbenutzers](#create-a-launchdarkly-test-user)**, um eine Entsprechung von Britta Simon in LaunchDarkly zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer LaunchDarkly-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit LaunchDarkly die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **LaunchDarkly** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Dialogfeld „Einmaliges Anmelden“](./media/launchdarkly-tutorial/tutorial_launchdarkly_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für LaunchDarkly** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![Informationen zur Domäne und zu den URLs für LaunchDarkly für das einmalige Anmelden](./media/launchdarkly-tutorial/tutorial_launchdarkly_url.png)

    a. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** die folgende URL ein: `app.launchdarkly.com`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://app.launchdarkly.com/trust/saml2/acs/<customers-unique-id>`
    
    > [!NOTE]
    > Der Wert der Antwort-URL entspricht nicht dem tatsächlichen Wert. Sie aktualisieren den Wert mit der tatsächlichen Antwort-URL. Dies wird später in diesem Tutorial beschrieben.

1. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![Informationen zur Domäne und zu den URLs für LaunchDarkly für das einmalige Anmelden](./media/launchdarkly-tutorial/tutorial_launchdarkly_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** diese URL ein: `https://app.launchdarkly.com`

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/launchdarkly-tutorial/tutorial_launchdarkly_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/launchdarkly-tutorial/tutorial_general_400.png)
    
1. Klicken Sie im Abschnitt **LaunchDarkly-Konfiguration** auf **LaunchDarkly konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Dienst-URL für einmalige Anmeldung** aus dem Abschnitt **Kurzübersicht**.

    ![LaunchDarkly-Konfiguration](./media/launchdarkly-tutorial/tutorial_launchdarkly_configure.png)

1. Melden Sie sich in einem anderen Webbrowserfenster bei der LaunchDarkly-Unternehmenswebsite als Administrator an.

1. Wählen Sie **Account Settings** (Kontoeinstellungen) im linken Navigationsbereich aus.

    ![LaunchDarkly-Konfiguration](./media/launchdarkly-tutorial/configure1.png)

1. Klicken Sie auf die Registerkarte **Security** (Sicherheit).

    ![LaunchDarkly-Konfiguration](./media/launchdarkly-tutorial/configure2.png)

1. Klicken Sie auf **ENABLE SSO** (SSO aktivieren) und dann auf **EDIT SAML CONFIGURATION** (SAML-Konfiguration bearbeiten).

    ![LaunchDarkly-Konfiguration](./media/launchdarkly-tutorial/configure3.png)

1. Führen Sie im Abschnitt **Edit your SAML Configuration** (SAML-Konfiguration bearbeiten) die folgenden Schritte aus:

    ![LaunchDarkly-Konfiguration](./media/launchdarkly-tutorial/configure4.png)

    a. Kopieren Sie die **SAML consumer service URL** (SAML-Verbraucherdienst-URL) für Ihre Instanz, und fügen Sie diese im Azure-Portal im Abschnitt **Domäne und URLs für LaunchDarkly** im Textfeld „Antwort-URL“ ein.

    b. Fügen Sie im Textfeld **Sign-On URL** (Anmelde-URL) den Wert von **Dienst-URL für einmalige Anmeldung** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Öffnen Sie das heruntergeladene Zertifikat aus dem Azure-Portal im Editor, kopieren Sie den Inhalt, und fügen Sie ihn im Feld **X.509 certificate** ein. Sie können das Zertifikat auch direkt hochladen, indem Sie auf **Upload one** klicken.

    d. Klicken Sie unten auf der Seite auf **Speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/launchdarkly-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/launchdarkly-tutorial/create_aaduser_02.png)

1. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/launchdarkly-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/launchdarkly-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="create-a-launchdarkly-test-user"></a>Erstellen eines LaunchDarkly-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in LaunchDarkly. LaunchDarkly unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf LaunchDarkly ein neuer Benutzer erstellt.
>[!Note]
>Wenn Sie einen Benutzer manuell erstellen müssen, setzen Sie sich mit dem [LaunchDarkly-Client-Supportteam ](mailto:support@launchdarkly.com) in Verbindung.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung der einmaligen Anmeldung von Azure, indem Sie ihr Zugriff auf LaunchDarkly gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon zu LaunchDarkly zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **LaunchDarkly** aus.

    ![LaunchDarkly-Link in der Anwendungsliste](./media/launchdarkly-tutorial/tutorial_launchdarkly_app.png)  

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „LaunchDarkly“ klicken, sollten Sie automatisch bei Ihrer LaunchDarkly-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/launchdarkly-tutorial/tutorial_general_01.png
[2]: ./media/launchdarkly-tutorial/tutorial_general_02.png
[3]: ./media/launchdarkly-tutorial/tutorial_general_03.png
[4]: ./media/launchdarkly-tutorial/tutorial_general_04.png

[100]: ./media/launchdarkly-tutorial/tutorial_general_100.png

[200]: ./media/launchdarkly-tutorial/tutorial_general_200.png
[201]: ./media/launchdarkly-tutorial/tutorial_general_201.png
[202]: ./media/launchdarkly-tutorial/tutorial_general_202.png
[203]: ./media/launchdarkly-tutorial/tutorial_general_203.png

