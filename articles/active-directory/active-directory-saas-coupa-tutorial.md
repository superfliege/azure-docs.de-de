---
title: 'Tutorial: Azure Active Directory-Integration mit Coupa | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Coupa konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 3e87502cbcdb0eff4310ad154083c7682d752e13
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a>Tutorial: Azure Active Directory-Integration mit Coupa

In diesem Tutorial erfahren Sie, wie Sie Coupa in Azure Active Directory (Azure AD) integrieren.

Die Integration von Coupa in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Coupa hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Coupa anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Coupa konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Coupa-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Coupa aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-coupa-from-the-gallery"></a>Hinzufügen von Coupa aus dem Katalog
Zum Konfigurieren der Integration von Coupa in Azure AD müssen Sie Coupa aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Coupa über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Coupa** ein, wählen Sie im Ergebnisbereich **Coupa** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Coupa in der Ergebnisliste](./media/active-directory-saas-coupa-tutorial/tutorial_coupa_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Coupa mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Coupa als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Coupa muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Coupa den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Coupa müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Coupa-Testbenutzers](#create-a-coupa-test-user)**, um ein Pendant von Britta Simon in Coupa zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Coupa-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Coupa die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Coupa** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-coupa-tutorial/tutorial_coupa_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Coupa** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und den URLs für Coupa](./media/active-directory-saas-coupa-tutorial/tutorial_coupa_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `http://<companyname>.Coupa.com`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `<companyname>.coupahost.com`

    c. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<companyname>.coupahost.com/sp/ACS.saml2`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL, den tatsächlichen Bezeichner und die tatsächliche Antwort-URL. Wenden Sie sich an das [Clientsupportteam von Coupa](https://success.coupa.com/Support/Contact_Us?), um diese Werte zu erhalten. Sie erhalten den Wert der Antwort-URL aus den Metadaten, die später in diesem Tutorial erläutert werden.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-coupa-tutorial/tutorial_coupa_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-coupa-tutorial/tutorial_general_400.png)

6. Melden Sie sich bei der Coupa-Unternehmenswebsite als Administrator an.

7. Navigieren Sie zu **Einrichtung \> Sicherheitskontrollen**.
   
   ![Sicherheitskontrollen](./media/active-directory-saas-coupa-tutorial/ic791900.png "Sicherheitskontrollen")

8. Führen Sie im Abschnitt **Mit Coupa-Anmeldeinformationen anmelden** die folgenden Schritte aus:

    ![Coupa SP-Metadaten](./media/active-directory-saas-coupa-tutorial/ic791901.png "Coupa SP-Metadaten")
    
    a. Wählen Sie **Mit SAML anmelden**aus.
    
    b. Klicken Sie auf **SP-Metadaten herunterladen und importieren**, um die Coupa-Metadatendatei auf Ihren Computer herunterzuladen. Öffnen Sie die Metadaten, kopieren Sie den Wert **AssertionConsumerService index/URL** (Index/URL für den AssertionConsumerService), und fügen Sie ihn im Abschnitt **Domäne und URLs für Coupa** in das Textfeld **Antwort-URL** ein. 
    
    c. Klicken Sie auf **Durchsuchen**, um die Metadaten hochzuladen, die Sie zuvor aus dem Azure-Portal heruntergeladen haben.
    
    d. Klicken Sie auf **Speichern**.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-coupa-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-coupa-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-coupa-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-coupa-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-coupa-test-user"></a>Erstellen eines Coupa-Testbenutzers

Damit sich Azure AD-Benutzer bei Coupa anmelden können, müssen sie in Coupa bereitgestellt werden.  

* Im Fall von Coupa ist die Bereitstellung eine manuelle Aufgabe.

**Um die Benutzerbereitstellung zu konfigurieren, führen Sie die folgenden Schritte durch:**

1. Melden Sie sich bei der **Coupa** -Unternehmenswebsite als Administrator an.

2. Klicken Sie oben im Menü auf **Setup** und dann auf **Benutzer**.
   
   ![Benutzer](./media/active-directory-saas-coupa-tutorial/ic791908.png "Benutzer")

3. Klicken Sie auf **Create**.
   
   ![Benutzer erstellen](./media/active-directory-saas-coupa-tutorial/ic791909.png "Benutzer erstellen")

4. Führen Sie im Abschnitt **Benutzer erstellen** die folgenden Schritte aus:
   
   ![Benutzerdetails](./media/active-directory-saas-coupa-tutorial/ic791910.png "Benutzerdetails")
   
   a. Geben Sie die Attribute **Benutzername**, **Vorname**, **Nachname**, **ID für einmaliges Anmelden** und **E-Mail** eines gültigen Azure Active Directory-Kontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein.

   b. Klicken Sie auf **Create**.   
   
   >[!NOTE]
   >Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird. 
   > 

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Coupa-Benutzerkonten oder mithilfe der von Coupa bereitgestellten APIs erstellen. 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Coupa erteilen.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um Britta Simon Coupa zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste die Option **Coupa**aus.

    ![Coupa-Link in der Anwendungsliste](./media/active-directory-saas-coupa-tutorial/tutorial_coupa_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Coupa-Kachel klicken, sollten Sie automatisch bei Ihrer Coupa-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-coupa-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-coupa-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-coupa-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-coupa-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-coupa-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-coupa-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-coupa-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-coupa-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-coupa-tutorial/tutorial_general_203.png

