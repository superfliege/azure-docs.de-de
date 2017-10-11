---
title: 'Lernprogramm: Azure Active Directory-Integration mit 8 x 8 virtuelle Office | Microsoft Docs'
description: Erfahren Sie, wie einmaliges Anmelden zwischen Azure Active Directory und 8 x 8 virtuelle Office konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: d8dcf0171b93fec15347e810a1b525bd815dbf04
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Lernprogramm: Azure Active Directory-Integration mit 8 x 8 virtuelle Office

In diesem Lernprogramm erfahren Sie, wie 8 x 8 virtuelle Office mit Azure Active Directory (Azure AD) integrieren.

Die Integration von 8 x 8 bietet virtuelle Office mit Azure AD die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf 8 x 8 virtuelle Office hat
- Sie können Ihre Benutzer für die automatisch signiert 8 x 8 virtuelle Office (Single Sign-On) bei Abrufen mit ihren Azure AD-Konten aktivieren.
- Sie können Ihre Konten an einem zentralen Ort - Azure-Portal verwalten.

Wenn Sie weitere Informationen zum Integrieren von SaaS-Apps in Azure AD wissen möchten, finden Sie unter [was Anwendungszugriff und einmaliges Anmelden bei Azure Active Directory ist](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren von Azure AD-Integration mit 8 x 8 virtuelle Office benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Ein 8 x 8 virtuelle Office einmaliges Anmelden für Abonnement mit aktivierter

> [!NOTE]
> Um die Schritte in diesem Lernprogramm zu testen, führen Sie wird nicht empfohlen, mithilfe einer produktiven Umgebung.

Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlungen beachten:

- Verwenden Sie nicht Ihre produktionsumgebung, wenn es erforderlich ist.
- Wenn Sie nicht über eine Testversion Azure Active Directory-Umgebung verfügen, erhalten Sie eine einmonatige Testversion [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Lernprogramm testen Sie Azure AD einmaliges Anmelden in einer testumgebung. In diesem Lernprogramm beschriebene Szenario besteht aus zwei Hauptbausteinen:

1. 8 x 8 virtuelle Office hinzufügen aus dem Katalog
2. Konfigurieren und Testen von Azure AD-einmaliges Anmelden

## <a name="adding-8x8-virtual-office-from-the-gallery"></a>8 x 8 virtuelle Office hinzufügen aus dem Katalog
Um die 8 x 8 virtuelle Office-Integration in Azure AD zu konfigurieren, müssen Sie 8 x 8 virtuelle Office die Liste der verwalteten SaaS-apps aus dem Katalog hinzufügen.

**Um 8 x 8 virtuelle Office aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. In der  **[Azure-Portal](https://portal.azure.com)**, klicken Sie auf den linken Navigationsbereich auf **Azure Active Directory** Symbol. 

    ![Active Directory][1]

2. Navigieren Sie zu **unternehmensanwendungen**. Klicken Sie dann auf **alle Anwendungen**.

    ![Applications][2]
    
3. Neue Anwendung hinzufügen möchten, klicken Sie auf **neue Anwendung** Schaltfläche oben Dialogfeld.

    ![Applications][3]

4. Geben Sie in das Suchfeld **8 x 8 virtuelle Office**.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_search.png)

5. Wählen Sie im Ergebnisbereich, **8 x 8 virtuelle Office**, und klicken Sie dann auf **hinzufügen** Schaltfläche, um die Anwendung hinzuzufügen.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD-einmaliges Anmelden
In diesem Abschnitt können Sie auch konfigurieren und Testen Sie Azure AD einmaliges Anmelden mit 8 x 8, die einen Testbenutzer namens "Britta Simon." virtuelle Office anhand

Für einmaliges Anmelden funktioniert muss Azure AD den Gegenstück in 8 x 8 Benutzer kennen, die virtuelle Office für einen Benutzer in Azure AD ist. Das heißt, muss eine linkbeziehung zwischen einem Azure AD-Benutzer und die entsprechenden Benutzer in 8 x 8 virtuelle Office eingerichtet werden kann.

In 8 x 8 virtuelle Zweigstelle, weisen Sie den Wert der die **Benutzername** in Azure AD als Wert des der **Benutzername** auf die linkbeziehung zu erstellen.

Zum Konfigurieren und Testen Sie Azure AD einmaliges Anmelden mit 8 x 8 virtuelle Office, müssen Sie den folgenden Bausteinen ausführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  – Informationen zum Aktivieren Ihrer Benutzer dieses Feature verwenden.
2. **[Erstellen ein Azure AD-Testbenutzer](#creating-an-azure-ad-test-user)**  – Informationen zum Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen einen 8 x 8 virtuelle Office Testbenutzer](#creating-a-8x8-virtual-office-test-user)**  – um eine Entsprechung Britta Simon 8 x 8 virtuelle Office umfassen, die mit der Azure AD-Darstellung eines Benutzers verknüpft ist.
4. **[Zuweisen von Azure AD-Testbenutzer](#assigning-the-azure-ad-test-user)**  – zum Aktivieren der Britta Simon einmaliges Anmelden für Azure AD verwenden.
5. **[Testen Sie einmaliges Anmelden für](#testing-single-sign-on)**  – zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

In diesem Abschnitt werden Azure AD einmaliges Anmelden im Azure-Portal aktivieren und konfigurieren Sie einmaliges Anmelden, in der 8 x 8 virtuelle Office-Anwendung.

**Führen Sie zum Konfigurieren von Azure AD einmaliges Anmelden mit 8 x 8 virtuelle Office die folgenden Schritte aus:**

1. Im Azure-Portal auf der **8 x 8 virtuelle Office** anwendungsintegrationsseite, klicken Sie auf **des einmaligen Anmeldens**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Auf der **des einmaligen Anmeldens** wählen Sie im Dialogfeld **Modus** als **SAML-basierten anmelden** einmaliges Anmelden aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_samlbase.png)

3. Auf der **8 x 8 virtuellen Office Domänen und URLs** führen die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_url.png)

    a. In der **Bezeichner** Textfeld eine URL mithilfe des folgenden Musters:

    | `https://sso.8x8.com/<companyname>` |
    | `https://www.8x8.com/<companyname>` |
    | `https://sso.8x8pilot.com/<companyname>` |

    b. In der **-Antwort-URL** Textfeld eine URL mithilfe des folgenden Musters:

    | `https://<subdomain>.8x8.com/saml2` |
    | `https://<subdomain>.8x8pilot.com/saml2`|

    > [!NOTE] 
    > Diese Werte sind nicht real. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und die Antwort-URL ein. Wenden Sie sich an [8 x 8 virtuelle Office Support-Team](https://www.8x8.com/about-us/contact-us) , diese Werte abzurufen.
 


4. Auf der **SAML-Signaturzertifikat** auf **Zertifikat (Raw)** und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_certificate.png) 

5. Klicken Sie auf **speichern** Schaltfläche.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_400.png)

6. Auf der **8 x 8 virtuelle Office Konfiguration** auf **konfigurieren 8 x 8 virtuelle Office** öffnen **Anmelden konfigurieren** Fenster. Kopieren der **URL Abmeldung, SAML-Entitäts-ID und SAML anmelden Dienst-URL für einmalige** aus der **Quick Reference-Abschnitt.**

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_configure.png) 

7. Anmelden Sie mit Ihrem 8 x 8 virtuelle Office-Mandanten als Administrator.

8. Wählen Sie **virtuellen Office Konto Mgr** im Bereich der Anwendung.
   
    ![Konfigurieren Sie auf der Seite der App](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

9. Wählen Sie **Business** Konto verwalten, und klicken Sie auf **anmelden** Schaltfläche.
   
    ![Konfigurieren Sie auf der Seite der App](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

10. Klicken Sie auf **Konten** Registerkarte in der Liste.
   
    ![Konfigurieren Sie auf der Seite der App](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

11. Klicken Sie auf **einmaliges Anmelden** in der Liste der Konten.
   
    ![Konfigurieren Sie auf der Seite der App](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

12. Wählen Sie **einmaliges Anmelden** unter Authentifizierungsmethode, und klicken Sie auf **SAML**.
    
    ![Konfigurieren Sie auf der Seite der App](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

13. Kopie **SAML SSO URL**, **melden, die Dienst-URL für einmaliges** und **Aussteller-URL** in Azure AD **Anmelde-URL**, **URL Abmelden** und **Aussteller-URL** in 8 x 8 virtuelle Zweigstelle. 
    
    ![Konfigurieren Sie auf der Seite der App](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)
    
14. Klicken Sie auf **Browser** Schaltfläche, um das Zertifikat hochzuladen, die Sie von Azure AD heruntergeladen, und klicken Sie auf die **speichern** Schaltfläche.

> [!TIP]
> Erfahren Sie jetzt eine kürzere Version dieser Anweisungen innerhalb der [Azure-Portal](https://portal.azure.com), während Sie die app einrichten.  Nach dem Hinzufügen der diese app aus der **Active Directory > Unternehmensanwendungen** einfach auf die **einmaliges Anmelden für** Registerkarte und Zugriff auf die embedded-Dokumentation über die **Konfiguration** Abschnitt unten. Weitere Informationen zu dieser Funktion embedded-Dokumentation: [Azure AD eingebettet Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen ein Azure AD-Testbenutzer
Das Ziel dieses Abschnitts ist zum Erstellen eines Testbenutzers im Azure-Portal Britta Simon aufgerufen.

![Erstellen von Azure AD-Benutzer][100]

**Führen Sie zum Erstellen eines Testbenutzers in Azure AD die folgenden Schritte aus:**

1. In der **Azure-Portal**, klicken Sie im linken Navigationsbereich auf **Azure Active Directory** Symbol.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_01.png) 

2. Um die Liste der Benutzer anzuzeigen, wechseln Sie zu **Benutzer und Gruppen** , und klicken Sie auf **alle Benutzer**.
    
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_02.png) 

3. So öffnen die **Benutzer** Dialogfeld klicken Sie auf **hinzufügen** oben im Dialogfeld auf.
 
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png) 

4. Auf der **Benutzer** Dialogfeld Seite, die folgenden Schritte aus:
 
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png) 

    a. In der **Namen** Textfeld **BrittaSimon**.

    b. In der **Benutzername** Textfeld die **e-Mail-Adresse** des BrittaSimon.

    c. Wählen Sie **Kennwort anzeigen** und notieren Sie sich den Wert, der die **Kennwort**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-8x8-virtual-office-test-user"></a>Einen 8 x 8 virtuelle Office Testbenutzer erstellen

Das Ziel dieses Abschnitts ist zum Erstellen eines Benutzers Britta Simon in 8 x 8 virtuelle Office aufgerufen. 8 x 8 unterstützt virtuelle Office Just-in-Time-Bereitstellung, die standardmäßig aktiviert.

Es ist kein Aktionselement, über das Sie in diesem Abschnitt. Während des Installierens 8 x 8 virtuelle Office zugreifen, wenn er noch nicht vorhanden ist, wird ein neuer Benutzer erstellt. 

>[!NOTE]
>Wenn Sie einen Benutzer manuell erstellen müssen, müssen Sie wenden Sie sich an den [8 x 8 virtuelle Office Support-Team](https://www.8x8.com/about-us/contact-us). 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon durch Gewähren des Zugriffs auf 8 x 8 virtuelle Office Azure einmaliges Anmelden zu verwenden.

![Weisen Sie Benutzer][200] 

**8 x 8 virtuelle Office Britta Simon zuweisen möchten, führen Sie die folgenden Schritte aus:**

1. Im Azure-Portal, öffnen Sie die Anwendungen anzuzeigen, und navigieren Sie zu dem Verzeichnisansicht, und wechseln Sie zu **unternehmensanwendungen** klicken Sie dann auf **alle Anwendungen**.

    ![Weisen Sie Benutzer][201] 

2. Wählen Sie in der Liste der Anwendungen **8 x 8 virtuelle Office**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Weisen Sie Benutzer][202] 

4. Klicken Sie auf **hinzufügen** Schaltfläche. Wählen Sie dann **Benutzer und Gruppen** auf **Zuweisung hinzufügen** Dialogfeld.

    ![Weisen Sie Benutzer][203]

5. Auf **Benutzer und Gruppen** wählen Sie im Dialogfeld **Britta Simon** in der Benutzerliste.

6. Klicken Sie auf **wählen** Schaltfläche **Benutzer und Gruppen** Dialogfeld.

7. Klicken Sie auf **zuweisen** Schaltfläche **Zuweisung hinzufügen** Dialogfeld.
    
### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

In diesem Abschnitt Testen Sie Ihre Azure AD einmalige Anmelden-Konfiguration über den Zugriffsbereich.

Wenn Sie die 8 x 8 virtuelle Office Kachel im Zugriffsbereich klicken, Sie sollten automatisch an die 8 x 8 virtuelle Office-Anwendung auf abrufen.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zur Integration von SaaS-Apps mit Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was ist Anwendungszugriff und einmaliges Anmelden bei Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png

