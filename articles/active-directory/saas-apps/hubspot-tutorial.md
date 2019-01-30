---
title: 'Tutorial: Azure Active Directory-Integration mit HubSpot | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und HubSpot konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 57343ccd-53ea-4e62-9e54-dee2a9562ed5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2018
ms.author: jeedes
ms.openlocfilehash: 2806288378e5fa080164155b97a47a7046ecbba1
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54814901"
---
# <a name="tutorial-azure-active-directory-integration-with-hubspot"></a>Tutorial: Azure Active Directory-Integration in HubSpot

In diesem Tutorial erfahren Sie, wie Sie HubSpot in Azure Active Directory (Azure AD) integrieren.

Die Integration von HubSpot in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf HubSpot hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei HubSpot anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit HubSpot konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein HubSpot-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von HubSpot aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-hubspot-from-the-gallery"></a>Hinzufügen von HubSpot aus dem Katalog

Zum Konfigurieren der Integration von HubSpot in Azure AD müssen Sie HubSpot über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um HubSpot aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

4. Geben Sie im Suchfeld den Suchbegriff **HubSpot** ein. Wählen Sie im Ergebnisbereich **HubSpot** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/hubspot-tutorial/tutorial_hubspot_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei HubSpot mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in HubSpot als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in HubSpot muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei HubSpot müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. **[Erstellen eines HubSpot-Testbenutzers](#creating-a-hubspot-saml-test-user)**, um eine Entsprechung von Britta Simon in HubSpot zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer HubSpot-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in HubSpot die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **HubSpot** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

2. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![Configure single sign-on](./media/hubspot-tutorial/tutorial_general_301.png)

3. Wenn Sie aus einem anderen Modus in den **SAML**-Modus wechseln müssen, können Sie am oberen Bildschirmrand auf **Modus für einmaliges Anmelden ändern** klicken.

    ![Configure single sign-on](./media/hubspot-tutorial/tutorial_general_300.png)

4. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Configure single sign-on](./media/hubspot-tutorial/tutorial_general_302.png)

5. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für HubSpot](./media/hubspot-tutorial/tutorial_hubspot_url.png)

    a. Geben Sie im Textfeld **Bezeichner** die URL nach folgendem Muster ein: `https://api.hubspot.com/login-api/v1/saml/login?portalId=<CUSTOMER ID>`.

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://api.hubspot.com/login-api/v1/saml/acs?portalId=<CUSTOMER ID>`.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner und die tatsächliche Antwort-URL. Dies wird später in diesem Tutorial beschrieben. Wenden Sie sich an das [Kundensupportteam von HubSpot](https://help.hubspot.com/), um diese Werte zu erhalten.

    c. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für HubSpot](./media/hubspot-tutorial/tutorial_hubspot_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** diese URL ein: `https://app.hubspot.com/login`

6. Klicken Sie auf der Seite **SAML-Signaturzertifikat** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das **Zertifikat (Base64)** herunterzuladen. Speichern Sie die Zertifikatsdatei anschließend auf Ihrem Computer.

    ![Configure single sign-on](./media/hubspot-tutorial/tutorial_hubspot_certificate.png)

7. Klicken Sie im Abschnitt **HubSpot einrichten** auf die Kopierschaltfläche, um die Werte für **Anmelde-URL** und **Azure AD-Bezeichner** zu kopieren.

    ![Configure single sign-on](./media/hubspot-tutorial/tutorial_hubspot_configure.png)

8. Öffnen Sie eine neue Registerkarte in Ihrem Browser, und melden Sie sich an Ihrem HubSpot-Administratorkonto an.

9. Klicken Sie in der oberen rechten Ecke der Seite auf das Symbol **Einstellungen**.

    ![Configure single sign-on](./media/hubspot-tutorial/config1.png)

10. Klicken Sie auf **Account Defaults** (Kontostandardwerte).

    ![Configure single sign-on](./media/hubspot-tutorial/config2.png)

11. Scrollen Sie nach unten zum Abschnitt **Security** (Sicherheit), und klicken Sie auf **Set up** (Einrichten).

    ![Configure single sign-on](./media/hubspot-tutorial/config3.png)

12. Führen Sie im Abschnitt **Einmaliges Anmelden einrichten** die folgenden Schritte aus:

    ![Configure single sign-on](./media/hubspot-tutorial/config4.png)

    a. Klicken Sie auf die Schaltfläche **Kopieren**, um den Wert von **Audience URl (Service Provider Entity ID)** (Zielgruppen-URl (Dienstanbieterentitäts-ID)) zu kopieren, und fügen Sie ihn im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** in das Textfeld **Bezeichner** ein.

    b. Klicken Sie auf die Schaltfläche **Kopieren**, um den Wert von **Sign on URl, ACS, Recipient, or Redirect** (Anmelde-URl, ACS, Empfänger oder Umleiten) zu kopieren, und fügen Sie ihn im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** in das Textfeld **Antwort-URL** ein.

    c. Fügen Sie im Textfeld **Identity Provider Identifier or Issuer URL** (Identitätsanbieterbezeichner oder Aussteller-URL) den Wert von **Azure AD-Bezeichner** ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Fügen Sie den Wert der **Anmelde-URL**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **URL für einmaliges Anmelden des Identitätsanbieters** ein.

    e. Öffnen Sie die heruntergeladene  **Zertifikat (Base64)** -Datei im Editor. Kopieren Sie den Inhalt in die Zwischenablage, und fügen Sie ihn in das Feld  **X.509-Zertifikat** ein.

    f. Klicken Sie auf **Überprüfen**.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Azure AD-Benutzer erstellen][100]

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Erstellen eines Azure AD-Testbenutzers](./media/hubspot-tutorial/create_aaduser_01.png) 

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Erstellen eines Azure AD-Testbenutzers](./media/hubspot-tutorial/create_aaduser_02.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

### <a name="creating-a-hubspot-test-user"></a>Erstellen eines HubSpot-Testbenutzers

Damit sich Azure AD-Benutzer bei HubSpot anmelden können, müssen sie in HubSpot bereitgestellt werden.
Im Fall von HubSpot ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der **HubSpot** -Unternehmenswebsite als Administrator an.

2. Klicken Sie in der oberen rechten Ecke der Seite auf das Symbol **Einstellungen**.

    ![Configure single sign-on](./media/hubspot-tutorial/config1.png)

3. Klicken Sie auf **Users & Teams** (Benutzer und Teams).

    ![Configure single sign-on](./media/hubspot-tutorial/user1.png)

4. Klicken Sie auf **Benutzer erstellen**.

    ![Configure single sign-on](./media/hubspot-tutorial/user2.png)

5. Geben Sie die E-Mail-Adresse des Benutzers im Format **brittasimon@contoso.com** in das Textfeld **Add email address(es)** (E-Mail-Adresse(n) hinzufügen) ein, und klicken Sie auf **Weiter**.

    ![Configure single sign-on](./media/hubspot-tutorial/user3.png)

6. Gehen Sie im Abschnitt **Create users** (Benutzer erstellen) die Registerkarten einzeln durch, und wählen Sie die passenden Optionen und Berechtigungen für den Benutzer aus. Klicken Sie anschließend auf **Weiter**.

    ![Configure single sign-on](./media/hubspot-tutorial/user4.png)

7. Klicken Sie auf **Senden**, um die Einladung an den Benutzer zu senden.

    ![Configure single sign-on](./media/hubspot-tutorial/user5.png)

    > [!NOTE]
    > Benutzer werden aktiviert, nachdem sie die Einladung angenommen haben.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf HubSpot gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **HubSpot** aus.

    ![Configure single sign-on](./media/hubspot-tutorial/tutorial_hubspot_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „HubSpot“ klicken, sollten Sie automatisch auf die Anmeldeseite der HubSpot-Anwendung gelangen.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/hubspot-tutorial/tutorial_general_01.png
[2]: ./media/hubspot-tutorial/tutorial_general_02.png
[3]: ./media/hubspot-tutorial/tutorial_general_03.png
[4]: ./media/hubspot-tutorial/tutorial_general_04.png
[100]: ./media/hubspot-tutorial/tutorial_general_100.png
[200]: ./media/hubspot-tutorial/tutorial_general_200.png
[201]: ./media/hubspot-tutorial/tutorial_general_201.png
[202]: ./media/hubspot-tutorial/tutorial_general_202.png
[203]: ./media/hubspot-tutorial/tutorial_general_203.png