---
title: "Lernprogramm: Azure Active Directory-Integration mit Gefühl United States (nicht-UltiPro) | Microsoft Docs"
description: "Informationen Sie zum Konfigurieren von einmaliges Anmelden zwischen Azure Active Directory und das Gefühl United States (nicht-UltiPro)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b4a8f026-cb5f-41eb-9680-68eddc33565e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 8e2f9f979f8b94e0c043d4db6e93bd7a53c3dd27
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-perception-united-states-non-ultipro"></a>Lernprogramm: Azure Active Directory-Integration mit Gefühl United States (nicht-UltiPro)

In diesem Lernprogramm erfahren Sie, wie Gefühl United States (nicht-UltiPro) mit Azure Active Directory (Azure AD) integrieren.

Integrieren von Gefühl United States (nicht-UltiPro) in Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Gefühl United States (nicht-UltiPro) hat.
- Sie können Ihre Benutzer für die automatisch signiert, Gefühl United States (nicht-UltiPro) (Single Sign-On) mit ihren Azure AD-Konten bei abrufen.
- Sie können Ihre Konten an einem zentralen Ort verwalten: im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um Azure AD-Integration mit Gefühl United States (nicht-UltiPro) konfigurieren, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Einem Wahrnehmung United States (nicht-UltiPro) einmaliges Anmelden für Abonnement mit aktivierter

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Gefühl United States (nicht-UltiPro) aus dem Katalog hinzufügen
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-perception-united-states-non-ultipro-from-the-gallery"></a>Gefühl United States (nicht-UltiPro) aus dem Katalog hinzufügen
Um die Integration von Gefühl United States (nicht-UltiPro) in Azure Active Directory zu konfigurieren, müssen Sie Gefühl United States (nicht-UltiPro) hinzufügen aus dem Katalog der Liste der verwalteten SaaS-apps.

**Um Gefühl United States (nicht-UltiPro) aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie in das Suchfeld **Gefühl United States (nicht-UltiPro)**Option **Gefühl United States (nicht-UltiPro)** über die Systemsteuerung über Ergebnis klicken Sie dann auf **hinzufügen** Schaltfläche, um die Anwendung hinzuzufügen.

    ![Gefühl United States (nicht-UltiPro) in der Ergebnisliste](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt Konfigurieren und Testen Sie Azure AD einmaliges Anmelden mit Gefühl United States (nicht-UltiPro) basierend auf einen Testbenutzer "Britta Simon" aufgerufen.

Für einmaliges Anmelden funktioniert muss Azure AD zu wissen, was der Benutzer Gegenstück in Gefühl United States (nicht-UltiPro) für einen Benutzer in Azure AD ist. Das heißt, muss eine linkbeziehung zwischen einem Azure AD-Benutzer und die entsprechenden Benutzer Gefühl United States (nicht-UltiPro) hergestellt werden.

In Gefühl United States (nicht-UltiPro), weisen Sie den Wert von der **Benutzername** in Azure AD als Wert des der **Benutzername** auf die linkbeziehung zu erstellen.

Zum Konfigurieren und Testen Sie Azure AD einmaliges Anmelden mit Gefühl United States (nicht-UltiPro), müssen Sie den folgenden Bausteinen ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Testbenutzers Gefühl United States (nicht-UltiPro)](#create-a-perception-united-states-non-ultipro-test-user)**  – um eine Entsprechung Britta Simon Gefühl United States (nicht-UltiPro) verfügen, die mit der Azure AD-Darstellung eines Benutzers verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt werden Azure AD einmaliges Anmelden im Azure-Portal aktivieren und konfigurieren Sie einmaliges Anmelden in Ihrer Anwendung Gefühl United States (nicht-UltiPro).

**Um Azure AD einmaliges Anmelden mit Gefühl United States (nicht-UltiPro) konfigurieren, führen Sie die folgenden Schritte aus:**

1. Im Azure-Portal auf der **Gefühl United States (nicht-UltiPro)** anwendungsintegrationsseite, klicken Sie auf **des einmaligen Anmeldens**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_samlbase.png)

3. Auf der **Gefühl United States (nicht-UltiPro)-Domäne und die URLs** führen die folgenden Schritte aus:

    ![Gefühl United States (nicht-UltiPro) URLs einmaliges Anmelden für Informationen zur Domänen- und](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_url.png)

    a. Geben Sie im Textfeld **Bezeichner** die folgende URL ein: `https://perception.kanjoya.com/sp`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://perception.kanjoya.com/sso?idp=<entity_id>`

    > [!NOTE] 
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Sie aktualisieren den Wert mit der tatsächlichen Antwort-URL. Dies wird später in diesem Tutorial beschrieben.
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_400.png)

6. Auf der **Gefühl United States (nicht-UltiPro) Konfiguration** auf **konfigurieren Gefühl United States (nicht-UltiPro)** öffnen **Anmelden konfigurieren** Fenster. Kopieren Sie die **SAML-Entitäts-ID** aus dem Abschnitt **Kurzübersicht**.

    a. Die **Gefühl United States (nicht-UltiPro)** Anwendung erfordert, dass die **SAML Entitäts-ID** -Wert, der Sie kopiert haben, Uri codiert sein. Um den codierten Uri-Wert zu erhalten, verwenden Sie den folgenden Link:**http://www.url-encode-decode.com/**.

    b. Nach dem Abrufen des URIs codierten Wert kombinieren Sie sie mit der **-Antwort-URL** wie erwähnt werden weiter unten.

    `https://perception.kanjoya.com/sso?idp=<URI encooded entity_id>`
    
    c. Fügen Sie den oben angegebenen Wert in der **-Antwort-URL** Textbox in **Gefühl United States (nicht-UltiPro)-Domäne und die URLs** Abschnitt.

    ![Gefühl United States (nicht UltiPro) Konfiguration](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_configure.png) 

7. Melden Sie in einem anderen Browserfenster an, um Ihre Gefühl United States (nicht-UltiPro)-Unternehmenswebsite als Administrator.

8. Klicken Sie auf der Hauptsymbolleiste auf **Kontoeinstellungen**.

    ![Gefühl United States (nicht-UltiPro) Benutzer](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_user.png)

9. Auf der **Kontoeinstellungen** Seite, die folgenden Schritte aus:

    ![Gefühl United States (nicht-UltiPro) Benutzer](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_account.png)

    a. In der **Firmenname** Textfeld den Namen des der **Unternehmen**.
    
    b. In der **Kontoname** Textfeld den Namen des der **Konto**.

    c. In **Standard Reply-To-e-Mail** Text geben die gültigen **E-Mail**.

    d. Wählen Sie **SSO-Identitätsanbieter** als **SAML 2.0**.

10. Auf der **SSO-Konfiguration** führen die folgenden Schritte aus:

    ![Gefühl United States (nicht UltiPro) SSOConfig](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_ssoconfig.png)

    a. Wählen Sie **SAML "NameID" Type** als **E-Mail**.

    b. In der **Name der SSO-Konfiguration** Textfeld den Namen des Ihrer **Konfiguration**.
    
    c. In **Identitätsanbietername** Textbox, fügen Sie den Wert der **SAML Entitäts-ID**, der Sie vom Azure-Portal kopiert haben. 

    d. In **SAML domänentextfeld**, geben Sie die Domäne wie  **@contoso.com** .

    e. Klicken Sie auf **erneut hochladen** zum Hochladen der **Metadata XML** Datei.

    f. Klicken Sie auf **Aktualisieren**.


> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
  
### <a name="create-a-perception-united-states-non-ultipro-test-user"></a>Erstellen eines Testbenutzers Gefühl United States (nicht-UltiPro)

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon Gefühl United States (nicht-UltiPro). Arbeiten mit [Gefühl United States (nicht-UltiPro)-Supportteam](http://www.ultimatesoftware.com/Contact/ContactUs) um die Benutzer in der Plattform Gefühl United States (nicht-UltiPro) hinzuzufügen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt können Sie Britta Simon Azure einmaliges Anmelden verwenden, durch Gewähren des Zugriffs auf Gefühl United States (nicht-UltiPro).

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon zu Gefühl United States (nicht-UltiPro) zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste der Anwendungen **Gefühl United States (nicht-UltiPro)**.

    ![Der Link Gefühl United States (nicht-UltiPro) in die Liste der Anwendungen](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie die Kachel Gefühl United States (nicht-UltiPro) im Zugriffsbereich klicken, Sie sollten automatisch für Ihre Anwendung Gefühl United States (nicht-UltiPro) angemeldete abrufen.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_203.png

