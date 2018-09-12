---
title: 'Tutorial: Azure Active Directory-Integration mit Snowflake | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und Snowflake konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3488ac27-0417-4ad9-b9a3-08325fe8ea0d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2018
ms.author: jeedes
ms.openlocfilehash: 3ad3f42563878d829f900d5cddb0c6866d2deab5
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43311251"
---
# <a name="tutorial-azure-active-directory-integration-with-snowflake"></a>Tutorial: Azure Active Directory-Integration mit Snowflake

In diesem Tutorial erfahren Sie, wie Sie Snowflake in Azure Active Directory (Azure AD) integrieren.

Die Integration von Snowflake in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Snowflake hat.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Snowflake anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Snowflake konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Snowflake-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Snowflake aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-snowflake-from-the-gallery"></a>Hinzufügen von Snowflake aus dem Katalog
Zum Konfigurieren der Integration von Snowflake in Azure AD müssen Sie Snowflake aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Snowflake aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Snowflake** ein, wählen Sie im Ergebnisbereich **Snowflake** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Snowflake in der Ergebnisliste](./media/snowflake-tutorial/tutorial_snowflake_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie einmaliges Anmelden mit Azure AD bei Snowflake mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Snowflake als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Snowflake muss eine Linkbeziehung eingerichtet werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Snowflake zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Snowflake-Testbenutzers](#create-a-snowflake-test-user)**, um eine Entsprechung von Britta Simon in Snowflake zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Snowflake-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Snowflake zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Snowflake** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/snowflake-tutorial/tutorial_snowflake_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Snowflake** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Snowflake](./media/snowflake-tutorial/tutorial_snowflake_url.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<SNOWFLAKE-URL>`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<SNOWFLAKE-URL>/fed/login`

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Snowflake](./media/snowflake-tutorial/tutorial_snowflake_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<SNOWFLAKE-URL>`.
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner, die Antwort-URL und die Anmelde-URL. Wenden Sie sich an das [Kundensupportteam von Snowflake](https://support.snowflake.net/s/snowflake-support), um diese Werte zu erhalten. 

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/snowflake-tutorial/tutorial_snowflake_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/snowflake-tutorial/tutorial_general_400.png)
    
7. Klicken Sie im Abschnitt **Snowflake-Konfiguration** auf **Snowflake konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Snowflake-Konfiguration](./media/snowflake-tutorial/tutorial_snowflake_configure.png) 

8. Zum Konfigurieren des einmaligen Anmeldens bei **Snowflake** müssen Sie das heruntergeladene **Zertifikat (Base64)** und die **SAML-Dienst-URL für einmaliges Anmelden** an das [Snowflake-Supportteam](https://support.snowflake.net/s/snowflake-support) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/snowflake-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/snowflake-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/snowflake-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/snowflake-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-snowflake-test-user"></a>Erstellen eines Snowflake-Testbenutzers

In diesem Abschnitt erstellen Sie in Snowflake eine Benutzerin namens Britta Simon. Lassen Sie sich beim Hinzufügen der Benutzer zur Snowflake-Plattform vom [Snowflake-Supportteam](https://support.snowflake.net/s/snowflake-support) unterstützen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Snowflake gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon zu Snowflake zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Snowflake**aus.

    ![Snowflake-Link in der Anwendungsliste](./media/snowflake-tutorial/tutorial_snowflake_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Snowflake“ klicken, sollten Sie automatisch bei Ihrer Snowflake-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/snowflake-tutorial/tutorial_general_01.png
[2]: ./media/snowflake-tutorial/tutorial_general_02.png
[3]: ./media/snowflake-tutorial/tutorial_general_03.png
[4]: ./media/snowflake-tutorial/tutorial_general_04.png

[100]: ./media/snowflake-tutorial/tutorial_general_100.png

[200]: ./media/snowflake-tutorial/tutorial_general_200.png
[201]: ./media/snowflake-tutorial/tutorial_general_201.png
[202]: ./media/snowflake-tutorial/tutorial_general_202.png
[203]: ./media/snowflake-tutorial/tutorial_general_203.png

