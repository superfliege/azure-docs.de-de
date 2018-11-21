---
title: 'Tutorial: Azure Active Directory-Integration mit SuccessFactors | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SuccessFactors konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: jeedes
ms.openlocfilehash: 89224b32efaecdf7a2797b034b1beac7ad191ee5
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51685224"
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Tutorial: Azure Active Directory-Integration in SuccessFactors

In diesem Tutorial erfahren Sie, wie Sie SuccessFactors in Azure Active Directory (Azure AD) integrieren.

Die Integration von SuccessFactors in Azure AD bietet folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf SuccessFactors haben soll.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei SuccessFactors anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit SuccessFactors konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SuccessFactors-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von SuccessFactors über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-successfactors-from-the-gallery"></a>Hinzufügen von SuccessFactors über den Katalog

Zum Konfigurieren der Integration von SuccessFactors in Azure AD müssen Sie SuccessFactors über den Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um SuccessFactors über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **SuccessFactors** ein, wählen Sie im Ergebnisbereich **SuccessFactors** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![SuccessFactors in der Ergebnisliste](./media/successfactors-tutorial/tutorial_successfactors_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei SuccessFactors basierend auf einer Testbenutzerin mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in SuccessFactors als Gegenpart für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SuccessFactors muss eine Linkbeziehung eingerichtet werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit SuccessFactors zu konfigurieren und zu testen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. **[Erstellen eines SuccessFactors-Testbenutzers](#creating-a-successfactors-test-user)**, um in SuccessFactors einen Gegenpart von Britta Simon zu erhalten, der mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer SuccessFactors-Anwendung.

**Führen Sie folgende Schritte aus, um das einmalige Anmelden von Azure AD mit SuccessFactors zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **SuccessFactors** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![Configure single sign-on](common/tutorial_general_301.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Configure single sign-on](common/editconfigure.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und den URLs für SuccessFactors](./media/successfactors-tutorial/tutorial_successfactors_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein:
    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.eu`|

    b. Geben Sie im Textfeld **Identifier** (Bezeichner) eine URL nach folgendem Muster ein:
    | |
    |--|
    | `https://www.successfactors.com/<companyname>`|
    | `https://www.successfactors.com`|
    | `https://<companyname>.successfactors.eu`|
    | `https://www.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://hcm4preview.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.eu`|
    | `https://www.successfactors.cn`|
    | `https://www.successfactors.cn/<companyname>`|

    c. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein:
    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.successfactors.com`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.successfactors.eu`|
    | `https://<companyname>.sapsf.eu`|
    | `https://<companyname>.sapsf.eu/<companyname>`|
    | `https://<companyname>.sapsf.cn`|
    | `https://<companyname>.sapsf.cn/<companyname>`|
         
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL, den tatsächlichen Bezeichner und die tatsächliche Antwort-URL. Die entsprechenden Werte erfahren Sie vom [SuccessFactors-Supportteam](https://www.successfactors.com/support.html). 

5. Klicken Sie auf der Seite **SAML-Signaturzertifikat** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das **Zertifikat (Base64)** herunterzuladen. Speichern Sie die Zertifikatsdatei anschließend auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/successfactors-tutorial/tutorial_successfactors_certificate.png) 

6. Kopieren Sie im Abschnitt **SuccessFactors einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

    ![SuccessFactors-Konfiguration](common/configuresection.png)

7. Melden Sie sich in einem anderen Webbrowserfenster als Administrator beim **SuccessFactors-Verwaltungsportal** an.
    
8. Rufen Sie **Anwendungssicherheit** auf, und navigieren **Sie zum Feature für einmaliges Anmelden**. 

9. Geben Sie unter **Reset Token** (Token zurücksetzen) einen beliebigen Wert an, und klicken Sie auf **Save Token** (Token speichern), um SAML-SSO zu aktivieren.
   
    ![App-seitiges Konfigurieren des einmaligen Anmeldens][11]

    > [!NOTE] 
    > Dieser Wert wird als Ein-/Ausschalter verwendet. Sobald ein beliebiger Wert gespeichert wird, wird SAML-SSO aktiviert. Wird kein Wert gespeichert, ist SAML-SSO deaktiviert.

10. Sehen Sie sich den folgenden Screenshot an, und führen Sie die folgenden Aktionen aus:
   
    ![App-seitiges Konfigurieren des einmaligen Anmeldens][12]
   
    a. Wählen Sie das Optionsfeld **SAML v2 SSO** aus.
   
    b. Legen Sie den **Namen der SAML-Assertionspartei** fest (beispielsweise auf SAML-Aussteller und Unternehmensname).
   
    c. Fügen Sie im Textfeld **Issuer URL** (Aussteller-URL) den **Azure AD-Bezeichner** ein, den Sie aus dem Azure-Portal kopiert haben.
   
    d. Wählen Sie unter **Require Mandatory Signature** (Pflichtsignatur erforderlich) **Assertion** aus.
   
    e. Wählen Sie für **Enable SAML Flag** (SAML-Flag aktivieren) die Option **Aktiviert** aus.
   
    f. Wählen Sie für **Login Request Signature(SF Generated/SP/RP)** (Anmeldeanforderungssignatur (SF-generiert/SP/RP)) die Option **Nein** aus.
   
    g. Wählen Sie für **SAML Profile** (SAML-Profil) die Option **Browser/Post Profile** (Browser/Post-Profil) aus.
   
    h. Wählen Sie für **Enforce Certificate Valid Period** (Gültigkeitszeitraum des Zertifikats erzwingen) die Option **Nein** aus.
   
    i. Kopieren Sie den Inhalt der heruntergeladenen Zertifikatdatei aus dem Azure-Portal, und fügen Sie ihn in das Textfeld **SAML Verifying Certificate** (SAML-Verifizierungszertifikat) ein.

    > [!NOTE] 
    > Der Inhalt des Zertifikats muss über Tags für Beginn und Ende des Zertifikats verfügen.

11. Navigieren Sie zu „SAML V2“, und führen Sie die folgenden Schritte aus:
   
    ![App-seitiges Konfigurieren des einmaligen Anmeldens][13]
   
    a. Wählen Sie für **Support SP-initiated Global Logout** (SP-initiiertes globales Abmelden unterstützen) die Option **Ja** aus.
   
    b. Fügen Sie in das Textfeld **Global Logout Service URL (LogoutRequest destination)** (URL des globalen Abmeldediensts (LogoutRequest-Ziel)) den Wert für die **Sign-Out URL** (Abmelde-URL) ein, den Sie aus dem Azure-Portal kopiert haben.
   
    c. Wählen Sie für **Require sp must encrypt all NameID elements** (SP muss alle NameID-Elemente verschlüsseln) die Option **Nein** aus.
   
    d. Wählen Sie für **NameID Format** (NameID-Format) die Option **unspecified** (keine Angabe) aus.
   
    e. Wählen Sie für **Enable sp initiated login (AuthnRequest)** (SP-initiierte Anmeldung aktivieren (AuthnRequest)) die Option **Ja** aus.
   
    f. Fügen Sie den Wert der **URL für den SAML-SSO-Dienst**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Send request as Company-Wide issuer** (Anforderung als unternehmensweiter Aussteller senden) ein.

12. Führen Sie die folgenden Schritte aus, wenn bei den Anmeldebenutzernamen die Groß-/Kleinschreibung nicht beachtet werden soll.
   
    ![Configure single sign-on][29]
    
    a. Rufen Sie **Unternehmenseinstellungen** (im unteren Bereich der Seite) auf.
   
    b. Aktivieren Sie das Kontrollkästchen **Enable Non-Case-Sensitive Username**(Groß-/Kleinschreibung bei Benutzernamen nicht beachten).
   
    c. Klicken Sie auf **Speichern**.
   
    > [!NOTE] 
    > Wenn Sie diese Funktion aktivieren, überprüft das System, ob ein doppelter SAML-Anmeldename erstellt wird. Dies wäre etwa der Fall, wenn der Kunde die Benutzernamen „Benutzer1“ und „benutzer1“ verwendet. Ohne Beachtung der Groß-/Kleinschreibung werden diese zu Duplikaten. In diesem Fall gibt das System eine Fehlermeldung aus, und das Feature wird nicht aktiviert. Der Kunde muss für einen der Benutzernamen eine abweichende Schreibweise verwenden.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Azure AD-Benutzer erstellen][100]

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Erstellen eines Azure AD-Testbenutzers](common/create_aaduser_01.png) 

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Erstellen eines Azure AD-Testbenutzers](common/create_aaduser_02.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** den Namen **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

### <a name="creating-a-successfactors-test-user"></a>Erstellen eines SuccessFactors-Testbenutzers

Damit sich Azure AD-Benutzer bei SuccessFactors anmelden können, müssen sie in SuccessFactors bereitgestellt werden.  
Im Fall von SuccessFactors ist die Bereitstellung eine manuelle Aufgabe.

Wenn Sie in SuccessFactors erstellte Benutzer abrufen möchten, wenden Sie sich an das [SuccessFactors-Supportteam](https://www.successfactors.com/support.html).

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf SuccessFactors erteilen.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste die Option **SuccessFactors**aus.

    ![Configure single sign-on](./media/successfactors-tutorial/tutorial_successfactors_app.png)

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „SuccessFactors“ klicken, sollten Sie automatisch bei Ihrer SuccessFactors-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
[11]: ./media/successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/successfactors-tutorial/tutorial_successfactors_09.png
[29]: ./media/successfactors-tutorial/tutorial_successfactors_10.png
