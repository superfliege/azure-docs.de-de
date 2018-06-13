---
title: 'Tutorial: Azure Active Directory-Integration mit Atlassian Cloud | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Atlassian Cloud konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: jeedes
ms.openlocfilehash: 261baed7b8e1e5480396c06a9f92ecfb48a8b7ac
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34338855"
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Tutorial: Azure Active Directory-Integration mit Atlassian Cloud

In diesem Tutorial erfahren Sie, wie Sie Atlassian Cloud in Azure Active Directory (Azure AD) integrieren.

Die Integration von Atlassian Cloud in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Atlassian Cloud hat.
- Sie können Benutzern die automatische Anmeldung (Single Sign-On, einmaliges Anmelden) bei Atlassian Cloud mit ihren Azure AD-Konten ermöglichen.
- Sie können Ihre Konten an einem zentralen Ort, im Azure-Portal, verwalten.

Weitere Informationen zur Integration von SaaS-Apps (Software-as-a-Service) in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Für das Konfigurieren der Azure AD-Integration mit Atlassian Cloud benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Um SAML-SSO (Security Assertion Markup Language) für Atlassian Cloud-Produkte zu ermöglichen, müssen Sie Identity Manager einrichten. Weitere Informationen zu [Identity Manager]( https://www.atlassian.com/enterprise/cloud/identity-manager).

> [!NOTE]
> Es wird nicht empfohlen, zum Testen der Schritte in diesem Tutorial eine Produktionsumgebung zu verwenden.

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.
Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

* Hinzufügen von Atlassian Cloud aus dem Katalog
* Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="add-atlassian-cloud-from-the-gallery"></a>Hinzufügen von Atlassian Cloud aus dem Katalog
Zum Konfigurieren der Integration von Atlassian Cloud in Azure AD müssen Sie Atlassian Cloud aus dem Katalog in folgender Weise der Liste mit den verwalteten SaaS-Apps hinzufügen:

1. Wählen Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“][1]

2. Wählen Sie **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Bereich „Unternehmensanwendungen“][2]
    
3. Wählen Sie zum Hinzufügen einer Anwendung **Neue Anwendung** aus.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Atlassian Cloud** ein, wählen Sie in der Liste der Ergebnisse **Atlassian Cloud**, und dann **Hinzufügen** aus.

    ![Atlassian Cloud in der Ergebnisliste](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens *Britta Simon* das einmalige Anmelden bei Atlassian Cloud mithilfe von Azure AD.

Damit das einmalige Anmelden funktioniert, muss Azure AD den Atlassian Cloud-Benutzer und sein Gegenstück in Azure AD identifizieren. Anders ausgedrückt, müssen Sie zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in der Atlassian Cloud eine Linkbeziehung einrichten.

Um die Linkbeziehung herzustellen, weisen Sie dem Atlassian Cloud-*Benutzernamen* den gleichen Wert zu, der dem Azure AD-*Benutzernamen* zugewiesen ist.

Zum Konfigurieren und Testen des einmaligen Anmeldens bei Atlassian Cloud mit Azure AD müssen Sie die Bausteine in den folgenden Abschnitten ausführen.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Atlassian Cloud-Anwendung.

Führen Sie zum Konfigurieren des einmaligen Anmeldens bei Atlassian Cloud mit Azure AD die folgenden Schritte aus:

1. Wählen Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Atlassian Cloud** **Einmaliges Anmelden** aus.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Fenster **Einmaliges Anmelden** im Feld **SSO-Mode****SAML-basierte Anmeldung** aus.

    ![Fenster „Einmaliges Anmelden“](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. Um die Anwendung im IDP-initiierten Modus zu konfigurieren, führen Sie unter **Domäne und URLs für Atlassian Cloud** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und den URLs für Atlassian Cloud](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)
    
    a. Geben Sie im Feld **Bezeichner** **`https://auth.atlassian.com/saml/<unique ID>`** ein.
    
    b. Geben Sie im Feld **Antwort-URL** **`https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`** ein.

    c. Geben Sie im Feld **Relayzustand** eine URL mit der folgenden Syntax ein: **`https://<instancename>.atlassian.net`**.

4. Um die Anwendung im SP-initiierten Modus zu konfigurieren, wählen Sie **Erweiterte URL-Einstellungen anzeigen** aus, und geben Sie dann im Feld **Anmelde-URL** eine URL mit der folgenden Syntax ein: **`https://<instancename>.atlassian.net`**.

    ![SSO-Informationen zur Domäne und den URLs für Atlassian Cloud](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    > [!NOTE]
    > Die vorangehenden Werte sind keine echten Werte. Aktualisieren Sie sie mit den tatsächlichen Werten für Bezeichner, Antwort-URL und Anmelde-URL. Sie finden die tatsächlichen Werte auf dem Bildschirm für die SAML-Konfiguration für Atlassian Cloud. Wir erläutern die Werte später im Tutorial.

5. Wählen Sie unter **SAML-Signaturzertifikat** die Option **Zertifikat (Base64)** aus, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png)

6. Ihre Atlassian Cloud-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. 

    Standardmäßig wird der Wert von **Benutzer-ID** dem „user.userprincipalname“ zugeordnet. Ändern Sie die Zuordnung dieses Werts in „user.mail“. Sie können je nach dem Setup Ihrer Organisation auch jeden anderen geeigneten Wert zuordnen, doch sollte E-Mail normalerweise funktionieren.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_attribute.png)

7. Wählen Sie **Speichern**aus.

    ![Einmaliges Anmelden konfigurieren – Schaltfläche „Speichern“](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

8. Wählen Sie im Abschnitt **Atlassian Cloud-Konfiguration** **Atlassian Cloud konfigurieren** aus, um das Fenster **Anmelden konfigurieren** zu öffnen.

9. Kopieren Sie im Abschnitt **Kurzübersicht** die **SAML-Entitäts-ID** und die **URL für den SAML-SSO-Dienst**.

    ![Atlassian Cloud-Konfiguration](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png)

10. Melden Sie sich zum Konfigurieren von SSO für Ihre Anwendung mit Administratoranmeldeinformationen beim Atlassian-Portal an.

11. Vor dem Konfigurieren des einmaligen Anmeldens müssen Sie Ihre Domäne überprüfen. Weitere Informationen finden Sie im Dokument zur [Atlassian-Domänenüberprüfung](https://confluence.atlassian.com/cloud/domain-verification-873871234.html).

12. Wählen Sie im linken Bereich **SAML single sign-on** (Einmalige SAML-Anmeldung) aus. Wenn dies noch nicht erfolgt ist, abonnieren Sie Identity Manager von Atlassian.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

13. Gehen Sie im Fenster **Add SAML configuration** (SAML-Konfiguration hinzufügen) folgendermaßen vor:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. Fügen Sie in das Feld **Identity provider Entity ID** (Entitäts-ID des Identitätsanbieters) die ID der SAML-Entität ein, die Sie aus dem Azure-Portal kopiert haben.

    b. Fügen Sie in das Feld **Identity provider SSO URL** (SSO-URL des Identitätsanbieters) die URL des SAML-SSO-Diensts ein, die Sie aus dem Azure-Portal kopiert haben.

    c. Öffnen Sie das aus dem Azure-Portal in einer TXT-Datei heruntergeladene Zertifikat, kopieren Sie den Wert (ohne die Zeilen *Begin Certificate* und *End Certificate*), und fügen Sie es dann in das Feld **Public X509 certificate** (Öffentliches X509-Zertifikat) ein.
    
    d. Wählen Sie **Konfiguration speichern** aus.
     
14. Um sicherzustellen, dass Sie die richtigen URLs eingerichtet haben, aktualisieren Sie folgendermaßen die Azure AD-Einstellungen:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. Kopieren Sie im SAML-Fenster die **SP Identity ID** (SP-Identitäts-ID), und fügen Sie sie dann im Azure-Portal unter **Domäne und URLs** für Atlassian Cloud in das Feld **Bezeichner** ein.
    
    b. Kopieren Sie im SAML-Fenster die **SP Assertion Consumer Service URL** (URL des SP-Assertionsverbraucherdiensts), und fügen Sie sie dann im Azure-Portal unter **Domäne und URLs** für Atlassian Cloud in das Feld **Antwort-URL** ein. Die Anmelde-URL ist die Mandanten-URL für Ihre Atlassian Cloud.

    > [!NOTE]
    > Wenn Sie Bestandskunde sind, wählen Sie nach dem Aktualisieren der Werte für **SP Identity ID** (SP-Identitäts-ID) und **SP Assertion Consumer Service URL** (URL des SP-Assertionsverbraucherdiensts) **Yes, update configuration** (Ja, Konfiguration aktualisieren) aus. Als Neukunde können Sie diesen Schritt überspringen.
    
15. Wählen Sie im Azure-Portal **Speichern** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal wie folgt einen Testbenutzer mit dem Namen Britta Simon:

   ![Erstellen eines Azure AD-Testbenutzers][100]

1. Wählen Sie im linken Bereich des Azure-Portals die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png)

2. Um die Liste der Benutzer anzuzeigen, wählen Sie **Benutzer und Gruppen** > **Alle Benutzer**.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png)

3. Wählen Sie im Fenster **Alle Benutzer** **Hinzufügen** aus.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png)

4. Gehen Sie im Fenster **Benutzer** wie folgt vor:

    ![Fenster „Benutzer“](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

### <a name="create-an-atlassian-cloud-test-user"></a>Erstellen eines Atlassian Cloud-Testbenutzers

Um Azure AD-Benutzern die Anmeldung bei Atlassian Cloud zu ermöglichen, stellen Sie ihre Benutzerkonten in Atlassian Cloud manuell bereit, indem Sie wie folgt vorgehen:

1. Wählen Sie im Bereich **Administration** (Verwaltung) **Users** (Benutzer) aus.

    ![Atlassian Cloud-Benutzerlink](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png)

2. Um einen Benutzer in Atlassian Cloud zu erstellen, wählen Sie **Invite user** (Benutzer einladen) aus.

    ![Erstellen eines Atlassian Cloud-Benutzers](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png)

3. Geben Sie im Feld **E-Mail-Adresse** die E-Mail-Adresse des Benutzers ein, und weisen Sie dann den Anwendungszugriff zu.

    ![Erstellen eines Atlassian Cloud-Benutzers](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)

4. Um dem Benutzer eine E-Mail-Einladung zu senden, wählen Sie **Invite users** (Benutzer einladen) aus. Eine E-Mail-Einladung wird an den Benutzer gesendet, und nach dem Akzeptieren der Einladung ist der Benutzer im System aktiv.

>[!NOTE]
>Sie können auch eine Massenerstellung von Benutzern ausführen, indem Sie die Schaltfläche **Bulk Create** (Massenerstellung) im Abschnitt **Users** (Benutzer) auswählen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie der Benutzerin „Britta Simon“ das einmalige Anmelden bei Azure, indem Sie Zugriff auf Atlassian Cloud erteilen. Führen Sie hierzu folgende Schritte aus:

![Zuweisen der Benutzerrolle][200]

1. Öffnen Sie im Azure-Portal die **Anwendungsansicht**, navigieren Sie zur Verzeichnisansicht, und wählen Sie dann **Unternehmensanwendungen** > **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der **Anwendungsliste** **Atlassian Cloud** aus.

    ![Wählen Sie in der Anwendungsliste Atlassian Cloud aus.](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png)

3. Wählen Sie auf der linken Seite **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Wählen Sie **Hinzufügen** und dann im Bereich **Zuweisung hinzufügen** die Option **Benutzer und Gruppen**.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Fenster **Benutzer und Gruppen** in der Liste **Benutzer** die Option **Britta Simon** aus.

6. Wählen Sie im Fenster **Benutzer und Gruppen** die Option **Auswählen** aus.

7. Wählen Sie auf der Seite **Zuweisung hinzufügen** die Option **Zuweisen** aus.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel **Atlassian Cloud** auswählen, sollten Sie automatisch bei Ihrer Atlassian Cloud-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png
