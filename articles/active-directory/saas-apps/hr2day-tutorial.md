---
title: 'Tutorial: Azure Active Directory-Integration mit HR2day by Merces | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und HR2day by Merces konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 853d08c9-27b1-48d4-b8e7-3705140eb67f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: bb03506bac22c8c82e856f403710a4908af6f9de
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55149253"
---
# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a>Tutorial: Azure Active Directory-Integration mit HR2day by Merces

In diesem Tutorial erfahren Sie, wie Sie HR2day by Merces in Azure Active Directory (Azure AD) integrieren.

Die Integration von HR2day by Merces in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf HR2day by Merces Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei HR2day by Merces anzumelden.
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit HR2day by Merces konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges HR2day by Merces-Abonnement

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Registrieren Sie sich für eine [einmonatige kostenlose Testversion von Azure AD](https://azure.microsoft.com/pricing/free-trial/), sofern nicht bereits geschehen.  

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das hier beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von HR2day by Merces aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="add-hr2day-by-merces-from-the-gallery"></a>Hinzufügen von HR2day by Merces aus dem Katalog
Zum Konfigurieren der Integration von HR2day by Merces in Azure AD fügen Sie HR2day by Merces aus dem Katalog der Liste der verwalteten SaaS-Apps hinzu.

**Um HR2day by Merces aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Wählen Sie im linken Navigationsbereich des [Azure-Portals](https://portal.azure.com) das Symbol für **Azure Active Directory** aus. 

    ![Active Directory][1]

1. Navigieren Sie zu **Unternehmensanwendungen ausführen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
1. Wählen Sie oben im Dialogfeld die Schaltfläche **Neue Anwendung** aus, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

1. Geben Sie im Suchfeld als Suchbegriff **HR2day by Merces**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/hr2day-tutorial/tutorial_hr2daybymerces_search.png)

1. Wählen Sie im Ergebnispanel die Option **HR2day by Merces** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/hr2day-tutorial/tutorial_hr2daybymerces_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei HR2day by Merces mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in HR2day by Merces als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Sie müssen zwischen einem Azure AD-Benutzer und dem dazugehörigen Benutzer in HR2day by Merces eine Verknüpfung einrichten.

Weisen Sie in HR2day by Merces **Benutzername** aus Azure AD **Benutzername** zu, um die Beziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD mit HR2day by Merces müssen Sie die folgenden Bausteine ausführen:

1. [Konfigurieren des einmaligen Anmeldens in Azure AD](#configuring-azure-ad-single-sign-on): Ermöglichen Sie Ihren Benutzern, dieses Feature zu verwenden.
1. [Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user): Testen Sie das einmalige Anmelden in Azure AD mit Britta Simon.
1. [Erstellen eines Testbenutzers für HR2day by Merces](#creating-an-hr2day-by-merces-test-user): Erstellen Sie eine Entsprechung für Britta Simon in HR2day by Merces, die mit der Darstellung des Benutzers in Azure AD verknüpft ist.
1. [Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user): Ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens in Azure AD.
1. [Testen des einmaligen Anmeldens](#testing-single-sign-on): Vergewissern Sie sich, dass die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer HR2day by Merces-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in HR2day by Merces folgende Schritte aus:**

1. Wählen Sie im Azure-Portal auf der Anwendungsintegrationsseite für **HR2day by Merces** die Option **Einmaliges Anmelden** aus.

    ![Einmaliges Anmelden konfigurieren][4]

1. Zum Aktivieren des einmaligen Anmeldens wählen Sie im Dialogfeld **Einmaliges Anmelden** für **Modus** die Option **SAML-basierte Anmeldung** aus.
 
    ![Einmaliges Anmelden konfigurieren](./media/hr2day-tutorial/tutorial_hr2daybymerces_samlbase.png)

1. Führen Sie im Bereich **Domäne und URLs für HR2day by Merces** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/hr2day-tutorial/tutorial_hr2daybymerces_url.png)

    a. Geben Sie im Feld **URL für Anmeldung** eine URL im folgenden Format ein: `https://<tenantname>.force.com/<instancename>`.

    b. Geben Sie im Feld **Bezeichner** eine URL im folgenden Format ein: `https://hr2day.force.com/<companyname>`.

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam für den HR2day by Merces-Client](mailto:servicedesk@merces.nl), um diese Werte zu erhalten. 
 


1. Wählen Sie im Bereich **SAML-Signaturzertifikat** die Option **Zertifikat (Base64)** aus, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/hr2day-tutorial/tutorial_hr2daybymerces_certificate.png) 

1. In diesem Abschnitt wird erläutert, wie Sie Benutzern ermöglichen, sich mit ihrem Azure AD-Konto bei HR2day by Merces zu authentifizieren. Hierbei wird ein Verbund verwendet, der auf dem SAML-Protokoll basiert.

    Die HR2day by Merces-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihren SAML-Token benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen. 

    ![Einmaliges Anmelden konfigurieren](./media/hr2day-tutorial/tutorial_hr2day_00.png)
    
    > [!NOTE] 
    Damit Sie die SAML-Assertion konfigurieren können, müssen Sie für Ihren Mandanten zunächst beim [Supportteam für den HR2day by Merces-Client](mailto:servicedesk@merces.nl) den Attributwert für den eindeutigen Bezeichner anfordern. Sie benötigen diesen Wert, um die Schritte im nächsten Abschnitt abzuschließen. 

1. Konfigurieren Sie das SAML-Tokenattribut im Bereich **Benutzerattribute** im Dialogfeld **Einmaliges Anmelden**, wie in der Abbildung dargestellt. Führen Sie dann die folgenden Schritte aus:
    
      | Attributname    |   Attributwert |  
    | ------------------- | -------------------- |    
    | ATTR_LOGINCLAIM | `join([mail],"102938475Z","@"` |
    
      a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Attribut hinzufügen** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/hr2day-tutorial/tutorial_attribute_04.png)

    ![Einmaliges Anmelden konfigurieren](./media/hr2day-tutorial/tutorial_attribute_05.png)

    b. Geben Sie im Feld **Name** die Zeichenfolge **ATTR_LOGINCLAIM** ein.

    c. Wählen Sie in der Liste **Wert** den Wert **Join()** aus.

    d. Wählen Sie in der Liste **Zeichenfolge1** die Option **user.mail** aus.

    e. Geben Sie für **String2** den eindeutigen Bezeichner ein, der Sie von Ihrem HR2day-Team erhalten haben.

    f. Geben Sie im Feld **Trennzeichen** das Zeichen **\@** ein.
    
    g. Klicken Sie auf **OK**.

1. Wählen Sie die Schaltfläche **Speichern** aus.

    ![Einmaliges Anmelden konfigurieren](./media/hr2day-tutorial/tutorial_general_400.png)

1. Wählen Sie im Bereich **Konfiguration für HR2day by Merces** die Option **HR2day by Merces konfigurieren** aus, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL**, die **SAML-Entitäts-ID** und die **URL für den SAML-SSO-Dienst** aus dem Bereich **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/hr2day-tutorial/tutorial_hr2daybymerces_configure.png) 

1. Um SSO für Ihre Anwendung zu konfigurieren, wenden Sie sich an das [Supportteam für den HR2day by Merces-Client](mailTo:servicedesk@merces.nl). Fügen Sie die heruntergeladene Datei mit dem **Zertifikat (Base64)** an Ihre E-Mail an. Geben Sie außerdem die **Abmelde-URL**, die **SAML-Entitäts-ID** und die **SAML-SSO-Dienst-URL** an, damit diese für die SSO-Integration konfiguriert werden können.

    > [!NOTE]
    >Informieren Sie das Merces-Team darüber, dass für diese Integration das Festlegen der Entitäts-ID im folgenden Format erforderlich ist: **https://hr2day.force.com/INSTANCENAME**.

    > [!TIP]
    >Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Bereich **Active Directory** > **Enterprise Applications** hinzugefügt haben, wählen Sie die Registerkarte **Einmaliges Anmelden** aus. Öffnen Sie dann die eingebettete Dokumentation über den Bereich **Konfiguration** am unteren Rand. Weitere Informationen zur eingebetteten Dokumentation finden Sie unter [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Wählen Sie im linken Navigationsbereich des **Azure-Portals** das Symbol für **Azure Active Directory** aus.

    ![Erstellen eines Azure AD-Testbenutzers](./media/hr2day-tutorial/create_aaduser_01.png) 

1. Navigieren Sie zu **Benutzer und Gruppen**, und wählen Sie dann **Alle Benutzer** aus.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/hr2day-tutorial/create_aaduser_02.png) 

1. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/hr2day-tutorial/create_aaduser_03.png) 

1. Führen Sie im Dialogfeld **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/hr2day-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich das Kennwort.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-an-hr2day-by-merces-test-user"></a>Erstellen eines Testbenutzers für HR2day by Merces

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in HR2day by Merces. Lassen Sie sich beim Hinzufügen der Benutzer im HR2day-Konto vom [Supportteam für den HR2day by Merces-Client](mailto:servicedesk@merces.nl) unterstützen. 

> [!NOTE]
> Wenn Sie einen Benutzer manuell erstellen müssen, setzen Sie sich mit dem [Supportteam für den HR2day by Merces-Client](mailto:servicedesk@merces.nl) in Verbindung.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf HR2day by Merces gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon HR2day by Merces zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht. Navigieren Sie zur Verzeichnisansicht und dann zu **Unternehmensanwendungen**. Wählen Sie anschließend **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **HR2day by Merces**aus.

    ![Einmaliges Anmelden konfigurieren](./media/hr2day-tutorial/tutorial_hr2daybymerces_app.png) 

1. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][202] 

1. Wählen Sie die Schaltfläche **Hinzufügen** aus. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste **Benutzer** die Option **Britta Simon** aus.

1. Klicken Sie auf die Schaltfläche **Auswählen**.

1. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Option **Zuweisen** aus.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über das Zugriffspanel.  

Wenn Sie im Zugriffspanel die Kachel „HR2day by Merces“ auswählen, werden Sie automatisch bei Ihrer HR2day by Merces-Anwendung angemeldet.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/hr2day-tutorial/tutorial_general_01.png
[2]: ./media/hr2day-tutorial/tutorial_general_02.png
[3]: ./media/hr2day-tutorial/tutorial_general_03.png
[4]: ./media/hr2day-tutorial/tutorial_general_04.png

[100]: ./media/hr2day-tutorial/tutorial_general_100.png

[200]: ./media/hr2day-tutorial/tutorial_general_200.png
[201]: ./media/hr2day-tutorial/tutorial_general_201.png
[202]: ./media/hr2day-tutorial/tutorial_general_202.png
[203]: ./media/hr2day-tutorial/tutorial_general_203.png

