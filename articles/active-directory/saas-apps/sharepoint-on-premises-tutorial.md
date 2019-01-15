---
title: 'Tutorial: Azure Active Directory-Integration in lokales SharePoint | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und die lokale SharePoint-Instanz konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 85b8d4d0-3f6a-4913-b9d3-8cc327d8280d
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.openlocfilehash: 789f58699f39f4b7eac453f4cf79ea55a5bfc8d3
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2019
ms.locfileid: "54159501"
---
# <a name="tutorial-azure-active-directory-integration-with-sharepoint-on-premises"></a>Tutorial: Azure Active Directory-Integration in lokales SharePoint

In diesem Tutorial erfahren Sie, wie Sie die lokale SharePoint-Instanz in Azure Active Directory (Azure AD) integrieren.
Die Integration einer lokalen SharePoint-Instanz in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf die lokale SharePoint-Instanz hat.
* Sie können Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei SharePoint (lokal) anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in die lokale SharePoint-Instanz konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Wenn Sie keine Azure AD-Umgebung besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.
* Ein Abonnement für SharePoint (lokal), für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* SharePoint (lokal) unterstützt das **SP-initiierte** einmalige Anmelden.

## <a name="adding-sharepoint-on-premises-from-the-gallery"></a>Hinzufügen der lokalen SharePoint-Instanz aus dem Katalog

Zum Konfigurieren der Integration der lokalen SharePoint-Instanz in Azure AD müssen Sie die lokale Instanz aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**So fügen Sie die lokale SharePoint-Instanz aus dem Katalog hinzu**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **lokales SharePoint** ein, wählen Sie im Ergebnisbereich **lokales SharePoint** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![Lokales SharePoint in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei einer lokalen SharePoint-Instanz anhand einer Testbenutzerin namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SharePoint (lokal) eingerichtet werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit einer lokalen SharePoint-Instanz zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für SharePoint (lokal)](#configure-sharepoint-on-premises-single-sign-on)**, um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erteilen des Zugriffs für lokalen SharePoint-Testbenutzer](#grant-access-to-sharepoint-on-premises-test-user)**, um ein Pendant zu Britta Simon in der lokalen SharePoint-Instanz zu erstellen, das mit der Azure AD-Repräsentation des Benutzers verknüpft ist.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit SharePoint (lokal) die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **SharePoint (lokal)** die Option **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für die lokale SharePoint-Instanz](common/sp-identifier-reply.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<YourSharePointServerURL>/_trust/default.aspx`

    b. Geben Sie im Feld **Bezeichner** eine URL im folgenden Format ein: `urn:sharepoint:federation`.

    c. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<YourSharePointServerURL>/_trust/default.aspx`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL, den tatsächlichen Bezeichner und die tatsächliche Antwort-URL. Wenden Sie sich an das [Supportteam der lokalen SharePoint-Instanz](https://support.office.com/), um diese Werte zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Zertifikat (Base64)** aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

    > [!Note]
    > Notieren Sie sich den Dateipfad, in dem Sie die Zertifikatdatei heruntergeladen haben. Sie benötigen die Datei später im PowerShell-Skript für die Konfiguration.

6. Kopieren Sie im Abschnitt **SharePoint (lokal) einrichten** die entsprechenden URLs gemäß Ihren Anforderungen. Verwenden Sie für **Dienst-URL für einmalige Anmeldung** einen Wert im folgenden Format: `https://login.microsoftonline.com/_my_directory_id_/wsfed` 

    > [!Note]
    > _my_directory_id_ ist die Mandanten-ID des Azure AD-Abonnements.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

    > [!NOTE]
    > Die lokale SharePoint-Anwendung verwendet SAML 1.1-Token, sodass Azure AD eine WS-Fed-Anforderung vom SharePoint-Server erwartet und nach der Authentifizierung das SAML 1.1-Token ausstellt.

### <a name="configure-sharepoint-on-premises-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für SharePoint (lokal)

1. Melden Sie sich in einem anderen Webbrowserfenster auf der Unternehmenswebsite der lokalen SharePoint-Instanz als Administrator an.

2. **Konfigurieren Sie einen neuen vertrauenswürdigen Identitätsanbieter in SharePoint Server 2016.**

    Melden Sie sich in SharePoint Server 2016 an, und öffnen Sie die SharePoint 2016-Verwaltungsshell. Geben Sie die Werte für „$realm“ (Bezeichnerwert aus dem Abschnitt „Domäne und URLs für lokale SharePoint-Instanz“ im Azure-Portal), „$wsfedurl“ (Dienst-URL für einmaliges Anmelden) und „$filepath“ (Dateipfad, in den Sie die Zertifikatdatei heruntergeladen haben) aus dem Azure-Portal ein, und führen Sie die folgenden Befehle aus, um einen neuen vertrauenswürdigen Identitätsanbieter zu konfigurieren.

    > [!TIP]
    > Unter [SharePoint PowerShell](https://docs.microsoft.com/powershell/sharepoint/overview?view=sharepoint-ps) finden Sie eine Übersicht über PowerShell und die Funktionen. 

    ```
    $realm = "<Identifier value from the SharePoint on-premises Domain and URLs section in the Azure portal>"
    $wsfedurl="<SAML single sign-on service URL value which you have copied from the Azure portal>"
    $filepath="<Full path to SAML signing certificate file which you have downloaded from the Azure portal>"
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($filepath)
    New-SPTrustedRootAuthority -Name "AzureAD" -Certificate $cert
    $map = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" -IncomingClaimTypeDisplayName "name" -LocalClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
    $map2 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname" -IncomingClaimTypeDisplayName "GivenName" -SameAsIncoming
    $map3 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" -IncomingClaimTypeDisplayName "SurName" -SameAsIncoming
    $map4 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress" -IncomingClaimTypeDisplayName "Email" -SameAsIncoming
    $ap = New-SPTrustedIdentityTokenIssuer -Name "AzureAD" -Description "SharePoint secured by Azure AD" -realm $realm -ImportTrustCertificate $cert -ClaimsMappings $map,$map2,$map3,$map4 -SignInUrl $wsfedurl -IdentifierClaim "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"
    ```

    Führen Sie als Nächstes diese Schritte aus, um den vertrauenswürdigen Identitätsanbieter für Ihre Anwendung zu aktivieren:

    a. Navigieren Sie in der Zentraladministration zu **Webanwendung verwalten**, und wählen Sie die Webanwendung aus, die Sie mit Azure AD schützen möchten.

    b. Klicken Sie im Menüband auf **Authentifizierungsanbieter**, und wählen Sie die Zone aus, die Sie verwenden möchten.

    c. Wählen Sie **Vertrauenswürdiger Identitätsanbieter** und den Identitätsanbieter aus, den Sie mit dem Namen *AzureAD* registriert haben.

    d. Wählen Sie in der Anmeldeseite-URL-Einstellung **Benutzerdefinierte Anmeldeseite** aus, und geben Sie den Wert „/_trust/“ an.

    e. Klicken Sie auf **OK**.

    ![Konfigurieren des Authentifizierungsanbieters](./media/sharepoint-on-premises-tutorial/fig10-configauthprovider.png)

    > [!NOTE]
    > Einige externe Benutzer können diese Integration des einmaligen Anmeldens nicht nutzen, da ihr UPN einen beschädigten Wert wie `MYEMAIL_outlook.com#ext#@TENANT.onmicrosoft.com` enthält. Die Möglichkeit zur benutzertypabhängigen Behandlung des UPN durch eine benutzerdefinierte App-Konfiguration wird in Kürze verfügbar gemacht. Anschließend sollten alle Gastbenutzer SSO genauso problemlos wie Mitarbeiter der Organisation verwenden können.

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

### <a name="grant-access-to-sharepoint-on-premises-test-user"></a>Erteilen des Zugriffs für lokalen SharePoint-Testbenutzer

Die Benutzer, die sich bei Azure AD anmelden und auf SharePoint zugreifen, müssen Zugriff auf die Anwendung erhalten. Führen Sie die folgenden Schritte aus, um die Berechtigungen für den Zugriff auf die Webanwendung festzulegen.

1. Klicken Sie in der Zentraladministration auf **Anwendungsverwaltung**.

2. Klicken Sie auf der Seite **Anwendungsverwaltung** im Abschnitt **Webanwendungen** auf **Webanwendungen verwalten**.

3. Klicken Sie auf die entsprechende Webanwendung und dann auf **Benutzerrichtlinie**.

4. Klicken Sie in der Richtlinie für die Webanwendung auf **Benutzer hinzufügen**.

    ![Suchen eines Benutzer anhand seines Namensanspruchs](./media/sharepoint-on-premises-tutorial/fig11-searchbynameclaim.png)

5. Klicken Sie im Dialogfeld **Benutzer hinzufügen** unter **Zonen** auf die entsprechende Zone und dann auf **Weiter**.

6. Klicken Sie im Dialogfeld **Richtlinie für Webanwendung** im Abschnitt **Benutzer auswählen** auf das Symbol **Durchsuchen**.

7. Geben Sie im Textfeld **Suchen** den Wert für den **Benutzerprinzipalnamen** ein, für den Sie die lokale SharePoint-Anwendung in Azure AD konfiguriert haben, und klicken Sie auf **Suche**. </br>Beispiel: *brittasimon@contoso.com*.

8. Wählen Sie unter der Azure AD-Überschrift in der Listenansicht die Eigenschaft „Name“ aus. Klicken Sie dann auf **Hinzufügen** und auf **OK**, um das Dialogfeld zu schließen.

9. Klicken Sie in den Berechtigungen auf **Vollzugriff**.

    ![Gewähren des Vollzugriffs für Benutzer mit Ansprüchen](./media/sharepoint-on-premises-tutorial/fig12-grantfullcontrol.png)

10. Klicken Sie auf **Fertig stellen** und dann auf **OK**.

### <a name="configuring-one-trusted-identity-provider-for-multiple-web-applications"></a>Konfigurieren eines vertrauenswürdigen Identitätsanbieters für mehrere Webanwendungen

Die Konfiguration funktioniert für eine einzelne Webanwendung, benötigt jedoch weitere Konfiguration, wenn Sie beabsichtigen, denselben vertrauenswürdigen Identitätsanbieter für mehrere Webanwendungen zu verwenden. Angenommen, eine Webanwendung wäre erweitert worden, um die URL `https://portal.contoso.local` zu verwenden. Nun möchten Sie die Benutzer für `https://sales.contoso.local` ebenfalls authentifizieren. Dazu müssen Sie den Identitätsanbieter aktualisieren, sodass dieser den WReply-Parameter berücksichtigt. Außerdem müssen Sie die Anwendungsregistrierung in Azure AD aktualisieren, um eine Antwort-URL hinzuzufügen.

1. Öffnen Sie im Azure-Portal das Azure AD-Verzeichnis. Klicken Sie auf **App-Registrierungen** und dann auf **Alle Anwendungen anzeigen**. Klicken Sie auf die Anwendung, die Sie zuvor erstellt haben (SharePoint-SAML-Integration).

2. Klicken Sie auf **Einstellungen**.

3. Klicken Sie auf dem Einstellungsblatt auf **Antwort-URLs**. 

4. Fügen Sie die URL für die zusätzliche Webanwendung hinzu, die auf `/_trust/default.aspx` endet (z.B. `https://sales.contoso.local/_trust/default.aspx`), und klicken Sie auf **Speichern**.

5. Öffnen Sie auf dem SharePoint-Server die **SharePoint 2016-Verwaltungsshell**, und führen Sie die folgenden Befehle mithilfe des Namens des Tokenausstellers des vertrauenswürdige Identitätsanbieter aus, den Sie zuvor verwendet haben.

    ```
    $t = Get-SPTrustedIdentityTokenIssuer "AzureAD"
    $t.UseWReplyParameter=$true
    $t.Update()
    ```
6. Navigieren Sie in der Zentraladministration zu der Web-Anwendung, und aktivieren Sie den vorhandenen vertrauenswürdigen Identitätsanbieter. Denken Sie daran, auch die URL der Anmeldeseite als eine benutzerdefinierte Anmeldeseite `/_trust/` zu konfigurieren.

7. Klicken Sie in der Zentraladministration auf die Webanwendung, und wählen Sie **User Policy** (Benutzerrichtlinie) aus. Fügen Sie einen Benutzer, wie zuvor in diesem Artikel gezeigt, mit den entsprechenden Berechtigungen hinzu.

### <a name="fixing-people-picker"></a>Verbesserte Personenauswahl

Benutzer können sich jetzt mit Azure AD-Identitäten bei SharePoint 2016 anmelden. Allerdings kann die Benutzerfreundlichkeit noch weiter verbessert werden. Beispielsweise liefert die Suche nach einem Benutzer mehrere Suchergebnisse in der Personenauswahl. Es gibt ein Suchergebnis für jeden der drei Anspruchstypen, der in der Anspruchszuordnung erstellt wurde. Um einen Benutzer mit der Personenauswahl auszuwählen, geben Sie seinen genauen Benutzernamen ein, und wählen Sie das Anspruchsergebnis **Name** aus.

![Suchergebnisse für Ansprüche](./media/sharepoint-on-premises-tutorial/fig16-claimssearchresults.png)

Die gesuchten Werte werden nicht überprüft, sodass möglicherweise Rechtschreibfehler auftreten, oder Benutzer versehentlich den falschen Anspruchstyp zum Zuweisen auswählen, z.B. den Anspruch **SurName**. Folglich können Benutzer nicht auf Ressourcen zugreifen.

Dieses Problem lässt sich mit der Open-Source-Lösung [AzureCP](https://yvand.github.io/AzureCP/) beheben, die einen benutzerdefinierten Anspruchsanbieter für SharePoint 2016 bereitstellt. Azure AD Graph klärt und überprüft die Eingabe der Benutzer. Weitere Informationen finden Sie unter [ AzureCP](https://yvand.github.io/AzureCP/).

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt gewähren Sie Britta Simon Zugriff auf die lokale SharePoint-Instanz, damit sie das einmalige Anmelden von Azure verwenden kann.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen**, **Alle Anwendungen** und dann **SharePoint (lokal)** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Geben Sie in der Anwendungsliste **SharePoint (lokal)** ein, und wählen Sie den Eintrag aus.

    ![Link für SharePoint (lokal) in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-sharepoint-on-premises-test-user"></a>Erstellen eines Testbenutzers für SharePoint (lokal)

In diesem Abschnitt erstellen Sie in SharePoint (lokal) einen Benutzer namens Britta Simon. Lassen Sie sich beim Hinzufügen der Benutzer auf der Plattform für SharePoint (lokal) ggf. vom  [Supportteam für SharePoint (lokal)](https://support.office.com/) unterstützen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „SharePoint (lokal)“ klicken, sollten Sie automatisch bei der Anwendung für SharePoint (lokal) angemeldet werden, für die Sie SSO eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
