---
title: 'Tutorial: Azure Active Directory-Integration mit BlueJeans| Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und BlueJeans konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2018
ms.author: jeedes
ms.openlocfilehash: cff1512c56dba9907adbf1bb4452f11d47d0787d
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50095227"
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Tutorial: Azure Active Directory-Integration mit BlueJeans

In diesem Tutorial erfahren Sie, wie Sie BlueJeans in Azure Active Directory (Azure AD) integrieren.

Die Integration von BlueJeans in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf BlueJeans hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei BlueJeans anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit BlueJeans konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein BlueJeans-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von BlueJeans aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-bluejeans-from-the-gallery"></a>Hinzufügen von BlueJeans aus dem Katalog

Zum Konfigurieren der Integration von BlueJeans in Azure AD müssen Sie BlueJeans aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um BlueJeans aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **BlueJeans** ein, wählen Sie im Ergebnisbereich **BlueJeans** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![BlueJeans in der Ergebnisliste](./media/bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei BlueJeans basierend auf einer Testbenutzerin mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in BlueJeans als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in BlueJeans muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei BlueJeans müssen Sie die folgenden Schritte ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. **[Erstellen eines BlueJeans-Testbenutzers](#creating-a-bluejeans-test-user)**, um ein Pendant von Britta Simon in BlueJeans zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer BlueJeans-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei BlueJeans die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **BlueJeans** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![Configure single sign-on](./media/bluejeans-tutorial/tutorial_general_301.png)

4. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Configure single sign-on](./media/bluejeans-tutorial/tutorial_bluejeans_editurl.png)

5. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für BlueJeans](./media/bluejeans-tutorial/tutorial_bluejeans_url.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<companyname>.BlueJeans.com`.

    > [!NOTE]
    > Der Wert der Anmelde-URL entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Supportteam für den BlueJeans-Client](https://support.bluejeans.com/contact), um diesen Wert zu erhalten.

6. Klicken Sie auf der Seite **SAML-Signaturzertifikat** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das **Zertifikat (Base64)** herunterzuladen. Speichern Sie die Zertifikatsdatei anschließend auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/bluejeans-tutorial/tutorial_bluejeans_certficate.png) 

7. Kopieren Sie im Abschnitt **BlueJeans einrichten** die entsprechende URL gemäß Ihren Anforderungen.

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

    ![BlueJeans-Konfiguration](./media/bluejeans-tutorial/tutorial_bluejeans_configure.png) 

8. Melden Sie sich in einem anderen Webbrowserfenster bei der **BlueJeans**-Unternehmenswebsite als Administrator an.

9. Wechseln Sie zu **ADMIN \> GROUP SETTINGS \> SECURITY** (ADMINISTRATOR > GRUPPENEINSTELLUNGEN > SICHERHEIT).

    ![Admin](./media/bluejeans-tutorial/IC785868.png "Admin")

10. Führen Sie im Abschnitt **SECURITY** (SICHERHEIT) die folgenden Schritte aus:

    ![Einmaliges Anmelden für SAML](./media/bluejeans-tutorial/IC785869.png "Einmaliges Anmelden für SAML")

    a. Wählen Sie **SAML Single Sign On**aus.

    b. Wählen Sie **Enable automatic provisioning**aus.

11. Führen Sie die folgenden Schritte aus:

    ![Zertifikatpfad](./media/bluejeans-tutorial/IC785870.png "Zertifikatpfad")

    a. Klicken Sie auf **Choose File** (Datei auswählen), um das Base64-codierte Zertifikat hochzuladen, das Sie über das Azure-Portal heruntergeladen haben.

    b. Fügen Sie in das Textfeld **Login URL** (Anmelde-URL) den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Fügen Sie in das Textfeld **Password Change URL** (URL für Kennwortänderung) den Wert der **URL für Kennwortänderung** ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Fügen Sie in das Textfeld **Logout URL** (Abmelde-URL) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

12. Führen Sie die folgenden Schritte aus:

    ![Änderungen speichern](./media/bluejeans-tutorial/IC785874.png "Änderungen speichern")

    a. Geben Sie in das Textfeld **Benutzer-ID** `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` ein.

    b. Geben Sie in das Textfeld **E-Mail** `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` ein.

    c. Klicken Sie auf **ÄNDERUNGEN SPEICHERN**.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Azure AD-Benutzer erstellen][100]

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Erstellen eines Azure AD-Testbenutzers](./media/bluejeans-tutorial/create_aaduser_01.png) 

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Erstellen eines Azure AD-Testbenutzers](./media/bluejeans-tutorial/create_aaduser_02.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** den Namen **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

### <a name="creating-a-bluejeans-test-user"></a>Erstellen eines BlueJeans-Testbenutzers

In diesem Abschnitt wird in BlueJeans eine Benutzerin namens Britta Simon erstellt. BlueJeans unterstützt die automatische Benutzerbereitstellung, die standardmäßig aktiviert ist. Weitere Details zur Konfiguration der automatischen Benutzerbereitstellung finden Sie [hier](bluejeans-provisioning-tutorial.md).

**Wenn Sie einen Benutzer manuell erstellen möchten, führen Sie die folgenden Schritte aus:**

1. Melden Sie sich bei der **BlueJeans** -Unternehmenswebsite als Administrator an.

2. Navigieren Sie zu **ADMIN \> MANAGE USERS \> ADD USER** (ADMINISTRATOR > BENUTZER VERWALTEN > BENUTZER HINZUFÜGEN).

    ![Admin](./media/bluejeans-tutorial/IC785877.png "Admin")

    >[!IMPORTANT]
    >Die Registerkarte **ADD USER** (BENUTZER HINZUFÜGEN) ist nur verfügbar, wenn auf der Registerkarte **SECURITY** (SICHERHEIT) die Option **Enable automatic provisioning** (Automatische Bereitstellung aktivieren) deaktiviert ist. 

3. Führen Sie im Abschnitt **ADD USER** (BENUTZER HINZUFÜGEN) die folgenden Schritte aus:

    ![Benutzer hinzufügen](./media/bluejeans-tutorial/IC785886.png "Benutzer hinzufügen")

    a. Geben Sie im Textfeld **First Name** (Vorname) den Vornamen des Benutzers ein, z.B. **Britta**.

    b. Geben Sie im Textfeld **Last Name** (Nachname) den Nachnamen des Benutzers ein, z.B. **Simon**.

    c. Geben Sie im Textfeld **Pick a BlueJeans Username** (BlueJeans-Benutzernamen auswählen) den Namen eines Benutzers ein, etwa **Brittasimon**

    d. Geben Sie in das Textfeld **Create a Password** (Kennwort erstellen) Ihr Kennwort ein.

    e. Geben Sie im Textfeld **Company** (Unternehmen) Ihr Unternehmen ein.

    f. Geben Sie im Textfeld **Email Address** (E-Mail-Adresse) die E-Mail-Adresse des Benutzers ein, z.B. **brittasimon@contoso.com**.

    g. Geben Sie im Textfeld **Create a BlueJeans Meeting I.D** (BlueJeans-Meeting-ID erstellen) Ihre Meeting-ID ein.

    h. Geben Sie im Textfeld **Pick a Moderator Passcode** (Moderatorenkennung auswählen) Ihre Kennung ein.

    i. Klicken Sie auf **CONTINUE** (WEITER).

    ![Hinzufügen eines Benutzers](./media/bluejeans-tutorial/IC785887.png "Hinzufügen eines Benutzers")

    J. Klicken Sie auf **BENUTZER HINZUFÜGEN**.

>[!NOTE]
>Sie können Azure AD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von BlueJeans-Benutzerkonten oder mithilfe von APIs bereitstellen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf BlueJeans gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste die Option **BlueJeans** aus.

    ![Configure single sign-on](./media/bluejeans-tutorial/tutorial_bluejeans_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „BlueJeans“ klicken, sollten Sie automatisch bei Ihrer BlueJeans-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/bluejeans-tutorial/tutorial_general_203.png
