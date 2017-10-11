---
title: 'Lernprogramm: Azure Active Directory-Integration mit WORKS MOBILE | Microsoft Docs'
description: Informationen Sie zum Konfigurieren von einmaliges Anmelden zwischen Azure Active Directory und WORKS MOBILE.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 725f32fd-d0ad-49c7-b137-1cc246bf85d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 139a1968a59424eae278de3e7fa227ad340a1eb8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-works-mobile"></a>Lernprogramm: Azure Active Directory-Integration mit WORKS MOBILE

In diesem Lernprogramm erfahren Sie, wie MOBILE funktioniert mit Azure Active Directory (Azure AD) integrieren.

Integration von MOBILE funktioniert mit Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf MOBILE funktioniert hat
- Sie können Ihre Benutzer für die automatisch signiert WORKS Mobile (Single Sign-On) bei Abrufen mit ihren Azure AD-Konten aktivieren.
- Sie können Ihre Konten an einem zentralen Ort - Azure-Portal verwalten.

Wenn Sie weitere Informationen zum Integrieren von SaaS-Apps in Azure AD wissen möchten, finden Sie unter [was Anwendungszugriff und einmaliges Anmelden bei Azure Active Directory ist](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren von Azure AD-Integration mit WORKS MOBILE benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Eine MOBILE funktioniert SSO-fähiges Abonnement

> [!NOTE]
> Um die Schritte in diesem Lernprogramm zu testen, führen Sie wird nicht empfohlen, mithilfe einer produktiven Umgebung.

Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlungen beachten:

- Verwenden Sie nicht Ihre produktionsumgebung, wenn es erforderlich ist.
- Wenn Sie nicht über eine Testversion Azure Active Directory-Umgebung verfügen, erhalten Sie eine einmonatige Testversion [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Lernprogramm testen Sie Azure AD einmaliges Anmelden in einer testumgebung. In diesem Lernprogramm beschriebene Szenario besteht aus zwei Hauptbausteinen:

1. WORKS MOBILE aus dem Katalog hinzufügen
2. Konfigurieren und Testen von Azure AD-einmaliges Anmelden

## <a name="adding-works-mobile-from-the-gallery"></a>WORKS MOBILE aus dem Katalog hinzufügen
Um die Integration von MOBILE funktioniert in Azure AD zu konfigurieren, müssen Sie WORKS MOBILE der Liste der verwalteten SaaS-apps aus dem Katalog hinzufügen.

**Um WORKS MOBILE aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. In der  **[Azure-Portal](https://portal.azure.com)**, klicken Sie auf den linken Navigationsbereich auf **Azure Active Directory** Symbol. 

    ![Active Directory][1]

2. Navigieren Sie zu **unternehmensanwendungen**. Klicken Sie dann auf **alle Anwendungen**.

    ![Applications][2]
    
3. Neue Anwendung hinzufügen möchten, klicken Sie auf **neue Anwendung** Schaltfläche oben Dialogfeld.

    ![Applications][3]

4. Geben Sie in das Suchfeld **WORKS MOBILE**.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_search.png)

5. Wählen Sie im Ergebnisbereich, **WORKS MOBILE**, und klicken Sie dann auf **hinzufügen** Schaltfläche, um die Anwendung hinzuzufügen.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD-einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Sie Azure AD einmaliges Anmelden für WORKS Mobile basierend auf einen Testbenutzer namens "Britta Simon."

Für einmaliges Anmelden funktioniert muss Azure AD zu wissen, was der Benutzer Gegenstück in WORKS MOBILE für einen Benutzer in Azure AD ist. Das heißt, muss eine linkbeziehung zwischen Azure AD-Benutzers und die entsprechenden Benutzer in WORKS MOBILE hergestellt werden.

Dieser Link wird Beziehung durch Zuweisen des Werts von der **Benutzername** in Azure AD als Wert des der **Benutzername** in MOBILE funktioniert.

Zum Konfigurieren und Testen Sie Azure AD einmaliges Anmelden mit MOBILE funktioniert, müssen Sie den folgenden Bausteinen ausführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  – Informationen zum Aktivieren Ihrer Benutzer dieses Feature verwenden.
2. **[Erstellen ein Azure AD-Testbenutzer](#creating-an-azure-ad-test-user)**  – Informationen zum Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen eines Testbenutzers WORKS MOBILE](#creating-a-works-mobile-test-user)**  – um eine Entsprechung Britta Simon WORKS MOBILE umfassen, die mit der Azure AD-Darstellung eines Benutzers verknüpft ist.
4. **[Zuweisen von Azure AD-Testbenutzer](#assigning-the-azure-ad-test-user)**  – zum Aktivieren der Britta Simon einmaliges Anmelden für Azure AD verwenden.
5. **[Testen Sie einmaliges Anmelden für](#testing-single-sign-on)**  – zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

In diesem Abschnitt werden Azure AD einmaliges Anmelden im Azure-Portal aktivieren und einmaliges Anmelden in Ihrer WORKS MOBILE Anwendung konfigurieren.

**Führen Sie zum Konfigurieren von Azure AD einmaliges Anmelden mit WORKS MOBILE die folgenden Schritte aus:**

1. Im Azure-Portal auf der **WORKS MOBILE** anwendungsintegrationsseite, klicken Sie auf **des einmaligen Anmeldens**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Auf der **des einmaligen Anmeldens** wählen Sie im Dialogfeld **Modus** als **SAML-basierten anmelden** einmaliges Anmelden aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_samlbase.png)

3. Auf der **WORKS MOBILE Domänen und URLs** führen die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_url.png)

    a. In der **Anmelde-URL** Textfeld eine URL mithilfe des folgenden Musters:`https://<subdomain>.worksmobile.com/jp/myservice`

    b. In der **Bezeichner** Textbox, geben Sie den Wert als`worksmobile.com`

    > [!NOTE] 
    > Dieser Wert ist keine echte. Aktualisieren Sie diesen Wert mit dem tatsächlichen Anmelde-URL ein. Wenden Sie sich an [WORKS MOBILE Client-Supportteam](mailto:dl_ssoinfo@worksmobile.com) zum Abrufen dieses Werts. 
 
4. Auf der **SAML-Signaturzertifikat** auf **Certificate(Raw)** und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_certificate.png) 

5. Klicken Sie auf **speichern** Schaltfläche.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-worksmobile-tutorial/tutorial_general_400.png)

6. Auf der **WORKS MOBILE Konfiguration** auf **konfigurieren WORKS MOBILE** öffnen **Anmelden konfigurieren** Fenster. Kopieren der **URL Abmeldung, SAML-Entitäts-ID und SAML anmelden Dienst-URL für einmalige** aus der **Quick Reference-Abschnitt.**

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_configure.png) 

7. Um SSO konfiguriert für Ihre Anwendung zu erhalten, wenden Sie sich an [WORKS MOBILE-Supportteam](mailto:dl_ssoinfo@worksmobile.com) und geben sie die folgenden Informationen: 

    • Der heruntergeladenen **Zertifikatdatei**

    • Die **URL des SAML-Dienst für einmaliges Anmelden**

    • Die **SAML Entitäts-ID**

    • Die **Abmelde-URL**

> [!TIP]
> Erfahren Sie jetzt eine kürzere Version dieser Anweisungen innerhalb der [Azure-Portal](https://portal.azure.com), während Sie die app einrichten.  Nach dem Hinzufügen der diese app aus der **Active Directory > Unternehmensanwendungen** einfach auf die **einmaliges Anmelden für** Registerkarte und Zugriff auf die embedded-Dokumentation über die **Konfiguration** Abschnitt unten. Weitere Informationen zu dieser Funktion embedded-Dokumentation: [Azure AD eingebettet Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Erstellen ein Azure AD-Testbenutzer
Das Ziel dieses Abschnitts ist zum Erstellen eines Testbenutzers im Azure-Portal Britta Simon aufgerufen.

![Erstellen von Azure AD-Benutzer][100]

**Führen Sie zum Erstellen eines Testbenutzers in Azure AD die folgenden Schritte aus:**

1. In der **Azure-Portal**, klicken Sie im linken Navigationsbereich auf **Azure Active Directory** Symbol.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_01.png) 

2. Um die Liste der Benutzer anzuzeigen, wechseln Sie zu **Benutzer und Gruppen** , und klicken Sie auf **alle Benutzer**.
    
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_02.png) 

3. So öffnen die **Benutzer** Dialogfeld klicken Sie auf **hinzufügen** oben im Dialogfeld auf.
 
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_03.png) 

4. Auf der **Benutzer** Dialogfeld Seite, die folgenden Schritte aus:
 
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_04.png) 

    a. In der **Namen** Textfeld **BrittaSimon**.

    b. In der **Benutzername** Textfeld die **e-Mail-Adresse** des BrittaSimon.

    c. Wählen Sie **Kennwort anzeigen** und notieren Sie sich den Wert, der die **Kennwort**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-works-mobile-test-user"></a>Erstellen eines Testbenutzers WORKS MOBILE

 In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in MOBILE funktioniert. Klären Sie mit [WORKS MOBILE-Supportteam](mailto:dl_ssoinfo@worksmobile.com) um die Benutzer in der WORKS MOBILE-Plattform hinzuzufügen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon durch Gewähren des Zugriffs auf MOBILE funktioniert Azure einmaliges Anmelden zu verwenden.

![Weisen Sie Benutzer][200] 

**WORKS MOBILE Britta Simon zuweisen möchten, führen Sie die folgenden Schritte aus:**

1. Im Azure-Portal, öffnen Sie die Anwendungen anzuzeigen, und navigieren Sie zu dem Verzeichnisansicht, und wechseln Sie zu **unternehmensanwendungen** klicken Sie dann auf **alle Anwendungen**.

    ![Weisen Sie Benutzer][201] 

2. Wählen Sie in der Liste der Anwendungen **WORKS MOBILE**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Weisen Sie Benutzer][202] 

4. Klicken Sie auf **hinzufügen** Schaltfläche. Wählen Sie dann **Benutzer und Gruppen** auf **Zuweisung hinzufügen** Dialogfeld.

    ![Weisen Sie Benutzer][203]

5. Auf **Benutzer und Gruppen** wählen Sie im Dialogfeld **Britta Simon** in der Benutzerliste.

6. Klicken Sie auf **wählen** Schaltfläche **Benutzer und Gruppen** Dialogfeld.

7. Klicken Sie auf **zuweisen** Schaltfläche **Zuweisung hinzufügen** Dialogfeld.
    
### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

In diesem Abschnitt Testen Sie Ihre Azure AD-SSO-Konfiguration, die über den Zugriffsbereich.

Wenn Sie die Kachel WORKS MOBILE im Zugriffsbereich klicken, Sie sollten automatisch für Ihre Anwendung funktioniert MOBILE angemeldete abrufen.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zur Integration von SaaS-Apps mit Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was ist Anwendungszugriff und einmaliges Anmelden bei Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_203.png

