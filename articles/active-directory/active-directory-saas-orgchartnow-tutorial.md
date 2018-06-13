---
title: 'Tutorial: Azure Active Directory-Integration mit OrgChart Now | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und OrgChart Now konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 50a1522f-81de-4d14-9b6b-dd27bb1338a4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2018
ms.author: jeedes
ms.openlocfilehash: c46a2e68699365d818c83be2b1b0ff2ce2ef88ab
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34349198"
---
# <a name="tutorial-azure-active-directory-integration-with-orgchart-now"></a>Tutorial: Azure Active Directory-Integration mit OrgChart Now

In diesem Tutorial erfahren Sie, wie Sie OrgChart Now in Azure Active Directory (Azure AD) integrieren.

Die Integration von OrgChart Now in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf OrgChart Now hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch an OrgChart Now anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit OrgChart Now konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein OrgChart Now-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von OrgChart Now über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-orgchart-now-from-the-gallery"></a>Hinzufügen von OrgChart Now über den Katalog
Zum Konfigurieren der Integration von OrgChart Now in Azure AD müssen Sie OrgChart Now aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um OrgChart Now aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **OrgChart Now** ein, wählen Sie im Ergebnisbereich **OrgChart Now** aus, und klicken Sie anschließend auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![OrgChart Now in der Ergebnisliste](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei OrgChart Now basierend auf einem Testbenutzer mit dem Namen „Britta Simon“.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in OrgChart Now als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in OrgChart Now muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei OrgChart Now müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines OrgChart Now-Testbenutzers](#create-an-orgchart-now-test-user)**, um eine Entsprechung von Britta Simon in OrgChart Now zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer OrgChart Now-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in OrgChart Now die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **OrgChart Now** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für OrgChart Now** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für OrgChart Now](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_url.png)

    Geben Sie im Textfeld **Bezeichner** eine URL ein: `https://sso2.orgchartnow.com`

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für OrgChart Now](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://sso2.orgchartnow.com/Shibboleth.sso/Login?entityID=<YourEntityID>&target=https://sso2.orgchartnow.com`.
     
    > [!NOTE]
    > `<YourEntityID>` ist die SAML-Entitäts-ID, die aus dem Abschnitt „Kurzübersicht“, der später im Tutorial beschrieben wird, kopiert wird.

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_400.png)
    
7. Klicken Sie im Abschnitt **OrgChart Now-Konfiguration** auf **OrgChart Now konfigurieren**, um das Fenster **Anmeldung konfigurieren**  zu öffnen. Kopieren Sie die **SAML-Entitäts-ID** aus dem **Abschnitt „Kurzübersicht“**, und fügen Sie sie in das Feld **Anmelde-URL** im Abschnitt **Domäne und URLs für OrgChart Now** ein.

    ![OrgChart Now-Konfiguration](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_configure.png) 

8. Zum Konfigurieren des einmaligen Anmeldens bei **OrgChart Now** müssen Sie die heruntergeladene **Metadaten-XML**-Datei an das [OrgChart Now-Supportteam](mailto:ocnsupport@officeworksoftware.com) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-orgchartnow-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-orgchartnow-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-orgchartnow-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-orgchartnow-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-an-orgchart-now-test-user"></a>Erstellen eines OrgChart Now-Testbenutzers

Damit sich Azure AD-Benutzer an OrgChart Now anmelden können, müssen sie in OrgChart Now bereitgestellt werden. 

1. OrgChart Now unterstützt die Just-In-Time-Bereitstellung, die standardmäßig aktiviert ist. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf OrgChart Now ein neuer Benutzer erstellt. Mit dem Feature für die Just-in-Time-Bereitstellung wird nur dann ein Benutzer mit **Lesezugriff** erstellt, wenn eine SSO-Anforderung von einem bekannten IDP stammt und die E-Mail-Adresse in der Benutzerliste der SAML-Assertion nicht gefunden wird. Für dieses Feature zur automatischen Bereitstellung müssen Sie in OrgChart Now eine Zugriffsgruppe mit dem Titel **General** (Allgemein) erstellen. Führen Sie die unten angegebenen Schritte aus, um eine Zugriffsgruppe zu erstellen:

    a. Navigieren Sie zur Option **Gruppen verwalten**, nachdem Sie oben rechts auf der Benutzeroberfläche auf das **Zahnrad** geklickt haben.

    ![OrgChart Now-Gruppen](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_manage.png)  

    b. Wählen Sie das Symbol **Hinzufügen**, und geben Sie der Gruppe den Namen **General**. Klicken Sie anschließend auf **OK**. 

    ![OrgChart Now – Hinzufügen](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_add.png)

    c. Wählen Sie die Ordner aus, für die Benutzer allgemeinen oder schreibgeschützten Zugriff erhalten sollen:

    ![OrgChart Now-Ordner](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_chart.png)

    d. Sperren Sie die Ordner mit **Lock** (Sperren), damit nur Administratorbenutzer diese ändern können. Wählen Sie anschließend **OK**.

    ![OrgChart Now – Sperren](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_lock.png)

2. Wenn Sie Benutzer vom Typ **Admin** und **Read/Write** (Lesen/Schreiben) erstellen möchten, müssen Sie einen Benutzer manuell erstellen, um Zugriff auf seine Berechtigungsstufe per SSO zu erhalten. Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:

    a. Melden Sie sich als Sicherheitsadministrator an OrgChart Now an.

    b.  Klicken Sie oben rechts auf **Settings** (Einstellungen), und navigieren Sie anschließend zu **Manage Users** (Benutzer verwalten).

    ![OrgChart Now-Einstellungen](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_settings.png)

    c. Klicken Sie auf **Add** (Hinzufügen), und führen Sie die folgenden Schritte aus:

    ![OrgChart Now – Verwalten](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_manageusers.png)

    * Geben Sie im Textfeld **User ID** (Benutzer-ID) die Benutzer-ID im Format **brittasimon@contoso.com** ein.

    * Geben Sie im Textfeld **Email Address** (E-Mail-Adresse) die E-Mail-Adresse des Benutzers ein, z.B. **brittasimon@contoso.com**.

    * Klicken Sie auf **Hinzufügen**.
    
### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf OrgChart Now gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon OrgChart Now zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **OrgChart Now** aus.

    ![OrgChart Now-Link in der Anwendungsliste](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „OrgChart Now“ klicken, sollten Sie automatisch an Ihrer OrgChart Now-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_203.png

