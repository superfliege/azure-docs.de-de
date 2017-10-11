---
title: 'Lernprogramm: Azure Active Directory-Integration mit Aha! | Microsoft Docs'
description: Informationen Sie zum Konfigurieren von einmaliges Anmelden zwischen Azure Active Directory und Aha!.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ad955d3d-896a-41bb-800d-68e8cb5ff48d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 7723864b2e1ab2d5b69d86f0fa18416b9d3f9aa3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-aha"></a>Lernprogramm: Azure Active Directory-Integration mit Aha!

In diesem Lernprogramm erfahren Sie, wie zum Integrieren von Aha! mit Azure Active Directory (Azure AD).

Integrieren von Anwendungsintegrationsseite! mit Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Aha hat!
- Sie können Ihre Benutzer für die automatisch signiert AHA bei erhalten! (Einmaliges Anmelden) mit ihren Azure AD-Konten
- Sie können Ihre Konten an einem zentralen Ort - Azure-Portal verwalten.

Wenn Sie weitere Informationen zum Integrieren von SaaS-Apps in Azure AD wissen möchten, finden Sie unter [was Anwendungszugriff und einmaliges Anmelden bei Azure Active Directory ist](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

So konfigurieren Sie Azure AD-Integration mit Aha!, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Ein Anwendungsintegrationsseite! für einmaliges Anmelden Abonnement mit aktivierter

> [!NOTE]
> Um die Schritte in diesem Lernprogramm zu testen, führen Sie wird nicht empfohlen, mithilfe einer produktiven Umgebung.

Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlungen beachten:

- Verwenden Sie nicht Ihre produktionsumgebung, wenn es erforderlich ist.
- Wenn Sie nicht über eine Testversion Azure Active Directory-Umgebung verfügen, erhalten Sie eine einmonatige Testversion [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Lernprogramm testen Sie Azure AD einmaliges Anmelden in einer testumgebung. In diesem Lernprogramm beschriebene Szenario besteht aus zwei Hauptbausteinen:

1. Hinzufügen von Anwendungsintegrationsseite! aus dem Katalog
2. Konfigurieren und Testen von Azure AD-einmaliges Anmelden

## <a name="adding-aha-from-the-gallery"></a>Hinzufügen von Anwendungsintegrationsseite! aus dem Katalog
So konfigurieren Sie die Integration von Aha! in Azure AD müssen Sie zum Hinzufügen von Aha! aus der Galerie der Liste der verwalteten SaaS-apps.

**Hinzufügen von Aha! Führen Sie die folgenden Schritte aus, aus dem Katalog:**

1. In der  **[Azure-Portal](https://portal.azure.com)**, klicken Sie auf den linken Navigationsbereich auf **Azure Active Directory** Symbol. 

    ![Active Directory][1]

2. Navigieren Sie zu **unternehmensanwendungen**. Klicken Sie dann auf **alle Anwendungen**.

    ![Applications][2]
    
3. Neue Anwendung hinzufügen möchten, klicken Sie auf **neue Anwendung** Schaltfläche oben Dialogfeld.

    ![Applications][3]

4. Geben Sie in das Suchfeld **Aha!**.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-aha-tutorial/tutorial_aha_search.png)

5. Wählen Sie im Ergebnisbereich, **Aha!**, und klicken Sie dann auf **hinzufügen** Schaltfläche, um die Anwendung hinzuzufügen.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-aha-tutorial/tutorial_aha_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD-einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Sie Azure AD einmaliges Anmelden mit Aha! basierend auf einen Testbenutzer namens "Britta Simon."

Für einmaliges Anmelden funktioniert muss Azure AD zu wissen, welche Benutzer die Gegenstück in Aha! ist für einen Benutzer in Azure AD. In anderen Worten, eine linkbeziehung zwischen einem Azure AD-Benutzer und die entsprechenden Benutzer in Aha! hergestellt werden muss.

In Aha!, weisen den Wert der die **Benutzername** in Azure AD als Wert des der **Benutzername** auf die linkbeziehung zu erstellen.

Konfigurieren und Testen Azure AD einmaliges Anmelden mit Aha!, müssen Sie den folgenden Bausteinen ausführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  – Informationen zum Aktivieren Ihrer Benutzer dieses Feature verwenden.
2. **[Erstellen ein Azure AD-Testbenutzer](#creating-an-azure-ad-test-user)**  – Informationen zum Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen einer Anwendungsintegrationsseite! Benutzer testen](#creating-an-aha-test-user)**  – damit eine Entsprechung Britta Simon in Aha! der Azure AD-Darstellung eines Benutzers verknüpft ist.
4. **[Zuweisen von Azure AD-Testbenutzer](#assigning-the-azure-ad-test-user)**  – zum Aktivieren der Britta Simon einmaliges Anmelden für Azure AD verwenden.
5. **[Testen Sie einmaliges Anmelden für](#testing-single-sign-on)**  – zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

In diesem Abschnitt werden Azure AD einmaliges Anmelden im Azure-Portal aktivieren und konfigurieren Sie einmaliges Anmelden in Ihrer Aha! die Anwendung.

**So konfigurieren Sie einmaliges Anmelden für Azure AD mit Aha!, die folgenden Schritte aus:**

1. Im Azure-Portal auf der **Aha!** anwendungsintegrationsseite, klicken Sie auf **des einmaligen Anmeldens**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Auf der **des einmaligen Anmeldens** wählen Sie im Dialogfeld **Modus** als **SAML-basierten anmelden** einmaliges Anmelden aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-aha-tutorial/tutorial_aha_samlbase.png)

3. Auf der **Anwendungsintegrationsseite! Domänen und URLs** führen die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-aha-tutorial/tutorial_aha_url.png)

    a. In der **Anmelde-URL** Textfeld eine URL mithilfe des folgenden Musters:`https://<companyname>.aha.io/session/new`

    b. In der **Bezeichner** Textfeld eine URL mithilfe des folgenden Musters:`https://<companyname>.aha.io`

    > [!NOTE] 
    > Diese Werte sind nicht real. Aktualisieren Sie diese Werte, mit dem tatsächlichen Anmelde-URL und Bezeichner. Wenden Sie sich an [Anwendungsintegrationsseite! Client-Supportteam](https://www.aha.io/company/contact) , diese Werte abzurufen. 
 
4. Auf der **SAML-Signaturzertifikat** auf **Metadata XML** und speichern Sie die Metadatendatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-aha-tutorial/tutorial_aha_certificate.png) 

5. Klicken Sie auf **speichern** Schaltfläche.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-aha-tutorial/tutorial_general_400.png)

6. Melden Sie sich in einem anderen Webbrowserfenster auf Ihrer Aha an! -Unternehmenswebsite als Administrator.

7. Klicken Sie im Menü am oberen Rand auf **Einstellungen**.

    ![Einstellungen](./media/active-directory-saas-aha-tutorial/IC798950.png "Einstellungen")

8. Klicken Sie auf **Konto**.
   
    ![Profil](./media/active-directory-saas-aha-tutorial/IC798951.png "Profil")

9. Klicken Sie auf **Sicherheit und einmaliges Anmelden für**.
   
    ![Sicherheit und einmaliges Anmelden für](./media/active-directory-saas-aha-tutorial/IC798952.png "Sicherheit und einmaliges Anmelden")

10. In **Single Sign-On** Abschnitt als **Identitätsanbieter**Option **SAML2.0**.
   
    ![Sicherheit und einmaliges Anmelden für](./media/active-directory-saas-aha-tutorial/IC798953.png "Sicherheit und einmaliges Anmelden")

11. Auf der **Single Sign-On** Konfiguration führen die folgenden Schritte aus:
    
    ![Einmaliges Anmelden](./media/active-directory-saas-aha-tutorial/IC798954.png "des einmaligen Anmeldens")
    
       a. In der **Namen** Textbox, geben Sie einen Namen für Ihre Konfiguration.

       b. Für **Konfiguration mithilfe von**Option **Metadatendatei**.
   
       c. Um Ihre heruntergeladene Metadatendatei hochzuladen, klicken Sie auf **Durchsuchen**.
   
       d. Klicken Sie auf **Aktualisieren**.

> [!TIP]
> Erfahren Sie jetzt eine kürzere Version dieser Anweisungen innerhalb der [Azure-Portal](https://portal.azure.com), während Sie die app einrichten.  Nach dem Hinzufügen der diese app aus der **Active Directory > Unternehmensanwendungen** einfach auf die **einmaliges Anmelden für** Registerkarte und Zugriff auf die embedded-Dokumentation über die **Konfiguration** Abschnitt unten. Weitere Informationen zu dieser Funktion embedded-Dokumentation: [Azure AD eingebettet Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen ein Azure AD-Testbenutzer
Das Ziel dieses Abschnitts ist zum Erstellen eines Testbenutzers im Azure-Portal Britta Simon aufgerufen.

![Erstellen von Azure AD-Benutzer][100]

**Führen Sie zum Erstellen eines Testbenutzers in Azure AD die folgenden Schritte aus:**

1. In der **Azure-Portal**, klicken Sie im linken Navigationsbereich auf **Azure Active Directory** Symbol.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-aha-tutorial/create_aaduser_01.png) 

2. Um die Liste der Benutzer anzuzeigen, wechseln Sie zu **Benutzer und Gruppen** , und klicken Sie auf **alle Benutzer**.
    
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-aha-tutorial/create_aaduser_02.png) 

3. So öffnen die **Benutzer** Dialogfeld klicken Sie auf **hinzufügen** oben im Dialogfeld auf.
 
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-aha-tutorial/create_aaduser_03.png) 

4. Auf der **Benutzer** Dialogfeld Seite, die folgenden Schritte aus:
 
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-aha-tutorial/create_aaduser_04.png) 

    a. In der **Namen** Textfeld **BrittaSimon**.

    b. In der **Benutzername** Textfeld die **e-Mail-Adresse** des BrittaSimon.

    c. Wählen Sie **Kennwort anzeigen** und notieren Sie sich den Wert, der die **Kennwort**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-an-aha-test-user"></a>Erstellen einer Anwendungsintegrationsseite! Benutzer testen

So aktivieren Sie Azure AD-Benutzer zum Anmelden bei Aha!, müssen sie in Aha bereitgestellt werden.  

Im Fall von Aha!, die Bereitstellung wird eine automatisierte Task. Es ist kein Aktionselement vorhanden.

Benutzer werden bei Bedarf automatisch beim ersten einmaliges Anmelden erstellt.

>[!NOTE]
>Sie können beliebige andere von Aha! benutzerkontoerstellungstools oder APIs von Aha! um die Bereitstellung von AAD-Benutzerkonten.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon Azure einmaliges Anmelden verwenden, durch das Gewähren des Zugriffs AHA!.

![Weisen Sie Benutzer][200] 

**Zuweisen von Britta Simon AHA!, die folgenden Schritte aus:**

1. Im Azure-Portal, öffnen Sie die Anwendungen anzuzeigen, und navigieren Sie zu dem Verzeichnisansicht, und wechseln Sie zu **unternehmensanwendungen** klicken Sie dann auf **alle Anwendungen**.

    ![Weisen Sie Benutzer][201] 

2. Wählen Sie in der Liste der Anwendungen **Aha!**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-aha-tutorial/tutorial_aha_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Weisen Sie Benutzer][202] 

4. Klicken Sie auf **hinzufügen** Schaltfläche. Wählen Sie dann **Benutzer und Gruppen** auf **Zuweisung hinzufügen** Dialogfeld.

    ![Weisen Sie Benutzer][203]

5. Auf **Benutzer und Gruppen** wählen Sie im Dialogfeld **Britta Simon** in der Benutzerliste.

6. Klicken Sie auf **wählen** Schaltfläche **Benutzer und Gruppen** Dialogfeld.

7. Klicken Sie auf **zuweisen** Schaltfläche **Zuweisung hinzufügen** Dialogfeld.
    
### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Wenn Sie Ihre Einstellungen für einmaliges Anmelden testen möchten, öffnen Sie auf den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zur Integration von SaaS-Apps mit Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was ist Anwendungszugriff und einmaliges Anmelden bei Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-aha-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-aha-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-aha-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-aha-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-aha-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-aha-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-aha-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-aha-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-aha-tutorial/tutorial_general_203.png

