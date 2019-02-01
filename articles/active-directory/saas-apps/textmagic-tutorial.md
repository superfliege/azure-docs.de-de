---
title: 'Tutorial: Azure Active Directory-Integration mit TextMagic | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und TextMagic konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3e5b49d2-7096-46bc-a9ce-90e09177ba28
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2018
ms.author: jeedes
ms.openlocfilehash: ed5107d581c880d130901bfb31d34afb9e986635
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55190087"
---
# <a name="tutorial-azure-active-directory-integration-with-textmagic"></a>Tutorial: Azure Active Directory-Integration mit TextMagic

In diesem Tutorial erfahren Sie, wie Sie TextMagic in Azure Active Directory (Azure AD) integrieren.

Die Integration von TextMagic in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf TextMagic hat.
- Sie können es Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei TextMagic anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit TextMagic konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein TextMagic-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von TextMagic aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-textmagic-from-the-gallery"></a>Hinzufügen von TextMagic aus dem Katalog

Zum Konfigurieren der Integration von TextMagic in Azure AD müssen Sie TextMagic aus dem Katalog zu der Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um TextMagic aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **TextMagic** ein, wählen Sie im Ergebnisbereich **TextMagic** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![TextMagic in der Ergebnisliste](./media/textmagic-tutorial/tutorial_textmagic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit TextMagic mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in TextMagic als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in TextMagic muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei TextMagic müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. **[Erstellen eines TextMagic-Testbenutzers](#creating-a-textmagic-test-user)**, um eine Entsprechung von Britta Simon in TextMagic zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden in Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer TextMagic-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens in Azure AD mit TextMagic die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **TextMagic** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![Configure single sign-on](common/tutorial_general_301.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Configure single sign-on](common/editconfigure.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für TextMagic](./media/textmagic-tutorial/tutorial_textmagic_url.png)

    Geben Sie im Textfeld **Bezeichner** eine URL ein: `https://my.textmagic.com/saml/metadata`

5. Die TextMagic-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute und Ansprüche** auf der Anwendungsintegrationsseite verwalten. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Benutzerattribute und Ansprüche** zu öffnen.

    ![image](./media/textmagic-tutorial/i4-attribute.png)

6. Konfigurieren Sie im Dialogfeld **Benutzerattribute und Ansprüche** im Abschnitt **Benutzeransprüche** das SAML-Tokenattribut wie in der obigen Abbildung gezeigt, und führen Sie die folgenden Schritte aus:

    | NAME  | Quellattribut  | Namespace |
    | --------------- | --------------- | --------------- |
    | company | user.companyname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | firstName               | user.givenname |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | lastName            | user.surname |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | phone               | user.telephonenumber |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    
    a. Klicken Sie auf das **Bearbeitungssymbol**, um die Angabe für **Wert für Namensbezeichner** von **user.userprinicipalname** in **user.mail** zu ändern.

    ![TextMagic-Attribut](./media/textmagic-tutorial/tutorial_textmagic_email.png)

    b. Klicken Sie auf **Neuen Anspruch hinzufügen**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    ![image](./common/new_save_attribute.png)

    ![image](./common/new_attribute_details.png)

    c. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    d. Geben Sie den **Namespace**-Wert ein.

    e. Wählen Sie „Source“ als **Attribut** aus.

    f. Geben Sie in der Liste **Quellattribut** den für diese Zeile angezeigten Attributwert ein.

    g. Klicken Sie auf **OK**.

    h. Klicken Sie auf **Speichern**. 

7. Klicken Sie auf der Seite **SAML-Signaturzertifikat** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das **Zertifikat (Base64)** herunterzuladen. Speichern Sie die Zertifikatsdatei anschließend auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/textmagic-tutorial/tutorial_textmagic_certificate.png) 

8. Kopieren Sie im Abschnitt **TextMagic einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

    ![TextMagic-Konfiguration](common/configuresection.png)

9. Melden Sie sich in einem anderen Webbrowserfenster auf der TextMagic-Unternehmenswebsite als Administrator an.

10. Wählen Sie unter dem Benutzernamen **Kontoeinstellungen** aus.

    ![TextMagic-Konfiguration](./media/textmagic-tutorial/config1.png)

11. Klicken Sie auf die Registerkarte **Einmaliges Anmelden (SSO)**, und füllen Sie folgende Felder aus:  
    
    ![TextMagic-Konfiguration](./media/textmagic-tutorial/config2.png)

    a. Fügen Sie den  **Azure AD-Bezeichner**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld  **Identity provider entity ID:** (Entitäts-ID des Identitätsanbieters:) ein.

    b. Fügen Sie die  **Anmelde-URL**, die Sie aus dem Azure-Portal kopiert haben, in das Textfeld  **Identity provider SSO URL:** (SSO-URL des Identitätsanbieters) ein.

    c. Fügen Sie die  **Abmelde-URL**, die Sie aus dem Azure-Portal kopiert haben, in das Textfeld  **Identity provider SLO URL:** (SLO-URL des Identitätsanbieters) ein.

    d. Öffnen Sie das **Base64-codierte Zertifikat** im Editor, das Sie aus dem Azure-Portal heruntergeladen haben, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld  **Public x509 certificate:** (Öffentliches x509-Zertifikat) ein.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Azure AD-Benutzer erstellen][100]

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Erstellen eines Azure AD-Testbenutzers](common/create_aaduser_01.png) 

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Erstellen eines Azure AD-Testbenutzers](common/create_aaduser_02.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** den Namen **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
  
### <a name="creating-a-textmagic-test-user"></a>Erstellen eines TextMagic-Testbenutzers

Die Anwendung unterstützt die **Just-in-Time-Benutzerbereitstellung**. Nach der Authentifizierung werden in der Anwendung automatisch Benutzer erstellt. Sie müssen die Informationen nur einmal beim ersten Anmelden eingeben, um ein Unterkonto in Ihrem System zu aktivieren.
Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf TextMagic gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste den Eintrag **TextMagic** aus.

    ![Configure single sign-on](./media/textmagic-tutorial/tutorial_textmagic_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „TextMagic“ klicken, sollten Sie automatisch bei Ihrer TextMagic-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
