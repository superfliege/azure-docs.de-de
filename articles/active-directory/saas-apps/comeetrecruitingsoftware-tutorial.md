---
title: 'Tutorial: Azure Active Directory-Integration mit Comeet Recruiting Software | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und Comeet Recruiting Software konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 75f51dc9-9525-4ec6-80bf-28374f0c8adf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2018
ms.author: jeedes
ms.openlocfilehash: 137ba7a7e82ff3e57d862868859e8049838701a3
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43311067"
---
# <a name="tutorial-azure-active-directory-integration-with-comeet-recruiting-software"></a>Tutorial: Azure Active Directory-Integration mit Comeet Recruiting Software

In diesem Tutorial erfahren Sie, wie Sie Comeet Recruiting Software in Azure Active Directory (Azure AD) integrieren.

Die Integration von Comeet Recruiting Software in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Comeet Recruiting Software hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Comeet Recruiting Software anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Comeet Recruiting Software konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Comeet Recruiting Software-Abonnement, für das einmaliges Anmelden aktiviert ist

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Comeet Recruiting Software aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-comeet-recruiting-software-from-the-gallery"></a>Hinzufügen von Comeet Recruiting Software aus dem Katalog

Zum Konfigurieren der Integration von Comeet Recruiting Software in Azure AD müssen Sie Comeet Recruiting Software aus dem Katalog der Liste Ihrer verwalteten SaaS-Apps hinzufügen.

**Um Comeet Recruiting Software aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Comeet Recruiting Software** ein. Wählen Sie im Ergebnisbereich **Comeet Recruiting Software** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Comeet Recruiting Software in der Ergebnisliste](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

Dieser Abschnitt veranschaulicht anhand eines Testbenutzers namens Britta Simon, wie das einmalige Anmelden von Azure AD in Comeet Recruiting Software konfiguriert und getestet wird.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, wer der entsprechende Gegenbenutzer in Comeet Recruiting Software zu einem Benutzer in Azure AD ist. Anders ausgedrückt muss zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Comeet Recruiting Software eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Comeet Recruiting Software müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Comeet Recruiting Software-Testbenutzers](#create-a-comeet-recruiting-software-test-user)**, um eine Entsprechung von Britta Simon in Comeet Recruiting Software zu erhalten, die mit ihrer Benutzerdarstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Comeet Recruiting Software-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Comeet Recruiting Software die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Comeet Recruiting Software** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Dialogfeld „Einmaliges Anmelden“](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Comeet Recruiting Software** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Comeet](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_url1.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://app.comeet.co/adfs_auth/acs/<UNIQUEID>/`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://app.comeet.co/adfs_auth/acs/<UNIQUEID>/`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit dem tatsächlichen Bezeichner und der Antwort-URL. Sie erhalten diese Werte vom Comeet Recruiting Software-Portal, wie auf der [Supportseite](https://support.comeet.co/knowledgebase/adfs-single-sign-on/) gezeigt.

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Comeet Recruiting Software](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_url2.png)

    Geben Sie im Textfeld **Anmelde-URL** diese URL ein: `https://app.comeet.co`

5. Die Comeet Recruiting Software-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: Der Standardwert von **Benutzer-ID** lautet **user.userprincipalname**, aber **Comeet Recruiting Software** erwartet, dass dieser Wert der E-Mail-Adresse des Benutzers zugeordnet ist. Hierfür können Sie das **user.mail**-Attribut aus der Liste verwenden oder den entsprechenden Attributwert gemäß der Konfiguration in Ihrer Organisation angeben.

    ![Configure single sign-on](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_attribute.png)

6. Aktivieren Sie im Abschnitt **Benutzerattribute** das Kontrollkästchen **Alle weiteren Benutzerattribute anzeigen und bearbeiten**, um die Attribute zu erweitern. Führen Sie die folgenden Schritte für jedes der angezeigten Attribute aus:

    | Attributname | Attributwert |
    | ---------------| --------------- |
    | comeet_id | user.userprincipalname |

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Configure single sign-on](./media/comeetrecruitingsoftware-tutorial/tutorial_attribute_04.png)

    ![Configure single sign-on](./media/comeetrecruitingsoftware-tutorial/tutorial_attribute_05.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten **Attributnamen** ein.

    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.

    d. Klicken Sie auf **OK**.

7. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_certificate.png)

8. Klicken Sie auf die Schaltfläche **Save**.

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/comeetrecruitingsoftware-tutorial/tutorial_general_400.png)

9. Um einmaliges Anmelden auf der Seite **Comeet Recruiting Software** zu konfigurieren, fügen Sie den Inhalt der heruntergeladenen XML-Elemente mit den Metadaten in Comeet Recruiting Software ein, wie auf der [Supportseite](https://support.comeet.co/knowledgebase/adfs-single-sign-on/) gezeigt.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/comeetrecruitingsoftware-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/comeetrecruitingsoftware-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/comeetrecruitingsoftware-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/comeetrecruitingsoftware-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="create-a-comeet-recruiting-software-test-user"></a>Erstellen eines Comeet Recruiting Software-Testbenutzers

In diesem Abschnitt erstellen Sie in der Comeet Recruiting Software eine Benutzerin namens Britta Simon. Arbeiten Sie mit dem [Comeet Recruiting Software-Supportteam](mailto:support@comeet.co) beim Hinzufügen der Benutzer zur Comeet Recruiting Software-Plattform zusammen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung von einmaligem Anmelden von Azure, indem Sie ihr Zugriff auf Comeet Recruiting Software gewähren.

![Zuweisen der Benutzerrolle][200]

**Um Britta Simon Comeet Recruiting Software zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Comeet Recruiting Software**aus.

    ![Der Link „Comeet Recruiting Software“ in der Liste „Anwendungen“](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Comeet Recruiting Software“ klicken, sollten Sie automatisch an Ihrer Comeet Recruiting Software-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_01.png
[2]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_02.png
[3]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_03.png
[4]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_04.png

[100]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_100.png

[200]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_200.png
[201]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_201.png
[202]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_202.png
[203]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_203.png