---
title: 'Tutorial: Azure Active Directory-Integration mit Vodeclic | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Vodeclic konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d77a0f53-e3a3-445e-ab3e-119cef6e2e1d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: jeedes
ms.openlocfilehash: 940c7bb5040fb91a03b01dc43ee07d52e3d4e63b
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2017
---
# <a name="tutorial-azure-active-directory-integration-with-vodeclic"></a>Tutorial: Azure Active Directory-Integration mit Vodeclic

In diesem Tutorial erfahren Sie, wie Sie Vodeclic in Azure Active Directory (Azure AD) integrieren.

Die Integration von Vodeclic in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Vodeclic hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Vodeclic anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Vodeclic konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges Vodeclic-Abonnement

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige kostenlose Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Vodeclic aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="add-vodeclic-from-the-gallery"></a>Hinzufügen von Vodeclic aus dem Katalog
Zum Konfigurieren der Integration von Vodeclic in Azure AD müssen Sie Vodeclic über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Vodeclic aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) auf das Symbol **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen ausführen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Wählen Sie oben im Dialogfeld die Schaltfläche **Neue Anwendung** aus, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld den Suchbegriff **Vodeclic** ein. Wählen Sie im Ergebnisbereich **Vodeclic** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Vodeclic in der Ergebnisliste](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Vodeclic mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Vodeclic als Pendant eines Benutzers in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Vodeclic muss eine Verknüpfung eingerichtet werden.

Weisen Sie in Vodeclic dem Wert **Benutzername** denselben Wert wie dem **Benutzernamen** in Azure AD zu. Damit haben Sie die Verknüpfung zwischen den beiden Benutzern eingerichtet.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Vodeclic müssen Sie die folgenden Aufgaben ausführen:

1. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on), um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. [Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user), um das einmalige Anmelden mit Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. [Erstellen eines Vodeclic-Testbenutzers](#create-a-vodeclic-test-user), um ein Pendant von Britta Simon in Vodeclic zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
4. [Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user), um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. [Testen der einmaligen Anmeldung](#test-single-sign-on), um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Vodeclic-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Vodeclic die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Vodeclic** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** unter **SSO-Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Vodeclic** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Vodeclic](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_url.png)

    a. Geben Sie im Feld **Bezeichner** eine URL im folgenden Format ein: `https://<companyname>.lms.vodeclic.net/auth/saml`

    b. Geben Sie im Feld **Antwort-URL** eine URL im folgenden Format ein: `https://<companyname>.lms.vodeclic.net/auth/saml/callback`

4. Wenn Sie die Anwendung im **SP**-initiierten Modus konfigurieren möchten, aktivieren Sie das Kontrollkästchen **Erweiterte Einstellungen anzeigen** und führen die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Vodeclic](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_url1.png)

    Geben Sie im Feld **URL für Anmeldung** eine URL im folgenden Format ein: `https://<companyname>.lms.vodeclic.net/auth/saml`.
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte mit dem tatsächlichen Bezeichner, der Antwort-URL und der Anmelde-URL aktualisieren. Wenden Sie sich an den [Vodeclic-Support](mailto:hotline@vodeclic.com), um diese Werte zu erhalten.

5. Wählen Sie im Abschnitt **SAML-Signaturzertifikat** die Option **Metadaten-XML** aus. Speichern Sie dann die Metadatendatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_certificate.png) 

6. Wählen Sie **Speichern** aus.

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-vodeclic-tutorial/tutorial_general_400.png)
    
7. Zum Konfigurieren des einmaligen Anmeldens bei **Vodeclic** müssen Sie die heruntergeladene **Metadaten-XML**-Datei an das [Vodeclic-Supportteam](mailto:hotline@vodeclic.com) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen. Nachdem Sie diese App über den Abschnitt **Active Directory** > **Unternehmensanwendungen** hinzugefügt haben, navigieren Sie zur Registerkarte **Einmaliges Anmelden**, und rufen Sie am unteren Rand im Abschnitt **Konfiguration** die eingebettete Dokumentation auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Wählen Sie im linken Bereich des Azure-Portals die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_01.png)

2. Um die Liste der Benutzer anzuzeigen, wechseln Sie zu **Benutzer und Gruppen**. Wählen Sie dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_02.png)

3. Klicken Sie im oberen Bereich des Dialogfelds **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-vodeclic-test-user"></a>Erstellen eines Vodeclic-Testbenutzers

In diesem Abschnitt erstellen Sie in Vodeclic einen Benutzer namens Britta Simon. Lassen Sie sich beim Hinzufügen der Benutzer zur Vodeclic-Plattform ggf. vom [Vodeclic-Supportteam](mailto:hotline@vodeclic.com) unterstützen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

> [!NOTE]
> Wenn die Anwendung dies erfordert, müssen Sie Ihren Computer der Whitelist hinzufügen. Dazu müssen Sie dem [Vodeclic-Supportteam](mailto:hotline@vodeclic.com) Ihre öffentliche IP-Adresse mitteilen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Vodeclic gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um Britta Simon Vodeclic zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, und navigieren Sie zur Verzeichnisansicht. Navigieren Sie nun zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Vodeclic** aus.

    ![Vodeclic-Link in der Anwendungsliste](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_app.png)  

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“][202]

4. Wählen Sie die Schaltfläche **Hinzufügen** aus. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** unter **Benutzer** die Option **Britta Simon** aus.

6. Wählen Sie im Dialogfeld **Benutzer und Gruppen** die Schaltfläche **Auswählen**.

7. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Vodeclic“ klicken, sollten Sie automatisch bei Ihrer Vodeclic-Anwendung angemeldet werden.

Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_203.png

