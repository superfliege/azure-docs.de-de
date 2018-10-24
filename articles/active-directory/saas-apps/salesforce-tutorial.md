---
title: 'Tutorial: Azure Active Directory-Integration mit Salesforce | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Salesforce konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2018
ms.author: jeedes
ms.openlocfilehash: 36f1bd9c11c8932968a3501ef22fdb7153411256
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867560"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a>Tutorial: Azure Active Directory-Integration mit Salesforce

In diesem Tutorial erfahren Sie, wie Sie Salesforce in Azure Active Directory (Azure AD) integrieren.

Die Integration von Salesforce in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Salesforce hat.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Salesforce anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Salesforce konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Salesforce-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Salesforce aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-salesforce-from-the-gallery"></a>Hinzufügen von Salesforce aus dem Katalog

Zum Konfigurieren der Integration von Salesforce in Azure AD müssen Sie Salesforce aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um Salesforce aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Salesforce** ein, wählen Sie im Ergebnisbereich **Salesforce** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Salesforce in der Ergebnisliste](./media/salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Salesforce mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Salesforce als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Salesforce muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Salesforce den Wert für **Benutzername** aus Azure AD als Wert für **Benutzername** zu, um die Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Salesforce müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Salesforce-Testbenutzers](#create-a-salesforce-test-user)**, um ein Pendant von Britta Simon in Salesforce zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Salesforce-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD in Salesforce zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Salesforce** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Klicken Sie im oberen Bereich des Bildschirms auf **Modus für einmaliges Anmelden ändern**, um den Modus **SAML** auszuwählen.

    ![Konfigurieren des Links für einmaliges Anmelden](./media/salesforce-tutorial/tutorial_general_300.png)

3. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![Konfigurieren des Links für einmaliges Anmelden](./media/salesforce-tutorial/tutorial_general_301.png)

4. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.
   
    ![Konfigurieren des Links für einmaliges Anmelden](./media/salesforce-tutorial/tutorial_general_302.png)

5. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Salesforce](./media/salesforce-tutorial/tutorial_salesforce_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** den Wert im folgenden Format ein:

    Enterprise-Konto: `https://<subdomain>.my.salesforce.com`

    Developer-Konto: `https://<subdomain>-dev-ed.my.salesforce.com`

    b. Geben Sie im Textfeld **Bezeichner** den Wert in folgendem Format ein:

    Enterprise-Konto: `https://<subdomain>.my.salesforce.com`

    Developer-Konto: `https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Kundensupportteam von Salesforce](https://help.salesforce.com/support), um diese Werte zu erhalten.

6. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um die Datei **Verbundmetadaten-XML** herunterzuladen, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/salesforce-tutorial/tutorial_salesforce_certificate.png) 

7. Öffnen Sie eine neue Registerkarte in Ihrem Browser, und melden Sie sich mit Ihrem Salesforce-Administratorkonto an.

8. Klicken Sie in der oberen rechten Ecke der Seite unter dem **Einstellungssymbol** auf **Setup**.

    ![Configure single sign-on](./media/salesforce-tutorial/configure1.png)

9. Scrollen Sie im Navigationsbereich nach unten bis zu den **SETTINGS** (EINSTELLUNGEN), und klicken Sie auf **Identity** (Identität), um den zugehörigen Bereich zu erweitern. Klicken Sie dann auf **Einstellungen für einmaliges Anmelden**.

    ![Configure single sign-on](./media/salesforce-tutorial/sf-admin-sso.png)

10. Klicken Sie auf der Seite **Einstellungen für einmaliges Anmelden** auf die Schaltfläche **Bearbeiten**.

    ![Configure single sign-on](./media/salesforce-tutorial/sf-admin-sso-edit.png)

    > [!NOTE]
    > Wenn Sie keine Einstellungen für die einmalige Anmeldung für Ihr Salesforce-Konto aktivieren können, müssen Sie sich an das [Kundensupportteam von Salesforce](https://help.salesforce.com/support) wenden.

11. Wählen Sie **SAML aktiviert**, und klicken Sie dann auf **Speichern**.

      ![Configure single sign-on](./media/salesforce-tutorial/sf-enable-saml.png)

12. Klicken Sie auf **Neu aus Metadatendatei**, um Ihre SAML-Einstellungen für einmaliges Anmelden zu konfigurieren.

    ![Configure single sign-on](./media/salesforce-tutorial/sf-admin-sso-new.png)

13. Klicken Sie auf **Datei auswählen**, um die Metadaten-XML-Datei hochzuladen, die Sie aus dem Azure-Portal heruntergeladen haben, und klicken Sie dann auf **Erstellen**.

    ![Configure single sign-on](./media/salesforce-tutorial/xmlchoose.png)

14. Auf der Seite **SAML-Einstellungen für einmaliges Anmelden** werden die Felder automatisch aufgefüllt. Klicken Sie auf „Speichern“.

    ![Configure single sign-on](./media/salesforce-tutorial/salesforcexml.png)

15. Klicken Sie in Salesforce im linken Navigationsbereich auf **Company Settings** (Unternehmenseinstellungen), um den zugehörigen Abschnitt zu erweitern, und klicken Sie dann auf **My Domain** (Meine Domäne).

    ![Configure single sign-on](./media/salesforce-tutorial/sf-my-domain.png)

16. Führen Sie einen Bildlauf nach unten zum Abschnitt **Authentifizierungskonfiguration** aus, und klicken Sie auf die Schaltfläche **Bearbeiten**.

    ![Configure single sign-on](./media/salesforce-tutorial/sf-edit-auth-config.png)

17. Wählen Sie im Abschnitt **Konfiguration der Authentifizierung** die Option **AzureSSO** als **Authentifizierungsdienst** Ihrer SAML-SSO-Konfiguration, und klicken Sie dann auf **Speichern**.

    ![Configure single sign-on](./media/salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Wenn mehrere Authentifizierungsdienste aktiviert sind, werden Benutzer aufgefordert, einen Authentifizierungsdienst zur Anmeldung auszuwählen, wenn das einmalige Anmelden bei der Salesforce-Umgebung initiiert wird. Wenn Sie dies nicht möchten, sollten Sie **alle anderen Authentifizierungsdienste deaktiviert lassen**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory** aus, wählen Sie **Benutzer** aus, und wählen Sie dann **Alle Benutzer** aus.

    ![Azure AD-Benutzer erstellen][100]

2. Wählen Sie im oberen Bereich des Bildschirms die Option **Neuer Benutzer** aus.

    ![Erstellen eines Azure AD-Testbenutzers](./media/salesforce-tutorial/create_aaduser_01.png) 

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Erstellen eines Azure AD-Testbenutzers](./media/salesforce-tutorial/create_aaduser_02.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** die Zeichenfolge **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

### <a name="create-a-salesforce-test-user"></a>Erstellen eines Salesforce-Testbenutzers

In diesem Abschnitt wird ein Benutzer mit dem Namen Britta Simon in Salesforce erstellt. Salesforce unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Falls ein Benutzer nicht bereits in Salesforce vorhanden ist, wird beim Versuch, auf Salesforce zuzugreifen, ein neuer Benutzer erstellt. Außerdem unterstützt Salesforce die automatische Benutzerbereitstellung. Weitere Informationen zum Konfigurieren der automatischen Benutzerbereitstellung finden Sie [hier](salesforce-provisioning-tutorial.md).

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf Salesforce gewähren.

![Zuweisen der Benutzerrolle][200]

**Um Britta Simon Salesforce zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Liste der Anwendungen **Salesforce** aus.

    ![Salesforce-Link in der Anwendungsliste](./media/salesforce-tutorial/tutorial_salesforce_app.png)

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Salesforce“ klicken, sollten Sie automatisch bei Ihrer Salesforce-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Konfigurieren der Benutzerbereitstellung](salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/salesforce-tutorial/tutorial_general_01.png
[2]: ./media/salesforce-tutorial/tutorial_general_02.png
[3]: ./media/salesforce-tutorial/tutorial_general_03.png
[4]: ./media/salesforce-tutorial/tutorial_general_04.png

[100]: ./media/salesforce-tutorial/tutorial_general_100.png

[200]: ./media/salesforce-tutorial/tutorial_general_200.png
[201]: ./media/salesforce-tutorial/tutorial_general_201.png
[202]: ./media/salesforce-tutorial/tutorial_general_202.png
[203]: ./media/salesforce-tutorial/tutorial_general_203.png