---
title: 'Tutorial: Azure Active Directory-Integration mit Meta Networks Connector | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und Meta Networks Connector konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4ae5f30d-113b-4261-b474-47ffbac08bf7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2018
ms.author: jeedes
ms.openlocfilehash: a3f40624e51ef287d70bed547eba7ec9e0882b0e
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669194"
---
# <a name="tutorial-azure-active-directory-integration-with-meta-networks-connector"></a>Tutorial: Azure Active Directory-Integration mit Meta Networks Connector

In diesem Tutorial erfahren Sie, wie Sie Meta Networks Connector in Azure Active Directory (Azure AD) integrieren.

Die Integration von Meta Networks Connector in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Meta Networks Connector hat.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Meta Networks Connector anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Meta Networks Connector konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Meta Networks Connector-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Meta Networks Connector aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-meta-networks-connector-from-the-gallery"></a>Hinzufügen von Meta Networks Connector aus dem Katalog
Zum Konfigurieren der Integration von Meta Networks Connector in Azure AD müssen Sie Meta Networks Connector aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um Meta Networks Connector aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 
    
    ![Schaltfläche „Azure Active Directory“][1]
    
1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.
    
    ![Blatt „Unternehmensanwendungen“][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.
    
    ![Schaltfläche „Neue Anwendung“][3]
    
1. Geben Sie in das Suchfeld **Meta Networks Connector** ein, wählen Sie **Meta Networks Connector** im Ergebnisbereich aus, und klicken Sie dann auf **Hinzufügen**, um die Anwendung hinzuzufügen.
    
    ![Meta Networks Connector in der Ergebnisliste](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Meta Networks Connector mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Meta Networks Connector als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Meta Networks Connector muss eine Verknüpfungsbeziehung eingerichtet werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Meta Networks Connector zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Erstellen eines Meta Networks Connector-Testbenutzers](#create-a-meta-networks-connector-test-user)**, um eine Entsprechung von Britta Simon in Meta Networks Connector zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Meta Networks Connector-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Meta Networks Connector die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Meta Networks Connector** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für Meta Networks Connector** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Meta Networks Connector](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_url.png)

    1. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://login.nsof.io/v1/<ORGANIZATION-SHORT-NAME>/saml/metadata`
    
    1. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://login.nsof.io/v1/<ORGANIZATION-SHORT-NAME>/sso/saml`
    
1. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Meta Networks Connector](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_url1.png)

    1. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<ORGANIZATION-SHORT-NAME>.metanetworks.com/login`.
    
    1. Geben Sie im Textfeld **Relayzustand** eine URL nach folgendem Muster ein: `https://<ORGANIZATION-SHORT-NAME>.metanetworks.com/#/`
    
    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächlichen Werte für Bezeichner, Antwort-URL und Anmelde-URL. Darauf wird später im Tutorial eingegangen.
    
1. Die Meta Networks Connector-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:
    
    ![Configure single sign-on](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_attribute.png)
    
1. Konfigurieren Sie das SAML-Tokenattribut im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute**, wie im obigen Bild gezeigt, und führen Sie die folgenden Schritte aus:
    
    | Attributname | Attributwert | NAMESPACE|
    | ---------------| --------------- | -------- |
    | firstname | user.givenname | |
    | lastname | user.surname | |
    | emailaddress| user.mail| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | name | user.userprincipalname| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | phone | user.telephonenumber | |

    1. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

        ![Configure single sign-on](./media/metanetworksconnector-tutorial/tutorial_attribute_04.png)
    
        ![Configure single sign-on](./media/metanetworksconnector-tutorial/tutorial_attribute_05.png)   
    
    1. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    1. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.
    
    1. Geben Sie im Textfeld **Namespace** den für die betreffende Zeile angezeigten Namespacewert ein.
    
    1. Klicken Sie auf **OK**.
    
1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.
    
    ![Downloadlink für das Zertifikat](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_certificate.png)
    
1. Klicken Sie im Abschnitt **Meta Networks Connector-Konfiguration** auf **Meta Networks Connector konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.
    
    ![Configure single sign-on](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_configure.png)
    
1. Klicken Sie auf die Schaltfläche **Save** .
    
    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/metanetworksconnector-tutorial/tutorial_general_400.png)
    
1. Öffnen Sie eine neue Registerkarte in Ihrem Browser, und melden Sie sich mit Ihrem Meta Networks Connector-Administratorkonto an.
    
    > [!NOTE]
    > Meta Networks Connector ist ein sicheres System. Vor dem Zugriff auf das Portal müssen Sie daher Ihre öffentliche IP-Adresse auf die Whitelist setzen lassen. Klicken Sie zum Abrufen Ihrer öffentlichen IP-Adresse auf [diesen](https://whatismyipaddress.com/) Link. Senden Sie Ihre IP-Adresse an das [Supportteam für den Meta Networks Connector-Client](mailto:support@metanetworks.com), um sie auf die Whitelist setzen zu lassen.
    
1. Klicken Sie auf **Verwaltung** und dann auf **Einstellungen**.
    
    ![Configure single sign-on](./media/metanetworksconnector-tutorial/configure3.png)
    
1. Stellen Sie sicher, dass **Log Internet Traffic** (Internetdatenverkehr protokollieren) und **Force VPN MFA** (VPN-MFA erzwingen) deaktiviert sind.
    
    ![Configure single sign-on](./media/metanetworksconnector-tutorial/configure1.png)
    
1. Klicken Sie auf **Verwaltung** und dann auf **SAML**.
    
    ![Configure single sign-on](./media/metanetworksconnector-tutorial/configure4.png)
    
1. Führen Sie auf der Registerkarte **DETAILS** die folgenden Schritte aus:
    
    ![Configure single sign-on](./media/metanetworksconnector-tutorial/configure2.png)
    
    1. Kopieren Sie den Wert für **SSO-URL**, und fügen Sie ihn im Abschnitt **Domäne und URLs für Meta Networks Connector** ins Textfeld **Anmelde-URL** ein.
    
    1. Kopieren Sie den Wert für **Empfänger-URL**, und fügen Sie ihn im Abschnitt **Domäne und URLs für Meta Networks Connector** ins Textfeld **Antwort-URL** ein.
    
    1. Kopieren Sie den Wert für **Audience URI (SP Entity ID)** (Zielgruppen-URI (SP-Identitäts-ID)), und fügen Sie ihn im Abschnitt **Domäne und URLs für Meta Networks Connector** ins Textfeld **Bezeichner (Entitäts-ID)** ein.
    
    1. Aktivieren Sie die SAML.
    
1. Führen Sie auf der Registerkarte **ALLGEMEIN** die folgenden Schritte aus:

    ![Configure single sign-on](./media/metanetworksconnector-tutorial/configure5.png)

    1. Fügen Sie den Wert der **SAML-Dienst-URL für einmaliges Anmelden**, den Sie aus dem Azure-Portal kopiert haben, in das Feld **URL für einmaliges Anmelden des Identitätsanbieters** ein.

    1. Fügen Sie in das Feld **Identity Provider Issuer** (Aussteller des Identitätsanbieters) die **SAML-Entitäts-ID** ein, die Sie aus dem Azure-Portal kopiert haben.

    1. Öffnen Sie das aus dem Azure-Portal heruntergeladene Zertifikat im Editor, und fügen Sie es in das Textfeld **X 509 Certificate** (X.509-Zertifikat) ein.

    1. Aktivieren Sie die **Just-in-Time-Bereitstellung**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.
    
![Erstellen eines Azure AD-Testbenutzers][100]
    
**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.
    
    ![Schaltfläche „Azure Active Directory“](./media/metanetworksconnector-tutorial/create_aaduser_01.png)
    
1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.
    
    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/metanetworksconnector-tutorial/create_aaduser_02.png)
    
1. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
    
    ![Schaltfläche „Hinzufügen“](./media/metanetworksconnector-tutorial/create_aaduser_03.png)
    
1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/metanetworksconnector-tutorial/create_aaduser_04.png)
    
    1. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    1. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.
    
    1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.
    
    1. Klicken Sie auf **Create**.
    
### <a name="create-a-meta-networks-connector-test-user"></a>Erstellen eines Meta Networks Connector-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Meta Networks Connector. Meta Networks Connector unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf Meta Networks Connector ein neuer Benutzer erstellt.

>[!Note]
>Wenn Sie einen Benutzer manuell erstellen müssen, setzen Sie sich mit dem [Supportteam für den Meta Networks Connector-Client](mailto:support@metanetworks.com) in Verbindung.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt gewähren Sie Britta Simon Zugriff auf Meta Networks Connector, damit sie das einmalige Anmelden von Azure verwenden kann.

![Zuweisen der Benutzerrolle][200]

**Um Britta Simon Meta Networks Connector zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.
    
    ![Benutzer zuweisen][201]
    
1. Wählen Sie in der Anwendungsliste **Meta Networks Connector** aus.
    
    ![Der Meta Networks Connector-Link in der Anwendungsliste](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_app.png)  
    
1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.
    
    ![Link „Benutzer und Gruppen“][202]
    
1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
    
    ![Bereich „Zuweisung hinzufügen“][203]
    
1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.
    
1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.
    
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Meta Networks Connector“ klicken, werden Sie automatisch bei Ihrer Meta Networks Connector-Anwendung angemeldet.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/metanetworksconnector-tutorial/tutorial_general_01.png
[2]: ./media/metanetworksconnector-tutorial/tutorial_general_02.png
[3]: ./media/metanetworksconnector-tutorial/tutorial_general_03.png
[4]: ./media/metanetworksconnector-tutorial/tutorial_general_04.png

[100]: ./media/metanetworksconnector-tutorial/tutorial_general_100.png

[200]: ./media/metanetworksconnector-tutorial/tutorial_general_200.png
[201]: ./media/metanetworksconnector-tutorial/tutorial_general_201.png
[202]: ./media/metanetworksconnector-tutorial/tutorial_general_202.png
[203]: ./media/metanetworksconnector-tutorial/tutorial_general_203.png

