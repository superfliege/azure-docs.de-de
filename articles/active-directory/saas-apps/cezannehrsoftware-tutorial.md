---
title: 'Tutorial: Azure Active Directory-Integration mit Cezanne HR Software | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Cezanne HR Software konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 62b42e15-c282-492d-823a-a7c1c539f2cc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jeedes
ms.openlocfilehash: 1d5edd1335d99c369fa3ef560470edab41276a7a
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54819729"
---
# <a name="tutorial-azure-active-directory-integration-with-cezanne-hr-software"></a>Tutorial: Azure Active Directory-Integration mit Cezanne HR Software

In diesem Tutorial erfahren Sie, wie Sie Cezanne HR Software in Azure Active Directory (Azure AD) integrieren.

Die Integration von Cezanne HR Software in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf Cezanne HR Software Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Cezanne HR Software anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Cezanne HR Software konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Cezanne HR Software-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Cezanne HR Software aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-cezanne-hr-software-from-the-gallery"></a>Hinzufügen von Cezanne HR Software aus dem Katalog
Zum Konfigurieren der Integration von Cezanne HR Software in Azure AD müssen Sie Cezanne HR Software aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Cezanne HR Software aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld **Cezanne HR Software** ein, wählen Sie im Ergebnisbereich **Cezanne HR Software** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Cezanne HR Software in der Ergebnisliste](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

Dieser Abschnitt veranschaulicht anhand eines Testbenutzers namens Britta Simon, wie das einmalige Anmelden von Azure AD in Cezanne HR Software konfiguriert und getestet wird.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, wer der entsprechende Gegenbenutzer in Cezanne HR Software zu einem Benutzer in Azure AD ist. Anders ausgedrückt muss zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Cezanne HR Software eine Linkbeziehung eingerichtet werden.

Weisen Sie in Cezanne HR Software den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Cezanne HR Software müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Erstellen eines Cezanne HR Software-Testbenutzers](#create-a-cezannehrsoftware-test-user)**, um eine Entsprechung von Britta Simon in Cezanne HR Software zu erhalten, die mit ihrer Benutzerdarstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Cezanne HR Software-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Cezanne HR Software die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Cezanne HR Software** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für Cezanne HR Software** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Cezanne HR Software](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** diese URL ein: `https://w3.cezanneondemand.com/CezanneOnDemand/-/<tenantidentifier>`

    b. Geben Sie im Textfeld **Bezeichner** die folgende URL ein: `https://w3.cezanneondemand.com/CezanneOnDemand/`.

    c. Geben Sie im Textfeld **Antwort-URL** folgende URL ein: `https://w3.cezanneondemand.com:443/cezanneondemand/-/<tenantidentifier>/Saml/samlp`
    
    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und Antwort-URL. Wenden Sie sich an das [Kundensupportteam von Cezanne HR Software](https://cezannehr.com/services/support/), um diese Werte zu erhalten.

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/cezannehrsoftware-tutorial/tutorial_general_400.png)

1. Klicken Sie im Abschnitt mit der **Cezanne HR Software-Konfiguration** auf **Cezanne HR Software konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen.

    ![Cezanne HR Software-Konfiguration](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure.png)

1. Scrollen Sie nach unten zum Abschnitt **Kurzübersicht**. Kopieren Sie die **URL für den SAML-SSO-Dienst und die SAML-Entitäts-ID** aus dem Abschnitt **Kurzübersicht**.

    ![Cezanne HR Software-Konfiguration](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure1.png)

1. Melden Sie sich in einem anderen Webbrowserfenster an Ihrem Cezanne HR Software-Mandanten als Administrator an.

1. Klicken Sie im linken Navigationsbereich auf **System Setup**(Systeminstallation). Wechseln Sie zu **Sicherheitseinstellungen**. Navigieren Sie anschließend zu **Single Sign-On Configuration**(Konfiguration des einmaligen Anmeldens).

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

1. Aktivieren Sie im Bereich **Allow users to log in using the following Single Sign-On (SSO) Service** (Benutzeranmeldung per SSO-Dienst zulassen) das Kontrollkästchen **SAML 2.0**, und wählen Sie die Option für die **Erweiterte Konfiguration** aus.

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

1. Klicken Sie auf die Schaltfläche **Neu hinzufügen** .

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

1. Führen Sie im Abschnitt **SAML 2.0 IDENTITY PROVIDERS** (SAML 2.0-IDENTITÄTSANBIETER) die folgenden Schritte aus:

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)
    
    a. Geben Sie den Namen Ihres Identitätsanbieters als **Anzeigename**ein.

    b. Fügen Sie in das Textfeld **Entitätsbezeichner** den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben. 

    c. Ändern Sie **SAML-Bindung** in „POST“.

    d. Fügen Sie in das Textfeld **Sicherheitstoken-Dienstendpunkt** den Wert der **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.

    e. Geben Sie im Textfeld „Benutzer-ID-Attributname“ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` ein.
    
    f. Klicken Sie auf das Symbol **Hochladen**, um das heruntergeladene Zertifikat aus dem Azure-Portal hochzuladen.
    
    g. Klicken Sie auf die Schaltfläche **OK** . 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Dokumentation zu eingebettetem Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/cezannehrsoftware-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/cezannehrsoftware-tutorial/create_aaduser_02.png)

1. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/cezannehrsoftware-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/cezannehrsoftware-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-cezanne-hr-software-test-user"></a>Erstellen eines Cezanne HR Software-Testbenutzers

Damit sich Azure AD-Benutzer bei Cezanne HR Software anmelden können, müssen sie in Cezanne HR Software bereitgestellt werden. Bei Cezanne HR Software ist die Bereitstellung ein manueller Vorgang.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1.  Melden Sie sich als Administrator auf Ihrer Cezanne HR Software-Unternehmenswebsite an.

1.  Klicken Sie im linken Navigationsbereich auf **System Setup**(Systeminstallation). Navigieren Sie zu **Benutzer verwalten**. Navigieren Sie anschließend zu **Neuen Benutzer hinzufügen**.

    ![Neuer Benutzer](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "Neuer Benutzer")

1.  Führen Sie im Abschnitt **Person Details** (Angaben zur Person) die folgenden Schritte aus:

    ![Neuer Benutzer](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "Neuer Benutzer")
    
    a. Legen Sie **Interner Benutzer** auf „AUS“ fest.
    
    b. Geben Sie im Textfeld **Vorname** den Vornamen des Benutzers ein, z.B. **Britta**.  
 
    c. Geben Sie im Textfeld **Nachname** den Nachnamen des Benutzers ein, z.B. **Simon**.
    
    d. Geben Sie im Textfeld **E-Mail-Adresse** die E-Mail-Adresse des Benutzers ein, z.B. Brittasimon@contoso.com.

1.  Führen Sie unter **Kontoinformationen** die folgenden Schritte aus:

    ![Neuer Benutzer](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "Neuer Benutzer")
    
    a. Geben Sie im Textfeld **Benutzername** die E-Mail-Adresse des Benutzers, z.B. Brittasimon@contoso.com, ein.
    
    b. Geben Sie im Textfeld **Kennwort** das Kennwort des Benutzers ein.
    
    c. Wählen Sie unter **Sicherheitsrolle** die Option **HR Professional** (HR-Mitarbeiter).
    
    d. Klicken Sie auf **OK**.

1. Navigieren Sie zur Registerkarte **Einmalige Anmeldung**, und wählen Sie im Bereich **SAML 2.0 Identifiers** (SAML 2.0-Bezeichner) die Option **Neu hinzufügen**.

    ![Benutzer](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "Benutzer")

1. Wählen Sie unter **Identitätsanbieter** Ihren Identitätsanbieter aus, und geben Sie im Textfeld von **Benutzer-ID** die E-Mail-Adresse des Kontos von Britta Simon ein.

    ![Benutzer](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "Benutzer")
    
1. Klicken Sie auf die Schaltfläche **Save** .

    ![Benutzer](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "Benutzer")

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Cezanne HR Software gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Cezanne HR Software zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **Cezanne HR Software**aus.

    ![Der Cezanne HR Software-Link in der Anwendungsliste](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_app.png)  

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Cezanne HR Software“ klicken, sollten Sie automatisch an Ihrer Cezanne HR Software-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/cezannehrsoftware-tutorial/tutorial_general_04.png

[100]: ./media/cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/cezannehrsoftware-tutorial/tutorial_general_201.png
[202]: ./media/cezannehrsoftware-tutorial/tutorial_general_202.png
[203]: ./media/cezannehrsoftware-tutorial/tutorial_general_203.png

