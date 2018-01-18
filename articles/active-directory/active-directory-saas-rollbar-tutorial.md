---
title: 'Tutorial: Azure Active Directory-Integration mit Rollbar | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Rollbar konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 57537e54-9388-4272-a610-805ce45a451f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/04/2017
ms.author: jeedes
ms.openlocfilehash: bb8a81327163513ab721d2ad72da19173b59bc1f
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2018
---
# <a name="tutorial-azure-active-directory-integration-with-rollbar"></a>Tutorial: Azure Active Directory-Integration mit Rollbar

In diesem Tutorial erfahren Sie, wie Sie Rollbar in Azure Active Directory (Azure AD) integrieren.

Die Integration von Rollbar in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Rollbar hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Rollbar anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Rollbar konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Rollbar-Abonnement, für das einmaliges Anmelden aktiviert ist.

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Rollbar aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-rollbar-from-the-gallery"></a>Hinzufügen von Rollbar aus dem Katalog
Zum Konfigurieren der Integration von Rollbar in Azure AD müssen Sie Rollbar aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Rollbar aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Rollbar** ein, wählen Sie im Ergebnisbereich **Rollbar** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Rollbar in der Ergebnisliste](./media/active-directory-saas-rollbar-tutorial/tutorial_rollbar_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Rollbar basierend auf einer Testbenutzerin mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Rollbar als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Rollbar muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Rollbar den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Rollbar müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Rollbar-Testbenutzers](#create-a-rollbar-test-user)**, um eine Entsprechung von Britta Simon in Rollbar zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Rollbar-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Rollbar die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Rollbar** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-rollbar-tutorial/tutorial_rollbar_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Rollbar** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und den URLs für Rollbar](./media/active-directory-saas-rollbar-tutorial/tutorial_rollbar_url.png)

    a. Geben Sie im Textfeld **Bezeichner** die folgende URL ein: `https://saml.rollbar.com`.

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://rollbar.com/<accountname>/saml/sso/azure/`

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und den URLs für Rollbar](./media/active-directory-saas-rollbar-tutorial/tutorial_rollbar_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://rollbar.com/<accountname>/saml/login/azure/`.
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Die Werte müssen durch die tatsächliche Antwort-URL und die tatsächliche Anmelde-URL ersetzt werden. Wenden Sie sich an den [Rollbar-Support](mailto:support@rollbar.com), um diese Werte zu erhalten. 

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-rollbar-tutorial/tutorial_rollbar_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-rollbar-tutorial/tutorial_general_400.png)
    
7. Melden Sie sich in einem anderen Webbrowserfenster bei der Rollbar-Unternehmenswebsite als Administrator an.

8. Klicken Sie in der oberen rechten Ecke auf **Profileinstellungen**, und klicken Sie dann auf **Einstellungen für den Kontonamen**.
    
    ![Konfiguration](./media/active-directory-saas-rollbar-tutorial/general.png)

9. Klicken Sie unter SICHERHEIT auf **Identitätsanbieter**.

    ![Konfiguration](./media/active-directory-saas-rollbar-tutorial/configure1.png)

10. Führen Sie im Abschnitt **SAML-Identitätsanbieter** die folgenden Schritte aus:
    
    ![Konfiguration](./media/active-directory-saas-rollbar-tutorial/configure2.png)

    a. Wählen Sie in der Dropdownliste **SAML-Identitätsanbieter** den Wert **AZURE** aus.

    b. Öffnen Sie Ihre Metadatendatei in Editor, kopieren Sie den Inhalt in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **SAML-Metadaten** ein.

    c. Klicken Sie auf **Speichern**.

11. Nach dem Klicken auf die Schaltfläche „Speichern“ sieht der Bildschirm wie folgt aus. Führen Sie in diesem Abschnitt die folgenden Schritte aus:
    
    ![Konfiguration](./media/active-directory-saas-rollbar-tutorial/configure3.png)

    a. Aktivieren Sie das Kontrollkästchen **Anmeldung über SAML-Identitätsanbieter vorschreiben**.

    b. Klicken Sie auf **Speichern**.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-rollbar-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-rollbar-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-rollbar-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-rollbar-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-rollbar-test-user"></a>Erstellen eines Rollbar-Testbenutzers

Damit sich Azure AD-Benutzer bei Rollbar anmelden können, müssen sie in Rollbar bereitgestellt werden. Im Fall von Rollbar ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrer Rollbar-Unternehmenswebsite als Administrator an.

2. Klicken Sie in der oberen rechten Ecke auf **Profileinstellungen**, und klicken Sie dann auf **Einstellungen für den Kontonamen**.

    ![Benutzer](./media/active-directory-saas-rollbar-tutorial/general.png)

3. Klicken Sie auf **Users**.
    
    ![Mitarbeiter hinzufügen](./media/active-directory-saas-rollbar-tutorial/user1.png)

4. Klicken Sie auf **Teammitglieder einladen**.

    ![Invite People](./media/active-directory-saas-rollbar-tutorial/user2.png)

5. Geben Sie im Textfeld den Namen eines Benutzers ein, z. B. **brittasimon@contoso.com**, und klicken Sie dann auf **Hinzufügen/Einladen**.

    ![Invite People](./media/active-directory-saas-rollbar-tutorial/user3.png)

6. Der Benutzer empfängt eine Einladung, und nachdem er/sie diese akzeptiert hat, wird er/sie im System erstellt.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf Rollbar erteilen.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Rollbar zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Rollbar**aus.

    ![Rollbar-Link in der Anwendungsliste](./media/active-directory-saas-rollbar-tutorial/tutorial_rollbar_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Rollbar-Kachel klicken, sollten Sie automatisch bei Ihrer Rollbar-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rollbar-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rollbar-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rollbar-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rollbar-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rollbar-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rollbar-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rollbar-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rollbar-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rollbar-tutorial/tutorial_general_203.png

