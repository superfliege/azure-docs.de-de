---
title: 'Tutorial: Azure Active Directory-Integration mit QPrism | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und QPrism konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 72ab75ba-132b-4f83-a34b-d28b81b6d7bc
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 286af8b927f488acaf2877e753f6e4689c4b000f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56199458"
---
# <a name="tutorial-azure-active-directory-integration-with-qprism"></a>Tutorial: Azure Active Directory-Integration mit QPrism

In diesem Tutorial erfahren Sie, wie Sie QPrism in Azure Active Directory (Azure AD) integrieren.

Die Integration von QPrism in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf QPrism hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei QPrism anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten: im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit QPrism konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges QPrism-Abonnement

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von QPrism aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="add-qprism-from-the-gallery"></a>Hinzufügen von QPrism aus dem Katalog
Zum Konfigurieren der Integration von QPrism in Azure AD müssen Sie QPrism aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**So fügen Sie QPrism aus dem Katalog hinzu**

1. Wählen Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) die Option **Azure Active Directory** aus. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Um eine neue Anwendung hinzuzufügen, wählen Sie oben im Dialogfeld **Neue Anwendung** aus.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie **QPrism** in das Suchfeld ein, und wählen Sie **QPrism** im Ergebnisbereich aus. Klicken Sie dann auf **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![QPrism in der Ergebnisliste](./media/qprism-tutorial/tutorial_qprism_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit QPrism basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in QPrism als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in QPrism muss eine Linkbeziehung bestehen.

Weisen Sie in QPrism den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um diese Beziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei QPrism müssen Sie die folgenden Aufgaben ausführen:

1. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on), um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. [Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user), um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. [Erstellen eines QPrism-Testbenutzers](#create-a-qprism-test-user), um eine Entsprechung von Britta Simon in QPrism zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. [Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user), um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. [Testen der einmaligen Anmeldung](#test-single-sign-on), um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer QPrism-Anwendung.

1. Wählen Sie im Azure-Portal auf der Anwendungsintegrationsseite für **QPrism** die Option **Einmaliges Anmelden** aus.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** für **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/qprism-tutorial/tutorial_qprism_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für QPrism** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für QPrism](./media/qprism-tutorial/tutorial_qprism_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<customer domain>.qmyzone.com/login`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://<customer domain>.qmyzone.com/metadata.php`.
         
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner und die tatsächliche Anmelde-URL. Wenden Sie sich an das [Clientsupportteam von QPrism](mailto:qsupport-ce@quatrro.com), um diese Werte zu erhalten. 

1. Klicken Sie im Abschnitt  **SAML-Signaturzertifikat** auf die Schaltfläche „Kopieren“, um die **Verbundmetadaten-URL der App** zu kopieren und in Editor einzufügen.

     ![Downloadlink für das Zertifikat](./media/qprism-tutorial/tutorial_qprism_certificate.png)

1. Wählen Sie **Speichern** aus.

    ![Einmaliges Anmelden konfigurieren: Schaltfläche „Speichern“](./media/qprism-tutorial/tutorial_general_400.png)
    
1. Zum Konfigurieren des einmaligen Anmeldens bei **QPrism** müssen Sie die **App-Verbundmetadaten-URL** an das [QPrism-Supportteam](mailto:qsupport-ce@quatrro.com) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**So erstellen Sie einen Testbenutzer in Azure AD**

1. Klicken Sie im linken Bereich des Azure-Portals auf **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/qprism-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/qprism-tutorial/create_aaduser_02.png)

1. Wählen Sie im oberen Bereich des Dialogfelds **Alle Benutzer** die Option **Hinzufügen** aus, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/qprism-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Benutzer** folgende Schritte aus:

    ![Dialogfeld „Benutzer“](./media/qprism-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-qprism-test-user"></a>Erstellen eines QPrism-Testbenutzers

In diesem Abschnitt erstellen Sie in QPrism einen Benutzer namens Britta Simon. Arbeiten Sie mit dem [QPrism-Supportteam](mailto:qsupport-ce@quatrro.com) zusammen, um Benutzer auf der QPrism-Plattform hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können. 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf QPrism gewähren.

![Zuweisen der Benutzerrolle][200] 

**So weisen Sie Britta Simon zu QPrism zu**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, und navigieren Sie zur Verzeichnisansicht. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **QPrism** aus.

    ![QPrism-Link in der Anwendungsliste](./media/qprism-tutorial/tutorial_qprism_app.png)  

1. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“][202]

1. Wählen Sie **Hinzufügen**. Wählen Sie dann unter **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste **Benutzer** den Eintrag **Britta Simon** aus.

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** die Option **Auswählen** aus.

1. Wählen Sie unter **Zuweisung hinzufügen** die Option **Zuweisen** aus.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel „QPrism“ auswählen, sollten Sie automatisch bei Ihrer QPrism-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/qprism-tutorial/tutorial_general_01.png
[2]: ./media/qprism-tutorial/tutorial_general_02.png
[3]: ./media/qprism-tutorial/tutorial_general_03.png
[4]: ./media/qprism-tutorial/tutorial_general_04.png

[100]: ./media/qprism-tutorial/tutorial_general_100.png

[200]: ./media/qprism-tutorial/tutorial_general_200.png
[201]: ./media/qprism-tutorial/tutorial_general_201.png
[202]: ./media/qprism-tutorial/tutorial_general_202.png
[203]: ./media/qprism-tutorial/tutorial_general_203.png

