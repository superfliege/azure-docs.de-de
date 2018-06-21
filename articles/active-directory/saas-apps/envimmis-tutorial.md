---
title: 'Tutorial: Azure Active Directory-Integration mit Envi MMIS | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Envi MMIS konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ab89f8ee-2507-4625-94bc-b24ef3d5e006
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: jeedes
ms.openlocfilehash: d115a2640da7a23cc0e7744bbe79251afb9302a1
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36229235"
---
# <a name="tutorial-azure-active-directory-integration-with-envi-mmis"></a>Tutorial: Azure Active Directory-Integration mit Envi MMIS

In diesem Tutorial erfahren Sie, wie Sie Envi MMIS in Azure Active Directory (Azure AD) integrieren.

Die Integration von Envi MMIS in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf Envi MMIS Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Envi MMIS anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Envi MMIS konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Envi MMIS-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Envi MMIS aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-envi-mmis-from-the-gallery"></a>Hinzufügen von Envi MMIS aus dem Katalog
Zum Konfigurieren der Integration von Envi MMIS in Azure AD müssen Sie Envi MMIS aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Envi MMIS aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Envi MMIS** ein, wählen Sie im Ergebnisbereich **Envi MMIS** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Envi MMIS in der Ergebnisliste](./media/envimmis-tutorial/tutorial_envimmis_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei eEnvi MMIS anhand eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Envi MMIS als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Envi MMIS muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD mit Envi MMIS müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Envi MMIS-Testbenutzers](#create-an-envi-mmis-test-user)**, um eine Entsprechung von Britta Simon in Envi MMIS zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Envi MMIS-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Envi MMIS die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Envi MMIS** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/envimmis-tutorial/tutorial_envimmis_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Envi MMIS** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Envi MMIS](./media/envimmis-tutorial/tutorial_envimmis_url.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://www.<CUSTOMER DOMAIN>.com/Account`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://www.<CUSTOMER DOMAIN>.com/Account/Acs`

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Envi MMIS](./media/envimmis-tutorial/tutorial_envimmis_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://www.<CUSTOMER DOMAIN>.com/Account`.
     
    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner, die Antwort-URL und die Anmelde-URL. Wenden Sie sich an das [Clientsupportteam von Envi MMIS](mailto:support@ioscorp.com), um diese Werte zu erhalten.

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/envimmis-tutorial/tutorial_envimmis_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/envimmis-tutorial/tutorial_general_400.png)

7. Melden Sie sich in einem anderen Webbrowserfenster bei der Envi MMIS-Website als Administrator an.

8. Klicken Sie auf die Registerkarte **My Domain**.

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/envimmis-tutorial/configure1.png)

9. Klicken Sie auf **Edit**.

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/envimmis-tutorial/configure2.png)

10. Aktivieren Sie das Kontrollkästchen **Use remote authentication** (Remoteauthentifizierung verwenden), und wählen Sie dann **HTTP Redirect** (HTTP-Umleitung) in der Dropdownliste **Authentication Type** (Authentifizierungstyp) aus.

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/envimmis-tutorial/configure3.png)

11. Wählen Sie die Registerkarte **Resources** (Ressourcen) aus, und klicken Sie dann auf **Upload Metadata** (Metadaten hochladen).

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/envimmis-tutorial/configure4.png)

12. Führen Sie im Popupmenü **Upload Metadata** (Metadaten hochladen) die folgenden Schritte aus:

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/envimmis-tutorial/configure5.png)

    a. Wählen Sie im Dropdownmenü **Upload from** (Hochladen aus) die Option **File** (Datei) aus.

    b. Laden Sie die heruntergeladene Metadatendatei aus dem Azure-Portal hoch, indem Sie auf das Symbol **Choose File** (Datei auswählen) klicken.

    c. Klicken Sie auf **OK**.

13. Nach dem Hochladen der heruntergeladenen Metadatendatei werden die Felder automatisch aufgefüllt. Klicken Sie auf **Aktualisieren**.

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/envimmis-tutorial/configure6.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/envimmis-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/envimmis-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/envimmis-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/envimmis-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-an-envi-mmis-test-user"></a>Erstellen eines Envi MMIS-Testbenutzers

Damit sich Azure AD-Benutzer bei Envi MMIS anmelden können, müssen sie in Envi MMIS bereitgestellt werden.  
Im Fall von Envi MMIS ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der Envi MMIS-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf die Registerkarte **User List** (Benutzerliste).

    ![Mitarbeiter hinzufügen](./media/envimmis-tutorial/user1.png)

3. Klicken Sie auf die Schaltfläche **Add User** (Benutzer hinzufügen).

    ![Mitarbeiter hinzufügen](./media/envimmis-tutorial/user2.png)

4. Führen Sie im Abschnitt **Add User** die folgenden Schritte aus:

    ![Mitarbeiter hinzufügen](./media/envimmis-tutorial/user3.png)

    a. Geben Sie im Textfeld **User Name** (Benutzername) den Benutzernamen des Kontos von Britta Simon ein: **brittasimon@contoso.com**.
    
    b. Geben Sie im Textfeld **First Name** (Vorname) den Vornamen von Britta Simon ein: **Britta**.

    c. Geben Sie im Textfeld **Last Name** (Nachname) den Nachnamen von Britta Simon ein: **Simon**.

    d. Geben Sie im Textfeld **Title** (Titel) den Titel des Benutzers ein.
    
    e. Geben Sie im Textfeld **Email Address** (E-Mail-Adresse) die E-Mail-Adresse des Kontos von Britta Simon ein: **brittasimon@contoso.com**.

    f. Geben Sie im Textfeld **SSO User Name** (Benutzername für einmaliges Anmelden) den Benutzernamen des Kontos von Britta Simon ein: **brittasimon@contoso.com**.

    g. Klicken Sie auf **Speichern**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Envi MMIS gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon zu Envi MMIS zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **Envi MMIS**aus.

    ![Envi MMIS-Link in der Anwendungsliste](./media/envimmis-tutorial/tutorial_envimmis_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Envi MMIS“ klicken, sollten Sie automatisch bei Ihrer Envi MMIS-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/envimmis-tutorial/tutorial_general_01.png
[2]: ./media/envimmis-tutorial/tutorial_general_02.png
[3]: ./media/envimmis-tutorial/tutorial_general_03.png
[4]: ./media/envimmis-tutorial/tutorial_general_04.png

[100]: ./media/envimmis-tutorial/tutorial_general_100.png

[200]: ./media/envimmis-tutorial/tutorial_general_200.png
[201]: ./media/envimmis-tutorial/tutorial_general_201.png
[202]: ./media/envimmis-tutorial/tutorial_general_202.png
[203]: ./media/envimmis-tutorial/tutorial_general_203.png

