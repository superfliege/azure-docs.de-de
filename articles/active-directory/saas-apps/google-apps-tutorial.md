---
title: 'Tutorial: Azure Active Directory-Integration mit G Suite | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und G Suite konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: jeedes
ms.openlocfilehash: 8001f2d38ac80bb6c67419faa54bf834531f0332
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39439401"
---
# <a name="tutorial-azure-active-directory-integration-with-g-suite"></a>Tutorial: Azure Active Directory-Integration mit G Suite

In diesem Tutorial erfahren Sie, wie Sie G Suite in Azure Active Directory (Azure AD) integrieren.

Die Integration von G Suite in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf G Suite hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei G Suite anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit G Suite konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein G Suite-Abonnement, für das einmaliges Anmelden aktiviert ist
- Eine Google Apps-Abonnement oder ein Google Cloud Platform-Abonnement.

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
1.  **F: Unterstützt diese Integration Google Cloud Platform-SSO-Integration in Azure AD?**
    
    A: Ja. Für Google Cloud Platform und Google Apps wird dieselbe Authentifizierungsplattform verwendet. Um die Integration von GCP vorzunehmen, müssen Sie daher SSO mit Google Apps konfigurieren.


1. **F: Sind Chromebooks und andere Chrome-Geräte mit dem einmaligen Anmelden in Azure AD kompatibel?**
   
    A: Ja, Benutzer können sich mit ihren Azure AD-Anmeldeinformationen bei ihren Chromebook-Geräten anmelden. In diesem [G Suite-Supportartikel](https://support.google.com/chrome/a/answer/6060880) finden Sie Informationen darüber, warum Benutzer möglicherweise zweimal zum Eingeben der Anmeldeinformationen aufgefordert werden.

1. **F: Wenn ich einmaliges Anmelden aktiviere, können Benutzer dann ihre Azure AD-Anmeldeinformationen zum Anmelden bei Google-Produkten wie Google Classroom, GMail, Google Drive, YouTube usw. verwenden?**
   
    A: Ja, je nachdem, [welche G Suite-Komponente](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) Sie für Ihre Organisation aktivieren oder deaktivieren.

1. **F: Kann ich einmaliges Anmelden nur für einen Teil meiner G Suite-Benutzer aktivieren?**
   
    A: Nein. Wenn Sie einmaliges Anmelden aktivieren, müssen sich Ihre G Suite-Benutzer sofort mit ihren Azure AD-Anmeldeinformationen authentifizieren. Da G Suite die Verwendung mehrerer Identitätsanbieter nicht unterstützt, kann entweder Azure AD oder Google als Identitätsanbieter für Ihre G Suite-Umgebung fungieren – aber nicht beide gleichzeitig.

1. **F: Wenn ein Benutzer über Windows angemeldet ist, wird er dann automatisch bei G Suite authentifiziert, ohne dass er zur Eingabe eines Kennworts aufgefordert wird?**
   
    A: Es gibt zwei Optionen zum Aktivieren dieses Szenarios. Benutzer können sich über die [Azure Active Directory-Einbindung](../device-management-introduction.md)bei Windows 10-Geräten anmelden. Alternativ dazu können Benutzer sich bei Windows-Geräten anmelden, die über eine Domäne mit einem lokalen Active Directory verbunden sind, das für einmaliges Anmelden bei Azure AD über eine [AD FS](../connect/active-directory-aadconnect-user-signin.md) -Bereitstellung (Active Directory-Verbunddienste) aktiviert wurde. Für beide Optionen müssen Sie die Schritte im folgenden Tutorial ausführen, um einmaliges Anmelden zwischen Azure AD und G Suite zu aktivieren.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von G Suite aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-g-suite-from-the-gallery"></a>Hinzufügen von G Suite aus dem Katalog
Zum Konfigurieren der Integration von G Suite in Azure AD müssen Sie G Suite aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um G Suite aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie **G Suite** in das Suchfeld ein, wählen Sie im Ergebnisbereich den Eintrag **G Suite** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![G Suite in der Ergebnisliste](./media/google-apps-tutorial/tutorial_googleapps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei G Suite mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in G Suite als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in G Suite muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in G Suite den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Um einmaliges Anmelden in Azure AD mit G Suite zu konfigurieren und zu testen, müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Erstellen eines G Suite-Testbenutzers](#create-a-g-suite-test-user)**, um ein Pendant von Britta Simon in G Suite zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer G Suite-Anwendung.

**Um einmaliges Anmelden von Azure AD mit G Suite zu konfigurieren, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **G Suite** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Dialogfeld „Einmaliges Anmelden“](./media/google-apps-tutorial/tutorial_googleapps_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für G Suite** die folgenden Schritte aus, wenn Sie die Konfiguration für **Gmail** vornehmen möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für G Suite](./media/google-apps-tutorial/tutorial_googleapps_urlgmail.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`.

    b. Geben Sie im Textfeld **Identifier** (Bezeichner) eine URL nach folgendem Muster ein: 
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |
 
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Clientsupportteam für G Suite](https://www.google.com/contact/), um diese Werte zu erhalten.

1. Führen Sie im Abschnitt **Domäne und URLs für G Suite** die folgenden Schritte aus, wenn Sie die Konfiguration für **Google Cloud Platform** vornehmen möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für G Suite](./media/google-apps-tutorial/tutorial_googleapps_url1.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://console.cloud.google.com `.

    b. Geben Sie im Textfeld **Identifier** (Bezeichner) eine URL nach folgendem Muster ein: 
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |
    
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Clientsupportteam für G Suite](https://www.google.com/contact/), um diese Werte zu erhalten. 

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/google-apps-tutorial/tutorial_googleapps_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/google-apps-tutorial/tutorial_general_400.png)

1. Klicken Sie im Abschnitt **G Suite-Konfiguration** auf **G Suite konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, URL für den SAML-SSO-Dienst und URL für Kennwortänderung** aus dem Abschnitt **Kurzübersicht**.

    ![G Suite-Konfiguration](./media/google-apps-tutorial/tutorial_googleapps_configure.png) 

1. Öffnen Sie eine neue Registerkarte in Ihrem Browser, und melden Sie sich bei der [G Suite-Verwaltungskonsole](http://admin.google.com/) mit Ihrem Administratorkonto an.

1. Klicken Sie auf **Sicherheit**. Wenn der Link nicht angezeigt wird, kann er unter dem Menü **Weitere Steuerelemente** im unteren Bereich des Bildschirms versteckt sein.
   
    ![Klicken Sie auf "Sicherheit".][10]

1. Klicken Sie auf der Seite **Sicherheit** auf **Einmaliges Anmelden (SSO) einrichten**.
   
    ![Klicken Sie auf "SSO".][11]

1. Führen Sie die folgenden Konfigurationsänderungen durch:
   
    ![Konfigurieren von SSO][12]
   
    a. Wählen Sie **Einmaliges Anmelden mit externem Identitätsanbieter einrichten**.

    b. Fügen Sie in G Suite im Feld **URL der Anmeldeseite** den Wert von **Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Fügen Sie in G Suite im Feld **URL der Abmeldeseite** den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben. 

    d. Fügen Sie in G Suite im Feld **Kennwort-URL ändern** den Wert von **Kennwort-URL ändern** ein, den Sie aus dem Azure-Portal kopiert haben. 

    e. Laden Sie in G Suite für das **Verifizierungszertifikat** das Zertifikat hoch, das Sie aus dem Azure-Portal heruntergeladen haben.

    f. Aktivieren Sie **Domainspezifischen Aussteller verwenden**.

    g. Klicken Sie auf **Änderungen speichern**.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/google-apps-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/google-apps-tutorial/create_aaduser_02.png)

1. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/google-apps-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/google-apps-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-g-suite-test-user"></a>Erstellen eines G-Suite-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen einer Benutzerin namens Britta Simon in G Suite Software. G Suite unterstützt die automatische Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt keine Aktion an. Wenn ein Benutzer nicht bereits in G Suite-Software vorhanden ist, wird ein neuer Benutzer erstellt, sobald Sie versuchen, auf G Suite-Software zuzugreifen.

>[!NOTE] 
>Setzen Sie sich mit dem [Supportteam von Google](https://www.google.com/contact/) in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf G Suite gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon zu G Suite zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste den Eintrag **G Suite** aus.

    ![G Suite-Link in der Anwendungsliste](./media/google-apps-tutorial/tutorial_googleapps_app.png)  

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „G Suite“ klicken, sollten Sie automatisch in Ihrer G Suite-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/googleapps-tutorial/tutorial_general_01.png
[2]: ./media/googleapps-tutorial/tutorial_general_02.png
[3]: ./media/googleapps-tutorial/tutorial_general_03.png
[4]: ./media/googleapps-tutorial/tutorial_general_04.png

[100]: ./media/googleapps-tutorial/tutorial_general_100.png

[200]: ./media/googleapps-tutorial/tutorial_general_200.png
[201]: ./media/googleapps-tutorial/tutorial_general_201.png
[202]: ./media/googleapps-tutorial/tutorial_general_202.png
[203]: ./media/googleapps-tutorial/tutorial_general_203.png
[10]: ./media/googleapps-tutorial/gapps-security.png
[11]: ./media/googleapps-tutorial/security-gapps.png
[12]: ./media/googleapps-tutorial/gapps-sso-config.png

