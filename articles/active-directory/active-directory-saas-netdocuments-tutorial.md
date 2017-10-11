---
title: 'Lernprogramm: Azure Active Directory-Integration in NetDocuments | Microsoft Docs'
description: Erfahren Sie, wie einmaliges Anmelden zwischen Azure Active Directory und NetDocuments konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1a47dc42-1a17-48a2-965e-eca4cfb2f197
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 87c3338d611daa837aa5f079c4b68e0e6fc58455
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Lernprogramm: Azure Active Directory-Integration in NetDocuments

In diesem Lernprogramm erfahren Sie, wie NetDocuments mit Azure Active Directory (Azure AD) integrieren.

Integrieren von NetDocuments in Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf NetDocuments hat
- Sie können Ihre Benutzer für die automatisch signiert netdocuments (Single Sign-On) mit ihren Azure AD-Konten bei abrufen
- Sie können Ihre Konten an einem zentralen Ort - Azure-Portal verwalten.

Wenn Sie weitere Informationen zum Integrieren von SaaS-Apps in Azure AD wissen möchten, finden Sie unter [was Anwendungszugriff und einmaliges Anmelden bei Azure Active Directory ist](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um Azure AD-Integration in NetDocuments zu konfigurieren, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Ein NetDocuments einmaliges Anmelden für Abonnement mit aktivierter

> [!NOTE]
> Um die Schritte in diesem Lernprogramm zu testen, führen Sie wird nicht empfohlen, mithilfe einer produktiven Umgebung.

Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlungen beachten:

- Verwenden Sie nicht Ihre produktionsumgebung, wenn es erforderlich ist.
- Wenn Sie nicht über eine Testversion Azure Active Directory-Umgebung verfügen, erhalten Sie eine einmonatige Testversion [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Lernprogramm testen Sie Azure AD einmaliges Anmelden in einer testumgebung. In diesem Lernprogramm beschriebene Szenario besteht aus zwei Hauptbausteinen:

1. Hinzufügen von NetDocuments aus dem Katalog
2. Konfigurieren und Testen von Azure AD-einmaliges Anmelden

## <a name="adding-netdocuments-from-the-gallery"></a>Hinzufügen von NetDocuments aus dem Katalog
Um die Integration von NetDocuments in Azure AD zu konfigurieren, müssen Sie NetDocuments der Liste der verwalteten SaaS-apps aus dem Katalog hinzufügen.

**Um NetDocuments aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. In der  **[Azure-Portal](https://portal.azure.com)**, klicken Sie auf den linken Navigationsbereich auf **Azure Active Directory** Symbol. 

    ![Active Directory][1]

2. Navigieren Sie zu **unternehmensanwendungen**. Klicken Sie dann auf **alle Anwendungen**.

    ![Applications][2]
    
3. Neue Anwendung hinzufügen möchten, klicken Sie auf **neue Anwendung** Schaltfläche oben Dialogfeld.

    ![Applications][3]

4. Geben Sie in das Suchfeld **NetDocuments**.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_search.png)

5. Wählen Sie im Ergebnisbereich, **NetDocuments**, und klicken Sie dann auf **hinzufügen** Schaltfläche, um die Anwendung hinzuzufügen.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD-einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Azure AD einmaliges Anmelden für NetDocuments basierend auf einen Testbenutzer "Britta Simon" aufgerufen.

Für einmaliges Anmelden funktioniert muss Azure AD zu wissen, was der Benutzer Gegenstück in NetDocuments für einen Benutzer in Azure AD ist. Das heißt, muss eine linkbeziehung zwischen einem Azure AD-Benutzer und die entsprechenden Benutzer in NetDocuments hergestellt werden.

In NetDocuments, weisen Sie den Wert der die **Benutzername** in Azure AD als Wert des der **Benutzername** auf die linkbeziehung zu erstellen.

Zum Konfigurieren und Testen Sie Azure AD einmaliges Anmelden mit NetDocuments, müssen Sie den folgenden Bausteinen ausführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  – Informationen zum Aktivieren Ihrer Benutzer dieses Feature verwenden.
2. **[Erstellen ein Azure AD-Testbenutzer](#creating-an-azure-ad-test-user)**  – Informationen zum Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen eines Testbenutzers NetDocuments](#creating-a-netdocuments-test-user)**  – um eine Entsprechung Britta Simon in NetDocuments verfügen, die mit der Azure AD-Darstellung eines Benutzers verknüpft ist.
4. **[Zuweisen von Azure AD-Testbenutzer](#assigning-the-azure-ad-test-user)**  – zum Aktivieren der Britta Simon einmaliges Anmelden für Azure AD verwenden.
5. **[Testen Sie einmaliges Anmelden für](#testing-single-sign-on)**  – zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

In diesem Abschnitt werden Azure AD einmaliges Anmelden im Azure-Portal aktivieren und konfigurieren Sie einmaliges Anmelden in Ihrer NetDocuments-Anwendung.

**Führen Sie zum Konfigurieren von Azure AD einmaliges Anmelden mit NetDocuments die folgenden Schritte aus:**

1. Im Azure-Portal auf der **NetDocuments** anwendungsintegrationsseite, klicken Sie auf **des einmaligen Anmeldens**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Auf der **des einmaligen Anmeldens** wählen Sie im Dialogfeld **Modus** als **SAML-basierten anmelden** einmaliges Anmelden aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_samlbase.png)

3. Auf der **NetDocuments-Domänen und URLs** führen die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_url.png)

    a. In der **Anmelde-URL** Textfeld eine URL mithilfe des folgenden Musters:`https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`

    b. In der **-Antwort-URL** Textfeld eine URL mithilfe des folgenden Musters:`https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`

    > [!NOTE] 
    > Diese Werte sind nicht real. Aktualisieren Sie diese Werte mit dem tatsächlichen Anmelde-URL und die Antwort-URL ein. Wenden Sie sich an [NetDocuments-Supportteam weiter](https://support.netdocuments.com/hc/) , diese Werte abzurufen.
 
4. Auf der **SAML-Signaturzertifikat** auf **Metadata XML** und speichern Sie die Metadatendatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_certificate.png) 

5. Klicken Sie auf **speichern** Schaltfläche.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-netdocuments-tutorial/tutorial_general_400.png)

6. Melden Sie in einem anderen Webbrowserfenster bei Ihrer NetDocuments-Unternehmenswebsite als Administrator.

7. Wechseln Sie zu **Admin**.

8. Klicken Sie auf **Add- und Remove-Benutzer und Gruppen**.
   
    ![Repository](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "Repository")

9. Klicken Sie auf **erweiterte Authentifizierungsoptionen konfigurieren**.
    
    ![Konfigurieren Sie erweiterte Authentifizierungsoptionen](./media/active-directory-saas-netdocuments-tutorial/ic795048.png "erweiterte Authentifizierungsoptionen konfigurieren")

10. Auf der **Identitätsverbund** im Dialogfeld die folgenden Schritte aus:
   
    ![Federated Identitty](./media/active-directory-saas-netdocuments-tutorial/ic795049.png "Identitty Verbund")
   
    a. Als **Federated Identity Servertyp**Option **Active Directory Federation Services**.
   
    b. Klicken Sie auf **Choose File**, um die heruntergeladene Metadatendatei hochzuladen, die Sie von Azure-Portal heruntergeladen haben.
   
    c. Klicken Sie auf **OK**.

> [!TIP]
> Erfahren Sie jetzt eine kürzere Version dieser Anweisungen innerhalb der [Azure-Portal](https://portal.azure.com), während Sie die app einrichten.  Nach dem Hinzufügen der diese app aus der **Active Directory > Unternehmensanwendungen** einfach auf die **einmaliges Anmelden für** Registerkarte und Zugriff auf die embedded-Dokumentation über die **Konfiguration** Abschnitt unten. Weitere Informationen zu dieser Funktion embedded-Dokumentation: [Azure AD eingebettet Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Erstellen ein Azure AD-Testbenutzer
Das Ziel dieses Abschnitts ist zum Erstellen eines Testbenutzers im Azure-Portal Britta Simon aufgerufen.

![Erstellen von Azure AD-Benutzer][100]

**Führen Sie zum Erstellen eines Testbenutzers in Azure AD die folgenden Schritte aus:**

1. In der **Azure-Portal**, klicken Sie im linken Navigationsbereich auf **Azure Active Directory** Symbol.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_01.png) 

2. Um die Liste der Benutzer anzuzeigen, wechseln Sie zu **Benutzer und Gruppen** , und klicken Sie auf **alle Benutzer**.
    
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_02.png) 

3. So öffnen die **Benutzer** Dialogfeld klicken Sie auf **hinzufügen** oben im Dialogfeld auf.
 
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_03.png) 

4. Auf der **Benutzer** Dialogfeld Seite, die folgenden Schritte aus:
 
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_04.png) 

    a. In der **Namen** Textfeld **BrittaSimon**.

    b. In der **Benutzername** Textfeld die **e-Mail-Adresse** des BrittaSimon.

    c. Wählen Sie **Kennwort anzeigen** und notieren Sie sich den Wert, der die **Kennwort**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-netdocuments-test-user"></a>Erstellen eines Testbenutzers NetDocuments

Um Azure AD-Benutzer zum Anmelden bei NetDocuments zu aktivieren, müssen sie in NetDocuments bereitgestellt werden.  
Bei NetDocuments erfolgt die Bereitstellung manuell ausgeführt.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich auf Ihre **NetDocuments** -Unternehmenswebsite als Administrator.

2. Klicken Sie im Menü am oberen Rand auf **Admin**.
   
    ![Admin](./media/active-directory-saas-netdocuments-tutorial/ic795051.png "Admin")

3. Klicken Sie auf **Add- und Remove-Benutzer und Gruppen**.
   
    ![Repository](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "Repository")

4. In der **e-Mail-Adresse** Textbox, geben Sie die e-Mail-Adresse eines gültigen Azure Active Directory-Kontos Sie bereitstellen möchten, und klicken Sie dann auf **Add User**.
   
    ![E-Mail-Adresse](./media/active-directory-saas-netdocuments-tutorial/ic795053.png "e-Mail-Adresse")
   
   >[!NOTE]
   >Der Azure Active Directory-Kontoinhaber erhält eine e-Mail, die enthält einen Link zum Bestätigen des Kontos, bevor es aktiviert wird. Können Sie alle anderen NetDocuments Benutzerkonten oder APIs von NetDocuments für die Bereitstellung von Azure Active Directory-Benutzerkonten.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon durch Gewähren des Zugriffs auf NetDocuments Azure einmaliges Anmelden zu verwenden.

![Weisen Sie Benutzer][200] 

**Britta Simon NetDocuments zuweisen möchten, führen Sie die folgenden Schritte aus:**

1. Im Azure-Portal, öffnen Sie die Anwendungen anzuzeigen, und navigieren Sie zu dem Verzeichnisansicht, und wechseln Sie zu **unternehmensanwendungen** klicken Sie dann auf **alle Anwendungen**.

    ![Weisen Sie Benutzer][201] 

2. Wählen Sie in der Liste der Anwendungen **NetDocuments**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Weisen Sie Benutzer][202] 

4. Klicken Sie auf **hinzufügen** Schaltfläche. Wählen Sie dann **Benutzer und Gruppen** auf **Zuweisung hinzufügen** Dialogfeld.

    ![Weisen Sie Benutzer][203]

5. Auf **Benutzer und Gruppen** wählen Sie im Dialogfeld **Britta Simon** in der Benutzerliste.

6. Klicken Sie auf **wählen** Schaltfläche **Benutzer und Gruppen** Dialogfeld.

7. Klicken Sie auf **zuweisen** Schaltfläche **Zuweisung hinzufügen** Dialogfeld.
    
### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

In diesem Abschnitt Testen Sie Ihre Azure AD einmalige Anmelden-Konfiguration über den Zugriffsbereich.

Wenn Sie die Kachel NetDocuments im Zugriffsbereich klicken, Sie sollten automatisch für Ihre Anwendung NetDocuments angemeldete abrufen.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zur Integration von SaaS-Apps mit Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was ist Anwendungszugriff und einmaliges Anmelden bei Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_203.png

