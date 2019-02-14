---
title: 'Tutorial: Azure Active Directory-Integration mit EBSCO | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und EBSCO konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 144f7f65-69e9-4016-a151-fe1104fd6ba8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c0973001cda4a817ab464f825ad50deb52019f65
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56180656"
---
# <a name="tutorial-azure-active-directory-integration-with-ebsco"></a>Tutorial: Azure Active Directory-Integration mit EBSCO

In diesem Tutorial erfahren Sie, wie Sie EBSCO in Azure Active Directory (Azure AD) integrieren.

Die Integration von EBSCO in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf EBSCO hat.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei EBSCO anzumelden (Sigle Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration mit EBSCO benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein EBSCO-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von EBSCO aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-ebsco-from-the-gallery"></a>Hinzufügen von EBSCO aus dem Katalog
Zum Konfigurieren der Integration von EBSCO in Azure AD müssen Sie EBSCO aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um EBSCO aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld **EBSCO** ein, wählen Sie im Ergebnisbereich **EBSCO** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![EBSCO in der Ergebnisliste](./media/ebsco-tutorial/tutorial_ebsco_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei EBSCO mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in EBSCO als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in EBSCO muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei EBSCO müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Erstellen eines EBSCO-Testbenutzers](#create-an-ebsco-test-user)**, um die Bereitstellung und Personalisierung von EBSCOhost-Benutzern zu automatisieren. EBSCO unterstützt die Just-In-Time-Bereitstellung.
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer EBSCO-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in EBSCO die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **EBSCO** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/ebsco-tutorial/tutorial_ebsco_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für EBSCO** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für EBSCO](./media/ebsco-tutorial/tutorial_ebsco_url.png)

    Geben Sie im Textfeld **Bezeichner** eine URL ein: `pingsso.ebscohost.com`

1. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für EBSCO](./media/ebsco-tutorial/tutorial_ebsco_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `http://search.ebscohost.com/login.aspx?authtype=sso&custid=<unique EBSCO customer ID>&profile=<profile ID>`.
     
    > [!NOTE] 
    > Der Wert der Anmelde-URL entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Clientsupportteam von EBSCO](mailto:sso@ebsco.com), um den Wert zu erhalten. 

    o   **Eindeutige Elemente:**  

    o   **Custid** – Geben Sie die eindeutige EBSCO-Kunden-ID ein. 

    o   **Profile** – Clients können den Link, mit dem Benutzer zu einem bestimmten Profil umgeleitet werden (je nachdem, was sie von EBSCO erwerben), anpassen. Sie können eine bestimmte Profil-ID eingeben. Die Haupt-IDs sind eds (EBSCO Discovery Service) und ehost (EBSOCOhost-Datenbanken). Die zugehörigen Anweisungen finden Sie [hier](https://help.ebsco.com/interfaces/EBSCOhost/EBSCOhost_FAQs/How_do_I_set_up_direct_links_to_EBSCOhost_profiles_and_or_databases#profile).

1. Die EBSCO-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:
    
    ![Configure single sign-on](./media/ebsco-tutorial/tutorial_ebsco_attribute.png)

    > [!Note]
    > Das **name**-Attribut ist obligatorisch. Es wird dem **Benutzerbezeichner** in der EBSCO-Anwendung zugeordnet. Es wird standardmäßig hinzugefügt, sodass Sie es nicht manuell hinzufügen müssen.
    
1. Konfigurieren Sie das SAML-Tokenattribut im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute**, wie im obigen Bild gezeigt, und führen Sie die folgenden Schritte aus:
    
    | Attributname | Attributwert |
    | ---------------| --------------- |    
    | FirstName   | user.givenname |
    | Nachname   | user.surname |
    | E-Mail   | user.mail |

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Configure single sign-on](./media/ebsco-tutorial/tutorial_officespace_04.png)

    ![Configure single sign-on](./media/ebsco-tutorial/tutorial_attribute_05.png)
    
    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.
    
    d. Klicken Sie auf **OK**.

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/ebsco-tutorial/tutorial_ebsco_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/ebsco-tutorial/tutorial_general_400.png)
    
1. Zum Konfigurieren des einmaligen Anmeldens bei **EBSCO** müssen Sie die heruntergeladene **Metadaten-XML**-Datei an das [EBSCO-Supportteam](mailto:sso@ebsco.com) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Dokumentation zu eingebettetem Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/ebsco-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/ebsco-tutorial/create_aaduser_02.png)

1. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/ebsco-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/ebsco-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-an-ebsco-test-user"></a>Erstellen eines EBSCO-Testbenutzers

Bei EBSCO erfolgt die Benutzerbereitstellung automatisch.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

Azure AD übermittelt die erforderlichen Daten an die EBSCO-Anwendung. Die Benutzerbereitstellung von EBSCO kann automatisch oder einmalig erfolgen. Dies hängt davon ab, ob der Client über viele bereits vorhandene EBSCOhost-Konten verfügt, in denen persönliche Einstellungen gespeichert sind. Dies kann während der Implementierung mit dem [EBSCO-Supportteam](mailto:sso@ebsco.com) erörtert werden. Der Client muss in keinem der beiden Fälle vor dem Testen EBSCOhost-Konten erstellen.

   >[!Note]
   >Sie können die Bereitstellung und Personalisierung von EBSCOhost-Benutzern automatisieren. Wenden Sie sich mit Fragen zur Just-In-Time-Benutzerbereitstellung an das [EBSCO-Supportteam](mailto:sso@ebsco.com). 
 
### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf EBSCO gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um Britta Simon EBSCO zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste die Option **EBSCO**aus.

    ![EBSCO-Link in der Anwendungsliste](./media/ebsco-tutorial/tutorial_ebsco_app.png)  

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

1. Wenn Sie im Zugriffsbereich auf die Kachel „EBSCO“ klicken, sollten Sie automatisch bei Ihrer EBSCO-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

1. Klicken Sie nach der Anmeldung bei der Anwendung in der rechten oberen Ecke auf die Schaltfläche **Anmelden**.

    ![EBSCO-Anmeldung in der Anwendungsliste](./media/ebsco-tutorial/tutorial_ebsco_signin.png)
 
1. Sie erhalten einmalig eine Aufforderung zum Koppeln der institutionellen/SAML-Anmeldung, die entweder **Link your existing MyEBSCOhost account to your institution account now** (Verknüpfen Sie Ihr vorhandenes MyEBSCOhost-Konto jetzt mit Ihrem Unternehmenskonto) ODER **Create a new MyEBSCOhost account and link it to your institution account** (Erstellen Sie ein neues MyEBSCOhost-Konto, und verknüpfen Sie es mit Ihrem Unternehmenskonto) lautet. Das Konto wird für die Personalisierung in der EBSCOhost-Anwendung verwendet. Wählen Sie die Option **Create a new account** (Neues Konto erstellen) aus. Daraufhin wird das Formular für die Personalisierung wie im folgenden Screenshot gezeigt vorab mit den Werten aus der SAML-Antwort aufgefüllt. Klicken Sie auf **Weiter**, um diese Auswahl zu speichern.
    
     ![Der EBSCO-Benutzer in der Anwendungsliste](./media/ebsco-tutorial/tutorial_ebsco_user.png)

1. Löschen Sie nach Abschluss der oben beschriebenen Einrichtung Ihre Cookies und den Cache, und melden Sie sich erneut an. Sie müssen sich nicht erneut manuell anmelden, und die Personalisierungseinstellungen werden angewendet.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/ebsco-tutorial/tutorial_general_01.png
[2]: ./media/ebsco-tutorial/tutorial_general_02.png
[3]: ./media/ebsco-tutorial/tutorial_general_03.png
[4]: ./media/ebsco-tutorial/tutorial_general_04.png

[100]: ./media/ebsco-tutorial/tutorial_general_100.png

[200]: ./media/ebsco-tutorial/tutorial_general_200.png
[201]: ./media/ebsco-tutorial/tutorial_general_201.png
[202]: ./media/ebsco-tutorial/tutorial_general_202.png
[203]: ./media/ebsco-tutorial/tutorial_general_203.png

