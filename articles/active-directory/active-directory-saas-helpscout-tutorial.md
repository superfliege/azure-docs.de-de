---
title: 'Tutorial: Azure Active Directory-Integration mit Help Scout | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Help Scout konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0aad9910-0bc1-4394-9f73-267cf39973ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/14/2017
ms.author: jeedes
ms.openlocfilehash: fe8775bd39173b2adf1f82d32f5e851ef1c19931
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a>Tutorial: Azure Active Directory-Integration mit Help Scout

In diesem Tutorial erfahren Sie, wie Sie Help Scout in Azure Active Directory (Azure AD) integrieren.

Die Integration von Help Scout in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Help Scout hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Help Scout anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Help Scout konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Help Scout-Abonnement, für das einmaliges Anmelden aktiviert ist

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Help Scout aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-help-scout-from-the-gallery"></a>Hinzufügen von Help Scout aus dem Katalog
Zum Konfigurieren der Integration von Help Scout in Azure AD müssen Sie Help Scout über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Help Scout aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Help Scout** ein, wählen Sie im Ergebnisbereich **Help Scout** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Help Scout in der Ergebnisliste](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Help Scout mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Help Scout als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Help Scout muss eine Linkbeziehung eingerichtet werden.

Für die Anmeldung bei Help Scout werden E-Mail-Adressen verwendet. Verwenden Sie daher zum Herstellen der Linkbeziehung die gleiche **E-Mail-Adresse** als **Benutzername** in Azure AD.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Help Scout müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Help Scout-Testbenutzers](#create-a-help-scout-test-user)**, um eine Entsprechung von Britta Simon in Help Scout zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Help Scout-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Help Scout die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Help Scout** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Help Scout** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Help Scout](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url.png)

    a. Der **Bezeichner** ist der **Zielgruppen-URI (ID der Dienstanbieterentität)** aus Help Scout und beginnt mit `urn:`.

    b. Die **Antwort-URL** ist die **Postback-URL (Assertionsverbraucherdienst-URL)** aus Help Scout und beginnt mit `https://`. 

    > [!NOTE] 
    > Die Werte in diesen URLs dienen nur Demonstrationszwecken. Aktualisieren Sie diese Werte mit der tatsächlichen Antwort-URL und dem tatsächlichen Bezeichner. Diese Werte finden Sie auf der Registerkarte **Einmaliges Anmelden** im Abschnitt „Authentifizierung“ (wird im weiteren Verlauf des Tutorials noch näher erläutert).

4. Wenn Sie die Anwendung im **SP**-initiierten Modus konfigurieren möchten, aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie den folgenden Schritt aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Help Scout](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** die URL folgendermaßen ein: `https://secure.helpscout.net/members/login/`.
     
5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-helpscout-tutorial/tutorial_general_400.png)


7. Klicken Sie im Abschnitt **Help Scout-Konfiguration** auf **Help Scout konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-helpscout-tutorial/config.png) 

8. Melden Sie sich in einem anderen Webbrowserfenster bei der Unternehmenswebsite von Help Scout als Administrator an.

9. Klicken Sie nach erfolgreicher Anmeldung im oberen Menü auf **Verwalten**, und wählen Sie anschließend im Dropdownmenü die Option **Unternehmen** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-helpscout-tutorial/settings1.png) 
 
10. Wählen Sie im linken Menü die Option **Authentifizierung** aus. 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-helpscout-tutorial/settings2.png) 

11. Dadurch gelangen Sie zum Abschnitt mit den SAML-Einstellungen. Führen Sie dort die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-helpscout-tutorial/settings3.png) 
 
    a. Kopieren Sie den Wert der **Postback-URL (Assertionsverbraucherdienst-URL)**, und fügen Sie ihn im Azure-Portal im Abschnitt **Domäne und URLs für Help Scout** in das Feld **Antwort-URL** ein.
    
    b. Kopieren Sie den Wert des **Zielgruppen-URIs (ID der Dienstanbieterentität)**, und fügen Sie ihn im Azure-Portal im Abschnitt **Domäne und URLs für Help Scout** in das Feld **Bezeichner** ein.

12. Aktivieren Sie **SAML aktivieren**, und führen Sie die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-helpscout-tutorial/settings4.png) 
 
    a. Fügen Sie die **Dienst-URL für einmalige Anmeldung**, die Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Single Sign-On URL** (URL für einmaliges Anmelden) ein.
    
    b. Klicken Sie auf **Zertifikat hochladen**, um das aus dem Azure-Portal heruntergeladene **Zertifikat (Base64)** hochzuladen.

    c. Geben Sie die E-Mail-Domäne(n) Ihrer Organisation (Beispiel: `contoso.com`) in das Textfeld **Email Domains** (E-Mail-Domänen) ein. Mehrere Domänen können jeweils durch ein Komma getrennt werden. Help Scout-Benutzer oder -Administratoren, die diese spezielle Domäne auf der [Anmeldeseite von Help Scout](https://secure.helpscout.net/members/login/) eingeben, werden zur Authentifizierung mit ihren Anmeldeinformationen an den Identitätsanbieter weitergeleitet.

    d. Abschließend können Sie noch **Force SAML Sign-on** (SAML-Anmeldung erzwingen) aktivieren, wenn sich Benutzer ausschließlich mit dieser Methode bei Help Scout anmelden sollen. Wenn die Benutzer weiterhin die Möglichkeit haben sollen, sich mit ihren Help Scout-Anmeldeinformationen anzumelden, lassen Sie die Einstellung deaktiviert. Auch bei aktivierter Einstellung kann sich ein Kontobesitzer jederzeit mit seinem Kontokennwort bei Help Scout anmelden.

    e. Klicken Sie auf **Speichern**.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-helpscout-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-helpscout-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-helpscout-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-helpscout-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-help-scout-test-user"></a>Erstellen eines Help Scout-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen einer Benutzerin namens Britta Simon in Help Scout. Help Scout unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Falls ein Benutzer nicht bereits in Help Scout vorhanden ist, wird beim Versuch, auf Help Scout zuzugreifen, ein neuer Benutzer erstellt.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Help Scout gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Help Scout zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Help Scout** aus.

    ![Help Scout-Link in der Anwendungsliste](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Help Scout“ klicken, sollten Sie automatisch bei Ihrer Help Scout-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_203.png

