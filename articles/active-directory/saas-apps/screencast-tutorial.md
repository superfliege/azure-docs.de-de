---
title: 'Tutorial: Azure Active Directory-Integration mit Screencast-O-Matic | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Screencast-O-Matic konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 525ad47d-5488-40e2-aeaf-ae6183745682
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 941a02ef9ab3d623d34c8dc6996040f6d9e44859
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58096842"
---
# <a name="tutorial-azure-active-directory-integration-with-screencast-o-matic"></a>Tutorial: Azure Active Directory-Integration mit Screencast-O-Matic

In diesem Tutorial erfahren Sie, wie Sie Screencast-O-Matic in Azure Active Directory (Azure AD) integrieren.

Die Integration von Screencast-O-Matic in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Screencast-O-Matic hat.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Screencast-O-Matic anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Screencast-O-Matic-Integration in Azure AD konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Screencast-O-Matic-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Screencast-O-Matic aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-screencast-o-matic-from-the-gallery"></a>Hinzufügen von Screencast-O-Matic aus dem Katalog
Zum Konfigurieren der Integration von Screencast-O-Matic in Azure AD müssen Sie Screencast-O-Matic über den Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Screencast-O-Matic aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Screencast-O-Matic** ein, wählen Sie im Ergebnisbereich **Screencast-O-Matic** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Screencast-O-Matic in der Ergebnisliste](./media/screencast-tutorial/tutorial_screencast_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Screencast-O-Matic mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Screencast-O-Matic als Pendant für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Screencast-O-Matic muss eine Linkbeziehung eingerichtet werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD in Screencast-O-Matic zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Screencast-O-Matic-Testbenutzers](#create-a-screencast-o-matic-test-user)**, um eine Entsprechung von Britta Simon in Screencast-O-Matic zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Screencast-O-Matic-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD in Screencast-O-Matic zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Screencast-O-Matic** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/screencast-tutorial/tutorial_screencast_samlbase.png)

3. Führen Sie auf der Seite **Domäne und URLs für Screencast-O-Matic** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Screencast-O-Matic](./media/screencast-tutorial/tutorial_screencast_url.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://screencast-o-matic.com/<InstanceName>`.

    > [!NOTE] 
    > Der Wert der Anmelde-URL entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Kundensupportteam von Screencast-O-Matic](mailto:support@screencast-o-matic.com), um den Wert zu erhalten. 
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/screencast-tutorial/tutorial_screencast_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/screencast-tutorial/tutorial_general_400.png)

6. Melden Sie sich in einem anderen Webbrowserfenster als Administrator bei Screencast-O-Matic an.

7. Klicken Sie auf **Subscription (Abonnement)**.

    ![Das Abonnement](./media/screencast-tutorial/tutorial_screencast_sub.png)

8. Klicken Sie im Abschnitt **Access page (Zugriffsseite)** auf **Setup**.

    ![Der Zugriff](./media/screencast-tutorial/tutorial_screencast_setup.png)

9. Führen Sie auf der **Setup Access Page (Setupzugriffsseite)** die folgenden Schritte aus:

   * Geben Sie im Abschnitt **Access URL (Zugriffs-URL)** Ihren Instanznamen in das angegebene Textfeld ein.

     ![Der Zugriff](./media/screencast-tutorial/tutorial_screencast_access.png)

   * Wählen Sie **Require Domain User (Domänenbenutzer erfordern)** im Abschnitt **SAML User Restriction (optional) (SAML-Benutzereinschränkung (optional))** aus.

   * Klicken Sie unter **Upload IDP Metadata XML File (IDP-Metadaten-XML-Datei hochladen)** auf **Choose File (Datei auswählen)**, um die Metadaten hochzuladen, die Sie im Azure-Portal heruntergeladen haben.

   * Klicken Sie auf **OK**. 

     ![Der Zugriff](./media/screencast-tutorial/tutorial_screencast_save.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/screencast-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/screencast-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/screencast-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/screencast-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-screencast-o-matic-test-user"></a>Erstellen eines Screencast-O-Matic-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen einer Benutzerin namens Britta Simon in Screencast-O-Matic. Screencast-O-Matic unterstützt die Just-In-Time-Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf Screencast-O-Matic ein neuer Benutzer erstellt.

>[!Note]
>Wenn Sie einen Benutzer manuell erstellen müssen, wenden Sie sich an das  [Screencast-O-Matic-Clientsupportteam](mailto:support@screencast-o-matic.com).

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Screencast-O-Matic gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu Screencast-O-Matic durchzuführen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Screencast-O-Matic** aus.

    ![Screencast-O-Matic-Link in der Anwendungsliste](./media/screencast-tutorial/tutorial_screencast_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Screencast-O-Matic“ klicken, sollten Sie automatisch bei Ihrer Screencast-O-Matic-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/screencast-tutorial/tutorial_general_01.png
[2]: ./media/screencast-tutorial/tutorial_general_02.png
[3]: ./media/screencast-tutorial/tutorial_general_03.png
[4]: ./media/screencast-tutorial/tutorial_general_04.png

[100]: ./media/screencast-tutorial/tutorial_general_100.png

[200]: ./media/screencast-tutorial/tutorial_general_200.png
[201]: ./media/screencast-tutorial/tutorial_general_201.png
[202]: ./media/screencast-tutorial/tutorial_general_202.png
[203]: ./media/screencast-tutorial/tutorial_general_203.png

