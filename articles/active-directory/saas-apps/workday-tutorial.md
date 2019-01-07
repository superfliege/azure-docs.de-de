---
title: 'Tutorial: Azure Active Directory-Integration mit Workday | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Workday konfigurieren.
services: active-directory
documentationCenter: na
author: cmmdesai
manager: mtillman
ms.reviewer: jeedes
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2018
ms.author: chmutali
ms.openlocfilehash: 9c789f5fec9b31b53d316b23faad5c438b52137c
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52843341"
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a>Tutorial: Azure Active Directory-Integration mit Workday

In diesem Tutorial erfahren Sie, wie Sie Workday in Azure Active Directory (Azure AD) integrieren.

Die Integration von Workday in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Workday hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Workday anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Workday konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Workday-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Workday aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-workday-from-the-gallery"></a>Hinzufügen von Workday aus dem Katalog

Zum Konfigurieren der Integration von Workday in Azure AD müssen Sie Workday aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Workday aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Workday** ein, wählen Sie im Ergebnisbereich **Workday** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Workday in der Ergebnisliste](./media/workday-tutorial/tutorial_workday_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Workday mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Workday als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Workday muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Workday den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Workday müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Workday-Testbenutzers](#create-a-workday-test-user)**, um ein Pendant von Britta Simon in Workday zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Workday-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Workday die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Workday** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Dialogfeld „Einmaliges Anmelden“](./media/workday-tutorial/tutorial_workday_samlbase.png)

3. Führen Sie auf der Seite **Domäne und URLs für Workday** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Workday](./media/workday-tutorial/tutorial_workday_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://impl.workday.com/<tenant>/login-saml2.htmld`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL ein: `https://www.workday.com`

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie den folgenden Schritt aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Workday](./media/workday-tutorial/tutorial_workday_url1.png)

    Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://impl.workday.com/<tenant>/login-saml.htmld`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und Antwort-URL. Ihre Antwort-URL muss eine Unterdomäne aufweisen (z.B. www, wd2, wd3, wd3-impl, wd5, wd5-impl).
    > Eine URL wie „*http://www.myworkday.com*“ funktioniert, aber „*http://myworkday.com*“ funktioniert dagegen nicht. Wenden Sie sich an das [Supportteam für den Workday-Client](https://www.workday.com/en-us/partners-services/services/support.html), um diese Werte zu erhalten.

5. Die Workday-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Konfiguration.

    ![Configure single sign-on](./media/Workday-tutorial/tutorial_workday_attributes.png)

    > [!NOTE]
    > Hier ist die Namens-ID mit Benutzerprinzipalname (user.userprincipalname) als Standard zugeordnet. Sie müssen die Namens-ID der tatsächlichen Benutzer-ID in Ihrem Workday-Konto (E-Mail-Adresse, UPN usw.) zuordnen, damit das einmalige Anmelden funktioniert.

6. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/workday-tutorial/tutorial_workday_certificate.png)

7. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/workday-tutorial/tutorial_general_400.png)

8. Klicken Sie im Abschnitt **Workday-Konfiguration** auf **Workday konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Workday-Konfiguration](./media/workday-tutorial/tutorial_workday_configure.png)

9. Melden Sie sich in einem anderen Webbrowserfenster auf der Workday-Unternehmenswebsite als Administrator an.

10. Geben Sie oben links auf der Startseite im **Suchfeld** den Namen **Mandanten-Setup bearbeiten – Sicherheit** ein.

    ![Mandantensicherheit bearbeiten](./media/workday-tutorial/IC782925.png "Mandantensicherheit bearbeiten")

11. Führen Sie im Abschnitt **Umleitungs-URLs** die folgenden Schritte aus:

    ![Umleitungs-URLs](./media/workday-tutorial/IC7829581.png "Umleitungs-URLs")

    a. Klicken Sie auf **Zeile hinzufügen**.

    b. Geben Sie in den Textfeldern **Umleitungs-URL für Anmeldung** und **Mobile Redirect URL** (Mobile Umleitungs-URL) die **Anmelde-URL** ein, die Sie im Abschnitt **Domäne und URLs für Workday** im Azure-Portal eingegeben haben.

    c. Kopieren Sie im Azure-Portal im Fenster **Anmeldung konfigurieren** die **Abmelde-URL**, und fügen Sie ihn in das Textfeld **Logout Redirect URL** (Umleitungs-URL für Abmeldung) ein.

    d. Wählen Sie im Textfeld **Used for Environments** (Für Umgebungen verwendet) den Umgebungsnamen aus.  

    >[!NOTE]
    > Der Wert des Umgebungsattributs wird an den Wert der Mandanten-URL gebunden:  
    >- Wenn der Domänenname der Workday-Mandanten-URL mit „impl“ beginnt (z.B. *https://impl.workday.com/\<tenant\>/login-saml2.htmld*), muss das Attribut **Environment** (Umgebung) auf „Implementation“ (Implementierung) festgelegt werden.  
    >- Wenn der Domänenname anders beginnt, müssen Sie sich mit dem [Supportteam für den Workday-Client](https://www.workday.com/en-us/partners-services/services/support.html) in Verbindung setzen, um den passenden Wert für die **Umgebung** zu erhalten.

12. Führen Sie im Abschnitt **SAML-Setup** die folgenden Schritte aus:

    ![SAML-Setup](./media/workday-tutorial/IC782926.png "SAML-Setup")

    a.  Wählen Sie die Option **SAML-Authentifizierung aktivieren**.

    b.  Klicken Sie auf **Zeile hinzufügen**.

13. Führen Sie im Abschnitt **SAML-Identitätsanbieter** die folgenden Schritte aus:

    ![SAML-Identitätsanbieter](./media/workday-tutorial/IC7829271.png "SAML-Identitätsanbieter")

    a. Geben Sie im Textfeld **Name des Identitätsanbieters** einen Anbieternamen ein (z.B: *SPInitiatedSSO*).

    b. Kopieren Sie im Azure-Portal im Fenster **Anmeldung konfigurieren** den Wert für die **SAML-Entitäts-ID**, und fügen Sie ihn in das Textfeld **Aussteller** ein.

    ![SAML-Identitätsanbieter](./media/workday-tutorial/IC7829272.png "SAML-Identitätsanbieter")

    c. Kopieren Sie im Azure-Portal im Fenster **Anmeldung konfigurieren** den Wert der **Abmelde-URL**, und fügen Sie ihn in das Textfeld **Logout Response URL** (Antwort-URL für Abmeldung) ein.

    d. Kopieren Sie im Azure-Portal im Fenster **Anmeldung konfigurieren** den Wert für die **SAML-Dienst-URL für einmaliges Anmelden**, und fügen Sie ihn in das Textfeld **IdP SSO Service URL** (Dienst-URL des Identitätsanbieters für einmaliges Anmelden) ein.

    e. Wählen Sie im Textfeld **Used for Environments** (Für Umgebungen verwendet) den Umgebungsnamen aus.

    f. Klicken Sie auf **Identity Provider Public Key Certificate** (Öffentliches Schlüsselzertifikat des Identitätsanbieters), und klicken Sie dann auf **Erstellen**.

    ![Erstellen](./media/workday-tutorial/IC782928.png "Erstellen")

    g. Klicken Sie auf **Öffentlichen x509-Schlüssel erstellen**.

    ![Erstellen](./media/workday-tutorial/IC782929.png "Erstellen")

14. Führen Sie im Abschnitt **Öffentlichen x509-Schlüssel anzeigen** die folgenden Schritte aus:

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

15. Führen Sie die folgenden Schritte aus:

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

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/workday-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/workday-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/workday-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/workday-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-workday-test-user"></a>Erstellen eines Workday-Testbenutzers

In diesem Abschnitt erstellen Sie in Workday einen Benutzer namens Britta Simon. Arbeiten Sie mit dem [Supportteam für den Workday-Client](https://www.workday.com/en-us/partners-services/services/support.html) zusammen, um die Benutzer zur Workday-Plattform hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können. 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Workday gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Workday zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Workday** aus.

    ![Workday-Link in der Anwendungsliste](./media/workday-tutorial/tutorial_workday_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über das Zugriffspanel.

Wenn Sie im Zugriffspanel auf die Kachel „Workday“ klicken, sollten Sie automatisch bei Ihrer Workday-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffspanel finden Sie unter [Einführung in das Zugriffspanel](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/workday-tutorial/tutorial_general_01.png
[2]: ./media/workday-tutorial/tutorial_general_02.png
[3]: ./media/workday-tutorial/tutorial_general_03.png
[4]: ./media/workday-tutorial/tutorial_general_04.png

[100]: ./media/workday-tutorial/tutorial_general_100.png

[200]: ./media/workday-tutorial/tutorial_general_200.png
[201]: ./media/workday-tutorial/tutorial_general_201.png
[202]: ./media/workday-tutorial/tutorial_general_202.png
[203]: ./media/workday-tutorial/tutorial_general_203.png
