---
title: 'Tutorial: Azure Active Directory-Integration mit Riskware | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und Riskware konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 81866167-b163-4695-8978-fd29a25dac7a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: jeedes
ms.openlocfilehash: 3b4c979bf03b23280c9389a043375f088624efe6
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55163244"
---
# <a name="tutorial-azure-active-directory-integration-with-riskware"></a>Tutorial: Azure Active Directory-Integration mit Riskware

In diesem Tutorial erfahren Sie, wie Sie Riskware in Azure Active Directory (Azure AD) integrieren.

Die Integration von Riskware in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Riskware hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Riskware anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in Riskware konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Riskware-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Riskware aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-riskware-from-the-gallery"></a>Hinzufügen von Riskware aus dem Katalog
Zum Konfigurieren der Integration von Riskware in Azure AD müssen Sie Riskware aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**So fügen Sie Riskware aus dem Katalog hinzu**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld **Riskware** ein, wählen Sie im Ergebnisbereich **Riskware** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Riskware in der Ergebnisliste](./media/riskware-tutorial/tutorial_riskware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Riskware anhand einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Riskware als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Riskware-Benutzer muss eine Linkbeziehung eingerichtet werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Riskware zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Erstellen eines Riskware-Testbenutzers](#create-a-riskware-test-user)**, um eine Entsprechung für Britta Simon in Riskware zu haben, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Riskware-Anwendung.

**So konfigurieren Sie das einmalige Anmelden von Azure AD mit Riskware**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Riskware** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Dialogfeld „Einmaliges Anmelden“](./media/riskware-tutorial/tutorial_riskware_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für Riskware** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Riskware](./media/riskware-tutorial/tutorial_riskware_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL nach folgendem Muster ein:
    | Environment| URL-Muster|
    |--|--|
    | UAT|  `https://riskcloud.net/uat?ccode=<COMPANYCODE>` |
    | PROD| `https://riskcloud.net/prod?ccode=<COMPANYCODE>` |
    | DEMO| `https://riskcloud.net/demo?ccode=<COMPANYCODE>` |
    |||

    b. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL ein:
    | Environment| URL-Muster|
    |--|--|
    | UAT| `https://riskcloud.net/uat` |
    | PROD| `https://riskcloud.net/prod` |
    | DEMO| `https://riskcloud.net/demo` |
    |||

    > [!NOTE]
    > Der Wert der Anmelde-URL entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Supportteam von Riskware](mailto:support@pansoftware.com.au), um diesen Wert zu erhalten.

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/riskware-tutorial/tutorial_riskware_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/riskware-tutorial/tutorial_general_400.png)

1. Klicken Sie im Abschnitt **Riskware-Konfiguration** auf **Riskware konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Riskware-Konfiguration](./media/riskware-tutorial/tutorial_riskware_configure.png)

1. Melden Sie sich in einem anderen Webbrowserfenster auf der Riskware-Unternehmenswebsite als Administrator an.

1. Klicken Sie oben rechts auf **Maintenance** (Wartung), um die Wartungsseite zu öffnen.

    ![Wartung bei Riskware-Konfiguration](./media/riskware-tutorial/tutorial_riskware_maintain.png)

1. Klicken Sie auf der Wartungsseite auf **Authentication** (Authentifizierung).

    ![Authentifizierung bei Riskware-Konfiguration](./media/riskware-tutorial/tutorial_riskware_authen.png)

1. Führen Sie auf der Seite **Authentication Configuration** (Authentifizierungskonfiguration) die folgenden Schritte aus:

    ![Authentifizierung und Wartung bei Riskware-Konfiguration](./media/riskware-tutorial/tutorial_riskware_config.png)

    a. Legen Sie für die Authentifizierung **Type** (Typ) als **SAML** (SAML) fest.

    b. Geben Sie im Textfeld **Code** (Code) den Code ein, z.B. „AZURE_UAT“.

    c. Geben Sie im Textfeld **Description** (Beschreibung) eine Beschreibung ein, z.B. „AZURE Configuration for SSO“ (AZURE-Konfiguration für SSO).

    d. Fügen Sie im Textfeld **Single Sign On Page** (Seite für einmaliges Anmelden) den Wert **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.

    e. Fügen Sie in das Textfeld **Sign out Page** (Abmeldeseite) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    f. Geben Sie im Textfeld **Post Form Field** (Formularfeld für POST) den Feldnamen in der SAML-Antwort ein, z.B. „SAMLResponse“.

    g. Geben Sie im Textfeld **XML Identity Tag Name** (Tagname der XML-Identität) das Attribut ein, das den eindeutigen Bezeichner in der SAML-Antwort enthält, z.B. „NameID“.

    h. Öffnen Sie die aus dem Azure-Portal heruntergeladene  **Metadaten-XML** im Editor, kopieren Sie das Zertifikat aus der Metadatendatei, und fügen Sie es in das Textfeld **Certificate** (Zertifikat) ein.

    i. Fügen Sie in das Textfeld **Consumer URL** (Consumer-URL) den Wert der **Antwort-URL** ein, den Sie vom Supportteam erhalten.

    j. Fügen Sie in das Textfeld **Issuer** (Aussteller) den Wert des **Bezeichners** ein, den Sie vom Supportteam erhalten.

    > [!Note]
    > Wenden Sie sich an das [Supportteam von Riskware](mailto:support@pansoftware.com.au), um diese Werte zu erhalten.

    k. Aktivieren Sie das Kontrollkästchen **Use POST** (POST verwenden).

    l. Aktivieren Sie das Kontrollkästchen **Use SAML Request** (SAML-Anforderung verwenden).

    m. Klicken Sie auf **Speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/riskware-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/riskware-tutorial/create_aaduser_02.png)

1. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/riskware-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/riskware-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="create-a-riskware-test-user"></a>Erstellen eines Riskware-Testbenutzers

Damit sich Azure AD-Benutzer bei Riskware anmelden können, müssen sie in Riskware bereitgestellt werden. Im Fall von Riskware muss die Bereitstellung manuell ausgeführt werden.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei Riskware als Sicherheitsadministrator an.

1. Klicken Sie oben rechts auf **Maintenance** (Wartung), um die Wartungsseite zu öffnen. 

    ![Wartung bei Riskware-Konfiguration](./media/riskware-tutorial/tutorial_riskware_maintain.png)

1. Klicken Sie auf der Wartungsseite auf **People** (Personen).

    ![Personen bei Riskware-Konfiguration](./media/riskware-tutorial/tutorial_riskware_people.png)

1. Wählen Sie die Registerkarte **Details** aus, und führen Sie die folgenden Schritte aus:

    ![Riskware-Konfigurationsdetails](./media/riskware-tutorial/tutorial_riskware_details.png)

    a. Wählen Sie **Person Type** (Personentyp) aus, z.B. „Employee“ (Mitarbeiter).

    b. Geben Sie im Textfeld **First Name** (Vorname) den Vornamen des Benutzers ein, z.B. **Britta**.

    c. Geben Sie im Textfeld **Surname** (Nachname) den Nachnamen des Benutzers ein, z.B. **Simon**.

1. Führen Sie im Abschnitt **Sicherheit** die folgenden Schritte aus:

    ![Sicherheit bei Riskware-Konfiguration](./media/riskware-tutorial/tutorial_riskware_security.png)

    a. Wählen Sie im Abschnitt **Authentication** (Authentifizierung) den von Ihnen eingerichteten Modus **Authentication** (Authentifizierung) aus, z.B. „AZURE Configuration for SSO“ (AZURE-Konfiguration für SSO).

    b. Geben Sie im Abschnitt **Logon Details** (Anmeldeinformationen) im Textfeld **User ID** (Benutzer-ID) die E-Mail-Adresse des Benutzers ein, z.B. **brittasimon@contoso.com**.

    c. Geben Sie im Textfeld **Password** (Kennwort) das Kennwort des Benutzers ein.

1. Führen Sie auf der Registerkarte **Organisation** die folgenden Schritte aus:

    ![Organisation bei Riskware-Konfiguration](./media/riskware-tutorial/tutorial_riskware_org.png)

    a. Wählen Sie als Option die Organisation **Level1** aus.

    b. Geben Sie im Abschnitt **Person's Primary Workplace** (Primärer Arbeitsplatz der Person) im Textfeld **Location** (Ort) Ihren Standort ein.

    c. Wählen Sie im Abschnitt **Employee** (Mitarbeiter) den **Employee Status** (Mitarbeiterstatus) aus, z.B. „Casual“ (Gelegentlich).

1. Klicken Sie auf **Speichern**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt gewähren Sie Britta Simon Zugriff auf Riskware, damit sie das einmalige Anmelden von Azure verwenden kann.

![Zuweisen der Benutzerrolle][200]

**So weisen Sie Britta Simon Riskware zu**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **Riskware** aus.

    ![Riskware-Link in der Anwendungsliste](./media/riskware-tutorial/tutorial_riskware_app.png)  

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Riskware“ klicken, werden Sie automatisch bei Ihrer Riskware-Anwendung angemeldet.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/riskware-tutorial/tutorial_general_01.png
[2]: ./media/riskware-tutorial/tutorial_general_02.png
[3]: ./media/riskware-tutorial/tutorial_general_03.png
[4]: ./media/riskware-tutorial/tutorial_general_04.png

[100]: ./media/riskware-tutorial/tutorial_general_100.png

[200]: ./media/riskware-tutorial/tutorial_general_200.png
[201]: ./media/riskware-tutorial/tutorial_general_201.png
[202]: ./media/riskware-tutorial/tutorial_general_202.png
[203]: ./media/riskware-tutorial/tutorial_general_203.png

