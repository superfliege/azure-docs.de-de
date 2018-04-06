---
title: 'Tutorial: Integration von Spacio in Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Spacio konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 9df8d199-b955-483c-aa4e-cabad1a0b9d6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: jeedes
ms.openlocfilehash: 604249151f3c1ca206f64a178b25ea1dae5b5023
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="tutorial-azure-active-directory-integration-with-spacio"></a>Tutorial: Integration von Spacio in Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie Spacio in Azure Active Directory (Azure AD) integrieren.

Die Integration von Spacio in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Spacio hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Spacio anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Integration von Spacio in Azure AD konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Spacio-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Spacio aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-spacio-from-the-gallery"></a>Hinzufügen von Spacio aus dem Katalog
Zum Konfigurieren der Integration von Spacio in Azure AD müssen Sie Spacio aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Spacio aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Spacio** ein, wählen Sie im Ergebnisbereich **Spacio** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Spacio in der Ergebnisliste](./media/active-directory-saas-spacio-tutorial/tutorial_spacio_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Spacio mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Spacio als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Spacio muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Spacio müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Spacio-Testbenutzers](#create-a-spacio-test-user)**, um eine Entsprechung von Britta Simon in Spacio zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Spacio-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Spacio die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Spacio** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-spacio-tutorial/tutorial_spacio_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Spacio** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Spacio](./media/active-directory-saas-spacio-tutorial/tutorial_spacio_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://sso.spac.io/<brokerageID>`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://sso.spac.io/<brokerageID>`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Clientsupportteam von Spacio](mailto:support@spac.io), um diese Werte zu erhalten. 

4. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-spacio-tutorial/tutorial_general_400.png)

5. Führen Sie zum Generieren der **Metadaten-URL** die folgenden Schritte aus:

    a. Klicken Sie auf **App-Registrierungen**.
    
    ![Configure Single Sign-On](./media/active-directory-saas-spacio-tutorial/tutorial_spacio_appregistrations.png)
   
    b. Klicken Sie auf **Endpunkte**, um das Dialogfeld **Endpunkte** zu öffnen.  
    
    ![Configure Single Sign-On](./media/active-directory-saas-spacio-tutorial/tutorial_spacio_endpointicon.png)

    c. Klicken Sie auf die Schaltfläche **Kopieren**, um die URL des **VERBUNDMETADATEN-DOKUMENTS** zu kopieren und in Editor einzufügen.
    
    ![Configure Single Sign-On](./media/active-directory-saas-spacio-tutorial/tutorial_spacio_endpoint.png)
     
    d. Kehren Sie nun zur Eigenschaftenseite von **Spacio** zurück, kopieren Sie die **Anwendungs-ID** mithilfe der Schaltfläche **Kopieren**, und fügen Sie sie in Editor ein.
 
    ![Configure Single Sign-On](./media/active-directory-saas-spacio-tutorial/tutorial_spacio_appid.png)

    e. Generieren Sie die **Metadaten-URL** mithilfe des folgenden Musters:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`

6. Zum Konfigurieren des einmaligen Anmeldens bei **Spacio** müssen Sie die generierte **Metadaten-URL** an das [Spacio-Supportteam](mailto:support@spac.io) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-spacio-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-spacio-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-spacio-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-spacio-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-spacio-test-user"></a>Erstellen eines Spacio-Testbenutzers

In diesem Abschnitt erstellen Sie in Spacio eine Benutzerin namens Britta Simon. Lassen Sie sich beim Hinzufügen der Benutzer zur Spacio-Plattform ggf. vom [Spacio-Supportteam](mailto:support@spac.io) unterstützen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Spacio gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Spacio zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **Spacio** aus.

    ![Spacio-Link in der Anwendungsliste](./media/active-directory-saas-spacio-tutorial/tutorial_spacio_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Spacio“ klicken, sollten Sie automatisch in Ihrer Spacio-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-spacio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-spacio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-spacio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-spacio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-spacio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-spacio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-spacio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-spacio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-spacio-tutorial/tutorial_general_203.png

