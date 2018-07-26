---
title: 'Tutorial: Azure Active Directory-Integration mit NetSuite | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und NetSuite konfigurieren.
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
ms.date: 07/11/2018
ms.author: jeedes
ms.openlocfilehash: 5a20af1130d50209b29ad44195c14f30cba30c43
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051852"
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a>Tutorial: Azure Active Directory-Integration mit NetSuite

In diesem Tutorial erfahren Sie, wie Sie NetSuite in Azure Active Directory (Azure AD) integrieren.

Die Integration von NetSuite in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf NetSuite hat.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei NetSuite anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration mit NetSuite benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein NetSuite-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von NetSuite aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-netsuite-from-the-gallery"></a>Hinzufügen von NetSuite aus dem Katalog
Zum Konfigurieren der NetSuite-Integration in Azure AD müssen Sie NetSuite aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um NetSuite aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**.

    ![ANWENDUNGEN][3]

4. Geben Sie im Suchfeld **NetSuite** ein, wählen Sie im Ergebnisbereich **NetSuite** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![NetSuite in der Ergebnisliste](./media/netsuite-tutorial/tutorial_netsuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei NetSuite mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in NetSuite als Pendant für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in NetSuite muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in NetSuite zuweisen.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens mit Azure AD bei NetSuite die folgenden Bausteine aus:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. **[Erstellen eines NetSuite-Testbenutzers](#creating-a-netsuite-test-user)**, um ein Pendant von Britta Simon in NetSuite zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer NetSuite-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens mit Azure AD bei NetSuite die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **NetSuite** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Configure single sign-on](./media/NetSuite-tutorial/tutorial_NetSuite_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für NetSuite** die folgenden Schritte aus:

    ![Configure single sign-on](./media/NetSuite-tutorial/tutorial_NetSuite_url.png)

    Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein:

    `https://<tenant-name>.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.NetSuite.com/saml2/acs`

    `https://<tenant-name>.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.sandbox.NetSuite.com/saml2/acs`
    
    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Antwort-URL. Wenden Sie sich an den [NetSuite-Support](http://www.NetSuite.com/portal/services/support.shtml), um diese Werte zu erhalten.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Configure single sign-on](./media/NetSuite-tutorial/tutorial_NetSuite_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/NetSuite-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **NetSuite-Konfiguration** auf **NetSuite konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Configure single sign-on](./media/NetSuite-tutorial/tutorial_NetSuite_configure.png)

7. Öffnen Sie eine neue Registerkarte in Ihrem Browser, und melden Sie sich bei Ihrer NetSuite-Unternehmenswebsite als Administrator an.

8. Klicken Sie auf der Symbolleiste am oberen Rand der Seite auf **Setup**, navigieren Sie zu **Unternehmen**, und klicken Sie auf **Features aktivieren**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-setupsaml.png)

9. Klicken Sie auf der Symbolleiste in der Mitte der Seite auf **SuiteCloud**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-suitecloud.png)

10. Klicken Sie im Abschnitt **Authentifizierung verwalten** auf **Einmaliges Anmelden für SAML**, um die Option „Einmaliges Anmelden für SAML“ in NetSuite zu aktivieren.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-ticksaml.png)

11. Klicken Sie auf der Symbolleiste am oberen Rand der Seite auf **Setup**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-setup.png)

12. Klicken Sie in der Liste **SETUPTASKS** auf **Integration**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-integration.png)

13. Klicken Sie im Abschnitt **Authentifizierung verwalten** auf **Einmaliges Anmelden für SAML**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-saml.png)

14. Führen Sie auf der Seite **SAML-Setup** unter **NetSuite-Konfiguration** die folgenden Schritte aus:

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-saml-setup.png)
  
    a. Wählen Sie **Primäre Authentifizierungsmethode** aus.

    b. Wählen Sie im Feld mit der Bezeichnung **SAMLV2-IDENTITÄTSANBIETERMETADATEN** die Option **IDP-METADATENDATEI HOCHLADEN** aus. Klicken Sie dann zum Hochladen der Metadatendatei, die Sie aus dem Azure-Portal heruntergeladen haben, auf **Durchsuchen** .

    c. Klicken Sie auf **Submit**.

15. Aktivieren Sie in Azure AD das Kontrollkästchen **Alle weiteren Benutzerattribute anzeigen und bearbeiten**, und fügen Sie ein Attribut hinzu.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-attributes.png)

16. Geben Sie im Feld **Attributname** die Zeichenfolge `account` ein. Geben Sie in das Feld **Attributwert** Ihre Konto-ID für NetSuite ein. Dieser Wert ist konstant und vom Konto abhängig. Beachten Sie die folgenden Anleitungen, um Ihre Konto-ID zu finden:

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-add-attribute.png)

    a. Klicken Sie in NetSuite auf **Setup**, navigieren Sie zu **Unternehmen**, und klicken Sie im oberen Navigationsmenü auf **Unternehmensinformationen**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-com.png)

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-account-id.png)

    b. Kopieren Sie auf der Seite **Unternehmensinformationen** die **Konto-ID** in der rechten Spalte.

    c. Fügen Sie die **Konto-ID**, die Sie im NetSuite-Konto kopiert haben, in das Feld **Attributwert** in Azure AD ein. 

17. Bevor Benutzer die einmalige Anmeldung in NetSuite ausführen können, müssen ihnen zunächst die entsprechenden Berechtigungen in NetSuite zugewiesen werden. Befolgen Sie die folgenden Anweisungen, um diese Berechtigungen zuzuweisen.

    a. Klicken Sie im oberen Navigationsmenü auf **Setup**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-setup.png)

    b. Wählen Sie im linken Navigationsmenü **Benutzer/Rollen** aus, und klicken Sie auf **Rollen verwalten**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-manage-roles.png)

    c. Klicken Sie auf **Neue Rolle**.

    d. Geben Sie einen **Namen** für die neue Rolle ein.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-new-role.png)

    e. Klicken Sie auf **Speichern**.

    f. Klicken Sie im oberen Menü auf **Berechtigungen**. Klicken Sie dann auf **Setup**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-sso.png)

    g. Wählen Sie **Einmaliges Anmelden für SAML** aus, und klicken Sie dann auf **Hinzufügen**.

    h. Klicken Sie auf **Speichern**.

    i. Klicken Sie im oberen Navigationsmenü auf **Setup** und dann auf **Setup-Manager**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-setup.png)

    j. Wählen Sie im linken Navigationsmenü **Benutzer/Rollen** aus, und klicken Sie auf **Benutzer verwalten**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-manage-users.png)

    k. Wählen Sie einen Testbenutzer. Klicken Sie auf **Bearbeiten**, und navigieren Sie anschließend zur Registerkarte **Zugriff**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-edit-user.png)

    l. Weisen Sie im Dialogfeld „Rollen“ die entsprechende Rolle zu, die Sie erstellt haben.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-add-role.png)

    m. Klicken Sie auf **Speichern**.
 
### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/NetSuite-tutorial/create_aaduser_01.png) 

2.  Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/NetSuite-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/NetSuite-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/NetSuite-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**. 

### <a name="creating-a-netsuite-test-user"></a>Erstellen eines NetSuite-Testbenutzers

In diesem Abschnitt wird in NetSuite eine Benutzerin namens Britta Simon erstellt. NetSuite unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.
Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Falls ein Benutzer nicht bereits in NetSuite vorhanden ist, wird beim Versuch, auf NetSuite zuzugreifen, ein neuer Benutzer erstellt.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens mit Azure, indem Sie ihr Zugriff auf NetSuite gewähren.

![Benutzer zuweisen][200] 

**Führen Sie zum Zuweisen von Britta Simon zu NetSuite die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **NetSuite** aus.

    ![Configure single sign-on](./media/NetSuite-tutorial/tutorial_NetSuite_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Öffnen Sie zum Testen Ihrer Einstellungen für das einmalige Anmelden den Zugriffsbereich unter [https://myapps.microsoft.com](https://myapps.microsoft.com/), melden Sie sich beim Testkonto an, und klicken Sie auf **NetSuite**.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Konfigurieren der Benutzerbereitstellung](NetSuite-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/NetSuite-tutorial/tutorial_general_01.png
[2]: ./media/NetSuite-tutorial/tutorial_general_02.png
[3]: ./media/NetSuite-tutorial/tutorial_general_03.png
[4]: ./media/NetSuite-tutorial/tutorial_general_04.png

[100]: ./media/NetSuite-tutorial/tutorial_general_100.png

[200]: ./media/NetSuite-tutorial/tutorial_general_200.png
[201]: ./media/NetSuite-tutorial/tutorial_general_201.png
[202]: ./media/NetSuite-tutorial/tutorial_general_202.png
[203]: ./media/NetSuite-tutorial/tutorial_general_203.png

