---
title: 'Tutorial: Azure Active Directory-Integration mit NetSuite | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Netsuite konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2018
ms.author: jeedes
ms.openlocfilehash: 55ee7f6b496def5669160f5cfafed7bc6d7eab11
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36219369"
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a>Tutorial: Azure Active Directory-Integration mit NetSuite

In diesem Tutorial erfahren Sie, wie Sie Netsuite in Azure Active Directory (Azure AD) integrieren.

Die Integration von Netsuite in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Netsuite hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Netsuite anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in Netsuite konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Netsuite-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Netsuite aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-netsuite-from-the-gallery"></a>Hinzufügen von Netsuite aus dem Katalog
Zum Konfigurieren der Integration von Netsuite in Azure AD müssen Sie Netsuite aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um Netsuite aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**.

    ![ANWENDUNGEN][3]

4. Geben Sie im Suchfeld den Namen **Netsuite** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/netsuite-tutorial/tutorial_netsuite_search.png)

5. Wählen Sie im Ergebnisbereich die Option **Netsuite** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/netsuite-tutorial/tutorial_netsuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Netsuite mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Netsuite als Pendant für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Netsuite muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in Netsuite zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Netsuite müssen die folgenden Schritte ausgeführt werden:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Netsuite-Testbenutzers](#creating-a-netsuite-test-user)**, um ein Pendant von Britta Simon in Netsuite zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Netsuite-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Netsuite die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Netsuite** auf **Einmaliges Anmelden**.

    ![Configure Single Sign-On][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Configure Single Sign-On](./media/netsuite-tutorial/tutorial_netsuite_samlbase.png)

3. Führen Sie auf der Seite **Domäne und URLs für Netsuite** die folgenden Schritte aus:

    ![Configure Single Sign-On](./media/netsuite-tutorial/tutorial_netsuite_url.png)

    Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein:   `https://<tenant-name>.netsuite.com/saml2/acs` `https://<tenant-name>.na1.netsuite.com/saml2/acs` `https://<tenant-name>.na2.netsuite.com/saml2/acs` `https://<tenant-name>.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Antwort-URL. Wenden Sie sich an das [Supportteam von Netsuite](http://www.netsuite.com/portal/services/support.shtml), um diese Werte zu erhalten.
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Configure Single Sign-On](./media/netsuite-tutorial/tutorial_netsuite_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure Single Sign-On](./media/netsuite-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Netsuite-Konfiguration** auf **Netsuite konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Configure Single Sign-On](./media/netsuite-tutorial/tutorial_netsuite_configure.png) 

7. Öffnen Sie eine neue Registerkarte in Ihrem Browser, und melden Sie sich bei Ihrer Netsuite-Unternehmenswebsite als Administrator an.

8. Klicken Sie auf der Symbolleiste am oberen Rand der Seite auf **Setup** und dann auf **Setup-Manager**.

    ![Configure Single Sign-On](./media/netsuite-tutorial/ns-setup.png)

9. Wählen Sie in der Liste **Setupaufgaben** die Option **Integration** aus.

    ![Configure Single Sign-On](./media/netsuite-tutorial/ns-integration.png)

10. Klicken Sie im Abschnitt **Authentifizierung verwalten** auf **Einmalige Anmeldung für SAML**.

    ![Configure Single Sign-On](./media/netsuite-tutorial/ns-saml.png)

11. Führen Sie auf der Seite **SAML-Setup** die folgenden Schritte aus:
   
    a. Kopieren Sie unter **Anmeldung konfigurieren** im Abschnitt **Kurzübersicht** den Wert der **SAML-Dienst-URL für einmaliges Anmelden**, und fügen Sie ihn in Netsuite ins Feld **Identity Provider Login Page** (Anmeldeseite für Identitätsanbieter) ein.

    ![Configure Single Sign-On](./media/netsuite-tutorial/ns-saml-setup.png)
  
    b. Wählen Sie in NetSuite **Primary Authentication Method** (Primäre Authentifizierungsmethode).

    c. Wählen Sie im Feld mit der Bezeichnung **SAMLV2-Identitätsanbietermetadaten** die Option **IDP-Metadatendatei hochladen** aus. Klicken Sie dann zum Hochladen der Metadatendatei, die Sie aus dem Azure-Portal heruntergeladen haben, auf **Durchsuchen** .

    ![Configure Single Sign-On](./media/netsuite-tutorial/ns-sso-setup.png)

    d. Klicken Sie auf **Submit**.

12. Aktivieren Sie in Azure AD das Kontrollkästchen **Alle weiteren Benutzerattribute anzeigen und bearbeiten**, und fügen Sie ein Attribut hinzu.

    ![Configure Single Sign-On](./media/netsuite-tutorial/ns-attributes.png)

13. Geben Sie im Feld **Attributname** die Zeichenfolge `account` ein. Geben Sie in das Feld **Attributwert** Ihre Konto-ID für NetSuite ein. Dieser Wert ist konstant und vom Konto abhängig. Beachten Sie die folgenden Anleitungen, um Ihre Konto-ID zu finden:

      ![Configure Single Sign-On](./media/netsuite-tutorial/ns-add-attribute.png)

    a. Klicken Sie in Netsuite im oberen Navigationsmenü auf **Setup**.

    b. Klicken Sie dann im linken Navigationsmenü im Abschnitt **Setupaufgaben** auf den Abschnitt **Integration**, und klicken Sie auf **Webdiensteeinstellungen**.

    c. Kopieren Sie Ihre Konto-ID für NetSuite, und fügen Sie sie in Azure AD in das Feld **Attributwert** ein.

    ![Configure Single Sign-On](./media/netsuite-tutorial/ns-account-id.png)

14. Bevor Benutzer die einmalige Anmeldung in NetSuite ausführen können, müssen ihnen zunächst die entsprechenden Berechtigungen in NetSuite zugewiesen werden. Befolgen Sie die folgenden Anweisungen, um diese Berechtigungen zuzuweisen.

    a. Klicken Sie im oberen Navigationsmenü auf **Setup** und dann auf **Setup-Manager**.
      
      ![Configure Single Sign-On](./media/netsuite-tutorial/ns-setup.png)

    b. Wählen Sie im linken Navigationsmenü **Benutzer/Rollen** aus, und klicken Sie auf **Rollen verwalten**.
      
      ![Configure Single Sign-On](./media/netsuite-tutorial/ns-manage-roles.png)

    c. Klicken Sie auf **Neue Rolle**.

    d. Geben Sie einen **Namen** für die neue Rolle ein.
      
      ![Configure Single Sign-On](./media/netsuite-tutorial/ns-new-role.png)

    e. Klicken Sie auf **Speichern**.

    f. Klicken Sie im oberen Menü auf **Berechtigungen**. Klicken Sie dann auf **Setup**.
      
       ![Configure Single Sign-On](./media/netsuite-tutorial/ns-sso.png)

    g. Wählen Sie **Set Up SAML Single Sign-on** (Einmaliges Anmelden für SAML einrichten) aus, und klicken Sie dann auf **Hinzufügen**.

    h. Klicken Sie auf **Speichern**.

    i. Klicken Sie im oberen Navigationsmenü auf **Setup** und dann auf **Setup-Manager**.
      
       ![Configure Single Sign-On](./media/netsuite-tutorial/ns-setup.png)

    j. Wählen Sie im linken Navigationsmenü **Benutzer/Rollen** aus, und klicken Sie auf **Benutzer verwalten**.
      
       ![Configure Single Sign-On](./media/netsuite-tutorial/ns-manage-users.png)

    k. Wählen Sie einen Testbenutzer. Klicken Sie dann auf **Bearbeiten**.
      
       ![Configure Single Sign-On](./media/netsuite-tutorial/ns-edit-user.png)

    l. Wählen Sie im Dialogfeld „Rollen“ die Rolle aus, die Sie erstellt haben, und klicken Sie auf **Hinzufügen**.
      
       ![Configure Single Sign-On](./media/netsuite-tutorial/ns-add-role.png)

    m. Klicken Sie auf **Speichern**.
    
### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/netsuite-tutorial/create_aaduser_01.png) 

2.  Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/netsuite-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/netsuite-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/netsuite-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**. 

### <a name="creating-a-netsuite-test-user"></a>Erstellen eines Netsuite-Testbenutzers

In diesem Abschnitt wird ein Benutzer mit dem Namen Britta Simon in Netsuite erstellt. Netsuite unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.
Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Falls ein Benutzer nicht bereits in Netsuite vorhanden ist, wird beim Versuch, auf Netsuite zuzugreifen, ein neuer Benutzer erstellt.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Netsuite gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Netsuite zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Netsuite** aus.

    ![Configure Single Sign-On](./media/netsuite-tutorial/tutorial_netsuite_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Um Ihre Einstellungen für das einmalige Anmelden zu testen, öffnen Sie den Zugriffsbereich unter [https://myapps.microsoft.com](https://myapps.microsoft.com/), melden Sie sich beim Testkonto an, und klicken Sie auf **Netsuite**.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Konfigurieren der Benutzerbereitstellung](netsuite-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/netsuite-tutorial/tutorial_general_01.png
[2]: ./media/netsuite-tutorial/tutorial_general_02.png
[3]: ./media/netsuite-tutorial/tutorial_general_03.png
[4]: ./media/netsuite-tutorial/tutorial_general_04.png

[100]: ./media/netsuite-tutorial/tutorial_general_100.png

[200]: ./media/netsuite-tutorial/tutorial_general_200.png
[201]: ./media/netsuite-tutorial/tutorial_general_201.png
[202]: ./media/netsuite-tutorial/tutorial_general_202.png
[203]: ./media/netsuite-tutorial/tutorial_general_203.png

