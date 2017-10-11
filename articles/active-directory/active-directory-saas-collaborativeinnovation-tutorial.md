---
title: 'Lernprogramm: Azure Active Directory-Integration mit Collaborative Innovation | Microsoft Docs'
description: Erfahren Sie, wie einmaliges Anmelden zwischen Azure Active Directory und Zusammenarbeit Innovation konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bba95df3-75a4-4a93-8805-b3a8aa3d4861
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: 5706ba9f4e7c92de77a0edc5146aa150de379c9f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-collaborative-innovation"></a>Lernprogramm: Azure Active Directory-Integration mit Collaborative Innovation

In diesem Lernprogramm erfahren Sie, wie Azure Active Directory (Azure AD) gemeinsamen Innovation integriert.

Gemeinsame Innovation Integrieren von Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf gemeinschaftliche Innovation hat
- Sie können Ihre Benutzer für die automatisch signiert (Single Sign-On) gemeinsamen Innovation bei Abrufen mit ihren Azure AD-Konten aktivieren.
- Sie können Ihre Konten an einem zentralen Ort - Azure-Portal verwalten.

Wenn Sie weitere Informationen zum Integrieren von SaaS-Apps in Azure AD wissen möchten, finden Sie unter [was Anwendungszugriff und einmaliges Anmelden bei Azure Active Directory ist](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren von Azure AD-Integration mit Collaborative Innovation benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Eine gemeinsame Innovation für einmaliges Anmelden Abonnement mit aktivierter

> [!NOTE]
> Um die Schritte in diesem Lernprogramm zu testen, führen Sie wird nicht empfohlen, mithilfe einer produktiven Umgebung.

Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlungen beachten:

- Verwenden Sie nicht Ihre produktionsumgebung, wenn es erforderlich ist.
- Wenn Sie nicht über eine Testversion Azure Active Directory-Umgebung verfügen, erhalten Sie eine einmonatige Testversion [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Lernprogramm testen Sie Azure AD einmaliges Anmelden in einer testumgebung. In diesem Lernprogramm beschriebene Szenario besteht aus zwei Hauptbausteinen:

1. Gemeinsame Innovation aus dem Katalog hinzufügen
2. Konfigurieren und Testen von Azure AD-einmaliges Anmelden

## <a name="adding-collaborative-innovation-from-the-gallery"></a>Gemeinsame Innovation aus dem Katalog hinzufügen
Um die Integration der Berichterstellungsumgebung Innovation in Azure Active Directory zu konfigurieren, müssen Sie gemeinsame Innovation der Liste der verwalteten SaaS-apps aus dem Katalog hinzufügen.

**Um die Zusammenarbeit Innovation aus dem Katalog hinzufügen, können führen Sie die folgenden Schritte aus:**

1. In der  **[Azure-Portal](https://portal.azure.com)**, klicken Sie auf den linken Navigationsbereich auf **Azure Active Directory** Symbol. 

    ![Active Directory][1]

2. Navigieren Sie zu **unternehmensanwendungen**. Klicken Sie dann auf **alle Anwendungen**.

    ![Applications][2]
    
3. Neue Anwendung hinzufügen möchten, klicken Sie auf **neue Anwendung** Schaltfläche oben Dialogfeld.

    ![Applications][3]

4. Geben Sie in das Suchfeld **gemeinschaftliche Innovation**.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_search.png)

5. Wählen Sie im Ergebnisbereich, **gemeinschaftliche Innovation**, und klicken Sie dann auf **hinzufügen** Schaltfläche, um die Anwendung hinzuzufügen.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD-einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Sie Azure AD einmaliges Anmelden für mit Collaborative Innovation basierend auf einen Testbenutzer namens "Britta Simon."

Für einmaliges Anmelden funktioniert muss Azure AD zu wissen, was der Benutzer Gegenstück in Zusammenarbeit Innovation für einen Benutzer in Azure AD ist. Das heißt, muss eine linkbeziehung zwischen Azure AD-Benutzers und die entsprechenden Benutzer in Zusammenarbeit Innovation hergestellt werden.

In Zusammenarbeit Innovation, weisen Sie den Wert der die **Benutzername** in Azure AD als Wert des der **Benutzername** auf die linkbeziehung zu erstellen.

Zum Konfigurieren und Testen Sie Azure AD einmaliges Anmelden mit Collaborative Innovation, müssen Sie den folgenden Bausteinen ausführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  – Informationen zum Aktivieren Ihrer Benutzer dieses Feature verwenden.
2. **[Erstellen ein Azure AD-Testbenutzer](#creating-an-azure-ad-test-user)**  – Informationen zum Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen eines Testbenutzers gemeinschaftliche Innovation](#creating-a-collaborative-innovation-test-user)**  – um eine Entsprechung Britta Simon gemeinschaftliche Innovation umfassen, die mit der Azure AD-Darstellung eines Benutzers verknüpft ist.
4. **[Zuweisen von Azure AD-Testbenutzer](#assigning-the-azure-ad-test-user)**  – zum Aktivieren der Britta Simon einmaliges Anmelden für Azure AD verwenden.
5. **[Testen Sie einmaliges Anmelden für](#testing-single-sign-on)**  – zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

In diesem Abschnitt werden Azure AD einmaliges Anmelden im Azure-Portal aktivieren und konfigurieren Sie einmaliges Anmelden in Ihrer Anwendung gemeinschaftliche Innovation.

**Zum Konfigurieren von Azure AD einmaliges Anmelden mit Collaborative Innovation führen Sie die folgenden Schritte aus:**

1. Im Azure-Portal auf der **gemeinschaftliche Innovation** anwendungsintegrationsseite, klicken Sie auf **des einmaligen Anmeldens**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Auf der **des einmaligen Anmeldens** wählen Sie im Dialogfeld **Modus** als **SAML-basierten anmelden** einmaliges Anmelden aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_samlbase.png)

3. Auf der **gemeinschaftliche Innovation Domänen und URLs** führen die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_url.png)

    a. In der **Anmelde-URL** Textfeld eine URL mithilfe des folgenden Musters:`https://<instancename>.foundry.<companyname>.com/`

    b. In der **Bezeichner** Textfeld eine URL mithilfe des folgenden Musters:`https://<instancename>.foundry.<companyname>.com`
    
    > [!NOTE] 
    > Diese Werte sind nicht real. Aktualisieren Sie diese Werte, mit dem tatsächlichen Anmelde-URL und Bezeichner. Wenden Sie sich an [gemeinschaftliche Innovation Client Support-Team](https://www.unilever.com/contact/) , diese Werte abzurufen.  

4. Gemeinsame Innovation-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung ein. Sie können die Werte dieser Attribute aus verwalten, die "**Benutzerattribute**" Abschnitt auf der anwendungsintegrationsseite. Der folgende Screenshot zeigt ein Beispiel dafür an.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-collaborativeinnovation-tutorial/attribute.png)
    
5. Klicken Sie auf **anzeigen und alle anderen Benutzerattribute bearbeiten** Kontrollkästchen in der **Benutzerattribute** Abschnitt zu die Attributen zu erweitern. Führen Sie die folgenden Schritte auf jedem der angezeigten Attribute-

    | Attributname | Attributwert |
    | ---------------| --------------- |    
    | "givenName" | User.GivenName |
    | surname | User.Surname |
    | EmailAddress | User.userPrincipalName |
    | Name | User.userPrincipalName |

    a. Klicken Sie auf das Attribut zum Öffnen der **Attribut bearbeiten** Fenster.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-collaborativeinnovation-tutorial/url_update.png)

    b. Löschen Sie den URL-Wert aus der **Namespace**.
    
    c. Klicken Sie auf **Ok** um die Einstellung zu speichern.

6. Auf der **SAML-Signaturzertifikat** auf **Metadata XML** und speichern Sie die Metadatendatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_certificate.png) 

7. Klicken Sie auf **speichern** Schaltfläche.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_400.png)

8. So konfigurieren Sie einmaliges Anmelden auf **gemeinschaftliche Innovation** Seite müssen Sie das heruntergeladene senden **Metadata XML** auf [gemeinschaftliche Innovation Support-Team](https://www.unilever.com/contact/). Sie legen diese Einstellung, um die SAML SSO-Verbindung auf beiden Seiten richtig festgelegt haben.

> [!TIP]
> Erfahren Sie jetzt eine kürzere Version dieser Anweisungen innerhalb der [Azure-Portal](https://portal.azure.com), während Sie die app einrichten.  Nach dem Hinzufügen der diese app aus der **Active Directory > Unternehmensanwendungen** einfach auf die **einmaliges Anmelden für** Registerkarte und Zugriff auf die embedded-Dokumentation über die **Konfiguration** Abschnitt unten. Weitere Informationen zu dieser Funktion embedded-Dokumentation: [Azure AD eingebettet Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen ein Azure AD-Testbenutzer
Das Ziel dieses Abschnitts ist zum Erstellen eines Testbenutzers im Azure-Portal Britta Simon aufgerufen.

![Erstellen von Azure AD-Benutzer][100]

**Führen Sie zum Erstellen eines Testbenutzers in Azure AD die folgenden Schritte aus:**

1. In der **Azure-Portal**, klicken Sie im linken Navigationsbereich auf **Azure Active Directory** Symbol.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_01.png) 

2. Um die Liste der Benutzer anzuzeigen, wechseln Sie zu **Benutzer und Gruppen** , und klicken Sie auf **alle Benutzer**.
    
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_02.png) 

3. So öffnen die **Benutzer** Dialogfeld klicken Sie auf **hinzufügen** oben im Dialogfeld auf.
 
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_03.png) 

4. Auf der **Benutzer** Dialogfeld Seite, die folgenden Schritte aus:
 
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_04.png) 

    a. In der **Namen** Textfeld **BrittaSimon**.

    b. In der **Benutzername** Textfeld die **e-Mail-Adresse** des BrittaSimon.

    c. Wählen Sie **Kennwort anzeigen** und notieren Sie sich den Wert, der die **Kennwort**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-collaborative-innovation-test-user"></a>Erstellen eines Testbenutzers gemeinschaftliche Innovation

Um Azure AD-Benutzer zum Anmelden bei gemeinschaftliche Innovation zu aktivieren, müssen sie in Zusammenarbeit Innovation bereitgestellt werden.  

Bei dieser Anwendung wird die Bereitstellung automatisch als Just-in-Time-benutzerbereitstellung von der Anwendung unterstützt. Es ist daher nicht erforderlich, hier keine Schritte auszuführen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon durch Gewähren des Zugriffs auf gemeinschaftliche Innovation Azure einmaliges Anmelden zu verwenden.

![Weisen Sie Benutzer][200] 

**Gemeinsame Innovation Britta Simon zuweisen möchten, führen Sie die folgenden Schritte aus:**

1. Im Azure-Portal, öffnen Sie die Anwendungen anzuzeigen, und navigieren Sie zu dem Verzeichnisansicht, und wechseln Sie zu **unternehmensanwendungen** klicken Sie dann auf **alle Anwendungen**.

    ![Weisen Sie Benutzer][201] 

2. Wählen Sie in der Liste der Anwendungen **gemeinschaftliche Innovation**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Weisen Sie Benutzer][202] 

4. Klicken Sie auf **hinzufügen** Schaltfläche. Wählen Sie dann **Benutzer und Gruppen** auf **Zuweisung hinzufügen** Dialogfeld.

    ![Weisen Sie Benutzer][203]

5. Auf **Benutzer und Gruppen** wählen Sie im Dialogfeld **Britta Simon** in der Benutzerliste.

6. Klicken Sie auf **wählen** Schaltfläche **Benutzer und Gruppen** Dialogfeld.

7. Klicken Sie auf **zuweisen** Schaltfläche **Zuweisung hinzufügen** Dialogfeld.
    
### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

In diesem Abschnitt Testen Sie Ihre Azure AD einmalige Anmelden-Konfiguration über den Zugriffsbereich.

Wenn Sie die Kachel gemeinschaftliche Innovation im Zugriffsbereich klicken, sollten Sie die Anmeldeseite der Zusammenarbeit Innovation Anwendung abrufen.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zur Integration von SaaS-Apps mit Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was ist Anwendungszugriff und einmaliges Anmelden bei Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_203.png

