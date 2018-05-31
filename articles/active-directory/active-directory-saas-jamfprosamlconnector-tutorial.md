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
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: 488787adfbae0147c3bd0425d839e2ad8c5ed786
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2018
ms.locfileid: "32308762"
---
# <a name="tutorial-azure-active-directory-integration-with-jamf-pro"></a>Tutorial: Integration von Jamf Pro in Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie Jamf Pro in Azure Active Directory (Azure AD) integrieren.

Die Integration von Jamf Pro in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Jamf Pro hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Jamf Pro anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

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
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Jamf Pro aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-jamf-pro-from-the-gallery"></a>Hinzufügen von Jamf Pro aus dem Katalog
Zum Konfigurieren der Integration von Jamf Pro in Azure AD müssen Sie Jamf Pro aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Jamf Pro aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Jamf Pro** ein, wählen Sie im Ergebnisbereich **Jamf Pro** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Jamf Pro in der Ergebnisliste](./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_addfromgallery.png)

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

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Jamf Pro** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Jamf Pro** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![„Domäne und URLs für Jamf Pro“, Informationen zu einmaligem Anmelden](./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url.png)

    a. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL nach folgendem Muster ein: `https://<subdomain>.jamfcloud.com/saml/metadata`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<subdomain>.jamfcloud.com/saml/SSO`

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![„Domäne und URLs für Jamf Pro“, Informationen zu einmaligem Anmelden](./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.jamfcloud.com`.
     
    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner, die Antwort-URL und die Anmelde-URL. Sie erhalten den tatsächlichen Wert des Bezeichners im Abschnitt **Single Sign-On** (Einmaliges Anmelden) des Jamf Pro-Portals, der später in diesem Tutorial beschrieben wird. Sie können den tatsächlichen Wert **subdomain** aus dem Bezeichnerwert extrahieren und diese **subdomain**-Informationen in Anmelde-URL und Antwort-URL verwenden.

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf die Schaltfläche „Kopieren“, um die **Verbundmetadaten-URL der App** zu kopieren und in Editor einzufügen.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_400.png)
    
7. Melden Sie sich in einem anderen Webbrowserfenster bei der Jamf Pro-Unternehmenswebsite als Administrator an.

8. Klicken Sie in der oberen rechten Ecke der Seite auf das **Einstellungssymbol**.

    ![Jamf Pro-Konfiguration](./media/active-directory-saas-jamfprosamlconnector-tutorial/configure1.png)

9. Klicken Sie auf **Single Sign On** (Einmaliges Anmelden).

    ![Jamf Pro-Konfiguration](./media/active-directory-saas-jamfprosamlconnector-tutorial/configure2.png)

10. Scrollen Sie im Abschnitt **Single Sign-On** (Einmaliges Anmelden) nach oben zu **IDENTITY PROVIDER** (IDENTITÄTSANBIETER), und führen Sie die folgenden Schritte aus:

    ![Jamf Pro-Konfiguration](./media/active-directory-saas-jamfprosamlconnector-tutorial/configure3.png)

    a. Wählen Sie **Other** (Sonstige) im Dropdownmenü **IDENTITY PROVIDER** (IDENTITÄTSANBIETER) aus.

    b. Geben Sie im Textfeld **OTHER PROVIDER** (Anderer Anbieter) **Azure AD** ein.

    c. Wählen Sie **Metadata URL** (Metadaten-URL) als Option in der Dropdownliste **IDENTITY PROVIDER METADATA SOURCE** (IDENTITÄTSANBIETER-METADATENQUELLE) aus, und fügen Sie in das folgende Textfeld den Wert **Verbundmetadaten-URL der App** ein, den Sie im Azure-Portal kopiert haben.

    d. Kopieren Sie den Wert der **Entity ID** (Entitäts-ID), und fügen Sie ihn im Azure-Portal im Abschnitt **Domäne und URLs für Jamf Pro** in das Textfeld **Bezeichner (Entitäts-ID)** ein.

    >[!NOTE]
    > Hier ist `aadsso` der Unterdomänenteil (zu Referenzzwecken). Schließen Sie mit diesem Wert die Anmelde-URL und Antwort-URL im Abschnitt **Domäne und URLs für Jamf Pro** im Azure-Portal ab.

    e. Klicken Sie auf **Speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-jamfprosamlconnector-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-jamfprosamlconnector-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-jamfprosamlconnector-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-jamfprosamlconnector-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-jamf-pro-test-user"></a>Erstellen eines Jamf Pro-Testbenutzers

Damit sich Azure AD-Benutzer bei Jamf Pro anmelden können, müssen sie in Jamf Pro bereitgestellt werden. Im Fall von Jamf Pro ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der Jamf Pro-Unternehmenswebsite als Administrator an.

2. Klicken Sie in der oberen rechten Ecke der Seite auf das **Einstellungssymbol**.

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-jamfprosamlconnector-tutorial/configure1.png)

3. Klicken Sie auf **Jamf Pro User Accounts & Groups** (Jamf Pro-Benutzerkonten und Gruppen).

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-jamfprosamlconnector-tutorial/user1.png)

4. Klicken Sie auf **New**.

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-jamfprosamlconnector-tutorial/user2.png)

5. Wählen Sie **Create Standard Account** (Standardkonto erstellen) aus.

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-jamfprosamlconnector-tutorial/user3.png)

6. Führen Sie im Dialogfeld **New Account** (Neues Konto) die folgenden Schritte aus:

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-jamfprosamlconnector-tutorial/user4.png)

    a. Geben Sie im Textfeld **USERNAME** (BENUTZERNAME) den vollständigen Namen von Britta Simon ein.

    b. Wählen Sie die entsprechenden Optionen gemäß Ihrer Organisation für **ACCESS LEVEL** (ZUGRIFFSEBENE), **PRIVILEGE SET** (BERECHTIGUNGENGRUPPE) und **ACCESS STATUS** (ZUGRIFFSSTATUS) ein.
    
    c. Geben Sie im Textfeld **FULL NAME** (VOLLSTÄNDIGER NAME) den vollständigen Namen von Britta Simon ein.

    d. Geben Sie im Textfeld **EMAIL ADDRESS** (E-MAIL-ADRESSE) die E-Mail-Adresse des Kontos von Britta Simon ein.

    e. Geben Sie im Textfeld **PASSWORD** (KENNWORT) das Kennwort des Benutzers ein.

    f. Geben Sie im Textfeld **VERIFY PASSWORD** (KENNWORT ÜBERPRÜFEN) das Kennwort des Benutzers ein.

    g. Klicken Sie auf **Speichern**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Jamf Pro gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Jamf Pro zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Jamf Pro** aus.

    ![Jamf Pro-Link in der Anwendungsliste](./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Jamf Pro“ klicken, sollten Sie automatisch in Ihrer Jamf Pro-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_203.png

