---
title: 'Tutorial: Azure Active Directory-Integration mit Mixpanel | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Mixpanel konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: a2df26ef-d441-44ac-a9f3-b37bf9709bcb
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 93203e82e71b66243818cac52f9ec431e85ba18f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55169245"
---
# <a name="tutorial-azure-active-directory-integration-with-mixpanel"></a>Tutorial: Azure Active Directory-Integration mit Mixpanel

In diesem Tutorial erfahren Sie, wie Sie Mixpanel in Azure Active Directory (Azure AD) integrieren.

Die Integration von Mixpanel in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Mixpanel hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Mixpanel anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Mixpanel konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges Mixpanel-Abonnement

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Mixpanel aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-mixpanel-from-the-gallery"></a>Hinzufügen von Mixpanel aus dem Katalog
Zum Konfigurieren der Integration von Mixpanel in Azure AD müssen Sie Mixpanel aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Mixpanel aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

1. Geben Sie im Suchfeld den Suchbegriff **Mixpanel**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/mixpanel-tutorial/tutorial_mixpanel_search.png)

1. Wählen Sie im Ergebnisbereich die Option **Mixpanel** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/mixpanel-tutorial/tutorial_mixpanel_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Mixpanel mithilfe eines Testbenutzers namens Britta Simon.

Für das einmalige Anmelden muss Azure AD wissen, welcher Benutzer in Mixpanel als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Mixpanel muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Mixpanel den Wert für **Benutzername** aus Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Mixpanel müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
1. **[Erstellen eines Mixpanel-Testbenutzers](#creating-a-mixpanel-test-user)**, um in Mixpanel einen Gegenpart von Britta Simon zu erhalten, der mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Mixpanel-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Mixpanel die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Mixpanel** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Configure single sign-on](./media/mixpanel-tutorial/tutorial_mixpanel_samlbase.png)

1. Führen Sie auf der Seite **Domäne und URLs für Mixpanel** die folgenden Schritte aus:

    ![Configure single sign-on](./media/mixpanel-tutorial/tutorial_mixpanel_url.png)

     Geben Sie im Textfeld **Anmelde-URL** die URL folgendermaßen ein: `https://mixpanel.com/login/`.

    > [!NOTE] 
    > Registrieren Sie sich unter [https://mixpanel.com/register/](https://mixpanel.com/register/), um Ihre Anmeldeinformationen einzurichten, und wenden Sie sich zur Aktivierung der SSO-Einstellungen für Ihren Mandanten an das [Mixpanel-Supportteam](mailto:support@mixpanel.com). Gegebenenfalls können Sie den Wert für die Anmelde-URL auch vom Mixpanel-Supportteam erhalten. 
 
1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Configure single sign-on](./media/mixpanel-tutorial/tutorial_mixpanel_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/mixpanel-tutorial/tutorial_general_400.png)

1. Klicken Sie im Abschnitt **Mixpanel-Konfiguration** auf **Mixpanel konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Configure single sign-on](./media/mixpanel-tutorial/tutorial_mixpanel_configure.png) 

1. Melden Sie sich in einem anderen Browserfenster in Ihrer Mixpanel-Anwendung als Administrator an.

1. Klicken in der linken unteren Ecke der Seite auf das kleine **Zahnradsymbol** . 
   
    ![Einmaliges Anmelden bei Mixpanel](./media/mixpanel-tutorial/tutorial_mixpanel_06.png) 

1. Klicken Sie auf die Registerkarte **Access security** (Zugriffssicherheit) und dann auf **Change settings** (Einstellungen ändern).
   
    ![Mixpanel-Einstellungen](./media/mixpanel-tutorial/tutorial_mixpanel_08.png) 

1. Klicken Sie auf der Dialogfeldseite **Change your certificate** (Zertifikat ändern) auf **Choose file** (Datei auswählen), um das heruntergeladene Zertifikat hochzuladen, und klicken Sie dann auf **NEXT** (WEITER).
   
    ![Mixpanel-Einstellungen](./media/mixpanel-tutorial/tutorial_mixpanel_09.png) 

1.  Fügen Sie auf der Dialogfeldseite **Change your authentication URL** (Authentifizierungs-URL ändern) im Textfeld für die Authentifizierungs-URL den Wert der **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben, und klicken Sie anschließend auf **NEXT** (WEITER).
   
   ![Mixpanel-Einstellungen](./media/mixpanel-tutorial/tutorial_mixpanel_10.png) 

1. Klicken Sie auf **Fertig**.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Dokumentation zu eingebettetem Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/mixpanel-tutorial/create_aaduser_01.png) 

1. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/mixpanel-tutorial/create_aaduser_02.png) 

1. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/mixpanel-tutorial/create_aaduser_03.png) 

1. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/mixpanel-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.
 
### <a name="creating-a-mixpanel-test-user"></a>Erstellen eines Mixpanel-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Mixpanel. 

1. Melden Sie sich bei Ihrer Mixpanel-Unternehmenswebsite als Administrator an.

1. Klicken Sie in der linken unteren Ecke der Seite auf die kleine Zahnrad-Schaltfläche, um das Fenster **Einstellungen** zu öffnen.

1. Klicken Sie auf die Registerkarte **Team** .

1. Geben Sie im Textfeld für **Team Member** die in Azure hinterlegte E-Mail-Adresse von Britta Simon ein.
   
    ![Mixpanel-Einstellungen](./media/mixpanel-tutorial/tutorial_mixpanel_11.png) 

1. Klicken Sie auf **Einladen**. 

> [!Note]
> Der Benutzer erhält eine E-Mail für die Profileinrichtung.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Mixpanel gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Mixpanel zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste den Eintrag **Mixpanel**aus.

    ![Configure single sign-on](./media/mixpanel-tutorial/tutorial_mixpanel_app.png) 

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Mixpanel“ klicken, sollten Sie automatisch bei Ihrer Mixpanel-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/mixpanel-tutorial/tutorial_general_01.png
[2]: ./media/mixpanel-tutorial/tutorial_general_02.png
[3]: ./media/mixpanel-tutorial/tutorial_general_03.png
[4]: ./media/mixpanel-tutorial/tutorial_general_04.png

[100]: ./media/mixpanel-tutorial/tutorial_general_100.png

[200]: ./media/mixpanel-tutorial/tutorial_general_200.png
[201]: ./media/mixpanel-tutorial/tutorial_general_201.png
[202]: ./media/mixpanel-tutorial/tutorial_general_202.png
[203]: ./media/mixpanel-tutorial/tutorial_general_203.png

