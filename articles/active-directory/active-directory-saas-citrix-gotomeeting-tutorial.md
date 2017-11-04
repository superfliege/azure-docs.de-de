---
title: 'Tutorial: Azure Active Directory-Integration mit GoToMeeting | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und GoToMeeting konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bcaf19f2-5809-4e1c-acbc-21a8d3498ccf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: jeedes
ms.openlocfilehash: e8871ea9c3aa9f962bf961a8b44b748392a56ba7
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="tutorial-azure-active-directory-integration-with-gotomeeting"></a>Tutorial: Azure Active Directory-Integration mit GoToMeeting

In diesem Tutorial erfahren Sie, wie Sie GoToMeeting in Azure Active Directory (Azure AD) integrieren.

Die Integration von GoToMeeting in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf GoToMeeting Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei GoToMeeting anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration mit GoToMeeting benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein GoToMeeting-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von GoToMeeting aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-gotomeeting-from-the-gallery"></a>Hinzufügen von GoToMeeting aus dem Katalog
Zum Konfigurieren der Integration von GoToMeeting in Azure AD müssen Sie GoToMeeting aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um GoToMeeting aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **GoToMeeting** ein, wählen Sie im Ergebnisbereich **GoToMeeting** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![GoToMeeting in der Ergebnisliste](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei GoToMeeting mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in GoToMeeting als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in GoToMeeting muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in GoToMeeting den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit GoToMeeting müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Testbenutzers für GoToMeeting](#create-a-gotomeeting-test-user)**, um eine Entsprechung von Britta Simon in GoToMeeting zu erhalten, die mit der Darstellung des Benutzers in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer GoToMeeting-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit GoToMeeting die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **GoToMeeting** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_samlbase.png)

3. Klicken Sie im Abschnitt **Domäne und URLs für GoToMeeting** auf **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Aktionen aus.

    ![SSO-Informationen zur Domäne und zu den URLs für GoToMeeting](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_ssourl.png)

    a. Geben Sie im Textfeld **Bezeichner** die folgende URL ein: `https://authentication.logmeininc.com/saml/sp`

    b. Geben Sie im Textfeld **Antwort-URL** folgende URL ein: `https://authentication.logmeininc.com/saml/acs`

    c. Geben Sie im Textfeld **Relayzustand** eine der folgenden URLs ein:

    **Für GoToMeeting**: `https://global.gotomeeting.com`
    
    **Für GoToTraining**: `https://global.gototraining.com`

    **Für GoToWebinar**: `https://global.gotowebinar.com`

    **Für GoToAssist**: `https://app.gotoassist.com`


4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_400.png)

6. Zum Generieren der **Metadaten**-URL führen Sie die folgenden Schritte aus:

    a. Klicken Sie auf **App-Registrierungen**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_appregistrations.png)
   
    b. Klicken Sie auf **Endpunkte**, um das Dialogfeld **Endpunkte** zu öffnen.  
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_endpointicon.png)

    c. Klicken Sie auf die Schaltfläche „Kopieren“, um die **VERBUNDMETADATENDOKUMENT**-URL zu kopieren und in Editor einzufügen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_endpoint.png)
     
    d. Kehren Sie nun zur Eigenschaftenseite von **GoToMeeting** zurück, kopieren Sie die **Anwendungs-ID** mithilfe der Schaltfläche **Kopieren**, und fügen Sie sie in Editor ein.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_appid.png)

    e. Generieren Sie die **Metadaten-URL** mithilfe des folgenden Musters:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`   

7. Klicken Sie im Abschnitt **GoToMeeting-Konfiguration** auf **GoToMeeting konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![GoToMeeting-Konfiguration](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_configure.png) 

8. Melden Sie sich in einem anderen Browserfenster bei [GoToMeeting Organization Center](https://account.citrixonline.com/organization/administration/) an.

9. Auf der Registerkarte **Identitätsanbieter** können Sie die Azure-Einstellungen konfigurieren, indem Sie entweder die generierte **Metadaten-URL** oder die heruntergeladene **Metadatendatei** oder **Manuell** angeben.

10. Führen Sie für **Metadaten-URL** die folgenden Schritte aus:

    ![GoToMeeting-Konfiguration](./media/active-directory-saas-gotomeeting-tutorial/config1.png)

    a. Wählen Sie unter **How would you like to configure your SAML IDP?** (Wie soll Ihr SAML-Identitätsanbieter konfiguriert werden?) in der Dropdownliste die Option **Automatische** aus.

    b. Fügen Sie die **Metadaten-URL**, die Sie in den vorherigen Schritten generiert haben, im Textfeld **Metadaten-URL** ein.

    c. Klicken Sie auf **Speichern**.

11. Führen Sie für **Metadatendatei** die folgenden Schritte aus:

    ![GoToMeeting-Konfiguration](./media/active-directory-saas-gotomeeting-tutorial/config2.png)

    a. Wählen Sie unter **How would you like to configure your SAML IDP?** (Wie soll Ihr SAML-Identitätsanbieter konfiguriert werden?) in der Dropdownliste die Option **Upload SAML metadata file** (SAML-Metadatendatei hochladen) aus.

    b. Klicken Sie auf **Upload metadata file** (Metadatendatei hochladen), um die heruntergeladene Metadatendatei hochzuladen.

    c. Klicken Sie auf **Speichern**.

12. Führen Sie für **Manuell** die folgenden Schritte aus:

    ![GoToMeeting-Konfiguration](./media/active-directory-saas-gotomeeting-tutorial/config3.png)

    a.  Fügen Sie in das Textfeld **Sign-in page URL** (URL der Anmeldeseite) den Wert der **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.

    b.  Fügen Sie in das Textfeld **Sign-out page URL** (URL der Abmeldeseite) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    c.  Fügen Sie im Textfeld **Entitäts-ID des Identitätsanbieters** den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Extrahieren Sie das X509Certificate aus der heruntergeladenen Metadatendatei, und laden Sie dieses Zertifikat hoch, indem Sie auf **Zertifikat hochladen** klicken.

    e. Klicken Sie auf **Speichern**.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-gotomeeting-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-gotomeeting-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-gotomeeting-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-gotomeeting-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-gotomeeting-test-user"></a>Erstellen eines Testbenutzers für GoToMeeting

In diesem Abschnitt wird eine Benutzerin mit dem Namen Britta Simon in GoToMeeting erstellt. GoToMeeting unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Falls ein Benutzer nicht bereits in GoToMeeting vorhanden ist, wird beim Versuch, auf GoToMeeting zuzugreifen, ein neuer Benutzer erstellt.

> [!NOTE]
> Setzen Sie sich mit dem [Supportteam für GoToMeeting](https://support.logmeininc.com/gotomeeting) in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie Zugriff auf GoToMeeting gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie zum Zuweisen von Britta Simon zu GoToMeeting folgende Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste der Anwendungen **GoToMeeting** aus.

    ![GoToMeeting-Link in der Anwendungsliste](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „GoToMeeting“ klicken, sollten Sie automatisch in Ihrer GoToMeeting-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurieren der Benutzerbereitstellung](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-citrixgotomeeting-provisioning-tutorial)


<!--Image references-->

[1]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_203.png

