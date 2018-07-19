---
title: 'Tutorial: Azure Active Directory-Integration mit AirWatch | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und AirWatch konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: f3bbcbb70759e7a995797cf89ad75a2a39314927
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048479"
---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Lernprogramm: Azure Active Directory-Integration mit AirWatch

In diesem Tutorial erfahren Sie, wie Sie AirWatch in Azure Active Directory (Azure AD) integrieren.

Die Integration von AirWatch in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf AirWatch hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei AirWatch anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit AirWatch konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein AirWatch-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von AirWatch aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-airwatch-from-the-gallery"></a>Hinzufügen von AirWatch aus dem Katalog
Zum Konfigurieren der Integration von AirWatch in Azure AD müssen Sie AirWatch aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um AirWatch aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

4. Geben Sie im Suchfeld als Suchbegriff **AirWatch** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/airwatch-tutorial/tutorial_airwatch_search.png)

5. Wählen Sie im Ergebnisbereich **AirWatch** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/airwatch-tutorial/tutorial_airwatch_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei AirWatch mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in AirWatch als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in AirWatch muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in AirWatch zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei AirWatch müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. **[Erstellen eines AirWatch-Testbenutzers](#creating-a-airwatch-test-user)**, um eine Entsprechung von Britta Simon in AirWatch zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer AirWatch-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in AirWatch die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **AirWatch** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Configure single sign-on](./media/airwatch-tutorial/tutorial_airwatch_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für AirWatch** die folgenden Schritte aus:

    ![Configure single sign-on](./media/airwatch-tutorial/tutorial_airwatch_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`.

    b. Geben Sie im Textfeld **Bezeichner** den Wert `AirWatch` ein.

    > [!NOTE] 
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Supportteam von AirWatch](http://www.air-watch.com/company/contact-us/), um diesen Wert zu erhalten. 
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Configure single sign-on](./media/airwatch-tutorial/tutorial_airwatch_certificate.png) 

5. Klicken Sie im Abschnitt **AirWatch-Konfiguration** auf **AirWatch konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Configure single sign-on](./media/airwatch-tutorial/tutorial_airwatch_configure.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/airwatch-tutorial/tutorial_general_400.png)
<CS>
7. Melden Sie sich in einem anderen Webbrowserfenster bei der AirWatch-Unternehmenswebsite als Administrator an.

8. Klicken Sie im linken Navigationsbereich auf **Konten**, und klicken Sie dann auf **Administratoren**.
   
   ![Administratoren](./media/airwatch-tutorial/ic791920.png "Administratoren")

9. Erweitern Sie das Menü **Einstellungen**, und klicken Sie dann auf **Verzeichnisdienste**.
   
   ![Einstellungen](./media/airwatch-tutorial/ic791921.png "Einstellungen")

10. Klicken Sie auf die Registerkarte **Benutzer**, geben Sie im Textfeld **Basis-DN** Ihren Domänennamen ein, und klicken Sie dann auf **Speichern**.
   
   ![Benutzer](./media/airwatch-tutorial/ic791922.png "Benutzer")

11. Klicken Sie auf die Registerkarte **Server** .
   
   ![Server](./media/airwatch-tutorial/ic791923.png "Server")

12. Führen Sie die folgenden Schritte aus:
    
    ![Hochladen](./media/airwatch-tutorial/ic791924.png "Hochladen")   
    
    a. Wählen Sie unter **Verzeichnistyp** die Option **Keiner** aus.

    b. Aktivieren Sie **Use SAML For Authentication**.

    c. Klicken Sie auf **Upload**, um das heruntergeladene Zertifikat hochzuladen.

13. Führen Sie im Abschnitt **Request** die folgenden Schritte aus:
    
    ![Anforderung](./media/airwatch-tutorial/ic791925.png "Anforderung")  

    a. Wählen Sie als **Anforderungsbindungstyp** die Option **POST** aus.

    b. Kopieren Sie im Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Airwatch** den Wert für **SAML-Dienst-URL für einmaliges Anmelden**, und fügen Sie ihn in das Textfeld **SSO-URL des Identitätsanbieters** ein.

    c. Wählen Sie als **NameID-Format** die Option **E-Mail-Adresse** aus.

    d. Klicken Sie auf **Speichern**.

14. Klicken Sie erneut auf die Registerkarte **User** .
    
    ![Benutzer](./media/airwatch-tutorial/ic791926.png "Benutzer")

15. Führen Sie im Abschnitt **Attribute** die folgenden Schritte aus:
    
    ![Attribut](./media/airwatch-tutorial/ic791927.png "Attribut")

    a. Geben Sie im Textfeld **Objektbezeichner** den Text **http://schemas.microsoft.com/identity/claims/objectidentifier** ein.

    b. Geben Sie im Textfeld **Benutzername** den Text **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** ein.

    c. Geben Sie im Textfeld **Anzeigename** den Text **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname** ein.

    d. Geben Sie im Textfeld **Vorname** den Text **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname** ein.

    e. Geben Sie im Textfeld **Nachname** den Text **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname** ein.

    f. Geben Sie im Textfeld **E-Mail** den Text **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** ein.

    g. Klicken Sie auf **Speichern**.

<CE>

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/airwatch-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/airwatch-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/airwatch-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/airwatch-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.
 
### <a name="creating-a-airwatch-test-user"></a>Erstellen eines AirWatch-Testbenutzers

Damit sich Azure AD-Benutzer bei AirWatch anmelden können, müssen sie in AirWatch bereitgestellt werden.

* Im Fall von AirWatch ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der **AirWatch** -Unternehmenswebsite als Administrator an.
2. Klicken Sie im Navigationsbereich links auf **Konten**, und klicken Sie dann auf **Benutzer**.
   
   ![Benutzer](./media/airwatch-tutorial/ic791929.png "Benutzer")
3. Klicken Sie im Menü **Benutzer** auf **Listenansicht**, und klicken Sie dann auf **Hinzufügen \> Benutzer hinzufügen**.
   
   ![Benutzer hinzufügen](./media/airwatch-tutorial/ic791930.png "Benutzer hinzufügen")
4. Führen Sie im Dialogfeld **Add / Edit User** die folgenden Schritte aus:

   ![Benutzer hinzufügen](./media/airwatch-tutorial/ic791931.png "Benutzer hinzufügen")   
   1. Geben Sie in die Textfelder **Benutzername**, **Kennwort**, **Kennwort bestätigen**, **Vorname**, **Nachname** und **E-Mail-Adresse** die Informationen eines gültigen Azure Active Directory-Kontos ein, das Sie bereitstellen möchten.
   2. Klicken Sie auf **Speichern**.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von AirWatch-Benutzerkonten oder mithilfe der von AirWatch bereitgestellten APIs erstellen.
>  

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf AirWatch gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon AirWatch zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **AirWatch** aus.

    ![Configure single sign-on](./media/airwatch-tutorial/tutorial_airwatch_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie die Einstellungen für einmaliges Anmelden testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/airwatch-tutorial/tutorial_general_01.png
[2]: ./media/airwatch-tutorial/tutorial_general_02.png
[3]: ./media/airwatch-tutorial/tutorial_general_03.png
[4]: ./media/airwatch-tutorial/tutorial_general_04.png

[100]: ./media/airwatch-tutorial/tutorial_general_100.png

[200]: ./media/airwatch-tutorial/tutorial_general_200.png
[201]: ./media/airwatch-tutorial/tutorial_general_201.png
[202]: ./media/airwatch-tutorial/tutorial_general_202.png
[203]: ./media/airwatch-tutorial/tutorial_general_203.png

