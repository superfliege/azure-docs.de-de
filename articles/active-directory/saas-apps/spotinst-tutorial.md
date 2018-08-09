---
title: 'Tutorial: Azure Active Directory-Integration mit Spotinst | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Spotinst konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2f6dbd70-c2db-4ae9-99ee-976c3090d214
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2018
ms.author: jeedes
ms.openlocfilehash: 7edf1cbc5cc351e25a9ae7b319768376ea9968a3
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "39267048"
---
# <a name="tutorial-azure-active-directory-integration-with-spotinst"></a>Tutorial: Azure Active Directory-Integration mit Spotinst

In diesem Tutorial erfahren Sie, wie Sie Spotinst in Azure Active Directory (Azure AD) integrieren.

Die Integration von Spotinst in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Spotinst hat.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Spotinst anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Spotinst konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Spotinst-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Spotinst aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-spotinst-from-the-gallery"></a>Hinzufügen von Spotinst aus dem Katalog
Zum Konfigurieren der Integration von Spotinst in Azure AD müssen Sie Keylight aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Spotinst aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Spotinst** ein, wählen Sie im Ergebnisbereich **Spotinst** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Spotinst in der Ergebnisliste](./media/spotinst-tutorial/tutorial_spotinst_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Spotinst mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Spotinst als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Spotinst muss eine Linkbeziehung eingerichtet werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Spotinst zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Spotinst-Testbenutzers](#create-a-spotinst-test-user)**, um ein Pendant von Britta Simon in Spotinst zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Spotinst-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Spotinst zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Spotinst** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/spotinst-tutorial/tutorial_spotinst_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Spotinst** die folgenden Schritte aus, wenn Sie die Anwendung im IDP-initiierten Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Spotinst](./media/spotinst-tutorial/tutorial_spotinst_url1.png)

    a. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**.

    b. Geben Sie im Textfeld **Relayzustand** einen Wert ein: `<ID>`

    c. Wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten, geben Sie die URL in das Textfeld **Anmelde-URL** ein: `https://console.spotinst.com`

    > [!NOTE]
    > Der Wert für den Relayzustand entspricht nicht dem tatsächlichen Wert. Sie aktualisieren den Wert für den Relayzustand mit dem tatsächlichen Wert für den Relayzustand. Dies wird später in diesem Tutorial beschrieben.

4. Die Spotinst-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen.

    ![Configure single sign-on](./media/spotinst-tutorial/tutorial_Spotinst_attribute.png)

5. Konfigurieren Sie das SAML-Tokenattribut im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute**, wie im obigen Bild gezeigt, und führen Sie die folgenden Schritte aus:

    | Attributname | Attributwert |
    | ---------------| --------------- |
    | E-Mail | user.mail |
    | FirstName | user.givenname |
    | Nachname | user.surname |
    
    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Configure single sign-on](./media/spotinst-tutorial/tutorial_attribute_04.png)

    ![Configure single sign-on](./media/spotinst-tutorial/tutorial_attribute_05.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.

    d. Lassen Sie den **Namespace** leer.

    e. Klicken Sie auf **OK**.

6. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/spotinst-tutorial/tutorial_spotinst_certificate.png) 

7. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/spotinst-tutorial/tutorial_general_400.png)

8. Melden Sie sich in einem anderen Webbrowserfenster als Sicherheitsadministrator bei Spotinst an.

9. Klicken Sie auf das **Benutzersymbol** in der oberen rechten Ecke und dann auf **Einstellungen**.

    ![Spotinst-Einstellungen](./media/spotinst-tutorial/tutorial_spotinst_settings.png)

10. Klicken Sie auf die Registerkarte **SICHERHEIT** am oberen Rand, wählen Sie **Identitätsanbieter**, und führen Sie die folgenden Schritte aus:

    ![Spotinst-Sicherheit](./media/spotinst-tutorial/tutorial_spotinst_security.png)

    a. Kopieren Sie den Wert für den **Relayzustand**, und fügen Sie ihn in das Textfeld **Relayzustand** im Abschnitt **Domäne und URLs für Spotinst** im Azure-Portal ein.

    b. Klicken Sie dann zum Hochladen der XML-Metadatendatei, die Sie aus dem Azure-Portal heruntergeladen haben, auf **DURCHSUCHEN**.

    c. Klicken Sie auf **SPEICHERN**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/spotinst-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/spotinst-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/spotinst-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/spotinst-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="create-a-spotinst-test-user"></a>Erstellen eines Spotinst-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Spotinst.

1. Wenn Sie die Anwendung im **SP**-initiierten Modus konfigurieren möchten, führen Sie die folgenden Schritte aus:

   a. Melden Sie sich in einem anderen Webbrowserfenster als Sicherheitsadministrator bei Spotinst an.

   b. Klicken Sie auf das **Benutzersymbol** in der oberen rechten Ecke und dann auf **Einstellungen**.

    ![Spotinst-Einstellungen](./media/spotinst-tutorial/tutorial_spotinst_settings.png)

    c. Klicken Sie auf **Benutzer**, und wählen Sie **BENUTZER HINZUFÜGEN**.

    ![Spotinst-Einstellungen](./media/spotinst-tutorial/adduser1.png)

    d. Führen Sie im Abschnitt „Benutzer hinzufügen“ die folgenden Schritte aus:

    ![Spotinst-Einstellungen](./media/spotinst-tutorial/adduser2.png)

    * Geben Sie im Textfeld **Vollständiger Name** den vollständigen Namen des Benutzers ein, z.B. **BrittaSimon**.

    * Geben Sie im Textfeld **E-Mail** die E-Mail-Adresse des Benutzers ein, z.B. **brittasimon@contoso.com**.

    * Wählen Sie Ihre organisationsspezifischen Details für die **Organisationsrolle, Kontorolle und Konten**.

2. Wenn Sie die Anwendung im **IDP**-initiierten Modus konfiguriert haben, finden Sie in diesem Abschnitt kein Aktionselement. Spotinst unterstützt die Just-In-Time-Bereitstellung, die standardmäßig aktiviert ist. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf Spotinst ein neuer Benutzer erstellt.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Spotinst gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Spotinst zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **Spotinst**aus.

    ![Spotinst-Link in der Anwendungsliste](./media/spotinst-tutorial/tutorial_spotinst_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Spotinst“ klicken, sollten Sie automatisch bei Ihrer Spotinst-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/spotinst-tutorial/tutorial_general_01.png
[2]: ./media/spotinst-tutorial/tutorial_general_02.png
[3]: ./media/spotinst-tutorial/tutorial_general_03.png
[4]: ./media/spotinst-tutorial/tutorial_general_04.png

[100]: ./media/spotinst-tutorial/tutorial_general_100.png

[200]: ./media/spotinst-tutorial/tutorial_general_200.png
[201]: ./media/spotinst-tutorial/tutorial_general_201.png
[202]: ./media/spotinst-tutorial/tutorial_general_202.png
[203]: ./media/spotinst-tutorial/tutorial_general_203.png

