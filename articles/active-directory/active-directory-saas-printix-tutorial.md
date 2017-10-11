---
title: 'Lernprogramm: Azure Active Directory-Integration mit Printix | Microsoft Docs'
description: Erfahren Sie, wie einmaliges Anmelden zwischen Azure Active Directory und Printix konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4aea7320-b2d5-49e0-9b63-aeaff0f6fe66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 97dbb3fa0531f2f679badb6bb9752f2e42fc9cb3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-printix"></a>Lernprogramm: Azure Active Directory-Integration mit Printix

In diesem Lernprogramm erfahren Sie, wie Printix mit Azure Active Directory (Azure AD) integrieren.

Integrieren von Printix in Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Printix hat
- Sie können Ihre Benutzer für die automatisch signiert, Printix (Single Sign-On) mit ihren Azure AD-Konten bei abrufen
- Sie können Ihre Konten an einem zentralen Ort - Azure-Portal verwalten.

Wenn Sie weitere Informationen zum Integrieren von SaaS-Apps in Azure AD wissen möchten, finden Sie unter [was Anwendungszugriff und einmaliges Anmelden bei Azure Active Directory ist](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren von Azure AD-Integration mit Printix benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Eine Printix einmaliges Anmelden für Abonnement mit aktivierter

> [!NOTE]
> Um die Schritte in diesem Lernprogramm zu testen, führen Sie wird nicht empfohlen, mithilfe einer produktiven Umgebung.

Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlungen beachten:

- Verwenden Sie nicht Ihre produktionsumgebung, wenn es erforderlich ist.
- Wenn Sie nicht über eine Testversion Azure Active Directory-Umgebung verfügen, erhalten Sie eine einmonatige Testversion [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Lernprogramm testen Sie Azure AD einmaliges Anmelden in einer testumgebung. In diesem Lernprogramm beschriebene Szenario besteht aus zwei Hauptbausteinen:

1. Printix aus dem Katalog hinzufügen
2. Konfigurieren und Testen von Azure AD-einmaliges Anmelden

## <a name="adding-printix-from-the-gallery"></a>Printix aus dem Katalog hinzufügen
Um die Integration von Printix in Azure AD zu konfigurieren, müssen Sie Printix der Liste der verwalteten SaaS-apps aus dem Katalog hinzufügen.

**Um Printix aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. In der  **[Azure-Portal](https://portal.azure.com)**, klicken Sie auf den linken Navigationsbereich auf **Azure Active Directory** Symbol. 

    ![Active Directory][1]

2. Navigieren Sie zu **unternehmensanwendungen**. Klicken Sie dann auf **alle Anwendungen**.

    ![Applications][2]
    
3. Neue Anwendung hinzufügen möchten, klicken Sie auf **neue Anwendung** Schaltfläche oben Dialogfeld.

    ![Applications][3]

4. Geben Sie in das Suchfeld **Printix**.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-printix-tutorial/tutorial_printix_search.png)

5. Wählen Sie im Ergebnisbereich, **Printix**, und klicken Sie dann auf **hinzufügen** Schaltfläche, um die Anwendung hinzuzufügen.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-printix-tutorial/tutorial_printix_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD-einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Sie Azure AD einmaliges Anmelden mit Printix basierend auf einen Testbenutzer "Britta Simon" aufgerufen.

Für einmaliges Anmelden funktioniert muss Azure AD zu wissen, was der Benutzer Gegenstück in Printix für einen Benutzer in Azure AD ist. Das heißt, muss eine linkbeziehung zwischen einem Azure AD-Benutzer und die entsprechenden Benutzer in Printix hergestellt werden.

In Printix, weisen Sie den Wert der die **Benutzername** in Azure AD als Wert des der **Benutzername** auf die linkbeziehung zu erstellen.

Zum Konfigurieren und Testen Sie Azure AD einmaliges Anmelden mit Printix, müssen Sie den folgenden Bausteinen ausführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  – Informationen zum Aktivieren Ihrer Benutzer dieses Feature verwenden.
2. **[Erstellen ein Azure AD-Testbenutzer](#creating-an-azure-ad-test-user)**  – Informationen zum Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen eines Testbenutzers Printix](#creating-a-printix-test-user)**  – um eine Entsprechung Britta Simon Printix umfassen, die mit der Azure AD-Darstellung eines Benutzers verknüpft ist.
4. **[Zuweisen von Azure AD-Testbenutzer](#assigning-the-azure-ad-test-user)**  – zum Aktivieren der Britta Simon einmaliges Anmelden für Azure AD verwenden.
5. **[Testen Sie einmaliges Anmelden für](#testing-single-sign-on)**  – zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

In diesem Abschnitt werden Azure AD einmaliges Anmelden im Azure-Portal aktivieren und konfigurieren Sie einmaliges Anmelden in Ihrer Anwendung Printix.

**Führen Sie zum Konfigurieren von Azure AD einmaliges Anmelden mit Printix die folgenden Schritte aus:**

1. Im Azure-Portal auf der **Printix** anwendungsintegrationsseite, klicken Sie auf **des einmaligen Anmeldens**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Auf der **des einmaligen Anmeldens** wählen Sie im Dialogfeld **Modus** als **SAML-basierten anmelden** einmaliges Anmelden aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-printix-tutorial/tutorial_printix_samlbase.png)

3. Auf der **Printix Domänen und URLs** führen die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-printix-tutorial/tutorial_printix_url.png)

    In der **Anmelde-URL** Textfeld eine URL mithilfe des folgenden Musters:`https://<subdomain>.printix.net`

    > [!NOTE] 
    > Der Wert ist keine echte. Aktualisieren Sie den Wert mit der tatsächlichen Anmelde-URL ein. Wenden Sie sich an [Printix Client Support-Team](mailto:support@printix.net) , den Wert abzurufen. 
 
4. Auf der **SAML-Signaturzertifikat** auf **Metadata XML** und speichern Sie die Metadatendatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-printix-tutorial/tutorial_printix_certificate.png) 

5. Klicken Sie auf **speichern** Schaltfläche.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-printix-tutorial/tutorial_general_400.png)

6. Anmelden Sie mit Ihrem Printix-Mandanten als Administrator.

7. Klicken Sie im Menü am oberen Rand klicken Sie auf das Symbol in der oberen rechten Ecke, und wählen Sie "**Authentifizierung**".
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-printix-tutorial/tutorial_printix_06.png)

8. Auf der **Setup** Registerkarte **aktivieren Azure/Office 365-Authentifizierung**
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-printix-tutorial/tutorial_printix_07.png)

9. Auf der **Azure** Registerkarte, geben Sie die Verbundmetadaten-URL auf das Textfeld "**verbundmetadatendokument**". 

    Fügen Sie die Metadaten-XML-Datei in Azure AD heruntergeladenen [Printix Support-Team](mailto:support@printix.net). Klicken Sie dann die XML-Datei hochladen und eine Verbundmetadaten-URL angeben.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-printix-tutorial/tutorial_printix_08.png)
   
10. Klicken Sie auf die "**testen**"Schaltfläche aus, und klicken Sie auf"**OK**" Schaltfläche, wenn der Test erfolgreich war.
   
     Azure active Directory-Seite wird angezeigt, nach dem Klicken auf die **testen** Schaltfläche. "Der Test erfolgreich war" bedeutet hier nach der Eingabe der Anmeldeinformationen für Ihr Azure-Testkonto, die es eingeblendet wird eine Meldung aus "Einstellungen OK getestet". Klicken Sie dann auf die **OK** Schaltfläche.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-printix-tutorial/tutorial_printix_09.png)

11. Klicken Sie auf die **speichern** auf die Schaltfläche "**Authentifizierung**" Seite ".


> [!TIP]
> Erfahren Sie jetzt eine kürzere Version dieser Anweisungen innerhalb der [Azure-Portal](https://portal.azure.com), während Sie die app einrichten.  Nach dem Hinzufügen der diese app aus der **Active Directory > Unternehmensanwendungen** einfach auf die **einmaliges Anmelden für** Registerkarte und Zugriff auf die embedded-Dokumentation über die **Konfiguration** Abschnitt unten. Weitere Informationen zu dieser Funktion embedded-Dokumentation: [Azure AD eingebettet Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen ein Azure AD-Testbenutzer
Das Ziel dieses Abschnitts ist zum Erstellen eines Testbenutzers im Azure-Portal Britta Simon aufgerufen.

![Erstellen von Azure AD-Benutzer][100]

**Führen Sie zum Erstellen eines Testbenutzers in Azure AD die folgenden Schritte aus:**

1. In der **Azure-Portal**, klicken Sie im linken Navigationsbereich auf **Azure Active Directory** Symbol.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-printix-tutorial/create_aaduser_01.png) 

2. Um die Liste der Benutzer anzuzeigen, wechseln Sie zu **Benutzer und Gruppen** , und klicken Sie auf **alle Benutzer**.
    
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-printix-tutorial/create_aaduser_02.png) 

3. So öffnen die **Benutzer** Dialogfeld klicken Sie auf **hinzufügen** oben im Dialogfeld auf.
 
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-printix-tutorial/create_aaduser_03.png) 

4. Auf der **Benutzer** Dialogfeld Seite, die folgenden Schritte aus:
 
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-printix-tutorial/create_aaduser_04.png) 

    a. In der **Namen** Textfeld **BrittaSimon**.

    b. In der **Benutzername** Textfeld die **e-Mail-Adresse** des BrittaSimon.

    c. Wählen Sie **Kennwort anzeigen** und notieren Sie sich den Wert, der die **Kennwort**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-printix-test-user"></a>Erstellen eines Testbenutzers Printix

Das Ziel dieses Abschnitts ist zum Erstellen eines Benutzers Britta Simon in Printix aufgerufen. Printix unterstützt Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Es ist kein Aktionselement, über das Sie in diesem Abschnitt. Während des Installierens Printix zugreifen, wenn er noch nicht vorhanden ist, wird ein neuer Benutzer erstellt. 

> [!NOTE]
> Wenn Sie einen Benutzer manuell erstellen müssen, müssen Sie wenden Sie sich an den [Printix Support-Team](mailto:support@printix.net).
> 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon durch Gewähren des Zugriffs auf Printix Azure einmaliges Anmelden zu verwenden.

![Weisen Sie Benutzer][200] 

**Britta Simon Printix zuweisen möchten, führen Sie die folgenden Schritte aus:**

1. Im Azure-Portal, öffnen Sie die Anwendungen anzuzeigen, und navigieren Sie zu dem Verzeichnisansicht, und wechseln Sie zu **unternehmensanwendungen** klicken Sie dann auf **alle Anwendungen**.

    ![Weisen Sie Benutzer][201] 

2. Wählen Sie in der Liste der Anwendungen **Printix**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-printix-tutorial/tutorial_printix_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Weisen Sie Benutzer][202] 

4. Klicken Sie auf **hinzufügen** Schaltfläche. Wählen Sie dann **Benutzer und Gruppen** auf **Zuweisung hinzufügen** Dialogfeld.

    ![Weisen Sie Benutzer][203]

5. Auf **Benutzer und Gruppen** wählen Sie im Dialogfeld **Britta Simon** in der Benutzerliste.

6. Klicken Sie auf **wählen** Schaltfläche **Benutzer und Gruppen** Dialogfeld.

7. Klicken Sie auf **zuweisen** Schaltfläche **Zuweisung hinzufügen** Dialogfeld.
    
### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

In diesem Abschnitt Testen Sie Ihre Azure AD einmalige Anmelden-Konfiguration über den Zugriffsbereich.

Wenn Sie die Kachel Printix im Zugriffsbereich klicken, Sie sollten automatisch für Ihre Anwendung Printix angemeldete abrufen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zur Integration von SaaS-Apps mit Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was ist Anwendungszugriff und einmaliges Anmelden bei Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-printix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-printix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-printix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-printix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-printix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-printix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-printix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-printix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-printix-tutorial/tutorial_general_203.png

