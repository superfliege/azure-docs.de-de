---
title: 'Tutorial: Azure Active Directory-Integration mit Freshdesk | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Freshdesk konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 3f640d729d002e89c4968442e8d8ab761f78d090
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49115163"
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Tutorial: Azure Active Directory-Integration mit Freshdesk

In diesem Tutorial erfahren Sie, wie Sie Freshdesk in Azure Active Directory (Azure AD) integrieren.

Die Integration von Freshdesk in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Freshdesk hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Freshdesk anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Freshdesk konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Freshdesk-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.
Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Freshdesk aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-freshdesk-from-the-gallery"></a>Hinzufügen von Freshdesk aus dem Katalog

Zum Konfigurieren der Integration von Freshdesk in Azure AD müssen Sie Freshdesk aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Freshdesk aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![ANWENDUNGEN][3]

4. Geben Sie im Suchfeld als Suchbegriff **Freshdesk** ein. Wählen Sie im Ergebnisbereich **FreshDesk** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Freshdesk basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Freshdesk als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Freshdesk muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert für die **E-Mail-Adresse** in FreshDesk zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Freshdesk sind die folgenden Bausteine erforderlich:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. **[Erstellen eines Freshdesk-Testbenutzers](#creating-a-freshdesk-test-user)**, um eine Entsprechung von Britta Simon in Freshdesk zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer FreshDesk-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Freshdesk die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **FreshDesk** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Configure single sign-on](./media/freshdesk-tutorial/tutorial_freshdesk_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für FreshDesk** die folgenden Schritte aus:

    ![Configure single sign-on](./media/freshdesk-tutorial/tutorial_freshdesk_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im Format `https://<tenant-name>.freshdesk.com` oder einen anderen von FreshDesk vorgeschlagenen Wert an.

    > [!NOTE]
    > Hinweis: Hierbei handelt es sich um einen Beispielwert. Sie müssen den Wert durch die richtige Anmelde-URL ersetzen. Wenden Sie sich an das [Supportteam von Freshdesk](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg), um diesen Wert zu erhalten.

4. Die Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: Der Standardwert von **Benutzer-ID** lautet **user.userprincipalname**, aber **FreshDesk** erwartet, dass dieser Wert der E-Mail-Adresse des Benutzers zugeordnet ist. Hierfür können Sie das **user.mail**-Attribut aus der Liste verwenden oder den entsprechenden Attributwert gemäß der Konfiguration in Ihrer Organisation angeben.

    ![Configure single sign-on](./media/freshdesk-tutorial/tutorial_attribute.png)

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Configure single sign-on](./media/freshdesk-tutorial/tutorial_freshdesk_certificate.png)

    > [!NOTE]
    > Wenn Probleme auftreten, lesen Sie die Informationen unter [diesem Link](https://support.freshdesk.com/support/discussions/topics/317543).

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/freshdesk-tutorial/tutorial_general_400.png)

7. Installieren Sie **OpenSSL** auf Ihrem System, sofern noch nicht geschehen.

8. Öffnen Sie eine **Eingabeaufforderung**, und führen Sie die folgenden Befehle aus:

    a. Geben Sie in der Eingabeaufforderung den Wert `openssl x509 -inform DER -in FreshDesk.cer -out certificate.crt` ein.

    > [!NOTE]
    > In diesem Fall ist **FreshDesk.cer** das Zertifikat, das Sie aus dem Azure-Portal heruntergeladen haben.

    b. Geben Sie in der Eingabeaufforderung den Wert `openssl x509 -noout -fingerprint -sha256 -inform pem -in certificate.crt` ein. 
    
    > [!NOTE]
    > In diesem Fall ist **certificate.crt** das Ausgabezertifikat, das im vorherigen Schritt erstellt wird.

    c. Kopieren Sie den Wert für **Fingerabdruck**, und fügen Sie ihn in Editor ein. Entfernen Sie die Doppelpunkte aus dem Fingerabdruck, um den endgültigen Fingerabdruckwert zu erhalten.

9. Klicken Sie im Abschnitt **Freshdesk-Konfiguration** auf **Freshdesk konfigurieren**, um das Fenster „Anmeldung konfigurieren“ zu öffnen. Kopieren Sie die URL für den SAML-SSO-Dienst und die Abmelde-URL aus dem Abschnitt **Kurzübersicht**.

    ![Configure single sign-on](./media/freshdesk-tutorial/tutorial_freshdesk_configure.png)

10. Melden Sie sich in einem anderen Webbrowserfenster bei der Freshdesk-Unternehmenswebsite als Administrator an.

11. Wählen Sie das Symbol **Einstellungen** aus, und führen Sie im Abschnitt **Sicherheit** die folgenden Schritte aus:

    ![Einmaliges Anmelden](./media/freshdesk-tutorial/IC776770.png "Einmaliges Anmelden")
  
    a. Wählen Sie für **Einmaliges Anmelden (SSO)** die Einstellung **Ein** aus.

    b. Wählen Sie **SAML-SSO**aus.

    c. Fügen Sie im Textfeld **SAML-Anmelde-URL** den Wert der **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Fügen Sie im Textfeld **Abmelde-URL** den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    e. Fügen Sie im Textfeld **Fingerabdruck des Sicherheitszertifikats** den Wert für **Fingerabdruck** ein, den Sie zuvor durch Entfernen der Doppelpunkte erhalten haben.
  
    f. Klicken Sie auf **Speichern**.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/freshdesk-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/freshdesk-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/freshdesk-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/freshdesk-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.

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

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Box gewähren.

![Benutzer zuweisen][200]

**Um Britta Simon Freshdesk zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Freshdesk** aus.

    ![Configure single sign-on](./media/freshdesk-tutorial/tutorial_freshdesk_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Freshdesk“ klicken, sollten Sie zur Anmeldeseite Ihrer Freshdesk-Anwendung weitergeleitet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/freshdesk-tutorial/tutorial_general_01.png
[2]: ./media/freshdesk-tutorial/tutorial_general_02.png
[3]: ./media/freshdesk-tutorial/tutorial_general_03.png
[4]: ./media/freshdesk-tutorial/tutorial_general_04.png

[100]: ./media/freshdesk-tutorial/tutorial_general_100.png

[200]: ./media/freshdesk-tutorial/tutorial_general_200.png
[201]: ./media/freshdesk-tutorial/tutorial_general_201.png
[202]: ./media/freshdesk-tutorial/tutorial_general_202.png
[203]: ./media/freshdesk-tutorial/tutorial_general_203.png