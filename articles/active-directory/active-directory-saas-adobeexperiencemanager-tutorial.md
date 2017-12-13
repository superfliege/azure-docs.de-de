---
title: 'Tutorial: Azure Active Directory-Integration mit Adobe Experience Manager | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Adobe Experience Manager konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 88a95bb5-c17c-474f-bb92-1f80f5344b5a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jeedes
ms.openlocfilehash: 4ba94f0f24b2791b3b32807aa60379aeadb79365
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-experience-manager"></a>Tutorial: Azure Active Directory-Integration mit Adobe Experience Manager

In diesem Tutorial erfahren Sie, wie Sie Adobe Experience Manager in Azure Active Directory (Azure AD) integrieren.

Die Integration von Adobe Experience Manager in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Adobe Experience Manager hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Adobe Experience Manager anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Adobe Experience Manager konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Adobe Experience Manager-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Adobe Experience Manager aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-adobe-experience-manager-from-the-gallery"></a>Hinzufügen von Adobe Experience Manager aus dem Katalog
Zum Konfigurieren der Integration von Adobe Experience Manager in Azure AD müssen Sie Adobe Experience Manager aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Adobe Experience Manager aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Adobe Experience Manager** ein, wählen Sie im Ergebnisbereich **Adobe Experience Manager** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Adobe Experience Manager in der Ergebnisliste](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Adobe Experience Manager mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Adobe Experience Manager als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Adobe Experience Manager muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Adobe Experience Manager den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD bei Adobe Experience Manager zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Adobe Experience Manager-Testbenutzers](#create-an-adobe-experience-manager-test-user)**, um eine Entsprechung von Britta Simon in Adobe Experience Manager zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden bei Ihrer Adobe Experience Manager-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD bei Adobe Experience Manager zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Adobe Experience Manager** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Adobe Experience Manager** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Adobe Experience Manager](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_url1.png)

    a. Geben Sie im Textfeld **Bezeichner** einen eindeutigen Wert ein, den Sie auch auf dem AEM-Server definieren. 

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<AEM Server Url>/saml_login`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Wenden Sie sich an das [Supportteam von Adobe Experience Manager](https://helpx.adobe.com/support/experience-manager.html), um diese Werte zu erhalten.
 
4. Aktivieren Sie „Erweiterte URL-Einstellungen anzeigen“, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP**-initiierten Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Adobe Experience Manager](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_spconfigure.png)

    Geben Sie im Textfeld **Anmelde-URL** die URL des Adobe Experience Manager-Servers ein. 

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_certificate.png) 

6. Klicken Sie im Abschnitt „Experience Manager Configuration“ auf „Adobe Experience Manager konfigurieren“, um das Fenster „Anmeldung konfigurieren“ zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst**, die **SAML-Entitäts-ID** und die **Abmelde-URL** aus dem Abschnitt „Kurzübersicht“.

    ![Link für Konfigurationsabschnitt](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_configure.png) 

7. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_400.png)

8. Öffnen Sie das Verwaltungsportal für **Adobe Experience Manager** in einem neuen Browserfenster.

9. Wählen Sie **Settings** (Einstellungen) -> **Security** (Sicherheit) -> **Users** (Benutzer) aus.

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user.png)

10. Wählen Sie **Administrator** oder einen anderen relevanten Benutzer aus.

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin6.png)

11. Wählen Sie **Account Settings** (Kontoeinstellungen) -> **Create/Manage TrustStore** (TrustStore erstellen/verwalten) aus.

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_managetrust.png)

12. Klicken Sie unter **Add Certificate from CER file** (Zertifikat aus CER-Datei hinzufügen) auf die Schaltfläche **Select Certificate File** (Zertifikatsdatei auswählen). Suchen Sie die Zertifikatsdatei, die Sie im Azure-Portal heruntergeladen haben, und wählen Sie sie aus.

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user2.png)

13. Das Zertifikat wird dem TrustStore hinzugefügt. Notieren Sie den Alias des Zertifikats.

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin7.png)

14. Wählen Sie auf der Seite **Users** (Benutzer) die Option **authentication-service** aus.

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin8.png)

15. Wählen Sie **Account Settings** (Kontoeinstellungen) -> **Create/Manage KeyStore** (KeyStore erstellen/verwalten) aus. Erstellen Sie den KeyStore durch Angeben eines Kennworts.

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin9.png)

16. Wechseln Sie wieder zum Verwaltungsbildschirm, und wählen Sie **Settings** (Einstellungen) -> **Operations** (Vorgänge) -> **Web Console** (Webkonsole) aus.

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin1.png)

17. Dadurch wird die Konfigurationsseite geöffnet.

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin2.png)

18. Suchen Sie **Adobe Granite SAML 2.0 Authentication Handler**, und klicken Sie auf das Symbol zum **Hinzufügen**.

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin3.png)

19. Führen Sie auf dieser Seite die folgenden Aktionen aus.

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin4.png)

    a. Geben Sie im Textfeld **Path** (Pfad) **/** ein.

    b. Geben Sie im Textfeld **IDP URL** (IdP-URL) den Wert der **URL für den SAML-SSO-Dienst** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Geben Sie im Textfeld **IDP Certificate Alias** (Alias des IdP-Zertifikats) den Wert des **Certificate Alias** (Zertifikatalias) ein, den Sie in TrustStore hinzugefügt haben.

    d. Geben Sie im Textfeld **Security Provided Entity ID** (Sicherheits-Entitäts-ID) den Wert der eindeutigen **SAML-Entitäts-ID** ein, den Sie im Azure-Portal konfiguriert haben.

    e. Geben Sie im Textfeld **Assertion Consumer Service URL** (Assertionsverbraucherdienst-URL) den Wert der **Antwort-URL** ein, den Sie im Azure-Portal konfiguriert haben.

    f. Geben Sie im Textfeld **Password of Key Store** (Kennwort des Schlüsselspeichers) das **Kennwort** ein, das Sie in KeyStore festgelegt haben.

    g. Geben Sie im Textfeld **User Attribute ID** (Benutzerattribut-ID) die **Namens-ID** oder eine andere relevante Benutzer-ID ein.

    h. Aktivieren Sie **Autocreate CRX Users** (CRX-Benutzer automatisch erstellen).

    i. Geben Sie im Textfeld **Logout URL** (Abmelde-URL) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    j. Klicken Sie unten auf der Seite auf **Speichern**

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
  
### <a name="create-an-adobe-experience-manager-test-user"></a>Erstellen eines Adobe Experience Manager-Testbenutzers

In diesem Abschnitt erstellen Sie in Adobe Experience Manager eine Benutzerin namens Britta Simon. Wenn Sie die Option **Autocreate CRX Users** (CRX-Benutzer automatisch erstellen) aktiviert haben, werden nach der erfolgreichen Authentifizierung automatisch Benutzer erstellt. 

Wenn Sie Benutzer manuell erstellen möchten, wenden Sie sich an das [Adobe Experience Manager-Supportteam](https://helpx.adobe.com/support/experience-manager.html), um die Benutzer in der Adobe Experience Manager-Plattform hinzuzufügen. 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden von Azure, indem Sie ihr Zugriff auf Adobe Experience Manager gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um Britta Simon zu Adobe Experience Manager zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Adobe Experience Manager** aus.

    ![Adobe Experience Manager-Link in der Anwendungsliste](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Adobe Experience Manager“ klicken, sollten Sie automatisch bei Ihrer Adobe Experience Manager-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_203.png

