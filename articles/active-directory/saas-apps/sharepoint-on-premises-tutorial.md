---
title: 'Tutorial: Azure Active Directory-Integration in lokales SharePoint | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und die lokale SharePoint-Instanz konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 85b8d4d0-3f6a-4913-b9d3-8cc327d8280d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: jeedes
ms.openlocfilehash: a792db670602f736489ee962df5078531e0a8e88
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050949"
---
# <a name="tutorial-azure-active-directory-integration-with-sharepoint-on-premises"></a>Tutorial: Azure Active Directory-Integration in lokales SharePoint

In diesem Tutorial erfahren Sie, wie Sie die lokale SharePoint-Instanz in Azure Active Directory (Azure AD) integrieren.

Die Integration einer lokalen SharePoint-Instanz in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf die lokale SharePoint-Instanz hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei der lokalen SharePoint-Instanz anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in die lokale SharePoint-Instanz konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein lokales SharePoint-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen der lokalen SharePoint-Instanz aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-sharepoint-on-premises-from-the-gallery"></a>Hinzufügen der lokalen SharePoint-Instanz aus dem Katalog
Zum Konfigurieren der Integration der lokalen SharePoint-Instanz in Azure AD müssen Sie die lokale Instanz aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**So fügen Sie die lokale SharePoint-Instanz aus dem Katalog hinzu**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **lokales SharePoint** ein, wählen Sie im Ergebnisbereich **lokales SharePoint** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Lokales SharePoint in der Ergebnisliste](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei einer lokalen SharePoint-Instanz anhand einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer im lokalen SharePoint als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer im lokalen SharePoint muss eine Linkbeziehung eingerichtet werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit einer lokalen SharePoint-Instanz zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erteilen des Zugriffs für lokalen SharePoint-Testbenutzer](#grant-access-to-sharePoint-on-premises-test-user)**, um ein Pendant zu Britta Simon in der lokalen SharePoint-Instanz zu erstellen, das mit der Azure AD-Repräsentation des Benutzers verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer lokalen SharePoint-Anwendung.

**So konfigurieren Sie das einmalige Anmelden in Azure AD im lokalen SharePoint**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für die **lokale SharePoint-Instanz** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Dialogfeld „Einmaliges Anmelden“](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für lokale SharePoint-Instanz** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für die lokale SharePoint-Instanz](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_url1.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<YourSharePointServerURL>/_trust/default.aspx`.

    b. Geben Sie im Textfeld **Bezeichner** die folgende URL ein: `urn:sharepoint:federation`.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_certificate.png)

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media\sharepoint-on-premises-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Lokale SharePoint-Konfiguration** auf **Lokale SharePoint-Instanz konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Dienst-URL für einmalige Anmeldung** aus dem Abschnitt **Kurzübersicht**.

    ![Lokale SharePoint-Konfiguration](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_configure.png)

    > [!NOTE]
    > Die lokale SharePoint-Anwendung verwendet SAML 1.1-Token, sodass Azure AD eine WS-Fed-Anforderung vom SharePoint-Server erwartet und nach der Authentifizierung SAML 1.1-Token ausstellt.

7. Melden Sie sich in einem anderen Webbrowserfenster auf der Unternehmenswebsite der lokalen SharePoint-Instanz als Administrator an.

8. **Konfigurieren Sie einen neuen vertrauenswürdigen Identitätsanbieter in SharePoint Server 2016.**

    Melden Sie sich in SharePoint Server 2016 an, und öffnen Sie die SharePoint 2016-Verwaltungsshell. Geben Sie die Werte für $realm $wsfedurl und $filepath aus dem Azure-Portal ein, und führen Sie die folgenden Befehle aus, um einen neuen vertrauenswürdigen Identitätsanbieter zu konfigurieren.

    > [!TIP]
    > Unter [SharePoint PowerShell](https://docs.microsoft.com/en-us/powershell/sharepoint/overview?view=sharepoint-ps) finden Sie eine Übersicht über PowerShell und die Funktionen. 

    ```
    $realm = "<Identifier value from the SharePoint on-premises Domain and URLs section in the Azure portal>"
    $wsfedurl="<SAML single sign-on service URL value which you have copied from the Azure portal>"
    $filepath="<Full path to SAML signing certificate file which you have copied from the Azure portal>"
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($filepath)
    New-SPTrustedRootAuthority -Name "AzureAD" -Certificate $cert
    $map = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" -IncomingClaimTypeDisplayName "name" -LocalClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
    $map2 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname" -IncomingClaimTypeDisplayName "GivenName" -SameAsIncoming
    $map3 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" -IncomingClaimTypeDisplayName "SurName" -SameAsIncoming
    $map4 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress" -IncomingClaimTypeDisplayName "Email" -SameAsIncoming
    $ap = New-SPTrustedIdentityTokenIssuer -Name "AzureAD" -Description "SharePoint secured by Azure AD" -realm $realm -ImportTrustCertificate $cert -ClaimsMappings $map,$map2,$map3 -SignInUrl $wsfedurl -IdentifierClaim "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"
    ```

    Führen Sie als Nächstes diese Schritte aus, um den vertrauenswürdigen Identitätsanbieter für Ihre Anwendung zu aktivieren:

    a. Navigieren Sie in der Zentraladministration zu **Webanwendung verwalten**, und wählen Sie die Webanwendung aus, die Sie mit Azure AD schützen möchten.

    b. Klicken Sie im Menüband auf **Authentifizierungsanbieter**, und wählen Sie die Zone aus, die Sie verwenden möchten.

    c. Wählen Sie **Vertrauenswürdiger Identitätsanbieter** und den Identitätsanbieter aus, den Sie mit dem Namen *AzureAD* registriert haben.

    d. Wählen Sie in der Anmeldeseite-URL-Einstellung **Benutzerdefinierte Anmeldeseite** aus, und geben Sie den Wert „/_trust/“ an.

    e. Klicken Sie auf **OK**.

    ![Konfigurieren des Authentifizierungsanbieters](./media\sharepoint-on-premises-tutorial/fig10-configauthprovider.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media\sharepoint-on-premises-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media\sharepoint-on-premises-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media\sharepoint-on-premises-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media\sharepoint-on-premises-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="grant-access-to-sharepoint-on-premises-test-user"></a>Erteilen des Zugriffs für lokalen SharePoint-Testbenutzer

Die Benutzer, die sich bei Azure AD anmelden und auf SharePoint zugreifen, müssen Zugriff auf die Anwendung erhalten. Führen Sie die folgenden Schritte aus, um die Berechtigungen für den Zugriff auf die Webanwendung festzulegen.

1. Klicken Sie in der Zentraladministration auf **Anwendungsverwaltung**.

2. Klicken Sie auf der Seite **Anwendungsverwaltung** im Abschnitt **Webanwendungen** auf **Webanwendungen verwalten**.

3. Klicken Sie auf die entsprechende Webanwendung und dann auf **Benutzerrichtlinie**.

4. Klicken Sie in der Richtlinie für die Webanwendung auf **Benutzer hinzufügen**.

    ![Suchen eines Benutzer anhand seines Namensanspruchs](./media\sharepoint-on-premises-tutorial/fig11-searchbynameclaim.png)

5. Klicken Sie im Dialogfeld **Benutzer hinzufügen** unter **Zonen** auf die entsprechende Zone und dann auf **Weiter**.

6. Klicken Sie im Dialogfeld **Richtlinie für Webanwendung** im Abschnitt **Benutzer auswählen** auf das Symbol **Durchsuchen**.

7. Geben Sie im Textfeld **Suchen** den Wert für den **Benutzerprinzipalnamen** ein, für den Sie die lokale SharePoint-Anwendung in Azure AD konfiguriert haben, und klicken Sie auf **Suche**. </br>Beispiel: *brittasimon@contoso.com*.

8. Wählen Sie unter der Azure AD-Überschrift in der Listenansicht die Eigenschaft „Name“ aus. Klicken Sie dann auf **Hinzufügen** und auf **OK**, um das Dialogfeld zu schließen.

9. Klicken Sie in den Berechtigungen auf **Vollzugriff**.

    ![Gewähren des Vollzugriffs für Benutzer mit Ansprüchen](./media\sharepoint-on-premises-tutorial/fig12-grantfullcontrol.png)

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

![Suchergebnisse für Ansprüche](./media\sharepoint-on-premises-tutorial/fig16-claimssearchresults.png)

Die gesuchten Werte werden nicht überprüft, sodass möglicherweise Rechtschreibfehler auftreten, oder Benutzer versehentlich den falschen Anspruchstyp zum Zuweisen auswählen, z.B. den Anspruch **SurName**. Folglich können Benutzer nicht auf Ressourcen zugreifen.

Dieses Problem lässt sich mit der Open-Source-Lösung [AzureCP](https://yvand.github.io/AzureCP/) beheben, die einen benutzerdefinierten Anspruchsanbieter für SharePoint 2016 bereitstellt. Azure AD Graph klärt und überprüft die Eingabe der Benutzer. Weitere Informationen finden Sie unter [ AzureCP](https://yvand.github.io/AzureCP/).

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt gewähren Sie Britta Simon Zugriff auf die lokale SharePoint-Instanz, damit sie das einmalige Anmelden von Azure verwenden kann.

![Zuweisen der Benutzerrolle][200]

**So weisen Sie Britta Simon einer lokalen SharePoint-Instanz zu**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **lokales SharePoint** aus.

    ![SharePoint-Link in der Anwendungsliste](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_app.png)

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Lokales SharePoint“ klicken, werden Sie automatisch bei Ihrer lokalen SharePoint-Anwendung angemeldet.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Verwenden von Azure AD für die SharePoint Server-Authentifizierung](https://docs.microsoft.com/en-us/office365/enterprise/using-azure-ad-for-sharepoint-server-authentication)

<!--Image references-->

[1]: ./media\sharepoint-on-premises-tutorial/tutorial_general_01.png
[2]: ./media\sharepoint-on-premises-tutorial/tutorial_general_02.png
[3]: ./media\sharepoint-on-premises-tutorial/tutorial_general_03.png
[4]: ./media\sharepoint-on-premises-tutorial/tutorial_general_04.png

[100]: ./media\sharepoint-on-premises-tutorial/tutorial_general_100.png

[200]: ./media\sharepoint-on-premises-tutorial/tutorial_general_200.png
[201]: ./media\sharepoint-on-premises-tutorial/tutorial_general_201.png
[202]: ./media\sharepoint-on-premises-tutorial/tutorial_general_202.png
[203]: ./media\sharepoint-on-premises-tutorial/tutorial_general_203.png

