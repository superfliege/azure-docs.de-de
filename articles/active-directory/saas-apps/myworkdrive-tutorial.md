---
title: 'Tutorial: Azure Active Directory-Integration von MyWorkDrive | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und MyWorkDrive konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4d049778-3c7b-46c0-92a4-f2633a32334b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: jeedes
ms.openlocfilehash: f0e2c499619df938bd6f4b05757ba607a9edf244
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55183355"
---
# <a name="tutorial-azure-active-directory-integration-with-myworkdrive"></a>Tutorial: Azure Active Directory-Integration von MyWorkDrive

In diesem Tutorial erfahren Sie, wie Sie MyWorkDrive in Azure Active Directory (Azure AD) integrieren.

Die Integration von MyWorkDrive in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf MyWorkDrive hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei MyWorkDrive anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit MyWorkDrive konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein MyWorkDrive-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von MyWorkDrive aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-myworkdrive-from-the-gallery"></a>Hinzufügen von MyWorkDrive aus dem Katalog

Zum Konfigurieren der Integration von MyWorkDrive in Azure AD müssen Sie MyWorkDrive aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um MyWorkDrive über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **MyWorkDrive** ein, wählen Sie im Ergebnisbereich **MyWorkDrive** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![MyWorkDrive in der Ergebnisliste](./media/myworkdrive-tutorial/tutorial_myworkdrive_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei MyWorkDrive mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in MyWorkDrive als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in MyWorkDrive muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei MyWorkDrive müssen die folgenden Schritte ausgeführt werden:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. **[Erstellen eines MyWorkDrive-Testbenutzers](#creating-a-myworkdrive-test-user)**, um eine Entsprechung von Britta Simon in MyWorkDrive zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer MyWorkDrive-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit MyWorkDrive zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **MyWorkDrive** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![Configure single sign-on](common/tutorial_general_301.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Configure single sign-on](common/editconfigure.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für MyWorkDrive](./media/myworkdrive-tutorial/tutorial_myworkdrive_url.png)

    Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<SERVER.DOMAIN.COM>/SAML/AssertionConsumerService.aspx`

5. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für MyWorkDrive](./media/myworkdrive-tutorial/tutorial_myworkdrive_url1.png)

     Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<SERVER.DOMAIN.COM>/Account/Login-saml`. 

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit der tatsächlichen Antwort-URL und Anmelde-URL.  Geben Sie den MyWorkDrive-Serverhostnamen Ihres Unternehmens ein, z.B.:
    > 
    > Antwort-URL: `https://yourserver.yourdomain.com/SAML/AssertionConsumerService.aspx`
    > 
    > Anmelde-URL: `https://yourserver.yourdomain.com/Account/Login-saml`
    > 
    > Wenden Sie sich an das Supportteam für den MyWorkDrive-Client, wenn Sie unsicher sind, wie Sie Ihren eigenen Hostnamen und das SSL-Zertifikat für diese Werte einrichten.

6. Klicken Sie auf der Seite **SAML-Signaturzertifikat** im Abschnitt **SAML-Signaturzertifikat** auf das **Kopiersymbol**, um die **App-Verbundmetadaten-URL** zu kopieren. Speichern Sie diese auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/myworkdrive-tutorial/tutorial_myworkdrive_certificate.png)

7. Melden Sie sich in einem anderen Webbrowserfenster als Sicherheitsadministrator bei MyWorkDrive an.

8. Klicken Sie auf dem MyWorkDrive-Server im Administratorpanel auf **ENTERPRISE**, und führen Sie die folgenden Schritte aus:

    ![Administrator](./media/myworkdrive-tutorial/tutorial_myworkdrive_admin.png)

    a. Aktivieren Sie **SAML/ADFS SSO**.

    b. Wählen Sie **SAML – Azure AD** aus.

    c. Fügen Sie im Textfeld **Azure App Federation Metadata Url** (Azure-App-Verbundmetadaten-URL) den Wert der **App-Verbundmetadaten-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Klicken Sie unten auf der Seite auf **Speichern**.

    >[!NOTE]
    >Weitere Informationen finden Sie im Artikel zur [Azure AD-Unterstützung für MyWorkDrive](https://www.myworkdrive.com/support/saml-single-sign-on-azure-ad/).

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Azure AD-Benutzer erstellen][100]

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Erstellen eines Azure AD-Testbenutzers](common/create_aaduser_01.png) 

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Erstellen eines Azure AD-Testbenutzers](common/create_aaduser_02.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** den Namen **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

### <a name="creating-a-myworkdrive-test-user"></a>Erstellen eines MyWorkDrive-Testbenutzers

In diesem Abschnitt erstellen Sie in MyWorkDrive einen Benutzer namens Britta Simon. Lassen Sie sich beim Hinzufügen der Benutzer auf der MyWorkDrive-Plattform ggf. vom  [MyWorkDrive-Supportteam](mailto:support@myworkdrive.com) unterstützen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf MyWorkDrive gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **MyWorkDrive** aus.

    ![Configure single sign-on](./media/myworkdrive-tutorial/tutorial_myworkdrive_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „MyWorkDrive“ klicken, sollten Sie automatisch bei Ihrer MyWorkDrive-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
