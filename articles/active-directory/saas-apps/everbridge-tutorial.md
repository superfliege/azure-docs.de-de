---
title: 'Tutorial: Azure Active Directory-Integration von EverBridge | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und EverBridge konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 58d7cd22-98c0-4606-9ce5-8bdb22ee8b3e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2018
ms.author: jeedes
ms.openlocfilehash: 40645db589409ac80c69f1e7595e20869b08f5d0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55194677"
---
# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a>Tutorial: Azure Active Directory-Integration von EverBridge

In diesem Tutorial erfahren Sie, wie Sie EverBridge in Azure Active Directory (Azure AD) integrieren.

Die Integration von EverBridge in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf EverBridge hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei EverBridge anzumelden (SSO, einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit EverBridge konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges EverBridge-Abonnement

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von EverBridge über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-everbridge-from-the-gallery"></a>Hinzufügen von EverBridge über den Katalog

Zum Konfigurieren der Integration von EverBridge in Azure AD müssen Sie EverBridge über den Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um EverBridge über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **EverBridge** ein, wählen Sie im Ergebnisbereich **EverBridge** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![EverBridge in der Ergebnisliste](./media/everbridge-tutorial/tutorial_everbridge_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei EverBridge mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in EverBridge als Pendant für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in EverBridge muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei EverBridge müssen die folgenden Schritte ausgeführt werden:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. **[Erstellen eines EverBridge-Testbenutzers](#creating-an-everbridge-test-user)**, um ein Pendant von Britta Simon in EverBridge zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer EverBridge-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei EverBridge die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **EverBridge** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![Configure single sign-on](common/tutorial_general_301.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Configure single sign-on](common/editconfigure.png)

    >[!NOTE]
    >Sie müssen die Anwendung ENTWEDER als Manager- ODER als Mitgliederportal auf beiden Seiten konfigurieren, d.h. im Azure-Portal und im EverBridge-Portal.

4. Um die Anwendung **EverBridge** als **EverBridge-Manager-Portal** zu konfigurieren, führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für EverBridge](./media/everbridge-tutorial/tutorial_everbridge_url.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://sso.everbridge.net/<API_Name>`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://manager.everbridge.net/saml/SSO/<API_Name>/alias/defaultAlias`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Wenden Sie sich an das [Supportteam von EverBridge](mailto:support@everbridge.com), um diese Werte zu erhalten.

5. Um die Anwendung **EverBridge** als **EverBridge-Mitgliederportal** zu konfigurieren, führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    * So konfigurieren Sie die Anwendung im **IDP-initiierten Modus**:

        ![SSO-Informationen zur Domäne und zu den URLs für EverBridge](./media/everbridge-tutorial/tutorial_everbridge_url1.png)

        * Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://sso.everbridge.net/<API_Name>/<Organization_ID>`

        * Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://member.everbridge.net/saml/SSO/<API_Name>/<Organization_ID>/alias/defaultAlias`

    * Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

        ![SSO-Informationen zur Domäne und zu den URLs für EverBridge](./media/everbridge-tutorial/tutorial_everbridge_url2.png)

        * Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://member.everbridge.net/saml/login/<API_Name>/<Organization_ID>/alias/defaultAlias?disco=true`.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächlichen Werte für Bezeichner, Antwort-URL und Anmelde-URL. Wenden Sie sich an das [Supportteam von EverBridge](mailto:support@everbridge.com), um diese Werte zu erhalten.

6. Klicken Sie auf der Seite **SAML-Signaturzertifikat** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um die **Verbundmetadaten-XML-Datei** herunterzuladen. Speichern Sie die Metadatendatei anschließend auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/everbridge-tutorial/tutorial_everbridge_certificate.png) 

7. Kopieren Sie im Abschnitt **EverBridge einrichten** die entsprechende URL gemäß Ihren Anforderungen.

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

    ![EverBridge-Konfiguration](common/configuresection.png)

8. Um das einmalige Anmelden für die Anwendung **EverBridge** als **EverBridge-Manager-Portal** zu konfigurieren, führen Sie die folgenden Schritte aus: 
 
9. Melden Sie sich in einem anderen Webbrowserfenster als Administrator von EverBridge an.

9. Klicken Sie im Menü am oberen Rand auf die Registerkarte **Einstellungen**, und wählen Sie unter **Sicherheit** die Option **Einmaliges Anmelden** aus.
   
    ![Configure single sign-on](./media/everbridge-tutorial/tutorial_everbridge_002.png)
   
    a. Geben Sie im Textfeld **Name** den Namen des Identitätsanbieters (beispielsweise den Namen Ihres Unternehmens) ein.
   
    b. Geben Sie im Textfeld **API-Name** den Namen der API ein.
   
    c. Klicken Sie auf die Schaltfläche **Datei auswählen**, um die Metadatendatei hochzuladen, die Sie aus dem Azure-Portal heruntergeladen haben.
   
    d. Wählen Sie für „Speicherort der SAML-Identität“ die Option **Identität ist im NameIdentifier-Element der Subject-Anweisung enthalten** aus.
   
    e. Fügen Sie in das Textfeld **Identity Provider Login URL** (Anmelde-URL des Identitätsanbieters) den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.
   
    f. Wählen Sie für „Vom Dienstanbieter initiierte Anforderungsbindung“ die Option **HTTP-Umleitung** aus.

    g. Klicken Sie unten auf der Seite auf **Speichern**.

10. Um das einmalige Anmelden für die Anwendung **EverBridge** als **EverBridge-Mitgliederportal** zu konfigurieren, senden Sie das heruntergeladene **Verbundmetadaten-XML-Datei** an das [EverBridge-Supportteam](mailto:support@everbridge.com). Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Azure AD-Benutzer erstellen][100]

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Erstellen eines Azure AD-Testbenutzers](common/create_aaduser_01.png) 

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Erstellen eines Azure AD-Testbenutzers](common/create_aaduser_02.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** den Namen **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
  
### <a name="creating-an-everbridge-test-user"></a>Erstellen eines EverBridge-Testbenutzers

In diesem Abschnitt erstellen Sie in Everbridge einen Benutzer namens Britta Simon. Arbeiten Sie mit dem [Supportteam von EverBridge](mailto:support@everbridge.com) zusammen, um Benutzer der EverBridge-Plattform hinzuzufügen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf EverBridge gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste den Eintrag **EverBridge**aus.

    ![Configure single sign-on](./media/everbridge-tutorial/tutorial_everbridge_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „EverBridge“ klicken, sollten Sie automatisch bei Ihrer EverBridge-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
