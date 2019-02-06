---
title: 'Tutorial: Azure Active Directory-Integration von Supermood | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und Supermood konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: afc04efa-2eba-4e47-8ce4-b71eb293cd09
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.openlocfilehash: 54a1f4a8ef0ec4381374585df57dc62490337190
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55173087"
---
# <a name="tutorial-azure-active-directory-integration-with-supermood"></a>Tutorial: Azure Active Directory-Integration von Supermood

In diesem Tutorial erfahren Sie, wie Sie Supermood in Azure Active Directory (Azure AD) integrieren.

Die Integration von Supermood in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Supermood hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Supermood anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in Supermood konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Supermood-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Supermood aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-supermood-from-the-gallery"></a>Hinzufügen von Supermood aus dem Katalog
Zum Konfigurieren der Integration von Supermood in Azure AD müssen Sie Supermood aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**So fügen Sie Supermood aus dem Katalog hinzu**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld **Supermood** ein, wählen Sie im Ergebnisbereich **Supermood** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Supermood in der Ergebnisliste](./media/supermood-tutorial/tutorial_supermood_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Supermood anhand einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Supermood als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Supermood-Benutzer muss eine Linkbeziehung eingerichtet werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Supermood zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Erstellen eines Supermood-Testbenutzers](#create-a-supermood-test-user)**, um eine Entsprechung für Britta Simon in Supermood zu haben, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Supermood-Anwendung.

**So konfigurieren Sie das einmalige Anmelden von Azure AD mit Supermood**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Supermood** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/supermood-tutorial/tutorial_supermood_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für Supermood** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Supermood](./media/supermood-tutorial/tutorial_supermood_url.png)

    a. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**.

    b. Wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten, geben Sie in das Textfeld **Relayzustand** eine URL ein: `https://supermood.co/auth/sso/saml20`

    c. Wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten, geben Sie in das Textfeld **Anmelde-URL** eine URL ein: `https://supermood.co/app/#!/loginv2`

1. Die Supermood-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:
    
    ![Configure single sign-on](./media/supermood-tutorial/tutorial_supermood_attribute.png)

1. Konfigurieren Sie das SAML-Tokenattribut im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute**, wie im obigen Bild gezeigt, und führen Sie die folgenden Schritte aus:
    
    | Attributname | Attributwert |
    | ---------------| --------------- |    
    | firstName | user.givenname |
    | lastName | user.surname |

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Configure single sign-on](./media/supermood-tutorial/tutorial_attribute_04.png)

    ![Configure single sign-on](./media/supermood-tutorial/tutorial_attribute_05.png)
    
    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.

    d. Lassen Sie den **Namespace** leer.
    
    d. Klicken Sie auf **OK**.

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf die Schaltfläche „Kopieren“, um die **Verbundmetadaten-URL der App** zu kopieren und in Editor einzufügen.

    ![Downloadlink für das Zertifikat](./media/supermood-tutorial/tutorial_supermood_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/supermood-tutorial/tutorial_general_400.png)

1. Melden Sie sich im Supermood.co-Administratorbereich als Sicherheitsadministrator an.

1. Klicken Sie unten links auf **My account** (Mein Konto) und auf **Single Sign On (SSO)** (Einmaliges Anmelden).

    ![SSO-Zertifikat](./media/supermood-tutorial/tutorial_supermood_single.png)
1. Klicken Sie unter **Your SAML 2.0 configurations** (Meine SAML 2.0-Konfigurationen) auf **Add an SAML 2.0 configuration for an email domain** (E-Mail-Domäne eine SAML 2.0-Konfiguration hinzufügen).

    ![Hinzufügen eines Zertifikats](./media/supermood-tutorial/tutorial_supermood_add.png)

1. Führen Sie im Abschnitt **Add an SAML 2.0 configuration for an email domain** (E-Mail-Domäne eine SAML 2.0-Konfiguration hinzufügen) die folgenden Schritte aus:

    ![SAML-Zertifikat](./media/supermood-tutorial/tutorial_supermood_saml.png)

    a. Geben Sie im Textfeld **Email domain for this Identity provider** (E-Mail-Domäne dieses Identitätsanbieters) Ihre Domäne ein.

    b. Fügen Sie im Textfeld **Use a metadata URL** (Metadaten-URL verwenden) die **App-Verbundmetadaten-URL** ein, die Sie aus dem Azure-Portal kopiert haben.

    c. Klicken Sie auf **Hinzufügen**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/supermood-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/supermood-tutorial/create_aaduser_02.png)

1. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/supermood-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/supermood-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-supermood-test-user"></a>Erstellen eines Supermood-Testbenutzers

In diesem Abschnitt erstellen Sie in Supermood eine Benutzerin namens Britta Simon. Supermood unterstützt die Just-In-Time-Bereitstellung. Sie ist standardmäßig für die Benutzer aktiviert, deren E-Mail-Adressen den Domänen angehören, die Supermood bei der Konfiguration hinzugefügt werden. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf Supermood ein neuer Benutzer erstellt.

>[!Note]
>Wenn Sie einen Benutzer manuell erstellen müssen, wenden Sie sich an das [Supermood-Supportteam](mailto:hello@supermood.fr).


### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt gewähren Sie Britta Simon Zugriff auf Supermood, damit sie das einmalige Anmelden von Azure verwenden kann.

![Zuweisen der Benutzerrolle][200] 

**So weisen Sie Britta Simon Supermood zu**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **Supermood** aus.

    ![Supermood-Link in der Anwendungsliste](./media/supermood-tutorial/tutorial_supermood_app.png)  

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Supermood“ klicken, werden Sie automatisch bei Ihrer Supermood-Anwendung angemeldet.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/supermood-tutorial/tutorial_general_01.png
[2]: ./media/supermood-tutorial/tutorial_general_02.png
[3]: ./media/supermood-tutorial/tutorial_general_03.png
[4]: ./media/supermood-tutorial/tutorial_general_04.png

[100]: ./media/supermood-tutorial/tutorial_general_100.png

[200]: ./media/supermood-tutorial/tutorial_general_200.png
[201]: ./media/supermood-tutorial/tutorial_general_201.png
[202]: ./media/supermood-tutorial/tutorial_general_202.png
[203]: ./media/supermood-tutorial/tutorial_general_203.png

