---
title: 'Tutorial: Azure Active Directory-Integration mit Fluxx Labs | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Fluxx Labs konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d8fac770-bb57-4e1f-b50b-9ffeae239d07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: 7bb15323a1f4ead02a24c4ee451de8890a635032
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39053041"
---
# <a name="tutorial-azure-active-directory-integration-with-fluxx-labs"></a>Tutorial: Azure Active Directory-Integration mit Fluxx Labs

In diesem Tutorial erfahren Sie, wie Sie Fluxx Labs in Azure Active Directory (Azure AD) integrieren.

Die Integration von Fluxx Labs in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Fluxx Labs hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Fluxx Labs anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Fluxx Labs konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Fluxx Labs-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Fluxx Labs aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-fluxx-labs-from-the-gallery"></a>Hinzufügen von Fluxx Labs aus dem Katalog
Zum Konfigurieren der Integration von Fluxx Labs in Azure AD müssen Sie Fluxx Labs aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Fluxx Labs aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Fluxx Labs** ein, wählen Sie im Ergebnisbereich **Fluxx Labs** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Fluxx-Labs in der Ergebnisliste](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Fluxx Labs mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Fluxx Labs als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Fluxx Labs muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Fluxx Labs den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Fluxx Labs müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Fluxx Labs-Testbenutzers](#create-a-fluxx-labs-test-user)**, um eine Entsprechung von Britta Simon in Fluxx Labs zu erhalten, die mit der Darstellung des Benutzers in Azure AD verknüpft ist
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden bei Ihrer Fluxx Labs-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Fluxx Labs die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Fluxx Labs** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Dialogfeld „Einmaliges Anmelden“](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Fluxx Labs** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Fluxx Labs](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_url.png)

    a. Geben Sie im Textfeld **Identifier** (Bezeichner) eine URL nach folgendem Muster ein:

    | Environment | URL-Muster|
    |-------------|------------|
    | Bereitstellung | `https://<subdomain>.fluxx.io` |
    | Vor der Bereitstellung | `https://<subdomain>.preprod.fluxxlabs.com`|
        
    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein:

    | Environment | URL-Muster|
    |-------------|------------|
    | Bereitstellung | `https://<subdomain>.fluxx.io/auth/saml/callback` |
    | Vor der Bereitstellung | `https://<subdomain>.preprod.fluxxlabs.com/auth/saml/callback`|

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Wenden Sie sich an das [Supportteam von Fluxx Labs](mailto:travis@fluxxlabs.com), um diese Werte zu erhalten.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/fluxxlabs-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Fluxx Labs-Konfiguration** auf **Fluxx Labs konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Fluxx Labs-Konfiguration](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_configure.png)

7. Melden Sie sich in einem anderen Webbrowserfenster bei der Fluxx Labs-Unternehmenswebsite als Administrator an.

8. Wählen Sie unten im Abschnitt **Settings** (Einstellungen) die Option **Admin** (Administrator).

    ![Fluxx Labs-Konfiguration](./media/fluxxlabs-tutorial/config1.png)

9. Klicken Sie im Verwaltungsbereich auf **Plug-Ins** > **Integrations** (Integrationen) und anschließend auf **SAML SSO-(Disabled)** (SAML-SSO (Deaktiviert)).

    ![Fluxx Labs-Konfiguration](./media/fluxxlabs-tutorial/config2.png)

10. Führen Sie im Abschnitt mit den Attributen die folgenden Schritte aus:

    ![Fluxx Labs-Konfiguration](./media/fluxxlabs-tutorial/config3.png)

    a. Aktivieren Sie das Kontrollkästchen **SAML SSO** (SAML-SSO).

    b. Geben Sie im Textfeld **Request Path** (Anforderungspfad) den Pfad **/auth/saml** ein.

    c. Geben Sie im Textfeld **Callback Path** (Rückrufpfad) den Pfad **/auth/saml/callback** ein.

    d. Geben Sie in das Textfeld **Assertion Consumer Service Url (Single Sign-On URL)** (Assertionsverbraucherdienst-URL (URL für einmaliges Anmelden)) den Wert der **Antwort-URL** ein, den Sie im Azure-Portal eingegeben haben.

    e. Geben Sie in das Textfeld **Audience (SP Entity ID)** (Zielgruppe (SP-Entitäts-ID)) den Wert unter **Bezeichner** ein, den Sie im Azure-Portal eingegeben haben.

    f. Fügen Sie den Wert der **URL für den SAML-SSO-Dienst**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Identity Provider SSO Target URL** (SSO-Ziel-URL des Identitätsanbieters) ein.

    g. Öffnen Sie das Base64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Zertifikat des Identitätsanbieters** ein.

    h. Geben Sie in das Textfeld **Name identifier Format** (Namensbezeichnerformat) den Wert `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress` ein.

    i. Klicken Sie auf **Speichern**.

    > [!NOTE]
    > Nachdem der Inhalt gespeichert wurde, wird das Feld aus Sicherheitsgründen leer angezeigt, der Wert wurde jedoch in der Konfiguration gespeichert.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/fluxxlabs-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/fluxxlabs-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/fluxxlabs-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/fluxxlabs-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
  
### <a name="create-a-fluxx-labs-test-user"></a>Erstellen eines Fluxx Labs-Testbenutzers

Damit sich Azure AD-Benutzer bei Fluxx Labs anmelden können, müssen sie in Fluxx Labs bereitgestellt werden. Im Fall von Fluxx Labs ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der Fluxx Labs-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf das unten angezeigte **Symbol**.

    ![Fluxx Labs-Konfiguration](./media/fluxxlabs-tutorial/config6.png)

3. Klicken Sie auf dem Dashboard auf das unten dargestellte Symbol, um die Karte **New PEOPLE** (Neue PERSONEN) zu öffnen.

    ![Fluxx Labs-Konfiguration](./media/fluxxlabs-tutorial/config4.png)

4. Führen Sie im Abschnitt **NEW PEOPLE** (NEUE PERSONEN) die folgenden Schritte aus:

    ![Fluxx Labs-Konfiguration](./media/fluxxlabs-tutorial/config5.png)

    a. Fluxx Labs verwendet die E-Mail-Adresse als eindeutigen Bezeichner für SSO-Anmeldenamen. Geben Sie im Feld **SSO UID** (SSO-UID) die E-Mail-Adresse des Benutzers ein, die der Benutzer für die SSO-Anmeldung verwendet.

    b. Klicken Sie auf **Speichern**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Fluxx Labs gewähren.

![Zuweisen der Benutzerrolle][200]

**Um Britta Simon Fluxx Labs zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Fluxx Labs** aus.

    ![Fluxx Labs-Link in der Anwendungsliste](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Fluxx Labs“ klicken, sollten Sie automatisch bei Ihrer Fluxx Labs-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/fluxxlabs-tutorial/tutorial_general_01.png
[2]: ./media/fluxxlabs-tutorial/tutorial_general_02.png
[3]: ./media/fluxxlabs-tutorial/tutorial_general_03.png
[4]: ./media/fluxxlabs-tutorial/tutorial_general_04.png

[100]: ./media/fluxxlabs-tutorial/tutorial_general_100.png

[200]: ./media/fluxxlabs-tutorial/tutorial_general_200.png
[201]: ./media/fluxxlabs-tutorial/tutorial_general_201.png
[202]: ./media/fluxxlabs-tutorial/tutorial_general_202.png
[203]: ./media/fluxxlabs-tutorial/tutorial_general_203.png
