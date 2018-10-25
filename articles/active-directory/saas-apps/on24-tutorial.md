---
title: 'Tutorial: Azure Active Directory-Integration mit ON24 Virtual Environment SAML Connection | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und ON24 Virtual Environment SAML Connection konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d4028fb5-b2ad-4c5d-b123-7b675c509d64
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.openlocfilehash: 1ec18f0013a7fa640395a8b8bedd9df8b0924c3a
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49071063"
---
# <a name="tutorial-azure-active-directory-integration-with-on24-virtual-environment-saml-connection"></a>Tutorial: Azure Active Directory-Integration mit ON24 Virtual Environment SAML Connection

In diesem Tutorial erfahren Sie, wie Sie ON24 Virtual Environment SAML Connection in Azure Active Directory (Azure AD) integrieren.

Die Integration von ON24 Virtual Environment SAML Connection in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf ON24 Virtual Environment SAML Connection Zugriff hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei ON24 Virtual Environment SAML Connection anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in ON24 Virtual Environment SAML Connection konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein ON24 Virtual Environment SAML Connection-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von ON24 Virtual Environment SAML Connection aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-on24-virtual-environment-saml-connection-from-the-gallery"></a>Hinzufügen von ON24 Virtual Environment SAML Connection aus dem Katalog
Zum Konfigurieren der Integration von ON24 Virtual Environment SAML Connection in Azure AD müssen Sie ON24 Virtual Environment SAML Connection aus dem Katalog Ihrer Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um ON24 Virtual Environment SAML Connection aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![image](./media/on24-tutorial/selectazuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![image](./media/on24-tutorial/a_select_app.png)
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![image](./media/on24-tutorial/a_new_app.png)

4. Geben Sie im Suchfeld **ON24 Virtual Environment SAML Connection** ein, wählen Sie im Ergebnisbereich **ON24 Virtual Environment SAML Connection** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![image](./media/on24-tutorial/tutorial_on24_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei ON24 Virtual Environment SAML Connection mithilfe einer Testbenutzerin namens „Britta Simon“.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in ON24 Virtual Environment SAML Connection als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in ON24 Virtual Environment SAML Connection muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei ON24 Virtual Environment SAML Connection müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines ON24 Virtual Environment SAML Connection-Testbenutzers](#create-an-on24-virtual-environment-saml-connection-test-user)**, um eine Entsprechung von Britta Simon in ON24 Virtual Environment SAML Connection zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer ON24 Virtual Environment SAML Connection-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei ON24 Virtual Environment SAML Connection die folgenden Schritte aus:**

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **ON24 Virtual Environment SAML Connection** die Option **Einmaliges Anmelden** aus.

    ![image](./media/on24-tutorial/B1_B2_Select_SSO.png)

2. Klicken Sie im oberen Bereich des Bildschirms auf **Modus für einmaliges Anmelden ändern**, um den Modus **SAML** auszuwählen.

      ![image](./media/on24-tutorial/b1_b2_saml_ssso.png)

3. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML** aus, um einmaliges Anmelden zu aktivieren.

    ![image](./media/on24-tutorial/b1_b2_saml_sso.png)

4. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![image](./media/on24-tutorial/b1-domains_and_urlsedit.png)

5. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![image](./media/on24-tutorial/tutorial_on24_url.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL ein:

     **URL der Produktionsumgebung**
    
    `SAML-VSHOW.on24.com`

    `SAML-Gateway.on24.com` 

    `SAP PROD SAML-EliteAudience.on24.com` 
                
     **URL der Qualitätssicherungsumgebung**
    
    `SAMLQA-VSHOW.on24.com` 

    `SAMLQA-Gateway.on24.com` 

    `SAMLQA-EliteAudience.on24.com`
 
    b. Geben Sie im Textfeld **Antwort-URL** eine URL ein:
    
     **URL der Produktionsumgebung**
    
    `https://federation.on24.com/sp/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1WU2hvdy5vbjI0LmNvbSJ9/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1HYXRld2F5Lm9uMjQuY29tIn0/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1FbGl0ZUF1ZGllbmNlLm9uMjQuY29tIn0/ACS.saml2`

     **URL der Qualitätssicherungsumgebung**
    
    `https://qafederation.on24.com/sp/ACS.saml2`

    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLVZzaG93Lm9uMjQuY29tIn0/ACS.saml2`

    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLUdhdGV3YXkub24yNC5jb20ifQ/ACS.saml2`
     
    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLUVsaXRlQXVkaWVuY2Uub24yNC5jb20ifQ/ACS.saml2` 

    c. Klicken Sie auf **Zusätzliche URLs festlegen**. 

    d. Geben Sie im Textfeld **Relayzustand** eine URL ein: `https://vshow.on24.com/vshow/ms_azure_saml_test?r=<ID>`

    e. Wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten, geben Sie im Textfeld **Anmelde-URL** eine URL ein: `https://vshow.on24.com/vshow/<INSTANCENAME>`

6. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![image](./media/on24-tutorial/tutorial_on24_certificate.png) 

7. Zum Konfigurieren des einmaligen Anmeldens in **ON24 Virtual Environment SAML Connection** müssen Sie das Zertifikat und die Metadaten, die Sie im Azure-Portal heruntergeladen haben, an das [ON24 Virtual Environment SAML Connection-Supportteam](https://www.on24.com/about-us/support/) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory** aus, wählen Sie dann **Benutzer** und anschließend **Alle Benutzer** aus.

    ![image](./media/on24-tutorial/d_users_and_groups.png)

2. Wählen Sie im oberen Bereich des Bildschirms die Option **Neuer Benutzer** aus.

    ![image](./media/on24-tutorial/d_adduser.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![image](./media/on24-tutorial/d_userproperties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** die Zeichenfolge **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-an-on24-virtual-environment-saml-connection-test-user"></a>Erstellen eines ON24 Virtual Environment SAML Connection-Testbenutzers

In diesem Abschnitt erstellen Sie in ON24 Virtual Environment SAML Connection eine Benutzerin mit dem Namen Britta Simon. Wenden Sie sich an das [ON24 Virtual Environment SAML Connection-Supportteam](https://www.on24.com/about-us/support/), um Benutzer in der ON24 Virtual Environment SAML Connection-Plattform hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf ON24 Virtual Environment SAML Connection gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![image](./media/on24-tutorial/d_all_applications.png)

2. Wählen Sie in der Liste der Anwendungen den Eintrag **ON24 Virtual Environment SAML Connection** aus.

    ![image](./media/on24-tutorial/tutorial_on24_app.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![image](./media/on24-tutorial/d_leftpaneusers.png)

4. Wählen Sie die Schaltfläche **Hinzufügen** und dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![image](./media/on24-tutorial/d_assign_user.png)

4. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

5. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „ON24 Virtual Environment SAML Connection“ klicken, sollten Sie automatisch bei Ihrer ON24 Virtual Environment SAML Connection-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

