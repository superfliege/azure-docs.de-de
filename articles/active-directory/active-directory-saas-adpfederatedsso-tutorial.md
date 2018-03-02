---
title: 'Tutorial: Azure Active Directory-Integration in ADP Federated SSO | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und ADP Federated SSO konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7be5331b-0481-48f7-9d6b-619dfec657e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: jeedes
<<<<<<< HEAD
ms.openlocfilehash: ad12dfd525afe1bde7026535dceb25556abf0a96
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2018
=======
ms.openlocfilehash: 76bbee514eac36aec8eaf84e8a20f85a27559fc7
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2018
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
---
# <a name="tutorial-azure-active-directory-integration-with-adp-federated-sso"></a>Tutorial: Azure Active Directory-Integration in ADP Federated SSO

In diesem Tutorial erfahren Sie, wie Sie Azure Active Directory (Azure AD) in ADP Federated SSO integrieren.

Die Integration von ADP Federated SSO in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf ADP Federated SSO hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei ADP Federated SSO anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in ADP Federated SSO konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein für ADP Federated SSO aktiviertes Abonnement

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von ADP Federated SSO aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-adp-federated-sso-from-the-gallery"></a>Hinzufügen von ADP Federated SSO aus dem Katalog
Zum Konfigurieren der Integration von ADP Federated SSO in Azure AD müssen Sie ADP Federated SSO aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um ADP Federated SSO aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1.  Melden Sie sich bei Ihrer Microsoft Azure-Identitätsanbieterumgebung als Administrator an.

2. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

3. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
4. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

5. Geben Sie im Suchfeld **ADP Federated SSO** ein, wählen Sie im Ergebnisbereich **ADP Federated SSO** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![ADP Federated SSO in der Ergebnisliste](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden über Azure AD bei ADP Federated SSO mithilfe eines Testbenutzers namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in ADP Federated SSO als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in ADP Federated SSO muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in ADP Federated SSO den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens mit Azure AD bei ADP Federated SSO müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines ADP Federated SSO-Testbenutzers](#create-an-adp-federated-sso-test-user)**, um in ADP Federated SSO eine Entsprechung von Britta Simon zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer ADP Federated SSO-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens bei ADP Federated SSO über Azure AD die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **ADP Federated SSO** auf die Registerkarte **Eigenschaften**, und führen Sie die folgenden Schritte aus: 

    ![Eigenschaften für das einmalige Anmelden](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_prop.png)

    a. Legen Sie den Wert im Feld **Aktiviert für die Benutzeranmeldung** auf **Ja** fest.

    b. Kopieren Sie die **URL für den Benutzerzugriff**. Diese muss im Abschnitt **Anmelde-URL konfigurieren** eingefügt werden, dies wird später in diesem Tutorial erläutert.

    c. Legen Sie das Feld **Benutzerzuweisung erforderlich** auf **Ja** fest.

    d. Legen Sie das Feld **Für Benutzer sichtbar** auf **Nein** fest.

2. Klicken Sie auf der Anwendungsintegrationsseite für **ADP Federated SSO** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

3. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_samlbase.png)

4. Führen Sie auf der Seite **Domäne und URLs für ADP Federated SSO** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für ADP Federated SSO](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_url.png)

    Geben Sie im Textfeld **Bezeichner** eine URL ein: `https://fed.adp.com/` 
    
5. Die ADP Federated SSO-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: Der Anspruchsname ist immer eine Kombination aus **PersonImmutableID** und dem Wert, den Sie **employeeid** zugeordnet haben. 

    Hier erfolgt die Benutzerzuordnung von Azure AD zu ADP Federated SSO auf Grundlage der **employeeid**, aber je nach Anwendungseinstellungen ist auch eine Zuordnung zu einem anderen Wert möglich. Wenden Sie sich also zunächst an das [ADP-Supportteam](https://www.adp.com/contact-us/overview.aspx), um den korrekten Bezeichner eines Benutzers zu verwenden, und ordnen Sie diesen Wert dann dem Anspruch **PersonImmutableID** zu.

    ![Configure Single Sign-On](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_attribute.png)

6. Konfigurieren Sie das SAML-Tokenattribut im Abschnitt **Benutzerattribute** im Dialogfeld **Einmaliges Anmelden**, wie in der Abbildung gezeigt, und führen Sie die folgenden Schritte aus:
    
    | Attributname | Attributwert |
    | ------------------- | -------------------- |    
    | PersonImmutableID | user.employeeid |
    
    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Configure Single Sign-On](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_attribute_04.png)

    ![Configure Single Sign-On](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_attribute_05.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.
    
    d. Klicken Sie auf **OK**.

    > [!NOTE] 
    > Bevor Sie die SAML-Assertion konfigurieren können, müssen Sie sich an Ihr [ADP-Supportteam](https://www.adp.com/contact-us/overview.aspx) wenden und für Ihren Mandanten den Wert des Attributs für den eindeutigen Bezeichner anfordern. Sie benötigen diesen Wert, um den benutzerdefinierten Anspruch für Ihre Anwendung zu konfigurieren. 

7. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_certificate.png) 

8. Um das einmalige Anmelden für **ADP Federated SSO** zu konfigurieren, müssen Sie die heruntergeladene **Metadaten-XML** auf die [ADP Federated SSO-Website](https://adpfedsso.adp.com/public/login/index.fcc) hochladen.

> [!NOTE]  
> Dieser Vorgang kann einige Tage in Anspruch nehmen. 

### <a name="configure-your-adp-services-for-federated-access"></a>Konfigurieren Ihrer ADP-Dienste für den Verbundzugriff

>[!Important]
> Mitarbeiter, die Verbundzugriff auf Ihre ADP-Dienste benötigen, müssen der ADP-Dienst-App zugewiesen und anschließend dem jeweiligen ADP-Dienst erneut zugewiesen werden.
Nach der Bestätigung durch Ihren ADP-Vertriebsbeauftragten konfigurieren Sie Ihre ADP-Dienste und weisen Benutzer zum Steuern des Benutzerzugriffs für die jeweiligen ADP-Dienste zu bzw. verwalten diese.

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **ADP Federated SSO** ein, wählen Sie im Ergebnisbereich **ADP Federated SSO** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![ADP Federated SSO in der Ergebnisliste](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_addservicegallery.png)

5. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **ADP Federated SSO** auf die Registerkarte **Eigenschaften**, und führen Sie die folgenden Schritte aus:  

    ![Verknüpfte Eigenschaften für einmaliges Anmelden](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_linkedproperties.png)

    a.  Legen Sie den Wert im Feld **Aktiviert für die Benutzeranmeldung** auf **Ja** fest.

    b.  Legen Sie das Feld **Benutzerzuweisung erforderlich** auf **Ja** fest.

    c.  Legen Sie das Feld **Für Benutzer sichtbar** auf **Ja** fest.

6. Klicken Sie auf der Anwendungsintegrationsseite für **ADP Federated SSO** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

7. Wählen Sie im Dialogfeld **Einmaliges Anmelden** unter **Modus** die Option **Anmeldung über Link**, um Ihre Anwendung mit **ADP Federated SSO** zu verknüpfen.
<<<<<<< HEAD
=======
<!---Loc Comment:Sentence "to link your application to ADP Federated SSO." should be concatenated with the previous sentence--->
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794

    ![Single sign-on linked](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_linked.png)

8. Navigieren Sie zum Abschnitt **Anmelde-URL konfigurieren**, und führen Sie die folgenden Schritte aus:

    ![Eigenschaften für einmaliges Anmelden](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_linkedsignon.png)
                                                              
    a. Fügen Sie die **URL für den Benutzerzugriff** ein, die Sie oben aus der Registerkarte **Eigenschaften** (in der ADP Federated SSO-App) kopiert haben.
                                                             
    b. Nachfolgend werden die 5 Apps aufgeführt, die unterschiedliche **Relayzustand-URLs** unterstützen. Sie müssen die geeignete **Relayzustand-URL** für die jeweilige Anwendung manuell an die **URL für den Benutzerzugriff** anfügen.
    
    * **ADP Workforce Now**
        
        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?WFN`

    * **ADP Workforce Now Enhanced Time**
        
        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?EETDC2`
    
    * **ADP Vantage HCM**
        
        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?ADPVANTAGE`

    * **ADP Enterprise HR**

        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?PORTAL`

    * **MyADP**

        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?REDBOX`

9. **Speichern** Sie die Änderungen.

10. Beginnen Sie nach Bestätigung durch Ihren ADP-Vertriebsbeauftragten mit dem Test für einen oder zwei Benutzer.

    a. Weisen Sie der ADP-Dienst-App einige Benutzer zu, um den Verbundzugriff zu testen.

    b. Der Test ist erfolgreich, wenn die Benutzer auf die ADP-Dienst-App im Katalog und auf ihren ADP-Dienst zugreifen können.
 
11. Wenn der Test erfolgreich war, weisen Sie den ADP-Verbunddienst einzelnen Benutzern oder Benutzergruppen zu, und führen Sie ein Rollout für Ihre Mitarbeiter durch. Dies wird später in diesem Tutorial erläutert. 

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-an-adp-federated-sso-test-user"></a>Erstellen eines ADP Federated SSO-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in ADP Federated SSO. Wenden Sie sich an das [ADP-Supportteam](https://www.adp.com/contact-us/overview.aspx), um die Benutzer unter dem ADP Federated SSO-Konto hinzufügen zu lassen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf ADP Federated SSO gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon zu ADP Federated SSO zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **ADP Federated SSO** aus.

    ![ADP Federated SSO-Link in der Anwendungsliste](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „ADP Federated SSO“ klicken, sollten Sie automatisch bei Ihrer ADP Federated SSO-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_203.png

