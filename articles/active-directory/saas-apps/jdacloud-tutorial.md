---
title: 'Tutorial: Azure Active Directory-Integration mit JDA Cloud | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und JDA Cloud konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 06af825f-79ec-4de9-8851-c79d65d1e586
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2018
ms.author: jeedes
ms.openlocfilehash: 371ff15121e3da0bec5be6159fea9c6764b0aeda
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2018
ms.locfileid: "43703373"
---
# <a name="tutorial-azure-active-directory-integration-with-jda-cloud"></a>Tutorial: Azure Active Directory-Integration mit JDA Cloud

In diesem Tutorial erfahren Sie, wie Sie JDA Cloud in Azure Active Directory (Azure AD) integrieren.

Die Integration von JDA Cloud in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf JDA Cloud hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei JDA Cloud anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit JDA Cloud konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein JDA Cloud-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von JDA Cloud aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-jda-cloud-from-the-gallery"></a>Hinzufügen von JDA Cloud aus dem Katalog

Zum Konfigurieren der Integration von JDA Cloud in Azure AD müssen Sie JDA Cloud aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um JDA Cloud aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **JDA Cloud** ein, wählen Sie im Ergebnisbereich **JDA Cloud** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![JDA Cloud in der Ergebnisliste](./media/jdacloud-tutorial/tutorial_jdacloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden mit Azure AD bei JDA Cloud.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in JDA Cloud als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in JDA Cloud muss eine Verknüpfungsbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens mit Azure AD bei JDA Cloud müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines JDA Cloud-Testbenutzers](#create-a-jda-cloud-test-user)**, um eine Entsprechung von Britta Simon in JDA Cloud zu erhalten, die mit ihrer Benutzerdarstellung in Azure AD verknüpft ist
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer JDA Cloud-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens mit Azure AD bei JDA Cloud die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **JDA Cloud** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Dialogfeld „Einmaliges Anmelden“](./media/jdacloud-tutorial/tutorial_jdacloud_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für JDA Cloud** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für JDA Cloud](./media/jdacloud-tutorial/tutorial_jdacloud_url1.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<SUBDOMAIN>.jdadelivers.com`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<SUBDOMAIN>.jdadelivers.com/sp/ACS.saml2`

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für JDA Cloud](./media/jdacloud-tutorial/tutorial_jdacloud_url2.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://ssonp-dl2.jdadelivers.com/sp/startSSO.ping?PartnerIdpId=<SAML Entity ID>`.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner, die Antwort-URL und die Anmelde-URL. Sie finden den Wert für **SAML-Entitäts-ID** im Abschnitt **Quick Reference** (Kurzübersicht) unter dem Abschnitt **JDA Cloud Configuration** (JDA Cloudkonfiguration). Wenden Sie sich an das [Clientsupportteam von JDA Cloud](https://support.jda.com/), um diese Werte zu erhalten.

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/jdacloud-tutorial/tutorial_jdacloud_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/jdacloud-tutorial/tutorial_general_400.png)

7. Klicken Sie im Abschnitt **JDA Cloud-Konfiguration** auf **JDA Cloud konfigurieren**, um das Fenster **Anmelden konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID** aus dem Abschnitt mit der **Kurzübersicht**.

    ![Configure single sign-on](./media/jdacloud-tutorial/tutorial_jdacloud_configure.png)

8. Zum Konfigurieren des einmaligen Anmeldens bei **JDA Cloud** müssen Sie die heruntergeladene **Metadaten-XML**-Datei an das [JDA Cloud-Supportteam](https://support.jda.com/) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/jdacloud-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/jdacloud-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/jdacloud-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/jdacloud-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="create-a-jda-cloud-test-user"></a>Erstellen eines JDA Cloud-Testbenutzers

In diesem Abschnitt erstellen Sie in JDA Cloud einen Benutzer namens Britta Simon. Lassen Sie sich beim Hinzufügen der Benutzer auf der JDA Cloud-Plattform ggf. vom [JDA Cloud-Supportteam](https://support.jda.com/) unterstützen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie Zugriff auf JDA Cloud gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um Britta Simon JDA Cloud zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **JDA Cloud** aus.

    ![JDA Cloud-Link in der Anwendungsliste](./media/jdacloud-tutorial/tutorial_jdacloud_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „JDA Cloud“ klicken, sollten Sie automatisch bei Ihrer JDA Cloud-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/jdacloud-tutorial/tutorial_general_01.png
[2]: ./media/jdacloud-tutorial/tutorial_general_02.png
[3]: ./media/jdacloud-tutorial/tutorial_general_03.png
[4]: ./media/jdacloud-tutorial/tutorial_general_04.png

[100]: ./media/jdacloud-tutorial/tutorial_general_100.png

[200]: ./media/jdacloud-tutorial/tutorial_general_200.png
[201]: ./media/jdacloud-tutorial/tutorial_general_201.png
[202]: ./media/jdacloud-tutorial/tutorial_general_202.png
[203]: ./media/jdacloud-tutorial/tutorial_general_203.png