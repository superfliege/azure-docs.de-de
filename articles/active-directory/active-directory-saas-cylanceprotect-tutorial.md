---
title: 'Tutorial: Azure Active Directory-Integration mit CylancePROTECT | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und CylancePROTECT konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ea392d8c-c8aa-4475-99d0-b08524ef0f3a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2018
ms.author: jeedes
ms.openlocfilehash: 406ff51e98e1816e1bd041d364313ee2a7b52ca3
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-cylanceprotect"></a>Tutorial: Azure Active Directory-Integration mit CylancePROTECT

In diesem Tutorial erfahren Sie, wie Sie CylancePROTECT in Azure Active Directory (Azure AD) integrieren.

Die Integration von CylancePROTECT in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf CylancePROTECT hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei CylancePROTECT anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit CylancePROTECT konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein CylancePROTECT-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von CylancePROTECT aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-cylanceprotect-from-the-gallery"></a>Hinzufügen von CylancePROTECT aus dem Katalog
Zum Konfigurieren der Integration von CylancePROTECT in Azure AD müssen Sie CylancePROTECT über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um CylancePROTECT über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **CylancePROTECT** ein, wählen Sie im Ergebnispanel **CylancePROTECT** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![CylancePROTECT in der Ergebnisliste](./media/active-directory-saas-cylanceprotect-tutorial/tutorial_cylanceprotect_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei CylancePROTECT mithilfe eines Testbenutzers namens Britta Simon.

Damit SSO funktioniert, muss Azure AD Kenntnis darüber haben, welches das Pendant zu dem Benutzer in CylancePROTECT ist. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in CylancePROTECT muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei CylancePROTECT müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines CylancePROTECT-Testbenutzers](#create-a-cylanceprotect-test-user)**, um ein Pendant von Britta Simon in CylancePROTECT zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer CylancePROTECT-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei CylancePROTECT die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **CylancePROTECT** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-cylanceprotect-tutorial/tutorial_cylanceprotect_samlbase.png)

3. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für CylancePROTECT** aus:

    ![SSO-Informationen zur Domäne und zu den URLs für CylancePROTECT](./media/active-directory-saas-cylanceprotect-tutorial/tutorial_cylanceprotect_url.png)

    a. Geben Sie im Textfeld **Bezeichner** die folgende URL ein:
    
    | Region | URL-Wert |
    |----------|---------|
    | Asien-Pazifik, Nordosten (APNE1)| ` https://login-apne1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Asien-Pazifik, Südosten (AU) | `https://login-au.cylance.com/EnterpriseLogin/ConsumeSaml` |
    | Europa, Mitte (EUC1)|`https://login-euc1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Nordamerika|`https://login.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Südamerika (SAE1)|`https://login-sae1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    
    b. Geben Sie im Textfeld **Antwort-URL** folgende URL ein:
    
    | Region | URL-Wert |
    |----------|---------|
    | Asien-Pazifik, Nordosten (APNE1)|`https://login-apne1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Asien-Pazifik, Südosten (AU)|`https://login-au.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Europa, Mitte (EUC1)|`https://login-euc1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Nordamerika|`https://login.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Südamerika (SAE1)|`https://login-sae1.cylance.com/EnterpriseLogin/ConsumeSaml`|

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-cylanceprotect-tutorial/tutorial_cylanceprotect_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **CylancePROTECT-Konfiguration** auf **CylancePROTECT konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![CylancePROTECT-Konfiguration](./media/active-directory-saas-cylanceprotect-tutorial/tutorial_cylanceprotect_configure.png) 

7. Zum Konfigurieren des einmaligen Anmeldens bei **CylancePROTECT** müssen Sie die heruntergeladenen Daten für **Zertifikat (Base64), Abmelde-URL, SAML-Entitäts-ID und SAML-Dienst-URL für einmaliges Anmelden** an den Konsolenadministrator senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-cylanceprotect-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-cylanceprotect-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-cylanceprotect-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-cylanceprotect-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
  
### <a name="create-a-cylanceprotect-test-user"></a>Erstellen eines CylancePROTECT-Testbenutzers

In diesem Abschnitt erstellen Sie in CylancePROTECT einen Benutzer namens Britta Simon. Arbeiten Sie mit dem Konsolenadministrator zusammen, um die Benutzer in der CylancePROTECT-Plattform hinzuzufügen. Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail und folgt einem Link zur Bestätigung des Kontos, bevor es aktiv wird.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf CylancePROTECT gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um Britta Simon CylancePROTECT zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste die Option **CylancePROTECT** aus.

    ![CylancePROTECT-Link in der Anwendungsliste](./media/active-directory-saas-cylanceprotect-tutorial/tutorial_cylanceprotect_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffspanel auf die Kachel „CylancePROTECT“ klicken, sollten Sie automatisch in Ihrer CylancePROTECT-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_203.png

