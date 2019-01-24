---
title: 'Tutorial: Azure Active Directory-Integration mit Atlassian Cloud | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Atlassian Cloud konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/20/2018
ms.author: jeedes
ms.openlocfilehash: 55c1aa4a478031ebc49ec5ab7ea5744d9d980470
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54825754"
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Tutorial: Azure Active Directory-Integration mit Atlassian Cloud

In diesem Tutorial erfahren Sie, wie Sie Atlassian Cloud in Azure Active Directory (Azure AD) integrieren.
Die Integration von Atlassian Cloud in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Atlassian Cloud hat.
* Sie können es Benutzern ermöglichen, dass sie mit ihren Azure AD-Konten automatisch bei Atlassian Cloud angemeldet werden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für das Konfigurieren der Azure AD-Integration mit Atlassian Cloud benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Wenn Sie keine Azure AD-Umgebung besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.
* Atlassian Cloud-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Atlassian Cloud unterstützt **SP- und IDP**-initiiertes einmaliges Anmelden.

## <a name="adding-atlassian-cloud-from-the-gallery"></a>Hinzufügen von Atlassian Cloud aus dem Katalog

Zum Konfigurieren der Integration von Atlassian Cloud in Azure AD müssen Sie Atlassian Cloud aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Atlassian Cloud aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Atlassian Cloud** ein, wählen Sie im Ergebnisbereich **Atlassian Cloud** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![Atlassian Cloud in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden in Azure AD mit Atlassian Cloud anhand eines Testbenutzers namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Atlassian Cloud eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD mit Atlassian Cloud müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Atlassian Cloud](#configure-atlassian-cloud-single-sign-on)**, um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines Atlassian Cloud-Testbenutzers](#create-atlassian-cloud-test-user)**, um eine Entsprechung von Britta Simon in Atlassian Cloud zu erhalten, die mit ihrer Benutzerdarstellung in Azure AD verknüpft ist.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit Atlassian Cloud die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Atlassian Cloud** die Option **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für [Anwendungsname]](common/idp-relay.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://auth.atlassian.com/saml/<unique ID>`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`

    c. Klicken Sie auf **Zusätzliche URLs festlegen**.

    d. Geben Sie im Textfeld **Relayzustand** eine URL nach folgendem Muster ein: `https://<instancename>.atlassian.net`.

    > [!NOTE]
    > Die vorangehenden Werte sind keine echten Werte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Diese tatsächlichen Werte können Sie dem Bildschirm für die Atlassian Cloud-SAML-Konfiguration entnehmen. Dies wird im Verlauf des Tutorials erläutert.

5. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für [Anwendungsname]](common/both-signonurl.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<instancename>.atlassian.net`

    > [!NOTE]
    > Der hier angegebene Wert für die Anmelde-URL entspricht nicht dem tatsächlichen Wert. Aktualisieren Sie den Wert mit der tatsächlichen Anmelde-URL. Wenden Sie sich an das [Supportteam des Atlassian Cloud-Clients](https://support.atlassian.com/), um diesen Wert zu erhalten.

6. Ihre Atlassian Cloud-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen.

    Standardmäßig wird der Wert von **Benutzer-ID** dem „user.userprincipalname“ zugeordnet. Ändern Sie die Zuordnung dieses Werts in „user.mail“. Sie können je nach dem Setup Ihrer Organisation auch jeden anderen geeigneten Wert zuordnen, doch sollte E-Mail normalerweise funktionieren. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Benutzerattribute** zu öffnen.

    ![image](common/edit-attribute.png)

7. Führen Sie im Dialogfeld **Benutzerattribute** im Abschnitt **Benutzeransprüche** die folgenden Schritte aus:

    a. Klicken Sie auf das Symbol **Bearbeiten**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    ![image](./media/atlassian-cloud-tutorial/tutorial_usermail.png)

    ![image](./media/atlassian-cloud-tutorial/tutorial_usermailedit.png)

    b. Wählen Sie in der Liste **Quellattribut** den Eintrag **user.mail** aus.

    c. Klicken Sie auf **Speichern**.

8. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Zertifikat (Base64)** aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

9. Kopieren Sie im Abschnitt **Atlassian Cloud einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-atlassian-cloud-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für Atlassian Cloud

1. Melden Sie sich zum Konfigurieren von SSO für Ihre Anwendung mit Administratoranmeldeinformationen beim Atlassian-Portal an.

2. Vor dem Konfigurieren des einmaligen Anmeldens müssen Sie Ihre Domäne überprüfen. Weitere Informationen finden Sie im Dokument zur [Atlassian-Domänenüberprüfung](https://confluence.atlassian.com/cloud/domain-verification-873871234.html).

3. Wählen Sie im linken Bereich **SAML single sign-on** (Einmalige SAML-Anmeldung) aus. Wenn dies noch nicht erfolgt ist, abonnieren Sie Identity Manager von Atlassian.

    ![Einmaliges Anmelden konfigurieren](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

4. Gehen Sie im Fenster **Add SAML configuration** (SAML-Konfiguration hinzufügen) folgendermaßen vor:

    ![Einmaliges Anmelden konfigurieren](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. Fügen Sie in das Feld **Identity provider Entity ID** (Entitäts-ID des Identitätsanbieters) die ID der SAML-Entität ein, die Sie aus dem Azure-Portal kopiert haben.

    b. Fügen Sie in das Feld **Identity provider SSO URL** (SSO-URL des Identitätsanbieters) die URL des SAML-SSO-Diensts ein, die Sie aus dem Azure-Portal kopiert haben.

    c. Öffnen Sie das aus dem Azure-Portal in einer TXT-Datei heruntergeladene Zertifikat, kopieren Sie den Wert (ohne die Zeilen *Begin Certificate* und *End Certificate*), und fügen Sie es dann in das Feld **Public X509 certificate** (Öffentliches X509-Zertifikat) ein.

    d. Klicken Sie auf **Konfiguration speichern**.

5. Um sicherzustellen, dass Sie die richtigen URLs eingerichtet haben, aktualisieren Sie folgendermaßen die Azure AD-Einstellungen:

    ![Einmaliges Anmelden konfigurieren](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. Kopieren Sie im SAML-Fenster die **SP Identity ID** (SP-Identitäts-ID), und fügen Sie sie dann im Azure-Portal unter **Domäne und URLs** für Atlassian Cloud in das Feld **Bezeichner** ein.

    b. Kopieren Sie im SAML-Fenster die **SP Assertion Consumer Service URL** (URL des SP-Assertionsverbraucherdiensts), und fügen Sie sie dann im Azure-Portal unter **Domäne und URLs** für Atlassian Cloud in das Feld **Antwort-URL** ein. Die Anmelde-URL ist die Mandanten-URL für Ihre Atlassian Cloud.

    > [!NOTE]
    > Wenn Sie Bestandskunde sind, wählen Sie nach dem Aktualisieren der Werte für **SP Identity ID** (SP-Identitäts-ID) und **SP Assertion Consumer Service URL** (URL des SP-Assertionsverbraucherdiensts) **Yes, update configuration** (Ja, Konfiguration aktualisieren) aus. Als Neukunde können Sie diesen Schritt überspringen.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](common/users.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Schaltfläche „Neuer Benutzer“](common/new-user.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Dialogfeld „Benutzer“](common/user-properties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie Zugriff auf Atlassian Cloud gewähren.

1. Wählen Sie im Azure-Portal nacheinander die Optionen **Unternehmensanwendungen**, **Alle Anwendungen** und **Atlassian Cloud**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Geben Sie in der Anwendungsliste **Atlassian Cloud** ein, und wählen Sie den Eintrag aus.

    ![Wählen Sie in der Anwendungsliste Atlassian Cloud aus.](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-atlassian-cloud-test-user"></a>Erstellen eines Atlassian Cloud-Testbenutzers

Um Azure AD-Benutzern die Anmeldung bei Atlassian Cloud zu ermöglichen, stellen Sie ihre Benutzerkonten in Atlassian Cloud manuell bereit, indem Sie wie folgt vorgehen:

1. Wählen Sie im Bereich **Administration** (Verwaltung) **Users** (Benutzer) aus.

    ![Atlassian Cloud-Benutzerlink](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png)

2. Um einen Benutzer in Atlassian Cloud zu erstellen, wählen Sie **Invite user** (Benutzer einladen) aus.

    ![Erstellen eines Atlassian Cloud-Benutzers](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png)

3. Geben Sie im Feld **E-Mail-Adresse** die E-Mail-Adresse des Benutzers ein, und weisen Sie dann den Anwendungszugriff zu.

    ![Erstellen eines Atlassian Cloud-Benutzers](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)

4. Um dem Benutzer eine E-Mail-Einladung zu senden, wählen Sie **Invite users** (Benutzer einladen) aus. Eine E-Mail-Einladung wird an den Benutzer gesendet, und nach dem Akzeptieren der Einladung ist der Benutzer im System aktiv.

> [!NOTE]
> Sie können auch eine Massenerstellung von Benutzern ausführen, indem Sie die Schaltfläche **Bulk Create** (Massenerstellung) im Abschnitt **Users** (Benutzer) auswählen.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Atlassian Cloud“ klicken, sollten Sie automatisch bei Ihrer Atlassian Cloud-Anwendung angemeldet werden. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
   