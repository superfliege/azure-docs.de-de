---
title: 'Tutorial: Azure Active Directory-Integration mit Appraisd | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Appraisd konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: db063306-4d0d-43ca-aae0-09f0426e7429
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2018
ms.author: jeedes
ms.openlocfilehash: dae9a59b89f03a50b1adaed55cd8f97c06906526
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49118196"
---
# <a name="tutorial-azure-active-directory-integration-with-appraisd"></a>Tutorial: Azure Active Directory-Integration mit Appraisd

In diesem Tutorial erfahren Sie, wie Sie Appraisd in Azure Active Directory (Azure AD) integrieren.

Die Integration von Appraisd in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Appraisd hat.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Appraisd anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Appraisd konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Appraisd-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Appraisd aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-appraisd-from-the-gallery"></a>Hinzufügen von Appraisd aus dem Katalog
Zum Konfigurieren der Integration von Appraisd in Azure AD müssen Sie Appraisd aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Appraisd aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![image](./media/appraisd-tutorial/selectazuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![image](./media/appraisd-tutorial/a_select_app.png)
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![image](./media/appraisd-tutorial/a_new_app.png)

4. Geben Sie im Suchfeld **Appraisd** ein, wählen Sie im Ergebnisbereich **Appraisd** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![image](./media/appraisd-tutorial/tutorial_appraisd_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Appraisd mithilfe einer Testbenutzerin namens „Britta Simon“.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Appraisd als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Appraisd muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Appraisd müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Appraisd-Testbenutzers](#create-an-appraisd-test-user)**, um in Appraisd ein Pendant von Britta Simon zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Appraisd-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Appraisd die folgenden Schritte aus:**

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Appraisd** die Option **Einmaliges Anmelden** aus.

    ![image](./media/appraisd-tutorial/B1_B2_Select_SSO.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML** aus, um einmaliges Anmelden zu aktivieren.

    ![image](./media/appraisd-tutorial/b1_b2_saml_sso.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![image](./media/appraisd-tutorial/b1-domains_and_urlsedit.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_url.png)

    a. Klicken Sie auf **Zusätzliche URLs festlegen**. 

    b. Geben Sie im Textfeld **Relayzustand** eine URL ein: `<TENANTCODE>`

    c. Wenn Sie die Anwendung im **SP**-initiierten Modus konfigurieren möchten, geben Sie im Textfeld **Anmelde-URL** eine URL unter Verwendung des folgenden Musters ein: `https://app.appraisd.com/saml/<TENANTCODE>`

    > [!NOTE]
    > Sie können die tatsächlichen Werte für „Anmelde-URL“ und „Relayzustand“ auf der SSO-Konfigurationsseite für Appraisd abrufen, was im weiteren Verlauf des Tutorials erläutert wird.
    
5. Die Appraisd-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Klicken Sie auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Benutzerattribute** zu öffnen.

    ![image](./media/appraisd-tutorial/i3-attribute.png)

6. Konfigurieren Sie im Dialogfeld **Benutzerattribute** im Abschnitt **Benutzeransprüche** das SAML-Tokenattribut wie in der obigen Abbildung gezeigt, und führen Sie die folgenden Schritte aus:
    
    a. Klicken Sie auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    ![image](./media/appraisd-tutorial/i2-attribute.png)

    ![image](./media/appraisd-tutorial/i4-attribute.png)

    b. Wählen Sie in der Liste **Quellattribut** den Attributwert aus.

    c. Klicken Sie auf **Speichern**. 

7. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das **Zertifikat (Base64)** herunterzuladen, und speichern Sie es auf Ihrem Computer.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_certficate.png) 

8. Kopieren Sie im Abschnitt **Appraisd einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

    ![image](./media/appraisd-tutorial/d1_samlsonfigure.png)

9. Melden Sie sich in einem anderen Webbrowserfenster als Sicherheitsadministrator bei Appraisd an.

10. Klicken Sie oben rechts auf der Seite auf das Symbol **Settings** (Einstellungen), und navigieren Sie dann zu **Configuration** (Konfiguration).

    ![image](./media/appraisd-tutorial/tutorial_appraisd_sett.png)

11. Klicken Sie im linken Bereich des Menüs auf **SAM Single Sign-On** (Einmaliges Anmelden mit SAML).

    ![image](./media/appraisd-tutorial/tutorial_appraisd_single.png)

12. Führen Sie auf der **Konfigurationsseite für einmaliges Anmelden mit SAML 2.0** die folgenden Schritte aus:
    
    ![image](./media/appraisd-tutorial/tutorial_appraisd_saml.png)

    a. Kopieren Sie den Wert für den **Standardrelayzustand**, und fügen Sie ihn in das Textfeld **Relayzustand** im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ein.

    b. Kopieren Sie den Wert für die **Vom Dienst initiierte Anmelde-URL**, und fügen Sie ihn in das Textfeld **Anmelde-URL** im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ein.

13. Scrollen Sie auf derselben Seite nach unten, und führen Sie unter **Benutzer werden identifiziert** die folgenden Schritte aus:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_identifying.png)

    a. Fügen Sie in das Textfeld **URL für einmaliges Anmelden des Identitätsanbieters** den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben, und klicken Sie auf **Speichern**.

    b. Fügen Sie in das Textfeld **Aussteller-URL des Identitätsanbieters** den Wert des **Azure AD-Bezeichners** ein, den Sie aus dem Azure-Portal kopiert haben, und klicken Sie auf **Speichern**.

    c. Öffnen Sie in Editor das Base64-codierte Zertifikat, das Sie aus dem Azure-Portal heruntergeladen haben, kopieren Sie den Inhalt, und fügen Sie ihn anschließend in das Feld **X.509-Zertifikat** ein, und klicken Sie auf **Speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![image](./media/appraisd-tutorial/d_users_and_groups.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![image](./media/appraisd-tutorial/d_adduser.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![image](./media/appraisd-tutorial/d_userproperties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-an-appraisd-test-user"></a>Erstellen eines Appraisd-Testbenutzers

Damit sich Azure AD-Benutzer bei Appraisd anmelden können, müssen sie in Appraisd bereitgestellt werden. Im Fall von Appraisd muss die Bereitstellung manuell ausgeführt werden.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei Appraisd als Sicherheitsadministrator an.

2. Klicken Sie oben rechts auf der Seite auf das Symbol **Settings** (Einstellungen), und navigieren Sie dann zu **Administration Centre**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_admin.png)

3. Klicken Sie auf der Symbolleiste am oberen Rand der Seite auf **People** (Personen), und navigieren Sie dann zu **Add a new user** (Neuen Benutzer hinzufügen).

    ![image](./media/appraisd-tutorial/tutorial_appraisd_user.png)

4. Führen Sie auf der Seite **Add a new user** (Neuen Benutzer hinzufügen) die folgenden Schritte aus:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_newuser.png)
    
    a. Geben Sie im Textfeld **First name** (Vorname) den Vornamen des Benutzers ein, z. B. **Britta**.

    b. Geben Sie im Textfeld **Last name** (Nachname) den Nachnamen des Benutzers ein, z. B. **Simon**.

    c. Geben Sie im Textfeld **Email** (E-Mail-Adresse) die E-Mail-Adresse des Benutzers ein, z.B. **Brittasimon@contoso.com**.

    d. Klicken Sie auf **Benutzer hinzufügen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf Appraisd gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![image](./media/appraisd-tutorial/d_all_applications.png)

2. Wählen Sie in der Anwendungsliste den Eintrag **Appraisd** aus.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_app.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![image](./media/appraisd-tutorial/d_leftpaneusers.png)

4. Wählen Sie die Schaltfläche **Hinzufügen** und dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![image](./media/appraisd-tutorial/d_assign_user.png)

4. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

5. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Appraisd-Kachel klicken, sollten Sie automatisch bei Ihrer Appraisd-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
