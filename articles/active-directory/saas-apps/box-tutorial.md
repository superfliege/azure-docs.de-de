---
title: 'Tutorial: Azure Active Directory-Integration mit Box | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Box konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3b565c8d-35e2-482a-b2f4-bf8fd7d8731f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/09/2018
ms.author: jeedes
ms.openlocfilehash: 535f59b1b0dc56b183c8a019d101b4fd4f1bfad6
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49116124"
---
# <a name="tutorial-azure-active-directory-integration-with-box"></a>Tutorial: Azure Active Directory-Integration mit Box

In diesem Tutorial erfahren Sie, wie Sie Box in Azure Active Directory (Azure AD) integrieren.

Die Integration von Box in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Box haben soll.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Box anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration in Box benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Box-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Box aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-box-from-the-gallery"></a>Hinzufügen von Box aus dem Katalog
Zum Konfigurieren der Integration von Box in Azure AD müssen Sie Box aus dem Katalog Ihrer Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Box aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![image](./media/box-tutorial/selectazuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![image](./media/box-tutorial/a_select_app.png)
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![image](./media/box-tutorial/a_new_app.png)

4. Geben Sie im Suchfeld **Box** ein, wählen Sie im Ergebnisbereich **Box** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![image](./media/box-tutorial/tutorial_Box_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Box anhand einer Testbenutzerin mit dem Namen „Britta Simon“.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Box als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Box muss eine Linkbeziehung hergestellt werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD mit Box müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Box-Testbenutzers](#create-a-box-test-user)**, um eine Entsprechung von Britta Simon in Box zu erhalten, die mit ihrer Benutzerdarstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Box-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens in Azure AD mit Box die folgenden Schritte aus:**

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Box** die Option **Einmaliges Anmelden** aus.

    ![image](./media/box-tutorial/b1_b2_select_sso.png)

2. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![image](./media/box-tutorial/b1_b2_saml_sso.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![image](./media/box-tutorial/b1-domains_and_urlsedit.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<SUBDOMAIN>.account.box.com`

    b. Geben Sie in das Textfeld **Bezeichner** eine URL ein:`box.net`

    ![image](./media/box-tutorial/tutorial_box_url.png)

    > [!NOTE] 
    > Der Wert der Anmelde-URL entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Supportteam von Box](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire), um diesen Wert zu erhalten.
 
5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um die **Verbundmetadaten-XML** herunterzuladen, und speichern Sie sie auf Ihrem Computer.

    ![image](./media/box-tutorial/tutorial_Box_certificate.png)

6. Um SSO für Ihre Anwendung zu konfigurieren, gehen Sie wie unter [Setting Up SSO on your own](https://community.box.com/t5/How-to-Guides-for-Admins/Setting-Up-Single-Sign-On-SSO-for-your-Enterprise/ta-p/1263#ssoonyourown) (Einrichten von SSO) beschrieben vor. 

>[!NOTE]
>Wenn Sie die SSO-Einstellungen für Ihr Box-Konto nicht konfigurieren können, müssen Sie die heruntergeladene **Verbundmetadaten-XML**-Datei an das [Box-Supportteam](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![image](./media/box-tutorial/d_users_and_groups.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![image](./media/box-tutorial/d_adduser.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![image](./media/box-tutorial/d_userproperties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-box-test-user"></a>Erstellen eines Testbenutzers in Box

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens „Britta Simon“ in Box. Box unterstützt die Just-In-Time-Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf Box ein neuer Benutzer erstellt.
>[!Note]
>Setzen Sie sich mit dem [Supportteam von Box](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Box gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![image](./media/box-tutorial/d_all_applications.png)

2. Wählen Sie in der Anwendungsliste die Option **Box**aus.

    ![image](./media/box-tutorial/tutorial_Box_app.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![image](./media/box-tutorial/d_leftpaneusers.png)

4. Wählen Sie die Schaltfläche **Hinzufügen** und dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![image](./media/box-tutorial/d_assign_user.png)

4. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

5. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel **Box** auswählen, wird die Anmeldeseite für die Anmeldung bei Ihrer Box-Anwendung angezeigt.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Konfigurieren der Benutzerbereitstellung](box-userprovisioning-tutorial.md)


