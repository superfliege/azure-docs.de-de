---
title: 'Tutorial: Azure Active Directory-Integration mit Freshdesk | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Freshdesk konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2018
ms.author: jeedes
ms.openlocfilehash: b5968b83fc9beb481e2ad2c0cd44d2c284747fa1
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51010812"
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Tutorial: Azure Active Directory-Integration mit Freshdesk

In diesem Tutorial erfahren Sie, wie Sie Freshdesk in Azure Active Directory (Azure AD) integrieren.

Die Integration von Freshdesk in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf FreshDesk hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Freshdesk anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Freshdesk konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein FreshDesk-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Freshdesk aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-freshdesk-from-the-gallery"></a>Hinzufügen von Freshdesk aus dem Katalog

Zum Konfigurieren der Integration von Freshdesk in Azure AD müssen Sie Freshdesk aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Freshdesk aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **FreshDesk** ein, wählen Sie im Ergebnisbereich **FreshDesk** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![FreshDesk in der Ergebnisliste](./media/freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Freshdesk basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Freshdesk als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Freshdesk muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Freshdesk sind die folgenden Bausteine erforderlich:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. **[Erstellen eines Freshdesk-Testbenutzers](#creating-a-freshdesk-test-user)**, um eine Entsprechung von Britta Simon in Freshdesk zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer FreshDesk-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Freshdesk die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **FreshDesk** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![Configure single sign-on](common/tutorial_general_301.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Configure single sign-on](common/editconfigure.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für FreshDesk](./media/freshdesk-tutorial/tutorial_freshdesk_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im Format `https://<tenant-name>.freshdesk.com` oder einen anderen von FreshDesk vorgeschlagenen Wert an.

    b. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL im Format `https://<tenant-name>.freshdesk.com` oder einen anderen von FreshDesk vorgeschlagenen Wert an.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam von FreshDesk](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg), um diese Werte zu erhalten.

5. Die FreshDesk-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: Der Standardwert von **Benutzer-ID** lautet **user.userprincipalname**, aber **FreshDesk** erwartet, dass dieser Wert der E-Mail-Adresse des Benutzers zugeordnet ist. Hierfür können Sie das **user.mail**-Attribut aus der Liste verwenden oder den entsprechenden Attributwert gemäß der Konfiguration in Ihrer Organisation angeben.

    ![image](./media/freshdesk-tutorial/i4-attribute.png)

6. Konfigurieren Sie im Dialogfeld **Benutzerattribute und Ansprüche** im Abschnitt **Benutzeransprüche** das SAML-Tokenattribut wie in der obigen Abbildung gezeigt, und führen Sie die folgenden Schritte aus:
    
    a. Klicken Sie auf das Symbol **Bearbeiten**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    ![image](./media/freshdesk-tutorial/i2-attribute.png)

    ![image](./media/freshdesk-tutorial/i3-attribute.png)

    b. Wählen Sie in der Liste **Quellattribut** den Eintrag **user.mail** aus.

    c. Klicken Sie auf **Speichern**.

7. Klicken Sie auf der Seite **SAML-Signaturzertifikat** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das **Zertifikat (Base64)** herunterzuladen. Speichern Sie die Zertifikatsdatei anschließend auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/freshdesk-tutorial/tutorial_freshdesk_certificate.png)

8. Öffnen Sie eine **Eingabeaufforderung**, und führen Sie die folgenden Befehle aus:

    a. Geben Sie in der Eingabeaufforderung den Wert `certutil.exe -dump FreshDesk.cer` ein.

    > [!NOTE]
    > In diesem Fall ist **FreshDesk.cer** das Zertifikat, das Sie aus dem Azure-Portal heruntergeladen haben.

    b. Kopieren Sie den Wert für **Cert Hash(sha256)**, und fügen Sie ihn in Notepad ein. 

9. Kopieren Sie im Abschnitt **FreshDesk einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

    ![FreshDesk-Konfiguration](common/configuresection.png)

10. Melden Sie sich in einem anderen Webbrowserfenster bei der Freshdesk-Unternehmenswebsite als Administrator an.

11. Wählen Sie das Symbol **Einstellungen** aus, und führen Sie im Abschnitt **Sicherheit** die folgenden Schritte aus:

    ![Einmaliges Anmelden](./media/freshdesk-tutorial/IC776770.png "Einmaliges Anmelden")
  
    a. Wählen Sie für **Einmaliges Anmelden (SSO)** die Einstellung **Ein** aus.

    b. Wählen Sie **SAML-SSO**aus.

    c. Fügen Sie im Textfeld **SAML-Anmelde-URL** den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Fügen Sie im Textfeld **Abmelde-URL** den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    e. Fügen Sie im Textfeld **Fingerabdruck des Sicherheitszertifikats** den Wert für **Cert Hash(sha256)** ein, den Sie zuvor erhalten haben.
  
    f. Klicken Sie auf **Speichern**.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Azure AD-Benutzer erstellen][100]

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Erstellen eines Azure AD-Testbenutzers](common/create_aaduser_01.png) 

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Erstellen eines Azure AD-Testbenutzers](common/create_aaduser_02.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

### <a name="creating-a-freshdesk-test-user"></a>Erstellen eines Freshdesk-Testbenutzers

Damit sich Azure AD-Benutzer bei Freshdesk anmelden können, müssen sie in Freshdesk bereitgestellt werden.  
Im Fall von Freshdesk ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrem **Freshdesk** -Mandanten an.

2. Klicken Sie oben im Menü auf **Administrator**.

    ![Admin](./media/freshdesk-tutorial/IC776772.png "Admin")

3. Klicken Sie auf der Registerkarte **Allgemeine Einstellungen** auf **Agents**.
  
    ![Agents](./media/freshdesk-tutorial/IC776773.png "Agents")

4. Klicken Sie auf **Neuer Agent**.

    ![Neuer Agent](./media/freshdesk-tutorial/IC776774.png "Neuer Agent")

5. Führen Sie im Dialogfeld „Agent-Informationen“ die folgenden Schritte aus:

    ![Agent-Informationen](./media/freshdesk-tutorial/IC776775.png "Agent-Informationen")

    a. Geben Sie im Textfeld **E-Mail** die E-Mail-Adresse des Azure AD-Kontos ein, das Sie bereitstellen möchten.

    b. Geben Sie im Textfeld **Vollständiger Name** den Namen des Azure AD-Kontos ein, das Sie bereitstellen möchten.

    c. Geben Sie im Textfeld **Titel** den Titel des Azure AD-Kontos ein, das Sie bereitstellen möchten.

    d. Klicken Sie auf **Speichern**.

    >[!NOTE]
    >Der Besitzer des Azure AD-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
    >
    >[!NOTE]
    >Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von FreshDesk-Benutzerkonten oder mithilfe der von FreshDesk bereitgestellten APIs in FreshDesk bereitstellen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf FreshDesk gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Freshdesk** aus.

    ![Configure single sign-on](./media/freshdesk-tutorial/tutorial_freshdesk_app.png)

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „FreshDesk“ klicken, sollten Sie automatisch bei Ihrer FreshDesk-Anwendung angemeldet werden.
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
