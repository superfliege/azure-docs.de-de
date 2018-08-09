---
title: 'Lernprogramm: Azure Active Directory-Integration mit QuickHelp | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und QuickHelp konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 655c9ad3-2076-4e2c-8e47-9ed3bf04be56
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: jeedes
ms.openlocfilehash: c99be60301085dddfd5c658ee1eed81b88238e54
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421588"
---
# <a name="tutorial-azure-active-directory-integration-with-quickhelp"></a>Lernprogramm: Azure Active Directory-Integration mit QuickHelp

In diesem Tutorial erfahren Sie, wie Sie QuickHelp in Azure Active Directory (Azure AD) integrieren.

Die Integration von QuickHelp in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf QuickHelp Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei QuickHelp anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit QuickHelp konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein QuickHelp-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von QuickHelp aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-quickhelp-from-the-gallery"></a>Hinzufügen von QuickHelp aus dem Katalog
Zum Konfigurieren der Integration von QuickHelp in Azure AD müssen Sie QuickHelp aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um QuickHelp aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

1. Geben Sie im Suchfeld das Wort **QuickHelp**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/quickhelp-tutorial/tutorial_quickhelp_search.png)

1. Wählen Sie im Ergebnisbereich die Option **QuickHelp** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/quickhelp-tutorial/tutorial_quickhelp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei QuickHelp mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in QuickHelp als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in QuickHelp muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in QuickHelp den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens mit Azure AD bei QuickHelp müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
1. **[Erstellen eines QuickHelp-Testbenutzers](#creating-a-quickhelp-test-user)**, um ein Pendant von Britta Simon in QuickHelp zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer QuickHelp-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in QuickHelp die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **QuickHelp** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Configure single sign-on](./media/quickhelp-tutorial/tutorial_quickhelp_samlbase.png)

1. Führen Sie auf der Seite **Domäne und URLs für QuickHelp** die folgenden Schritte aus:

    ![Configure single sign-on](./media/quickhelp-tutorial/tutorial_quickhelp_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://quickhelp.com/<ROUTEURL>`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL ein: `https://auth.quickhelp.com`

    > [!NOTE] 
    > Der Wert der Anmelde-URL entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an den Schnellhilfeadministrator Ihres Unternehmens oder Ihren BrainStorm Client Success Manager, um den Wert abzurufen.
 
1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Configure single sign-on](./media/quickhelp-tutorial/tutorial_quickhelp_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/quickhelp-tutorial/tutorial_general_400.png) 

1. Melden Sie sich bei Ihrer QuickHelp-Unternehmenswebsite als Administrator an.

1. Klicken Sie oben im Menü auf **Administrator**.
   
    ![Configure single sign-on][21]

1. Klicken Sie im Menü **QuickHelp Admin** auf **Einstellungen**.
   
    ![Configure single sign-on][22]

1. Klicken Sie auf **Authentifizierungseinstellungen**.

1. Führen Sie auf der Seite **Authentifizierungseinstellungen** die folgenden Schritte aus:
   
    ![Configure single sign-on][23]
   
    a. Wählen Sie für **SSO-Typ** die Option **WSFederation** aus.
   
    b. Klicken Sie zum Hochladen der heruntergeladenen Azure-Metadatendatei auf **Durchsuchen**, navigieren Sie zur Datei, und klicken Sie dann auf **Metadaten hochladen**.
   
    c. Geben Sie in das Textfeld **E-Mail** die Adresse `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` ein.
   
    d. Geben Sie in das Textfeld **Vorname** den Namen `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` ein.
   
    e. Geben Sie in das Textfeld **Nachname** den Namen `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` ein.
   
    f. Klicken Sie in der **Aktionsleiste** auf **Speichern**.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/quickhelp-tutorial/create_aaduser_01.png) 

1. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/quickhelp-tutorial/create_aaduser_02.png) 

1. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/quickhelp-tutorial/create_aaduser_03.png) 

1. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/quickhelp-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.
 
### <a name="creating-a-quickhelp-test-user"></a>Erstellen einen QuickHelp-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in QuickHelp.
Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in QuickHelp als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in QuickHelp muss eine Linkbeziehung eingerichtet werden.

QuickHelp unterstützt die Just-in-Time-Bereitstellung. Auf diese Weise kann ein Benutzerkonto bei Bedarf automatisch in QuickHelp erstellt und mit dem Azure AD-Konto verknüpft werden.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf QuickHelp gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon QuickHelp zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **QuickHelp**aus.

    ![Configure single sign-on](./media/quickhelp-tutorial/tutorial_quickhelp_app.png) 

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt soll Ihre Azure AD-Konfiguration für das einmalige Anmelden mithilfe des Zugriffsbereichs getestet werden.  

Wenn Sie im Zugriffsbereich auf die Kachel "QuickHelp" klicken, sollten Sie automatisch bei Ihrer QuickHelp-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/quickhelp-tutorial/tutorial_general_04.png

[100]: ./media/quickhelp-tutorial/tutorial_general_100.png

[200]: ./media/quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/quickhelp-tutorial/tutorial_general_202.png
[203]: ./media/quickhelp-tutorial/tutorial_general_203.png
[21]: ./media/quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/quickhelp-tutorial/tutorial_quickhelp_07.png
