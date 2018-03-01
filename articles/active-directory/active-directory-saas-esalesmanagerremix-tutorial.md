---
title: 'Tutorial: Azure Active Directory-Integration mit E Sales Manager Remix | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und E Sales Manager Remix konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 89b5022c-0d5b-4103-9877-ddd32b6e1c02
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: jeedes
ms.openlocfilehash: 56c2860b6aeac9fdc66f2b1fdd1ed9126bf77d3f
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2018
---
# <a name="tutorial-azure-active-directory-integration-with-e-sales-manager-remix"></a>Tutorial: Azure Active Directory-Integration mit E Sales Manager Remix

In diesem Tutorial erfahren Sie, wie Sie E Sales Manager Remix in Azure Active Directory (Azure AD) integrieren.

Die Integration von E Sales Manager Remix in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf E Sales Manager Remix hat.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei E Sales Manager Remix anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit E Sales Manager Remix konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein E Sales Manager Remix-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von E Sales Manager Remix aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-e-sales-manager-remix-from-the-gallery"></a>Hinzufügen von E Sales Manager Remix aus dem Katalog
Zum Konfigurieren der Integration von E Sales Manager Remix in Azure AD müssen Sie E Sales Manager Remix aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um E Sales Manager Remix aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **E Sales Manager Remix** ein, wählen Sie im Ergebnisbereich **E Sales Manager Remix** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![E Sales Manager Remix in der Ergebnisliste](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei E Sales Manager Remix mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in E Sales Manager Remix als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in E Sales Manager Remix muss eine Linkbeziehung eingerichtet werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD bei E Sales Manager Remix zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines E Sales Manager Remix-Testbenutzers](#create-an-e-sales-manager-remix-test-user)**, um eine Entsprechung von Britta Simon in E Sales Manager Remix zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer E Sales Manager Remix-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD bei E Sales Manager Remix zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **E Sales Manager Remix** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für E Sales Manager Remix** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für E Sales Manager Remix](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<Server-Based-URL>/<sub-domain>/esales-pc`

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<Server-Based-URL>/<sub-domain>/`

    c. Kopieren Sie den Wert für **Bezeichner** im Editor. Sie verwenden diesen Wert später in diesem Tutorial.
    
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam für den E Sales Manager Remix-Client](mailto:esupport@softbrain.co.jp), um diese Werte zu erhalten.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_certificate.png) 

5. Wählen Sie **Alle weiteren Benutzerattribute anzeigen und bearbeiten** aus, und klicken Sie auf das Attribut **emailaddress**.
    
    ![E Sales Manager Remix-Konfiguration](./media/active-directory-saas-esalesmanagerremix-tutorial/configure1.png)

6. Kopieren Sie den Wert für **Namespace** und **Name** des Anspruchs aus dem Textfeld. Generieren Sie den Wert im Format `<Namespace>/<Name>`. Sie verwenden diesen Wert später in diesem Tutorial.

    ![E Sales Manager Remix-Konfiguration](./media/active-directory-saas-esalesmanagerremix-tutorial/configure2.png)

7. Klicken Sie im Abschnitt **E Sales Manager Remix-Konfiguration** auf **E Sales Manager Remix konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![E Sales Manager Remix-Konfiguration](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_configure.png) 

8. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_400.png)

9. Melden Sie sich bei Ihrer E Sales Manager Remix-Anwendung als Administrator an.

10. Wählen Sie **To Administrator Menu** (Zum Administratormenü) aus dem Menü oben rechts aus.

    ![E Sales Manager Remix-Konfiguration](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

11. Wählen Sie **System settings** > **Cooperation with external system** (Systemeinstellungen > Zusammenarbeit mit externem System) aus.

    ![E Sales Manager Remix-Konfiguration](./media/active-directory-saas-esalesmanagerremix-tutorial/configure5.png)
    
12. Wählen Sie **SAML**aus.

    ![E Sales Manager Remix-Konfiguration](./media/active-directory-saas-esalesmanagerremix-tutorial/configure6.png)

13. Führen Sie im Abschnitt **SAML authentication setting** (SAML-Authentifizierungseinstellung) die folgenden Schritte aus:

    ![E Sales Manager Remix-Konfiguration](./media/active-directory-saas-esalesmanagerremix-tutorial/configure3.png)
    
    a. Wählen Sie **PC version** aus.
    
    b. Wählen Sie **email** aus der Dropdownliste im Abschnitt „Collaboration item“ (Zusammenarbeitselement) aus.

    c. Fügen Sie in das Textfeld im Abschnitt „Collaboration item“ (Zusammenarbeitselement) den **Anspruchswert** ein, den Sie aus dem Azure-Portal kopiert haben, d. h. `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. Fügen Sie in das Textfeld **Issuer (entity ID)** (Aussteller (Entitäts-ID)) den Wert für **Bezeichner** ein, den Sie aus dem Azure-Portal im Abschnitt **Domäne und URLs für E Sales Manager Remix** kopiert haben.

    e. Wählen Sie **File selection** (Dateiauswahl) aus, um das heruntergeladene **Zertifikat** aus dem Azure-Portal hochzuladen.

    f. Fügen Sie in das Textfeld **ID provider login URL** (Anmelde-URL des Identitätsanbieters) die **URL für den SAML-SSO-Dienst** ein, die Sie aus dem Azure-Portal kopiert haben.

    g. Fügen Sie in das Textfeld **Identity Provider Logout URL** (Abmelde-URL des Identitätsanbieters) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    h. Klicken Sie auf **Setting complete** (Einstellung abgeschlossen).

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-an-e-sales-manager-remix-test-user"></a>Erstellen eines E Sales Manager Remix-Testbenutzers

1. Melden Sie sich bei Ihrer E Sales Manager Remix-Anwendung als Administrator an.

2. Wählen Sie **To Administrator Menu** (Zum Administratormenü) aus dem Menü oben rechts aus.

    ![E Sales Manager Remix-Konfiguration](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

3. Wählen Sie **Your company settings** > **Maintenance of departments and employees** (Ihre Unternehmenseinstellungen > Verwaltung von Abteilungen und Mitarbeitern) und dann **Employees registered** (Registrierte Mitarbeiter) aus.

    ![Der Benutzer](./media/active-directory-saas-esalesmanagerremix-tutorial/user1.png)

4. Führen Sie im Abschnitt **New employee registration** (Neue Mitarbeiterregistrierung) die folgenden Schritte aus:
    
    ![Der Benutzer](./media/active-directory-saas-esalesmanagerremix-tutorial/user2.png)

    a. Geben Sie im Textfeld **Employee Name** (Mitarbeitername) den Namen des Benutzers ein, z. B. „Britta“.

    b. Füllen Sie alle entsprechenden Pflichtfelder mit den Benutzerinformationen aus.
    
    c. Wenn Sie SAML aktivieren, kann sich der Administrator nicht über den Anmeldebildschirm anmelden. Deshalb wählen Sie für den Benutzer **Admin Login** (Administratoranmeldung) aus, um ihm Administrator-Anmeldeberechtigungen zu gewähren.

    d. Klicken Sie auf **Registration** (Registrierung).

5. Wenn Sie sich zukünftig als Administrator anmelden möchten, melden Sie sich mit dem Benutzer an, dem die Administratorberechtigung erteilt wurde, und klicken Sie auf **To Administrator Menu** (Zum Administratormenü) im Menü oben rechts.

    ![Der Benutzer](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden von Azure, indem Sie ihr Zugriff auf E Sales Manager Remix gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon E Sales Manager Remix zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **E Sales Manager Remix** aus.

    ![Link „E Sales Manager Remix“ in der Liste „Anwendungen“](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „E Sales Manager Remix“ klicken, sollten Sie automatisch bei Ihrer E Sales Manager Remix-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_203.png

