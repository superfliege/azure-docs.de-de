---
title: 'Tutorial: Azure Active Directory-Integration mit Salesforce Sandbox | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Salesforce Sandbox konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2018
ms.author: jeedes
ms.openlocfilehash: b04e4a83862cdec26e0432eb7d8b3419e36d5a7a
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55176419"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Tutorial: Azure Active Directory-Integration mit Salesforce Sandbox

In diesem Tutorial erfahren Sie, wie Sie Salesforce Sandbox in Azure Active Directory (Azure AD) integrieren.

Sandboxes bieten Ihnen die Möglichkeit, mehrere Kopien Ihres Unternehmens in separaten Umgebungen für eine Vielzahl von Zwecken, wie z. B. Entwicklung, Tests und Schulungen, ohne eine Beeinträchtigung der Daten und Anwendungen in Ihrer Vertriebsproduktions-Organisation zu erstellen.
Weitere Informationen finden Sie in der [Übersicht zu Sandbox](https://help.salesforce.com/articleView?id=create_test_instance.htm&language=en_us&type=5).

Die Integration von Salesforce Sandbox in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Salesforce Sandbox hat.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Salesforce Sandbox anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in Salesforce Sandbox konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Salesforce Sandbox-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Salesforce Sandbox aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Hinzufügen von Salesforce Sandbox aus dem Katalog

Zum Konfigurieren der Integration von Salesforce Sandbox in Azure AD müssen Sie Salesforce Sandbox aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um Salesforce Sandbox aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie in das Suchfeld den Namen **Salesforce Sandbox** ein, wählen Sie **Salesforce Sandbox** im Ergebnisbereich aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Salesforce Sandbox in der Ergebnisliste](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Salesforce Sandbox mithilfe einer Testbenutzerin namens „Britta Simon“.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Salesforce Sandbox als Pendant für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Salesforce Sandbox muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Salesforce Sandbox den Wert für **Benutzername** aus Azure AD als Wert für **Benutzername** zu, um die Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Salesforce Sandbox müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Salesforce Sandbox-Testbenutzers](#create-a-salesforce-sandbox-test-user)**, um ein Pendant von Britta Simon in Salesforce Sandbox zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Salesforce Sandbox-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD in Salesforce Sandbox zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Salesforce Sandbox** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Klicken Sie oben im Bildschirm auf **Modus für einmaliges Anmelden ändern**, um den Modus **SAML** auszuwählen.

      ![Konfigurieren des Links für einmaliges Anmelden](./media/salesforce-sandbox-tutorial/tutorial_general_300.png)

3. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![Konfigurieren des Links für einmaliges Anmelden](./media/salesforce-sandbox-tutorial/tutorial_general_301.png)

4. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.
   
    ![Konfigurieren des Links für einmaliges Anmelden](./media/salesforce-sandbox-tutorial/tutorial_general_302.png)

5. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    a. Klicken Sie auf **Metadatendatei hochladen**.

    ![Metadatendatei hochladen](./media/salesforce-sandbox-tutorial/upload_metadata.png)

    b. Klicken Sie auf das **Ordnerlogo**, wählen Sie die Metadatendatei aus, und klicken Sie auf **Hochladen**.

    ![Metadatendatei auswählen](./media/salesforce-sandbox-tutorial/browse_upload_metadata.png)

    > [!NOTE]
    > Sie können die Dienstanbieter-Metadatendatei im Salesforce Sandbox-Verwaltungsportal abrufen, was im weiteren Verlauf des Tutorials erläutert wird.

    c. Nachdem die Metadatendatei erfolgreich hochgeladen wurde, wird der Wert von **Antwort-URL** automatisch im Textfeld **Antwort-URL** aufgefüllt.

    ![SSO-Informationen zur Domäne und zu den URLs für Salesforce Sandbox](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url1.png)

6. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um die Datei **Verbundmetadaten-XML** herunterzuladen, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png)

7. Öffnen Sie eine neue Registerkarte in Ihrem Browser, und melden Sie sich mit Ihrem Salesforce Sandbox-Administratorkonto an.

8. Klicken Sie in der oberen rechten Ecke der Seite unter dem **Einstellungssymbol** auf **Setup**.

    ![Configure single sign-on](./media/salesforce-sandbox-tutorial/configure1.png)

9. Scrollen Sie im linken Navigationsbereich nach unten bis zu den **EINSTELLUNGEN**, und klicken Sie auf **Identität**, um den zugehörigen Bereich zu erweitern. Klicken Sie dann auf **Einstellungen für einmaliges Anmelden**.

    ![Configure single sign-on](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

10. Klicken Sie auf der Seite **Einstellungen für einmaliges Anmelden** auf die Schaltfläche **Bearbeiten**.

    ![Configure single sign-on](./media/salesforce-sandbox-tutorial/configure3.png)

11. Wählen Sie **SAML aktiviert**, und klicken Sie dann auf **Speichern**.

    ![Configure single sign-on](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

12. Klicken Sie auf **Neu aus Metadatendatei**, um Ihre SAML-Einstellungen für einmaliges Anmelden zu konfigurieren.

    ![Configure single sign-on](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

13. Klicken Sie auf **Datei auswählen**, um die Metadaten-XML-Datei hochzuladen, die Sie aus dem Azure-Portal heruntergeladen haben, und klicken Sie dann auf **Erstellen**.

    ![Configure single sign-on](./media/salesforce-sandbox-tutorial/xmlchoose.png)

14. Auf der Seite **SAML-Einstellungen für einmaliges Anmelden** werden die Felder automatisch aufgefüllt. Klicken Sie auf „Speichern“.

    ![Configure single sign-on](./media/salesforce-sandbox-tutorial/salesforcexml.png)

15. Klicken Sie auf der Seite **Einstellungen für einmaliges Anmelden** auf die Schaltfläche **Metadaten herunterladen**, um die Dienstanbieter-Metadatendatei herunterzuladen. Verwenden Sie diese Datei im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal, um die notwendigen URLs wie oben erläutert zu konfigurieren.

    ![Configure single sign-on](./media/salesforce-sandbox-tutorial/configure4.png)

16. Wenn Sie die Anwendung im **SP**-initiierten Modus konfigurieren möchten, sind dies die Voraussetzungen:

    a. Sie sollten über eine überprüfte Domäne verfügen.

    b. Sie müssen Ihre Domäne in Salesforce Sandbox konfigurieren und aktivieren. Die entsprechenden Schritte werden später in diesem Tutorial erläutert.

    c. Klicken Sie im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus:
  
    ![SSO-Informationen zur Domäne und zu den URLs für Salesforce Sandbox](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url.png)

    Geben Sie im Textfeld **Anmelde-URL** den Wert im folgenden Format ein: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`.

    > [!NOTE]
    > Dieser Wert sollte aus dem Salesforce Sandbox-Portal kopiert werden, nachdem Sie die Domäne aktiviert haben.

17. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Verbundmetadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png)

18. Öffnen Sie eine neue Registerkarte in Ihrem Browser, und melden Sie sich mit Ihrem Salesforce Sandbox-Administratorkonto an.

19. Klicken Sie in der oberen rechten Ecke der Seite unter dem **Einstellungssymbol** auf **Setup**.

    ![Configure single sign-on](./media/salesforce-sandbox-tutorial/configure1.png)

20. Scrollen Sie im linken Navigationsbereich nach unten bis zu den **EINSTELLUNGEN**, und klicken Sie auf **Identität**, um den zugehörigen Bereich zu erweitern. Klicken Sie dann auf **Einstellungen für einmaliges Anmelden**.

    ![Configure single sign-on](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

21. Klicken Sie auf der Seite **Einstellungen für einmaliges Anmelden** auf die Schaltfläche **Bearbeiten**.

    ![Configure single sign-on](./media/salesforce-sandbox-tutorial/configure3.png)

22. Wählen Sie **SAML aktiviert**, und klicken Sie dann auf **Speichern**.

    ![Configure single sign-on](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

23. Klicken Sie auf **Neu aus Metadatendatei**, um Ihre SAML-Einstellungen für einmaliges Anmelden zu konfigurieren.

    ![Configure single sign-on](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

24. Klicken Sie auf **Datei auswählen**, um die Metadaten-XML-Datei hochzuladen, und klicken Sie auf **Erstellen**.

    ![Configure single sign-on](./media/salesforce-sandbox-tutorial/xmlchoose.png)

25. Auf der Seite **SAML-Einstellungen für einmaliges Anmelden** werden Felder automatisch aufgefüllt. Geben Sie den Namen der Konfiguration (z.B. *SPSSOWAAD_Test*) in das Textfeld **Name** ein, und klicken Sie auf „Speichern“.

    ![Configure single sign-on](./media/salesforce-sandbox-tutorial/sf-saml-config.png)

26. Führen Sie die folgenden Schritte aus, um Ihre Domäne in Salesforce Sandbox zu aktivieren.

    > [!NOTE]
    > Vor dem Aktivieren der Domäne müssen Sie die gleiche Domäne in Salesforce Sandbox erstellen. Weitere Informationen finden Sie unter [Defining Your Domain Name](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US) (Festlegen des Domänennamens). Nachdem die Domäne erstellt wurde, sollten Sie sicherstellen, dass diese richtig konfiguriert wurde.

    * Klicken Sie in Salesforce Sandbox im linken Navigationsbereich auf **Unternehmenseinstellungen**, um den zugehörigen Abschnitt zu erweitern, und klicken Sie dann auf **My Domain** (Meine Domäne).

         ![Configure single sign-on](./media/salesforce-sandbox-tutorial/sf-my-domain.png)

    * Klicken Sie im Abschnitt **Authentifizierungskonfiguration** auf **Bearbeiten**.

        ![Configure single sign-on](./media/salesforce-sandbox-tutorial/sf-edit-auth-config.png)

    * Wählen Sie im Abschnitt **Authentifizierungskonfiguration** für den **Authentifizierungsdienst** den Namen der SAML-Einstellung für einmaliges Anmelden aus, den Sie während der SSO-Konfiguration in Salesforce Sandbox festgelegt haben, und klicken Sie auf **Speichern**.

        ![Configure single sign-on](./media/salesforce-sandbox-tutorial/configure2.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Azure AD-Benutzer erstellen][100]

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Erstellen eines Azure AD-Testbenutzers](./media/salesforce-sandbox-tutorial/create_aaduser_01.png) 

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Erstellen eines Azure AD-Testbenutzers](./media/salesforce-sandbox-tutorial/create_aaduser_02.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

### <a name="create-a-salesforce-sandbox-test-user"></a>Erstellen eines Salesforce Sandbox-Testbenutzers

In diesem Abschnitt wird ein Benutzer mit dem Namen Britta Simon in Salesforce Sandbox erstellt. Salesforce Sandbox unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Falls ein Benutzer nicht bereits in Salesforce Sandbox vorhanden ist, wird beim Versuch, auf Salesforce Sandbox zuzugreifen, ein neuer Benutzer erstellt. Außerdem unterstützt Salesforce Sandbox die automatische Benutzerbereitstellung. Weitere Informationen zum Konfigurieren der automatischen Benutzerbereitstellung finden Sie [hier](salesforce-sandbox-provisioning-tutorial.md).

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Salesforce Sandbox gewähren.

![Zuweisen der Benutzerrolle][200]

**Um Britta Simon Salesforce Sandbox zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste der Anwendungen **Salesforce Sandbox** aus.

    ![Der Salesforce Sandbox-Link in der Liste der Anwendungen](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Salesforce Sandbox“ klicken, sollten Sie automatisch bei Ihrer Salesforce Sandbox-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Konfigurieren der Benutzerbereitstellung](salesforce-sandbox-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/salesforce-sandbox-tutorial/tutorial_general_01.png
[2]: ./media/salesforce-sandbox-tutorial/tutorial_general_02.png
[3]: ./media/salesforce-sandbox-tutorial/tutorial_general_03.png
[4]: ./media/salesforce-sandbox-tutorial/tutorial_general_04.png

[100]: ./media/salesforce-sandbox-tutorial/tutorial_general_100.png

[200]: ./media/salesforce-sandbox-tutorial/tutorial_general_200.png
[201]: ./media/salesforce-sandbox-tutorial/tutorial_general_201.png
[202]: ./media/salesforce-sandbox-tutorial/tutorial_general_202.png
[203]: ./media/salesforce-sandbox-tutorial/tutorial_general_203.png
