---
title: 'Tutorial: Azure Active Directory-Integration mit SAP Cloud Platform Identity Authentication | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SAP Cloud Platform Identity Authentication konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: jeedes
ms.openlocfilehash: dc62d7a24a53ffa04026c0f1a40b05b18e6db71d
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36231388"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform-identity-authentication"></a>Tutorial: Azure Active Directory-Integration mit SAP Cloud Platform Identity Authentication

In diesem Tutorial erfahren Sie, wie Sie SAP Cloud Platform Identity Authentication in Azure Active Directory (Azure AD) integrieren. SAP Cloud Platform Identity Authentication wird als Proxy-IdP für den Zugriff auf SAP-Anwendungen verwendet, wobei Azure AD als Haupt-IdP fungiert.

Wenn Sie SAP Cloud Platform Identity Authentication in Azure AD integrieren, bringt Ihnen das folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf die SAP-Anwendungen haben soll.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei SAP-Anwendungen anzumelden.
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie im Artikel [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration mit SAP Cloud Platform Identity Authentication benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges Abonnement für SAP Cloud Platform Identity Authentication

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige kostenlose Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von SAP Cloud Platform Identity Authentication über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

Bevor Sie sich mit den technischen Details beschäftigen, müssen Sie die Konzepte verstehen, die hier behandelt werden. Durch den Verbund von SAP Cloud Platform Identity Authentication und Active Directory-Verbunddienste (AD FS) können Sie einmaliges Anmelden (SSO) für Anwendungen oder Dienste, die durch Azure AD (als IdP) geschützt sind, mit SAP-Anwendungen und -Diensten implementieren, die durch SAP Cloud Platform Identity Authentication geschützt sind.

SAP Cloud Platform Identity Authentication fungiert derzeit als Proxyidentitätsanbieter für SAP-Anwendungen. Azure Active Directory dient in diesem Setup als führender Identitätsanbieter. 

Das folgende Diagramm veranschaulicht diese Beziehung:

![Erstellen eines Azure AD-Testbenutzers](./media/sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

Mit diesem Setup wird Ihr SAP Cloud Platform Identity Authentication-Mandant als vertrauenswürdige Anwendung in Azure Active Directory konfiguriert. 

Alle SAP-Anwendungen und -Dienste, die Sie auf diese Weise schützen möchten, werden anschließend in der SAP Cloud Platform Identity Authentication-Verwaltungskonsole konfiguriert.

Daher muss die Autorisierung zum Gewähren von Zugriff auf SAP-Anwendungen und -Dienste in SAP Cloud Platform Identity Authentication erfolgen (anstatt in Azure Active Directory).

Durch Konfigurieren von SAP Cloud Platform Identity Authentication als eine Anwendung über den Azure Active Directory Marketplace entfällt die Notwendigkeit, dass Sie einzelne Ansprüche oder SAML-Assertionen konfigurieren.

>[!NOTE] 
>Bisher wurde nur Web-SSO von beiden Parteien getestet. Die Datenflüsse, die für die Kommunikation zwischen Apps und APIs oder zwischen APIs benötigt werden, sollten funktionieren, wurden aber noch nicht getestet. Sie werden im Rahmen späterer Aktivitäten getestet.
>

## <a name="add-sap-cloud-platform-identity-authentication-from-the-gallery"></a>Hinzufügen von SAP Cloud Platform Identity Authentication aus dem Katalog
Um die Integration von SAP Cloud Platform Identity Authentication in Azure AD zu konfigurieren, müssen Sie SAP Cloud Platform Identity Authentication über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um SAP Cloud Platform Identity Authentication über den Katalog hinzuzufügen:**

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich das Symbol **Azure Active Directory** aus. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen ausführen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Wählen Sie im oberen Bereich des Dialogfelds die Schaltfläche **Neue Anwendung** aus, um die neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie in das Suchfeld den Begriff **SAP Cloud Platform Identity Authentication** ein. 

5. Wählen Sie **SAP Cloud Platform Identity Authentication** im Ergebnisbereich aus, und wählen Sie dann die Schaltfläche **Hinzufügen** aus.

    ![SAP Cloud Platform Identity Authentication in der Ergebnisliste](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit SAP Cloud Platform Identity Authentication. Dazu verwenden Sie eine Testbenutzerin namens „Britta Simon“.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in SAP Cloud Platform Identity Authentication der entsprechende Benutzer ist. Das heißt, Sie müssen eine Verknüpfung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SAP Cloud Platform Identity Authentication einrichten.

Geben Sie in SAP Cloud Platform Identity Authentication für **Username** derselben Wert an wie für **Benutzername** in Azure AD. Damit haben Sie die Verknüpfung zwischen den beiden Benutzern eingerichtet.

Um das einmalige Anmelden von Azure AD mit SAP Cloud Platform Identity Authentication zu konfigurieren und zu testen, müssen Sie die folgenden Bausteine ausführen:

1. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on), um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. [Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user), um das einmalige Anmelden mit Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. [Erstellen eines SAP Cloud Platform Identity Authentication-Testbenutzers](#create-an-sap-cloud-platform-identity-authentication-test-user), um in SAP Cloud Platform Identity Authentication eine Entsprechung von Britta Simon zu erhalten, die mit ihrer Azure AD-Darstellung verknüpft ist.
4. [Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user), um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. [Testen des einmaligen Anmeldens](#test-single-sign-on), um sicherzustellen, dass die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren es in Ihrer SAP Cloud Platform Identity Authentication-Anwendung.

**Um das einmalige Anmelden von Azure AD mit SAP Cloud Platform Identity Authentication zu konfigurieren, führen Sie die folgenden Schritte aus:**

1. Wählen Sie im Azure-Portal auf der Anwendungsintegrationsseite für **SAP Cloud Platform Identity Authentication** die Option **Einmaliges Anmelden** aus.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** unter **SAML-basierte Anmeldung** die Option **Modus** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_samlbase.png)

3. Wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten, führen Sie im Abschnitt **Domäne und URLs für SAP Cloud Platform Identity Authentication** die folgenden Schritte aus:  

    ![SSO-Informationen zur Domäne und zu den URLs für SAP Cloud Platform Identity Authentication](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_url.png)

    a. Geben Sie im Feld **Bezeichner** eine URL im folgenden Format ein: `<IAS-tenant-id>.accounts.ondemand.com`

    b. Geben Sie im Feld **Antwort-URL** eine URL im folgenden Format ein: `https://<IAS-tenant-id>.accounts.ondemand.com/saml2/idp/acs/<IAS-tenant-id>.accounts.ondemand.com`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Diese Werte erhalten Sie vom [Supportteam für den SAP Cloud Platform Identity Authentication-Client](https://cloudplatform.sap.com/capabilities/security/trustcenter.html). Falls Sie diesen Bezeichnerwert nicht verstehen, können Sie die SAP Cloud Platform Identity Authentication-Dokumentation zum Thema [Tenant SAML 2.0 Configuration](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html) lesen.

4. Wenn Sie die Anwendung im **SP**-initiierten Modus konfigurieren möchten, wählen Sie **Erweiterte URL-Einstellungen anzeigen** aus.

    ![SSO-Informationen zur Domäne und zu den URLs für SAP Cloud Platform Identity Authentication](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_url1.png)

    Geben Sie in das Feld **Anmelde-URL** eine URL im folgenden Format ein: `{YOUR BUSINESS APPLICATION URL}`.

    > [!NOTE]
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Verwenden Sie Ihre jeweilige Anmelde-URL für die Geschäftsanwendung. Wenden Sie sich bei etwaigen Fragen an das [Supportteam für den SAP Cloud Platform Identity Authentication-Client](https://cloudplatform.sap.com/capabilities/security/trustcenter.html).

5. Wählen Sie im Abschnitt **SAML-Signaturzertifikat** die Option **Metadaten-XML** aus. Speichern Sie dann die Metadatendatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_certificate.png)

6. Die SAP Cloud Platform Identity Authentication-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Verwalten Sie die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite. Der folgende Screenshot zeigt ein Beispiel für das Format. 

    ![Einmaliges Anmelden konfigurieren](./media/sap-hana-cloud-platform-identity-authentication-tutorial/attribute.png)

7. Wenn Ihre SAP-Anwendung ein Attribut wie **firstName** erwartet, fügen Sie das **firstName**-Attribut im Abschnitt **Benutzerattribute** hinzu. Diese Option ist im Dialogfeld **Einmaliges Anmelden** in der Option **SAML-Tokenattribute** verfügbar.

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Attribut hinzufügen** zu öffnen. 
    
    ![Einmaliges Anmelden konfigurieren](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_attribute_04.png)
    
    ![Einmaliges Anmelden konfigurieren](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_attribute_05.png)
    
    b. Geben Sie in das Feld **Name** den Attributnamen **firstName** ein.
    
    c. Wählen Sie in der Liste **Wert** den Attributwert **user.givenname** aus.
    
    d. Klicken Sie auf **OK**.

8. Wählen Sie die Schaltfläche **Speichern** aus.

    ![Einmaliges Anmelden konfigurieren: Schaltfläche „Speichern“](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_400.png)

9. Wählen Sie im Abschnitt **Konfiguration für SAP Cloud Platform Identity Authentication** die Option **SAP Cloud Platform Identity Authentication konfigurieren** aus, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![SAP Cloud Platform Identity Authentication-Konfiguration](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_configure.png) 

10. Wechseln Sie zum Konfigurieren von SSO für Ihre Anwendung zur SAP Cloud Platform Identity Authentication-Verwaltungskonsole. Die URL hat das folgende Muster: `https://<tenant-id>.accounts.ondemand.com/admin`. Lesen Sie dann in der Dokumentation von SAP Cloud Platform Identity Authentication den Artikel [Integration with Microsoft Azure AD](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html). 

11. Wählen Sie im Azure-Portal die Schaltfläche **Speichern** aus.

12. Führen Sie die folgenden Schritte nur aus, wenn Sie SSO für eine weitere SAP-Anwendung hinzufügen und aktivieren möchten. Wiederholen Sie die Schritte aus dem Abschnitt **Hinzufügen von SAP Cloud Platform Identity Authentication über den Katalog**.

13. Wählen Sie im Azure-Portal auf der Anwendungsintegrationsseite für **SAP Cloud Platform Identity Authentication** die Option **Anmeldung über Link** aus.

    ![Konfigurieren der Anmeldung über Link](./media/sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)

14. Speichern Sie die Konfiguration.

>[!NOTE] 
>Die neue Anwendung nutzt die „Einmaliges Anmelden“-Konfiguration der vorherigen SAP-Anwendung. Achten Sie darauf, dass Sie in der SAP Cloud Platform Identity Authentication-Verwaltungskonsole dieselben Unternehmensidentitätsanbieter verwenden.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory** > **Unternehmensanwendungen** hinzugefügt haben, wählen Sie die Registerkarte **Einmaliges Anmelden** aus, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** am unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Wählen Sie im linken Bereich des Azure-Portals die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und wählen Sie dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_02.png)

3. Klicken Sie im oberen Bereich des Dialogfelds **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-an-sap-cloud-platform-identity-authentication-test-user"></a>Erstellen eines SAP Cloud Platform Identity Authentication-Testbenutzers

Sie müssen keinen Benutzer in SAP Cloud Platform Identity Authentication erstellen. Benutzer im Azure AD-Benutzerspeicher können die Funktionalität für einmaliges Anmelden (SSO) verwenden.

SAP Cloud Platform Identity Authentication unterstützt die Identitätsverbundoption. Durch diese Option kann die Anwendung überprüfen, ob im Benutzerspeicher von SAP Cloud Platform Identity Authentication Benutzer vorhanden sind, die vom Unternehmensidentitätsanbieter authentifiziert werden. 

Die Option für Identitätsverbund ist standardmäßig deaktiviert. Ist Identitätsverbund aktiviert, können auf die Anwendung nur Benutzer zugreifen, die in SAP Cloud Platform Identity Authentication importiert wurden. 

Weitere Informationen zum Aktivieren oder Deaktivieren des Identitätsverbunds in SAP Cloud Platform Identity Authentication finden Sie in [Configure Identity Federation with the User Store of SAP Cloud Platform Identity Authentication](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html) (Konfigurieren des Identitätsverbunds mit dem Benutzerspeicher von SAP Cloud Platform Identity Authentication) unter „Enable Identity Federation with SAP Cloud Platform Identity Authentication“ (Aktivieren des Identitätsverbunds in SAP Cloud Platform Identity Authentication).

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf SAP Cloud Platform Identity Authentication gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu SAP Cloud Platform Identity Authentication auszuführen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, und navigieren Sie zur Verzeichnisansicht. Navigieren Sie nun zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **SAP Cloud Platform Identity Authentication** aus.

    ![Der Link „SAP Cloud Platform Identity Authentication“ in der Anwendungsliste](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_app.png)  

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“][202]

4. Wählen Sie die Schaltfläche **Hinzufügen** aus. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel für SAP Cloud Platform Identity Authentication auswählen, werden Sie automatisch bei Ihrer SAP Cloud Platform Identity Authentication-Anwendung angemeldet.

Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/sapcloudauth-tutorial/tutorial_general_01.png
[2]: ./media/sapcloudauth-tutorial/tutorial_general_02.png
[3]: ./media/sapcloudauth-tutorial/tutorial_general_03.png
[4]: ./media/sapcloudauth-tutorial/tutorial_general_04.png

[100]: ./media/sapcloudauth-tutorial/tutorial_general_100.png

[200]: ./media/sapcloudauth-tutorial/tutorial_general_200.png
[201]: ./media/sapcloudauth-tutorial/tutorial_general_201.png
[202]: ./media/sapcloudauth-tutorial/tutorial_general_202.png
[203]: ./media/sapcloudauth-tutorial/tutorial_general_203.png
