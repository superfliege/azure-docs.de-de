---
title: 'Tutorial: Azure Active Directory-Integration mit ServiceNow | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und ServiceNow konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a5a1a264-7497-47e7-b129-a1b5b1ebff5b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 5d5c4d5e26fa21488dd637805a4c22bd3ed18a7f
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421082"
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Tutorial: Azure Active Directory-Integration mit ServiceNow

In diesem Tutorial erfahren Sie, wie Sie ServiceNow in Azure Active Directory (Azure AD) integrieren.

Die Integration von ServiceNow in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf ServiceNow hat.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto bei ServiceNow automatisch anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit ServiceNow konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Für ServiceNow eine Instanz oder einen Mandanten von ServiceNow, Calgary-Version oder höher
- Für ServiceNow Express eine Instanz von ServiceNow Express, Helsinki-Version oder höher
- Für den ServiceNow-Mandanten muss das [SSO-Plug-In für mehrere Anbieter](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) aktiviert sein. Dazu kann [eine Serviceanfrage übermittelt werden](https://hi.service-now.com).
- Aktivieren Sie für die automatische Konfiguration das Multi-Provider-Plug-In für ServiceNow.

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von ServiceNow aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-servicenow-from-the-gallery"></a>Hinzufügen von ServiceNow aus dem Katalog
Zum Konfigurieren der Integration von ServiceNow in Azure AD müssen Sie ServiceNow aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um ServiceNow aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld **ServiceNow** ein, wählen Sie im Ergebnisbereich **ServiceNow** aus, und klicken Sie anschließend auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![ServiceNow in der Ergebnisliste](./media/servicenow-tutorial/tutorial_servicenow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit ServiceNow basierend auf einer Testbenutzerin mit dem Namen „Britta Simon“.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in ServiceNow als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in ServiceNow muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in ServiceNow den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit ServiceNow zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens in Azure AD für ServiceNow](#configure-azure-ad-single-sign-on-for-servicenow)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens in Azure AD für ServiceNow Express](#configure-azure-ad-single-sign-on-for-servicenow-express)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Erstellen eines ServiceNow-Testbenutzers](#create-a-servicenow-test-user)**, um eine Entsprechung von Britta Simon in ServiceNow zu haben, die mit der Azure AD-Darstellung von Britta Simon verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on-for-servicenow"></a>Konfigurieren des einmaligen Anmeldens von Azure AD für ServiceNow

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer ServiceNow-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit ServiceNow zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **ServiceNow** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/servicenow-tutorial/tutorial_servicenow_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für ServiceNow** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für ServiceNow](./media/servicenow-tutorial/tutorial_servicenow_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<instance-name>.service-now.com/navpage.do`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<instance-name>.service-now.com`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner ersetzen. Dies wird an späterer Stelle im Tutorial erläutert.

1. Führen Sie im Abschnitt **SAML-Signaturzertifikat** die folgenden Schritte aus: 

    ![Downloadlink für das Zertifikat](./media/servicenow-tutorial/tutorial_servicenow_certificate.png)

    a. Klicken Sie auf die Schaltfläche „Kopieren“, um die **Verbundmetadaten-URL der App**  zu kopieren und in Editor einzufügen. Die Verbundmetadaten-URL der App wird später in diesem Tutorial verwendet.

    b. Klicken Sie auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/servicenow-tutorial/tutorial_general_400.png)

1. Melden Sie sich bei Ihrer ServiceNow-Anwendung als Administrator an.

1. Aktivieren Sie das Plug-In **Integration – Multiple Provider Single Sign-On Installer** (Integration – SSO-Installationsprogramm für mehrere Anbieter), indem Sie die nächsten Schritte ausführen:

    a. Suchen Sie im Navigationsbereich auf der linken Seite den Abschnitt **Systemdefinition** auf der Suchleiste, und klicken Sie auf **Plug-Ins**.

    ![Plug-In aktivieren](./media/servicenow-tutorial/tutorial_servicenow_03.png "Plug-In aktivieren")

     b. Suchen Sie nach **Integration – Multiple Provider Single Sign-On Installer** (Integration – SSO-Installationsprogramm für mehrere Anbieter).

     ![Plug-In aktivieren](./media/servicenow-tutorial/tutorial_servicenow_04.png "Plug-In aktivieren")

    c. Wählen Sie das Plug-In aus. Klicken Sie mit der rechten Maustaste, und wählen Sie **Activate/Upgrade** (Aktivieren/Upgrade) aus.

    d. Klicken Sie auf die Schaltfläche **Aktivieren**.

1. Es gibt zwei Möglichkeiten für die Konfiguration von **ServiceNow**: automatisch und manuell.

1. Führen Sie für die automatische Konfiguration von **ServiceNow** die unten angegebenen Schritte aus.

    a. Wechseln Sie zurück zur **ServiceNow**-SSO-Seite im Azure-Portal.

    b. Für ServiceNow gibt es einen Ein-Klick-Konfigurationsdienst, d. h., Azure AD konfiguriert ServiceNow für die SAML-Authentifizierung automatisch. Um diesen Dienst zu aktivieren, wechseln Sie in den Abschnitt **ServiceNow-Konfiguration**, und klicken Sie auf **ServiceNow konfigurieren**, um das Fenster „Anmeldung konfigurieren“ zu öffnen.

    ![Configure single sign-on](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

    c. Geben Sie im Formular **Anmeldung konfigurieren** Ihren ServiceNow-Instanznamen, Administratorbenutzernamen und das Administratorkennwort ein, und klicken Sie auf **Jetzt konfigurieren**. Beachten Sie, dass dem angegebenen Administratorbenutzernamen in ServiceNow die Rolle **security_admin** zugewiesen sein muss, damit dies funktioniert. Wenn Sie hingegen ServiceNow für die Verwendung von Azure AD als SAML-Identitätsanbieter manuell konfigurieren möchten, klicken Sie auf **Einmaliges Anmelden manuell konfigurieren**, und kopieren Sie aus dem Abschnitt „Kurzübersicht“ **die Abmelde-URL, die SAML-Entitäts-ID und die SAML-Dienst-URL für einmaliges Anmelden**.

    ![App-URL konfigurieren](./media/servicenow-tutorial/configure.png "App-URL konfigurieren")

    d. Melden Sie sich bei Ihrer ServiceNow-Anwendung als Administrator an.

    e. Bei der automatischen Konfiguration werden alle erforderlichen Einstellungen aufseiten von **ServiceNow** konfiguriert, aber das **X.509-Zertifikat** ist standardmäßig nicht aktiviert. Sie müssen es Ihrem Identitätsanbieter in ServiceNow manuell zuordnen. Führen Sie hierzu die unten angegebenen Schritte aus:
    
    * Klicken Sie im Navigationsbereich auf der linken Seite unter **Multi-Provider SSO** (Multi-Provider-SSO) auf **Identity Providers** (Identitätsanbieter).

      ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/tutorial_servicenow_07.png "Einmaliges Anmelden konfigurieren")

    * Klicken Sie auf den automatisch generierten Identitätsanbieter.

      ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/tutorial_servicenow_08.png "Einmaliges Anmelden konfigurieren")

    * Scrollen Sie nach unten zum Abschnitt **X.509-Zertifikat**. Klicken Sie auf **Bearbeiten**.

      ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/tutorial_servicenow_09.png "Einmaliges Anmelden konfigurieren")
    
    * Wählen Sie das Zertifikat aus, und klicken Sie auf den Pfeil nach rechts, um das Zertifikat hinzuzufügen.

      ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/tutorial_servicenow_11.png "Einmaliges Anmelden konfigurieren")

    * Klicken Sie auf **Speichern**.

    * Klicken Sie oben rechts auf der Seite auf **Aktivieren**.

1. Führen Sie für die manuelle Konfiguration von **ServiceNow** die unten angegebenen Schritte aus.

1. Melden Sie sich bei Ihrer ServiceNow-Anwendung als Administrator an.

1. Suchen Sie im Navigationsbereich auf der linken Seite den Abschnitt **Multi-Provider SSO** auf der Suchleiste, und klicken Sie auf **Eigenschaften**.

    ![App-URL konfigurieren](./media/servicenow-tutorial/tutorial_servicenow_06.png "App-URL konfigurieren")

1. Führen Sie im Dialogfeld **Multiple Provider SSO Properties** die folgenden Schritte aus:

    ![App-URL konfigurieren](./media/servicenow-tutorial/ic7694981.png "App-URL konfigurieren")

    a. Bei **Enable multiple provider SSO** wählen Sie **Yes** aus.

    b. Wählen Sie bei **Enable Auto Importing of users from all identity providers into the user table** (Automatisches Importieren von Benutzern aller Identitätsanbieter in die Benutzertabelle) **Yes** (Ja) aus.

    c. Wählen Sie bei **Enable debug logging for the multiple provider SSO integration** (Debugprotokollierung für die Multi-Provider SSO-Integration) **Yes** (Ja) aus.

    d. Im Textfeld **The field on the user table that...** geben Sie **user_name** ein.

    e. Klicken Sie auf **Speichern**.

1. Suchen Sie im Navigationsbereich auf der linken Seite den Abschnitt **Multi-Provider SSO** auf der Suchleiste, und klicken Sie auf **x509 Certificates** (x509-Zertifikate).

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/tutorial_servicenow_05.png "Einmaliges Anmelden konfigurieren")

1. Klicken Sie im Dialogfeld **X.509 Certificates** auf **New**.

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/ic7694974.png "Einmaliges Anmelden konfigurieren")

1. Führen Sie im Dialogfeld **X.509 Certificates** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/ic7694975.png "Einmaliges Anmelden konfigurieren")

    a. Geben Sie im Textfeld **Name** einen Namen für die Konfiguration ein (z.B. **TestSAML2.0**).

    b. Wählen Sie **Aktiv**.

    c. Wählen Sie unter **Format** das Format **PEM** aus.

    d. Wählen Sie unter **Type** den Typ **Trust Store Cert** aus.

    e. Öffnen Sie das aus Azure heruntergeladene Base64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **PEM Certificate** (PEM-Zertifikat) ein.

     f. Klicken Sie auf **Submit**.

1. Klicken Sie im Navigationsbereich auf der linken Seite auf **Identity Providers**.

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/tutorial_servicenow_07.png "Einmaliges Anmelden konfigurieren")

1. Klicken Sie im Dialogfeld **Identitätsanbieter** auf **Neu**.

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/ic7694977.png "Einmaliges Anmelden konfigurieren")

1. Klicken Sie im Dialogfeld **Identitätsanbieter** auf **SAML2 Update1?**.

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/ic7694978.png "Einmaliges Anmelden konfigurieren")

1. Führen Sie im Dialogfeld „SAML2 Update1 Properties“ folgende Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/idp.png "Einmaliges Anmelden konfigurieren")

    a. Wählen Sie im Dialogfeld **Import Identity Provider Metadata** (Identitätsanbieter-Metadaten importieren) die Option **URL** aus.

    b. Geben Sie die **Verbundmetadaten-URL der App** ein, die Sie aus dem Azure-Portal kopiert haben.

    c. Klicken Sie auf **Importieren**.

1. Die IdP-Metadaten-URL wird gelesen, und alle Felder werden aufgefüllt.

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/ic7694982.png "Einmaliges Anmelden konfigurieren")

    a. Geben Sie im Textfeld **Name** einen Namen für die Konfiguration ein (z.B. **SAML 2.0**).
    
    b. Kopieren Sie den Wert für die **ServiceNow-Homepage**, und fügen Sie ihn im Azure-Portal im Abschnitt **Domäne und URLs für ServiceNow** im Textfeld **Anmelde-URL** ein.

    > [!NOTE]
    > Die URL zur Homepage der ServiceNow-Instanz ist eine Verkettung Ihrer **ServiceNow-Mandanten-URL** und **/navpage.do** (beispielsweise `https://fabrikam.service-now.com/navpage.do`).

    c. Kopieren Sie den Wert für **Entitäts-ID/Aussteller**, und fügen Sie ihn im Azure-Portal im Abschnitt **Domäne und URLs für ServiceNow** im Textfeld **Bezeichner** ein.

    d. Klicken Sie auf **Erweitert**. Geben Sie im Textfeld **User Field** (Benutzerfeld) den Wert **email** oder **user_name** ein, je nachdem, welches Feld für die eindeutige Identifizierung von Benutzern in Ihrer ServiceNow-Bereitstellung verwendet wird.

    > [!NOTE]
    > Sie können Azure AD so konfigurieren, dass entweder die Azure AD-Benutzer-ID (Benutzerprinzipalname) oder die E-Mail-Adresse als eindeutiger Bezeichner im SAML-Token ausgegeben wird. Wechseln Sie dazu im Azure-Portal zum Abschnitt **ServiceNow > Attribute > Einmaliges Anmelden**, und weisen Sie dem **nameidentifier**-Attribut das gewünschte Feld zu. Der gespeicherte Wert für das ausgewählte Attribut in Azure AD (z.B. Benutzerprinzipalname) muss dem in ServiceNow gespeicherten Wert für das eingegebene Feld (z.B. user_name) entsprechen.

    e. Unter **x509 Certificate** (x509-Zertifikat) wird das im vorherigen Schritt erstellte Zertifikat aufgelistet.

    > [!NOTE]
    > In ServiceNow ist die IdP-Aktivierung nicht zulässig, ohne dass auf die Schaltfläche zum Testen der Verbindung geklickt wird. Führen Sie die hier angegebenen Schritte aus, um diesen Vorgang außer Kraft zu setzen.

1. Klicken Sie auf das Menüsymbol Ihres neuen Identitätsanbieters, den Sie im Rahmen der Konfiguration erstellt haben, und wählen Sie in der Liste den Eintrag **copy sys_id**.

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/ic7694992.png "Einmaliges Anmelden konfigurieren")

1. Suchen Sie im Suchfeld oben links nach **sys_properties.list**, und drücken Sie die EINGABETASTE.

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/ic7694993.png "Einmaliges Anmelden konfigurieren")

1. Klicken Sie auf **New**.

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/ic7694994.png "Einmaliges Anmelden konfigurieren")

1. Führen Sie im Abschnitt **Systemeigenschaft** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/ic7694995.png "Einmaliges Anmelden konfigurieren")

    a. Geben Sie den Wert `glide.authenticate.sso.redirect.idp` in das Textfeld für den Namen ein.

    b. Fügen Sie im Textfeld **Wert** den Wert „copy sys_id“ ein, den Sie in den vorherigen Schritten kopiert haben.

    c. Wählen Sie **Privat**.

    d. Klicken Sie auf **Submit**.

1. Klicken Sie auf **New**.

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/ic7694994.png "Einmaliges Anmelden konfigurieren")

1. Führen Sie im Abschnitt **Systemeigenschaft** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/ic7694996.png "Einmaliges Anmelden konfigurieren")

    a. Geben Sie den Wert `glide.authenticate.multisso.test.connection.mandatory` in das Textfeld für den Namen ein.

    b. Geben Sie im Textfeld **Wert** den Text **false** ein.

    c. Klicken Sie auf **Submit**.

1. Nach Ausführung des obigen Schritts können Sie jetzt Ihren neuen Identitätsanbieter aktivieren, und SSO sollte bei Ihnen funktionieren.

> [!NOTE]
> Beachten Sie außerdem, dass Sie Ihre neue IdP-Konfiguration in einem neuen Inkognitofenster testen müssen.

### <a name="configure-azure-ad-single-sign-on-for-servicenow-express"></a>Konfigurieren des einmaligen Anmeldens von Azure AD für ServiceNow Express

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **ServiceNow** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Configure single sign-on](./media/servicenow-tutorial/tutorial_servicenow_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für ServiceNow** die folgenden Schritte aus:

    ![Configure single sign-on](./media/servicenow-tutorial/tutorial_servicenow_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** den Wert im folgenden Format ein: `https://<instance-name>.service-now.com/navpage.do`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [ServiceNow-Clientsupportteam](https://www.servicenow.com/support/contact-support.html), um diese Werte zu erhalten.

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Configure single sign-on](./media/servicenow-tutorial/tutorial_servicenow_certificates.png)

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/servicenow-tutorial/tutorial_general_400.png)

1. Für ServiceNow gibt es einen Ein-Klick-Konfigurationsdienst, d. h., Azure AD konfiguriert ServiceNow für die SAML-Authentifizierung automatisch. Um diesen Dienst zu aktivieren, wechseln Sie in den Abschnitt **ServiceNow-Konfiguration**, und klicken Sie auf **ServiceNow konfigurieren**, um das Fenster „Anmeldung konfigurieren“ zu öffnen.

    ![Configure single sign-on](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

1. Geben Sie im Formular **Anmeldung konfigurieren** Ihren ServiceNow-Instanznamen, Administratorbenutzernamen und das Administratorkennwort ein, und klicken Sie auf **Jetzt konfigurieren**. Beachten Sie, dass dem angegebenen Administratorbenutzernamen in ServiceNow die Rolle **security_admin** zugewiesen sein muss, damit dies funktioniert. Wenn Sie hingegen ServiceNow für die Verwendung von Azure AD als SAML-Identitätsanbieter manuell konfigurieren möchten, klicken Sie auf **Einmaliges Anmelden manuell konfigurieren**, und kopieren Sie aus dem Abschnitt „Kurzübersicht“ **die Abmelde-URL, die SAML-Entitäts-ID und die SAML-Dienst-URL für einmaliges Anmelden**.

    ![App-URL konfigurieren](./media/servicenow-tutorial/configure.png "App-URL konfigurieren")

1. Melden Sie sich bei Ihrer ServiceNow Express-Anwendung als Administrator an.

1. Klicken Sie links im Navigationsbereich auf **Einmaliges Anmelden**.

    ![App-URL konfigurieren](./media/servicenow-tutorial/ic7694980ex.png "App-URL konfigurieren")

1. Klicken Sie im Dialogfeld **Einmaliges Anmelden** oben rechts auf das Konfigurationssymbol, und legen Sie die folgenden Eigenschaften fest:

    ![App-URL konfigurieren](./media/servicenow-tutorial/ic7694981ex.png "App-URL konfigurieren")

    a. Schieben Sie den Umschalter der Option **Enable multiple provider SSO** (SSO für mehrere Anbieter aktivieren) nach rechts.
    
    b. Schieben Sie den Umschalter für **Enable debug logging for the multiple provider SSO integration** (Debugprotokollierung für die SSO-Integration für mehrere Anbieter aktivieren) nach rechts.
    
    c. Im Textfeld **The field on the user table that...** geben Sie **user_name** ein.

1. Klicken Sie im Dialogfeld **Einmaliges Anmelden** auf **Neues Zertifikat hinzufügen**.

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/ic7694973ex.png "Einmaliges Anmelden konfigurieren")

1. Führen Sie im Dialogfeld **X.509 Certificates** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/ic7694975.png "Einmaliges Anmelden konfigurieren")

    a. Geben Sie im Textfeld **Name** einen Namen für die Konfiguration ein (z.B. **TestSAML2.0**).

    b. Wählen Sie **Aktiv**.

    c. Wählen Sie unter **Format** das Format **PEM** aus.

    d. Wählen Sie unter **Type** den Typ **Trust Store Cert** aus.

    e. Öffnen Sie das vom Azure-Portal heruntergeladene Base64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **PEM Certificate** (PEM-Zertifikat) ein.

    f. Klicken Sie auf **Aktualisieren**.

1. Klicken Sie im Dialogfeld **Einmaliges Anmelden** auf **Add New IdP** (Neuen IdP hinzufügen).

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/ic7694976ex.png "Einmaliges Anmelden konfigurieren")

1. Führen Sie im Dialogfeld **Add New Identity Provider** (Neuen Identitätsanbieter hinzufügen) unter **Configure Identity Provider** (Identitätsanbieter konfigurieren) die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/ic7694982ex.png "Einmaliges Anmelden konfigurieren")

    a. Geben Sie im Textfeld **Name** einen Namen für die Konfiguration ein (z.B. **SAML 2.0**).

    b. Fügen Sie in das Feld **Identity Provider URL** den Wert für die **Identitätsanbieter-ID** ein, den Sie aus dem Azure-Portal kopiert haben.
    
    c. Fügen Sie in das Feld **Identity Provider's AuthnRequest** den Wert für die **Authentifizierungsanforderungs-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Fügen Sie in das Feld **Identity Provider's SingleLogoutRequest** den Wert für die **Dienst-URL für einmalige Abmeldung** ein, den Sie aus dem Azure-Portal kopiert haben.

    e. Wählen Sie als **Identity Provider Certificate** (Identitätsanbieterzertifikat) das Zertifikat aus, das Sie im vorherigen Schritt erstellt haben.

1. Klicken Sie auf **Advanced Settings** (Erweiterte Einstellungen), und führen Sie unter **Additional Identity Provider Properties** (Weitere Identitätsanbietereigenschaften) die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/ic7694983ex.png "Einmaliges Anmelden konfigurieren")

    a. Geben Sie im Textfeld **Protocol Binding for the IDP's SingleLogoutRequest** die Zeichenfolge **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect** ein.

    b. Geben Sie im Textfeld **NameID Policy** die Zeichenfolge **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified** ein.

    c. Geben Sie unter **AuthnContextClassRef Method** die URL `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` ein.

    d. Deaktivieren Sie **Create an AuthnContextClass**.

1. Führen Sie unter **Additional Service Provider Properties** (Weitere Service Provider-Eigenschaften) die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/servicenow-tutorial/ic7694984ex.png "Einmaliges Anmelden konfigurieren")

    a. Geben Sie im Textfeld **ServiceNow Homepage** die URL zur Homepage Ihrer ServiceNow-Instanz ein.

    > [!NOTE]
    > Die URL zur Homepage der ServiceNow-Instanz ist eine Verkettung Ihrer **ServiceNow-Mandanten-URL** und **/navpage.do** (beispielsweise `https://fabrikam.service-now.com/navpage.do`).

    b. Geben Sie im Textfeld **Entity ID / Issuer** die URL Ihres ServiceNow-Mandaten ein.

    c. Geben Sie im Textfeld **Audience URI** (Benutzergruppen-URI) die URL Ihres ServiceNow-Mandanten ein.

    d. Im Textfeld **Clock Skew** geben Sie **60** ein.

    e. Geben Sie im Textfeld **User Field** (Benutzerfeld) den Wert **email** oder **user_name** ein, je nachdem, welches Feld für die eindeutige Identifizierung von Benutzern in Ihrer ServiceNow-Bereitstellung verwendet wird.

    > [!NOTE]
    > Sie können Azure AD so konfigurieren, dass entweder die Azure AD-Benutzer-ID (Benutzerprinzipalname) oder die E-Mail-Adresse als eindeutiger Bezeichner im SAML-Token ausgegeben wird. Wechseln Sie dazu im Azure-Portal zum Abschnitt **ServiceNow > Attribute > Einmaliges Anmelden**, und weisen Sie dem **nameidentifier**-Attribut das gewünschte Feld zu. Der gespeicherte Wert für das ausgewählte Attribut in Azure AD (z.B. Benutzerprinzipalname) muss dem in ServiceNow gespeicherten Wert für das eingegebene Feld (z.B. user_name) entsprechen.

    f. Klicken Sie auf **Speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/servicenow-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/servicenow-tutorial/create_aaduser_02.png)

1. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/servicenow-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/servicenow-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="create-a-servicenow-test-user"></a>Erstellen eines ServiceNow-Testbenutzers

In diesem Abschnitt wird in ServiceNow eine Benutzerin namens Britta Simon erstellt. ServiceNow unterstützt die automatische Benutzerbereitstellung, die standardmäßig aktiviert ist. Weitere Details zur Konfiguration der automatischen Benutzerbereitstellung finden Sie [hier](servicenow-provisioning-tutorial.md).

> [!NOTE]
> Wenn Sie einen Benutzer manuell erstellen möchten, wenden Sie sich an das [ServiceNow-Supportteam](https://www.servicenow.com/support/contact-support.html).

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf ServiceNow gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon ServiceNow zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste den Eintrag **ServiceNow** aus.

    ![ServiceNow-Link in der Anwendungsliste](./media/servicenow-tutorial/tutorial_servicenow_app.png)  

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „ServiceNow“ klicken, sollten Sie automatisch bei Ihrer ServiceNow-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Konfigurieren der Benutzerbereitstellung](servicenow-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/servicenow-tutorial/tutorial_general_01.png
[2]: ./media/servicenow-tutorial/tutorial_general_02.png
[3]: ./media/servicenow-tutorial/tutorial_general_03.png
[4]: ./media/servicenow-tutorial/tutorial_general_04.png

[100]: ./media/servicenow-tutorial/tutorial_general_100.png

[200]: ./media/servicenow-tutorial/tutorial_general_200.png
[201]: ./media/servicenow-tutorial/tutorial_general_201.png
[202]: ./media/servicenow-tutorial/tutorial_general_202.png
[203]: ./media/servicenow-tutorial/tutorial_general_203.png
