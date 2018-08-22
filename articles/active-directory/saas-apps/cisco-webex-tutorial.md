---
title: 'Tutorial: Azure Active Directory-Integration mit Cisco Webex | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Cisco Webex konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2018
ms.author: jeedes
ms.openlocfilehash: 73e20afdcacec76482f8ebf01bf2cef2105912a6
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005517"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Tutorial: Azure Active Directory-Integration mit Cisco Webex

In diesem Tutorial erfahren Sie, wie Sie Cisco Webex in Azure Active Directory (Azure AD) integrieren.

Die Integration von Cisco Webex in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf Cisco Webex Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Cisco Webex anzumelden.
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Cisco Webex konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Cisco Webex-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige kostenlose Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Cisco Webex aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="add-cisco-webex-from-the-gallery"></a>Hinzufügen von Cisco Webex aus dem Katalog
Zum Konfigurieren der Integration von Cisco Webex in Azure AD müssen Sie Cisco Webex aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Cisco Webex aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) auf das Symbol **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen ausführen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Wählen Sie oben im Dialogfeld die Schaltfläche **Neue Anwendung** aus, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld den Begriff **Cisco Webex** ein. 

5. Wählen Sie im Ergebnisbereich **Cisco Webex** aus. Wählen Sie dann die Schaltfläche **Hinzufügen** aus, um die Anwendung hinzuzufügen.

    ![Cisco Webex in der Ergebnisliste](./media/cisco-webex-tutorial/tutorial_ciscowebex_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Cisco Webex mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Cisco Webex als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und einem entsprechenden Benutzer in Cisco Webex muss eine Verknüpfung eingerichtet werden.

Weisen Sie in Cisco Webex dem Wert **Username** denselben Wert wie dem **Benutzernamen** in Azure AD zu. Damit haben Sie die Verknüpfung zwischen den beiden Benutzern eingerichtet. 

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Cisco Webex müssen Sie die folgenden Aufgaben ausführen:

1. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on), um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. [Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user), um das einmalige Anmelden mit Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. [Erstellen eines Cisco Webex-Testbenutzers](#create-a-cisco-webex-test-user), um ein Pendant zu Britta Simon in Cisco Webex zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. [Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user), um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. [Testen des einmaligen Anmeldens](#test-single-sign-on), um sicherzustellen, dass die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Cisco Webex-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Cisco Webex die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Cisco Webex** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie zum Aktivieren des einmaligen Anmeldens im Dialogfeld **Einmaliges Anmelden** in der Dropdownliste **Modus** die Option **SAML-basierte Anmeldung** aus.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/cisco-webex-tutorial/tutorial_ciscowebex_samlbase.png)

3. Melden Sie sich in einem anderen Webbrowserfenster bei der Cisco Webex-Unternehmenswebsite als Administrator an.

4. Klicken Sie links im Menü auf **Settings** (Einstellungen).

    ![Configure single sign-on](./media/cisco-webex-tutorial/tutorial_cisco_webex_10.png)

5. Klicken Sie auf der Einstellungsseite im Bereich **Authentication** (Authentifizierung) auf **Modify** (Ändern).

    ![Configure single sign-on](./media/cisco-webex-tutorial/tutorial_cisco_webex_14.png)

6. Wählen Sie **Integrate a 3rd-party identity provider. (Advanced)** (Identitätsanbieter von Drittanbieter integrieren (Erweitert)) aus, und wechseln Sie zum nächsten Bildschirm.

    ![Configure single sign-on](./media/cisco-webex-tutorial/tutorial_cisco_webex_15.png)

7. Klicken Sie auf der Seite **Export Directory Metadata** (Verzeichnismetadaten exportieren) auf **Download Metadata File** (Metadatendatei herunterladen), um die Metadatendatei herunterzuladen.

    ![Configure single sign-on](./media/cisco-webex-tutorial/tutorial_cisco_webex_16.png)

8. Laden Sie im Azure-Portal im Abschnitt **Domäne und URLs für Cisco Webex** die heruntergeladene **Metadatendatei des Dienstanbieters** herunter, und konfigurieren Sie die Anwendung folgendermaßen:

    a. Klicken Sie auf **Metadatendatei hochladen**.

    ![SSO-Informationen zur Domäne und zu den URLs für Cisco Webex](./media/cisco-webex-tutorial/tutorial_ciscowebex_upload.png)

    b. Klicken Sie auf das **Ordnerlogo**, wählen Sie die Metadatendatei aus, und klicken Sie auf **Hochladen**.

    ![SSO-Informationen zur Domäne und zu den URLs für Cisco Webex](./media/cisco-webex-tutorial/tutorial_ciscowebex_uploadconfig.png)

    c. Nachdem Sie die **Metadatendatei des Dienstanbieters** hochgeladen haben, werden im Abschnitt **Domäne und URLs für Cisco Webex** die Werte für **Bezeichner** und **Antwort-URL** automatisch ausgefüllt, wie unten dargestellt:

    ![SSO-Informationen zur Domäne und zu den URLs für Cisco Webex](./media/cisco-webex-tutorial/tutorial_ciscowebex_url.png)

    d. Geben Sie im Feld **URL für Anmeldung** eine URL im folgenden Format ein: `https://<SUBDOMAIN>.webex.com/`.
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie die Werte mit der tatsächlichen Anmelde-URL. Wenden Sie sich an das [Clientsupportteam von Cisco Webex](https://www.webex.co.in/support/support-overview.html), um diese Werte zu erhalten.

9. Die Cisco Webex-Anwendung erwartet, dass die SAML-Assertionen bestimmte Attribute enthalten. Konfigurieren Sie die folgenden Attribute für diese Anwendung. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:
    
    ![Configure single sign-on](./media/cisco-webex-tutorial/tutorial_ciscowebex_07.png) 

10. Konfigurieren Sie das SAML-Tokenattribut im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute**, wie im obigen Bild gezeigt, und führen Sie die folgenden Schritte aus:
    
    |  Attributname  | Attributwert |
    | --------------- | -------------------- |    
    |   firstname    | user.givenname |
    |   lastname    | user.surname |
    |   uid    | user.mail |

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Configure single sign-on](./media/cisco-webex-tutorial/tutorial_attribute_04.png)

    ![Configure single sign-on](./media/cisco-webex-tutorial/tutorial_attribute_05.png)
    
    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.
    
    d. Klicken Sie auf **OK**.

11. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/cisco-webex-tutorial/tutorial_ciscowebex_certificate.png) 

12. Wählen Sie **Speichern**aus.

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/cisco-webex-tutorial/tutorial_general_400.png)
    
13. Verwenden Sie auf der Cisco Webex-Unternehmenswebsite für Administratoren die Dateisuchoption, um die Azure AD-Metadatendatei zu suchen und hochzuladen. Wählen Sie anschließend **Require certificate signed by a certificate authority in Metadata (more secure)** (Signatur einer Zertifizierungsstelle in den Metadaten erzwingen (sicherer)) aus, und wechseln Sie zum nächsten Bildschirm. 

    ![Configure single sign-on](./media/cisco-webex-tutorial/tutorial_cisco_webex_11.png)

14. Wählen Sie **Test SSO Connection** (SSO-Verbindung testen) aus, und authentifizieren Sie sich, wenn eine neue Browserregisterkarte geöffnet wird, durch Ihre Anmeldung bei Azure AD.

15. Wechseln Sie zurück zur Browserregisterkarte **Cisco Cloud Collaboration Management**. Wenn der Test erfolgreich war, wählen Sie **This test was successful. Enable Single Sign-On option** (Der Test war erfolgreich. Einmaliges Anmelden aktivieren), und klicken Sie auf **Speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Wählen Sie im linken Bereich des Azure-Portals die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/cisco-webex-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und wählen Sie dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/cisco-webex-tutorial/create_aaduser_02.png)

3. Klicken Sie im oberen Bereich des Dialogfelds **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/cisco-webex-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/cisco-webex-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-cisco-webex-test-user"></a>Erstellen eines Cisco Webex-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Cisco Webex. Cisco Webex unterstützt die Just-in-Time-Bereitstellung und die automatische Benutzerbereitstellung, die standardmäßig aktiviert ist. Weitere Details zur Konfiguration der automatischen Benutzerbereitstellung finden Sie [hier](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial).

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie der Benutzerin Britta Simon das einmalige Anmelden von Azure, indem Sie Zugriff auf Cisco Webex erteilen.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um Britta Simon Cisco Webex zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht. Wechseln Sie als Nächstes zur Verzeichnisansicht und dann zu **Unternehmensanwendungen**.  

2. Wählen Sie **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201] 

3. Wählen Sie in der Anwendungsliste **Cisco Webex**aus.

    ![Cisco Webex-Link in der Anwendungsliste](./media/cisco-webex-tutorial/tutorial_ciscowebex_app.png)  

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“][202]

4. Wählen Sie die Schaltfläche **Hinzufügen** aus. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** unter **Benutzer** die Option **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel „Cisco Webex“ auswählen, werden Sie automatisch bei Ihrer Cisco Webex-Anwendung angemeldet.

Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/cisco-webex-tutorial/tutorial_general_01.png
[2]: ./media/cisco-webex-tutorial/tutorial_general_02.png
[3]: ./media/cisco-webex-tutorial/tutorial_general_03.png
[4]: ./media/cisco-webex-tutorial/tutorial_general_04.png

[100]: ./media/cisco-webex-tutorial/tutorial_general_100.png

[200]: ./media/cisco-webex-tutorial/tutorial_general_200.png
[201]: ./media/cisco-webex-tutorial/tutorial_general_201.png
[202]: ./media/cisco-webex-tutorial/tutorial_general_202.png
[203]: ./media/cisco-webex-tutorial/tutorial_general_203.png

