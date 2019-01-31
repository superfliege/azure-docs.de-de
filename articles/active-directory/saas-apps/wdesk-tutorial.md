---
title: 'Tutorial: Azure Active Directory-Integration mit Wdesk | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Wdesk konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 06900a91-a326-4663-8ba6-69ae741a536e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 0894b1ccf976cfa1a932b76f6ed06e1bd45ee1b7
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180907"
---
# <a name="tutorial-azure-active-directory-integration-with-wdesk"></a>Tutorial: Azure Active Directory-Integration mit Wdesk

In diesem Tutorial erfahren Sie, wie Sie Wdesk in Azure Active Directory (Azure AD) integrieren.

Die Integration von Wdesk in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Wdesk hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Wdesk anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zum Integrieren von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Wdesk konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Wdesk-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Wdesk aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-wdesk-from-the-gallery"></a>Hinzufügen von Wdesk aus dem Katalog
Zum Konfigurieren der Integration von Wdesk in Azure AD müssen Sie Wdesk aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Wdesk aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

1. Geben Sie im Suchfeld als Suchbegriff **Wdesk** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/wdesk-tutorial/tutorial_wdesk_search.png)

1. Wählen Sie im Ergebnisbereich **Wdesk** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/wdesk-tutorial/tutorial_wdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Wdesk mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Wdesk als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Wdesk muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in Wdesk zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Wdesk müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
1. **[Erstellen eines Wdesk-Testbenutzers](#creating-a-wdesk-test-user)**, um eine Entsprechung von Britta Simon in Wdesk zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Wdesk-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Wdesk die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Wdesk** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Configure single sign-on](./media/wdesk-tutorial/tutorial_wdesk_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für Wdesk** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![Configure single sign-on](./media/wdesk-tutorial/tutorial_wdesk_url.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`

1. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**. Wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten, führen Sie die folgenden Schritte durch:

    ![Configure single sign-on](./media/wdesk-tutorial/tutorial_wdesk_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`.
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner, die Antwort-URL und die Anmelde-URL. Sie erhalten diese Werte im WDesk-Portal bei der Konfiguration von SSO. 
  
1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Configure single sign-on](./media/wdesk-tutorial/tutorial_wdesk_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/wdesk-tutorial/tutorial_general_400.png)
    
1. Melden Sie sich in einem anderen Webbrowserfenster als Sicherheitsadministrator bei Wdesk an.

1. Klicken Sie links unten auf **Admin**, und wählen Sie **Account Admin** (Kontoadministrator) aus:
 
     ![Configure single sign-on](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

1. Navigieren Sie in Wdesk Admin zu **Security** (Sicherheit) und dann zu **SAML** > **SAML Settings** (SAML-Einstellungen):

    ![Configure single sign-on](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig2.png)

1. Aktivieren Sie unter **General Settings** (Allgemeine Einstellungen) die Option **Enable SAML Single Sign On** (SAML Single Sign On aktivieren):

    ![Configure single sign-on](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig3.png)

1. Führen Sie unter **Service Provider Details** (Dienstanbieterdetails) die folgenden Schritte aus:

    ![Configure single sign-on](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig4.png)

      a. Kopieren Sie die **Anmelde-URL**, und fügen Sie sie im Azure-Portal in das Textfeld **Anmelde-URL** ein.
   
      b. Kopieren Sie die **Metadaten-URL**, und fügen Sie sie im Azure-Portal in das Textfeld **Bezeichner** ein.
       
      c. Kopieren Sie die **Consumer-URL**, und fügen Sie sie im Azure-Portal in das Textfeld **Antwort-URL** ein.
   
      d. Klicken Sie im Azure-Portal auf **Speichern**, um die Änderungen zu speichern.      

1. Klicken Sie auf **Configure IdP Settings** (IdP-Einstellungen konfigurieren), um das Dialogfeld **Edit IdP Settings** (IdP-Einstellungen bearbeiten) zu öffnen. Klicken Sie auf **Choose File** (Datei auswählen), um die Datei **Metadata.xml**, die Sie im Azure-Portal gespeichert haben, zu suchen und hochzuladen.
    
    ![Configure single sign-on](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig5.png)
  
1. Klicken Sie auf **Änderungen speichern**.

    ![Configure single sign-on](./media/wdesk-tutorial/tutorial_wdesk_ssoconfigsavebutton.png)

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Dokumentation zu eingebettetem Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/wdesk-tutorial/create_aaduser_01.png) 

1. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/wdesk-tutorial/create_aaduser_02.png) 

1. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/wdesk-tutorial/create_aaduser_03.png) 

1. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/wdesk-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.
 
### <a name="creating-a-wdesk-test-user"></a>Erstellen eines Wdesk-Testbenutzers

Damit sich Azure AD-Benutzer bei Wdesk anmelden können, müssen sie in Wdesk bereitgestellt werden. Im Fall von Wdesk ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei Wdesk als Sicherheitsadministrator an.
1. Navigieren Sie zu **Admin** > **Account Admin** (Kontoadministrator).

     ![Configure single sign-on](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

1. Klicken Sie unter **People** (Personen) auf **Members** (Mitglieder).

1. Klicken Sie nun auf **Add Member** (Mitglied hinzufügen), um das Dialogfeld **Add Member** (Mitglied hinzufügen) zu öffnen. 
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/wdesk-tutorial/createuser1.png)  

1. Geben Sie im Textfeld **User** (Benutzer) den Benutzernamen des Benutzers (z.B. **brittasimon@contoso.com**) ein, und klicken Sie dann auf **Continue** (Weiter).

    ![Erstellen eines Azure AD-Testbenutzers](./media/wdesk-tutorial/createuser3.png)

1.  Geben Sie die Details wie folgt ein:
  
    ![Erstellen eines Azure AD-Testbenutzers](./media/wdesk-tutorial/createuser4.png)
 
    a. Geben Sie im Textfeld **E-mail** (E-Mail-Adresse) die E-Mail-Adresse des Benutzers ein, z.B. **brittasimon@contoso.com**.

    b. Geben Sie im Textfeld **First Name** (Vorname) den Vornamen des Benutzers ein, z.B. **Britta**.

    c. Geben Sie im Textfeld **Last Name** (Nachname) den Nachnamen des Benutzers ein, z.B. **Simon**.

1. Klicken Sie auf die Schaltfläche **Save Member** (Mitglied speichern).  

    ![Erstellen eines Azure AD-Testbenutzers](./media/wdesk-tutorial/createuser5.png)

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Wdesk gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Wdesk zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **Wdesk** aus.

    ![Configure single sign-on](./media/wdesk-tutorial/tutorial_wdesk_app.png) 

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Wdesk“ klicken, sollten Sie automatisch bei Ihrer Wdesk-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/wdesk-tutorial/tutorial_general_01.png
[2]: ./media/wdesk-tutorial/tutorial_general_02.png
[3]: ./media/wdesk-tutorial/tutorial_general_03.png
[4]: ./media/wdesk-tutorial/tutorial_general_04.png

[100]: ./media/wdesk-tutorial/tutorial_general_100.png

[200]: ./media/wdesk-tutorial/tutorial_general_200.png
[201]: ./media/wdesk-tutorial/tutorial_general_201.png
[202]: ./media/wdesk-tutorial/tutorial_general_202.png
[203]: ./media/wdesk-tutorial/tutorial_general_203.png

