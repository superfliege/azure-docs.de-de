---
title: 'Tutorial: Azure Active Directory-Integration in iPass SmartConnect | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und iPass SmartConnect konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: dee6d039-f9bb-49a2-a408-5ed40ef17d9f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: 5ae600142f7e90a7f6185c3a948a4174ce4c7797
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2018
ms.locfileid: "36323305"
---
# <a name="tutorial-azure-active-directory-integration-with-ipass-smartconnect"></a>Tutorial: Azure Active Directory-Integration in iPass SmartConnect

In diesem Tutorial erfahren Sie, wie Sie iPass SmartConnect in Azure Active Directory (Azure AD) integrieren.

Die Integration von iPass SmartConnect in Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer auf iPass SmartConnect Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei iPass SmartConnect anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in iPass SmartConnect konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein iPass SmartConnect-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von iPass SmartConnect aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-ipass-smartconnect-from-the-gallery"></a>Hinzufügen von iPass SmartConnect aus dem Katalog
Zum Konfigurieren der Integration von iPass SmartConnect in Azure AD müssen Sie iPass SmartConnect aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um iPass SmartConnect aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **iPass SmartConnect** ein, wählen Sie im Ergebnisbereich **iPass SmartConnect** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![iPass SmartConnect in der Ergebnisliste](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei iPass SmartConnect mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in iPass SmartConnect als Gegenpart für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in iPass SmartConnect muss eine Verknüpfungsbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei iPass SmartConnect müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines iPass SmartConnect-Testbenutzers](#create-an-ipass-smartconnect-test-user)**, um eine Entsprechung von Britta Simon in iPass SmartConnect zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer iPass SmartConnect-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei iPass SmartConnect die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **iPass SmartConnect** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Dialogfeld „Einmaliges Anmelden“](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_samlbase.png)

3. Sie müssen im Abschnitt **Domäne und URLs für iPass SmartConnect** keine Schritte ausführen, wenn Sie die Anwendung im **IdP**-initiierten Modus konfigurieren möchten.

    ![SSO-Informationen zur Domäne und zu den URLs für iPass SmartConnect](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_url1.png)

4. Aktivieren Sie „Erweiterte URL-Einstellungen anzeigen“, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP**-initiierten Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für iPass SmartConnect](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_url2.png)

    Geben Sie im Textfeld „Anmelde-URL“ eine URL wie die Folgende ein: `https://om-activation.ipass.com/ClientActivation/ssolanding.go`

5. Die iPass SmartConnect-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie für diese Anwendung die folgenden Ansprüche. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:

    ![Configure Single Sign-On](./media/ipasssmartconnect-tutorial/attribute.png)

6. Aktivieren Sie im Abschnitt **Benutzerattribute** das Kontrollkästchen **Alle weiteren Benutzerattribute anzeigen und bearbeiten**, um die Attribute zu erweitern. Führen Sie die folgenden Schritte für jedes der angezeigten Attribute aus:

    | Attributname | Attributwert | Namespacewert|
    | ---------------| --------------- |----------------|
    | firstName | user.givenname |   |
    | lastName | user.surname | |
    | E-Mail | user.userprincipalname | |
    | username | user.userprincipalname | |

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Configure Single Sign-On](./media/ipasssmartconnect-tutorial/tutorial_attribute_04.png)

    ![Configure Single Sign-On](./media/ipasssmartconnect-tutorial/tutorial_attribute_05.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.

    d. Lassen Sie den Namespacewert für diese Zeile leer.

    e. Klicken Sie auf **OK**.

7. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_certificate.png)

8. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/ipasssmartconnect-tutorial/tutorial_general_400.png)

9. Zum Konfigurieren des einmaligen Anmeldens bei **iPass SmartConnect** müssen Sie die heruntergeladene **Metadaten-XML-Datei** und Ihren **Domänennamen** an das [iPass SmartConnect-Supportteam](mailto:help@ipass.com) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/ipasssmartconnect-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/ipasssmartconnect-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/ipasssmartconnect-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/ipasssmartconnect-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="create-an-ipass-smartconnect-test-user"></a>Erstellen eines iPass SmartConnect-Testbenutzers

In diesem Abschnitt erstellen Sie in iPass SmartConnect einen Benutzer namens Britta Simon. Das [iPass SmartConnect-Supportteam](mailto:help@ipass.com) kann Sie beim Hinzufügen von Benutzern oder Domänen unterstützen, die auf der iPass SmartConnect-Plattform auf die Whitelist gesetzt werden sollen. Wird die Domäne vom Team hinzugefügt, werden Benutzer automatisch auf der iPass SmartConnect-Plattform bereitgestellt. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf iPass SmartConnect gewähren.

![Zuweisen der Benutzerrolle][200]

**Führen Sie die folgenden Schritte aus, um Britta Simon iPass SmartConnect zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **iPass SmartConnect** aus.

    ![Der iPass SmartConnect-Link in der Anwendungsliste](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

**Um die Anwendung im SP-initiierten Fluss zu testen, führen Sie die folgenden Schritte durch:**

a. Den iPass SmartConnect-Client für Windows können Sie [hier](https://om-activation.ipass.com/ClientActivation/ssolanding.go) herunterladen.

![Der iPass SmartConnect-Link in der Anwendungsliste](./media/ipasssmartconnect-tutorial/testing3.png)

b. Installieren Sie den Client, und starten Sie ihn.

c. Klicken Sie auf **Get Started** (Erste Schritte).

![Der iPass SmartConnect-Link in der Anwendungsliste](./media/ipasssmartconnect-tutorial/testing1.png) 

d. Geben Sie einen Azure-Benutzernamen mit Domäne ein. Klicken Sie auf **Continue** (Weiter). Sie werden dann zur Azure-Anmeldeseite weitergeleitet.

![Der iPass SmartConnect-Link in der Anwendungsliste](./media/ipasssmartconnect-tutorial/testing2.png) 

e. Nach erfolgreicher Authentifizierung wird die Clientaktivierung gestartet. Der Client wird aktiviert.

**Um die Anwendung im IdP-initiierten Fluss zu testen, führen Sie die folgenden Schritte durch:**

a. Melden Sie sich bei [https://myapps.microsoft.com](https://myapps.microsoft.com) an.

b. Klicken Sie auf die iPass SmartConnect-App.

c. Die SSA-Seite wird angezeigt. Klicken Sie auf **Download App for Windows** (App für Windows herunterladen), um den iPass SmartConnect-Client zu installieren.

![Der iPass SmartConnect-Link in der Anwendungsliste](./media/ipasssmartconnect-tutorial/testing4.png)

d. Wenn die Installation abgeschlossen ist und Sie die Bedingungen akzeptiert haben, nimmt der Client beim ersten Start automatisch die Aktivierung vor.

e. Wenn die Aktivierung nicht erfolgt, klicken Sie auf der SSA-Seite auf die Schaltfläche „Aktivieren“, um die Aktivierung zu initiieren.

f. Der Client wird aktiviert.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/ipasssmartconnect-tutorial/tutorial_general_01.png
[2]: ./media/ipasssmartconnect-tutorial/tutorial_general_02.png
[3]: ./media/ipasssmartconnect-tutorial/tutorial_general_03.png
[4]: ./media/ipasssmartconnect-tutorial/tutorial_general_04.png

[100]: ./media/ipasssmartconnect-tutorial/tutorial_general_100.png

[200]: ./media/ipasssmartconnect-tutorial/tutorial_general_200.png
[201]: ./media/ipasssmartconnect-tutorial/tutorial_general_201.png
[202]: ./media/ipasssmartconnect-tutorial/tutorial_general_202.png
[203]: ./media/ipasssmartconnect-tutorial/tutorial_general_203.png

