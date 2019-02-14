---
title: 'Tutorial: Azure Active Directory-Integration mit Central Desktop | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie die einmalige Anmeldung zwischen Azure Active Directory und Central Desktop konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d389a9675f7d4de56a03026c0d392b9988f6b0da
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56201675"
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Tutorial: Azure Active Directory-Integration mit Central Desktop

In diesem Tutorial erfahren Sie, wie Sie Central Desktop in Azure Active Directory (Azure AD) integrieren.

Die Integration von Central Desktop in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Central Desktop hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Central Desktop anzumelden.
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration in Central Desktop benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Central Desktop-Abonnement, für das die einmalige Anmeldung aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie noch keine Azure AD-Testumgebung haben, können Sie eine [einmonatige kostenlose Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Central Desktop aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="add-central-desktop-from-the-gallery"></a>Hinzufügen von Central Desktop aus dem Katalog
Zum Konfigurieren der Integration von Central Desktop in Azure AD müssen Sie Central Desktop aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Central Desktop aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) auf das Symbol **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen ausführen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Klicken Sie im oberen Bereich des Dialogfelds auf die Schaltfläche **Neue Anwendung**, um neue Anwendungen hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld als Suchbegriff **Central Desktop** ein. Wählen Sie im Ergebnisbereich **Central Desktop** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Central Desktop in der Ergebnisliste](./media/central-desktop-tutorial/tutorial_centraldesktop_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Central Desktop mithilfe der Testbenutzerin Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Central Desktop als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und einem entsprechenden Benutzer in Central Desktop muss eine Verknüpfung eingerichtet werden.

Weisen Sie in Central Desktop dem Wert **Username** denselben Wert wie dem **Benutzernamen** in Azure AD zu. Damit haben Sie die Verknüpfung zwischen den beiden Benutzern eingerichtet.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Central Desktop müssen Sie die folgenden Bausteine ausführen:

1. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on), um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. [Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user), um das einmalige Anmelden mit Azure AD mit der Testbenutzerin Britta Simon zu testen.
1. [Erstellen eines Central Desktop-Testbenutzers](#create-a-central-desktop-test-user), um ein Pendant zu Britta Simon in Central Desktop zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
1. [Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user), um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. [Testen des einmaligen Anmeldens](#test-single-sign-on), um sicherzustellen, dass die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Central Desktop-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Central Desktop die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Central Desktop** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie zum Aktivieren des einmaligen Anmeldens im Dialogfeld **Einmaliges Anmelden** in der Dropdownliste **Modus** die Option **SAML-basierte Anmeldung** aus.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/central-desktop-tutorial/tutorial_centraldesktop_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für Central Desktop** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Central Desktop](./media/central-desktop-tutorial/tutorial_centraldesktop_url.png)

    a. Geben Sie im Feld **URL für Anmeldung** eine URL im folgenden Format ein: `https://<companyname>.centraldesktop.com`.

    b. Geben Sie im Feld **Bezeichner** eine URL im folgenden Format ein:
    | |
    |--|
    | `https://<companyname>.centraldesktop.com/saml2-metadata.php`|
    | `https://<companyname>.imeetcentral.com/saml2-metadata.php`|

    c. Geben Sie im Feld **Antwort-URL** eine URL im folgenden Format ein: `https://<companyname>.centraldesktop.com/saml2-assertion.php`    
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte mit dem tatsächlichen Bezeichner, der Antwort-URL und der Anmelde-URL aktualisieren. Wenden Sie sich an den [Central Desktop-Support](https://imeetcentral.com/contact-us), um diese Werte zu erhalten. 

1. Wählen Sie im Abschnitt **SAML-Signaturzertifikat** die Option **Zertifikat** aus. Speichern Sie das Zertifikat auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/central-desktop-tutorial/tutorial_centraldesktop_certificate.png) 

1. Wählen Sie die Schaltfläche **Speichern** aus.

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/central-desktop-tutorial/tutorial_general_400.png)
    
1. Wählen Sie im Abschnitt **Central Desktop-Konfiguration** die Option **Central Desktop konfigurieren** aus, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Central Desktop-Konfiguration](./media/central-desktop-tutorial/tutorial_centraldesktop_configure.png) 

1. Melden Sie sich bei Ihrem **Central Desktop**-Mandanten an.

1. Wechseln Sie zu **Einstellungen**. Wählen Sie **Advanced**, und wählen Sie dann **Single Sign On**.

    ![Setup – Erweitert](./media/central-desktop-tutorial/ic769563.png "Setup – Erweitert")

1. Führen Sie im Dialogfeld **Single Sign On Settings** die folgenden Schritte aus:

    ![Einstellungen für einmaliges Anmelden](./media/central-desktop-tutorial/ic769564.png "Einstellungen für einmaliges Anmelden")
    
    a. Aktivieren Sie **Enable SAML v2 Single Sign On**.
    
    b. Fügen Sie im Feld **SSO URL** den Wert für **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.
    
    c. Fügen Sie im Feld **SSO Login URL** den Wert für **URL für den SAML-SSO-Dienst** ein, den Sie aus dem Azure-Portal kopiert haben.
    
    d. Fügen Sie im Feld **SSO Logout URL** den Wert für  **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

1. Führen Sie im Abschnitt **Message Signature Verification Method** die folgenden Schritte aus:

    ![Methode zur Nachrichtensignaturüberprüfung](./media/central-desktop-tutorial/ic769565.png "Methode zur Nachrichtensignaturüberprüfung") a. Wählen Sie **Certificate**aus.
    
    b. Wählen Sie in der Liste **SSO Certificate** die Option **RSH SHA256** aus.
    
    c. Öffnen Sie das heruntergeladene Zertifikat in Editor. Kopieren Sie dann den Inhalt des Zertifikats, und fügen Sie ihn in das Feld **SSO Certificate** ein.
        
    d. Aktivieren Sie **Display a link to your SAMLv2 login page**.
    
    e. Wählen Sie **Update** aus.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen. Nachdem Sie diese App über den Abschnitt **Active Directory** > **Unternehmensanwendungen** hinzugefügt haben, navigieren Sie zur Registerkarte **Einmaliges Anmelden**, und rufen Sie am unteren Rand im Abschnitt **Konfiguration** die eingebettete Dokumentation auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Wählen Sie im linken Bereich des Azure-Portals die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/central-desktop-tutorial/create_aaduser_01.png)

1. Um die Liste der Benutzer anzuzeigen, wechseln Sie zu **Benutzer und Gruppen**. Wählen Sie dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/central-desktop-tutorial/create_aaduser_02.png)

1. Klicken Sie im oberen Bereich des Dialogfelds **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/central-desktop-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/central-desktop-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-central-desktop-test-user"></a>Erstellen eines Testbenutzers für Central Desktop

Damit sich Azure AD-Benutzer anmelden können, müssen sie in der Central Desktop-Anwendung bereitgestellt werden. In diesem Abschnitt wird beschrieben, wie in Central Desktop Azure AD-Benutzerkonten erstellt werden.

> [!NOTE]
> Sie können Azure AD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Central Desktop-Benutzerkonten oder mithilfe der von Central Desktop bereitgestellten APIs bereitstellen.

**So stellen Sie Benutzerkonten für Central Desktop bereit**

1. Melden Sie sich bei Ihrem Central Desktop-Mandanten an.

1. Wechseln Sie zu **People** > **Internal Members**.

1. Wählen Sie **Add Internal Members** aus.

    ![Personen](./media/central-desktop-tutorial/ic781051.png "Personen")
    
1. Geben Sie im Textfeld **Email Address of New Members** ein Azure AD-Konto ein, das Sie bereitstellen möchten, und klicken Sie dann auf **Next**.

    ![E-Mail-Adressen neuer Mitglieder](./media/central-desktop-tutorial/ic781052.png "E-Mail-Adressen neuer Mitglieder")

1. Wählen Sie **Add Internal member(s)** aus.

    ![Internes Mitglied hinzufügen](./media/central-desktop-tutorial/ic781053.png "Internes Mitglied hinzufügen")
   
   >[!NOTE]
   >Die Benutzer, die Sie hinzufügen, erhalten eine E-Mail, die einen Bestätigungslink zum Aktivieren ihrer Konten enthält.
   
### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie der Benutzerin Britta Simon das einmalige Anmelden von Azure, indem Sie Zugriff auf Central Desktop erteilen.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Central Desktop zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht. Wechseln Sie dann zur Verzeichnisansicht und zu **Unternehmensanwendungen**.

1. Wählen Sie **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **Central Desktop** aus.

    ![Central Desktop-Link in der Anwendungsliste](./media/central-desktop-tutorial/tutorial_centraldesktop_app.png)  

1. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“][202]

1. Wählen Sie die Schaltfläche **Hinzufügen** aus. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** unter **Benutzer** die Option **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel „Central Desktop“ auswählen, werden Sie automatisch bei Ihrer Central Desktop-Anwendung angemeldet.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/central-desktop-tutorial/tutorial_general_01.png
[2]: ./media/central-desktop-tutorial/tutorial_general_02.png
[3]: ./media/central-desktop-tutorial/tutorial_general_03.png
[4]: ./media/central-desktop-tutorial/tutorial_general_04.png

[100]: ./media/central-desktop-tutorial/tutorial_general_100.png

[200]: ./media/central-desktop-tutorial/tutorial_general_200.png
[201]: ./media/central-desktop-tutorial/tutorial_general_201.png
[202]: ./media/central-desktop-tutorial/tutorial_general_202.png
[203]: ./media/central-desktop-tutorial/tutorial_general_203.png

