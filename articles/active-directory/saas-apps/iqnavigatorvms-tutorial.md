---
title: 'Tutorial: Azure Active Directory-Integration von IQNavigator VMS | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und IQNavigator VMS konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a8a09b25-dfa5-4c31-aea2-53bf1853b365
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: jeedes
ms.openlocfilehash: 062c83c7e1d621a80a5e81cdaf6b070eb446453c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180533"
---
# <a name="tutorial-azure-active-directory-integration-with-iqnavigator-vms"></a>Tutorial: Azure Active Directory-Integration von IQNavigator VMS

In diesem Tutorial erfahren Sie, wie Sie IQNavigator VMS in Azure Active Directory (Azure AD) integrieren.

Die Integration von IQNavigator VMS in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf IQNavigator VMS hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei IQNavigator VMS anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in IQNavigator VMS konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein IQNavigator VMS-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie nicht über eine Azure AD-Testumgebung verfügen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von IQNavigator VMS aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-iqnavigator-vms-from-the-gallery"></a>Hinzufügen von IQNavigator VMS aus dem Katalog
Zum Konfigurieren der Integration von IQNavigator VMS in Azure AD müssen Sie IQNavigator VMS aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um IQNavigator VMS aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

1. Geben Sie in das Suchfeld **IQNavigator VMS** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_search.png)

1. Wählen Sie im Ergebnisbereich **IQNavigator VMS** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei IQNavigator VMS mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in IQNavigator VMS als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in IQNavigator VMS muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in IQNavigator VMS den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei IQNavigator VMS müssen die folgenden Schritte ausgeführt werden:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
1. **[Erstellen eines IQNavigator VMS-Testbenutzers](#creating-a-iqnavigator-vms-test-user)**, um ein Pendant von Britta Simon in IQNavigator VMS zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer IQNavigator VMS-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei IQNavigator VMS die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **IQNavigator VMS** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Configure single sign-on](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für IQNavigator VMS** die folgenden Schritte aus:

    ![Configure single sign-on](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url.png)

    a. Geben Sie im Textfeld **Bezeichner** die folgende URL ein: `iqn.com`.

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<subdomain>.iqnavigator.com/security/login?client_name=https://sts.window.net/<instance name>`

1. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie den folgenden Schritt aus:

    ![Configure single sign-on](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url1.png)

    Geben Sie im Textfeld **Relayzustand** eine URL nach dem folgenden Muster ein: `https://<subdomain>.iqnavigator.com`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Antwort-URL und den tatsächlichen Relayzustand. Wenden Sie sich an das [Supportteam für den IQNavigator VMS-Client](https://www.beeline.com/iqn-product-support/), um diese Werte zu erhalten.

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf die Schaltfläche „Kopieren“, um die **Verbundmetadaten-URL der App** zu kopieren, und fügen Sie die URL in Editor ein.
    
    ![Configure single sign-on](./media/iqnavigatorvms-tutorial/tutorial_metadataurl.png)

1. Die IQNavigator-Anwendung erwartet den Wert der eindeutigen Benutzer-ID im Namensbezeichneranspruch. Der Kunde kann den richtigen Wert für den Namensbezeichneranspruch zuordnen. In diesem Fall haben wir zu Demozwecken „user.UserPrincipalName“ zugeordnet. Sie müssen jedoch basierend auf Ihren Organisationseinstellungen den richtigen Wert zuordnen.

    ![Configure single sign-on](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_attribute.png)

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/iqnavigatorvms-tutorial/tutorial_general_400.png)

1. Klicken Sie im Abschnitt **IQNavigator VMS-Konfiguration** auf **IQNavigator VMS konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Configure single sign-on](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_configure.png)

1. Zum Konfigurieren des einmaligen Anmeldens auf der Seite von **IQNavigator VMS** müssen Sie die **Verbundmetadaten-URL der App**, **Abmelde-URL, SAML-Entitäts-ID und SAML-Dienst-URL für einmaliges Anmelden** an das [IQNavigator VMS-Supportteam](https://www.beeline.com/iqn-product-support/) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/iqnavigatorvms-tutorial/create_aaduser_01.png) 

1. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/iqnavigatorvms-tutorial/create_aaduser_02.png)

1. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/iqnavigatorvms-tutorial/create_aaduser_03.png)

1. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/iqnavigatorvms-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.

### <a name="creating-a-iqnavigator-vms-test-user"></a>Erstellen eines IQNavigator VMS-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in IQNavigator VMS. Lassen Sie sich beim Hinzufügen der Benutzer im IQNavigator VMS-Konto ggf. vom [IQNavigator VMS-Supportteam](https://www.beeline.com/iqn-product-support/) unterstützen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf IQNavigator VMS gewähren.

![Benutzer zuweisen][200]

**Um Britta Simon IQNavigator VMS zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201]

1. Wählen Sie in der Anwendungsliste **IQNavigator VMS** aus.

    ![Configure single sign-on](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_app.png)

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „IQNavigator VMS“ klicken, sollten Sie automatisch bei Ihrer IQNavigator VMS-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/iqnavigatorvms-tutorial/tutorial_general_01.png
[2]: ./media/iqnavigatorvms-tutorial/tutorial_general_02.png
[3]: ./media/iqnavigatorvms-tutorial/tutorial_general_03.png
[4]: ./media/iqnavigatorvms-tutorial/tutorial_general_04.png

[100]: ./media/iqnavigatorvms-tutorial/tutorial_general_100.png

[200]: ./media/iqnavigatorvms-tutorial/tutorial_general_200.png
[201]: ./media/iqnavigatorvms-tutorial/tutorial_general_201.png
[202]: ./media/iqnavigatorvms-tutorial/tutorial_general_202.png
[203]: ./media/iqnavigatorvms-tutorial/tutorial_general_203.png

