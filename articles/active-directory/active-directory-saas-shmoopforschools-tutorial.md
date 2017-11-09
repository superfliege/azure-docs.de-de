---
title: 'Tutorial: Azure Active Directory-Integration mit Shmoop For Schools | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Shmoop For Schools konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1d75560a-55b3-42e9-bda1-92b01c572d8e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: jeedes
ms.openlocfilehash: f30e0967384fb881bab045f6544989b8baaccfa0
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="tutorial-azure-active-directory-integration-with-shmoop-for-schools"></a>Tutorial: Azure Active Directory-Integration mit Shmoop For Schools

In diesem Tutorial erfahren Sie, wie Sie Shmoop For Schools in Azure Active Directory (Azure AD) integrieren.

Die Integration von Shmoop For Schools in Azure AD hat folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Shmoop For Schools haben soll.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Shmoop For Schools anzumelden.
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration mit Shmoop For Schools benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges Shmoop For Schools-Abonnement

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie wie folgt vorgehen:

- Verwenden Sie Ihre Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Fordern Sie eine [kostenlose einmonatige Testversion an](https://azure.microsoft.com/pricing/free-trial/), wenn Sie noch keine Azure AD-Testumgebung haben.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Shmoop For Schools über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="add-shmoop-for-schools-from-the-gallery"></a>Hinzufügen von Shmoop For Schools über den Katalog
Zum Konfigurieren der Integration von Shmoop For Schools in Azure AD müssen Sie Shmoop For Schools aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Shmoop For Schools über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) auf das Symbol **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen ausführen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Wählen Sie oben im Dialogfeld die Schaltfläche **Neue Anwendung** aus, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie in das Suchfeld **Shmoop For Schools** ein. Wählen Sie in den Ergebnissen **Shmoop For Schools** aus, und wählen Sie dann die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Shmoop For Schools in der Ergebnisliste](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Shmoop For Schools mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Shmoop For Schools als Pendant für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Sie müssen zwischen einem Azure AD-Benutzer und dem dazugehörigen Benutzer in Shmoop For Schools eine Verknüpfung einrichten.

Verwenden Sie in Shmoop For Schools für **Username** den gleichen Wert wie für **Benutzername** in Azure AD. Sie haben jetzt die Linkbeziehung hergestellt.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD bei Shmoop For Schools zu konfigurieren und zu testen:

1. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on), um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. [Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user), um das einmalige Anmelden mit Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. [Erstellen eines Shmoop For Schools-Testbenutzers](#create-a-shmoop-for-schools-test-user), um in Shmoop For Schools eine Entsprechung von Britta Simon zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. [Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user), um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. [Testen des einmaligen Anmeldens](#test-single-sign-on), um sicherzustellen, dass die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren es in Ihrer Shmoop For Schools-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Shmoop For Schools zu konfigurieren:**

1. Wählen Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Shmoop For Schools** die Option **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** im Dropdownmenü unter **SSO-Modus** die Option **SAML-basierte Anmeldung** aus.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Shmoop For Schools** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_url.png)

    a. Geben Sie im Feld **URL für Anmeldung** eine URL im folgenden Format ein: `https://schools.shmoop.com/public-api/saml2/start/<uniqueid>`.

    b. Geben Sie im Feld **Bezeichner** eine URL im folgenden Format ein: `https://schools.shmoop.com/<uniqueid>`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Die Werte erhalten Sie vom [Supportteam für den Shmoop For Schools-Client](mailto:support@shmoop.com). 
 
4. Die Shmoop For Schools-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt, wie die Assertionen konfiguriert werden:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_attribute.png)

    > [!NOTE]
    > Shmoop For Schools unterstützt zwei Rollen für Benutzer: **Lehrer** und **Schüler**. Richten Sie diese Rollen in Azure AD ein, damit Benutzern die passenden Rollen zugewiesen werden können. Um zu erfahren, wie Rollen in Azure AD konfiguriert werden, lesen Sie [Roles-based access control in cloud applications using Azure AD](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/) (Rollenbasierte Zugriffssteuerung in Cloudanwendungen mithilfe von Azure AD).
    
5. Konfigurieren Sie das SAML-Tokenattribut im Bereich **Benutzerattribute** im Dialogfeld **Einmaliges Anmelden**, wie in der obigen Abbildung dargestellt.  Führen Sie dann die folgenden Schritte aus:

    | Attributname | Attributwert |
    | -------------- | --------------- |
    | role           | user.assignedroles |

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Attribut hinzufügen** zu öffnen.
    
    ![Einmaliges Anmelden konfigurieren ](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_attribute_04.png)
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_attribute_05.png)
    
    b. Geben Sie im Feld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Wählen Sie in der Liste **Wert** den für diese Zeile angezeigten Attributwert aus.

    d. Lassen Sie das Feld **Namespace** leer.
    
    e. Klicken Sie auf **OK**.

6. Wählen Sie die Schaltfläche **Speichern** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_400.png)

7. Führen Sie zum Generieren der **Metadaten-URL** die folgenden Schritte aus:

    a. Wählen Sie **App-Registrierungen** aus.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_appregistrations.png)
   
    b. Wählen Sie **Endpunkte** aus, um das Dialogfeld **Endpunkte** zu öffnen.  
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_endpointicon.png)

    c. Wählen Sie die Schaltfläche „Kopieren“ aus, um die **VERBUNDMETADATENDOKUMENT**-URL zu kopieren und in Editor einzufügen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_endpoint.png)
     
    d. Wechseln Sie zur Eigenschaftenseite von **Shmoop For Schools**. Kopieren Sie dann die **Anwendungs-ID** mithilfe der Schaltfläche **Kopieren**. Fügen Sie sie in Editor ein.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_appid.png)

    e. Generieren Sie die **Metadaten-URL** mithilfe des folgenden Musters: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`.   

8. Zum Konfigurieren des einmaligen Anmeldens bei **Shmoop For Schools** müssen Sie die **Metadaten-URL** an das [Shmoop For Schools-Supportteam](mailto:support@shmoop.com) senden.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen. Nachdem Sie diese App über den Abschnitt **Active Directory** > **Unternehmensanwendungen** hinzugefügt haben, navigieren Sie zur Registerkarte **Einmaliges Anmelden**, und rufen Sie am unteren Rand im Abschnitt **Konfiguration** die eingebettete Dokumentation auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt wird über das Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Wählen Sie im linken Bereich des Azure-Portals die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-shmoopforschools-tutorial/create_aaduser_01.png)

2. Um die Liste der Benutzer anzuzeigen, wechseln Sie zu **Benutzer und Gruppen**. Wählen Sie dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-shmoopforschools-tutorial/create_aaduser_02.png)

3. Klicken Sie im oberen Bereich des Dialogfelds **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-shmoopforschools-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-shmoopforschools-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-shmoop-for-schools-test-user"></a>Erstellen eines Shmoop For Schools-Testbenutzers

In diesem Abschnitt wird in Shmoop For Schools ein Benutzer namens Britta Simon erstellt. Shmoop For Schools unterstützt die (standardmäßig aktivierte) Just-In-Time-Bereitstellung. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein neuer Benutzer vorhanden ist, wird er beim Zugreifen auf Shmoop For Schools erstellt.

>[!NOTE]
>Setzen Sie sich mit dem [Shmoop For Schools-Supportteam](mailto:support@shmoop.com) in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Shmoop For Schools gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu Shmoop For Schools durchzuführen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht. Wechseln Sie dann in der Verzeichnisansicht zu **Unternehmensanwendungen**.  Wählen Sie anschließend **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **Shmoop For Schools** aus.

    ![Der Link „Shmoop For Schools“ in der Anwendungsliste](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_app.png)  

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“][202]

4. Wählen Sie die Schaltfläche **Hinzufügen** aus. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**. 

7. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel **Shmoop For Schools** wählen, sollten Sie automatisch bei Ihrer Shmoop For Schools-Anwendung angemeldet werden.

Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_203.png

