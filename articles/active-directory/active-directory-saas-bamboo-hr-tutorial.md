---
title: 'Tutorial: Azure Active Directory-Integration in BambooHR | Microsoft-Dokumentation'
description: In diesem Artikel erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und BambooHR konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f826b5d2-9c64-47df-bbbf-0adf9eb0fa71
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: jeedes
ms.openlocfilehash: c63a625c1eca7008c751d6904f182f76ee5f343b
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34349719"
---
# <a name="tutorial-azure-active-directory-integration-with-bamboohr"></a>Tutorial: Azure Active Directory-Integration in BambooHR

In diesem Tutorial erfahren Sie, wie Sie Azure Active Directory (Azure AD) in BambooHR integrieren.

Die Integration von Azure AD in BambooHR bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf BambooHR hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei BambooHR anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort, im Azure-Portal, verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Für die Konfiguration der Azure AD-Integration in BambooHR benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges BambooHR-Abonnement

> [!NOTE]
> Es wird nicht empfohlen, zum Testen der Schritte in diesem Tutorial eine Produktionsumgebung zu verwenden.

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [kostenlose einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. 

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von BambooHR aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="add-bamboohr-from-the-gallery"></a>Hinzufügen von BambooHR aus dem Katalog
Zum Konfigurieren der Integration von BambooHR in Azure AD fügen Sie BambooHR aus dem Katalog zur Liste mit den verwalteten SaaS-Apps wie folgt hinzu:

1. Wählen Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) die Option **Azure Active Directory** aus. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Wählen Sie **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Bereich „Unternehmensanwendungen“][2]
    
3. Wählen Sie zum Hinzufügen einer Anwendung **Neue Anwendung** aus.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld als Suchbegriff **BambooHR** ein. Wählen Sie in der Ergebnisliste **BambooHR** und dann **Hinzufügen**.

    ![BambooHR in der Ergebnisliste](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei BambooHR basierend auf einem Testbenutzer namens Britta Simon.

Damit SSO funktioniert, muss Azure AD Kenntnis darüber haben, welches das Pendant zu dem Benutzer in BambooHR ist. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in BambooHR muss eine Linkbeziehung eingerichtet werden.

Um die Linkbeziehung in BambooHR herzustellen, weisen Sie den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** in BambooHR zu.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD mit BambooHR führen Sie die Bausteine in den nächsten fünf Abschnitten aus.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie wie folgt das einmalige Anmelden von Azure AD im Azure-Portal und führen die entsprechende Konfiguration in Ihrer BambooHR-Anwendung durch:

1. Wählen Sie im Azure-Portal auf der Anwendungsintegrationsseite für **BambooHR** die Option **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Fenster **Einmaliges Anmelden** in der Dropdownliste **Modus** die Option **SAML-basierte Anmeldung** aus.
 
    ![Fenster „Einmaliges Anmelden“](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_samlbase.png)

3. Gehen Sie unter **Domäne und URLs für BambooHR** folgendermaßen vor:

    ![Der Abschnitt zu Domäne und URLs von BambooHR](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_url.png)

    a. Geben Sie im Feld **URL für Anmeldung** eine URL im folgenden Format ein: `https://<company>.bamboohr.com`.

    b. Geben Sie im Feld **Bezeichner** einen Wert ein: `BambooHR-SAML`.

    > [!NOTE] 
    > Der Wert **Anmelde-URL** entspricht nicht dem tatsächlichen Wert. Aktualisieren Sie sie mit der tatsächlichen Anmelde-URL. Wenden Sie sich an das [Kundensupportteam von BambooHR](https://www.bamboohr.com/contact.php), um diesen Wert zu erhalten. 
 
4. Wählen Sie unter **SAML-Signaturzertifikat** die Option **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_certificate.png) 

5. Wählen Sie **Speichern**aus.

    ![Die Schaltfläche „Speichern“](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_400.png)

6. Wählen Sie im Abschnitt **BambooHR-Konfiguration** die Option **BambooHR konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie im Abschnitt **Kurzübersicht** die **URL für den SAML-SSO-Dienst** für die spätere Verwendung.

    ![BambooHR-Konfiguration](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_configure.png) 

7. Melden Sie sich in einem neuen Fenster bei der BambooHR-Unternehmenswebsite als Administrator an.

8. Gehen Sie auf der Startseite wie folgt vor:
   
    ![Die BambooHR-SSO-Seite](./media/active-directory-saas-bamboo-hr-tutorial/ic796691.png "Einmaliges Anmelden")   

    a. Wählen Sie **Apps**.
   
    b. Wählen Sie im Bereich **Apps** die Option **Single Sign-On** (Einmaliges Anmelden).
   
    c. Wählen Sie **SAML Single Sign-On** (Einmaliges Anmelden für SAML).

9. Gehen Sie im Bereich **SAML Single Sign-On** (Einmaliges Anmelden für SAML) wie folgt vor:
   
    ![Bereich „Einmaliges Anmelden für SAML“](./media/active-directory-saas-bamboo-hr-tutorial/IC796692.png "Einmaliges Anmelden für SAML")
   
    a. Fügen Sie im Feld **SSO Login Url** (SSO-Anmelde-URL) die **URL für den SAML-SSO-Dienst** ein, die Sie aus dem Azure-Portal in Schritt 6 kopiert haben.
      
    b. Öffnen Sie im Editor das Base64-codierte Zertifikat, das Sie aus dem Azure-Portal heruntergeladen haben, kopieren Sie den Inhalt, und fügen Sie ihn anschließend in das Feld **X.509-Zertifikat** ein.
   
    c. Wählen Sie **Speichern**aus.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen. Nachdem Sie diese App über den Abschnitt **Active Directory** > **Unternehmensanwendungen** hinzugefügt haben, navigieren Sie einfach zur Registerkarte **Einmaliges Anmelden**, und rufen Sie am unteren Rand im Abschnitt **Konfiguration** die eingebettete Dokumentation auf. Weitere Informationen finden Sie unter [Verwalten des einmaligen Anmeldens für Unternehmens-Apps]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt wird über das Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.

   ![Erstellen des Azure AD-Testbenutzers „Britta Simon“][100]

Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:

1. Klicken Sie im linken Bereich des Azure-Portals auf **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und wählen Sie dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_02.png)

3. Wählen Sie oben im Bereich **Alle Benutzer** die Option **Hinzufügen** aus.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_03.png)

4. Gehen Sie im Fenster **Benutzer** wie folgt vor:

    ![Fenster „Benutzer“](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-bamboohr-test-user"></a>Erstellen eines BambooHR-Testbenutzers

Damit sich Azure AD-Benutzer bei BambooHR anmelden können, richten Sie diese wie folgt manuell in BambooHR ein:

1. Melden Sie sich bei Ihrer **BambooHR**-Website als Administrator an.

2. Wählen Sie oben in der Symbolleiste **Einstellungen**.
   
    ![Die Schaltfläche „Einstellungen“](./media/active-directory-saas-bamboo-hr-tutorial/IC796694.png "Einstellung")

3. Wählen Sie **Übersicht**.

4. Wählen Sie im linken Bereich **Security** (Sicherheit) > **Users** (Benutzer).

5. Geben Sie Benutzername, Kennwort und E-Mail-Adresse eines gültigen Azure AD-Kontos ein, das Sie einrichten möchten.

6. Wählen Sie **Speichern**aus.
        
>[!NOTE]
>Zum Einrichten von Azure AD-Benutzerkonten können Sie auch Tools oder APIs zum Erstellen von BambooHR-Benutzerkonten verwenden.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

Ermöglichen Sie dem Benutzer Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf BambooHR gewähren.

![Zuweisen der Benutzerrolle][200] 

Gehen Sie wie folgt vor, um die Zuweisung von Britta Simon zu BambooHR durchzuführen:

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, und wählen Sie dann **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste **Unternehmensanwendungen** den Eintrag **BambooHR** aus.

    ![BambooHR-Link in der Liste der Unternehmensanwendungen](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_app.png)  

3. Wählen Sie auf der linken Seite **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Wählen Sie die Schaltfläche **Hinzufügen**, und wählen Sie dann im Bereich **Zuweisung hinzufügen** die Option **Benutzer und Gruppen**.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Fenster **Benutzer und Gruppen** in der Liste **Benutzer** die Option **Britta Simon** aus.

6. Wählen Sie die Schaltfläche **Auswählen** aus.

7. Wählen Sie im Fenster **Zuweisung hinzufügen** die Schaltfläche **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

Testen Sie die Azure AD-Konfiguration für einmaliges Anmelden (SSO) über das Zugriffspanel.

Wenn Sie im Zugriffspanel die Kachel **BambooHR** wählen, sollten Sie automatisch bei Ihrer BambooHR-Anwendung angemeldet werden.

Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_203.png

