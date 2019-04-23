---
title: 'Tutorial: Azure Active Directory-Integration mit Workday | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Workday konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/04/2019
ms.author: jeedes
ms.openlocfilehash: 8451fd692409933803f5f8023f1e1161c3a97daf
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59278529"
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a>Tutorial: Azure Active Directory-Integration mit Workday

In diesem Tutorial erfahren Sie, wie Sie Workday in Azure Active Directory (Azure AD) integrieren.
Die Integration von Workday in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Workday hat.
* Sie können es Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Workday anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Workday konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Wenn Sie keine Azure AD-Umgebung besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.
* Ein Workday-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Workday unterstützt **SP-** und **IDP-initiiertes** einmaliges Anmelden.

## <a name="adding-workday-from-the-gallery"></a>Hinzufügen von Workday aus dem Katalog

Zum Konfigurieren der Integration von Workday in Azure AD müssen Sie Workday aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Workday aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Workday** ein, wählen Sie im Ergebnisbereich **Workday** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![Workday in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Workday mithilfe eines Testbenutzers namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Workday eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Workday müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Workday](#configure-workday-single-sign-on)**, um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines Workday-Testbenutzers](#create-workday-test-user)**, um ein Pendant von Britta Simon in Workday zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit Workday die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Workday** die Option **Einmaliges Anmelden** aus.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Workday](common/sp-identifier.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https:\//impl.workday.com/<tenant>/login-saml2.flex`

    b. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://www.workday.com`

5. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Workday](./media/workday-tutorial/reply.png)

    Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https:\//impl.workday.com/<tenant>/login-saml.htmld`.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und Antwort-URL. Ihre Antwort-URL muss eine Unterdomäne aufweisen (z.B. www, wd2, wd3, wd3-impl, wd5, wd5-impl).
    > Eine URL wie `http://www.myworkday.com` funktioniert, `http://myworkday.com` hingegen nicht. Wenden Sie sich an das [Supportteam für den Workday-Client](https://www.workday.com/en-us/partners-services/services/support.html), um diese Werte zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

6. Die Workday-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute. **nameidentifier** ist hier **user.userprincipalname** zugeordnet. Die Workday-Anwendung erwartet, dass **nameidentifier** die Werte für **user.mail**, **UPN** usw. zugeordnet sind. Sie müssen die Attributzuordnung daher bearbeiten, indem Sie auf das Symbol **Bearbeiten** klicken und die Attributzuordnung ändern.

    ![image](common/edit-attribute.png)

    > [!NOTE]
    > Hier ist die Namens-ID mit Benutzerprinzipalname (user.userprincipalname) als Standard zugeordnet. Sie müssen die Namens-ID der tatsächlichen Benutzer-ID in Ihrem Workday-Konto (E-Mail-Adresse, UPN usw.) zuordnen, damit das einmalige Anmelden funktioniert.

7. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Zertifikat (Base64)** aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

8. Kopieren Sie im Abschnitt **Workday einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-workday-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für Workday

1. Melden Sie sich in einem anderen Webbrowserfenster auf der Workday-Unternehmenswebsite als Administrator an.

2. Geben Sie oben links auf der Startseite im **Suchfeld** den Namen **Mandanten-Setup bearbeiten – Sicherheit** ein.

    ![Mandantensicherheit bearbeiten](./media/workday-tutorial/IC782925.png "Mandantensicherheit bearbeiten")

3. Führen Sie im Abschnitt **Umleitungs-URLs** die folgenden Schritte aus:

    ![Umleitungs-URLs](./media/workday-tutorial/IC7829581.png "Umleitungs-URLs")

    a. Klicken Sie auf **Zeile hinzufügen**.

    b. Geben Sie in den Textfeldern **Login Redirect URL** (Umleitungs-URL für Anmeldung) und **Mobile Redirect URL** (Mobile Umleitungs-URL) die **Anmelde-URL** ein, die Sie im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** eingegeben haben.

    c. Kopieren Sie im Azure-Portal im Abschnitt **Workday einrichten** die **Abmelde-URL**, und fügen Sie sie in das Textfeld **Logout Redirect URL** (Umleitungs-URL für Abmeldung) ein.

    d. Wählen Sie im Textfeld **Used for Environments** (Für Umgebungen verwendet) den Umgebungsnamen aus.  

   > [!NOTE]
   > Der Wert des Umgebungsattributs wird an den Wert der Mandanten-URL gebunden:  
   > - Wenn der Domänenname der Workday-Mandanten-URL mit „impl“ beginnt (z. B. *https:\//impl.workday.com/\<tenant\>/login-saml2.flex*) muss das Attribut **Environment** (Umgebung) auf „Implementation“ (Implementierung) festgelegt werden.  
   > - Wenn der Domänenname anders beginnt, müssen Sie sich mit dem [Supportteam für den Workday-Client](https://www.workday.com/en-us/partners-services/services/support.html) in Verbindung setzen, um den passenden Wert für die **Umgebung** zu erhalten.

4. Führen Sie im Abschnitt **SAML-Setup** die folgenden Schritte aus:

    ![SAML-Setup](./media/workday-tutorial/IC782926.png "SAML-Setup")

    a.  Wählen Sie die Option **SAML-Authentifizierung aktivieren**.

    b.  Klicken Sie auf **Zeile hinzufügen**.

5. Führen Sie im Abschnitt **SAML-Identitätsanbieter** die folgenden Schritte aus:

    ![SAML-Identitätsanbieter](./media/workday-tutorial/IC7829271.png "SAML-Identitätsanbieter")

    a. Geben Sie im Textfeld **Name des Identitätsanbieters** einen Anbieternamen ein (z.B: *SPInitiatedSSO*).

    b. Kopieren Sie im Azure-Portal im Abschnitt **Workday einrichten** den **Azure AD-Bezeichner**, und fügen Sie sie in das Textfeld **Issuer** (Aussteller) ein.

    ![SAML-Identitätsanbieter](./media/workday-tutorial/IC7829272.png "SAML-Identitätsanbieter")

    c. Kopieren Sie im Azure-Portal im Abschnitt **Workday einrichten** die **Abmelde-URL**, und fügen Sie sie in das Textfeld **Logout Response URL** (Antwort-URL für Abmeldung) ein.

    d. Kopieren Sie im Azure-Portal im Abschnitt **Workday einrichten** die **Anmelde-URL**, und fügen Sie sie in das Textfeld **IdP SSO Service URL** (Dienst-URL des Identitätsanbieters für einmaliges Anmelden) ein.

    e. Wählen Sie im Textfeld **Used for Environments** (Für Umgebungen verwendet) den Umgebungsnamen aus.

    f. Klicken Sie auf **Identity Provider Public Key Certificate** (Öffentliches Schlüsselzertifikat des Identitätsanbieters), und klicken Sie dann auf **Erstellen**.

    ![Erstellen](./media/workday-tutorial/IC782928.png "Erstellen")

    g. Klicken Sie auf **Öffentlichen x509-Schlüssel erstellen**.

    ![Erstellen](./media/workday-tutorial/IC782929.png "Erstellen")

6. Führen Sie im Abschnitt **Öffentlichen x509-Schlüssel anzeigen** die folgenden Schritte aus:

    ![Öffentlichen x509-Schlüssel anzeigen](./media/workday-tutorial/IC782930.png "Öffentlichen x509-Schlüssel anzeigen")

    a. Geben Sie im Textfeld **Name** einen Namen für Ihr Zertifikat ein (z.B.: *PPE\_SP*).

    b. Geben Sie im Textfeld **Gültig ab** den „Gültig-ab“-Attributwert des Zertifikats ein.

    c.  Geben Sie im Textfeld **Gültig bis** den „Gültig-bis“-Attributwert des Zertifikats ein.

    > [!NOTE]
    > Sie können die „Gültig ab“- und „Gültig bis“-Daten per Doppelklick aus dem heruntergeladenen Zertifikat abrufen.  Die Datumsangaben sind in der Registerkarte **Details** aufgeführt.
    >
    >

    d.  Öffnen Sie das Base64-codierte Zertifikat in Editor, und kopieren Sie seinen Inhalt.

    e.  Fügen Sie in der Textbox **Zertifikat** den Inhalt der Zwischenablage ein.

    f.  Klicken Sie auf **OK**.

7. Führen Sie die folgenden Schritte aus:

    ![SSO-Konfiguration](./media/workday-tutorial/WorkdaySSOConfiguratio.png "SSO-Konfiguration")

    a.  Geben Sie in das Textfeld **Dienstanbieter-ID** Folgendes ein: **https://www.workday.com**.

    b. Wählen Sie **SP-initiierte Authentifizierungsanfrage nicht verkleinern**.

    c. Wählen Sie als **Authentication Request Signature Method** (Signaturmethode für Authentifizierungsanfragen) die Option **SHA256** aus.

    ![Signaturmethode für Authentifizierungsanfragen](./media/workday-tutorial/WorkdaySSOConfiguration.png "Signaturmethode für Authentifizierungsanfragen") 

    d. Klicken Sie auf **OK**.

    ![OK](./media/workday-tutorial/IC782933.png "OK")

    > [!NOTE]
    > Stellen Sie sicher, dass Sie das einmalige Anmelden ordnungsgemäß eingerichtet haben. Falls Sie das einmalige Anmelden mit der falschen Einrichtung aktivieren, können Sie die Anwendung möglicherweise nicht mit Ihren Anmeldeinformationen öffnen und werden gesperrt. In diesem Fall bietet Workday eine Sicherungs-URL für die Anmeldung, mit der Benutzer ihren normalen Benutzernamen mit Kennwort im folgenden Format verwenden können: [Ihre Workday-URL]/login.flex?redirect=n

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers 

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](common/users.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Schaltfläche „Neuer Benutzer“](common/new-user.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Dialogfeld „Benutzer“](common/user-properties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** Folgendes ein: **brittasimon\@ihreunternehmensdomäne.erweiterung**.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Workday gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** > **Workday** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Workday** aus.

    ![Workday-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-workday-test-user"></a>Erstellen eines Workday-Testbenutzers

In diesem Abschnitt erstellen Sie in Workday einen Benutzer namens Britta Simon. Arbeiten Sie mit dem [Supportteam für den Workday-Client](https://www.workday.com/en-us/partners-services/services/support.html) zusammen, um die Benutzer zur Workday-Plattform hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Workday“ klicken, sollten Sie automatisch bei der Workday-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

