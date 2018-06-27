---
title: 'Tutorial: Azure Active Directory-Integration in Adaptive Insights | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und Adaptive Insights konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/14/2018
ms.author: jeedes
ms.openlocfilehash: 307c3cf258a74d1ddfb409f0d5b22d9e1fd6bf4b
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36213820"
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-insights"></a>Tutorial: Azure Active Directory-Integration in Adaptive Insights

In diesem Tutorial erfahren Sie, wie Sie Adaptive Insights in Azure Active Directory (Azure AD) integrieren.

Die Integration von Adaptive Insights in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Adaptive Insights hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Adaptive Insights anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in Adaptive Insights konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Adaptive Insights-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Adaptive Insights aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-adaptive-insights-from-the-gallery"></a>Hinzufügen von Adaptive Insights aus dem Katalog
Zum Konfigurieren der Integration von Adaptive Insights in Azure AD müssen Sie Adaptive Insights aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**So fügen Sie Adaptive Insights aus dem Katalog hinzu**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

4. Geben Sie im Suchfeld **Adaptive Insights** ein, wählen Sie im Ergebnisbereich **Adaptive Insights** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Adaptive Insights anhand einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Adaptive Insights als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Adaptive Insights-Benutzer muss eine Linkbeziehung eingerichtet werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Adaptive Insights zu konfigurieren und zu testen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. **[Erstellen eines Adaptive Insights-Testbenutzers](#creating-an-adaptive-insights-test-user)**, um eine Entsprechung für Britta Simon in Adaptive Insights zu haben, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Adaptive Insights-Anwendung.

**So konfigurieren Sie das einmalige Anmelden von Azure AD mit Adaptive Insights**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Adaptive Insights** auf **Einmaliges Anmelden**.

    ![Configure Single Sign-On][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Configure Single Sign-On](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Adaptive Insights** die folgenden Schritte aus:

    ![Configure Single Sign-On](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

    a. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL nach folgendem Muster ein: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    >[!NOTE]
    > Sie finden die Werte für den Bezeichner (Entitäts-ID) und die Antwort-URL auf der Adaptive Insights-Seite **SAML SSO Settings** (SAML-SSO-Einstellungen).

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Configure Single Sign-On](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_certificate.png)

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure Single Sign-On](./media/adaptivesuite-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Adaptive Insights-Konfiguration** auf **Adaptive Insights konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID und die SAML-Dienst-URL für einmaliges Anmelden** aus dem Abschnitt **Kurzübersicht**.

    ![Configure Single Sign-On](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_configure.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster auf der Adaptive Insights-Unternehmenswebsite als Administrator an.

8. Wechseln Sie zu **Administrator**.

    ![Admin](./media/adaptivesuite-tutorial/IC805644.png "Admin")

9. Klicken Sie im Abschnitt **Benutzer und Rollen** auf **SAML-SSO-Einstellungen verwalten**.

    ![SAML-SSO-Einstellungen verwalten](./media/adaptivesuite-tutorial/IC805645.png "SAML-SSO-Einstellungen verwalten")

10. Führen Sie auf der Seite **SAML SSO Settings** die folgenden Schritte aus:

    ![SAML-SSO-Einstellungen](./media/adaptivesuite-tutorial/IC805646.png "SAML-SSO-Einstellungen")

    a. Geben Sie im Textfeld **Identity provider name** einen Namen für die Konfiguration ein.

    b. Fügen Sie den Wert der **SAML-Entitäts-ID**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Identity Provider EntityDescriptor Entity ID** (EntityDescriptor-Entitäts-ID des Identitätsanbieters) ein.

    c. Fügen Sie den Wert der **SAML-Dienst-URL für einmaliges Anmelden**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Identity provider SSO URL** (SSO-URL des Identitätsanbieters) ein.

    d. Fügen Sie den Wert der **SAML-Dienst-URL für einmaliges Anmelden**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Custom logout URL** (Benutzerdefinierte Abmelde-URL) ein.

    e. Klicken Sie auf **Choose file**, um das heruntergeladene Zertifikat hochzuladen.

    f. Wählen Sie Folgendes aus:

    * Wählen Sie als **SAML user id** (SAML-Benutzer-ID) die Option **User’s Adaptive Insights user name** (Adaptive Insights-Benutzername des Benutzers) aus.

    * Wählen Sie als **SAML user id location** (SAML-Benutzer-ID-Speicherort) die **User id in NameID of Subject** (Benutzer-ID in NameID von Subject) aus.

    * Wählen Sie als **SAML NameID format** (SAML-NameID-Format) die Option **E-Mail-Adresse** aus.

    * Wählen Sie für **SAML aktivieren** die Option **Allow SAML SSO and direct Adaptive Insights login** (SAML-SSO und direkte Adaptive Insights-Anmeldung) aus.

    g. Kopieren Sie den Wert der **Adaptive Insights SSO URL** (Adaptive Insights- SSO-URL) und fügen Sie diesen im Azure-Portal im Abschnitt **Domäne und URLs für Adaptive Insights** in die Textfelder **Bezeichner (Entitäts-ID)** und **Antwort-URL** ein.

    h. Klicken Sie auf **Speichern**.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/adaptivesuite-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/adaptivesuite-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/adaptivesuite-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/adaptivesuite-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.

### <a name="creating-an-adaptive-insights-test-user"></a>Erstellen eines Adaptive Insights-Testbenutzers

Damit sich Azure AD-Benutzer bei Adaptive Insights anmelden können, müssen sie in Adaptive Insights bereitgestellt werden. Im Fall von Adaptive Insights muss die Bereitstellung manuell ausgeführt werden.

**Um die Benutzerbereitstellung zu konfigurieren, führen Sie die folgenden Schritte durch:** 

1. Melden Sie sich auf der **Adaptive Insights**-Unternehmenswebsite als Administrator an.
2. Wechseln Sie zu **Administrator**.

   ![Admin](./media/adaptivesuite-tutorial/IC805644.png "Admin")
3. Klicken Sie im Abschnitt **Benutzer und Rollen** auf **Benutzer hinzufügen**.

   ![Benutzer hinzufügen](./media/adaptivesuite-tutorial/IC805648.png "Benutzer hinzufügen")
4. Führen Sie im Abschnitt **Neuer Benutzer** die folgenden Schritte aus:

   ![Senden](./media/adaptivesuite-tutorial/IC805649.png "Senden")

   a. Geben Sie in die Textfelder **Name**, **Anmeldename**, **E-Mail** und **Kennwort** die entsprechenden Informationen eines gültigen Azure Active Directory-Benutzerkontos ein, das Sie bereitstellen möchten.

   b. Wählen Sie eine **Role**aus.

   c. Klicken Sie auf **Submit**.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mit anderen Tools zum Erstellen von Adaptive Insights-Benutzerkonten oder mit den von Adaptive Insights bereitgestellten APIs erstellen.
>

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt gewähren Sie Britta Simon Zugriff auf Adaptive Insights, damit sie das einmalige Anmelden von Azure verwenden kann.

![Benutzer zuweisen][200]

**So weisen Sie Britta Simon Adaptive Insights zu**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Adaptive Insights** aus.

    ![Configure Single Sign-On](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_app.png)

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt wird die Microsoft Azure AD-Konfiguration für einmaliges Anmelden mithilfe des Zugriffsbereichs getestet.

Wenn Sie im Zugriffsbereich auf die Kachel „Adaptive Insights“ klicken, werden Sie automatisch bei Ihrer Adaptive Insights-Anwendung angemeldet.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/adaptivesuite-tutorial/tutorial_general_01.png
[2]: ./media/adaptivesuite-tutorial/tutorial_general_02.png
[3]: ./media/adaptivesuite-tutorial/tutorial_general_03.png
[4]: ./media/adaptivesuite-tutorial/tutorial_general_04.png

[100]: ./media/adaptivesuite-tutorial/tutorial_general_100.png

[200]: ./media/adaptivesuite-tutorial/tutorial_general_200.png
[201]: ./media/adaptivesuite-tutorial/tutorial_general_201.png
[202]: ./media/adaptivesuite-tutorial/tutorial_general_202.png
[203]: ./media/adaptivesuite-tutorial/tutorial_general_203.png
