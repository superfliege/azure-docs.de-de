---
title: 'Tutorial: Azure Active Directory-Integration mit Confluence SAML SSO by Microsoft | Microsoft-Dokumentation'
description: In diesem Artikel erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Confluence SAML SSO by Microsoft konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1ad1cf90-52bc-4b71-ab2b-9a5a1280fb2d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: jeedes
ms.openlocfilehash: 22189c3d2d2164ba0fa3c2d790c36361fb0f5854
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="tutorial-azure-active-directory-integration-with-confluence-saml-sso-by-microsoft"></a>Tutorial: Azure Active Directory-Integration mit Confluence SAML SSO by Microsoft

In diesem Tutorial erfahren Sie, wie Sie Confluence SAML SSO by Microsoft in Azure Active Directory (Azure AD) integrieren.

Die Integration von Confluence SAML SSO by Microsoft in Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Confluence SAML SSO by Microsoft hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Confluence SAML SSO by Microsoft anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="description"></a>Beschreibung:

Verwenden Sie Ihr Microsoft Azure Active Directory-Konto mit einem Atlassian Confluence-Server, um einmaliges Anmelden zu ermöglichen. Auf diese Weise können sich alle Benutzer in Ihrer Organisation mit den Azure AD-Anmeldeinformationen bei der Confluence-Anwendung anmelden. Dieses Plug-In nutzt SAML 2.0 für den Verbund.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Confluence SAML SSO by Microsoft konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Eine Confluence-Serveranwendung, die auf einem 64-Bit-Server unter Windows installiert ist (lokal oder in der Cloud-IaaS-Infrastruktur)
- Einen HTTPS-fähigen Confluence-Server
- Beachten Sie, dass die unterstützten Versionen für das Confluence-Plug-In weiter unten aufgeführt werden.
- Der Confluence-Server sollte über das Internet erreichbar sein, insbesondere zur Authentifizierung über die Azure AD-Anmeldeseite. Außerdem sollte er in der Lage sein, das Token von Azure AD zu erhalten.
- In Confluence eingerichtete Administratoranmeldeinformationen
- WebSudo in Confluence deaktiviert
- Einen in der Confluence-Serveranwendung erstellten Testbenutzer

> [!NOTE]
> Zum Testen der Schritte in diesem Tutorial wird empfohlen, keine Produktionsumgebung von Confluence zu verwenden. Testen Sie die Integration zuerst in der Entwicklungs- oder Stagingumgebung der Anwendung, und verwenden Sie erst danach die Produktionsumgebung.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie hier eine einmonatige Testversion anfordern: [Testversion](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-confluence"></a>Unterstützte Confluence-Versionen 

Zum aktuellen Zeitpunkt werden die folgenden Confluence-Versionen unterstützt:

- Confluence: 5.0 bis 5.10

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Confluence SAML SSO by Microsoft aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-confluence-saml-sso-by-microsoft-from-the-gallery"></a>Hinzufügen von Confluence SAML SSO by Microsoft aus dem Katalog
Zum Konfigurieren der Integration von Confluence SAML SSO by Microsoft in Azure AD müssen Sie Confluence SAML SSO by Microsoft aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Confluence SAML SSO by Microsoft aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

4. Geben Sie in das Suchfeld den Suchbegriff **Confluence SAML SSO by Microsoft** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_search.png)

5. Wählen Sie im Ergebnisbereich die Option **Confluence SAML SSO by Microsoft** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Confluence SAML SSO by Microsoft mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Confluence SAML SSO by Microsoft als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Confluence SAML SSO by Microsoft muss eine Linkbeziehung hergestellt werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens mit Azure AD bei Confluence SAML SSO by Microsoft müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. **[Erstellen eines Confluence SAML SSO by Microsoft-Testbenutzers](#creating-a-confluence-saml-sso-by-microsoft-test-user)**, um in Confluence SAML SSO by Microsoft einen Gegenpart von Britta Simon zu erhalten, der mit ihrer Benutzerdarstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Confluence SAML SSO by Microsoft-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens mit Azure AD bei Confluence SAML SSO by Microsoft die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Confluence SAML SSO by Microsoft** auf **Einmaliges Anmelden**.

    ![Configure Single Sign-On][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Configure Single Sign-On](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Confluence SAML SSO by Microsoft** die folgenden Schritte aus:

    ![Configure Single Sign-On](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<domain:port>/plugins/servlet/saml/auth`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<domain:port>/`

    c. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner, die Antwort-URL und die Anmelde-URL. Der Port ist optional, sofern es sich um eine benannte URL handelt. Diese Werte werden während der Konfiguration des Confluence-Plug-Ins empfangen, die später im Tutorial beschrieben wird.
 

4. Zum Generieren der **Metadaten**-URL führen Sie die folgenden Schritte aus:

    a. Klicken Sie auf **App-Registrierungen**.
    
    ![Configure Single Sign-On](./media/active-directory-saas-Confluencemicrosoft-tutorial/appregistrations.png)
   
    b. Klicken Sie auf **Endpunkte**, um das Dialogfeld **Endpunkte** zu öffnen.  
    
    ![Configure Single Sign-On](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpointicon.png)

    c. Klicken Sie auf die Schaltfläche „Kopieren“, um die **VERBUNDMETADATENDOKUMENT**-URL zu kopieren und in Editor einzufügen.
    
    ![Configure Single Sign-On](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpoint.png)
     
    d. Kehren Sie nun zur Eigenschaftenseite von **Confluence SAML SSO by Microsoft** zurück. Kopieren Sie die **Anwendungs-ID** mithilfe der Schaltfläche **Kopieren**, und fügen Sie sie im Editor ein.
 
    ![Configure Single Sign-On](./media/active-directory-saas-Confluencemicrosoft-tutorial/appid.png)

    e. Generieren Sie die **Metadaten-URL** nach dem folgenden Muster: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`. Kopieren Sie diesen Wert in den Editor, da Sie ihn später für die Konfiguration des Plug-Ins benötigen.

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure Single Sign-On](./media/active-directory-saas-Confluencemicrosoft-tutorial/tutorial_general_400.png)

6. Melden Sie sich in einem anderen Webbrowserfenster bei Ihrer Confluence-Instanz als Administrator an.

7. Fahren Sie mit dem Mauszeiger über das Zahnrad, und klicken Sie auf die **Add-Ons**.
    
    ![Configure Single Sign-On](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon1.png)

8. Laden Sie das Plug-In aus dem [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=56503) herunter. Laden Sie das von Microsoft bereitgestellte Plug-In manuell mithilfe des Menüs **Upload add-on** (Add-On hochladen) hoch.
    
    ![Configure Single Sign-On](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon12.png)

9. Sobald das Plug-In installiert ist, wird es im Abschnitt **Add-Ons verwalten** unter **User Installed** (Vom Benutzer installiert) angezeigt. Klicken Sie auf **Konfigurieren**, um das neue Plug-In zu konfigurieren.
    
    ![Configure Single Sign-On](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon13.png)

10. Führen Sie auf der Konfigurationsseite die folgenden Schritte aus:

    ![Configure Single Sign-On](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon52.png)
 
    > [!TIP]
    > Stellen Sie sicher, dass der App nur ein Zertifikat zugeordnet wird, um einen Fehler bei der Auflösung der Metadaten zu vermeiden. Bei mehreren Zertifikaten erhält der Administrator nach dem Auflösen der Metadaten eine Fehlermeldung.

    a. Fügen Sie unter **Metadaten-URL** die von Azure AD generierte **Metadaten-URL** ein, und klicken Sie auf die Schaltfläche **Auflösen**. Die IdP-Metadaten-URL wird gelesen, und alle Felder werden aufgefüllt.

    b. Kopieren Sie die Werte von **Bezeichner, Antwort-URL und Anmelde-URL**. Fügen Sie sie anschließend in die Textfelder **Bezeichner, Antwort-URL und Anmelde-URL** bzw. im Abschnitt **Domain und URLs für Confluence SAML SSO by Microsoft** im Azure-Portal ein.

    c. Geben Sie in **Login Button Name** (Name der Anmeldeschaltfläche) den Schaltflächennamen ein, der auf dem Anmeldebildschirm für Ihre Benutzer angezeigt werden soll.

    d. Wählen Sie unter **SAML User ID Locations** (Speicherorte der SAML-Benutzer-ID) entweder die Option **User ID is in the NameIdentifier element of the Subject statement** (Benutzer-ID ist im NameIdentifier-Element der Subject-Anweisung enthalten) oder **User ID is in an Attribute element** (Benutzer-ID ist in einem Attribute-Element enthalten) aus.  Diese ID muss die Confluence-Benutzer-ID sein. Stimmt die Benutzer-ID nicht überein, ist eine Anmeldung nicht möglich. 

    > [!Note]
    > Der Standardspeicherort der SAML-Benutzer-ID ist „Name Identifier“. Sie können dies in ein Attribut ändern und den entsprechenden Attributnamen eingeben.
    
    e. Wenn Sie die Option **User ID is in an Attribute element** (Benutzer-ID ist in einem Attribute-Element enthalten) auswählen, geben Sie im Textfeld **Attributname** den Namen des Attributs ein, in dem die Benutzer-ID erwartet wird. 

    f. Wenn Sie die Verbunddomäne von Azure AD (z.B. AD FS usw.) verwenden, klicken Sie auf die Option **Startbereichserkennung aktivieren**, und konfigurieren Sie den **Domänennamen**.
    
    g. Geben Sie in **Domänenname** den Domänennamen für den Fall einer auf AD FS basierenden Anmeldung ein.

    h. Setzen Sie ein Häkchen bei **Einmaliges Abmelden aktivieren**, wenn ein Benutzer beim Abmelden von Confluence bei Azure AD abgemeldet werden soll. 

    i. Klicken Sie auf die Schaltfläche **Save**, um die Änderungen zu speichern.


> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.
 
### <a name="creating-a-confluence-saml-sso-by-microsoft-test-user"></a>Erstellen eines Confluence SAML SSO by Microsoft-Testbenutzers

Um es Azure AD-Benutzern zu ermöglichen, sich auf dem lokalen Confluence-Server anzumelden, müssen sie in Confluence SAML SSO by Microsoft bereitgestellt werden. Bei Confluence SAML SSO by Microsoft ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrem lokalen Confluence-Server als Administrator an.

2. Fahren Sie mit dem Mauszeiger über das Zahnrad, und klicken Sie auf **Benutzerverwaltung**.

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-confluencemicrosoft-tutorial/user1.png) 

3. Klicken Sie im Abschnitt „Benutzer“ auf die Registerkarte **Benutzer hinzufügen**. Führen Sie auf der Dialogfeldseite **Benutzer hinzufügen** die folgenden Schritte aus:

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-confluencemicrosoft-tutorial/user2.png) 

    a. Geben Sie im Textfeld **Benutzername** die E-Mail-Adresse des Benutzers, z.B. von Britta Simon, ein.

    b. Geben Sie im Textfeld **Vollständiger Name** den vollständigen Namen des Benutzers, z.B. „Britta Simon“, ein.

    c. Geben Sie im Textfeld **E-Mail-Adresse** die E-Mail-Adresse des Benutzers, z.B. Brittasimon@contoso.com, ein.

    d. Geben Sie im Textfeld **Kennwort** das Kennwort von Britta Simon ein.

    e. Klicken Sie auf **Kennwort bestätigen**, um das Kennwort erneut einzugeben.
    
    f. Klicken Sie auf die Schaltfläche **Hinzufügen**.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Confluence SAML SSO by Microsoft gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu Confluence SAML SSO by Microsoft hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Confluence SAML SSO by Microsoft** aus.

    ![Configure Single Sign-On](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Confluence SAML SSO by Microsoft“ klicken, sollten Sie automatisch bei Ihrer Confluence SAML SSO by Microsoft-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_203.png

