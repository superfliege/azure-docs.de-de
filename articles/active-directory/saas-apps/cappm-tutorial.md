---
title: 'Tutorial: Azure Active Directory-Integration mit CA PPM | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und CA PPM konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: ca9d5e71-e429-4891-8d10-3498e7210e89
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: a0fd2b111ae904e94830b067f9a60409a5499ed0
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421280"
---
# <a name="tutorial-azure-active-directory-integration-with-ca-ppm"></a>Tutorial: Azure Active Directory-Integration mit CA PPM

In diesem Tutorial erfahren Sie, wie Sie CA PPM in Azure Active Directory (Azure AD) integrieren.

Die Integration von CA PPM in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf CA PPM hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei CA PPM anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit CA PPM konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein CA PPM-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von CA PPM aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-ca-ppm-from-the-gallery"></a>Hinzufügen von CA PPM aus dem Katalog
Zum Konfigurieren der Integration von CA PPM in Azure AD müssen Sie CA PPM aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um CA PPM aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

1. Geben Sie im Suchfeld als Suchbegriff **CA PPM**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/cappm-tutorial/tutorial_cappm_search.png)

1. Wählen Sie im Ergebnisbereich **CA PPM** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/cappm-tutorial/tutorial_cappm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei CA PPM basierend auf einer Testbenutzerin mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in CA PPM als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in CA PPM muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in CA PPM den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei CA PPM müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
1. **[Erstellen eines CA PPM-Testbenutzers](#creating-a-ca-ppm-test-user)**, um ein Pendant von Britta Simon in CA PPM zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer CA PPM-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei CA PPM die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **CA PPM** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Configure single sign-on](./media/cappm-tutorial/tutorial_cappm_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für CA PPM** die folgenden Schritte aus:

    ![Configure single sign-on](./media/cappm-tutorial/tutorial_cappm_url.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://ca.ondemand.saml.20.post.<companyname>`
    
    b. Geben Sie im Textfeld **Antwort-URL** Folgendes ein: `https://fedsso.ondemand.ca.com/affwebservices/public/saml2assertionconsumer`

    > [!NOTE] 
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Aktualisieren Sie den Wert mit dem tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam von CA PPM](mailto:catechnicalsupport@ca.com), um diese Werte zu erhalten.
 
1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Configure single sign-on](./media/cappm-tutorial/tutorial_cappm_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/cappm-tutorial/tutorial_general_400.png)

1. Klicken Sie im Abschnitt **CA PPM-Konfiguration** auf **CA PPM konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID** aus dem **Abschnitt mit der Kurzübersicht**.

    ![Configure single sign-on](./media/cappm-tutorial/tutorial_cappm_configure.png) 

1. Für die Konfiguration des einmaligen Anmeldens auf der **CA PPM**-Seite müssen Sie das heruntergeladene **Zertifikat (Base64)** und die **SAML-Entitäts-ID** an das [CA PPM-Supportteam](mailto:catechnicalsupport@ca.com) senden.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/cappm-tutorial/create_aaduser_01.png) 

1. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/cappm-tutorial/create_aaduser_02.png) 

1. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/cappm-tutorial/create_aaduser_03.png) 

1. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/cappm-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.
 
### <a name="creating-a-ca-ppm-test-user"></a>Erstellen eines CA PPM-Testbenutzers

In diesem Abschnitt erstellen Sie in CA PPM einen Benutzer mit dem Namen Britta Simon. Wenden Sie sich an das [Supportteam von CA PPM](mailto:catechnicalsupport@ca.com), um die Benutzer in der CA PPM-Plattform hinzuzufügen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf CA PPM gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon CA PPM zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **CA PPM**aus.

    ![Configure single sign-on](./media/cappm-tutorial/tutorial_cappm_app.png) 

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „CA PPM“ klicken, sollten Sie automatisch bei Ihrer CA PPM-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/cappm-tutorial/tutorial_general_01.png
[2]: ./media/cappm-tutorial/tutorial_general_02.png
[3]: ./media/cappm-tutorial/tutorial_general_03.png
[4]: ./media/cappm-tutorial/tutorial_general_04.png

[100]: ./media/cappm-tutorial/tutorial_general_100.png

[200]: ./media/cappm-tutorial/tutorial_general_200.png
[201]: ./media/cappm-tutorial/tutorial_general_201.png
[202]: ./media/cappm-tutorial/tutorial_general_202.png
[203]: ./media/cappm-tutorial/tutorial_general_203.png

