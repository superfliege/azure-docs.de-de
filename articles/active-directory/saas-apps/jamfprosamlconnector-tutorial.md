---
title: 'Tutorial: Integration von Jamf Pro in Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Jamf Pro konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: d28e28a2c4f8144da16c4838f07c9b8bb5ce67f0
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268156"
---
# <a name="tutorial-azure-active-directory-integration-with-jamf-pro"></a>Tutorial: Integration von Jamf Pro in Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie Jamf Pro in Azure Active Directory (Azure AD) integrieren.

Die Integration von Jamf Pro in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Jamf Pro hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Jamf Pro anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Jamf Pro konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Jamf Pro-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.
Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Jamf Pro aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-jamf-pro-from-the-gallery"></a>Hinzufügen von Jamf Pro aus dem Katalog

Zum Konfigurieren der Integration von Jamf Pro in Azure AD müssen Sie Jamf Pro aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Jamf Pro aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![image](./media/jamfprosamlconnector-tutorial/selectazuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![image](./media/jamfprosamlconnector-tutorial/a_select_app.png)
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![image](./media/jamfprosamlconnector-tutorial/a_new_app.png)

4. Geben Sie im Suchfeld **Jamf Pro** ein, wählen Sie im Ergebnisbereich **Jamf Pro** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![image](./media/jamfprosamlconnector-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Jamf Pro mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Jamf Pro als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Jamf Pro muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Jamf Pro müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Jamf Pro-Testbenutzers](#create-a-jamf-pro-test-user)**, um ein Pendant von Britta Simon in Jamf Pro zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Jamf Pro-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Jamf Pro die folgenden Schritte aus:**

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Jamf Pro** die Option **Einmaliges Anmelden** aus.

    ![image](./media/jamfprosamlconnector-tutorial/b1_b2_select_sso.png)

2. Klicken Sie im oberen Bereich des Bildschirms auf **Modus für einmaliges Anmelden ändern**, um den Modus **SAML** auszuwählen.

      ![image](./media/jamfprosamlconnector-tutorial/b1_b2_saml_ssso.png)

3. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![image](./media/jamfprosamlconnector-tutorial/b1_b2_saml_sso.png)

4. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![image](./media/jamfprosamlconnector-tutorial/b1-domains_and_urlsedit.png)

5. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://<subdomain>.jamfcloud.com/saml/metadata`.

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<subdomain>.jamfcloud.com/saml/SSO`.

    ![image](./media/jamfprosamlconnector-tutorial//b2-domains_and_urls.png)

    c. Klicken Sie auf **Zusätzliche URLs festlegen**.

    d. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.jamfcloud.com`.

    ![image](./media/jamfprosamlconnector-tutorial//b4-domains_and_urls.png)

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner, die Antwort-URL und die Anmelde-URL. Sie erhalten den tatsächlichen Wert des Bezeichners im Abschnitt **Single Sign-On** (Einmaliges Anmelden) des Jamf Pro-Portals, der später in diesem Tutorial beschrieben wird. Sie können den tatsächlichen Wert **subdomain** aus dem Bezeichnerwert extrahieren und diese **subdomain**-Informationen in Anmelde-URL und Antwort-URL verwenden.

6. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf die Schaltfläche „Kopieren“, um die **App-Verbundmetadaten-URL** zu kopieren, und speichern Sie sie auf Ihrem Computer.

    ![image](./media/jamfprosamlconnector-tutorial/C2_certificate.png)

7. Wenn Sie die Konfiguration in Jamf Pro automatisieren möchten, müssen Sie die **Browsererweiterung „Meine Apps“ für die sichere Anmeldung** installieren, indem Sie auf **Erweiterung installieren** klicken.

    ![image](./media/jamfprosamlconnector-tutorial/install_extension.png)
 
8. Klicken Sie nach dem Hinzufügen der Erweiterung zum Browser auf **Jamf Pro einrichten**, um zur Anwendung Jamf Pro weitergeleitet zu werden. Geben Sie dort die Administratoranmeldeinformationen ein, um sich bei Jamf Pro anzumelden. Die Browsererweiterung konfiguriert die Anwendung automatisch für Sie und automatisiert die Schritte 9 bis 12.

    ![image](./media/jamfprosamlconnector-tutorial/d1_saml.png)

9. Wenn Sie Jamf Pro manuell einrichten möchten, öffnen Sie ein neues Webbrowserfenster, melden Sie sich bei der Jamf Pro-Unternehmenswebsite als Administrator an, und führen Sie die folgenden Schritte aus:

10. Klicken Sie in der oberen rechten Ecke der Seite auf das **Einstellungssymbol**.

    ![Jamf Pro-Konfiguration](./media/jamfprosamlconnector-tutorial/configure1.png)

11. Klicken Sie auf **Single Sign On** (Einmaliges Anmelden).

    ![Jamf Pro-Konfiguration](./media/jamfprosamlconnector-tutorial/configure2.png)

12. Führen Sie auf der Seite **Single Sign-On** (Einmaliges Anmelden) die folgenden Schritte aus:

    ![Jamf Pro – einmaliges Anmelden](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_single.png)

    a. Wählen Sie **Jamf Pro Server** aus, um den Zugriff per einmaligem Anmelden zu aktivieren.

    b. Wenn Sie **Allow bypass for all users** (Umgehung für alle Benutzer zulassen) auswählen, werden Benutzer nicht zur Authentifizierung an die Anmeldeseite des Identitätsanbieters umgeleitet, sondern können sich stattdessen direkt bei Jamf Pro anmelden. Wenn ein Benutzer versucht, sich über den Identitätsanbieter auf Jamf Pro zuzugreifen, erfolgen eine IdP-initiierte SSO-Authentifizierung und -Autorisierung.

    c. Wählen Sie für **USER MAPPING: SAML** (BENUTZERZUORDNUNG: SAML) die Option **NameID** aus. Diese Einstellung ist standardmäßig auf **NameID** festgelegt, Sie können jedoch auch ein benutzerdefiniertes Attribut definieren.

    d. Wählen Sie für **USER MAPPING: JAMF PRO** (BENUTZERZUORDNUNG: JAMF PRO) die Option **Email** aus. Jamf Pro ordnet von der IdP gesendete SAML-Attribute anhand von Benutzern oder von Gruppen zu. Wenn ein Benutzer versucht, auf Jamf Pro zuzugreifen, ruft Jamf Pro standardmäßig Informationen zum Benutzer aus dem Identitätsanbieter ab und gleicht sie mit den Jamf Pro-Benutzerkonten ab. Wenn das Konto eines Benutzers in Jamf Pro nicht vorhanden ist, erfolgt ein Abgleich mit dem Gruppennamen.

    e. Fügen Sie den Wert `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` in das Textfeld **GROUP ATTRIBUTE NAME** (ATTRIBUTNAME DER GRUPPE) ein.

13. Scrollen Sie auf der gleichen Seite im Abschnitt **Single Sign-On** (Einmaliges Anmelden) zu **IDENTITY PROVIDER** (IDENTITÄTSANBIETER), und führen Sie die folgenden Schritte aus:

    ![Jamf Pro-Konfiguration](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. Wählen Sie **Other** (Sonstige) im Dropdownmenü **IDENTITY PROVIDER** (IDENTITÄTSANBIETER) aus.

    b. Geben Sie im Textfeld **OTHER PROVIDER** (Anderer Anbieter) **Azure AD** ein.

    c. Wählen Sie **Metadata URL** (Metadaten-URL) als Option in der Dropdownliste **IDENTITY PROVIDER METADATA SOURCE** (IDENTITÄTSANBIETER-METADATENQUELLE) aus, und fügen Sie in das folgende Textfeld den Wert **Verbundmetadaten-URL der App** ein, den Sie im Azure-Portal kopiert haben.

    d. Kopieren Sie den Wert der **Entity ID** (Entitäts-ID), und fügen Sie ihn im Azure-Portal im Abschnitt **Domäne und URLs für Jamf Pro** in das Textfeld **Bezeichner (Entitäts-ID)** ein.

    >[!NOTE]
    > Der hier unkenntlich gemachte Name ist die Unterdomäne. Schließen Sie mit diesem Wert die Anmelde-URL und Antwort-URL im Abschnitt **Domäne und URLs für Jamf Pro** im Azure-Portal ab.

    e. Klicken Sie auf **Speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers 

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory** aus, wählen Sie **Benutzer** aus, und wählen Sie dann **Alle Benutzer** aus.

    ![image](./media/jamfprosamlconnector-tutorial/d_users_and_groups.png)

2. Wählen Sie im oberen Bereich des Bildschirms die Option **Neuer Benutzer** aus.

    ![image](./media/jamfprosamlconnector-tutorial/d_adduser.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![image](./media/jamfprosamlconnector-tutorial/d_userproperties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** die Zeichenfolge **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

### <a name="create-a-jamf-pro-test-user"></a>Erstellen eines Jamf Pro-Testbenutzers

Damit sich Azure AD-Benutzer bei Jamf Pro anmelden können, müssen sie in Jamf Pro bereitgestellt werden. Im Fall von Jamf Pro ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der Jamf Pro-Unternehmenswebsite als Administrator an.

2. Klicken Sie in der oberen rechten Ecke der Seite auf das **Einstellungssymbol**.

    ![Mitarbeiter hinzufügen](./media/jamfprosamlconnector-tutorial/configure1.png)

3. Klicken Sie auf **Jamf Pro User Accounts & Groups** (Jamf Pro-Benutzerkonten und Gruppen).

    ![Mitarbeiter hinzufügen](./media/jamfprosamlconnector-tutorial/user1.png)

4. Klicken Sie auf **New**.

    ![Mitarbeiter hinzufügen](./media/jamfprosamlconnector-tutorial/user2.png)

5. Wählen Sie **Create Standard Account** (Standardkonto erstellen) aus.

    ![Mitarbeiter hinzufügen](./media/jamfprosamlconnector-tutorial/user3.png)

6. Führen Sie im Dialogfeld **New Account** (Neues Konto) die folgenden Schritte aus:

    ![Mitarbeiter hinzufügen](./media/jamfprosamlconnector-tutorial/user4.png)

    a. Geben Sie im Textfeld **USERNAME** (BENUTZERNAME) den vollständigen Namen von Britta Simon ein.

    b. Wählen Sie die entsprechenden Optionen gemäß Ihrer Organisation für **ACCESS LEVEL** (ZUGRIFFSEBENE), **PRIVILEGE SET** (BERECHTIGUNGENGRUPPE) und **ACCESS STATUS** (ZUGRIFFSSTATUS) ein.

    c. Geben Sie im Textfeld **FULL NAME** (VOLLSTÄNDIGER NAME) den vollständigen Namen von Britta Simon ein.

    d. Geben Sie im Textfeld **EMAIL ADDRESS** (E-MAIL-ADRESSE) die E-Mail-Adresse des Kontos von Britta Simon ein.

    e. Geben Sie im Textfeld **PASSWORD** (KENNWORT) das Kennwort des Benutzers ein.

    f. Geben Sie im Textfeld **VERIFY PASSWORD** (KENNWORT ÜBERPRÜFEN) das Kennwort des Benutzers ein.

    g. Klicken Sie auf **Speichern**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Jamf Pro gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** aus, wählen Sie **Alle Anwendungen** aus, und wählen Sie dann **Jamf Pro** aus.

    ![image](./media/jamfprosamlconnector-tutorial/d_all_applications.png)

2. Wählen Sie in der Anwendungsliste **Jamf Pro** aus.

    ![image](./media/jamfprosamlconnector-tutorial/d_all_proapplications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![image](./media/jamfprosamlconnector-tutorial/d_leftpaneusers.png)

4. Wählen Sie die Schaltfläche **Hinzufügen** aus, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![image](./media/jamfprosamlconnector-tutorial/d_assign_user.png)

4. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann im unteren Bereich des Bildschirms auf die Schaltfläche **Auswählen**.

5. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Jamf Pro“ klicken, sollten Sie automatisch in Ihrer Jamf Pro-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
