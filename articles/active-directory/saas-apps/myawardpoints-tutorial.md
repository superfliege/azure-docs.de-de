---
title: 'Tutorial: Azure Active Directory-Integration in My Award Points Top Sub/Top Team | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und My Award Points Top Sub/Top Team konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a7a08eed-7a6b-4a83-8f8e-0add6d2fb8cf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: jeedes
ms.openlocfilehash: 9e3e3ff05836cb49d46a25ef3fa55d40a0bd134f
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47184559"
---
# <a name="tutorial-azure-active-directory-integration-with-my-award-points-top-subtop-team"></a>Tutorial: Azure Active Directory-Integration in My Award Points Top Sub/Top Team

In diesem Tutorial erfahren Sie, wie Sie My Award Points Top Sub/Top Team in Azure Active Directory (Azure AD) integrieren.

Die Integration von My Award Points Top Sub/Top Team in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf My Award Points Top Sub/Top Team hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei My Award Points Top Sub/Top Team anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in My Award Points Top Sub/Top Team zu konfigurieren, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein My Award Points Top Sub/Top Team-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von My Award Points Top Sub/Top Team aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-my-award-points-top-subtop-team-from-the-gallery"></a>Hinzufügen von My Award Points Top Sub/Top Team aus dem Katalog

Zum Konfigurieren der Integration von My Award Points Top Sub/Top Team in Azure AD müssen Sie My Award Points Top Sub/Top Team aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um My Award Points Top Sub/Top Team aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie **My Award Points Top Sub/Top Team** in das Suchfeld ein, wählen Sie **My Award Points Top Sub/Top Team** im Ergebnisbereich aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![My Award Points Top Sub/Top Team in der Ergebnisliste](./media/myawardpoints-tutorial/tutorial_myawardpoints_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD mit My Award Points Top Sub/Top Team.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in My Award Points Top Sub/Top Team das Gegenstück zu einem Benutzer in Azure AD ist. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in My Award Points Top Sub/Top Team muss eine Verknüpfungsbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei My Award Points Top Sub/Top Team sind folgende Bausteine erforderlich:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines My Award Points Top Sub/Top Team-Testbenutzers](#create-a-my-award-points-top-subtop-team-test-user)**, um eine Entsprechung für Britta Simon in My Award Points Top Sub/Top Team zu haben, die mit ihrer Azure AD-Darstellung verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal, und Sie konfigurieren das einmalige Anmelden in Ihrer My Award Points Top Sub/Top Team-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in My Award Points Top Sub/Top Team die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **My Award Points Top Sub/Top Team** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Dialogfeld „Einmaliges Anmelden“](./media/myawardpoints-tutorial/tutorial_myawardpoints_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für My Award Points Top Sub/Top Team** die folgenden Schritte aus:

    ![Informationen zum einmaligen Anmelden im Abschnitt „Domäne und URLs für My Award Points Top Sub/Top Team“](./media/myawardpoints-tutorial/tutorial_myawardpoints_url.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://microsoftrr.performnet.com/biwv1auth/Shibboleth.sso/Login?providerId=<SAMLENTITYID>`.

    > [!NOTE]
    > Der Wert der Anmelde-URL entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Supportteam von My Award Points Top Sub/Top Team](mailto:myawardpoints@biworldwide.com), um diesen Wert zu erhalten.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/myawardpoints-tutorial/tutorial_myawardpoints_certificate.png)

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/myawardpoints-tutorial/tutorial_general_400.png)

6. Zum Konfigurieren des einmaligen Anmeldens bei **My Award Points Top Sub/Top Team** müssen Sie die heruntergeladene **Metadaten-XML**-Datei an das [Supportteam von My Award Points Top Sub/Top Team](mailto:myawardpoints@biworldwide.com) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/myawardpoints-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/myawardpoints-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/myawardpoints-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/myawardpoints-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="create-a-my-award-points-top-subtop-team-test-user"></a>Erstellen eines My Award Points Top Sub/Top Team-Testbenutzers

In diesem Abschnitt erstellen Sie in My Award Points Top Sub/Top Team einen Benutzer namens Britta Simon. Lassen Sie sich beim Hinzufügen der Benutzer in der My Award Points Top Sub/Top Team-Plattform ggf. vom [Supportteam von My Award Points Top Sub/Top Team](mailto:myawardpoints@biworldwide.com) unterstützen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf My Award Points Top Sub/Top Team gewähren.

![Zuweisen der Benutzerrolle][200]

**Führen Sie die folgenden Schritte aus, um Britta Simon My Award Points Top Sub/Top Team zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste den Eintrag **My Award Points Top Sub/Top Team** aus.

    ![Link „My Award Points Top Sub/Top Team“ in der Anwendungsliste](./media/myawardpoints-tutorial/tutorial_myawardpoints_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „My Award Points Top Sub/Top Team“ klicken, sollten Sie automatisch bei Ihrer My Award Points Top Sub/Top Team-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/myawardpoints-tutorial/tutorial_general_01.png
[2]: ./media/myawardpoints-tutorial/tutorial_general_02.png
[3]: ./media/myawardpoints-tutorial/tutorial_general_03.png
[4]: ./media/myawardpoints-tutorial/tutorial_general_04.png

[100]: ./media/myawardpoints-tutorial/tutorial_general_100.png

[200]: ./media/myawardpoints-tutorial/tutorial_general_200.png
[201]: ./media/myawardpoints-tutorial/tutorial_general_201.png
[202]: ./media/myawardpoints-tutorial/tutorial_general_202.png
[203]: ./media/myawardpoints-tutorial/tutorial_general_203.png