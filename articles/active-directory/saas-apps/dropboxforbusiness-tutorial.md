---
title: 'Tutorial: Azure Active Directory-Integration mit Dropbox für Unternehmen | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Dropbox für Unternehmen konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/20/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 948121ad6b3a0f666fcc8fa64096bab91927b474
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56186147"
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Tutorial: Azure Active Directory-Integration mit Dropbox für Unternehmen

In diesem Tutorial erfahren Sie, wie Sie Dropbox für Unternehmen in Azure Active Directory (Azure AD) integrieren.
Die Integration von Dropbox für Unternehmen in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD den Zugriff auf Dropbox für Unternehmen steuern.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Dropbox für Unternehmen anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Dropbox für Unternehmen konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Wenn Sie keine Azure AD-Umgebung besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.
* Abonnement von Dropbox für Unternehmen, mit dem das einmalige Anmelden möglich ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Dropbox für Unternehmen unterstützt **SP**-initiiertes SSO.

* Dropbox für Unternehmen unterstützt die **Just-in-Time**-Benutzerbereitstellung.

## <a name="adding-dropbox-for-business-from-the-gallery"></a>Hinzufügen von Dropbox für Unternehmen aus dem Katalog

Zum Konfigurieren der Integration von Dropbox für Unternehmen in Azure AD müssen Sie Dropbox für Unternehmen aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Dropbox für Unternehmen aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Dropbox für Unternehmen** ein, wählen Sie im Ergebnisbereich **Dropbox für Unternehmen** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![Dropbox für Unternehmen in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Dropbox für Unternehmen mit einem Testbenutzer namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Dropbox für Unternehmen eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Dropbox für Unternehmen müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Dropbox für Unternehmen](#configure-dropbox-for-business-single-sign-on)**, um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines Dropbox für Unternehmen-Testbenutzers](#create-dropbox-for-business-test-user)**, um in Dropbox für Unternehmen eine Entsprechung von Britta Simon zu erhalten, die mit ihrer Benutzerdarstellung in Azure AD verknüpft ist.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Dropbox für Unternehmen die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Dropbox für Unternehmen** die Option **Einmaliges Anmelden** aus.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Dropbox für Unternehmen](common/sp-identifier.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://www.dropbox.com/sso/<id>`.

    b. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** einen Wert ein: `Dropbox`.

    > [!NOTE]
    > Der hier angegebene Wert für die Anmelde-URL ist kein echter Wert. Er muss mit der tatsächlichen Anmelde-URL aktualisiert werden. Dies wird weiter unten in diesem Tutorial beschrieben.

4. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Zertifikat (Base64)** aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

6. Kopieren Sie im Abschnitt **Dropbox für Unternehmen einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-dropbox-for-business-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für Dropbox für Unternehmen

1. Navigieren Sie zu Ihrem Dropbox für Unternehmen-Mandanten, und melden Sie sich bei diesem Mandanten an, um das einmalige Anmelden aufseiten von **Dropbox für Unternehmen** zu konfigurieren.

    ![Einmaliges Anmelden konfigurieren](./media/dropboxforbusiness-tutorial/ic769509.png "Einmaliges Anmelden konfigurieren")

2. Klicken Sie auf das **Benutzersymbol**, und wählen Sie die Registerkarte **Einstellungen** aus.

    ![Einmaliges Anmelden konfigurieren](./media/dropboxforbusiness-tutorial/configure1.png "Einmaliges Anmelden konfigurieren")

3. Klicken Sie im Navigationsbereich auf der linken Seite auf **Verwaltungskonsole**.

    ![Einmaliges Anmelden konfigurieren](./media/dropboxforbusiness-tutorial/configure2.png "Einmaliges Anmelden konfigurieren")

4. Klicken Sie in der **Verwaltungskonsole** im linken Navigationsbereich auf **Einstellungen**.

    ![Einmaliges Anmelden konfigurieren](./media/dropboxforbusiness-tutorial/configure3.png "Einmaliges Anmelden konfigurieren")

5. Wählen Sie im Abschnitt **Authentifizierung** die Option **Einmaliges Anmelden** aus.

    ![Einmaliges Anmelden konfigurieren](./media/dropboxforbusiness-tutorial/configure4.png "Einmaliges Anmelden konfigurieren")

6. Führen Sie im Abschnitt **Einmaliges Anmelden** die folgenden Schritte aus:  

    ![Einmaliges Anmelden konfigurieren](./media/dropboxforbusiness-tutorial/configure5.png "Einmaliges Anmelden konfigurieren")

    a. Wählen Sie in der Dropdownliste für **Einmaliges Anmelden** die Option **Erforderlich** aus.

    b. Klicken Sie auf **Anmelde-URL hinzufügen**, und fügen Sie den Wert der **Anmelde-URL**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Anmelde-URL des Identitätsanbieters** ein. Anschließend klicken Sie auf **Fertig**.

    ![Einmaliges Anmelden konfigurieren](./media/dropboxforbusiness-tutorial/configure6.png "Einmaliges Anmelden konfigurieren")

    c. Klicken Sie auf **Zertifikat hochladen**, und navigieren Sie dann zu Ihrer **Base64-codierten Zertifikatsdatei**, die Sie aus dem Azure-Portal heruntergeladen haben.

    d. Klicken Sie auf **Link kopieren**, und fügen Sie den kopierten Wert in das Textfeld **Anmelde-URL** im Abschnitt **Domäne und URLs für Dropbox für Unternehmen** im Azure-Portal ein.

    e. Klicken Sie auf **Speichern**.

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

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Dropbox für Unternehmen gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen**, **Alle Anwendungen** und dann **Dropbox für Unternehmen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Geben Sie in der Anwendungsliste **Dropbox für Unternehmen** ein und wählen Sie es aus.

    ![Der Link „Dropbox für Unternehmen“ in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-dropbox-for-business-test-user"></a>Erstellen eines Dropbox für Unternehmen-Testbenutzers

In diesem Abschnitt wird ein Benutzer mit dem Namen Britta Simon in Dropbox für Unternehmen erstellt. Dropbox für Unternehmen unterstützt die Just-in-Time-Benutzerbereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist ein Benutzer noch nicht in Dropbox für Unternehmen vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt.

>[!Note]
>Wenn Sie einen Benutzer manuell erstellen müssen, wenden Sie sich an das [Kundensupportteam von Dropbox für Unternehmen](https://www.dropbox.com/business/contact).

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel Dropbox für Unternehmen klicken, sollten Sie automatisch bei Ihrer Dropbox für Unternehmen-Anwendung angemeldet werden, für die Sie das einmalige Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

