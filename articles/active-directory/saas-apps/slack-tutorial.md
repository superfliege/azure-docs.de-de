---
title: 'Tutorial: Azure Active Directory-Integration mit Slack | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Slack konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffc5e73f-6c38-4bbb-876a-a7dd269d4e1c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/14/2018
ms.author: jeedes
ms.openlocfilehash: 5b1099e46cf1aa2fd4b948fee8407cfd859390ce
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46129114"
---
# <a name="tutorial-azure-active-directory-integration-with-slack"></a>Tutorial: Azure Active Directory-Integration mit Slack

In diesem Tutorial erfahren Sie, wie Sie Slack in Azure Active Directory (Azure AD) integrieren.

Die Integration von Slack in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Slack hat.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Slack anzumelden (Sigle Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Slack konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Slack-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Slack aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-slack-from-the-gallery"></a>Hinzufügen von Slack aus dem Katalog
Um die Integration von Slack in Azure AD zu konfigurieren, müssen Sie Slack über den Katalog Ihrer Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Slack über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

1. Geben Sie in das Suchfeld den Suchbegriff **Slack** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/slack-tutorial/tutorial_slack_search.png)

1. Wählen Sie im Ergebnisbereich die Option **Slack** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/slack-tutorial/tutorial_slack_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD mit Slack.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Slack als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Slack muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Slack den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Slack müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
1. **[Erstellen eines Slack-Testbenutzers](#creating-a-slack-test-user)**, um ein Pendant von Britta Simon in Slack zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Slack-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Slack die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Slack** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Configure single sign-on](./media/slack-tutorial/tutorial_slack_samlbase.png)

1. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für Slack** aus:

    ![Configure single sign-on](./media/slack-tutorial/tutorial_slack_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<companyname>.slack.com`.

    b. Aktualisieren Sie im Feld **Bezeichner** den Wert mit der Anmelde-URL. Dies ist Ihre Arbeitsbereichsdomäne. Beispiel: `https://contoso.slack.com`

1. Die Slack-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:
    
    ![Configure single sign-on](./media/slack-tutorial/tutorial_slack_attribute.png)

    > [!NOTE] 
    > Wenn Sie Benutzer haben, deren **E-Mail-Adresse** nicht in einer Office365-Lizenz enthalten ist, wird der **User.Email**-Anspruch nicht im SAML-Token angezeigt. In diesen Fällen empfehlen wir, **user.userprincipalname** als **User.Email**-Attributwert zu verwenden, um ihn stattdessen als **eindeutigen Bezeichner** zuzuordnen.

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute** den Eintrag **user.mail** als **Benutzerbezeichner** aus, und führen Sie für jede in der folgenden Tabelle aufgeführte Zeile die folgenden Schritte aus:
    
    | Attributname | Attributwert |
    | --- | --- |
    | first_name | user.givenname |
    | last_name | user.surname |
    | User.Email | user.mail |  
    | User.Username | user.userprincipalname |

    a. Klicken Sie auf **Attribut**, um das Dialogfeld **Attribut bearbeiten** zu öffnen, und führen Sie die folgenden Schritte aus:

    ![Configure single sign-on](./media/slack-tutorial/tutorial_slack_attribute1.png)

    a. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    b. Wählen Sie in der Liste **Wert** den für diese Zeile angezeigten Attributwert aus.

    c. Lassen Sie den **Namespace** leer.

    d. Klicken Sie auf **OK**

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Configure single sign-on](./media/slack-tutorial/tutorial_slack_certificate.png)

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/slack-tutorial/tutorial_general_400.png)

1. Klicken Sie im Abschnitt **Slack-Konfiguration** auf **Slack konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID und die SAML-Dienst-URL für einmaliges Anmelden** aus dem Abschnitt **Kurzübersicht**.

    ![Configure single sign-on](./media/slack-tutorial/tutorial_slack_configure.png)

1. Melden Sie sich in einem anderen Webbrowserfenster bei der Slack-Unternehmenswebsite als Administrator an.

1. Navigieren Sie zu **Microsoft Azure AD** und dann zu **Teameinstellungen**.

     ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/slack-tutorial/tutorial_slack_001.png)

1. Klicken Sie im Abschnitt **Teameinstellungen** auf die Registerkarte **Authentifizierung**, und klicken Sie dann auf **Einstellungen ändern**.

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/slack-tutorial/tutorial_slack_002.png)

1. Führen Sie im Dialogfeld **SAML-Authentifizierungseinstellungen** die folgenden Schritte aus:

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/slack-tutorial/tutorial_slack_003.png)

    a.  Fügen Sie in das Textfeld **SAML 2.0 Endpoint (HTTP)** den Wert der **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.

    b.  Fügen Sie in das Textfeld **Aussteller des Identitätsanbieters** den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.

    c.  Öffnen Sie das heruntergeladene Zertifikat in Editor, kopieren Sie den Inhalt in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Öffentliches Zertifikat** ein.

    d. Konfigurieren Sie die oben genannten drei Einstellungen gemäß den Anforderungen Ihres Slack-Teams. Weitere Informationen zu den Einstellungen finden Sie hier im **SSO-Konfigurationshandbuch für Slack**. `https://get.slack.help/hc/articles/220403548-Guide-to-single-sign-on-with-Slack%60`

    e.  Klicken Sie auf **Konfiguration speichern**.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/slack-tutorial/create_aaduser_01.png) 

1. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/slack-tutorial/create_aaduser_02.png) 

1. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/slack-tutorial/create_aaduser_03.png)

1. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/slack-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.

### <a name="creating-a-slack-test-user"></a>Erstellen eines Slack-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Slack. Slack unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf Slack ein neuer Benutzer erstellt. Außerdem unterstützt Slack die automatische Benutzerbereitstellung. Weitere Informationen zum Konfigurieren der automatischen Benutzerbereitstellung finden Sie [hier](slack-provisioning-tutorial.md).

> [!NOTE]
> Setzen Sie sich mit dem [Supportteam von Slack](https://slack.com/help/contact) in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.

> [!NOTE]
> Azure AD Connect ist das Synchronisierungstool, das lokale Active Directory-Identitäten mit Azure AD synchronisieren kann, woraufhin diese synchronisierten Benutzer ebenfalls die Anwendungen wie alle anderen Cloudbenutzer verwenden können.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Slack gewähren.

![Benutzer zuweisen][200]

**Um Britta Simon Slack zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste die Option **Slack** aus.

    ![Configure single sign-on](./media/slack-tutorial/tutorial_slack_app.png) 

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Slack“ klicken, sollten Sie automatisch bei Ihrer Slack-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Konfigurieren der Benutzerbereitstellung](slack-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/slack-tutorial/tutorial_general_01.png
[2]: ./media/slack-tutorial/tutorial_general_02.png
[3]: ./media/slack-tutorial/tutorial_general_03.png
[4]: ./media/slack-tutorial/tutorial_general_04.png

[100]: ./media/slack-tutorial/tutorial_general_100.png

[200]: ./media/slack-tutorial/tutorial_general_200.png
[201]: ./media/slack-tutorial/tutorial_general_201.png
[202]: ./media/slack-tutorial/tutorial_general_202.png
[203]: ./media/slack-tutorial/tutorial_general_203.png
