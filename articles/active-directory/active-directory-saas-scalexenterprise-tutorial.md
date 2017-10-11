---
title: 'Lernprogramm: Azure Active Directory-Integration mit ScaleX Enterprise | Microsoft Docs'
description: Informationen Sie zum Konfigurieren von einmaliges Anmelden zwischen Azure Active Directory und ScaleX Enterprise.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: 0ebed0c2605862426384c0e219e52c9d626b6246
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scalex-enterprise"></a>Lernprogramm: Azure Active Directory-Integration mit ScaleX Enterprise

In diesem Lernprogramm erfahren Sie, wie ScaleX Enterprise mit Azure Active Directory (Azure AD) integrieren.

Integrieren von ScaleX Enterprise mit Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf ScaleX Enterprise hat
- Sie können Ihre Benutzer für die automatisch signiert und ScaleX Unternehmen (Single Sign-On) bei Abrufen mit ihren Azure AD-Konten aktivieren.
- Sie können Ihre Konten an einem zentralen Ort - Azure-Portal verwalten.

Wenn Sie weitere Informationen zum Integrieren von SaaS-Apps in Azure AD wissen möchten, finden Sie unter. Was ist Anwendungszugriff und einmaliges Anmelden mit [Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren von Azure AD-Integration mit ScaleX Unternehmen benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Eine ScaleX Enterprise Single Sign-On Abonnement mit aktivierter

> [!NOTE]
> Um die Schritte in diesem Lernprogramm zu testen, führen Sie wird nicht empfohlen, mithilfe einer produktiven Umgebung.

Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlungen beachten:

- Verwenden Sie nicht Ihre produktionsumgebung, wenn dies erforderlich ist.
- Wenn Sie nicht über eine Testversion Azure Active Directory-Umgebung verfügen, erhalten Sie eine einmonatige Testversion [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Lernprogramm testen Sie Azure AD einmaliges Anmelden in einer testumgebung. In diesem Lernprogramm beschriebene Szenario besteht aus zwei Hauptbausteinen:

1. ScaleX Enterprise aus dem Katalog hinzufügen
2. Konfigurieren und Testen von Azure AD-einmaliges Anmelden

## <a name="adding-scalex-enterprise-from-the-gallery"></a>ScaleX Enterprise aus dem Katalog hinzufügen
Um die Integration von ScaleX Unternehmen in Azure AD zu konfigurieren, müssen Sie ScaleX Enterprise der Liste der verwalteten SaaS-apps aus dem Katalog hinzufügen.

**Um ScaleX Enterprise aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. In der  **[Azure-Portal](https://portal.azure.com)**, klicken Sie auf den linken Navigationsbereich auf **Azure Active Directory** Symbol. 

    ![Active Directory][1]

2. Navigieren Sie zu **unternehmensanwendungen**. Klicken Sie dann auf **alle Anwendungen**.

    ![Applications][2]
    
3. Klicken Sie auf **hinzufügen** Schaltfläche oben im Dialogfeld auf.

    ![Applications][3]

4. Geben Sie in das Suchfeld **ScaleX Enterprise**.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_search.png)

5. Wählen Sie im Ergebnisbereich, **ScaleX Enterprise**, und klicken Sie dann auf **hinzufügen** Schaltfläche, um die Anwendung hinzuzufügen.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD-einmaliges Anmelden
In diesem Abschnitt Konfigurieren Sie, und Tests einmaliges Anmelden für Azure AD mit ScaleX Enterprise auf Grundlage eines Testbenutzers namens "Britta Simon."

Für einmaliges Anmelden funktioniert muss Azure AD zu wissen, was der Benutzer Gegenstück in Unternehmen ScaleX für einen Benutzer in Azure AD ist. Das heißt, muss eine linkbeziehung zwischen Azure AD-Benutzers und die entsprechenden Benutzer in ScaleX Unternehmen hergestellt werden.

Dieser Link wird Beziehung durch Zuweisen des Werts von der **Benutzername** in Azure AD als Wert des der **Benutzername** in ScaleX Unternehmen.

Zum Konfigurieren und Testen Sie Azure AD einmaliges Anmelden mit ScaleX Enterprise, müssen Sie den folgenden Bausteinen ausführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  – Informationen zum Aktivieren Ihrer Benutzer dieses Feature verwenden.
2. **[Erstellen ein Azure AD-Testbenutzer](#creating-an-azure-ad-test-user)**  – Informationen zum Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen eines Testbenutzers ScaleX Enterprise](#creating-a-scalex-enterprise-test-user)**  – um eine Entsprechung Britta Simon in ScaleX Unternehmen verfügen, die mit der Azure AD-Darstellung eines Benutzers verknüpft ist.
4. **[Zuweisen von Azure AD-Testbenutzer](#assigning-the-azure-ad-test-user)**  – zum Aktivieren der Britta Simon einmaliges Anmelden für Azure AD verwenden.
5. **[Testen Sie einmaliges Anmelden für](#testing-single-sign-on)**  – zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

In diesem Abschnitt werden Azure AD einmaliges Anmelden im Azure-Portal aktivieren und konfigurieren Sie einmaliges Anmelden in Ihrer Anwendung ScaleX Enterprise.

**Führen Sie zum Konfigurieren von Azure AD einmaliges Anmelden mit ScaleX Enterprise die folgenden Schritte aus:**

1. Im Azure-Portal auf der **ScaleX Enterprise** anwendungsintegrationsseite, klicken Sie auf **des einmaligen Anmeldens**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Auf der **des einmaligen Anmeldens** Dialogfeld als **Modus** wählen **SAML-basierten anmelden** einmaliges Anmelden aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_samlbase.png)

3. Auf der **ScaleX Unternehmensdomäne und URLs** Abschnitt, führen Sie die folgenden Schritte aus, wenn Sie möchten, konfigurieren Sie die Anwendung in **IDP** initiiertem:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url1.png)

    a. In der **Bezeichner** Textbox, geben Sie den Wert mithilfe des folgenden Musters:`https://platform.rescale.com/saml2/<company id>/`

    b. In der **-Antwort-URL** Textfeld eine URL mithilfe des folgenden Musters:`https://platform.rescale.com/saml2/<company id>/acs/`

4. Überprüfen Sie **erweiterte URL-Einstellungen anzeigen**, wenn Sie, konfigurieren Sie die Anwendung in möchten **SP** initiiertem:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url2.png)

    In der **Anmelde-URL** Textbox, geben Sie den Wert mithilfe des folgenden Musters:`https://platform.rescale.com/saml2/<company id>/sso/`
     
    > [!NOTE] 
    > Diese sind nicht die tatsächlichen Werte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner, Antwort-URL oder Anmelde-URL ein. Wenden Sie sich an [ScaleX Enterprise Client Support-Team](http://info.rescale.com/contact_sales) , diese Werte abzurufen. 

5. Die ScaleX-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format, das wofür Sie die Konfiguration des SAML-tokenattribute benutzerdefinierte attributzuordnungen ändern muss. Klicken Sie auf **anzeigen und Bearbeiten von anderen Benutzerattribute** Kontrollkästchen, um die benutzerdefinierte öffnen Attribute Einstellungen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/scalex_attributes.png)
    
    a. Klicken Sie mit der mit der rechten Maustaste auf das Attribut **Namen** , und klicken Sie auf Löschen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/delete_attribute_name.png)

    b. Klicken Sie auf **Emailaddress** Attribut zum Öffnen des Fensters Attribut bearbeiten. Ändern Sie den Wert von **user.mail** auf **user.userprincipalname** , und klicken Sie auf Ok.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/edit_email_attribute.png)   
    
5. Auf der **SAML-Signaturzertifikat** auf **Zertifikat (Base64)** und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_certificate.png) 

6. Klicken Sie auf **speichern** Schaltfläche.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_400.png)
    
7. Auf der **ScaleX Unternehmenskonfiguration** auf **konfigurieren ScaleX Enterprise** öffnen **Anmelden konfigurieren** Fenster. Kopieren der **SAML Entitäts-ID** und **SAML anmelden Dienst-URL für einmalige** aus der **Quick Reference-Abschnitt.**

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_configure.png) 

8. So konfigurieren Sie einmaliges Anmelden auf **ScaleX Enterprise** Side, Anmeldung bei der ScaleX Enterprise-Unternehmenswebsite als Administrator.

9. Klicken Sie auf das Menü in der oberen rechten, und wählen **Contoso Verwaltung**.

    > [!NOTE] 
    > Contoso ist nur ein Beispiel. Dies sollte den tatsächlichen Firmennamen sein. 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/Test_Admin.png) 

10. Wählen Sie **Integrationen** aus der oberen Menüleiste, und wählen **Single Sign-On**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/admin_sso.png) 

11. Füllen Sie das Formular wie folgt:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/scalex_admin_save.png) 
    
    a. Wählen Sie **"Erstellen Sie mit SSO jeder Benutzer, der authentifiziert werden kann."**

    b. **Dienstanbieter Saml**: Fügen Sie den Wert ***Urn: Oasis: Namen: Tc: SAML:2.0:nameid-Format: permanente***

    c. **Name des Identitätsanbieters e-Mail-Feld im ACS-Antwort**: Fügen Sie den Wert`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. **Identitätsanbieter EntityDescriptor Entität-ID:** Einfügen der **SAML Entitäts-ID** Wert kopiert, aus dem Azure-Portal.

    e. **URL des Identitätsanbieters SingleSignOnService:** Einfügen der **SAML anmelden Dienst-URL für einmalige** vom Azure-Portal.

    f. **Identitätsanbieterzertifikat öffentlichen X509:** öffnen die X509 Zertifikat aus der Azure im Editor heruntergeladen, und fügen Sie den Inhalt in diesem Feld. Stellen Sie sicher sind, die keine Zeilenumbrüche in der Mitte des Zertifikat-Inhalts.
    
    g. Aktivieren Sie die folgenden Kontrollkästchen: **aktiviert, verschlüsseln "NameID" und AuthnRequests anmelden.**

    h. Klicken Sie auf **Update-Einstellungen für einmaliges Anmelden** zum Speichern der Einstellungen.

> [!TIP]
> Erfahren Sie jetzt eine kürzere Version dieser Anweisungen innerhalb der [Azure-Portal](https://portal.azure.com), während Sie die app einrichten.  Nach dem Hinzufügen der diese app aus der **Active Directory > Unternehmensanwendungen** einfach auf die **einmaliges Anmelden für** Registerkarte und Zugriff auf die embedded-Dokumentation über die **Konfiguration** Abschnitt unten. Weitere Informationen zu dieser Funktion embedded-Dokumentation: [Azure AD eingebettet Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen ein Azure AD-Testbenutzer
Das Ziel dieses Abschnitts ist zum Erstellen eines Testbenutzers im Azure-Portal Britta Simon aufgerufen.

![Erstellen von Azure AD-Benutzer][100]

**Führen Sie zum Erstellen eines Testbenutzers in Azure AD die folgenden Schritte aus:**

1. In der **Azure-Portal**, klicken Sie im linken Navigationsbereich auf **Azure Active Directory** Symbol.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen** , und klicken Sie auf **alle Benutzer** um die Liste von Benutzern anzuzeigen.
    
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_02.png) 

3. Klicken Sie im oberen Bereich des Dialogfelds auf **hinzufügen** So öffnen die **Benutzer** Dialogfeld.
 
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_03.png) 

4. Auf der **Benutzer** Dialogfeld Seite, die folgenden Schritte aus:
 
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_04.png) 

    a. In der **Namen** Textfeld **BrittaSimon**.

    b. In der **Benutzername** Textfeld die **e-Mail-Adresse** des BrittaSimon.

    c. Wählen Sie **Kennwort anzeigen** und notieren Sie sich den Wert, der die **Kennwort**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-scalex-enterprise-test-user"></a>Erstellen eines Testbenutzers ScaleX Enterprise

Um Azure AD-Benutzer zum Anmelden bei ScaleX Enterprise zu aktivieren, müssen sie Enterprise ScaleX in bereitgestellt werden. Im Fall von ScaleX Enterprise eine Aufgabe zur automatischen Bereitstellung ist und keine manuelle Schritte erforderlich sind. Jeder Benutzer, die erfolgreich mit der SSO-Anmeldeinformationen authentifiziert werden kann, wird automatisch auf der Seite ScaleX bereitgestellt werden.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon Azure einmaliges Anmelden verwenden, durch das Gewähren von Benutzerzugriff auf ScaleX Enterprise.

![Weisen Sie Benutzer][200] 

**ScaleX Enterprise Britta Simon zuweisen möchten, führen Sie die folgenden Schritte aus:**

1. Im Azure-Portal, öffnen Sie die Anwendungen anzuzeigen, und navigieren Sie zu dem Verzeichnisansicht, und wechseln Sie zu **unternehmensanwendungen** klicken Sie dann auf **alle Anwendungen**.

    ![Weisen Sie Benutzer][201] 

2. Wählen Sie in der Liste der Anwendungen **ScaleX Enterprise**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Weisen Sie Benutzer][202] 

4. Klicken Sie auf **hinzufügen** Schaltfläche. Wählen Sie dann **Benutzer und Gruppen** auf **Zuweisung hinzufügen** Dialogfeld.

    ![Weisen Sie Benutzer][203]

5. Auf **Benutzer und Gruppen** wählen Sie im Dialogfeld **Britta Simon** in der Benutzerliste.

6. Klicken Sie auf **wählen** Schaltfläche **Benutzer und Gruppen** Dialogfeld.

7. Klicken Sie auf **zuweisen** Schaltfläche **Zuweisung hinzufügen** Dialogfeld.

### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

In diesem Abschnitt Testen Sie Ihre Azure AD einmalige Anmelden-Konfiguration über den Zugriffsbereich.

Klicken Sie auf die Kachel ScaleX Enterprise im Zugriffsbereich, Sie werden abrufen automatisch auf die ScaleX Enterprise-Anwendung. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://msdn.microsoft.com/library/dn308586).


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zur Integration von SaaS-Apps mit Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was ist Anwendungszugriff und einmaliges Anmelden bei Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_203.png

