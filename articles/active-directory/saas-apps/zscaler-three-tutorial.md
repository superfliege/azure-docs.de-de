---
title: 'Tutorial: Azure Active Directory-Integration mit Zscaler Three | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Zscaler Three konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f352e00d-68d3-4a77-bb92-717d055da56f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2018
ms.author: jeedes
ms.openlocfilehash: 0ef8fc2ea8b006d49dd54d638183a58bf78a5797
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2018
ms.locfileid: "49311956"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-three"></a>Tutorial: Azure Active Directory-Integration in Zscaler Three

In diesem Tutorial erfahren Sie, wie Sie Zscaler Three in Azure Active Directory (Azure AD) integrieren.

Die Integration von Zscaler Three in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Zscaler Three hat.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Zscaler Three (Single Sign-On, SSO; einmaliges Anmelden) anzumelden.
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in Zscaler Three konfigurieren zu können, ist Folgendes erforderlich:

- Ein Azure AD-Abonnement
- Ein Zscaler Three-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie hier eine einmonatige Testversion anfordern: [Testversion](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.
Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Zscaler Three aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-zscaler-three-from-the-gallery"></a>Hinzufügen von Zscaler Three aus dem Katalog

Zum Konfigurieren der Integration von Zscaler Three in Azure AD müssen Sie Zscaler Three aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Zscaler Three aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![ANWENDUNGEN][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**.

    ![ANWENDUNGEN][3]

4. Wählen Sie im Ergebnisbereich die Option **Zscaler Three** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/zscaler-three-tutorial/tutorial_zscalerthree_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Zscaler Three mithilfe einer Testbenutzerin namens „Britta Simon“.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Zscaler Three als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Zscaler Three muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Zscaler Three müssen die folgenden Schritte ausgeführt werden:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Konfigurieren von Proxyeinstellungen](#configuring-proxy-settings)** zum Konfigurieren der Proxyeinstellungen in Internet Explorer
3. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
4. **[Erstellen eines Zscaler Three-Testbenutzers](#creating-a-zscaler-three-test-user)**, um ein Pendant von Britta Simon in Zscaler Three zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
5. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
6. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Zscaler Three-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Zscaler Three die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Zscaler Three** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Configure single sign-on](./media/zscaler-three-tutorial/tutorial_general_301.png)

3. Wenn Sie aus einem anderen Modus in den **SAML**-Modus wechseln müssen, können Sie am oberen Bildschirmrand auf **Modus für einmaliges Anmelden ändern** klicken.

    ![Configure single sign-on](./media/zscaler-three-tutorial/tutorial_general_300.png)

4. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Configure single sign-on](./media/zscaler-three-tutorial/tutorial_general_302.png)

5. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![Configure single sign-on](./media/zscaler-three-tutorial/tutorial_zscalerthree_url.png)

    Geben Sie die URL in das Textfeld „Antwort-URL“ ein: `https://login.zscalerthree.net/sfc_sso`

    > [!NOTE]
    > Sie müssen den Wert mit der richtigen Anmelde-URL aktualisieren. Wenden Sie sich an das [Supportteam für den Zscaler Three-Client](https://www.zscaler.com/company/contact), um diese Werte zu erhalten.

6. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das **Zertifikat (Base64)** herunterzuladen, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.

    ![Configure single sign-on](./media/zscaler-three-tutorial/tutorial_zscalerthree_certificate.png)

8. Kopieren Sie im Abschnitt **Zscaler Three einrichten** die **Anmelde-URL**.

    ![Configure single sign-on](./media/zscaler-three-tutorial/tutorial_zscalerthree_configure.png)

9. Melden Sie sich in einem anderen Webbrowserfenster bei der Zscaler Three-Unternehmenswebsite als Administrator an.

10. Klicken Sie oben im Menü auf **Verwaltung**.

    ![Verwaltung](./media/zscaler-three-tutorial/ic800206.png "Verwaltung")

9. Klicken Sie unter **Administratoren & Rollen verwalten** auf **Benutzer & Authentifizierung verwalten**.

    ![Benutzer &amp; Authentifizierung verwalten](./media/zscaler-three-tutorial/ic800207.png "Benutzer &amp; Authentifizierung verwalten")

10. Führen Sie im Abschnitt **Auswählen von Authentifizierungsoptionen für Ihre Organisation** die folgenden Schritte aus:

    ![Authentifizierung](./media/zscaler-three-tutorial/ic800208.png "Authentifizierung")

    a. Wählen Sie **Authentifizeren mit der einmaligen Anmeldung für SAML**.

    b. Klicken Sie auf **Einzelne Parameter der einmaligen Anmeldung für SAML konfigurieren**.

11. Führen Sie auf der Dialogfeldseite **Parameter der einmaligen Anmeldung für SAML konfigurieren** die folgenden Schritte aus, und klicken Sie dann auf **Fertig**.

    ![Einmaliges Anmelden](./media/zscaler-three-tutorial/ic800209.png "des einmaligen Anmeldens")

    a. Fügen Sie den Wert der **Anmelde-URL**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **URL des SAML-Portals, zu dem Benutzer zur Authentifizierung geleitet werden**, ein.

    b. Geben Sie im Textfeld **Attribut mit Anmeldenamen** die **NameID** ein.

    c. Klicken Sie auf **Zscaler pem**, um das heruntergeladene Zertifikat hochzuladen.

    d. Wählen Sie **Automatische SAML-Bereitstellung aktivieren**.

12. Führen Sie auf der Dialogseite **Benutzerauthentifizierung konfigurieren** die folgenden Schritte aus:

    ![Verwaltung](./media/zscaler-three-tutorial/ic800210.png "Verwaltung")

    a. Klicken Sie auf **Speichern**.

    b. Klicken Sie auf **Jetzt aktivieren**.

## <a name="configuring-proxy-settings"></a>Konfigurieren von Proxyeinstellungen

### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>So konfigurieren Sie die Proxyeinstellungen in Internet Explorer:

1. Starten Sie **Internet Explorer**.

2. Wählen Sie im Menü **Extras** die Option **Internetoptionen**, um das Dialogfeld **Internetoptionen** zu öffnen.

     ![Internetoptionen](./media/zscaler-three-tutorial/ic769492.png "Internetoptionen")

3. Klicken Sie auf die Registerkarte **Verbindungen** .
  
     ![Verbindungen](./media/zscaler-three-tutorial/ic769493.png "Verbindungen")

4. Klicken Sie zum Öffnen des Dialogfelds **LAN-Einstellungen** auf **LAN-Einstellungen**.

5. Führen Sie im Abschnitt "Proxyserver" die folgenden Schritte aus:

    ![Proxyserver](./media/zscaler-three-tutorial/ic769494.png "Proxyserver")

    a. Wählen Sie **Proxyserver für LAN verwenden** aus.

    b. Geben Sie in das Textfeld "Adresse" **gateway.zscalerthree.net**ein.

    c. Geben Sie im Textfeld „Port“ **80**ein.

    d. Wählen Sie **Proxyserver für lokale Adressen umgehen**.

    e. Klicken Sie zum Schließen des Dialogfelds **Einstellungen für lokales Netzwerk** auf **OK**.

6. Klicken Sie zum Schließen des Dialogfelds **Internetoptionen** auf **OK**.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Azure AD-Benutzer erstellen][100]

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Erstellen eines Azure AD-Testbenutzers](./media/zscaler-three-tutorial/create_aaduser_01.png) 

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Erstellen eines Azure AD-Testbenutzers](./media/zscaler-three-tutorial/create_aaduser_02.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="creating-a-zscaler-three-test-user"></a>Erstellen eines Zscaler Three-Testbenutzers

Damit sich Azure AD-Benutzer bei Zscaler Three anmelden können, müssen sie in Zscaler Three bereitgestellt werden. Im Fall von Zscaler Three ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung

1. Melden Sie sich bei Ihrem **Zscaler Three** -Mandanten an.

2. Klicken Sie auf **Verwaltung**.

    ![Verwaltung](./media/zscaler-three-tutorial/ic781035.png "Verwaltung")

3. Klicken Sie auf **Benutzerverwaltung**.

     ![Hinzufügen](./media/zscaler-three-tutorial/ic781036.png "Hinzufügen")

4. Klicken Sie auf der Registerkarte **Benutzer** auf **Hinzufügen**.

    ![Hinzufügen](./media/zscaler-three-tutorial/ic781037.png "Hinzufügen")

5. Führen Sie im Abschnitt "Benutzer hinzufügen" die folgenden Schritte aus:

    ![Benutzer hinzufügen](./media/zscaler-three-tutorial/ic781038.png "Benutzer hinzufügen")

    a. Geben Sie die entsprechenden Werte in die Felder **Benutzer-ID**, **Benutzeranzeigename**, **Kennwort** und **Kennwort bestätigen** ein, und wählen Sie dann **Gruppen** und die **Abteilung** eines gültigen Azure AD-Kontos aus, das Sie bereitstellen möchten.

    b. Klicken Sie auf **Speichern**.

> [!NOTE]
> Sie können Azure AD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Zscaler Three-Benutzerkonten oder mithilfe der von Zscaler Three bereitgestellten APIs erstellen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf Zscaler Three gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Liste der Anwendungen **Zscaler Three** aus.

    ![Configure single sign-on](./media/zscaler-three-tutorial/tutorial_zscalerthree_app.png)

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202]

4. Klicken auf **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Zscaler Three“ klicken, sollten Sie automatisch bei Ihrer Zscaler Three-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/zscaler-three-tutorial/tutorial_general_01.png
[2]: ./media/zscaler-three-tutorial/tutorial_general_02.png
[3]: ./media/zscaler-three-tutorial/tutorial_general_03.png
[4]: ./media/zscaler-three-tutorial/tutorial_general_04.png

[100]: ./media/zscaler-three-tutorial/tutorial_general_100.png

[200]: ./media/zscaler-three-tutorial/tutorial_general_200.png
[201]: ./media/zscaler-three-tutorial/tutorial_general_201.png
[202]: ./media/zscaler-three-tutorial/tutorial_general_202.png
[203]: ./media/zscaler-three-tutorial/tutorial_general_203.png