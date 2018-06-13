---
title: 'Tutorial: Azure Active Directory-Integration in SilkRoad Life Suite | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und der SilkRoad Life Suite konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: jeedes
ms.openlocfilehash: 96f57f8dca4826840ccb04db25506fb9e45c59ef
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34352785"
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>Tutorial: Azure Active Directory-Integration in die SilkRoad Life Suite

In diesem Tutorial erfahren Sie, wie Sie SilkRoad Life Suite in Azure Active Directory (Azure AD) integrieren.

Die Integration der SilkRoad Life Suite in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf die SilkRoad Life Suite zugreifen kann.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei SilkRoad Life Suite anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in die SilkRoad Life Suite konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SilkRoad Life Suite-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen der SilkRoad Life Suite aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-silkroad-life-suite-from-the-gallery"></a>Hinzufügen der SilkRoad Life Suite aus dem Katalog
Zum Konfigurieren der Integration der SilkRoad Life Suite in Azure AD müssen Sie die SilkRoad Life Suite aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um die SilkRoad Life Suite aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **SilkRoad Life Suite** ein, wählen Sie im Ergebnisbereich **SilkRoad Life Suite** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![SilkRoad Life Suite in der Ergebnisliste](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei SilkRoad Life Suite mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in der SilkRoad Life Suite einem Benutzer in Azure AD entspricht. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in der SilkRoad Life Suite muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in SilkRoad Life Suite den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei der SilkRoad Life Suite müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines SilkRoad Life Suite-Testbenutzers](#create-a-silkroad-life-suite-test-user)**, um eine Entsprechung von Britta Simon in der SilkRoad Life Suite zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer SilkRoad Life Suite-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei der SilkRoad Life Suite die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **SilkRoad Life Suite** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für SilkRoad Life Suite** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und den URLs für SilkRoad Life Suite](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_url1.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.silkroad-eng.com/Authentication/`.

    b. Geben Sie im Textfeld **Identifier** (Bezeichner) eine URL nach folgendem Muster ein: 
    | |
    |--|
    | `https://<subdomain>.silkroad-eng.com/Authentication/SP` |
    | `https://<subdomain>.silkroad.com/Authentication/SP` |

    c. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: 
    | |
    |--|
    | `https://<subdomain>.silkroad-eng.com/Authentication/` |
    | `https://<subdomain>.silkroad.com/Authentication/` |
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner, die Antwort-URL und die Anmelde-URL. Wenden Sie sich an das [Clientsupportteam von SilkRoad Life Suite](https://www.silkroad.com/locations/), um diese Werte zu erhalten. 

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_400.png)
    
6. Klicken Sie im Abschnitt **SilkRoad Life Suite-Konfiguration** auf **SilkRoad Life Suite konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![SilkRoad Life Suite-Konfiguration](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_configure.png) 

7. Melden Sie sich bei Ihrer SilkRoad Life Suite-Unternehmenswebsite als Administrator an. 
 
    >[!NOTE] 
    > Um Zugriff auf die Authentifizierungsanwendung der Anwendung SilkRoad Life Suite zu erhalten und den Verbund mit Microsoft Azure AD zu konfigurieren, wenden Sie sich an den SilkRoad-Support oder Ihren Vertriebsmitarbeiter für SilkRoad-Dienste.

8. Wechseln Sie zu **Dienstanbieter**, und klicken Sie dann auf **Verbunddetails**. 
   
    ![Azure AD – einmaliges Anmelden][10]

9. Klicken Sie auf **Download Federation Metadata**(Verbundmetadaten herunterladen), und speichern Sie die Metadatendatei dann auf Ihrem Computer.
   
    ![Azure AD – einmaliges Anmelden][11] 

10. Klicken Sie in der Anwendung **SilkRoad** auf **Authentication Sources** (Authentifizierungsquellen).
   
    ![Azure AD – einmaliges Anmelden][12] 

11. Klicken Sie auf **Add Authentication Source**(Authentifizierungsquelle hinzufügen). 
   
    ![Azure AD – einmaliges Anmelden][13] 

12. Führen Sie im Abschnitt **Add Authentication Source** (Authentifizierungsquelle hinzufügen) die folgenden Schritte aus: 
   
    ![Azure AD – einmaliges Anmelden][14]
  
    a. Klicken Sie unter **Option 2 - Metadata File** (Option 2 – Metadatendatei) auf **Browse (Durchsuchen)**, um die aus dem Azure-Portal heruntergeladene Metadatendatei hochzuladen.
  
    b. Klicken Sie auf **Create Identity Provider using File Data**(Identitätsanbieter mithilfe von Dateidaten erstellen).

13. Klicken Sie im Abschnitt **Authentication Sources (Authentifizierungsquellen)** auf **Edit (Bearbeiten)**. 
    
     ![Azure AD – einmaliges Anmelden][15] 

14. Führen Sie im Dialogfeld **Edit Authentication Source** (Authentifizierungsquelle bearbeiten) die folgenden Schritte aus: 
    
     ![Azure AD – einmaliges Anmelden][16] 

    a. Wählen Sie für **Enabled (Aktiviert)** die Option **Yes (Ja)** aus.

    b. Fügen Sie in das Textfeld **Entitäts-ID** den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.
   
    c. Geben Sie im Textfeld **IdP Description** (IdP-Beschreibung) eine Beschreibung für die Konfiguration ein (etwa *Azure AD SSO*).

    d. Laden Sie im Textfeld **Metadata file**  (Metadatendatei), die **Metadatendatei** hoch, die Sie aus dem Azure-Portal heruntergeladen haben.
  
    e. Geben Sie im Textfeld **IdP Name** (IdP-Name) einen für Ihre Konfiguration spezifischen Namen ein (etwa *Azure SP*).
  
    f. Fügen Sie in das Textfeld **Logout Service URL** (URL des Abmeldediensts) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    g. Fügen Sie in das Textfeld **sign-on service URL** (URL des Diensts für einmalige Anmeldung) den Wert der **SAML-Dienst-URL für einmalige Anmeldung** ein, den Sie aus dem Azure-Portal kopiert haben.

    h. Klicken Sie auf **Speichern**.

15. Deaktivieren Sie alle anderen Authentifizierungsquellen. 
    
     ![Azure AD – einmaliges Anmelden][17]

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-silkroad-life-suite-test-user"></a>Erstellen eines SilkRoad Life Suite-Testbenutzers

In diesem Abschnitt erstellen Sie in SilkRoad Life Suite einen Benutzer namens Britta Simon. Arbeiten Sie mit dem [Clientsupportteam von SilkRoad Life Suite](https://www.silkroad.com/locations/) zusammen, um der SilkRoad Life Suite-Plattform Benutzer hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können. 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf SilkRoad Life Suite erteilen.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um Britta Simon der SilkRoad Life Suite zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **SilkRoad Life Suite**aus.

    ![SilkRoad Life Suite-Link in der Anwendungsliste](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel für die SilkRoad Life Suite klicken, sollten Sie automatisch an Ihrer SilkRoad Life Suite-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
