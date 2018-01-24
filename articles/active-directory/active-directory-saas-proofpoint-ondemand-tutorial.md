---
title: 'Tutorial: Azure Active Directory-Integration mit Proofpoint on Demand | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Proofpoint on Demand konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 773e7f7d-ec31-411b-860d-6a6633335d43
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/13/2017
ms.author: jeedes
ms.openlocfilehash: 55479406487bf445c5f449b13663c0bfaee751fd
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2017
---
# <a name="tutorial-azure-active-directory-integration-with-proofpoint-on-demand"></a>Tutorial: Azure Active Directory-Integration mit Proofpoint on Demand

In diesem Tutorial erfahren Sie, wie Sie Proofpoint on Demand in Azure Active Directory (Azure AD) integrieren.

Die Integration von Proofpoint on Demand in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Proofpoint on Demand hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Proofpoint on Demand anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Proofpoint on Demand konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Proofpoint on Demand-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Proofpoint on Demand aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-proofpoint-on-demand-from-the-gallery"></a>Hinzufügen von Proofpoint on Demand aus dem Katalog
Um die Integration von Proofpoint on Demand in Azure AD zu konfigurieren, müssen Sie Proofpoint on Demand über den Katalog Ihrer Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Proofpoint on Demand aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Proofpoint on Demand** ein, wählen Sie im Ergebnisbereich **Proofpoint on Demand** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Proofpoint on Demand in der Ergebnisliste](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD mit Proofpoint on Demand.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Proofpoint on Demand als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Proofpoint on Demand muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Proofpoint on Demand den Wert für **Benutzername** aus Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Proofpoint on Demand müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Proofpoint on Demand-Testbenutzers](#create-a-proofpoint-on-demand-test-user)**, um in Proofpoint on Demand eine Entsprechung von Britta Simon zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Proofpoint on Demand-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Proofpoint on Demand die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Proofpoint on Demand** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Proofpoint on Demand** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Proofpoint on Demand](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<hostname>.pphosted.com/ppssamlsp_hostname`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<hostname>.pphosted.com/ppssamlsp`

    c. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<hostname>.pphosted.com:portnumber/v1/samlauth/samlconsumer`
    
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner, die Antwort-URL und die Anmelde-URL. Wenden Sie sich an das [Kundensupportteam von Proofpoint on Demand](https://www.proofpoint.com/us/support-services), um diese Werte zu erhalten.

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_400.png)
    
7. Klicken Sie im Abschnitt **Proofpoint on Demand-Konfiguration** auf **Proofpoint on Demand konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Konfiguration von Proofpoint on Demand](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_configure.png) 

8. Zum Konfigurieren des einmaligen Anmeldens auf der Seite von **Proofpoint on Demand** müssen Sie das heruntergeladene **Zertifikat (Base64)**, die **Abmelde-URL, die SAML-Entitäts-ID und die SAML-Dienst-URL für einmaliges Anmelden** an das [Supportteam von Proofpoint on Demand](https://www.proofpoint.com/us/support-services) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-proofpoint-on-demand-test-user"></a>Erstellen eines Proofpoint on Demand-Testbenutzers

In diesem Abschnitt erstellen Sie in Proofpoint on Demand einen Benutzer namens Britta Simon. Wenden Sie sich an das [Kundensupportteam von Proofpoint on Demand](https://www.proofpoint.com/us/support-services), um Benutzer der Proofpoint on Demand-Plattform hinzufügen zu lassen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie Zugriff auf Proofpoint on Demand gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Proofpoint on Demand zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste der Anwendungen **Proofpoint on Demand** aus.

    ![Der Link „Proofpoint on Demand“ in der Anwendungsliste](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Proofpoint on Demand“ klicken, sollten Sie automatisch bei Ihrer Proofpoint on Demand-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_203.png

