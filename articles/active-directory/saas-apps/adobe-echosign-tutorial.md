---
title: 'Tutorial: Azure Active Directory-Integration in Adobe Sign | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Adobe Sign konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: jeedes
ms.openlocfilehash: 9a1a1f1a1866e5221775d583a9bafe86eef17131
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36221222"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a>Tutorial: Azure Active Directory-Integration in Adobe Sign

In diesem Tutorial erfahren Sie, wie Sie Azure Active Directory (Azure AD) in Adobe Sign integrieren.

Die Integration von Azure AD in Adobe Sign bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Adobe Sign hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Adobe Sign anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in Adobe Sign konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Adobe Sign-Abonnement, für das einmaliges Anmelden aktiviert ist.

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Adobe Sign aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="add-adobe-sign-from-the-gallery"></a>Hinzufügen von Adobe Sign aus dem Katalog
Zum Konfigurieren der Integration von Azure AD in Adobe Sign müssen Sie Adobe Sign aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Klicken Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) auf das Symbol **Azure Active Directory**. 

    ![Screenshot vom Azure Active Directory-Symbol][1]

2. Navigieren Sie zu **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Screenshot von Azure Active Directory-Menüs mit Hervorhebung von „Unternehmensanwendungen“ und „Alle Anwendungen“][2]
    
3. Wählen Sie oben im Dialogfeld **Neue Anwendung** aus, um eine neue Anwendung hinzuzufügen.

    ![Screenshot von Option „Neue Anwendung“ oben im Dialogfeld][3]

4. Geben Sie im Suchfeld als Suchbegriff **Adobe Sign** ein.

    ![Screenshot vom Suchfeld](./media/adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. Wählen Sie im Ergebnisbereich, **Adobe Sign** und dann **Hinzufügen** aus.

    ![Screenshot vom Ergebnisbereich](./media/adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Adobe Sign anhand einer Testbenutzerin namens Britta Simon.

Azure AD muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Adobe Sign erkennen, damit das einmalige Anmelden funktioniert.

Weisen Sie in Adobe Sign den **Benutzername**-Wert in Azure AD dem Feld **Username** (Benutzername) zu, um eine Linkbeziehung herzustellen.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Adobe Sign zu konfigurieren und zu testen:

1. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on), um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. [Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user), um das einmalige Anmelden mit Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. [Erstellen eines Adobe Sign-Testbenutzers](#creating-an-adobe-sign-test-user), um eine Entsprechung für Britta Simon in Adobe Sign zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. [Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user), um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. [Testen der einmaligen Anmeldung](#testing-single-sign-on), um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Adobe Sign-Anwendung.

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Adobe Sign** auf **Einmaliges Anmelden**.

    ![Screenshot von Adobe Sign-Anwendungsintegrationsseite mit Hervorhebung von „Einmaliges Anmelden“][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Screenshot vom Dialogfeld „Einmaliges Anmelden“ mit Hervorhebung des Felds „Modus“](./media/adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Adobe Sign** die folgenden Schritte aus:

    ![Screenshot vom Abschnitt „Domäne und URLs für Adobe Sign“](./media/adobe-echosign-tutorial/tutorial_adobesign_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<companyname>.echosign.com/`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://<companyname>.echosign.com`.

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam von Adobe Sign](https://helpx.adobe.com/in/contact/support.html), um diese Werte zu erhalten.

4. Wählen Sie im Bereich **SAML-Signaturzertifikat** **Zertifikat (Base64)** aus, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Screenshot vom Abschnitt „SAML-Signaturzertifikat“](./media/adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. Wählen Sie **Speichern**aus.

    ![Screenshot von Schaltfläche „Speichern“](./media/adobe-echosign-tutorial/tutorial_general_400.png)

6. Wählen Sie im Abschnitt **Adobe Sign-Konfiguration** **Adobe Sign konfigurieren** aus, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL**, die **SAML-Entitäts-ID** und die **URL für den SAML-SSO-Dienst** aus dem Bereich **Kurzübersicht**.

    ![Screenshot vom Abschnitt „Adobe Sign-Konfiguration“ mit Hervorhebung von „Adobe Sign konfigurieren“](./media/adobe-echosign-tutorial/tutorial_adobesign_configure.png)

7. Wenden Sie sich vor der Konfiguration an das [Adobe Sign-Supportteam](https://helpx.adobe.com/in/contact/support.html), um Ihre Domäne in Adobe Sign auf die Whitelist setzen zu lassen. So fügen Sie die Domäne hinzu:

    a. Das [Adobe Sign-Supportteam](https://helpx.adobe.com/in/contact/support.html) sendet Ihnen ein zufällig generiertes Token. Für Ihre Domäne sieht das Token wie folgt aus: **adobe-sign-verification= xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx**

    b. Veröffentlichen Sie das Überprüfungstoken in einem DNS-Texteintrag, und benachrichtigen Sie das [Adobe Sign-Supportteam](https://helpx.adobe.com/in/contact/support.html).
    
    > [!NOTE]
    > Die Bearbeitung kann ein paar Tage oder länger dauern. Beachten Sie, dass Verzögerungen bei der DNS-Verteilung bedeuten, dass ein in DNS veröffentlichter Wert möglicherweise mindestens eine Stunde nicht angezeigt wird. Ihr IT-Administrator weiß, wie dieses Token in einem DNS-Texteintrag veröffentlicht wird.
    
    c. Wenn Sie das [Adobe Sign-Supportteam](https://helpx.adobe.com/in/contact/support.html) nach der Tokenveröffentlichung über das Supportticket benachrichtigen, überprüft das Team die Domäne und fügt sie Ihrem Konto hinzu.
    
    d. So veröffentlichen Sie generell ein Token in einem DNS-Eintrag

    * Melden Sie sich in Ihrem Domänenkonto an.
    * Suchen Sie die Seite zum Aktualisieren des DNS-Eintrags. Diese Seite heißt möglicherweise „DNS-Verwaltung“, „Namensserververwaltung“, „Erweiterte Einstellungen“ usw.
    * Suchen Sie die TXT-Einträge für Ihre Domäne.
    * Fügen Sie einen TXT-Eintrag mit dem vollständigen Tokenwert hinzu, den Adobe bereitgestellt hat.
    * Speichern Sie die Änderungen.

8. Melden Sie sich in einem anderen Webbrowserfenster auf der Adobe Sign-Unternehmenswebsite als Administrator an.

9. Rufen Sie im SAML-Menü **Account Settings** (Kontoeinstellungen)  >  **SAML Settings** (SAML-Einstellungen) auf.
   
    ![Screenshot von Adobe Sign-SAML-Einstellungsseite] (./media/adobe-echosign-tutorial/ic789520.png "Account") (Konto)

10. Führen Sie im Abschnitt für die **SAML-Einstellungen** die folgenden Schritte aus:
  
    ![Screenshot von SAML-Einstellungen] (./media/adobe-echosign-tutorial/ic789521.png "SAML Settings")
   
    a. Wählen Sie als **SAML Mode** (SAML-Modus) **SAML Mandatory** (SAML erforderlich) aus.
   
    b. Aktivieren Sie **Allow Echosign Account Administrators to log in using their Echosign Credentials** (EchoSign-Kontoadministratoren dürfen sich mit ihren EchoSign-Anmeldeinformationen anmelden).
   
    c. Wählen Sie unter **User Creation** (Benutzererstellung) **Automatically add users authenticated through SAML** (SAML-authentifizierte Benutzer automatisch hinzufügen) aus.

    d. Fügen Sie den Wert der **SAML-Entitäts-ID**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Entity ID/Issuer URL** (Entitäts-ID/Aussteller-URL) ein.
    
    e. Fügen Sie den Wert der **SAML-Dienst-URL für einmaliges Anmelden**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Login URL/SSO Endpoint** (Anmelde-URL/SSO-Endpunkt) ein.
   
    f. Fügen Sie den Wert der **Abmelde-URL**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Logout URL/SLO Endpoint** (Abmelde-URL/SLO-Endpunkt) ein.

    g. Öffnen Sie die heruntergeladene **Zertifikat (Base64)**-Datei im Editor. Kopieren Sie den Inhalt in die Zwischenablage, und fügen Sie ihn in das Textfeld **IdP Certificate** (IdP-Zertifikat) ein.

    h. Klicken Sie auf **Save changes** (Änderungen speichern).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.

![Screenshot vom Testbenutzernamen im Azure-Portal][100]

1. Klicken Sie im linken Bereich des **Azure-Portals** auf das Symbol **Azure Active Directory**.

    ![Screenshot vom Azure AD-Symbol](./media/adobe-echosign-tutorial/create_aaduser_01.png) 

2. Navigieren Sie zum Anzeigen der Benutzerliste zu **Benutzer und Gruppen**, und wählen Sie dann **Alle Benutzer** aus.
    
    ![Screenshot von Azure AD-Menüs mit Hervorhebung von „Benutzer und Gruppen“ und „Alle Benutzer“](./media/adobe-echosign-tutorial/create_aaduser_02.png) 

3. Zum Öffnen des Dialogfelds **Benutzer** wählen Sie **Hinzufügen**.
 
    ![Screenshot vom Dialogfeld „Alle Benutzer“ mit Hervorhebung von „Option hinzufügen“](./media/adobe-echosign-tutorial/create_aaduser_03.png) 

4. Führen Sie im Dialogfeld **Benutzer** die folgenden Schritte aus:
 
    ![Screenshot vom Dialogfeld „Benutzer“](./media/adobe-echosign-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Textfeld **Benutzername** die E-Mail-Adresse von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich das **Kennwort**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-an-adobe-sign-test-user"></a>Erstellen eines Adobe Sign-Testbenutzer

Damit sich Azure AD-Benutzer bei Adobe Sign anmelden können, müssen sie in Adobe Sign bereitgestellt werden. Diese Aufgabe muss manuell ausgeführt werden.

>[!NOTE]
>Sie können Azure AD-Benutzerkonten auch mit anderen Tools zum Erstellen von Adobe Sign-Benutzerkonten oder mit den von Adobe Sign bereitgestellten APIs erstellen. 

1. Melden Sie sich auf der **Adobe Sign**-Unternehmenswebsite als Administrator an.

2. Wählen Sie oben im Menü **Account** (Konto) aus. Wählen Sie im linken Bereich **Users & Groups** (Benutzer & Gruppen)  >  **Create a new user** (Neuen Benutzer erstellen) aus.
   
    ![Screenshot von Adobe Sign-Unternehmenswebsite mit Hervorhebung von „Account“ (Konto), „Users & Groups“ (Benutzer & Gruppen) und „Create a new user“ (Neuen Benutzer erstellen) – ](./media/adobe-echosign-tutorial/ic789524.png "Account") (Konto)
   
3. Führen Sie im Abschnitt **Create New User** die folgenden Schritte aus:
   
    ![Screenshot vom Abschnitt „Create New User“ (Neuen Benutzer erstellen) – ](./media/adobe-echosign-tutorial/ic789525.png "Create User") (Benutzer erstellen)
   
    a. Geben Sie in die Textfelder **Email Address** (E-Mail-Adresse), **First Name** (Vorname) und **Last Name** (Nachname) die entsprechenden Informationen eines gültigen Azure AD-Kontos ein, das Sie bereitstellen möchten.
   
    b. Klicken Sie auf **Create User** (Benutzer erstellen).

>[!NOTE]
>Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail mit einem Bestätigungslink für das Konto, bevor es aktiv wird. 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt gewähren Sie Britta Simon Zugriff auf Adobe Sign, damit sie das einmalige Anmelden von Azure verwenden kann.

![Screenshot vom Azure-Portal mit einmaligem Anmelden][200] 

1. Öffnen Sie im Azure-Portal die Anwendungsansicht. Gehen Sie zur Verzeichnisansicht, rufen Sie **Unternehmensanwendungen** auf, und wählen Sie **Alle Anwendungen** aus.

    ![Screenshot von Anwendungsansicht im Azure-Portal mit Hervorhebung von „Unternehmensanwendungen“ und „Alle Anwendungen“][201] 

2. Wählen Sie in der Anwendungsliste **Adobe Sign** aus.

    ![Screenshot von Anwendungsliste mit Hervorhebung von Adobe Sign](./media/adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Screenshot vom Menü mit Hervorhebung von „Benutzer und Gruppen“][202] 

4. Wählen Sie **Hinzufügen**. Wählen Sie im Bereich **Zuweisung hinzufügen** **Benutzer und Gruppen** aus.

    ![Screenshot von Seite „Benutzer und Gruppen“ und Abschnitt „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf **Auswählen**.

7. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Option **Zuweisen** aus.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

Wenn Sie im Zugriffsbereich die Kachel „Adobe Sign“ auswählen, werden Sie automatisch bei Ihrer Adobe Sign-Anwendung angemeldet. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/adobe-echosign-tutorial/tutorial_general_01.png
[2]: ./media/adobe-echosign-tutorial/tutorial_general_02.png
[3]: ./media/adobe-echosign-tutorial/tutorial_general_03.png
[4]: ./media/adobe-echosign-tutorial/tutorial_general_04.png

[100]: ./media/adobe-echosign-tutorial/tutorial_general_100.png

[200]: ./media/adobe-echosign-tutorial/tutorial_general_200.png
[201]: ./media/adobe-echosign-tutorial/tutorial_general_201.png
[202]: ./media/adobe-echosign-tutorial/tutorial_general_202.png
[203]: ./media/adobe-echosign-tutorial/tutorial_general_203.png
