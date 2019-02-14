---
title: 'Tutorial: Azure Active Directory-Integration von Tableau Server | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und Tableau Server konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6f0a8f6ce325dd087aec5e055e09e20e7de62a40
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56204830"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Tutorial: Azure Active Directory-Integration von Tableau Server

In diesem Tutorial erfahren Sie, wie Sie Tableau Server in Azure Active Directory (Azure AD) integrieren.

Die Integration von Tableau Server in Azure AD bietet die folgenden Vorteile:

- In Azure AD lässt sich steuern, wer Zugriff auf Tableau Server hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Tableau Server anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Tableau Server konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Tableau Server-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Tableau Server aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-tableau-server-from-the-gallery"></a>Hinzufügen von Tableau Server aus dem Katalog

Zum Konfigurieren der Integration von Tableau Server in Azure AD müssen Sie Tableau Server aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Tableau Server aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Tableau Server** ein, wählen Sie im Ergebnisfenster **Tableau Server** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Tableau Server in der Ergebnisliste](./media/tableauserver-tutorial/tutorial-tableauserver-addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Tableau Server basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Tableau Server als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Tableau Server muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Tableau Server müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Tableau Server](#configure-tableau-server-single-sign-on)**, um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Erstellen eines Tableau Server-Testbenutzers](#create-tableau-server-test-user)**, um eine Entsprechung von Britta Simon in Zscaler Three zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
5. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Tableau Server-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Tableau Server die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Tableau Server** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![Configure single sign-on](common/tutorial-general-301.png)

3. Die Tableau Server-Anwendung erwartet einen benutzerdefinierten Anspruch vom Typ **Benutzername**, der wie folgt definiert werden muss. Dies wird als Benutzer-ID anstelle des Anspruchs „Eindeutige Benutzer-ID“ verwendet. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute und Ansprüche** auf der Anwendungsintegrationsseite verwalten. Klicken Sie auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Benutzerattribute und Ansprüche** zu öffnen.

    ![image](./media/tableauserver-tutorial/tutorial-tableauserver-attribute.png)

4. Konfigurieren Sie im Dialogfeld **Benutzerattribute und Ansprüche** im Abschnitt **Benutzeransprüche** das SAML-Tokenattribut wie in der obigen Abbildung gezeigt, und führen Sie die folgenden Schritte aus:
    
    | Attributname | Attributwert | Namespace |
    | ---------------| --------------- | ----------- |   
    | username | user.userprincipalname | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |

    a. Klicken Sie auf **Neuen Anspruch hinzufügen**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    ![image](./media/tableauserver-tutorial/tutorial-tableauserver-add-attribute.png)

    ![image](./media/tableauserver-tutorial/tutorial-tableauserver-manage-attribute.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Geben Sie den **Namespace**-Wert ein.

    d. Wählen Sie „Source“ als **Attribut** aus.

    e. Geben Sie in der Liste **Quellattribut** den für diese Zeile angezeigten Attributwert ein.

    f. Klicken Sie auf **Speichern**.

5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Configure single sign-on](common/editconfigure.png)

6. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://azure.<domain name>.link`.
    
    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://azure.<domain name>.link`

    c. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://azure.<domain name>.link/wg/saml/SSO/index.html`

    ![image](./media/tableauserver-tutorial/tutorial-tableauserver-url.png)
     
    > [!NOTE] 
    > Die vorangehenden Werte sind keine echten Werte. Aktualisieren Sie die Werte mit der aktuellen URL und dem Bezeichner von der Seite „Tableau Server Configuration“ (Tableau Server-Konfiguration) (wird später im Tutorial erläutert).

7. Klicken Sie auf der Seite **SAML-Signaturzertifikat** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um die **Verbundmetadaten-XML-Datei** herunterzuladen. Speichern Sie das Zertifikat anschließend auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/tableauserver-tutorial/tutorial-tableauserver-certificate.png)

### <a name="configure-tableau-server-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für Tableau Server 

1. Zum Konfigurieren des einmaligen Anmeldens für Ihre Anwendung müssen Sie sich als Administrator bei Ihrem Tableau Server-Mandanten anmelden.

2. Wählen Sie auf der Registerkarte **CONFIGURATION (KONFIGURATION)** **User Identity & Access (Benutzeridentität und Zugriff)** aus, und wählen Sie dann die Registerkarte **Authentication Method (Authentifizierungsmethode)** aus.

    ![Configure single sign-on](./media/tableauserver-tutorial/tutorial-tableauserver-auth.png)

3. Führen Sie auf der Seite **CONFIGURATION (KONFIGURATION)** die folgenden Schritte aus:

    ![Configure single sign-on](./media/tableauserver-tutorial/tutorial-tableauserver-config.png)

    a. Wählen Sie als **Authentication Method (Authentifizierungsmethode)** die Option „SAML“ aus.
    
    b. Aktivieren Sie das Kontrollkästchen **Enable SAML Authentication for the server (SAML-Authentifizierung für den Server aktivieren)**.

    c. „Tableau Server return URL“: Die URL, auf die Tableau Server-Benutzer zugreifen, z. B. http://tableau_server. Ein Verwenden von http://localhost ist nicht zu empfehlen. Die Verwendung einer URL mit einem nachstehenden Schrägstrich (z. B. http://tableau_server/)) wird nicht unterstützt. Kopieren Sie den Wert von **Tableau Server return URL**, und fügen Sie ihn im Abschnitt **Tableau Server-Domäne und -URLs** in das Azure AD-Textfeld **Anmelde-URL** ein.

    d. „SAML entity ID“: Die Entitäts-ID zur eindeutigen Identifizierung Ihrer Tableau Server-Installation durch den IdP. Sie können in dieses Feld erneut Ihre Tableau Server-URL eingeben, es muss jedoch nicht die Tableau Server-URL verwendet werden. Kopieren Sie die **SAML-Entitäts-ID**, und fügen Sie sie im Abschnitt **Tableau Server-Domäne und -URLs** in das Azure AD-Textfeld **Bezeichner** ein.

    e. Klicken Sie auf **Download XML Metadata File (XML-Metadatendatei herunterladen)**, und öffnen Sie die Datei im Text-Editor. Suchen Sie nach „Assertion Consumer Service URL“ mit HTTP Post und Index 0, und kopieren Sie die URL. Fügen Sie sie nun im Abschnitt **Tableau Server-Domäne und -URLs** in das Azure AD-Textfeld **Antwort-URL** ein.

    f. Suchen Sie nach der Datei mit Ihren Verbundmetadaten, die Sie aus dem Azure-Portal heruntergeladen haben, und laden Sie sie in die **SAML Idp metadata file** hoch.

    g. Geben Sie die Namen für die Attribute ein, die der IdP verwendet, um Benutzernamen, Anzeigenamen und E-Mail-Adressen aufzunehmen.

    h. Klicken Sie unten auf der Seite auf **Speichern**.

    >[!NOTE] 
    >Kunden müssen ein Zertifikat in die Tableau Server SAML-SSO-Konfiguration hochladen, das im SSO-Fluss ignoriert wird.
    >Wenn Sie Hilfe bei der Konfiguration von SAML für Tableau Server benötigen, finden Sie weitere Informationen im Artikel [Konfigurieren von SAML](https://onlinehelp.tableau.com/v2018.2/server/en-us/saml_config_steps_tsm_ui.htm).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Azure AD-Benutzer erstellen][100]

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Erstellen eines Azure AD-Testbenutzers](common/create-aaduser-01.png) 

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Erstellen eines Azure AD-Testbenutzers](common/create-aaduser-02.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** den Namen **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
  
### <a name="create-tableau-server-test-user"></a>Erstellen eines Tableau Server-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Tableau Server. Sie müssen alle Benutzer in Tableau Server bereitstellen. 

Der username-Wert des Benutzers muss dem Wert entsprechen, den Sie im benutzerdefinierten Attribut **username** in Azure AD konfiguriert haben. Bei ordnungsgemäßer Zuordnung sollte die Integration funktionieren: Konfigurieren des einmaligen Anmeldens in Azure AD.

>[!NOTE]
>Wenn Sie einen Benutzer manuell erstellen müssen, wenden Sie sich an den Tableau Server-Administrator in Ihrer Organisation.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Tableau Server gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Tableau Server**.

    ![Configure single sign-on](./media/tableauserver-tutorial/tutorial-tableauserver-app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Tableau Server“ klicken, sollten Sie automatisch bei Ihrer Tableau Server-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial-general-01.png
[2]: common/tutorial-general-02.png
[3]: common/tutorial-general-03.png
[4]: common/tutorial-general-04.png

[100]: common/tutorial-general-100.png

[201]: common/tutorial-general-201.png
[202]: common/tutorial-general-202.png
[203]: common/tutorial-general-203.png
