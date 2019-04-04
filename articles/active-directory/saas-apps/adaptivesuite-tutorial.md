---
title: 'Tutorial: Azure Active Directory-Integration mit Adaptive Insights | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und Adaptive Insights konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d42c86ec262cd9d3d3db3035d252429e44c1208f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57884879"
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-insights"></a>Tutorial: Azure Active Directory-Integration mit Adaptive Insights

In diesem Tutorial erfahren Sie, wie Sie Adaptive Insights in Azure Active Directory (Azure AD) integrieren.

Die Integration von Adaptive Insights in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Adaptive Insights hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Adaptive Insights anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

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

    ![image](./media/adaptivesuite-tutorial/selectazuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![image](./media/adaptivesuite-tutorial/a_select_app.png)
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![image](./media/adaptivesuite-tutorial/a_new_app.png)

4. Geben Sie im Suchfeld **Adaptive Insights** ein, wählen Sie im Ergebnisbereich **Adaptive Insights** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![image](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Adaptive Insights anhand einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Adaptive Insights als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Adaptive Insights-Benutzer muss eine Linkbeziehung eingerichtet werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Adaptive Insights zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Adaptive Insights-Testbenutzers](#create-an-adaptive-insights-test-user)**, um eine Entsprechung für Britta Simon in Adaptive Insights zu haben, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Adaptive Insights-Anwendung.

**So konfigurieren Sie das einmalige Anmelden von Azure AD mit Adaptive Insights**

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Adaptive Insights** die Option **Einmaliges Anmelden** aus.

    ![image](./media/adaptivesuite-tutorial/B1_B2_Select_SSO.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML** aus, um einmaliges Anmelden zu aktivieren.

    ![image](./media/adaptivesuite-tutorial/b1_b2_saml_sso.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![image](./media/adaptivesuite-tutorial/b1-domains_and_urlsedit.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![image](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

    a. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL nach folgendem Muster ein: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    >[!NOTE]
    > Sie finden die Werte für den Bezeichner (Entitäts-ID) und die Antwort-URL auf der Adaptive Insights-Seite **SAML SSO Settings** (SAML-SSO-Einstellungen).
 
5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das **Zertifikat (Base64)** herunterzuladen, und speichern Sie es auf Ihrem Computer.

    ![image](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_certficate.png) 

6. Kopieren Sie im Abschnitt **Adaptive Insights einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    Beachten Sie, dass für die URL ggf. Folgendes angezeigt wird:

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

    ![image](./media/adaptivesuite-tutorial/d1_samlsonfigure.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster auf der Adaptive Insights-Unternehmenswebsite als Administrator an.

8. Wechseln Sie zu **Administrator**.

    ![Admin](./media/adaptivesuite-tutorial/IC805644.png "Admin")

9. Klicken Sie im Abschnitt **Benutzer und Rollen** auf **SAML-SSO-Einstellungen verwalten**.

    ![SAML-SSO-Einstellungen verwalten](./media/adaptivesuite-tutorial/IC805645.png "SAML-SSO-Einstellungen verwalten")

10. Führen Sie auf der Seite **SAML SSO Settings** die folgenden Schritte aus:

    ![SAML-SSO-Einstellungen](./media/adaptivesuite-tutorial/IC805646.png "SAML-SSO-Einstellungen")

    a. Geben Sie im Textfeld **Identity provider name** einen Namen für die Konfiguration ein.

    b. Fügen Sie den Wert der **Azure AD-ID**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Identity provider Entity ID** (Entitäts-ID des Identitätsanbieters) ein.

    c. Fügen Sie den Wert der **Anmelde-URL**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Identity Provider SSO URL** (SSO-URL des Identitätsanbieters) ein.

    d. Fügen Sie den Wert der **Abmelde-URL**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Custom logout URL** (Benutzerdefinierte Abmelde-URL) ein.

    e. Klicken Sie auf **Choose file**, um das heruntergeladene Zertifikat hochzuladen.

    f. Wählen Sie Folgendes aus:

    * Wählen Sie als **SAML user id** (SAML-Benutzer-ID) die Option **User’s Adaptive Insights user name** (Adaptive Insights-Benutzername des Benutzers) aus.

    * Wählen Sie als **SAML user id location** (SAML-Benutzer-ID-Speicherort) die **User id in NameID of Subject** (Benutzer-ID in NameID von Subject) aus.

    * Wählen Sie als **SAML NameID format** (SAML-NameID-Format) die Option **E-Mail-Adresse** aus.

    * Wählen Sie für **SAML aktivieren** die Option **Allow SAML SSO and direct Adaptive Insights login** (SAML-SSO und direkte Adaptive Insights-Anmeldung) aus.

    g. Kopieren Sie den Wert der **Adaptive Insights SSO URL** (Adaptive Insights- SSO-URL) und fügen Sie diesen im Azure-Portal im Abschnitt **Domäne und URLs für Adaptive Insights** in die Textfelder **Bezeichner (Entitäts-ID)** und **Antwort-URL** ein.

    h. Klicken Sie auf **Speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![image](./media/adaptivesuite-tutorial/d_users_and_groups.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![image](./media/adaptivesuite-tutorial/d_adduser.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![image](./media/adaptivesuite-tutorial/d_userproperties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** Folgendes ein: **brittasimon\@ihreunternehmensdomäne.erweiterung**.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-an-adaptive-insights-test-user"></a>Erstellen eines Adaptive Insights-Testbenutzers

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

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt gewähren Sie Britta Simon Zugriff auf Adaptive Insights, damit sie das einmalige Anmelden von Azure verwenden kann.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![image](./media/adaptivesuite-tutorial/d_all_applications.png)

2. Wählen Sie in der Anwendungsliste **Adaptive Insights** aus.

    ![image](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_app.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![image](./media/adaptivesuite-tutorial/d_leftpaneusers.png)

4. Wählen Sie die Schaltfläche **Hinzufügen** und dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![image](./media/adaptivesuite-tutorial/d_assign_user.png)

4. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

5. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Adaptive Insights“ klicken, werden Sie automatisch bei Ihrer Adaptive Insights-Anwendung angemeldet.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
