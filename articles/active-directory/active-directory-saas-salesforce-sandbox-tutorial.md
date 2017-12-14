---
title: 'Tutorial: Azure Active Directory-Integration mit Salesforce Sandbox | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Salesforce Sandbox konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: jeedes
ms.openlocfilehash: 16b60342106c69cb9959aad539b794cb8003acda
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Tutorial: Azure Active Directory-Integration mit Salesforce Sandbox

In diesem Tutorial erfahren Sie, wie Sie Salesforce Sandbox in Azure Active Directory (Azure AD) integrieren.

Die Integration von Salesforce Sandbox in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Salesforce Sandbox hat.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Salesforce Sandbox anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in Salesforce Sandbox konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Salesforce Sandbox-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Salesforce Sandbox aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Hinzufügen von Salesforce Sandbox aus dem Katalog
Zum Konfigurieren der Integration von Salesforce Sandbox in Azure AD müssen Sie Salesforce Sandbox aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um Salesforce Sandbox aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie in das Suchfeld den Namen **Salesforce Sandbox** ein, wählen Sie **Salesforce Sandbox** im Ergebnisbereich aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Salesforce Sandbox in der Ergebnisliste](./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Salesforce Sandbox mithilfe einer Testbenutzerin namens „Britta Simon“.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Salesforce Sandbox als Pendant für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Salesforce Sandbox muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Salesforce Sandbox den Wert für **Benutzername** aus Azure AD als Wert für **Benutzername** zu, um die Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Salesforce Sandbox müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Salesforce Sandbox-Testbenutzers](#create-a-salesforce-sandbox-test-user)**, um ein Pendant von Britta Simon in Salesforce Sandbox zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Salesforce Sandbox-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD in Salesforce Sandbox zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Salesforce Sandbox** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für Salesforce Sandbox** aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** den Wert im folgenden Format ein: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`.

    b. Geben Sie im Textfeld **Bezeichner** einen Wert nach folgendem Muster ein: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`
    
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Kundensupportteam von Salesforce](https://help.salesforce.com/support), um diese Werte zu erhalten.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Salesforce Sandbox-Konfiguration** auf **Salesforce Sandbox konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_salesforcesandbox_configure.png) 

7. Öffnen Sie eine neue Registerkarte in Ihrem Browser, und melden Sie sich mit Ihrem Salesforce Sandbox-Administratorkonto an.

8. Klicken Sie in der oberen rechten Ecke der Seite unter dem **Einstellungssymbol** auf **Setup**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-sandbox-tutorial/configure1.png)

9. Scrollen Sie im Navigationsbereich nach unten bis zu den **SETTINGS** (EINSTELLUNGEN), und klicken Sie auf **Identity** (Identität), um den zugehörigen Bereich zu erweitern. Klicken Sie dann auf **Einstellungen für einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-sandbox-tutorial/sf-admin-sso.png)

10. Wählen Sie **SAML aktiviert**, und klicken Sie dann auf **Speichern**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-sandbox-tutorial/sf-enable-saml.png)

11. Um Ihre SAML-Einstellungen für die einmalige Anmeldung zu konfigurieren, klicken Sie auf **Neu**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-sandbox-tutorial/sf-admin-sso-new.png)

12. Führen Sie im Abschnitt „Einstellungen für einmalige Anmelden für SAML“ die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-sandbox-tutorial/sf-saml-config.png)

    a. Geben Sie in das Textfeld **Name** den Namen der Konfiguration ein (z.B. *SPSSOWAAD_Test*). 

    b. Fügen Sie in das Feld **Aussteller** den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Geben Sie in das Textfeld **Entitäts-ID** die Zeichenfolge `https://<instancename>--Sandbox.<entityid>.my.salesforce.com` ein, wenn dies die erste Salesforce Sandbox-Instanz ist, die Sie Ihrem Verzeichnis hinzufügen. Wenn Sie bereits eine Instanz der Salesforce Sandbox hinzugefügt haben, geben Sie als **Entitäts-ID** die **Anmelde-URL** im folgenden Format ein: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`  
 
    d. Um das **Zertifikat des Identitätsanbieters** hochzuladen, klicken Sie auf **Choose File** (Datei auswählen). Navigieren Sie dann zu der Zertifikatdatei, die Sie aus dem Azure-Portal heruntergeladen haben, und wählen Sie diese aus.  

    e. Wählen Sie als **SAML Identity Type** (SAML-Identitätstyp) eine der folgenden Optionen aus:
    
      * Wählen Sie **Assertion contains the User's Salesforce username** (Assertion enthält den Salesforce-Benutzernamen des Benutzers) aus, wenn in der SAML-Assertion der Salesforce-Benutzername des Benutzers übergeben wird.

      * Wählen Sie **Assertion contains the Federation ID from the User object** (Assertion enthält die Verbund-ID aus dem Benutzerobjekt) aus, wenn die Verbund-ID aus dem Benutzerobjekt in der SAML-Assertion übergeben wird.

      * Wählen Sie **Assertion contains the User ID from the User object** (Assertion enthält die Benutzer-ID aus dem Benutzerobjekt) aus, wenn die Benutzer-ID aus dem Benutzerobjekt in der SAML-Assertion übergeben wird.
 
    f. Wählen Sie für **SAML Identity Location** (Speicherort der SAML-Identität) die Option **Identity is in the NameIdentifier element of the Subject statement** (Identität ist im NameIdentifier-Element der Subject-Anweisung enthalten) aus.

    g. Wählen Sie für **Service Provider Initiated Request Binding** (Vom Dienstanbieter initiierte Anforderungsbindung) die Option **HTTP Post** aus. 

    h. Fügen Sie in das Textfeld **Anmelde-URL des Identitätsanbieters** den Wert der **Dienst-URL für einmalige Anmeldung** ein, den Sie aus dem Azure-Portal kopiert haben. 

    i. SFDC unterstützt die SAML-Abmeldung nicht.  Fügen Sie als Problemumgehung den Link `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0` in das Textfeld **Abmelde-URL des Identitätsanbieters** ein.

    j. Klicken Sie auf **Speichern**.

### <a name="enable-your-domain"></a>Aktivieren Ihrer Domäne
In diesem Abschnitt wird davon ausgegangen, dass Sie bereits eine Domäne erstellt haben.  Weitere Informationen finden Sie unter [Defining Your Domain Name](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US) (Festlegen des Domänennamens).

**Führen Sie zum Aktivieren Ihrer Domäne die folgenden Schritte aus:**

1. Klicken Sie in Salesforce im linken Navigationsbereich auf **Company Settings** (Unternehmenseinstellungen), um den zugehörigen Abschnitt zu erweitern, und klicken Sie dann auf **My Domain** (Meine Domäne).
   
     ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-sandbox-tutorial/sf-my-domain.png)
   
   >[!NOTE]
   >Stellen Sie sicher, dass Ihre Domäne richtig konfiguriert wurde. 

2. Klicken Sie im Abschnitt **Authentication Configuration** (Authentifizierungskonfiguration) auf **Bearbeiten**, und wählen Sie dann als **Authentifizierungsdienst** den Namen der SAML-Einstellung zum einfachen Anmelden aus dem vorherigen Abschnitt aus. Klicken Sie abschließend auf **Speichern**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-sandbox-tutorial/sf-edit-auth-config.png)

Sobald Sie eine Domäne konfiguriert haben, sollten Ihre Benutzer die Domänen-URL für die Anmeldung der Salesforce Sandbox verwenden.  

Wenn den Wert der URL abrufen möchten, klicken Sie auf das SSO-Profil, das Sie im vorherigen Abschnitt erstellt haben.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-salesforce-sandbox-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-salesforce-sandbox-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-salesforce-sandbox-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-salesforce-sandbox-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-salesforce-sandbox-test-user"></a>Erstellen eines Salesforce Sandbox-Testbenutzers

In diesem Abschnitt wird ein Benutzer mit dem Namen Britta Simon in Salesforce Sandbox erstellt. Salesforce Sandbox unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.
Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Falls ein Benutzer nicht bereits in Salesforce Sandbox vorhanden ist, wird beim Versuch, auf Salesforce Sandbox zuzugreifen, ein neuer Benutzer erstellt.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Salesforce Sandbox gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Salesforce Sandbox zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste der Anwendungen **Salesforce Sandbox** aus.

    ![Der Salesforce Sandbox-Link in der Liste der Anwendungen](./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_salesforcesandbox_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Salesforce Sandbox“ klicken, sollten Sie automatisch bei Ihrer Salesforce Sandbox-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_203.png

