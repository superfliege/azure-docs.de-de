---
title: 'Tutorial: Azure Active Directory-Integration von Arc Publishing - SSO | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Arc Publishing - SSO konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ae609583-f875-4cb8-b68e-1b0b7938e9a7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2018
ms.author: jeedes
ms.openlocfilehash: 8c4922c817d9667b1a25846df53f9366e2018342
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55187112"
---
# <a name="tutorial-azure-active-directory-integration-with-arc-publishing---sso"></a>Tutorial: Azure Active Directory-Integration von Arc Publishing - SSO

In diesem Tutorial erfahren Sie, wie Sie Azure Active Directory (Azure AD) in Arc Publishing - SSO integrieren.

Die Integration von Arc Publishing - SSO in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Arc Publishing - SSO hat.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Arc Publishing - SSO anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in Arc Publishing - SSO konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Arc Publishing - SSO-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Arc Publishing - SSO aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-arc-publishing---sso-from-the-gallery"></a>Hinzufügen von Arc Publishing - SSO aus dem Katalog
Zum Konfigurieren der Integration von Arc Publishing - SSO in Azure AD müssen Sie Arc Publishing - SSO aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um Arc Publishing - SSO aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld **Arc Publishing - SSO** ein, wählen Sie im Ergebnisbereich **Arc Publishing - SSO** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Arc Publishing - SSO in der Ergebnisliste](./media/arc-tutorial/tutorial_arc_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden über Azure AD bei Arc Publishing - SSO mithilfe eines Testbenutzers namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Arc Publishing - SSO als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Arc Publishing - SSO muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens mit Azure AD bei Arc Publishing - SSO müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Erstellen eines Arc Publishing - SSO-Testbenutzers](#create-an-arc-publishing---sso-test-user)**, um in Arc Publishing - SSO eine Entsprechung von Britta Simon zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Arc Publishing - SSO-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens bei Arc Publishing - SSO über Azure AD die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Arc Publishing - SSO** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/arc-tutorial/tutorial_arc_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für Arc Publishing - SSO** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Arc Publishing - SSO](./media/arc-tutorial/tutorial_arc_url.png)

    1. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://www.okta.com/saml2/service-provider/<Unique ID>`

    1. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://arcpublishing-<Customer>.okta.com/sso/saml2/<Unique ID>`

1. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Arc Publishing - SSO](./media/arc-tutorial/tutorial_arc_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://arcpublishing-<Customer>.okta.com/sso/saml2/<Unique ID>`.
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner, die Antwort-URL und die Anmelde-URL. Wenden Sie sich an das [Supportteam für Arc Publishing - SSO](mailto:inf@washpost.com), um diese Werte zu erhalten. 

1. Die Arc Publishing - SSO-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:
    
    ![Configure single sign-on](./media/arc-tutorial/tutorial_arc_attribute.png)

1. Konfigurieren Sie das SAML-Tokenattribut im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute**, wie im obigen Bild gezeigt, und führen Sie die folgenden Schritte aus:
    
    | Attributname | Attributwert |
    | ---------------| --------------- |    
    | firstName | user.givenname |
    | lastName | user.surname |
    | E-Mail | user.mail |
    | groups | user.assignedroles |

    1. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

     ![Configure single sign-on](./media/arc-tutorial/tutorial_attribute_04.png)

     ![Configure single sign-on](./media/arc-tutorial/tutorial_attribute_05.png)
    
    1. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    1. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.

    1. Lassen Sie den **Namespace** leer.
    
    1. Klicken Sie auf **OK**.

    > [!NOTE]
    > Hier wird **user.assignedroles** das Attribut **groups** zugeordnet. Dabei handelt es sich um in Azure AD erstellte benutzerdefinierte Rollen für die Zuordnung von Gruppennamen in der Anwendung. Weitere Informationen zum Erstellen von benutzerdefinierten Rollen in Azure AD finden Sie [hier](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management). 

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/arc-tutorial/tutorial_arc_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/arc-tutorial/tutorial_general_400.png)
    
1. Klicken Sie im Abschnitt **Arc Publishing - SSO-Konfiguration** auf **Arc Publishing - SSO konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Arc Publishing - SSO-Konfiguration](./media/arc-tutorial/tutorial_arc_configure.png) 

1. Zum Konfigurieren des einmaligen Anmeldens bei **Arc Publishing - SSO** müssen Sie das heruntergeladene **Zertifikat (Base64), Abmelde-URL, SAML-Entitäts-ID und SAML-Dienst-URL für einmaliges Anmelden** an das [Supportteam von Arc Publishing - SSO](mailto:inf@washpost.com) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/arc-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/arc-tutorial/create_aaduser_02.png)

1. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/arc-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/arc-tutorial/create_aaduser_04.png)

    1. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    1. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    1. Klicken Sie auf **Create**.
 
### <a name="create-an-arc-publishing---sso-test-user"></a>Erstellen eines Arc Publishing - SSO-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Arc Publishing - SSO. Arc Publishing - SSO unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf Arc Publishing - SSO ein neuer Benutzer erstellt.

>[!Note]
>Wenn Sie einen Benutzer manuell erstellen müssen, wenden Sie sich an das [Supportteam von Arc Publishing - SSO](mailto:inf@washpost.com).

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Arc Publishing - SSO gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon zu Arc Publishing - SSO zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **Arc Publishing - SSO** aus.

    ![Arc Publishing - SSO-Link in der Anwendungsliste](./media/arc-tutorial/tutorial_arc_app.png)  

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Arc Publishing - SSO“ klicken, sollten Sie automatisch bei Ihrer Arc Publishing - SSO-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/arc-tutorial/tutorial_general_01.png
[2]: ./media/arc-tutorial/tutorial_general_02.png
[3]: ./media/arc-tutorial/tutorial_general_03.png
[4]: ./media/arc-tutorial/tutorial_general_04.png

[100]: ./media/arc-tutorial/tutorial_general_100.png

[200]: ./media/arc-tutorial/tutorial_general_200.png
[201]: ./media/arc-tutorial/tutorial_general_201.png
[202]: ./media/arc-tutorial/tutorial_general_202.png
[203]: ./media/arc-tutorial/tutorial_general_203.png

