---
title: 'Tutorial: Azure Active Directory-Integration mit Soloinsight-CloudGate SSO | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Soloinsight-CloudGate SSO konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 9263c241-85a4-4724-afac-0351d6275958
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/07/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5839d7212958cffe45fae19f1e502ecf04e744a3
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56162050"
---
# <a name="tutorial-azure-active-directory-integration-with-soloinsight-cloudgate-sso"></a>Tutorial: Azure Active Directory-Integration mit Soloinsight-CloudGate SSO

In diesem Tutorial erfahren Sie, wie Sie Soloinsight-CloudGate SSO in Azure Active Directory (Azure AD) integrieren.
Die Integration von Soloinsight-CloudGate SSO in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Soloinsight-CloudGate SSO hat.
* Sie können es Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Soloinsight-CloudGate SSO anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Soloinsight-CloudGate SSO konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Wenn Sie keine Azure AD-Umgebung besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.
* Soloinsight-CloudGate SSO-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Soloinsight-CloudGate SSO unterstützt **SP**-initiiertes einmaliges Anmelden.

## <a name="adding-soloinsight-cloudgate-sso-from-the-gallery"></a>Hinzufügen von Soloinsight-CloudGate SSO aus dem Katalog

Zum Konfigurieren der Integration von Soloinsight-CloudGate SSO in Azure AD müssen Sie Soloinsight-CloudGate SSO aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Soloinsight-CloudGate SSO aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Soloinsight-CloudGate SSO** ein, wählen Sie im Ergebnisbereich **Soloinsight-CloudGate SSO**, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![Soloinsight-CloudGate SSO in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden über Azure AD bei Soloinsight-CloudGate SSO mithilfe einer Testbenutzerin namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Soloinsight-CloudGate SSO eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens bei Soloinsight-CloudGate SSO über Azure AD müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Soloinsight-CloudGate SSO](#configure-soloinsight-cloudgate-sso-single-sign-on)**, um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines Soloinsight-CloudGate SSO-Testbenutzers](#create-soloinsight-cloudgate-sso-test-user)**, um über eine Entsprechung von Britta Simon in Soloinsight-CloudGate SSO zu verfügen, die mit der Darstellung des Benutzers in Azure AD verknüpft ist.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens bei Soloinsight-CloudGate SSO über Azure AD die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Soloinsight-CloudGate SSO** die Option **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Soloinsight-CloudGate SSO](common/sp-identifier.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<SUBDOMAIN>.sigateway.com/login`.

    b. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL im folgenden Format ein: `https://<SUBDOMAIN>.sigateway.com/process/sso`.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit der tatsächlichen Anmelde-URL und dem Bezeichner. Dies ist später in diesem Tutorial im Abschnitt **Konfigurieren des einmaligen Anmeldens für Soloinsight-CloudGate SSO** beschrieben.

4. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Zertifikat (Base64)** aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

6. Kopieren Sie im Abschnitt **Soloinsight-CloudGate SSO einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-soloinsight-cloudgate-sso-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für Soloinsight-CloudGate SSO

1. Melden Sie sich mit Ihren Anmeldeinformationen am CloudGate-Webportal an, um die Werte abzurufen, die bei der grundlegenden SAML-Konfiguration im Azure-Portal eingefügt werden sollen. Greifen Sie anschließend auf die SSO-Einstellungen zu, die sich unter dem Pfad **Startseite > Verwaltung > Systemeinstellungen > Allgemein** befinden.

    ![CloudGate SSO-Einstellungen](./media/soloinsight-cloudgate-sso-tutorial/sso-main-settings.png)

2. **SAML-Consumer-URL**

    * Kopieren Sie die Links in den Feldern **SAML-Consumer-URL** und **Umleitungs-URL**, und fügen Sie sie im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** in den Feldern **Bezeichner (Entitäts-ID)** und **Antwort-URL** ein.

        ![SAMLIdentifier](./media/soloinsight-cloudgate-sso-tutorial/saml-identifier.png)

3. **SAML-Signaturzertifikat**

    * Navigieren Sie zur Quelle der Zertifikatdatei (Base64), die aus den Listen unter „SAML-Signaturzertifikat“ im Azure-Portal heruntergeladen wurden, und klicken Sie mit der rechten Maustaste darauf. Wählen Sie in der Liste die Option **Edit with Notepad++** (Mit Notepad++ bearbeiten). 

        ![SAMLcertificate](./media/soloinsight-cloudgate-sso-tutorial/certificate-file.png)

    * Kopieren Sie den Inhalt der Notepad++-Zertifikatdatei (Base64).

        ![Zertifikatkopie](./media/soloinsight-cloudgate-sso-tutorial/certificate-copy.png)

    * Fügen Sie den Inhalt in den SSO-Einstellungen im CloudGate-Webportal in das Feld **Zertifikat** ein, und klicken Sie auf die Schaltfläche „Speichern“.

        ![Zertifikatportal](./media/soloinsight-cloudgate-sso-tutorial/certificate-portal.png)

4. **Default Group** (Standardgruppe)

    * Wählen Sie im CloudGate-Webportal in der Dropdownliste von **Default Group** (Standardgruppe) die Option **Business Admin** (Unternehmensadministrator).

        ![Standardgruppe](./media/soloinsight-cloudgate-sso-tutorial/default-group.png)

5. **AD-Bezeichner und Anmelde-URL**

    * Die kopierte **Anmelde-URL** aus der Konfiguration unter **Soloinsight-CloudGate SSO einrichten** im Azure-Portal muss in den Abschnitt mit den SSO-Einstellungen des CloudGate-Webportals eingegeben werden. 

    * Fügen Sie den Link **Anmelde-URL** aus dem Azure-Portal in das Feld **AD-Anmelde-URL** im CloudGate-Webportal ein.
     
    * Fügen Sie den Link **Azure AD-Bezeichner** aus dem Azure-Portal in das Feld **AD-Bezeichner** im CloudGate-Webportal ein.

        ![AD-Anmeldung](./media/soloinsight-cloudgate-sso-tutorial/ad-login.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers 

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](common/users.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Schaltfläche „Neuer Benutzer“](common/new-user.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Dialogfeld „Benutzer“](common/user-properties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** den Namen **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf Soloinsight-CloudGate SSO gewähren.

1. Wählen Sie im Azure-Portal nacheinander die Optionen **Unternehmensanwendungen**, **Alle Anwendungen** und **Soloinsight-CloudGate SSO**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste die Option **Soloinsight-CloudGate SSO**.

    ![Soloinsight-CloudGate SSO-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-soloinsight-cloudgate-sso-test-user"></a>Erstellen eines Soloinsight-CloudGate SSO-Testbenutzers

Wählen Sie zum Erstellen eines Testbenutzers im Hauptmenü Ihres CloudGate-Webportals die Option **Mitarbeiter**, und füllen Sie das Formular „Add New employee“ (Neuen Mitarbeiter hinzufügen) aus. Die Autoritätsebene, die dem Testbenutzer zugewiesen werden soll, lautet **Business Admin** (Unternehmensadministrator). Klicken Sie auf **Erstellen**, nachdem alle erforderlichen Felder ausgefüllt wurden.

![Mitarbeitertest](./media/soloinsight-cloudgate-sso-tutorial/employee-test.png)

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Soloinsight-CloudGate SSO“ klicken, sollten Sie automatisch an der Soloinsight-CloudGate SSO-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

