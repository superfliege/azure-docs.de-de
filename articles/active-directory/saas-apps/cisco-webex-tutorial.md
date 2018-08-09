---
title: 'Tutorial: Azure Active Directory-Integration mit Cisco Webex | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Cisco Webex konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 4dcf487afdad899853c97d3d2a1493a6123b3bab
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440712"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Tutorial: Azure Active Directory-Integration mit Cisco Webex

In diesem Tutorial erfahren Sie, wie Sie Cisco Webex in Azure Active Directory (Azure AD) integrieren.

Die Integration von Cisco Webex in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf Cisco Webex Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Cisco Webex anzumelden.
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Cisco Webex konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Cisco Webex-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige kostenlose Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Cisco Webex aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="add-cisco-webex-from-the-gallery"></a>Hinzufügen von Cisco Webex aus dem Katalog
Zum Konfigurieren der Integration von Cisco Webex in Azure AD müssen Sie Cisco Webex aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Cisco Webex aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) auf das Symbol **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen ausführen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Wählen Sie oben im Dialogfeld die Schaltfläche **Neue Anwendung** aus, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld den Begriff **Cisco Webex** ein. 

1. Wählen Sie im Ergebnisbereich **Cisco Webex** aus. Wählen Sie dann die Schaltfläche **Hinzufügen** aus, um die Anwendung hinzuzufügen.

    ![Cisco Webex in der Ergebnisliste](./media/cisco-webex-tutorial/tutorial_ciscowebex_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Cisco Webex mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Cisco Webex als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und einem entsprechenden Benutzer in Cisco Webex muss eine Verknüpfung eingerichtet werden.

Weisen Sie in Cisco Webex dem Wert **Username** denselben Wert wie dem **Benutzernamen** in Azure AD zu. Damit haben Sie die Verknüpfung zwischen den beiden Benutzern eingerichtet. 

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Cisco Webex müssen Sie die folgenden Aufgaben ausführen:

1. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on), um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. [Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user), um das einmalige Anmelden mit Azure AD mit der Testbenutzerin Britta Simon zu testen.
1. [Erstellen eines Cisco Webex-Testbenutzers](#create-a-cisco-webex-test-user), um ein Pendant zu Britta Simon in Cisco Webex zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
1. [Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user), um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. [Testen des einmaligen Anmeldens](#test-single-sign-on), um sicherzustellen, dass die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Cisco Webex-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Cisco Webex die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Cisco Webex** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie zum Aktivieren des einmaligen Anmeldens im Dialogfeld **Einmaliges Anmelden** in der Dropdownliste **Modus** die Option **SAML-basierte Anmeldung** aus.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/cisco-webex-tutorial/tutorial_ciscowebex_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für Cisco Webex** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Cisco Webex](./media/cisco-webex-tutorial/tutorial_ciscowebex_url.png)

    a. Geben Sie im Feld **URL für Anmeldung** eine URL im folgenden Format ein: `https://<subdomain>.webex.com`.

    b. Geben Sie im Feld **Bezeichner** die URL ein`http://www.webex.com`.

    c. Geben Sie im Feld **Antwort-URL** eine URL im folgenden Format ein: `https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company`
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Die Werte müssen durch die tatsächliche Antwort-URL und die tatsächliche Anmelde-URL ersetzt werden. Wenden Sie sich an das [Clientsupportteam von Cisco Webex](https://www.webex.co.in/support/support-overview.html), um diese Werte zu erhalten. 

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/cisco-webex-tutorial/tutorial_ciscowebex_certificate.png) 

1. Wählen Sie **Speichern**aus.

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/cisco-webex-tutorial/tutorial_general_400.png)
    
1. Wählen Sie im Abschnitt **Cisco Webex-Konfiguration** die Option **Cisco Webex konfigurieren** aus, um das Fenster **Anmeldung konfigurieren**  zu öffnen. Kopieren Sie die **Abmelde-URL**, die **SAML-Entitäts-ID** und die **URL für den SAML-SSO-Dienst** aus dem Bereich **Kurzübersicht**.

    ![Configure single sign-on](./media/cisco-webex-tutorial/tutorial_ciscowebex_configure.png) 

1. Melden Sie sich in einem anderen Webbrowserfenster bei der Cisco Webex-Unternehmenswebsite als Administrator an.

1. Wählen Sie im oberen Menü **Site Administration** aus.

    ![Websiteverwaltung](./media/cisco-webex-tutorial/ic777621.png "Websiteverwaltung")

1. Klicken Sie im Abschnitt **Manage Site** auf **SSO Configuration**.
   
    ![SSO-Konfiguration](./media/cisco-webex-tutorial/ic777622.png "SSO-Konfiguration")

1. Führen Sie im Abschnitt **Federated Web SSO Configuration** die folgenden Schritte aus:
   
    ![Verbund-SSO-Konfiguration](./media/cisco-webex-tutorial/ic777623.png "Verbund-SSO-Konfiguration")  

    a. Wählen Sie in der Liste **Federation Protocol** die Option **SAML 2.0** aus.

    b. Aktivieren Sie für **SSO profile** die Option **SP Initiated**.

    c. Öffnen Sie das heruntergeladene Zertifikat in Editor, und kopieren Sie den Inhalt.

    d. Wählen Sie **Import SAML Metadata** aus, und fügen Sie den kopierten Inhalt des Zertifikats ein.

    e. Fügen Sie in das Feld **Issuer for SAML (IdP ID)** den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.

    f. Fügen Sie in das Feld **Customer SSO Service Login URL** die **URL für den SAML-SSO-Dienst** ein, die Sie aus dem Azure-Portal kopiert haben.

    g. Wählen Sie in der Liste **NameID-Format** die Option **E-Mail-Adresse** aus.

    h. Geben Sie in das Feld **AuthnContextClassRef** die Zeichenfolge **urn:oasis:names:tc:SAML:2.0:ac:classes:Password** ein.

    i. Fügen Sie in das Feld **Customer SSO Service Logout URL** die **Abmelde-URL** ein, die Sie aus dem Azure-Portal kopiert haben.
   
    j. Wählen Sie **Update** aus.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen. Nachdem Sie diese App über den Abschnitt **Active Directory** > **Unternehmensanwendungen** hinzugefügt haben, navigieren Sie zur Registerkarte **Einmaliges Anmelden**, und rufen Sie am unteren Rand im Abschnitt **Konfiguration** die eingebettete Dokumentation auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation](https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Wählen Sie im linken Bereich des Azure-Portals die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/cisco-webex-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und wählen Sie dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/cisco-webex-tutorial/create_aaduser_02.png)

1. Klicken Sie im oberen Bereich des Dialogfelds **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/cisco-webex-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/cisco-webex-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-cisco-webex-test-user"></a>Erstellen eines Cisco Webex-Testbenutzers

Damit sich Azure AD-Benutzer bei Cisco Webex anmelden können, müssen sie in Cisco Webex bereitgestellt werden. Im Fall von Cisco Webex ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrem **Cisco Webex**-Mandanten an.

1. Wählen Sie **Manage Users** > **Add User** aus.
   
    ![Benutzer hinzufügen](./media/cisco-webex-tutorial/ic777625.png "Benutzer hinzufügen")

1. Führen Sie im Abschnitt **Add User** die folgenden Schritte aus:
   
    ![Benutzer hinzufügen](./media/cisco-webex-tutorial/ic777626.png "Benutzer hinzufügen")   

    a. Wählen Sie unter **Account Type** den Kontotyp **Host** aus.

    b. Geben Sie im Feld **First Name** den Vornamen des Benutzers ein (in diesem Fall **Britta**).

    c. Geben Sie im Feld **Last Name** den Nachnamen des Benutzers ein (in diesem Fall **Simon**).

    d. Geben Sie im Feld **Username** die E-Mail-Adresse des Benutzers ein (in diesem Fall **Brittasimon@contoso.com**).

    e. Geben Sie im Feld **Email** die E-Mail-Adresse des Benutzers ein (in diesem Fall **Brittasimon@contoso.com**).

    f. Geben Sie im Feld **Password** das Kennwort des Benutzers ein.

    g. Geben Sie das Kennwort im Feld **Confirm Password** das Kennwort des Benutzers noch einmal ein.

    h. Wählen Sie **Hinzufügen**.

>[!NOTE]
>Sie können Azure AD-Benutzerkonten auch mithilfe von anderen Tools zum Erstellen von Cisco Webex-Benutzerkonten oder mithilfe der von Cisco Webex bereitgestellten APIs bereitstellen. 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie der Benutzerin Britta Simon das einmalige Anmelden von Azure, indem Sie Zugriff auf Cisco Webex erteilen.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um Britta Simon Cisco Webex zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht. Wechseln Sie als Nächstes zur Verzeichnisansicht und dann zu **Unternehmensanwendungen**.  

1. Wählen Sie **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **Cisco Webex**aus.

    ![Cisco Webex-Link in der Anwendungsliste](./media/cisco-webex-tutorial/tutorial_ciscowebex_app.png)  

1. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“][202]

1. Wählen Sie die Schaltfläche **Hinzufügen** aus. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** unter **Benutzer** die Option **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel „Cisco Webex“ auswählen, werden Sie automatisch bei Ihrer Cisco Webex-Anwendung angemeldet.

Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/cisco-webex-tutorial/tutorial_general_01.png
[2]: ./media/cisco-webex-tutorial/tutorial_general_02.png
[3]: ./media/cisco-webex-tutorial/tutorial_general_03.png
[4]: ./media/cisco-webex-tutorial/tutorial_general_04.png

[100]: ./media/cisco-webex-tutorial/tutorial_general_100.png

[200]: ./media/cisco-webex-tutorial/tutorial_general_200.png
[201]: ./media/cisco-webex-tutorial/tutorial_general_201.png
[202]: ./media/cisco-webex-tutorial/tutorial_general_202.png
[203]: ./media/cisco-webex-tutorial/tutorial_general_203.png

