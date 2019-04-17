---
title: 'Tutorial: Azure Active Directory-Integration mit iLMS | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und iLMS konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9bcb465f76e09675333e6e608249cba11f722e3
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59274654"
---
# <a name="tutorial-azure-active-directory-integration-with-ilms"></a>Tutorial: Azure Active Directory-Integration in iLMS

In diesem Tutorial erfahren Sie, wie Sie iLMS in Azure Active Directory (Azure AD) integrieren.
Die Integration von iLMS in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf iLMS hat.
* Sie können Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei iLMS anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit iLMS konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Wenn Sie keine Azure AD-Umgebung besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.
* iLMS-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* iLMS unterstützt **SP- und IDP**-initiiertes einmaliges Anmelden.

## <a name="adding-ilms-from-the-gallery"></a>Hinzufügen von iLMS aus dem Katalog

Zum Konfigurieren der Integration von iLMS in Azure AD müssen Sie iLMS aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um iLMS aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **iLMS** ein, wählen Sie im Ergebnisbereich **iLMS** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![iLMS in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei iLMS basierend auf einer Testbenutzerin mit dem Namen **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in iLMS eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei iLMS müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für iLMS](#configure-ilms-single-sign-on)**, um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines iLMS-Testbenutzers](#create-ilms-test-user)**, um eine Entsprechung von Britta Simon in iLMS zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in iLMS die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **iLMS** die Option **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für iLMS](common/idp-intiated.png)

    a. Fügen Sie im Textfeld**Bezeichner** den Wert ein, den Sie im iLMS-Verwaltungsportal im Abschnitt **Service Provider** der SAML-Einstellungen aus dem Feld **Identifier** kopieren.

    b. Fügen Sie im Textfeld **Antwort-URL** den Wert ein, den Sie im iLMS-Verwaltungsportal im Abschnitt **Service Provider** der SAML-Einstellungen aus dem Feld **Endpoint (URL)** kopieren. Der Wert weist folgendes Muster auf: `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

5. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für iLMS](common/metadata-upload-additional-signon.png)

    Fügen Sie im Textfeld **Anmelde-URL** den Wert ein, den Sie im iLMS-Verwaltungsportal im Abschnitt **Service Provider** der SAML-Einstellungen aus dem Feld **Endpoint (URL)** kopieren. Der Wert weist folgendes Muster auf: `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

6. Für die Aktivierung der JIT-Bereitstellung erwartet Ihre iLMS-Anwendung die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute. Klicken Sie auf das Symbol **Bearbeiten**, um das Dialogfeld „Benutzerattribute“ zu öffnen.

    ![image](common/edit-attribute.png)

    > [!NOTE]
    > Sie müssen die Option **Create Un-recognized User Account** in iLMS aktivieren, um diese Attribute zuzuordnen. Befolgen Sie die Anweisungen [hier](http://support.inspiredelearning.com/customer/portal/articles/2204526), um eine Vorstellung von der Konfiguration der Attribute zu erhalten.

7. Darüber hinaus wird von der iLMS-Anwendung erwartet, dass in der SAML-Antwort noch einige weitere Attribute zurückgegeben werden. Führen Sie im Dialogfeld **Benutzerattribute** im Abschnitt **Benutzeransprüche** die folgenden Schritte aus, um das SAML-Tokenattribut wie in der folgenden Tabelle gezeigt hinzuzufügen:

    | NAME | Quellattribut|
    | --------|------------- |
    | division | user.department |
    | region | user.state |
    | department | user.jobtitle |

    a. Klicken Sie auf **Neuen Anspruch hinzufügen**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Lassen Sie den **Namespace** leer.

    d. Wählen Sie „Source“ als **Attribut** aus.

    e. Geben Sie in der Liste **Quellattribut** den für diese Zeile angezeigten Attributwert ein.

    f. Klicken Sie auf **OK**.

    g. Klicken Sie auf **Speichern**.

8. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um den Ihren Anforderungen entsprechenden **Verbundmetadaten-XML**-Code aus den verfügbaren Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

9. Kopieren Sie im Abschnitt **iLMS einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-ilms-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für iLMS

1. Melden Sie sich in einem anderen Webbrowserfenster im **iLMS-Verwaltungsportal** als Administrator an.

2. Klicken Sie auf der Registerkarte **Settings** auf **SSO:SAML**, um die SAML-Einstellungen zu öffnen, und führen Sie die folgenden Schritte aus:

    ![Configure single sign-on](./media/ilms-tutorial/1.png)

3. Erweitern Sie den Abschnitt **Service Provider**, und kopieren Sie die Werte für **Identifier** und **Endpoint (URL)**.

    ![Configure single sign-on](./media/ilms-tutorial/2.png) 

4. Klicken Sie im Abschnitt **Identity Provider** auf **Import Metadata**.

5. Wählen Sie die Datei mit **Verbundmetadaten** aus, die Sie aus dem Abschnitt **SAML-Signaturzertifikat** des Azure-Portals heruntergeladen haben.

    ![Configure single sign-on](./media/ilms-tutorial/tutorial_ilms_ssoconfig1.png)

6. Wenn Sie die JIT-Bereitstellung aktivieren möchten, um iLMS-Konten für unbekannte Benutzer zu erstellen, führen Sie die folgenden Schritte aus:

    a. Aktivieren Sie das Kontrollkästchen **Create Un-recognized User Account**.

    ![Configure single sign-on](./media/ilms-tutorial/tutorial_ilms_ssoconfig2.png)

    b. Ordnen Sie die Attribute in Azure AD den Attributen in iLMS zu. Geben Sie in der Spalte mit den Attributen den Attributnamen oder den Standardwert an.

    c. Wechseln Sie zur Registerkarte **Business Rules**, und führen Sie die folgenden Schritte aus:

    ![Configure single sign-on](./media/ilms-tutorial/5.png)

    d. Aktivieren Sie das Kontrollkästchen **Create Un-recognized Regions, Divisions and Departments**, um Regionen, Sparten und Abteilungen zu erstellen, die zum Zeitpunkt des einmaligen Anmeldens noch nicht vorhanden waren.

    e. Aktivieren Sie das Kontrollkästchen **Update User Profile During Sign-in**, um anzugeben, ob das Benutzerprofil bei jedem einmaligen Anmelden aktualisiert werden soll.

    f. Wenn das Kontrollkästchen **Update Blank Values for Non Mandatory Fields in User Profile** aktiviert ist, führen optionale Felder im Profil, die während des Anmeldevorgangs leer sind, dazu, dass das iLMS-Profil für diese Felder ebenfalls leere Werte enthält.

    g. Aktivieren Sie das Kontrollkästchen **Send Error Notification Email**, und geben Sie die E-Mail-Adresse des Benutzers ein, der bei Fehlern Benachrichtigungs-E-Mails erhalten soll.

7. Klicken Sie auf die Schaltfläche **Save**, um die Änderungen zu speichern.

    ![Configure single sign-on](./media/ilms-tutorial/save.png)

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

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf iLMS gewähren.

1. Wählen Sie im Azure-Portal nacheinander die Optionen **Unternehmensanwendungen**, **Alle Anwendungen** und **iLMS**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **iLMS** aus.

    ![iLMS-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-ilms-test-user"></a>Erstellen eines iLMS-Testbenutzers

Die Anwendung unterstützt die Just-in-Time-Benutzerbereitstellung (JIT). Nach der Authentifizierung werden Benutzer in der Anwendung automatisch erstellt. JIT funktioniert, wenn Sie während der Konfiguration der SAML-Einstellungen im iLMS-Verwaltungsportal das Kontrollkästchen **Create Un-recognized User Account** aktiviert haben.

Führen Sie die folgenden Schritte aus, wenn Sie einen Benutzer manuell erstellen müssen:

1. Melden Sie sich bei der iLMS-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf der Registerkarte **Users** auf **Register User**, um die Seite **Register User** zu öffnen.

   ![Mitarbeiter hinzufügen](./media/ilms-tutorial/3.png)

3. Führen Sie auf der Seite **Register User** die folgenden Schritte aus.

    ![Mitarbeiter hinzufügen](./media/ilms-tutorial/create_testuser_add.png)

    a. Geben Sie im Textfeld **Vorname** den Vornamen ein, z.B. Britta.

    b. Geben Sie im Textfeld **Nachname** den Nachnamen ein, z.B. Simon.

    c. Geben Sie im Textfeld **Email ID** die E-Mail-Adresse des Benutzers wie folgt ein: BrittaSimon@contoso.com.

    d. Wählen Sie in der Dropdownliste **Region** den Wert für die Region aus.

    e. Wählen Sie in der Dropdownliste **Division** den Wert für die Sparte aus.

    f. Wählen Sie in der Dropdownliste **Department** den Wert für die Abteilung aus.

    g. Klicken Sie auf **Speichern**.

    > [!NOTE]
    > Sie können eine Registrierungs-E-Mail an den Benutzer senden, indem Sie das Kontrollkästchen **Send Registration Mail** aktivieren.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „iLMS“ klicken, sollten Sie automatisch bei der iLMS-Anwendung angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)