---
title: 'Tutorial: Azure Active Directory-Integration mit Phraseanet | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Phraseanet konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 078d84ce-e054-4ae1-a52e-46a94a6959a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/09/2018
ms.author: jeedes
ms.openlocfilehash: cfeadbf1d46d9e36f8619cafe29d9dd69aad6eec
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49118187"
---
# <a name="tutorial-azure-active-directory-integration-with-phraseanet"></a>Tutorial: Azure Active Directory-Integration mit Phraseanet

In diesem Tutorial erfahren Sie, wie Sie Phraseanet in Azure Active Directory (Azure AD) integrieren.

Die Integration von Phraseanet in Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Phraseanet hat.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Phraseanet anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Phraseanet konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Phraseanet-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Phraseanet aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-phraseanet-from-the-gallery"></a>Hinzufügen von Phraseanet aus dem Katalog
Zum Konfigurieren der Integration von Phraseanet in Azure AD müssen Sie Phraseanet aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Phraseanet über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![image](./media/phraseanet-tutorial/selectazuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![image](./media/phraseanet-tutorial/a_select_app.png)
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![image](./media/phraseanet-tutorial/a_new_app.png)

4. Geben Sie im Suchfeld **Phraseanet** ein, wählen Sie im Ergebnisbereich **Phraseanet** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![image](./media/phraseanet-tutorial/tutorial_phraseanet_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Phraseanet basierend auf einem Testbenutzer mit dem Namen „Britta Simon“.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Phraseanet als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Phraseanet muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Phraseanet müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Phraseanet-Testbenutzers](#create-a-phraseanet-test-user)**, um ein Pendant von Britta Simon in Phraseanet zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Phraseanet-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Phraseanet die folgenden Schritte aus:**

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Phraseanet** **Einmaliges Anmelden** aus.

    ![image](./media/phraseanet-tutorial/b1_b2_select_sso.png)

2. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![image](./media/phraseanet-tutorial/b1_b2_saml_sso.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![image](./media/phraseanet-tutorial/b1-domains_and_urlsedit.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    Geben Sie im Textfeld **Anmelde-URL** die URL wie folgt ein: `https://<SUBDOMAIN>.alchemyasp.com`

    ![image](./media/phraseanet-tutorial/tutorial_phraseanet_url.png)

    > [!NOTE] 
    > Der Wert der Anmelde-URL entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Supportteam von Phraseanet](mailto:support@alchemy.fr), um diesen Wert zu erhalten.
 
5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um die **Verbundmetadaten-XML** herunterzuladen, und speichern Sie sie auf Ihrem Computer.

    ![image](./media/phraseanet-tutorial/tutorial_phraseanet_certificate.png) 

6. Zum Konfigurieren des einmaligen Anmeldens bei **Phraseanet** müssen Sie die heruntergeladene **Verbundmetadaten-XML**-Datei an das [Phraseanet-Supportteam](mailto:support@alchemy.fr) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![image](./media/phraseanet-tutorial/d_users_and_groups.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![image](./media/phraseanet-tutorial/d_adduser.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![image](./media/phraseanet-tutorial/d_userproperties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-phraseanet-test-user"></a>Erstellen eines Phraseanet-Testbenutzers

In diesem Abschnitt erstellen Sie in Phraseanet einen Benutzer namens „Britta Simon“. Lassen Sie sich beim Hinzufügen der Benutzer zur Phraseanet-Plattform ggf. vom [Phraseanet-Supportteam](mailto:support@alchemy.fr) unterstützen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf Phraseanet gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![image](./media/phraseanet-tutorial/d_all_applications.png)

2. Wählen Sie in der Anwendungsliste **Phraseanet** aus.

    ![image](./media/phraseanet-tutorial/tutorial_phraseanet_app.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![image](./media/phraseanet-tutorial/d_leftpaneusers.png)

4. Wählen Sie die Schaltfläche **Hinzufügen** und dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![image](./media/phraseanet-tutorial/d_assign_user.png)

4. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

5. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Phraseanet“ klicken, sollten Sie automatisch bei Ihrer Phraseanet-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


