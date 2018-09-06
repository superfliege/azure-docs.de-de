---
title: 'Tutorial: Azure Active Directory-Integration mit ARcGIS Enterprise | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und ArcGIS Enterprise konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 24809e9d-a4aa-4504-95a9-e4fcf484f431
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2018
ms.author: jeedes
ms.openlocfilehash: ea2b32b43fedacba7b8a60db29762c32fda65aa5
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43306341"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-enterprise"></a>Tutorial: Azure Active Directory-Integration in ArcGIS Enterprise

In diesem Tutorial erfahren Sie, wie Sie ArcGIS Enterprise in Azure Active Directory (Azure AD) integrieren.

Die Integration von ArcGIS Enterprise in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf ArcGIS Enterprise Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei ArcGIS Enterprise anzumelden (einmaliges Anmelden, Single Sign-On).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit ArcGIS Enterprise konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein ArcGIS Enterprise-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von ArcGIS Enterprise aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-arcgis-enterprise-from-the-gallery"></a>Hinzufügen von ArcGIS Enterprise aus dem Katalog

Zum Konfigurieren der Integration von ArcGIS Enterprise in Azure AD müssen Sie ArcGIS Enterprise aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um ArcGIS Enterprise aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **ArcGIS Enterprise** ein, wählen Sie im Ergebnisbereich **ArcGIS Enterprise** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![ArcGIS Enterprise in der Ergebnisliste](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei ArcGIS Enterprise basierend auf einer Testbenutzerin mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in ArcGIS Enterprise als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in ArcGIS Enterprise muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei ArcGIS Enterprise müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines ArcGIS Enterprise-Testbenutzers](#create-an-arcgis-enterprise-test-user)**, um eine Entsprechung von Britta Simon in ArcGIS Enterprise zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer ArcGIS Enterprise-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in ArcGIS Enterprise die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **ArcGIS Enterprise** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Dialogfeld „Einmaliges Anmelden“](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für ArcGIS Enterprise** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für ArcGIS Enterprise](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_url1.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `<EXTERNAL_DNS_NAME>.portal`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<EXTERNAL_DNS_NAME>/portal/sharing/rest/oauth2/saml/signin2`

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für ArcGIS Enterprise](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_url2.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<EXTERNAL_DNS_NAME>/portal/sharing/rest/oauth2/saml/signin`.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner, die Antwort-URL und die Anmelde-URL. Wenden Sie sich an das [Supportteam von ArcGIS Enterprise Client](mailto:support@esri.com), um diese Werte zu erhalten. Sie erhalten den Bezeichnerwert im Abschnitt **Festlegen des Identitätsanbieters** weiter unten in diesem Tutorial.

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf die Schaltfläche „Kopieren“, um die **Verbundmetadaten-URL der App** zu kopieren und in Editor einzufügen.

    ![Downloadlink für das Zertifikat](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_certificate.png)

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/arcgisenterprise-tutorial/tutorial_general_400.png)

7. Melden Sie sich in einem anderen Webbrowserfenster bei der ArcGIS Enterprise-Unternehmenswebsite als Administrator an.

8. Wählen Sie **Organisation > EINSTELLUNGEN BEARBEITEN**.

    ![ArcGIS Enterprise-Konfiguration](./media/arcgisenterprise-tutorial/configure1.png)

9. Wählen Sie die Registerkarte **Security** (Sicherheit) aus.

    ![ArcGIS Enterprise-Konfiguration](./media/arcgisenterprise-tutorial/configure2.png)

10. Führen Sie einen Bildlauf nach unten zum Abschnitt **Enterprise-Anmeldungen über SAML** aus, und wählen Sie **ENTERPRISE-ANMELDUNG FESTLEGEN** aus.

    ![ArcGIS Enterprise-Konfiguration](./media/arcgisenterprise-tutorial/configure3.png)

11. Führen Sie im Abschnitt **Festlegen des Identitätsanbieters** die folgenden Schritte aus:

    ![ArcGIS Enterprise-Konfiguration](./media/arcgisenterprise-tutorial/configure4.png)

    a. Geben Sie einen Namen wie **Azure Active Directory-Test** in das Textfeld **Name** ein.

    b. Fügen Sie im Textfeld **URL** den Wert der **Verbundmetadaten-URL der App** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Klicken Sie auf **Erweiterte Einstellungen anzeigen**, und kopieren Sie den Wert **Entitäts-ID** und fügen Sie ihn in das Textfeld **Bezeichner** im Abschnitt **ArcGIS-Enterprise-Domäne und URLs** im Azure-Portal ein.
    
    ![ArcGIS Enterprise-Konfiguration](./media/arcgisenterprise-tutorial/configure5.png)

    d. Klicken Sie auf **IDENTITÄTSANBIETER AKTUALISIEREN**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/arcgisenterprise-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/arcgisenterprise-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/arcgisenterprise-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/arcgisenterprise-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="create-an-arcgis-enterprise-test-user"></a>Erstellen eines ArcGIS Enterprise-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in ArcGIS Enterprise. ArcGIS Enterprise unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf ArcGIS Enterprise ein neuer Benutzer erstellt.

> [!Note]
> Wenn Sie einen Benutzer manuell erstellen müssen, wenden Sie sich an das [ArcGIS Enterprise-Supportteam](mailto:support@esri.com).

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf ArcGIS Enterprise gewähren.

![Zuweisen der Benutzerrolle][200]

**Um Britta Simon ArcGIS Enterprise zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **ArcGIS Enterprise** aus.

    ![ArcGIS Enterprise-Link in der Anwendungsliste](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „ArcGIS Enterprise“ klicken, sollten Sie automatisch bei Ihrer ArcGIS Enterprise-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/arcgisenterprise-tutorial/tutorial_general_01.png
[2]: ./media/arcgisenterprise-tutorial/tutorial_general_02.png
[3]: ./media/arcgisenterprise-tutorial/tutorial_general_03.png
[4]: ./media/arcgisenterprise-tutorial/tutorial_general_04.png

[100]: ./media/arcgisenterprise-tutorial/tutorial_general_100.png

[200]: ./media/arcgisenterprise-tutorial/tutorial_general_200.png
[201]: ./media/arcgisenterprise-tutorial/tutorial_general_201.png
[202]: ./media/arcgisenterprise-tutorial/tutorial_general_202.png
[203]: ./media/arcgisenterprise-tutorial/tutorial_general_203.png