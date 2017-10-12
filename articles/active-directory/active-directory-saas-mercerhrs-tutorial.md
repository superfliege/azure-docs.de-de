---
title: 'Tutorial: Azure Active Directory-Integration mit Mercer BenefitsCentral (MBC) | Microsoft-Dokumentation'
description: In diesem Artikel erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Mercer BenefitsCentral (MBC) konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3788b28c-49aa-4208-9acd-630362008e89
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2017
ms.author: jeedes
ms.openlocfilehash: c577a0163af04bec5737a215e788c3fb92c653c5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mercer-benefitscentral-mbc"></a>Tutorial: Azure Active Directory-Integration mit Mercer BenefitsCentral (MBC)

In diesem Tutorial erfahren Sie, wie Sie Mercer BenefitsCentral (MBC) in Azure Active Directory (Azure AD) integrieren.

Die Integration von Mercer BenefitsCentral (MBC) in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Mercer BenefitsCentral (MBC) hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Mercer BenefitsCentral (MBC) anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration mit Mercer BenefitsCentral (MBC) benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Mercer BenefitsCentral (MBC)-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Mercer BenefitsCentral (MBC) aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-mercer-benefitscentral-mbc-from-the-gallery"></a>Hinzufügen von Mercer BenefitsCentral (MBC) aus dem Katalog
Zum Integrieren von Mercer BenefitsCentral (MBC) in Azure AD müssen Sie Mercer BenefitsCentral (MBC) aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Mercer BenefitsCentral (MBC) aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld den Suchbegriff **Mercer BenefitsCentral (MBC)** ein, wählen Sie im Ergebnisbereich **Mercer BenefitsCentral (MBC)** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Mercer BenefitsCentral (MBC) in der Ergebnisliste](./media/active-directory-saas-mercerhrs-tutorial/tutorial_mercerhrs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Mercer BenefitsCentral (MBC) basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Mercer BenefitsCentral (MBC) als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Mercer BenefitsCentral (MBC) muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Mercer BenefitsCentral (MBC) den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Mercer BenefitsCentral (MBC) müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Mercer BenefitsCentral-Testbenutzers (MBC)](#create-a-mercer-benefitscentral-mbc-test-user)**, um eine Entsprechung von Britta Simon in Mercer BenefitsCentral (MBC) zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Mercer BenefitsCentral (MBC)-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Mercer BenefitsCentral (MBC) die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Mercer BenefitsCentral (MBC)** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-mercerhrs-tutorial/tutorial_mercerhrs_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Mercer BenefitsCentral (MBC)** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Mercer BenefitsCentral (MBC)](./media/active-directory-saas-mercerhrs-tutorial/tutorial_mercerhrs_url.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `stg.mercerhrs.com/saml2.0`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://ssous-stg.mercerhrs.com/SP2/Saml2AssertionConsumer.aspx`

    > [!NOTE] 
    > Der Wert der Antwort-URL entspricht nicht dem tatsächlichen Wert. Aktualisieren Sie den Wert mit der richtigen Antwort-URL. Wenden Sie sich an das [Supportteam von Mercer BenefitsCentral (MBC)](https://www.mercer.com/contact-us.html), um diesen Wert zu erhalten.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-mercerhrs-tutorial/tutorial_mercerhrs_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Mercer BenefitsCentral (MBC)-Konfiguration** auf **Mercer BenefitsCentral (MBC) konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Konfiguration von Mercer BenefitsCentral (MBC)](./media/active-directory-saas-mercerhrs-tutorial/tutorial_mercerhrs_configure.png) 

7. Zum Konfigurieren des einmaligen Anmeldens aufseiten von **Mercer BenefitsCentral (MBC)** müssen Sie die heruntergeladene **Metadaten-XML** und die **URL für den SAML-SSO-Dienst** an das [Supportteam von Mercer BenefitsCentral (MBC)](https://www.mercer.com/contact-us.html) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-mercerhrs-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-mercerhrs-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-mercerhrs-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-mercerhrs-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
  
### <a name="create-a-mercer-benefitscentral-mbc-test-user"></a>Erstellen eines Mercer BenefitsCentral (MBC)-Testbenutzers

In diesem Abschnitt erstellen Sie in Mercer HRS einen Benutzer namens Britta Simon. Arbeiten Sie mit dem [Supportteam von Mercer BenefitsCentral (MBC)](https://www.mercer.com/contact-us.html) zusammen, um die Benutzer auf der Mercer HRS-Plattform hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Mercer BenefitsCentral (MBC) gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu Mercer BenefitsCentral (MBC) durchzuführen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste die Option **Mercer BenefitsCentral (MBC)** aus.

    ![Link für Mercer BenefitsCentral (MBC) in der Anwendungsliste](./media/active-directory-saas-mercerhrs-tutorial/tutorial_mercerhrs_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel für Mercer BenefitsCentral (MBC) klicken, sollten Sie automatisch bei Ihrer Mercer BenefitsCentral (MBC)-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_203.png

