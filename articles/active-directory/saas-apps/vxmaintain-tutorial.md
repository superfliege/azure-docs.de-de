---
title: 'Tutorial: Azure Active Directory-Integration mit vxMaintain | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und vxMaintain konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 841a1066-593c-4603-9abe-f48496d73d10
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1d0e8f8526d866c308be8684546397f282dcce51
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56194103"
---
# <a name="tutorial-azure-active-directory-integration-with-vxmaintain"></a>Tutorial: Azure Active Directory-Integration mit vxMaintain

In diesem Tutorial erfahren Sie, wie Sie vxMaintain in Azure Active Directory (Azure AD) integrieren.

Diese Integration bietet mehrere wichtige Vorteile. Ihre Möglichkeiten:

- Steuern Sie in Azure AD , wer Zugriff auf vxMaintain hat.
- Ermöglichen Sie Benutzern, sich mit ihren Azure AD-Konten automatisch mit einmaligem Anmelden (Single Sign-On, SSO) bei vxMaintain anzumelden.
- Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit vxMaintain konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges vxMaintain-Abonnement

> [!NOTE]
> Es wird nicht empfohlen, zum Testen der Schritte in diesem Tutorial eine Produktionsumgebung zu verwenden.

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. 

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

* Hinzufügen von vxMaintain aus dem Katalog
* Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="add-vxmaintain-from-the-gallery"></a>Hinzufügen von vxMaintain aus dem Katalog
Zum Konfigurieren der Integration von vxMaintain in Azure AD müssen Sie vxMaintain aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

Um vxMaintain aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:

1. Wählen Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) die Schaltfläche **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Wählen Sie **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Bereich „Unternehmensanwendungen“][2]
    
1. Um eine Anwendung hinzuzufügen, wählen Sie im Dialogfeld **Alle Anwendungen** die Schaltfläche **Neue Anwendung** aus.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld als Suchbegriff **vxMaintain**ein.

    ![Die Dropdownliste „SSO-Modus“](./media/vxmaintain-tutorial/tutorial_vxmaintain_search.png)

1. Wählen Sie in der Ergebnisliste **vxMaintain** und dann **Hinzufügen**.

    ![Der vxMaintain-Link](./media/vxmaintain-tutorial/tutorial_vxmaintain_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden (SSO) von Azure AD mit vxMaintain basierend auf einer Testbenutzerin mit dem Namen Britta Simon.

Damit einmaliges Anmelden (SSO) funktioniert, muss Azure AD wissen, welcher Benutzer in vxMaintain als Entsprechung zu einem Benutzer in Azure AD fungiert. Sie müssen also eine Linkbeziehung zwischen dem Azure AD-Benutzer und dem entsprechenden vxMaintain-Benutzer einrichten.

Um die Linkbeziehung herzustellen, weisen Sie in vxMaintain den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei vxMaintain führen Sie die folgenden Bausteine aus.

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

In diesem Abschnitt können Sie folgendermaßen das einmalige Anmelden von Azure AD sowohl im Azure-Portal aktivieren als auch in Ihrer vxMaintain-Anwendung konfigurieren:

1. Wählen Sie im Azure-Portal auf der Anwendungsintegrationsseite für **vxMaintain** die Option **Einmaliges Anmelden**.

    ![Der Befehl „Einmaliges Anmelden“][4]

1. Um das einmalige Anmelden zu aktivieren, wählen Sie in der Dropdwonliste **SSO-Modus** die Option **SAML-basierte Anmeldung** aus.
 
    ![Der Befehl „SAML-basierte Anmeldung“](./media/vxmaintain-tutorial/tutorial_vxmaintain_samlbase.png)

1. Gehen Sie unter **Domäne und URLs für vxMaintain** folgendermaßen vor:

    ![Abschnitt „Domäne und URLs für vxMaintain“](./media/vxmaintain-tutorial/tutorial_vxmaintain_url.png)

    a. Geben Sie im Feld **Bezeichner** eine URL mit folgender Syntax ein: `https://<company name>.verisae.com`.

    b. Geben Sie im Feld **Antwort-URL** eine URL mit folgender Syntax ein: `https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`.

    > [!NOTE] 
    > Die vorangehenden Werte sind keine echten Werte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Wenden Sie sich an das [Supportteam von vxMaintain](https://www.hubspot.com/company/contact), um diese Werte zu erhalten.
 
1. Wählen Sie unter **SAML-Signaturzertifikat** die Option **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Der Abschnitt „SAML-Signaturzertifikat“](./media/vxmaintain-tutorial/tutorial_vxmaintain_certificate.png) 

1. Wählen Sie **Speichern** aus.

    ![Die Schaltfläche „Speichern“](./media/vxmaintain-tutorial/tutorial_general_400.png)

1. Um **vxMaintain**-SSO zu konfigurieren, senden Sie die heruntergeladene **Metadaten-XML**-Datei an das [vxMaintain Support-Team](https://www.hubspot.com/company/contact).

> [!TIP]
> Wie Sie die App einrichten, können Sie eine kürzere Version der vorherigen Anweisungen im [Azure-Portal](https://portal.azure.com) lesen. Nachdem Sie diese App über den Abschnitt **Active Directory** > **Unternehmensanwendungen** hinzugefügt haben, wählen Sie die Registerkarte **Einmaliges Anmelden** und rufen anschließend die eingebettete Dokumentation über den Abschnitt **Konfiguration** auf. 
>
>Weitere Informationen zum Feature der eingebetteten Dokumentation finden Sie unter [Verwalten des einmaligen Anmeldens für Unternehmens-Apps](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im Azure-Portal wie folgt einen Testbenutzer mit dem Namen Britta Simon:

![Der Azure AD-Testbenutzer][100]

1. Wählen Sie im linken Bereich des **Azure-Portals** die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/vxmaintain-tutorial/create_aaduser_01.png) 

1. Um die Liste der Benutzer anzuzeigen, gehen Sie zu **Benutzer und Gruppen** > **Alle Benutzer**.
    
    ![Der Link „Alle Benutzer“](./media/vxmaintain-tutorial/create_aaduser_02.png)  
    Das Dialogfeld **Alle Benutzer** wird geöffnet. 

1. Zum Öffnen des Dialogfelds **Benutzer** wählen Sie **Hinzufügen**.
 
    ![Schaltfläche „Hinzufügen“](./media/vxmaintain-tutorial/create_aaduser_03.png) 

1. Führen Sie im Dialogfeld **Benutzer** folgende Schritte aus:
 
    ![Dialogfeld „Benutzer“](./media/vxmaintain-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse der Testbenutzerin Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** generiert wurde.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-vxmaintain-test-user"></a>Erstellen eines vxMaintain-Testbenutzers

In diesem Abschnitt erstellen Sie in vxMaintain eine Testbenutzerin mit dem Namen Britta Simon. Lassen Sie sich beim Hinzufügen der Benutzer auf der vxMaintain-Plattform vom  [vxMaintain-Supportteam](https://www.hubspot.com/company/contact) unterstützen. Bevor Sie SSO verwenden, erstellen und aktivieren Sie die Benutzer.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie der Testbenutzerin Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf vxMaintain gewähren. Führen Sie hierzu folgende Schritte aus:

![Testbenutzerin in der Liste „Anzeigename“][200] 

1. Navigieren Sie im Azure-Portal in der Ansicht **Anwendungen** zur Ansicht **Verzeichnis** > **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Der Link „Alle Anwendungen“][201] 

1. Wählen Sie in der Liste **Anwendungen** die Option **vxMaintain**.

    ![Der vxMaintain-Link](./media/vxmaintain-tutorial/tutorial_vxmaintain_app.png) 

1. Wählen Sie auf der linken Seite **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202] 

1. Wählen Sie **Hinzufügen** und dann im Bereich **Zuweisung hinzufügen** die Option **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste **Benutzer** den Eintrag **Britta Simon** und dann die Schaltfläche **Auswählen** aus.

1. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Option **Zuweisen** aus.
    
### <a name="test-your-azure-ad-single-sign-on"></a>Testen des einmaligen Anmeldens mit Azure AD

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Bei Auswahl der Kachel **vxMaintain** im Zugriffsbereich sollten Sie automatisch bei Ihrer vxMaintain-Anwendung angemeldet werden.

Weitere Informationen zum Zugriffspanel finden Sie unter [Einführung in das Zugriffspanel](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="next-steps"></a>Nächste Schritte

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/vxmaintain-tutorial/tutorial_general_01.png
[2]: ./media/vxmaintain-tutorial/tutorial_general_02.png
[3]: ./media/vxmaintain-tutorial/tutorial_general_03.png
[4]: ./media/vxmaintain-tutorial/tutorial_general_04.png

[100]: ./media/vxmaintain-tutorial/tutorial_general_100.png

[200]: ./media/vxmaintain-tutorial/tutorial_general_200.png
[201]: ./media/vxmaintain-tutorial/tutorial_general_201.png
[202]: ./media/vxmaintain-tutorial/tutorial_general_202.png
[203]: ./media/vxmaintain-tutorial/tutorial_general_203.png

