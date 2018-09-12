---
title: 'Tutorial: Azure Active Directory-Integration mit Figma | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Figma konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8569cae1-87dd-4c40-9bbb-527ac80d6a96
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2018
ms.author: jeedes
ms.openlocfilehash: 4094de1a1c17e844d96ac789bb4bc1655fdc1546
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669242"
---
# <a name="tutorial-azure-active-directory-integration-with-figma"></a>Tutorial: Azure Active Directory-Integration mit Figma

In diesem Tutorial erfahren Sie, wie Sie Figma in Azure Active Directory (Azure AD) integrieren.

Die Integration von Figma in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Figma hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Figma anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration mit Figma benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Figma-Abonnement, [für das einmaliges Anmelden aktiviert ist](https://www.figma.com/pricing/)

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden. Neukunden und aktive Abonnenten von Figma Professional Team können sich an Figma wenden, um ein [Upgrade ihres Abonnements](https://www.figma.com/pricing/) auf die Organisationsebene von Figma durchzuführen.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Figma aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-figma-from-the-gallery"></a>Hinzufügen von Figma aus dem Katalog

Zum Konfigurieren der Integration von Figma in Azure AD müssen Sie Figma aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Figma aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Figma** ein, wählen Sie im Ergebnisbereich **Figma** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Figma in der Ergebnisliste](./media/figma-tutorial/tutorial_figma_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Figma mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD mit Figma verknüpft werden.  Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Figma die folgenden Schritte aus:

1. [**Wenden Sie sich an das Figma-Supportteam**](mailto:support@figma.com?subject=SAML+Config), um eine SAML-Konfiguration für Ihre Organisation zu initiieren und eine ORG_SAML_CONFIG_ID zu erhalten.
2. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Erstellen eines Figma-Testbenutzers](#create-a-figma-test-user)**, um ein Pendant von Britta Simon in Figma zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
5. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Figma-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Figma die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Figma** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Dialogfeld „Einmaliges Anmelden“](./media/figma-tutorial/tutorial_figma_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Figma** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Figma](./media/figma-tutorial/tutorial_figma_url1.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://www.figma.com/saml/<ORG_SAML_CONFIG_ID>`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://www.figma.com/saml/<ORG_SAML_CONFIG_ID>/consume`

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Figma](./media/figma-tutorial/tutorial_figma_url2.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://www.figma.com/saml/<ORG_SAML_CONFIG_ID>/start`.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner, die Antwort-URL und die Anmelde-URL. Wenden Sie sich an das [Supportteam von Figma](mailto:support@figma.com?subject=SAML+Config), um diese Werte zu erhalten.

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf die Schaltfläche „Kopieren“, um die **Verbundmetadaten-URL der App** zu kopieren und in Editor einzufügen.

    ![Downloadlink für das Zertifikat](./media/figma-tutorial/tutorial_figma_certificate.png)

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/figma-tutorial/tutorial_general_400.png)

7. Zum Konfigurieren des einmaligen Anmeldens auf Figma-Seite füllen Sie dieses Formular aus: [https://goo.gl/forms/XkRB1z5ed4eVUzXn2](https://goo.gl/forms/XkRB1z5ed4eVUzXn2). Es akzeptiert Ihre **App-Verbundmetadaten-URL** aus Schritt 5.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/figma-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/figma-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/figma-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/figma-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="create-a-figma-test-user"></a>Erstellen eines Figma-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Figma. Figma unterstützt die Just-In-Time-Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf Figma ein neuer Benutzer erstellt.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Figma gewähren.

![Zuweisen der Benutzerrolle][200]

**Führen Sie die folgenden Schritte aus, um Britta Simon Figma zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Figma**aus.

    ![Figma-Link in der Anwendungsliste](./media/figma-tutorial/tutorial_figma_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Figma“ klicken, sollten Sie automatisch bei Ihrer Figma-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/figma-tutorial/tutorial_general_01.png
[2]: ./media/figma-tutorial/tutorial_general_02.png
[3]: ./media/figma-tutorial/tutorial_general_03.png
[4]: ./media/figma-tutorial/tutorial_general_04.png

[100]: ./media/figma-tutorial/tutorial_general_100.png

[200]: ./media/figma-tutorial/tutorial_general_200.png
[201]: ./media/figma-tutorial/tutorial_general_201.png
[202]: ./media/figma-tutorial/tutorial_general_202.png
[203]: ./media/figma-tutorial/tutorial_general_203.png