---
title: 'Tutorial: Azure Active Directory-Integration mit Zendesk | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Zendesk konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: 37d20eabfc8fb883cda346dc8b55a17b8b959218
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268173"
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Tutorial: Azure Active Directory-Integration mit Zendesk

In diesem Tutorial erfahren Sie, wie Sie Zendesk in Azure Active Directory (Azure AD) integrieren.

Die Integration von Zendesk in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Zendesk hat.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Zendesk anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Zendesk konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Zendesk-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.
Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Zendesk aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-zendesk-from-the-gallery"></a>Hinzufügen von Zendesk aus dem Katalog

Zum Konfigurieren der Integration von Zendesk in Azure AD müssen Sie Zendesk aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Zendesk aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![image](./media/zendesk-tutorial/selectazuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![image](./media/zendesk-tutorial/a_select_app.png)
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![image](./media/zendesk-tutorial/a_new_app.png)

4. Geben Sie in das Suchfeld den Namen **Zendesk** ein, wählen Sie im Ergebnisbereich den Namen **Zendesk** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![image](./media/zendesk-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Zendesk mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Zendesk als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Zendesk muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Zendesk den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Zendesk müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Zendesk-Testbenutzers](#create-a-zendesk-test-user)**, um eine Entsprechung von Britta Simon in Zendesk zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Zendesk-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Zendesk die folgenden Schritte aus:**

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Zendesk** die Option **Einmaliges Anmelden** aus.

    ![image](./media/zendesk-tutorial/b1_b2_select_sso.png)

2. Klicken Sie im oberen Bereich des Bildschirms auf **Modus für einmaliges Anmelden ändern**, um den Modus **SAML** auszuwählen.

      ![image](./media/zendesk-tutorial/b1_b2_saml_ssso.png)

3. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![image](./media/zendesk-tutorial/b1_b2_saml_sso.png)

4. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![image](./media/zendesk-tutorial/b1-domains_and_urlsedit.png)

5. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.zendesk.com`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `<subdomain>.zendesk.com`.

    ![image](./media/zendesk-tutorial/b1-domains_and_urls.png)

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam für Zendesk-Kunden](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise), um diese Werte zu erhalten.

6. Zendesk erwartet die SAML-Assertionen in einem bestimmten Format. Es gibt keine vorgeschriebenen SAML-Attribute, Sie können jedoch optional ein Attribut aus dem Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite hinzufügen. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Benutzerattribute** zu öffnen.

    ![image](./media/zendesk-tutorial/i4-attribute.png)

7. Konfigurieren Sie im Dialogfeld **Benutzerattribute** im Abschnitt **Benutzeransprüche** das SAML-Tokenattribut wie in der obigen Abbildung gezeigt, und führen Sie die folgenden Schritte aus:

    a. Klicken Sie auf **Neuen Anspruch hinzufügen**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    ![image](./media/zendesk-tutorial/i2-attribute.png)

    ![image](./media/zendesk-tutorial/i3-attribute.png)
    
    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Lassen Sie den **Namespace** leer.

    d. Wählen Sie „Source“ als **Attribut** aus.
    
    e. Geben Sie in der Liste **Quellattribut** den für diese Zeile angezeigten Attributwert ein.
    
    f. Klicken Sie auf **OK**.

    g. Klicken Sie auf **Speichern**.

    > [!NOTE]
    > Sie verwenden Erweiterungsattribute, um Attribute hinzuzufügen, die standardmäßig nicht in Azure AD verfügbar sind. Klicken Sie auf [User attributes that can be set in SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) (Benutzerattribute für SAML), um die vollständige Liste der SAML-Attribute abzurufen, die **Zendesk** akzeptiert.

8. Kopieren Sie im Abschnitt **SAML-Signaturzertifikat** den **Fingerabdruck**, und speichern Sie ihn auf Ihrem Computer.

    ![image](./media/zendesk-tutorial/C3_certificate.png)

    a. Wählen Sie bei Bedarf die entsprechende Option für **Signaturoption** aus.

    b. Wählen Sie bei Bedarf die entsprechende Option für **Signaturalgorithmus** aus.

    c. Klicken Sie unten auf der Seite auf **Speichern**.

9. Klicken Sie im Abschnitt **Zendesk einrichten** auf **Schrittanleitung anzeigen**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die folgenden URLs aus dem Abschnitt **Kurzübersicht**.

    Beachten Sie, dass für die URL Folgendes angezeigt werden kann:

    a. SAML-Dienst-URL für einmaliges Anmelden

    b. Entitäts-ID

    c. Abmelde-URL

    ![image](./media/zendesk-tutorial/d1_saml.png) 

10. Es gibt zwei Möglichkeiten, wie Sie Zendesk konfigurieren können – automatisch oder manuell.
  
11. Wenn Sie die Konfiguration in Zendesk automatisieren möchten, müssen Sie die **Browsererweiterung „Meine Apps“ für die sichere Anmeldung** installieren, indem Sie auf **Erweiterung installieren** klicken.

    ![image](./media/zendesk-tutorial/install_extension.png)

12. Klicken Sie nach dem Hinzufügen der Erweiterung zum Browser auf **Zendesk einrichten**, um zur Anwendung Zendesk weitergeleitet zu werden. Geben Sie dort die Administratoranmeldeinformationen ein, um sich bei Zendesk anzumelden. Die Browsererweiterung konfiguriert die Anwendung automatisch für Sie und automatisiert Schritt 13.

     ![image](./media/zendesk-tutorial/d2_saml.png) 

13. Wenn Sie Zendesk manuell einrichten möchten, öffnen Sie ein neues Webbrowserfenster, melden Sie sich bei der Zendesk-Unternehmenswebsite als Administrator an, und führen Sie die folgenden Schritte aus:

    * Klicken Sie auf **Administrator**.

    * Klicken Sie im linken Navigationsbereich auf **Einstellungen** und anschließend auf **Sicherheit**.

    * Führen Sie auf der Seite **Sicherheit** die folgenden Schritte aus:

      ![Sicherheit](././media/zendesk-tutorial/ic773089.png "Sicherheit")

      ![Einmaliges Anmelden](././media/zendesk-tutorial/ic773090.png "Einmaliges Anmelden")

      a. Klicken Sie auf die Registerkarte **Admin & Agents**.

      b. Wählen Sie **Einmaliges Anmelden (SSO) und SAML** und anschließend **SAML** aus.

      c. Fügen Sie in das Textfeld **SAML SSO URL** (SAML-Anmelde-URL) den Wert der **URL für den SAML-SSO-Dienst** ein, den Sie aus dem Azure-Portal kopiert haben.

      d. Fügen Sie in das Textfeld **Remote Logout URL** (Remoteabmelde-URL) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

      e. Fügen Sie in das Textfeld **Certificate Fingerprint** (Zertifikatsfingerabdruck) den Wert **Fingerabdruck** des Zertifikats ein, das Sie aus dem Azure-Portal kopiert haben.

      f. Klicken Sie auf **Speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers 

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory** aus, wählen Sie **Benutzer** aus, und wählen Sie dann **Alle Benutzer** aus.

    ![image](./media/zendesk-tutorial/d_users_and_groups.png)

2. Wählen Sie im oberen Bereich des Bildschirms die Option **Neuer Benutzer** aus.

    ![image](./media/zendesk-tutorial/d_adduser.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![image](./media/zendesk-tutorial/d_userproperties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** die Zeichenfolge **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

### <a name="create-a-zendesk-test-user"></a>Erstellen eines Zendesk-Testbenutzers

In diesem Abschnitt wird in Zendesk eine Benutzerin namens Britta Simon erstellt. Zendesk unterstützt die automatische Benutzerbereitstellung, die standardmäßig aktiviert ist. Weitere Details zur Konfiguration der automatischen Benutzerbereitstellung finden Sie [hier](Zendesk-provisioning-tutorial.md).

**Wenn Sie einen Benutzer manuell erstellen möchten, führen Sie die folgenden Schritte aus:**

> [!NOTE]
> **Endbenutzer**-Konten werden bei der Anmeldung automatisch bereitgestellt. **Agent**- und **Admin**-Konten müssen vor dem Anmelden manuell in **Zendesk** bereitgestellt werden.

1. Melden Sie sich bei Ihrem **Zendesk** -Mandanten an.

2. Wählen Sie die Registerkarte **Kundenliste** .

3. Wählen Sie die Registerkarte **Benutzer** aus, und klicken Sie auf **Hinzufügen**.

    ![Benutzer hinzufügen](././media/zendesk-tutorial/ic773632.png "Benutzer hinzufügen")
4. Geben Sie **Name** und **E-Mail** eines vorhandenen Azure AD-Kontos ein, das Sie bereitstellen möchten, und klicken Sie dann auf **Speichern**.

    ![Neuer Benutzer](././media/zendesk-tutorial/ic773633.png "Neuer Benutzer")

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Zendesk-Benutzerkonten oder mithilfe der von Zendesk bereitgestellten APIs erstellen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Zendesk gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![image](./media/zendesk-tutorial/d_all_applications.png)

2. Wählen Sie in der Anwendungsliste **Zendesk** aus.

    ![image](./media/zendesk-tutorial/d_all_zendeskapplications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![image](./media/zendesk-tutorial/d_leftpaneusers.png)

4. Wählen Sie die Schaltfläche **Hinzufügen** aus, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![image](./media/zendesk-tutorial/d_assign_user.png)

4. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann im unteren Bereich des Bildschirms auf die Schaltfläche **Auswählen**.

5. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Zendesk“ klicken, sollten Sie automatisch bei Ihrer Zendesk-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Konfigurieren der Benutzerbereitstellung](zendesk-provisioning-tutorial.md)
