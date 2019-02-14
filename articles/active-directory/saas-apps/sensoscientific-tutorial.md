---
title: 'Tutorial: Azure Active Directory integration with SensoScientific Wireless Temperature Monitoring System | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SensoScientific Wireless Temperature Monitoring System konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: ee9a924d-ccde-45b0-ab40-877f82f5dfa2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 33a625e82f41bee1b8e3980192076d24a7471953
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56183410"
---
# <a name="tutorial-azure-active-directory-integration-with-sensoscientific-wireless-temperature-monitoring-system"></a>Tutorial: Azure Active Directory-Integration mit SensoScientific Wireless Temperature Monitoring System

In diesem Tutorial erfahren Sie, wie Sie SensoScientific Wireless Temperature Monitoring System in Azure Active Directory (Azure AD) integrieren.

Die Integration von SensoScientific Wireless Temperature Monitoring System in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf SensoScientific Wireless Temperature Monitoring System hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei SensoScientific Wireless Temperature Monitoring System anzumelden (einmaliges Anmelden, Single Sign-On).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit SensoScientific Wireless Temperature Monitoring System konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SensoScientific Wireless Temperature Monitoring System-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von SensoScientific Wireless Temperature Monitoring System aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-sensoscientific-wireless-temperature-monitoring-system-from-the-gallery"></a>Hinzufügen von SensoScientific Wireless Temperature Monitoring System aus dem Katalog
Zum Konfigurieren der Integration von SensoScientific Wireless Temperature Monitoring System in Azure AD müssen Sie SensoScientific Wireless Temperature Monitoring System aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um SensoScientific Wireless Temperature Monitoring System aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

1. Geben Sie im Suchfeld den Suchbegriff **SensoScientific Wireless Temperature Monitoring System** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_search.png)

1. Wählen Sie im Ergebnisbereich die Option **SensoScientific Wireless Temperature Monitoring System** aus, und klicken Sie dann auf die Schaltfläche **Add**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei SensoScientific Wireless Temperature Monitoring System basierend auf einer Testbenutzerin mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in SensoScientific Wireless Temperature Monitoring System als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SensoScientific Wireless Temperature Monitoring System muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert für den **Benutzernamen** in Azure AD als Wert für den **Benutzernamen** in SensoScientific Wireless Temperature Monitoring System zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei SensoScientific Wireless Temperature Monitoring System müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
1. **[Erstellen eines SensoScientific Wireless Temperature Monitoring System-Testbenutzers](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)**, um eine Entsprechung von Britta Simon in SensoScientific Wireless Temperature Monitoring System zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer SensoScientific Wireless Temperature Monitoring System-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in SensoScientific Wireless Temperature Monitoring System die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **SensoScientific Wireless Temperature Monitoring System** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Configure single sign-on](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_samlbase.png)

1. Im Abschnitt **SensoScientific Wireless Temperature Monitoring System Domain and URLs** sind keine Schritte erforderlich, da die App bereits in Azure integriert ist.

    ![Configure single sign-on](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_url.png)

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Configure single sign-on](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/sensoscientific-tutorial/tutorial_general_400.png)

1. Klicken Sie im Abschnitt **SensoScientific Wireless Temperature Monitoring System Configuration** auf **SensoScientific Wireless Temperature Monitoring System konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID** und die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Configure single sign-on](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_configure.png) 

1. Melden Sie sich bei der SensoScientific Wireless Temperature Monitoring System-Anwendung als Administrator an.

1. Klicken Sie im oberen Navigationsmenü auf **Configuration**, und klicken Sie unter **Single Sign On** auf **Configure**, um die Seite „Single Sign On Settings“ zu öffnen.

    ![Configure single sign-on](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_admin.png) 

1. Führen Sie im Formular **Single Sign On Settings** die folgenden Schritte aus:
 
    a. Wählen Sie als **Issuer Name** die Option „Azure AD“ aus.
    
    b. Fügen Sie die **SAML-Entitäts-ID**, die Sie aus dem Azure-Portal kopiert haben, in das Textfeld „Issuer URL“ ein.
    
    c. Fügen Sie die **URL für den SAML-SSO-Dienst**, die Sie aus dem Azure-Portal kopiert haben, in das Textfeld „Single Sign-On Service URL“ ein.

    d. Fügen Sie die **Abmelde-URL**, die Sie aus dem Azure-Portal kopiert haben, in das Textfeld „Single Sign-Out Service URL“ ein.

    e. Navigieren Sie zu dem Zertifikat, das Sie aus dem Azure-Portal heruntergeladen haben, und laden Sie es hier hoch.
    
    f. Klicken Sie auf **Speichern**.
  
> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Dokumentation zu eingebettetem Azure AD](https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/sensoscientific-tutorial/create_aaduser_01.png) 

1. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/sensoscientific-tutorial/create_aaduser_02.png) 

1. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/sensoscientific-tutorial/create_aaduser_03.png) 

1. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/sensoscientific-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.
 
### <a name="creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user"></a>Erstellen eines SensoScientific Wireless Temperature Monitoring System-Testbenutzers

Damit sich Azure AD-Benutzer bei SensoScientific Wireless Temperature Monitoring System anmelden können, müssen sie in SensoScientific Wireless Temperature Monitoring System bereitgestellt werden. Arbeiten Sie mit dem  [Supportteam von SensoScientific Wireless Temperature Monitoring System](https://www.sensoscientific.com/contact-us/) zusammen, um die Benutzer zur SensoScientific Wireless Temperature Monitoring System-Plattform hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können. 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf SensoScientific Wireless Temperature Monitoring System gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu SensoScientific Wireless Temperature Monitoring System zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **SensoScientific Wireless Temperature Monitoring System** aus.

    ![Configure single sign-on](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_app.png) 

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich. Klicken Sie im Zugriffsbereich auf die Kachel „SensoScientific Wireless Temperature Monitoring System“ – Sie werden automatisch bei Ihrer SensoScientific Wireless Temperature Monitoring System-Anwendung angemeldet. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sensoscientific-tutorial/tutorial_general_01.png
[2]: ./media/sensoscientific-tutorial/tutorial_general_02.png
[3]: ./media/sensoscientific-tutorial/tutorial_general_03.png
[4]: ./media/sensoscientific-tutorial/tutorial_general_04.png

[100]: ./media/sensoscientific-tutorial/tutorial_general_100.png

[200]: ./media/sensoscientific-tutorial/tutorial_general_200.png
[201]: ./media/sensoscientific-tutorial/tutorial_general_201.png
[202]: ./media/sensoscientific-tutorial/tutorial_general_202.png
[203]: ./media/sensoscientific-tutorial/tutorial_general_203.png

