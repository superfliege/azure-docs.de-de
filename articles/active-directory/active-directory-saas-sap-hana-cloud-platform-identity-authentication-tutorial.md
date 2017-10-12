---
title: 'Tutorial: Azure Active Directory-Integration mit SAP Cloud Platform Identity Authentication | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SAP Cloud Platform Identity Authentication konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: jeedes
ms.openlocfilehash: e4e7c7273acc216ae82a52b3e6dcd530a6ad1bd7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform-identity-authentication"></a>Tutorial: Azure Active Directory-Integration mit SAP Cloud Platform Identity Authentication

In diesem Tutorial erfahren Sie, wie Sie SAP Cloud Platform Identity Authentication in Azure Active Directory (Azure AD) integrieren. SAP Cloud Platform Identity Authentication wird als Proxy-IdP für den Zugriff auf SAP-Anwendungen verwendet, wobei Azure AD als Haupt-IdP fungiert.

Die Integration von SAP Cloud Platform Identity Authentication in Azure AD hat folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf die SAP-Anwendung haben soll.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei SAP-Anwendungen anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration mit SAP Cloud Platform Identity Authentication benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges SAP Cloud Platform Identity Authentication-Abonnement

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von SAP Cloud Platform Identity Authentication über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

Bevor wir uns mit den technischen Details beschäftigen, müssen Sie die Konzepte verstehen, die hier behandelt werden. Durch den Verbund von SAP Cloud Platform Identity Authentication und Azure Active Directory können Sie einmaliges Anmelden (SSO) für Anwendungen oder Dienste, die durch AAD (als IdP) geschützt sind, mit SAP-Anwendungen und -Diensten implementieren, die durch SAP Cloud Platform Identity Authentication geschützt sind.

SAP Cloud Platform Identity Authentication fungiert derzeit als Proxyidentitätsanbieter für SAP-Anwendungen. Azure Active Directory dient in diesem Setup als führender Identitätsanbieter. 

Die folgende Abbildung veranschaulicht dies:

![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sapcloudauth-tutorial/architecture-01.png)

Mit diesem Setup wird Ihr SAP Cloud Platform Identity Authentication-Mandant als vertrauenswürdige Anwendung in Azure Active Directory konfiguriert. 

Alle SAP-Anwendungen und -Dienste, die Sie auf diese Weise schützen möchten, werden anschließend in der SAP Cloud Platform Identity Authentication-Verwaltungskonsole konfiguriert.

Das bedeutet, dass die Autorisierung zum Gewähren von Zugriff auf SAP-Anwendungen und -Dienste für ein solches Setup in SAP Cloud Platform Identity Authentication erfolgen muss (statt die Autorisierung in Azure Active Directory zu konfigurieren).

Wenn Sie SAP Cloud Platform Identity Authentication als Anwendung über den Azure Active Directory Marketplace konfigurieren, müssen Sie sich nicht um die Konfiguration der einzelnen Ansprüche/SAML-Assertionen und -Transformationen kümmern, die zur Erstellung eines gültigen Authentifizierungstokens für SAP-Anwendungen benötigt werden.

>[!NOTE] 
>Web-SSO wurde gegenwärtig nur von beiden Parteien getestet. Datenflüsse, die für die Kommunikation zwischen Apps und APIs oder zwischen APIs benötigt werden, sollten funktionieren, wurden aber noch nicht getestet. Sie werden im Rahmen späterer Aktivitäten getestet.
>

## <a name="adding-sap-cloud-platform-identity-authentication-from-the-gallery"></a>Hinzufügen von SAP Cloud Platform Identity Authentication über den Katalog
Um die Integration von SAP Cloud Platform Identity Authentication in Azure AD zu konfigurieren, müssen Sie SAP Cloud Platform Identity Authentication über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um SAP Cloud Platform Identity Authentication über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld die Zeichenfolge **SAP Cloud Platform Identity Authentication** ein, wählen Sie im Ergebnisbereich die Option **SAP Cloud Platform Identity Authentication** aus, und klicken Sie anschließend auf **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![SAP Cloud Platform Identity Authentication in der Ergebnisliste](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei SAP Cloud Platform Identity Authentication mithilfe eines Testbenutzers namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in SAP Cloud Platform Identity Authentication als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SAP Cloud Platform Identity Authentication muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in SAP Cloud Platform Identity Authentication den Wert für **Benutzername** aus Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei SAP Cloud Platform Identity Authentication müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines SAP Cloud Platform Identity Authentication-Testbenutzers](#create-an-sap-cloud-platform-identity-authentication-test-user)**, um in SAP Cloud Platform Identity Authentication eine Entsprechung von Britta Simon zu erhalten, die mit ihrer Azure AD-Darstellung verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren es in Ihrer SAP Cloud Platform Identity Authentication-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei SAP Cloud Platform Identity Authentication die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **SAP Cloud Platform Identity Authentication** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für SAP Cloud Platform Identity Authentication** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für SAP Cloud Platform Identity Authentication](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_url.png)

    Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<entity-id>.accounts.ondemand.com`

    > [!NOTE] 
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Aktualisieren Sie den Wert mit dem tatsächlichen Bezeichner. Diesen Wert erhalten Sie vom [Supportteam für den SAP Cloud Platform Identity Authentication-Client](https://cloudplatform.sap.com/capabilities/security/trustcenter.html). Falls Sie den Wert nicht kennen, lesen Sie in der SAP Cloud Platform Identity Authentication-Dokumentation unter [Tenant SAML 2.0 Configuration](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html) (SAML 2.0-Mandantenkonfiguration) nach.

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**. Wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für SAP Cloud Platform Identity Authentication](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<entity-id>.accounts.ondemand.com/admin`

    > [!NOTE] 
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Diesen Wert erhalten Sie vom [Supportteam für den SAP Cloud Platform Identity Authentication-Client](https://cloudplatform.sap.com/capabilities/security/trustcenter.html).

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_certificate.png)

6. Die SAP Cloud Platform Identity Authentication-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapcloudauth-tutorial/attribute.png)

7. Im Abschnitt **Benutzerattribute** des Dialogfelds **Einmaliges Anmelden** erwartet die SAP-Anwendung möglicherweise ein Attribut, z.B. „firstName“. Fügen Sie das Attribut „firstName“ im Dialogfeld „SAML-Tokenattribute“ hinzu.

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_attribute_04.png)
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_attribute_05.png)
    
    b. Geben Sie im Textfeld **Name** den Attributnamen „firstName“ ein.
    
    c. Wählen Sie in der Liste **Wert** den Attributwert „user.givenname“ aus.
    
    d. Klicken Sie auf **OK**.

8. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_400.png)

9. Klicken Sie im Abschnitt **Konfiguration für SAP Cloud Platform Identity Authentication** auf **SAP Cloud Platform Identity Authentication konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![SAP Cloud Platform Identity Authentication-Konfiguration](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_configure.png) 

10. Wechseln Sie zum Konfigurieren von SSO für Ihre Anwendung zur SAP Cloud Platform Identity Authentication-Verwaltungskonsole. Die URL hat das folgende Muster: `https://<tenant-id>.accounts.ondemand.com/admin`. Gehen Sie dann gemäß der Dokumentation für SAP Cloud Platform Identity Authentication unter [Configure Microsoft Azure AD as Corporate Identity Provider at SAP Cloud Platform Identity Authentication](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html) (Konfigurieren von Microsoft Azure AD als Identitätsanbieter des Unternehmens für SAP Cloud Platform Identity Authentication) vor. 

11. Klicken Sie im Azure-Portal auf die Schaltfläche **Speichern**.

12. Führen Sie die folgenden Schritte nur aus, wenn Sie SSO für eine andere SAP-Anwendung hinzufügen und aktivieren möchten. Wiederholen Sie die Schritte des Abschnitts „Hinzufügen von SAP Cloud Platform Identity Authentication über den Katalog“, um eine weitere SAP Cloud Platform Identity Authentication-Instanz hinzuzufügen.

13. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **SAP Cloud Platform Identity Authentication** auf **Anmeldung über Link**.

    ![Konfigurieren der Anmeldung über Link](./media/active-directory-saas-sapcloudauth-tutorial/linked_sign_on.png)

14. Speichern Sie die Konfiguration.

>[!NOTE] 
>Die neue Anwendung nutzt die SSO-Konfiguration für die vorherige SAP-Anwendung. Achten Sie darauf, dass Sie in der SAP Cloud Platform Identity Authentication-Verwaltungskonsole die gleichen Unternehmensidentitätsanbieter verwenden.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-an-sap-cloud-platform-identity-authentication-test-user"></a>Erstellen eines SAP Cloud Platform Identity Authentication-Testbenutzers

Sie müssen keinen Benutzer in SAP Cloud Platform Identity Authentication erstellen. Benutzer im Azure AD-Benutzerspeicher können die Funktionalität für einmaliges Anmelden (SSO) verwenden.

SAP Cloud Platform Identity Authentication unterstützt die Identitätsverbundoption. Durch diese Option kann die Anwendung überprüfen, ob die vom Unternehmensidentitätsanbieter authentifizierten Benutzer im Benutzerspeicher von SAP Cloud Platform Identity Authentication vorhanden sind. 

In der Standardeinstellung ist die Option für Identitätsverbund deaktiviert. Bei aktiviertem Identitätsverbund können nur in SAP Cloud Platform Identity Authentication importierte Benutzer auf die Anwendung zugreifen. 

Weitere Informationen zum Aktivieren oder Deaktivieren des Identitätsverbunds in SAP Cloud Platform Identity Authentication finden Sie in [Configure Identity Federation with the User Store of SAP Cloud Platform Identity Authentication](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html) (Konfigurieren des Identitätsverbunds mit dem Benutzerspeicher von SAP Cloud Platform Identity Authentication) unter „Enable Identity Federation with SAP Cloud Platform Identity Authentication“ (Aktivieren des Identitätsverbunds in SAP Cloud Platform Identity Authentication).

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf SAP Cloud Platform Identity Authentication gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu SAP Cloud Platform Identity Authentication durchzuführen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **SAP Cloud Platform Identity Authentication** aus.

    ![Der Link „SAP Cloud Platform Identity Authentication“ in der Anwendungsliste](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel für SAP Cloud Platform Identity Authentication klicken, sollten Sie automatisch bei Ihrer SAP Cloud Platform Identity Authentication-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_203.png

