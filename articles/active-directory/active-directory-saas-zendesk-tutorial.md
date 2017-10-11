---
title: 'Lernprogramm: Azure Active Directory-Integration mit Zendesk | Microsoft Docs'
description: Erfahren Sie, wie einmaliges Anmelden zwischen Azure Active Directory und Zendesk konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 51c06d838c5ed6286dfb99ea25faaaf33bad5f3c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Lernprogramm: Azure Active Directory-Integration mit Zendesk

In diesem Lernprogramm erfahren Sie, wie Zendesk mit Azure Active Directory (Azure AD) integrieren.

Integrieren von Zendesk in Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Zendesk hat
- Sie können Ihre Benutzer für die automatisch signiert zendesk (Single Sign-On) mit ihren Azure AD-Konten bei abrufen
- Sie können Ihre Konten an einem zentralen Ort - Azure-Portal verwalten.

Wenn Sie weitere Informationen zum Integrieren von SaaS-Apps in Azure AD wissen möchten, finden Sie unter [was Anwendungszugriff und einmaliges Anmelden bei Azure Active Directory ist](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Konfiguration von Azure AD-Integration mit Zendesk benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Zendesk einmaliges Anmelden für Abonnement mit aktivierter


> [!NOTE]
> Um die Schritte in diesem Lernprogramm zu testen, führen Sie wird nicht empfohlen, mithilfe einer produktiven Umgebung.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlungen beachten:

- Verwenden Sie nicht Ihre produktionsumgebung, wenn es erforderlich ist.
- Wenn Sie nicht über eine Testversion Azure Active Directory-Umgebung verfügen, erhalten Sie eine einmonatige Testversion [hier](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Lernprogramm testen Sie Azure AD einmaliges Anmelden in einer testumgebung. In diesem Lernprogramm beschriebene Szenario besteht aus zwei Hauptbausteinen:

1. Zendesk aus dem Katalog hinzufügen
2. Konfigurieren und Testen von Azure AD-einmaliges Anmelden


## <a name="adding-zendesk-from-the-gallery"></a>Zendesk aus dem Katalog hinzufügen
Um die Zendesk-Integration in Azure AD zu konfigurieren, müssen Sie Zendesk der Liste der verwalteten SaaS-apps aus dem Katalog hinzufügen.

**Um Zendesk aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. In der  **[Azure-Portal](https://portal.azure.com)**, klicken Sie auf den linken Navigationsbereich auf **Azure Active Directory** Symbol. 

    ![Active Directory][1]

2. Navigieren Sie zu **unternehmensanwendungen**. Klicken Sie dann auf **alle Anwendungen**.

    ![Applications][2]
    
3. Klicken Sie auf **neue Anwendung** Schaltfläche oben im Dialogfeld auf.

    ![Applications][3]

4. Geben Sie in das Suchfeld **Zendesk**.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_search.png)

5. Wählen Sie im Ergebnisbereich, **Zendesk**, und klicken Sie dann auf **hinzufügen** Schaltfläche, um die Anwendung hinzuzufügen.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD-einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Azure AD einmaliges Anmelden für Zendesk basierend auf einen Testbenutzer "Britta Simon" aufgerufen.

Für einmaliges Anmelden funktioniert muss Azure AD zu wissen, was der Benutzer Gegenstück in Zendesk für einen Benutzer in Azure AD ist. Das heißt, muss eine linkbeziehung zwischen einem Azure AD-Benutzer und die entsprechenden Benutzer in Zendesk hergestellt werden.

Dieser Link wird Beziehung durch Zuweisen des Werts von der **Benutzername** in Azure AD als Wert des der **Benutzername** in Zendesk.

Zum Konfigurieren und Testen Sie Azure AD einmaliges Anmelden mit Zendesk, müssen Sie den folgenden Bausteinen ausführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  – Informationen zum Aktivieren Ihrer Benutzer dieses Feature verwenden.
2. **[Erstellen ein Azure AD-Testbenutzer](#creating-an-azure-ad-test-user)**  – Informationen zum Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen eines Testbenutzers Zendesk](#creating-a-zendesk-test-user)**  – um eine Entsprechung Britta Simon in Zendesk verfügen, die mit der Azure AD-Darstellung eines Benutzers verknüpft ist.
4. **[Zuweisen von Azure AD-Testbenutzer](#assigning-the-azure-ad-test-user)**  – zum Aktivieren der Britta Simon einmaliges Anmelden für Azure AD verwenden.
5. **[Testen Sie einmaliges Anmelden für](#testing-single-sign-on)**  – zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

In diesem Abschnitt Azure AD einmaliges Anmelden in Azure-Portal aktivieren und einmaliges Anmelden in der Zendesk-Anwendung konfigurieren.

**Führen Sie zum Konfigurieren von Azure AD einmaliges Anmelden mit Zendesk die folgenden Schritte aus:**

1. Im Azure-Portal auf der **Zendesk** anwendungsintegrationsseite, klicken Sie auf **des einmaligen Anmeldens**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Auf der **des einmaligen Anmeldens** wählen Sie im Dialogfeld **Modus** als **SAML-basierten anmelden** einmaliges Anmelden aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. Auf der **Zendesk-Domänen und URLs** führen die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_url.png)

    a. In der **Anmelde-URL** Textbox, geben Sie den Wert mithilfe des folgenden Musters:`https://<subdomain>.zendesk.com`

    b. In der **Bezeichner** Textbox, geben Sie den Wert mithilfe des folgenden Musters:`https://<subdomain>.zendesk.com`

    > [!NOTE] 
    > Diese Werte sind nicht real. Aktualisieren Sie diese Werte mit dem tatsächlichen Anmelde-URL und Bezeichner-URL ein. Wenden Sie sich an [Zendesk-Supportteam](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) , diese Werte abzurufen. 

4. Zendesk erwartet die SAML-Assertionen in einem bestimmten Format. Keine verbindliche SAML-Attribute vorhanden sind, aber Sie können optional ein Attribut hinzufügen **Benutzerattribute** Abschnitt anhand der unten beschriebenen Schritte aus: 

     ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes1.png)

    a. Klicken Sie auf die **anzeigen und bearbeiten Sie die anderen Attribute** Kontrollkästchen.
     
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes2.png)
   
    b. Klicken Sie auf die **Attribut hinzufügen** öffnen **Add Attribut** Dialogfeld.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_05.png)

    c. In der **Namen** Textbox, geben Sie den Attributnamen (z. B. **Emailaddress**).
    
    d. Aus der **Wert** , wählen Sie den Wert des Attributs (als **user.mail**).
    
    e. Klicken Sie auf **Ok**
 
    > [!NOTE] 
    > Erweiterungsattribute wird verwendet, um Attribute hinzuzufügen, die nicht in Azure AD standardmäßig sind. Klicken Sie auf [Benutzerattribute, die im SAML festzulegenden](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) SAML abzurufenden die vollständige Liste der Attribute, **Zendesk** akzeptiert. 

5. Auf der **SAML-Signaturzertifikat** kopieren Sie die **FINGERABDRUCK** Wert des Zertifikats.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_certificate.png) 

6. Auf der **Zendesk-Konfiguration** auf **Zendesk konfigurieren** öffnen **Anmelden konfigurieren** Fenster. Kopieren der **-URL für Abmeldung und SAML anmelden Dienst-URL für einmalige** aus der **Quick Reference-Abschnitt.**

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_configure.png) 

7. Melden Sie in einem anderen Webbrowserfenster bei Ihrer Zendesk-Unternehmenswebsite als Administrator.

8. Klicken Sie auf **Admin**.

9. Klicken Sie im linken Navigationsbereich auf **Einstellungen**, und klicken Sie dann auf **Sicherheit**.

10. Auf der **Sicherheit** führen die folgenden Schritte aus: 
   
     ![Sicherheit](./media/active-directory-saas-zendesk-tutorial/ic773089.png "Sicherheit")

    ![Einmaliges Anmelden](./media/active-directory-saas-zendesk-tutorial/ic773090.png "des einmaligen Anmeldens")

     a. Klicken Sie auf die **Admin & Agents** Registerkarte.

     b. Wählen Sie **einmaliges Anmelden (SSO) und SAML**, und wählen Sie dann **SAML**.

     c. In **SAML SSO URL** Textbox, fügen Sie den Wert der **SAML anmelden Dienst-URL für einmalige** der Sie vom Azure-Portal kopiert haben. 

     d. In **Remote-Abmelde-URL** Textbox, fügen Sie den Wert der **URL Abmeldung** der Sie vom Azure-Portal kopiert haben.
        
     e. In **Zertifikatsfingerabdruck** Textfeld einfügen der **Fingerabdruck** Wert des Zertifikats, das Sie Azure-Portal kopiert haben.
     
     f. Klicken Sie auf **Speichern**.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen ein Azure AD-Testbenutzer
Das Ziel dieses Abschnitts ist zum Erstellen eines Testbenutzers im Azure-Portal Britta Simon aufgerufen.

![Erstellen von Azure AD-Benutzer][100]

**Führen Sie zum Erstellen eines Testbenutzers in Azure AD die folgenden Schritte aus:**

1. In der **Azure-Portal**, klicken Sie im linken Navigationsbereich auf **Azure Active Directory** Symbol.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-zendesk-tutorial/create_aaduser_01.png) 

2. Zum Anzeigen die Liste der Benutzer wechseln Sie zu **Benutzer und Gruppen** , und klicken Sie auf **alle Benutzer**.
    
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-zendesk-tutorial/create_aaduser_02.png) 

3. Klicken Sie im oberen Bereich des Dialogfelds auf **hinzufügen** So öffnen die **Benutzer** Dialogfeld.
 
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-zendesk-tutorial/create_aaduser_03.png) 

4. Auf der **Benutzer** Dialogfeld Seite, die folgenden Schritte aus:
 
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-zendesk-tutorial/create_aaduser_04.png) 

    a. In der **Namen** Textfeld **BrittaSimon**.

    b. In der **Benutzername** Textfeld die **e-Mail-Adresse** des BrittaSimon.

    c. Wählen Sie **Kennwort anzeigen** und notieren Sie sich den Wert, der die **Kennwort**.

    d. Klicken Sie auf **Erstellen**. 

### <a name="creating-a-zendesk-test-user"></a>Erstellen eines Testbenutzers Zendesk

Damit Azure AD-Benutzer anmelden **Zendesk**, sie müssen bereitgestellt werden, in **Zendesk**.  
Abhängig von der Rolle, die in den apps zugewiesen wird ist es das erwartete Verhalten:

 1. **Endbenutzer** Konten werden automatisch bereitgestellt werden, bei der Anmeldung.
 2. **Agent** und **Admin** Konten müssen manuell in bereitgestellt werden **Zendesk** vor dem anmelden.
 
**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich auf Ihre **Zendesk** Mandanten.

2. Wählen Sie die **Kundenliste** Registerkarte.

3. Wählen Sie die **Benutzer** Registerkarte, und klicken Sie auf **hinzufügen**.
   
    ![Benutzer hinzufügen](./media/active-directory-saas-zendesk-tutorial/ic773632.png "Benutzer hinzufügen")
4. Geben Sie die e-Mail-Adresse eines vorhandenen Azure AD-Kontos Sie bereitstellen möchten, und klicken Sie dann auf **speichern**.
   
    ![Neuer Benutzer](./media/active-directory-saas-zendesk-tutorial/ic773633.png "neuer Benutzer")

> [!NOTE]
> Sie können eine beliebige andere Zendesk Benutzerkonten verwenden oder APIs von Zendesk zu AAD-Benutzerkonten.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon durch Gewähren des Zugriffs auf Zendesk Azure einmaliges Anmelden zu verwenden.

![Weisen Sie Benutzer][200] 

**Zendesk Britta Simon zuweisen möchten, führen Sie die folgenden Schritte aus:**

1. Im Azure-Portal, öffnen Sie die Anwendungen anzuzeigen, und navigieren Sie zu dem Verzeichnisansicht, und wechseln Sie zu **unternehmensanwendungen** klicken Sie dann auf **alle Anwendungen**.

    ![Weisen Sie Benutzer][201] 

2. Wählen Sie in der Liste der Anwendungen **Zendesk**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Weisen Sie Benutzer][202] 

4. Klicken Sie auf **hinzufügen** Schaltfläche. Wählen Sie dann **Benutzer und Gruppen** auf **Zuweisung hinzufügen** Dialogfeld.

    ![Weisen Sie Benutzer][203]

5. Auf **Benutzer und Gruppen** wählen Sie im Dialogfeld **Britta Simon** in der Benutzerliste.

6. Klicken Sie auf **wählen** Schaltfläche **Benutzer und Gruppen** Dialogfeld.

7. Klicken Sie auf **zuweisen** Schaltfläche **Zuweisung hinzufügen** Dialogfeld.
    
### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

In diesem Abschnitt Testen Sie Ihre Azure AD einmalige Anmelden-Konfiguration über den Zugriffsbereich.

Wenn Sie auf den Zugriffsbereich die Zendesk-Kachel klicken, Sie sollten automatisch auf Ihre Zendesk-Anwendung auf abrufen.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zur Integration von SaaS-Apps mit Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was ist Anwendungszugriff und einmaliges Anmelden bei Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_203.png
