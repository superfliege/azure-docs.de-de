---
title: 'Tutorial: Azure Active Directory-Integration mit SAP NetWeaver | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SAP NetWeaver konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1b9e59e3-e7ae-4e74-b16c-8c1a7ccfdef3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2018
ms.author: jeedes
ms.openlocfilehash: fac22508e679c1e1c93ec62a5b120ba9c7c52317
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52162349"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-netweaver"></a>Tutorial: Azure Active Directory-Integration mit SAP NetWeaver

In diesem Tutorial erfahren Sie, wie Sie SAP NetWeaver in Azure Active Directory (Azure AD) integrieren.

Die Integration von SAP NetWeaver in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf SAP NetWeaver hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei SAP NetWeaver anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit SAP NetWeaver konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SAP NetWeaver-Abonnement, für das einmaliges Anmelden aktiviert ist
- Mindestens SAP NetWeaver V7.20

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.
Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von SAP NetWeaver aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-sap-netweaver-from-the-gallery"></a>Hinzufügen von SAP NetWeaver aus dem Katalog

Zum Konfigurieren der Integration von SAP NetWeaver in Azure AD müssen Sie SAP NetWeaver aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um SAP NetWeaver aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **SAP NetWeaver** ein, wählen Sie im Ergebnisbereich **SAP NetWeaver** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![SAP NetWeaver in der Ergebnisliste](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei SAP NetWeaver mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in SAP NetWeaver als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SAP NetWeaver muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei SAP NetWeaver müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. **[Erstellen eines SAP NetWeaver-Testbenutzers](#creating-sapnetweaver-test-user)**, um eine Entsprechung von Britta Simon in SAP NetWeaver zu erstellen, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren es in Ihrer SAP NetWeaver-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in SAP NetWeaver die folgenden Schritte aus:**

1. Melden Sie sich in einem neuen Webbrowserfenster bei der SAP NetWeaver-Unternehmenswebsite als Administrator an.

2. Stellen Sie sicher, dass **http**- und **https**-Dienste aktiv sind und die entsprechenden Ports im T-Code **SMICM** zugewiesen wurden.

3. Melden Sie sich beim Unternehmensclient des SAP-Systems (T01) an, bei dem das einmalige Anmelden erforderlich ist, und aktivieren Sie die Verwaltung der HTTP-Sicherheitssitzung.

    a. Navigieren Sie zum Transaktionscode **SICF_SESSIONS**. Hier werden alle relevanten Profilparameter mit ihren aktuellen Werten angezeigt. Sie sehen wie folgt aus:
    ```
    login/create_sso2_ticket = 2
    login/accept_sso2_ticket = 1
    login/ticketcache_entries_max = 1000
    login/ticketcache_off = 0  login/ticket_only_by_https = 0 
    icf/set_HTTPonly_flag_on_cookies = 3
    icf/user_recheck = 0  http/security_session_timeout = 1800
    http/security_context_cache_size = 2500
    rdisp/plugin_auto_logout = 1800
    rdisp/autothtime = 60
    ```
    >[!NOTE]
    > Passen Sie die obigen Parameter an die Anforderungen Ihrer Organisation an. Die gezeigten Parameter dienen lediglich als Beispiele.

    b. Passen Sie ggf. die Parameter in der Instanz/dem Standardprofil des SAP-Systems an, und starten Sie das SAP-System neu.

    c. Doppelklicken Sie auf den entsprechenden Client, um die HTTP-Sicherheitssitzung zu aktivieren.

    ![Downloadlink für das Zertifikat](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_profileparameter.png)

    d. Aktivieren Sie die folgenden SICF-Dienste:
    ```
    /sap/public/bc/sec/saml2
    /sap/public/bc/sec/cdc_ext_service
    /sap/bc/webdynpro/sap/saml2
    /sap/bc/webdynpro/sap/sec_diag_tool (This is only to enable / disable trace)
    ```
4. Wechseln Sie im Unternehmensclient des SAP-Systems [T01/122] zum Transaktionscode **SAML2**. Es wird eine Benutzeroberfläche in einem Browser geöffnet. In diesem Beispiel wird von 122 als SAP-Unternehmensclient ausgegangen.

    ![Downloadlink für das Zertifikat](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_sapbusinessclient.png)

5. Geben Sie Ihren Benutzernamen und Ihr Kennwort ein, um die Benutzeroberfläche zu starten, und klicken Sie auf **Edit** (Bearbeiten).

    ![Downloadlink für das Zertifikat](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_userpwd.png)

6. Ersetzen Sie den **Provider Name** (Anbietername) von T01122 durch **http://T01122**, und klicken Sie auf **Save** (Speichern).

    > [!NOTE]
    > In der Standardeinstellung hat der Anbietername das Format <sid><client>, Azure AD erwartet den Namen jedoch im Format <protocol>://<name>. Es wird empfohlen, den Anbieternamen als „https://<sid><client>“ anzugeben, damit mehrere SAP NetWeaver ABAP-Engines in Azure AD konfiguriert werden können.

    ![Downloadlink für das Zertifikat](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_providername.png)

7. **Generieren von Metadaten des Dienstanbieters:** Sobald Sie mit dem Konfigurieren der Einstellungen **Local Provider** (Lokaler Anbieter) und **Trusted Providers** (Vertrauenswürdige Anbieter) auf der SAML 2.0-Benutzeroberfläche fertig sind, generieren Sie im nächsten Schritt die Metadatendatei des Dienstanbieters (die alle Einstellungen, die Authentifizierungskontexte und andere Konfigurationen in SAP enthält). Nachdem diese Datei generiert wurde, müssen Sie sie in Azure AD hochladen.

    ![Downloadlink für das Zertifikat](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_generatesp.png)

    a. Wechseln Sie zu **Local Provider** (Lokaler Anbieter).

    b. Klicken Sie auf **Metadata** (Metadaten).

    c. Speichern Sie die generierte XML-Datei mit den **Metadaten** auf Ihrem Computer, und laden Sie die Datei im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** hoch, damit die Werte **Bezeichner** und **Antwort-URL** automatisch ausgefüllt werden.

8. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **SAP NetWeaver** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

9. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![Configure single sign-on](common/tutorial_general_301.png)

10. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Configure single sign-on](common/editconfigure.png)

11. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    a. Klicken Sie auf **Metadatendatei hochladen**, um die **Metadatendatei des Dienstanbieters**, die Sie zuvor abgerufen haben, hochzuladen.

    ![Metadatendatei hochladen](common/editmetadataupload.png)

    b. Klicken Sie auf das **Ordnerlogo**, wählen Sie die Metadatendatei aus, und klicken Sie auf **Hochladen**.

    ![Metadatendatei hochladen](common/uploadmetadata.png)

    c. Nach dem erfolgreichen Upload der Metadatendatei werden die Werte **Bezeichner** und **Antwort-URL** im Abschnitt **Grundlegende SAML-Konfiguration** in den entsprechenden Textfeldern automatisch ausgefüllt (wie unten dargestellt):

    ![SSO-Informationen zur Domäne und zu den URLs für SAP NetWeaver](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_url.png)

    d. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<your company instance of SAP NetWeaver>`.

12. Die SAP NetWeaver-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Benutzerattribute** zu öffnen.

    ![Attributabschnitt](./media/sapnetweaver-tutorial/edit_attribute.png)

13. Konfigurieren Sie im Dialogfeld **Benutzerattribute** im Abschnitt **Benutzeransprüche** das SAML-Tokenattribut wie in der obigen Abbildung gezeigt, und führen Sie die folgenden Schritte aus:

    a. Klicken Sie auf das Symbol **Bearbeiten**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.
    
    ![Attributabschnitt](./media/sapnetweaver-tutorial/nameidattribute.png)

    b. Führen Sie auf der Registerkarte **Benutzeransprüche verwalten** die folgenden Schritte aus:

    ![Attributabschnitt](./media/sapnetweaver-tutorial/nameidattribute1.png)

    * Wählen Sie **Transformation** aus.
  
    * Wählen Sie in der Liste **Transformation** die Option `ExtractMailPrefix()` aus.
  
    * Wählen Sie in der Liste **Parameter 1** die Option `user.userprincipalname` aus.

    * Klicken Sie auf **Speichern**.

14. Klicken Sie auf der Seite **SAML-Signaturzertifikat** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um die **Verbundmetadaten-XML-Datei** herunterzuladen. Speichern Sie die Metadatendatei anschließend auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_certificate.png)

15. Kopieren Sie im Abschnitt **SAP NetWeaver einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

    ![SAP NetWeaver-Konfiguration](common/configuresection.png)

16. Melden Sie sich beim SAP-System an, und navigieren Sie zum Transaktionscode „SAML2“. Damit öffnen Sie ein neues Browserfenster mit dem SAML-Konfigurationsbildschirm.

17. Wechseln Sie zum Konfigurieren von Endpunkten für vertrauenswürdige Identitätsanbieter Provider (Azure AD) zur Registerkarte **Trusted Providers** (Vertrauenswürdige Anbieter).

    ![Configure single sign-on](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_samlconfig.png)

18. Wählen Sie **Hinzufügen** und dann im Kontextmenü **Metadatendatei hochladen** aus.

    ![Configure single sign-on](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_uploadmetadata.png)

19. Laden Sie die Metadatendatei hoch, die Sie aus dem Azure-Portal heruntergeladen haben.

    ![Configure single sign-on](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_metadatafile.png)

20. Geben Sie im nächsten Bildschirm den Aliasnamen ein. Geben Sie z.B. „aadsts“ ein, und wählen Sie zum Fortfahren **Weiter** aus.

    ![Configure single sign-on](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_aliasname.png)

21. Stellen Sie sicher, dass Ihr **Digest Algorithm** (Digest-Algorithmus) **SHA-256** lautet und keine Änderungen erfordert. Wählen Sie dann **Weiter** aus.

    ![Configure single sign-on](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_identityprovider.png)

22. Verwenden Sie für **Single Sign-On Endpoints** (Endpunkte für einmaliges Anmelden) die Option **HTTP POST**, und klicken Sie zum Fortfahren auf **Weiter**.

    ![Configure single sign-on](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_httpredirect.png)

23. Verwenden Sie für **Single Logout Endpoints** (Endpunkte für einmaliges Abmelden) die Option **HTTPRedirect**, und klicken Sie zum Fortfahren auf **Weiter**.

    ![Configure single sign-on](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_httpredirect1.png)

24. Wählen Sie unter **Artifact Endpoints** (Artefaktendpunkte) zum Fortfahren **Weiter** aus.

    ![Configure single sign-on](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_artifactendpoint.png)

25. Klicken Sie unter **Authentication Requirements** (Authentifizierungsanforderungen) auf **Fertig stellen**.

    ![Configure single sign-on](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_authentication.png)

26. Wechseln Sie zur Registerkarte **Trusted Provider** > **Identity Federation** (Vertrauenswürdige Anbieter > Identitätsverbund) (unten auf dem Bildschirm). Klicken Sie auf **Edit**.

    ![Configure single sign-on](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_trustedprovider.png)

27. Klicken Sie auf der Registerkarte **Identity Federation** (Identitätsverbund) (im Fenster unten) auf **Add** (Hinzufügen).

    ![Configure single sign-on](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_addidentityprovider.png)

28. Wählen Sie im Popupfenster **Unspecified** (Nicht angegeben) für **Supported NameID formats** (Unterstützte Formate für die Namens-ID) aus, und klicken Sie auf „OK“.

    ![Configure single sign-on](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_nameid.png)

29. Beachten Sie, dass die Werte **user ID Source** (Benutzer-ID-Quelle) und **user id mapping mode** (Zuordnungsmodus für Benutzer-ID) die Verbindung zwischen SAP-Benutzern und Azure AD-Ansprüchen festlegen.  

    ####<a name="scenario-sap-user-to-azure-ad-user-mapping"></a>Szenario: Zuordnung zwischen SAP-Benutzer und Azure AD-Benutzer.

    a. Bildschirm „NameID details“ (Details zur Namens-ID) von SAP.

    ![Configure single sign-on](./media/sapnetweaver-tutorial/nameiddetails.png)

    b. Screenshot der erforderlichen Ansprüche aus Azure AD.

    ![Configure single sign-on](./media/sapnetweaver-tutorial/claimsaad1.png)

    ####<a name="scenario-select-sap-user-id-based-on-configured-email-address-in-su01-in-this-case-email-id-should-be-configured-in-su01-for-each-user-who-requires-sso"></a>Szenario: Auswählen der SAP-Benutzer-ID basierend auf der konfigurierten E-Mail-Adresse in SU01. In diesem Fall sollte die E-Mail-ID in SU01 für jeden Benutzer konfiguriert werden, der SSO benötigt.

    a.  Bildschirm „NameID details“ (Details zur Namens-ID) von SAP.

    ![Configure single sign-on](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_nameiddetails1.png)

    b. Screenshot der erforderlichen Ansprüche aus Azure AD.

    ![Configure single sign-on](./media/sapnetweaver-tutorial/claimsaad2.png)

30. Klicken Sie auf **Save** (Speichern) und dann auf **Enable** (Aktivieren), um den Identitätsanbieter zu aktivieren.

    ![Configure single sign-on](./media/sapnetweaver-tutorial/configuration1.png)

31. Klicken Sie auf **OK**, wenn Sie dazu aufgefordert werden.

    ![Configure single sign-on](./media/sapnetweaver-tutorial/configuration2.png)

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Azure AD-Benutzer erstellen][100]

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Erstellen eines Azure AD-Testbenutzers](common/create_aaduser_01.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Erstellen eines Azure AD-Testbenutzers](common/create_aaduser_02.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** den Namen **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

### <a name="creating-sap-netweaver-test-user"></a>Erstellen eines SAP NetWeaver-Testbenutzers

In diesem Abschnitt erstellen Sie in SAP NetWeaver einen Benutzer namens Britta Simon. Arbeiten Sie mit Ihrem SAP-Expertenteam vor Ort oder mit dem SAP-Partner Ihres Unternehmens zusammen, um die Benutzer auf der SAP NetWeaver-Plattform hinzuzufügen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf SAP NetWeaver gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste den Eintrag **SAP NetWeaver**aus.

    ![Configure single sign-on](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

1. Nachdem der Identitätsanbieter Azure AD aktiviert wurde, versuchen Sie, auf die URL unten zuzugreifen, um das einmalige Anmelden zu testen (es wird kein Benutzername und kein Kennwort angefordert).

    `https://<sapurl>/sap/bc/bsp/sap/it00/default.htm`

    (Oder:) Verwenden Sie die unten aufgeführte URL.

    `https://<sapurl>/sap/bc/bsp/sap/it00/default.htm`

    > [!NOTE]
    > Ersetzen Sie „sapurl“ durch den tatsächlichen SAP-Hostnamen.

2. Über die obige URL sollten Sie zum unten gezeigten Bildschirm gelangen. Wenn Sie die unten abgebildete Seite erreichen können, wurde die Einrichtung des einmaligen Anmeldens von Azure AD erfolgreich abgeschlossen.

    ![Configure single sign-on](./media/sapnetweaver-tutorial/testingsso.png)

3. Wenn Sie zur Eingabe von Benutzername und Kennwort aufgefordert werden, führen Sie eine Diagnose des Problems durch. Aktivieren Sie dazu die Ablaufverfolgung mithilfe der folgenden URL.

    `https://<sapurl>/sap/bc/webdynpro/sap/sec_diag_tool?sap-client=122&sap-language=EN#`

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
