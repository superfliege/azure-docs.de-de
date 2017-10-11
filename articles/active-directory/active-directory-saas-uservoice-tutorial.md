---
title: 'Lernprogramm: Azure Active Directory-Integration in UserVoice | Microsoft Docs'
description: Erfahren Sie, wie einmaliges Anmelden zwischen Azure Active Directory und UserVoice zu konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: fcfda1c2ecb162fb93b70574a18bd745b72ee4db
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Lernprogramm: Azure Active Directory-Integration in UserVoice

In diesem Lernprogramm erfahren Sie, wie UserVoice mit Azure Active Directory (Azure AD) integrieren.

Integrieren von UserVoice in Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer uservoice zugreifen.
- Sie können Ihre Benutzer für die automatisch signiert uservoice (Single Sign-On) mit ihren Azure AD-Konten bei abrufen.
- Sie können Ihre Konten an einem zentralen Ort verwalten: im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um Azure AD-Integration in UserVoice zu konfigurieren, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Ein UserVoice einmaliges Anmelden für Abonnement mit aktivierter

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von UserVoice aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-uservoice-from-the-gallery"></a>Hinzufügen von UserVoice aus dem Katalog
Um die UserVoice-Integration in Azure AD zu konfigurieren, müssen Sie UserVoice der Liste der verwalteten SaaS-apps aus dem Katalog hinzufügen.

**Um UserVoice aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie in das Suchfeld **UserVoice**Option **UserVoice** über die Systemsteuerung über Ergebnis klicken Sie dann auf **hinzufügen** Schaltfläche, um die Anwendung hinzuzufügen.

    ![UserVoice in der Ergebnisliste](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt Konfigurieren und Testen Sie Azure AD einmaliges Anmelden für mit UserVoice basierend auf einen Testbenutzer "Britta Simon" aufgerufen.

Für einmaliges Anmelden funktioniert muss Azure AD zu wissen, was der Benutzer Gegenstück in UserVoice für einen Benutzer in Azure AD ist. Das heißt, muss eine linkbeziehung zwischen einem Azure AD-Benutzer und die entsprechenden Benutzer in UserVoice hergestellt werden.

In UserVoice, weisen Sie den Wert der die **Benutzername** in Azure AD als Wert des der **Benutzername** auf die linkbeziehung zu erstellen.

Zum Konfigurieren und Testen Sie Azure AD einmaliges Anmelden mit UserVoice, müssen Sie den folgenden Bausteinen ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Testbenutzers UserVoice](#create-a-uservoice-test-user)**  – um eine Entsprechung Britta Simon in UserVoice verfügen, die mit der Azure AD-Darstellung eines Benutzers verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt werden Azure AD einmaliges Anmelden im Azure-Portal aktivieren und konfigurieren Sie einmaliges Anmelden in Ihrer UserVoice-Anwendung.

**Führen Sie zum Konfigurieren von Azure AD einmaliges Anmelden mit UserVoice die folgenden Schritte aus:**

1. Im Azure-Portal auf der **UserVoice** anwendungsintegrationsseite, klicken Sie auf **des einmaligen Anmeldens**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_samlbase.png)

3. Auf der **UserVoice-Domänen und URLs** führen die folgenden Schritte aus:

    ![UserVoice-URLs einmaliges Anmelden für Informationen zur Domänen- und](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<tenantname>.UserVoice.com`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<tenantname>.UserVoice.com`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an [UserVoice-Client-Supportteam](https://www.uservoice.com/) , diese Werte abzurufen.

4. Kopieren Sie im Abschnitt **SAML-Signaturzertifikat** den Wert **FINGERABDRUCK** des Zertifikats.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-uservoice-tutorial/tutorial_general_400.png)

6. Auf der **UserVoice Konfiguration** auf **konfigurieren UserVoice** öffnen **Anmelden konfigurieren** Fenster. Kopieren Sie die **Abmelde-URL und die SAML-Dienst-URL für einmaliges Anmelden** aus dem Abschnitt **Kurzübersicht**.

    ![UserVoice-Konfiguration](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_configure.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster auf Ihrer UserVoice-Unternehmenswebsite als Administrator an an.

8. Klicken Sie in der Symbolleiste am oberen Rand auf **Einstellungen**, und wählen Sie dann **Webportal** aus dem Menü.
   
    ![Einstellungsabschnitt auf App-Seite](./media/active-directory-saas-uservoice-tutorial/ic777519.png "Einstellungen")

9. Auf der **Webportal** Registerkarte der **Benutzerauthentifizierung** auf **bearbeiten** So öffnen die **Edit User Authentication** Seite.
   
    ![Webportal Registerkarte](./media/active-directory-saas-uservoice-tutorial/ic777520.png "-Webportal")

10. Führen Sie auf der Dialogseite **Benutzerauthentifizierung bearbeiten** die folgenden Schritte aus:
   
    ![Benutzerauthentifizierung bearbeiten](./media/active-directory-saas-uservoice-tutorial/ic777521.png "Benutzerauthentifizierung bearbeiten")
   
    a. Klicken Sie auf **Einmaliges Anmelden (SSO)**.
 
    b. Fügen Sie der **SAML anmelden Dienst-URL für einmalige** -Wert, der vom Azure-Portal in der Sie kopiert haben die **SSO Remote Sign-in** Textfeld.

    c. Fügen Sie der **URL Abmeldung** -Wert, der vom Azure-Portal in der Sie kopiert haben die **SSO Remote Sign-Out Textfeld**.
 
    d. Fügen Sie der **Fingerabdruck** -Wert, der Sie in Azure-Portal kopiert haben die **aktuelle SHA1-zertifikatsfingerabdruck** Textfeld.
    
    e. Klicken Sie auf **Authentifizierungseinstellungen speichern**.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-uservoice-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-uservoice-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-uservoice-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-uservoice-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-uservoice-test-user"></a>Erstellen eines Testbenutzers UserVoice

Um Azure AD-Benutzer zum Anmelden bei UserVoice zu aktivieren, müssen sie in UserVoice bereitgestellt werden. Im Fall von UserVoice ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:
1. Melden Sie sich bei Ihrem **UserVoice** -Mandanten an.

2. Wechseln Sie zu **Einstellungen**.
   
    ![Einstellungen](./media/active-directory-saas-uservoice-tutorial/ic777811.png "Einstellungen")

3. Klicken Sie auf **Allgemein**.

4. Klicken Sie auf **Agents und Berechtigungen**.
   
    ![Agents und Berechtigungen](./media/active-directory-saas-uservoice-tutorial/ic777812.png "Agents und Berechtigungen")

5. Klicken Sie auf **Administratoren hinzufügen**.
   
    ![Administratoren hinzufügen](./media/active-directory-saas-uservoice-tutorial/ic777813.png "Administratoren hinzufügen")

6. Führen Sie im Dialogfeld **Benutzer einladen** die folgenden Schritte aus:
   
    ![Administratoren einladen](./media/active-directory-saas-uservoice-tutorial/ic777814.png "Administratoren einladen")
   
    a. Geben Sie in das Textfeld für die e-Mail-Nachrichten, die e-Mail-Adresse des Kontos, die Sie bereitstellen möchten, und klicken Sie dann auf **hinzufügen**.
   
    b. Klicken Sie auf **Invite**.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von UserVoice-Benutzerkonten oder mithilfe der von UserVoice bereitgestellten APIs erstellen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt können Sie Britta Simon durch Gewähren des Zugriffs auf UserVoice Azure einmaliges Anmelden zu verwenden.

![Zuweisen der Benutzerrolle][200] 

**UserVoice Britta Simon zuweisen möchten, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste der Anwendungen **UserVoice**.

    ![Die UserVoice-Link in der Anwendungsliste aus](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie auf den Zugriffsbereich die UserVoice-Kachel klicken, Sie sollten automatisch für Ihre UserVoice-Anwendung auf abrufen.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_203.png

