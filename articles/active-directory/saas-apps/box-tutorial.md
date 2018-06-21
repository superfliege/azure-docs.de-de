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
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.openlocfilehash: c15894f8cd559a08a1d75e2ac29cc0da0fc9e963
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36226577"
---
# <a name="integrate-azure-active-directory-with-box"></a>Integrieren von Azure Active Directory in Box

In diesem Tutorial erfahren Sie, wie Sie Azure Active Directory (Azure AD) in Box integrieren.

Die Integration von Azure AD in Box bietet Ihnen die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Box haben soll.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Box anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort, im Azure-Portal, verwalten.

Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration in Box benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Box-SSO-fähiges Abonnement

> [!NOTE]
> Es wird *nicht* empfohlen, zum Testen der Schritte in diesem Tutorial eine Produktionsumgebung zu verwenden.

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. 

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Box aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="add-box-from-the-gallery"></a>Hinzufügen von Box aus dem Katalog
Um die Integration von Azure AD in Box zu konfigurieren, fügen Sie Box wie folgt aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzu:

1. Wählen Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) die Option **Azure Active Directory** aus. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Wählen Sie **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Fenster „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Fenster auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Box** ein, wählen Sie in der Ergebnisliste **Box** aus, und klicken Sie dann auf **Hinzufügen**.

    ![Box in der Ergebnisliste](./media/box-tutorial/tutorial_box_search.png)
### <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Box anhand einer Testbenutzerin mit dem Namen „Britta Simon“.

Damit das einmalige Anmelden funktioniert, muss Azure AD den Box-Benutzer und sein Gegenstück in Azure AD identifizieren. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Box muss eine Linkbeziehung eingerichtet werden.

Um die Linkbeziehung herzustellen, weisen Sie als *Benutzernamen* in Box den Wert von *Benutzername* in Azure AD zu.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Box führen Sie die Hauptschritte (Hauptelemente) in den nächsten fünf Abschnitten aus.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

Aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal, und konfigurieren Sie das einmalige Anmelden in Ihrer Box-Anwendung, indem Sie die folgenden Schritte ausführen:

1. Wählen Sie im Azure-Portal im Fenster der Anwendungsintegration für **Box** die Option **Einmaliges Anmelden** aus.

    ![Link „Einmaliges Anmelden“][4]

2. Wählen Sie im Fenster **Einmaliges Anmelden** im Feld **SSO-Mode****SAML-basierte Anmeldung** aus.
 
    ![Fenster „Einmaliges Anmelden“](./media/box-tutorial/tutorial_box_samlbase.png)

3. Gehen Sie unter **Domäne und URLs für Box** folgendermaßen vor:

    ![SSO-Informationen unter „Domäne und URLs für Box“](./media/box-tutorial/url3.png)

    a. Geben Sie im Feld **Anmelde-URL** eine URL im folgenden Format ein: *https://\<Unterdomäne>.box.com*.

    b. Geben Sie **box.net** in das Textfeld **Bezeichner** ein.
     
    > [!NOTE] 
    > Die vorangehenden Werte sind keine echten Werte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam von Box](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire), um diese Werte zu erhalten. 

4. Wählen Sie unter **SAML-Signaturzertifikat** die Option **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/box-tutorial/tutorial_box_certificate.png) 

5. Wählen Sie **Speichern**aus.

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/box-tutorial/tutorial_general_400.png)
    
6. Um SSO für Ihre Anwendung zu konfigurieren, gehen Sie wie unter [Setting Up SSO on your own](https://community.box.com/t5/How-to-Guides-for-Admins/Setting-Up-Single-Sign-On-SSO-for-your-Enterprise/ta-p/1263#ssoonyourown) (Einrichten von SSO) beschrieben vor.

> [!NOTE] 
> Falls Sie die SSO-Einstellungen für Ihr Box-Konto nicht aktivieren können, müssen Sie sich möglicherweise an das [Kundensupportteam von Box](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) wenden und die heruntergeladene XML-Datei bereitstellen.

> [!TIP]
> Zum Einrichten der App können Sie eine Kurzversion der vorstehenden Anweisungen im [Azure-Portal](https://portal.azure.com) lesen. Nachdem Sie diese App über den Abschnitt **Active Directory** > **Unternehmensanwendungen** hinzugefügt haben, navigieren Sie zur Registerkarte **Einmaliges Anmelden**, und rufen Sie unten im Abschnitt **Konfiguration** die eingebettete Dokumentation auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie unter [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
>

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal wie folgt einen Testbenutzer mit dem Namen Britta Simon:

![Erstellen eines Azure AD-Testbenutzers][100]

1. Klicken Sie im linken Bereich des Azure-Portals auf **Azure Active Directory**.

    ![Link „Azure Active Directory“](./media/box-tutorial/create_aaduser_01.png)

2. Um die Liste der aktuellen Benutzer anzuzeigen, wählen Sie **Benutzer und Gruppen** > **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/box-tutorial/create_aaduser_02.png)

3. Wählen Sie oben im Fenster **Alle Benutzer** die Option **Hinzufügen** aus.

    ![Schaltfläche „Hinzufügen“](./media/box-tutorial/create_aaduser_03.png)

    Das Fenster **Benutzer** wird geöffnet.

4. Gehen Sie im Fenster **Benutzer** wie folgt vor:

    ![Fenster „Benutzer“](./media/box-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-box-test-user"></a>Erstellen eines Testbenutzers in Box

In diesem Abschnitt erstellen Sie eine Testbenutzerin mit dem Namen Britta Simon in Box. Box unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist. Wenn ein Benutzer nicht bereits vorhanden ist, wird beim Versuch, auf Box zuzugreifen, ein neuer Benutzer erstellt. Zum Erstellen des Benutzers ist keine Aktion Ihrerseits erforderlich.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Box gewähren. Führen Sie hierzu folgende Schritte aus:

![Zuweisen der Benutzerrolle][200]

1. Öffnen Sie im Azure-Portal die **Anwendungsansicht**, navigieren Sie zur **Verzeichnisansicht**, und wählen Sie dann **Unternehmensanwendungen** > **Alle Anwendungen** aus.

    ![Links „Unternehmensanwendungen“ und „Alle Anwendungen“][201] 

2. Wählen Sie in der Liste **Anwendungen** den Eintrag **Box** aus.

    ![Link „Box“](./media/box-tutorial/tutorial_box_app.png)  

3. Wählen Sie auf der linken Seite **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Wählen Sie **Hinzufügen** und dann im Bereich **Zuweisung hinzufügen** die Option **Benutzer und Gruppen**.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Fenster **Benutzer und Gruppen** in der Liste **Benutzer** die Option **Britta Simon** aus.

6. Wählen Sie die Schaltfläche **Auswählen** aus.

7. Wählen Sie auf der Seite **Zuweisung hinzufügen** die Option **Zuweisen** aus.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel **Box** klicken, wird die Anmeldeseite für die Anmeldung bei Ihrer Box-Anwendung geöffnet.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Konfigurieren der Benutzerbereitstellung](box-userprovisioning-tutorial.md)



<!--Image references-->

[1]: ./media/box-tutorial/tutorial_general_01.png
[2]: ./media/box-tutorial/tutorial_general_02.png
[3]: ./media/box-tutorial/tutorial_general_03.png
[4]: ./media/box-tutorial/tutorial_general_04.png

[100]: ./media/box-tutorial/tutorial_general_100.png

[200]: ./media/box-tutorial/tutorial_general_200.png
[201]: ./media/box-tutorial/tutorial_general_201.png
[202]: ./media/box-tutorial/tutorial_general_202.png
[203]: ./media/box-tutorial/tutorial_general_203.png

