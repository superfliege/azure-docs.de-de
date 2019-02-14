---
title: 'Tutorial: Azure Active Directory-Integration mit N2F – Spesenberichte| Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und N2F konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f56d53d7-5a08-490a-bfb9-78fefc2751ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3166a4e19bc137d57b97dbb516e7037228e1c020
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56188816"
---
# <a name="tutorial-azure-active-directory-integration-with-n2f---expense-reports"></a>Tutorial: Azure Active Directory-Integration mit N2F – Spesenberichte

In diesem Tutorial erfahren Sie, wie Sie N2F für Spesenberichte in Azure Active Directory (Azure AD) integrieren.

Das Integrieren von N2F in Azure AD bietet Ihnen die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf die N2F-Spesenberichte hat.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei N2F anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit N2F zu konfigurieren, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein N2F-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von N2F aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-n2f---expense-reports-from-the-gallery"></a>Hinzufügen von N2F aus dem Katalog

Um die Integration von N2F in Azure AD zu konfigurieren, müssen Sie N2F aus dem Katalog Ihrer Liste von verwalteten SaaS-Anwendungen hinzufügen.

**So fügen Sie N2F aus dem Katalog hinzu**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **N2F** ein, wählen Sie im Ergebnisbereich **N2F** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![N2F in der Ergebnisliste](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit N2F mithilfe der Testbenutzerin Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in N2F als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und einem entsprechenden Benutzer in N2F muss eine Verknüpfung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei N2F müssen Sie die folgenden Aufgaben ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **Erstellen eines N2F-Testbenutzers**, um ein Pendant zu Britta Simon in N2F – Spesenberichte zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer N2F-Anwendung.

**So konfigurieren Sie das einmalige Anmelden von Azure AD mit N2F**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **N2F** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Dialogfeld „Einmaliges Anmelden“](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_samlbase.png)

3. Wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten, müssen Sie im Abschnitt **Domäne und URLs für N2F** keine Schritte ausführen, da die App bereits vorab in Azure integriert wurde.

    ![SSO-Informationen zur Domäne und zu den URLs für N2F](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_url1.png)

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für N2F](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_url2.png)

    Geben Sie im Textfeld **Anmelde-URL** diese URL ein: `https://www.n2f.com/app/`

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf die Schaltfläche „Kopieren“, um die **Verbundmetadaten-URL der App** zu kopieren und in Editor einzufügen.

    ![Downloadlink für das Zertifikat](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_certificate.png)

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/n2f-expensereports-tutorial/tutorial_general_400.png)

7. Klicken Sie im Abschnitt **N2F-Konfiguration** auf **N2F konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID** aus dem Abschnitt **Kurzübersicht**.

    ![N2F-Konfiguration](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_configure.png)

8. Melden Sie sich in einem anderen Webbrowserfenster auf der N2F-Unternehmenswebsite als Administrator an.

9. Klicken Sie auf **Settings** (Einstellungen), und wählen Sie aus der Dropdownliste **Advanced Settings** (Erweiterte Einstellungen) aus.

    ![N2F-Konfiguration](./media/n2f-expensereports-tutorial/configure1.png)

10. Wählen Sie die Registerkarte **Account settings** (Kontoeinstellungen) aus.

    ![N2F-Konfiguration](./media/n2f-expensereports-tutorial/configure2.png)

11. Wählen Sie **Authentication** (Authentifizierung) und dann die Registerkarte **+ Add an authentication method** (Authentifizierungsmethode hinzufügen) aus.

    ![N2F-Konfiguration](./media/n2f-expensereports-tutorial/configure3.png)

12. Wählen Sie als Authentifizierungsmethode **SAML Microsoft Office 365** (Microsoft Office 365-SAML) aus.

    ![N2F-Konfiguration](./media/n2f-expensereports-tutorial/configure4.png)

13. Führen Sie im Abschnitt **Authentication method** (Authentifizierungsmethode) die folgenden Schritte aus:

    ![N2F-Konfiguration](./media/n2f-expensereports-tutorial/configure5.png)

    a. Fügen Sie im Textfeld **Entity ID** (Entitäts-ID) den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.

    b. Fügen Sie im Textfeld **Metadata URL** (Metadaten-URL) den Wert der **Verbundmetadaten-URL der App** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Klicken Sie auf **Speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/n2f-expensereports-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/n2f-expensereports-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/n2f-expensereports-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/n2f-expensereports-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="create-a-n2f---expense-reports-test-user"></a>Erstellen eines N2F-Testbenutzers

Azure AD-Benutzer müssen in N2F bereitgestellt werden, um sich bei N2F anmelden zu können. Im Fall von N2F muss die Bereitstellung manuell erfolgen.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich auf der N2F-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf **Settings** (Einstellungen), und wählen Sie aus der Dropdownliste **Advanced Settings** (Erweiterte Einstellungen) aus.

   ![Hinzufügen eines Benutzers in N2F](./media/n2f-expensereports-tutorial/configure1.png)

3. Wählen Sie links im Navigationsbereich die Registerkarte **Users** (Benutzer) aus.

    ![N2F-Konfiguration](./media/n2f-expensereports-tutorial/user1.png)

4. Wählen Sie die Registerkarte **+ New user** (Neuer Benutzer) aus.

   ![N2F-Konfiguration](./media/n2f-expensereports-tutorial/user2.png)

5. Führen Sie im Abschnitt **User** (Benutzer) die folgenden Schritte aus:

    ![N2F-Konfiguration](./media/n2f-expensereports-tutorial/user3.png)

    a. Geben Sie im Textfeld **Email address** (E-Mail-Adresse) die E-Mail-Adresse des Benutzers ein, z.B. **brittasimon@contoso.com**.

    b. Geben Sie im Textfeld **Vorname** den Vornamen des Benutzers ein (z.B. **Britta**).

    c. Geben Sie im Textfeld **Name** den Namen des Benutzers ein, z.B. **BrittaSimon**.

    d. Wählen Sie **Role, Direct manager (N+1)** (Rolle, Direkter Vorgesetzter (N+1)) und **Division** (Abteilung) gemäß Ihren Organisationsanforderungen aus.

    e. Klicken Sie auf **Validate and send invitation** (Bestätigen und Einladung senden).

    > [!NOTE]
    > Wenn beim Hinzufügen des Benutzers Probleme auftreten, wenden Sie sich an das [N2F-Supportteam](mailto:support@n2f.com).

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie der Benutzerin Britta Simon das einmalige Anmelden von Azure, indem Sie ihr Zugriff auf N2F gewähren.

![Zuweisen der Benutzerrolle][200]

**So weisen Sie Britta Simon N2F zu**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **N2F**aus.

    ![N2F-Link in der Anwendungsliste](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „N2F“ klicken, sollten Sie automatisch bei Ihrer N2F-Anwendung für Spesenberichte angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/n2f-expensereports-tutorial/tutorial_general_01.png
[2]: ./media/n2f-expensereports-tutorial/tutorial_general_02.png
[3]: ./media/n2f-expensereports-tutorial/tutorial_general_03.png
[4]: ./media/n2f-expensereports-tutorial/tutorial_general_04.png

[100]: ./media/n2f-expensereports-tutorial/tutorial_general_100.png

[200]: ./media/n2f-expensereports-tutorial/tutorial_general_200.png
[201]: ./media/n2f-expensereports-tutorial/tutorial_general_201.png
[202]: ./media/n2f-expensereports-tutorial/tutorial_general_202.png
[203]: ./media/n2f-expensereports-tutorial/tutorial_general_203.png

