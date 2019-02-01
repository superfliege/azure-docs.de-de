---
title: 'Tutorial: Azure Active Directory-Integration mit HappyFox | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und HappyFox konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 8204ee77-f64b-4fac-b64a-25ea534feac0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jeedes
ms.openlocfilehash: 94bd0367b8626a83fd10fc51d8b606ab38f6182c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55178068"
---
# <a name="tutorial-azure-active-directory-integration-with-happyfox"></a>Tutorial: Azure Active Directory-Integration mit HappyFox

In diesem Tutorial erfahren Sie, wie Sie HappyFox in Azure Active Directory (Azure AD) integrieren.

Die Integration von HappyFox in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf HappyFox hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei HappyFox anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit HappyFox konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein HappyFox-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von HappyFox aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-happyfox-from-the-gallery"></a>Hinzufügen von HappyFox aus dem Katalog
Zum Konfigurieren der Integration von HappyFox in Azure AD müssen Sie HappyFox aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um HappyFox aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

1. Geben Sie im Suchfeld als Suchbegriff **HappyFox** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/happyfox-tutorial/tutorial_happyfox_search.png)

1. Wählen Sie im Ergebnisbereich **HappyFox** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/happyfox-tutorial/tutorial_happyfox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei HappyFox mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in HappyFox als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in HappyFox muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in HappyFox den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei HappyFox müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
1. **[Erstellen eines HappyFox-Testbenutzers](#creating-a-happyfox-test-user)**, um eine Entsprechung von Britta Simon in HappyFox zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer HappyFox-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in HappyFox die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **HappyFox** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Configure single sign-on](./media/happyfox-tutorial/tutorial_happyfox_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für HappyFox** die folgenden Schritte aus:

    ![Configure single sign-on](./media/happyfox-tutorial/tutorial_happyfox_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.happyfox.com/`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<subdomain>.happyfox.com/saml/metadata/`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam von HappyFox](https://support.happyfox.com/home), um diese Werte zu erhalten. 
 
1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.

    ![Configure single sign-on](./media/happyfox-tutorial/tutorial_happyfox_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/happyfox-tutorial/tutorial_general_400.png)

1. Klicken Sie im Abschnitt **HappyFox-Konfiguration** auf **HappyFox konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Configure single sign-on](./media/happyfox-tutorial/tutorial_happyfox_configure.png) 

1. Melden Sie sich beim HappyFox-Mitarbeiterportal an, und navigieren Sie zu **Manage** (Verwalten). Klicken Sie anschließend auf die Registerkarte **Integrations**.

    ![Configure single sign-on](./media/happyfox-tutorial/header.png) 

1. Klicken Sie auf der Registerkarte „Integrations“ unter **SAML Integration** auf **Configure** (Konfigurieren), um die Einstellungen für einmaliges Anmelden zu öffnen.

    ![Configure single sign-on](./media/happyfox-tutorial/configure.png) 

1. Fügen Sie im Abschnitt für die SAML-Konfiguration, die **SAML-Dienst-URL für einmaliges Anmelden**, die Sie im Azure-Portal kopiert haben, in das Textfeld **SSO Target URL** (SSO-Ziel-URL) ein.

    ![Configure single sign-on](./media/happyfox-tutorial/targeturl.png)

1. Öffnen Sie das Zertifikat, das Sie aus dem Azure-Portal heruntergeladen haben, in Editor, und fügen Sie den Inhalt im Abschnitt **IdP Signature** ein.
 
    ![Configure single sign-on](./media/happyfox-tutorial/cert.png)

1. Klicken Sie auf die Schaltfläche **Save settings** (Einstellungen speichern).

    ![Configure single sign-on](./media/happyfox-tutorial/savesettings.png)

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Dokumentation zu eingebettetem Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/happyfox-tutorial/create_aaduser_01.png) 

1. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/happyfox-tutorial/create_aaduser_02.png) 

1. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/happyfox-tutorial/create_aaduser_03.png) 

1. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/happyfox-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.
 
### <a name="creating-a-happyfox-test-user"></a>Erstellen eines HappyFox-Testbenutzers

Die Anwendung unterstützt die Just-in-Time-Benutzerbereitstellung (JIT). Nach der Authentifizierung werden Benutzer in der Anwendung automatisch erstellt.
        
### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf HappyFox gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon HappyFox zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **HappyFox** aus.

    ![Configure single sign-on](./media/happyfox-tutorial/tutorial_happyfox_app.png) 

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

1. Wenn Sie im Zugriffsbereich auf die Kachel „HappyFox“ klicken, wird die Anmeldeseite der HappyFox-Anwendung aufgerufen. Auf der Anmeldeseite wird die Schaltfläche **SAML** angezeigt.

    ![Plug-In](./media/happyfox-tutorial/saml.png) 

1. Klicken Sie auf die Schaltfläche **SAML**, um sich mit Ihrem Azure AD-Konto bei HappyFox anzumelden.

Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/happyfox-tutorial/tutorial_general_01.png
[2]: ./media/happyfox-tutorial/tutorial_general_02.png
[3]: ./media/happyfox-tutorial/tutorial_general_03.png
[4]: ./media/happyfox-tutorial/tutorial_general_04.png

[100]: ./media/happyfox-tutorial/tutorial_general_100.png

[200]: ./media/happyfox-tutorial/tutorial_general_200.png
[201]: ./media/happyfox-tutorial/tutorial_general_201.png
[202]: ./media/happyfox-tutorial/tutorial_general_202.png
[203]: ./media/happyfox-tutorial/tutorial_general_203.png

