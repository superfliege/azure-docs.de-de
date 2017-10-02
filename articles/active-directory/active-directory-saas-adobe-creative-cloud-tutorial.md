---
title: 'Tutorial: Azure Active Directory-Integration mit Adobe Creative Cloud | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Adobe Creative Cloud konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c199073f-02ce-45c2-b515-8285d4bbbca2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 1868e5fd0427a5e1b1eeed244c80a570a39eb6a9
ms.openlocfilehash: ad2c3999ac8bfc8dfe0fc662ee37b41fff38444f
ms.contentlocale: de-de
ms.lasthandoff: 09/19/2017

---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a>Tutorial: Azure Active Directory-Integration mit Adobe Creative Cloud

In diesem Tutorial erfahren Sie, wie Sie Adobe Creative Cloud in Azure Active Directory (Azure AD) integrieren.

Die Integration von Adobe Creative Cloud in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf Adobe Creative Cloud Zugriff besitzt.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Adobe Creative Cloud anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Adobe Creative Cloud konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges Adobe Creative Cloud-Abonnement

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Adobe Creative Cloud aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-adobe-creative-cloud-from-the-gallery"></a>Hinzufügen von Adobe Creative Cloud aus dem Katalog
Zum Konfigurieren der Integration von Adobe Creative Cloud in Azure AD müssen Sie Adobe Creative Cloud aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Adobe Creative Cloud aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Adobe Creative Cloud** ein, wählen Sie im Ergebnisbereich **Adobe Creative Cloud** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Adobe Creative Cloud in der Ergebnisliste](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD bei Adobe Creative Cloud.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Adobe Creative Cloud als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Adobe Creative Cloud muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Adobe Creative Cloud den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Adobe Creative Cloud müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Adobe Creative Cloud-Testbenutzers](#create-an-adobe-creative-cloud-test-user)**, um eine Entsprechung von Britta Simon in Adobe Creative Cloud zu erhalten, die mit der Darstellung des Benutzers in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Adobe Creative Cloud-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Adobe Creative Cloud die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Adobe Creative Cloud** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Adobe Creative Cloud** die folgenden Schritte aus, wenn Sie die Anwendung im IDP-initiierten Modus konfigurieren möchten:

    ![Informationen zu einmaligem Anmelden für Adobe Creative Cloud-Domäne und -URLs](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_url.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://www.okta.com/saml2/service-provider/<token>`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<company name>.okta.com/auth/saml20/accauthlinktest`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Wenden Sie sich an das [Supportteam von Adobe Creative Cloud](https://helpx.adobe.com/in/contact/support.html), um diese Werte zu erhalten. 

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![Informationen zu einmaligem Anmelden für Adobe Creative Cloud-Domäne und -URLs](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_url2.png)

    Geben Sie im Textfeld **Anmelde-URL** den Wert wie folgt ein: `https://adobe.com`

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_certificate.png)
     
6. Die Adobe Creative Cloud-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute auf der Registerkarte **Benutzerattribute** der Anwendung verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_attribute.png)

7. Konfigurieren Sie das SAML-Tokenattribut im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute**, wie im obigen Bild gezeigt, und führen Sie die folgenden Schritte aus:

    | Attributname | Attributwert |
    | ---------------| ----------------|
    | Vorname |user.givenname |
    | Nachname |user.surname |
    | E-Mail |user.mail |

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_attribute_04.png)
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_attribute_05.png)
    
    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.
    
    d. Klicken Sie auf **OK**.
    
8. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_400.png)
    
9. Klicken Sie im Abschnitt **Adobe Creative Cloud-Konfiguration** auf **Adobe Creative Cloud konfigurieren**, um das Fenster **Anmelden konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Adobe Creative Cloud-Konfiguration](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_configure.png)
    
10. Melden Sie sich in einem anderen Webbrowserfenster bei Ihrem Adobe Creative Cloud-Mandanten als Administrator an.

11. Wechseln Sie im linken Navigationsbereich zu **Identität**, und klicken Sie auf Ihre Domäne. Führen Sie im Abschnitt **SSO-Konfiguration erforderlich** die folgenden Schritte aus.

    ![Einstellungen](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "Einstellungen")
    
    a. Klicken Sie auf **Durchsuchen**, um das aus Azure AD heruntergeladene Zertifikat in das **IDP-Zertifikat** hochzuladen.
    
    b. Fügen Sie im Textfeld **IDP-Aussteller** den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Abschnitt **Anmelden konfigurieren** im Azure-Portal kopiert haben.
    
    c. Fügen Sie im Textfeld **IDP-Anmelde-URL** den Wert der **SAML-SSO-Dienst-URL** ein, den Sie aus dem Abschnitt **Anmelden konfigurieren** im Azure-Portal kopiert haben.
    
    d. Wählen Sie **HTTP-Umleitung** als **IDP-Bindung** aus.
    
    e. Wählen Sie **E-Mail-Adresse** als **Einstellung für die Benutzeranmeldung** aus.
    
    f. Klicken Sie auf die Schaltfläche **Save** .

12. Im Dashboard wird jetzt die XML-Datei **Metadaten herunterladen** angezeigt. Sie enthält die EntityDescriptor-URL und die AssertionConsumerService-URL von Adobe. Öffnen Sie die Datei, und konfigurieren sie diese in der Azure AD-Anwendung.

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_002.png)

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    a. Verwenden Sie den von Adobe bereitgestellten EntityDescriptor-Wert für **Bezeichner** im Dialogfeld **App-Einstellungen konfigurieren**.

    b. Verwenden Sie den von Adobe bereitgestellten AssertionConsumerService-Wert für **Antwort-URL** im Dialogfeld **App-Einstellungen konfigurieren**.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-an-adobe-creative-cloud-test-user"></a>Erstellen eines Adobe Creative Cloud-Testbenutzers

Damit sich Azure AD-Benutzer bei Adobe Creative Cloud anmelden können, müssen sie in Adobe Creative Cloud bereitgestellt werden. Im Fall von Adobe Creative Cloud ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:

1. Melden Sie sich bei der Adobe Creative Cloud-Unternehmenswebsite als Administrator an.

2. Hinzufügen des Benutzers in der Adobe-Konsole als Verbund-ID und Zuweisen zu einer Gruppe für die Berechtigung

    ![Der Adobe Creative Cloud-Link in der Anwendungsliste](./media/active-directory-saas-adobe-creative-cloud-tutorial/users.png)  

3. Geben Sie nun Ihre E-Mail-Adresse und Ihren UPN in das Adobe-Anmeldeformular ein, und drücken Sie die TAB-TASTE. Sie sollten zurück zum Azure AD-Verbund gelangen:
    * Webzugriff: www.adobe.com > sign-in
    * Im Desktop-App-Hilfsprogramm > sign-in
    * In der Anwendung > help > sign-in

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie Zugriff auf Adobe Creative Cloud gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Adobe Creative Cloud zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Adobe Creative Cloud** aus.

    ![Der Adobe Creative Cloud-Link in der Anwendungsliste](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Adobe Creative Cloud“ klicken, werden Sie automatisch bei Ihrer Adobe Creative Cloud-Anwendung angemeldet.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_203.png


