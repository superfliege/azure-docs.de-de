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
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: 6974350c6abbc6c5f5a8e10b22e91796e2564b08
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268058"
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
   
    A: Es gibt zwei Optionen zum Aktivieren dieses Szenarios. Benutzer können sich über die [Azure Active Directory-Einbindung](../device-management-introduction.md)bei Windows 10-Geräten anmelden. Alternativ dazu können Benutzer sich bei Windows-Geräten anmelden, die über eine Domäne mit einem lokalen Active Directory verbunden sind, das für einmaliges Anmelden bei Azure AD über eine [AD FS](../hybrid/plan-connect-user-signin.md) -Bereitstellung (Active Directory-Verbunddienste) aktiviert wurde. Für beide Optionen müssen Sie die Schritte im folgenden Tutorial ausführen, um einmaliges Anmelden zwischen Azure AD und G Suite zu aktivieren.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von G Suite aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-g-suite-from-the-gallery"></a>Hinzufügen von G Suite aus dem Katalog

Zum Konfigurieren der Integration von G Suite in Azure AD müssen Sie G Suite aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um G Suite aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![image](./media/google-apps-tutorial/selectazuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![image](./media/google-apps-tutorial/a_select_app.png)
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![image](./media/google-apps-tutorial/a_new_app.png)

4. Geben Sie **G Suite** in das Suchfeld ein, wählen Sie im Ergebnisbereich den Eintrag **G Suite** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![image](./media/google-apps-tutorial/a_add_app.png)

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

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **G Suite** **Einmaliges Anmelden** aus.

    ![image](./media/google-apps-tutorial/b1_b2_select_sso.png)

2. Klicken Sie oben im Bildschirm auf **Modus für einmaliges Anmelden ändern**, um den Modus **SAML** auszuwählen.

      ![image](./media/google-apps-tutorial/b1_b2_saml_ssso.png)

3. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![image](./media/google-apps-tutorial/b1_b2_saml_sso.png)

4. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![image](./media/google-apps-tutorial/b1-domains_and_urlsedit.png)

5. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`.

    b. Geben Sie im Textfeld **Identifier** (Bezeichner) eine URL nach folgendem Muster ein: 
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |

    ![image](./media/google-apps-tutorial/b1-domains_and_urls.png)
 
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Clientsupportteam für G Suite](https://www.google.com/contact/), um diese Werte zu erhalten.

6. Die G Suite-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Benutzerattribute** zu öffnen.

    ![image](./media/google-apps-tutorial/i3-attribute.png)

7. Konfigurieren Sie im Dialogfeld **Benutzerattribute** im Abschnitt **Benutzeransprüche** das SAML-Tokenattribut wie in der obigen Abbildung gezeigt, und führen Sie die folgenden Schritte aus:
    
    a. Klicken Sie auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    ![image](./media/google-apps-tutorial/i2-attribute.png)

    ![image](./media/google-apps-tutorial/i4-attribute.png)

    b. Wählen Sie in der Liste **Quellattribut** den Attributwert aus.

    c. Klicken Sie auf **Speichern**.

8. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![image](./media/google-apps-tutorial/certificatebase64.png)

9. Kopieren Sie im Abschnitt **G Suite einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    Beachten Sie, dass für die URL ggf. Folgendes angezeigt wird:

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

    ![image](./media/google-apps-tutorial/d1_saml.png) 

10. Öffnen Sie eine neue Registerkarte in Ihrem Browser, und melden Sie sich bei der [G Suite-Verwaltungskonsole](http://admin.google.com/) mit Ihrem Administratorkonto an.

11. Klicken Sie auf **Sicherheit**. Wenn der Link nicht angezeigt wird, kann er unter dem Menü **Weitere Steuerelemente** im unteren Bereich des Bildschirms versteckt sein.
   
    ![Klicken Sie auf "Sicherheit".][10]

12. Klicken Sie auf der Seite **Sicherheit** auf **Einmaliges Anmelden (SSO) einrichten**.
   
    ![Klicken Sie auf "SSO".][11]

13. Führen Sie die folgenden Konfigurationsänderungen durch:
   
    ![Konfigurieren von SSO][12]
   
    a. Wählen Sie **Einmaliges Anmelden mit externem Identitätsanbieter einrichten**.

    b. Fügen Sie in G Suite im Feld **URL der Anmeldeseite** den Wert von **Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Fügen Sie in G Suite im Feld **URL der Abmeldeseite** den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben. 

    d. Fügen Sie in G Suite im Feld **Kennwort-URL ändern** den Wert von **Kennwort-URL ändern** ein, den Sie aus dem Azure-Portal kopiert haben. 

    e. Laden Sie in G Suite für das **Verifizierungszertifikat** das Zertifikat hoch, das Sie aus dem Azure-Portal heruntergeladen haben.

    f. Aktivieren Sie **Domainspezifischen Aussteller verwenden**.

    g. Klicken Sie auf **Änderungen speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![image](./media/google-apps-tutorial/d_users_and_groups.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![image](./media/google-apps-tutorial/d_adduser.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![image](./media/google-apps-tutorial/d_userproperties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-g-suite-test-user"></a>Erstellen eines G-Suite-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen einer Benutzerin namens Britta Simon in G Suite Software. G Suite unterstützt die automatische Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt keine Aktion an. Wenn ein Benutzer nicht bereits in G Suite-Software vorhanden ist, wird ein neuer Benutzer erstellt, sobald Sie versuchen, auf G Suite-Software zuzugreifen.

>[!NOTE]
>Stellen Sie sicher, dass Ihr Benutzer bereits in G Suite vorhanden ist, wenn die Bereitstellung in Azure AD vor dem Testen des einmaligen Anmeldens nicht aktiviert wurde.

>[!NOTE] 
>Setzen Sie sich mit dem [Supportteam von Google](https://www.google.com/contact/) in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.



### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf G Suite gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![image](./media/google-apps-tutorial/d_all_applications.png)

2. Wählen Sie in der Anwendungsliste den Eintrag **G Suite** aus.

    ![image](./media/google-apps-tutorial/d_all_proapplications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![image](./media/google-apps-tutorial/d_leftpaneusers.png)

4. Wählen Sie die Schaltfläche **Hinzufügen** und dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![image](./media/google-apps-tutorial/d_assign_user.png)

4. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

5. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „G Suite“ klicken, sollten Sie automatisch in Ihrer G Suite-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[10]: ./media/google-apps-tutorial/gapps-security.png
[11]: ./media/google-apps-tutorial/security-gapps.png
[12]: ./media/google-apps-tutorial/gapps-sso-config.png

