---
title: 'Tutorial: Azure Active Directory-Integration mit Palo Alto Networks - Admin UI | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Palo Alto Networks - Admin UI konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a826eaec-15af-4c85-8855-8a3374d1efb9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.openlocfilehash: fed368c0df265495d9fee764f86825957fae8bab
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447423"
---
# <a name="integrate-azure-active-directory-with-palo-alto-networks---admin-ui"></a>Integrieren von Azure Active Directory in Palo Alto Networks – Admin UI

In diesem Tutorial erfahren Sie, wie Sie Azure Active Directory (Azure AD) in Palo Alto Networks – Admin UI integrieren.

Die Integration von Azure AD in Palo Alto Networks – Admin UI bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Palo Alto Networks - Admin UI hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Palo Alto Networks – Admin UI anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort, im Azure-Portal, verwalten.

Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Palo Alto Networks - Admin UI konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Eine Firewall der nächsten Generation oder Panorama von Palo Alto Networks (zentralisiertes Verwaltungssystem für die Firewalls)

> [!NOTE]
> Es wird *nicht* empfohlen, zum Testen der Schritte in diesem Tutorial eine Produktionsumgebung zu verwenden.

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. 

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

* Hinzufügen von Palo Alto Networks - Admin UI aus dem Katalog
* Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="add-palo-alto-networks---admin-ui-from-the-gallery"></a>Hinzufügen von Palo Alto Networks – Admin UI aus dem Katalog
Zum Konfigurieren der Integration von Azure AD in Palo Alto Networks – Admin UI fügen Sie Palo Alto Networks – Admin UI wie folgt aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzu:

1. Wählen Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) die Option **Azure Active Directory** aus. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Wählen Sie **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Fenster „Unternehmensanwendungen“][2]
    
1. Klicken Sie oben im Fenster auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld **Palo Alto Networks – Admin UI** ein, wählen Sie in der Ergebnisliste **Palo Alto Networks – Admin UI** aus, und klicken Sie dann auf **Hinzufügen**.

    ![Palo Alto Networks - Admin UI in der Ergebnisliste](./media/paloaltoadmin-tutorial/tutorial_step4-add-from-the-gallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Palo Alto Networks – Admin UI anhand einer Testbenutzerin mit dem Namen „Britta Simon“.

Damit das einmalige Anmelden funktioniert, muss Azure AD Palo Alto Networks – Admin UI und sein Gegenstück in Azure AD identifizieren. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Palo Alto Networks – Admin UI muss eine Linkbeziehung eingerichtet werden.

Um die Linkbeziehung herzustellen, weisen Sie als *Benutzernamen* in Palo Alto Networks – Admin UI den Wert von *Benutzername* in Azure AD zu.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Palo Alto Networks – Admin UI führen Sie die Hauptschritte (Hauptelemente) in den nächsten fünf Abschnitten aus.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

Aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal, und konfigurieren Sie das einmalige Anmelden in Ihrer Palo Alto Networks – Admin UI-Anwendung, indem Sie die folgenden Schritte ausführen:

1. Wählen Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Palo Alto Networks – Admin UI** die Option **Einmaliges Anmelden** aus.

    ![Link „Einmaliges Anmelden“][4]

1. Wählen Sie im Fenster **Einmaliges Anmelden** im Feld **SSO-Mode****SAML-basierte Anmeldung** aus.
 
    ![Fenster „Einmaliges Anmelden“](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_samlbase.png)

1. Führen Sie unter **Domäne und URLs für Palo Alto Networks – Admin UI** die folgenden Schritte aus:

    ![Informationen für das einmalige Anmelden unter „Domäne und URLs für Palo Alto Networks – Admin UI“](./media/paloaltoadmin-tutorial/tutorial_general_show_advanced_url.png)
    
    a. Geben Sie im Feld **Anmelde-URL** eine URL im folgenden Format ein: *https://\<FQDN der Kundenfirewall>/php/login.php*.

    b. Geben Sie im Feld **Bezeichner** eine URL im folgenden Format ein: *https://\<FQDN der Kundenfirewall>:443/SAML20/SP*.
    
    c. Geben Sie im Feld **Antwort-URL** die URL des Assertionsverbraucherdiensts (ACS) im folgenden Format ein: *https://\<FQDN der Kundenfirewall>:443/SAML20/SP/ACS*.
    
    > [!NOTE] 
    > Die vorangehenden Werte sind keine echten Werte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Kundensupportteam von Palo Alto Networks – Admin UI](https://support.paloaltonetworks.com/support), um diese Werte zu erhalten. 
 
1. Da die Palo Alto Networks – Admin UI-Anwendung die SAML-Assertionen in einem bestimmten Format erwartet, konfigurieren Sie die Ansprüche wie in der folgenden Abbildung dargestellt. Verwalten Sie die Attributwerte im Abschnitt **Benutzerattribute** der Seite **Anwendungsintegration**, indem Sie die folgenden Schritte ausführen:
    
    ![Liste „SAML-Tokenattribute“](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_attribute.png)
    
   > [!NOTE]
   > Bei den Attributwerten handelt es sich nur um Beispiele. Sie müssen die entsprechenden Werte für *username* und *adminrole* zuordnen. Es ist ein weiteres optionales Attribut *accessdomain* verfügbar, mit dem der Administratorzugriff auf bestimmte virtuelle Systeme in der Firewall beschränkt wird.
   >
        
    | Attributname | Attributwert |
    | --- | --- |    
    | username | user.userprincipalname |
    | adminrole | customadmin |

    a. Klicken Sie auf **Attribut hinzufügen**.  
    
    ![Schaltfläche „Attribut hinzufügen“](./media/paloaltoadmin-tutorial/tutorial_attribute_04.png)

    Das Fenster **Attribut hinzufügen** wird geöffnet.

    ![Fenster „Attribut hinzufügen“](./media/paloaltoadmin-tutorial/tutorial_attribute_05.png)
    
    b. Geben Sie im Feld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie im Feld **Wert** den für diese Zeile angezeigten Attributwert ein.
    
    d. Klicken Sie auf **OK**.

    > [!NOTE]
    > Weitere Informationen zu den Attributen finden Sie in den folgenden Artikeln:
    > * [Administrative role profile for Admin UI (adminrole)](https://www.paloaltonetworks.com/documentation/80/pan-os/pan-os/firewall-administration/manage-firewall-administrators/configure-an-admin-role-profile) (Administratorrollenprofil für Admin UI – adminrole)
    > * [Device access domain for Admin UI (accessdomain)](https://www.paloaltonetworks.com/documentation/80/pan-os/web-interface-help/device/device-access-domain) (Gerätezugriffsdomäne für Admin UI – accessdomain)
    >

1. Wählen Sie unter **SAML-Signaturzertifikat** die Option **Metadaten-XML** aus, und klicken Sie auf **Speichern**.

    ![Downloadlink „Metadaten-XML“](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_certificate.png) 

    ![Die Schaltfläche „Speichern“](./media/paloaltoadmin-tutorial/tutorial_general_400.png)

1. Öffnen Sie als Administrator in einem neuen Fenster die Administratoroberfläche für die Palo Alto Networks-Firewall.

1. Klicken Sie auf die Registerkarte **Device** (Gerät).

    ![Registerkarte „Device“ (Gerät)](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin1.png)

1. Wählen Sie im linken Bereich **SAML Identity Provider** (SAML-Identitätsanbieter) aus, und klicken Sie anschließend auf **Import** (Importieren), um die Metadatendatei zu importieren.

    ![Schaltfläche zum Importieren der Metadatendatei](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin2.png)

1. Führen Sie im Fenster **SAML Identity Provider Server Profile Import** (Import des SAML-Identitätsanbieter-Serverprofils) die folgenden Schritte aus:

    ![Fenster „SAML Identity Provider Server Profile Import“ (Import des SAML-Identitätsanbieter-Serverprofils)](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp.png)

    a. Geben Sie im Feld **Profile Name** (Profilname) einen Namen ein (z. B. **AzureAD Admin UI**).
    
    b. Klicken Sie unter **Identity Provider Metadata** (Identitätsanbieter-Metadaten) auf **Browse** (Durchsuchen), und wählen Sie die Datei „metadata.xml“ aus, die Sie zuvor aus dem Azure-Portal heruntergeladen haben.
    
    c. Deaktivieren Sie das Kontrollkästchen **Validate Identity Provider Certificate** (Zertifikat des Identitätsanbieters überprüfen).
    
    d. Klicken Sie auf **OK**.
    
    e. Committen Sie die Konfigurationen der Firewall, indem Sie auf die Schaltfläche **Commit** (Committen) klicken.

1. Wählen Sie im linken Bereich **SAML Identity Provider** (SAML-Identitätsanbieter) und anschließend das SAML-Identitätsanbieterprofil (z. B. **AzureAD Admin UI**) aus, das Sie im vorherigen Schritt erstellt haben. 

    ![SAML-Identitätsanbieterprofil](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp_select.png)

1. Führen Sie im Fenster **SAML Identity Provider Server Profile** (SAML-Identitätsanbieter-Serverprofil) die folgenden Schritte aus:

    ![Fenster „SAML Identity Provider Server Profile“ (SAML-Identitätsanbieter-Serverprofil)](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_slo.png)
  
    a. Ersetzen Sie im Feld **Identity Provider SLO URL** (SLO-URL des Identitätsanbieters) die zuvor importierte SLO-URL durch die folgende URL: **https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0**.
  
    b. Klicken Sie auf **OK**.

1. Klicken Sie auf der Administratoroberfläche für die Palo Alto Networks-Firewall auf **Device** (Gerät) und dann auf **Admin Roles** (Administratorrollen).

1. Wählen Sie die Schaltfläche **Hinzufügen** aus. 

1. Geben Sie im Fenster **Admin Role Profile** (Administratorrollenprofil) einen Namen für die Administratorrolle in das Feld **Name** ein (z. B. **fwadmin**).  
    Der Administratorrollenname sollte mit dem vom Identitätsanbieter gesendeten Attributnamen für die SAML-Administratorrolle identisch sein. Der Name und der Wert für die Administratorrolle wurden in Schritt 4 erstellt.

    ![Konfigurieren der Administratorrolle für Palo Alto Networks](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_adminrole.png)
  
1. Klicken Sie auf der Administratoroberfläche der Firewall auf **Device** (Gerät) und dann auf **Authentication Profile** (Authentifizierungsprofil).

1. Wählen Sie die Schaltfläche **Hinzufügen** aus. 

1. Führen Sie im Fenster **Authentication Profile** (Authentifizierungsprofil) die folgenden Schritte aus: 

    ![Fenster „Authentication Profile“ (Authentifizierungsprofil)](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authentication_profile.png)

    a. Geben Sie im Feld **Name** einen Namen ein (z. B. **AzureSAML_Admin_AuthProfile**).
    
    b. Wählen Sie in der Dropdownliste **Type** (Typ) den Eintrag **SAML** aus. 
   
    c. Wählen Sie in der Dropdownliste **IdP Server Profile** (IdP-Serverprofil) das entsprechende SAML-Identitätsanbieter-Serverprofil aus (z. B. **AzureAD Admin UI**).
   
    c. Aktivieren Sie das Kontrollkästchen **Enable Single Logout** (Einmaliges Abmelden aktivieren).
    
    d. Geben Sie im Feld **Admin Role Attribute** (Administratorrollenattribut) den Attributnamen ein (z. B. **adminrole**). 
    
    e. Klicken Sie auf die Registerkarte **Advanced** (Erweitert) und dann unter **Allow List** (Liste „Zulassen) auf **Add** (Hinzufügen). 
    
    ![Schaltfläche „Add“ (Hinzufügen) auf der Registerkarte „Advanced“ (Erweitert)](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_allowlist.png)
    
    f. Aktivieren Sie das Kontrollkästchen **All** (Alle), oder wählen Sie die Benutzer und Gruppen aus, die sich mit diesem Profil authentifizieren können.  
    Bei der Authentifizierung eines Benutzers gleicht die Firewall den zugewiesenen Benutzernamen bzw. die zugewiesene Gruppe mit den Einträgen in der Liste ab. Wenn Sie keine Einträge hinzufügen, können sich keine Benutzer authentifizieren.

    g. Klicken Sie auf **OK**.

1. Um Administratoren die Verwendung von SAML-SSO mit Azure zu ermöglichen, klicken Sie auf **Device** > **Setup** (Gerät > Einrichten). Klicken Sie im Bereich **Setup** (Einrichten) auf die Registerkarte **Management** (Verwaltung) und dann unter **Authentication Settings** (Authentifizierungseinstellungen) auf die Schaltfläche mit dem Zahnradsymbol (**Einstellungen**). 

 ![Schaltfläche „Settings“ (Einstellungen)](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsetup.png)

1. Wählen Sie das SAML-Authentifizierungsprofil aus, das Sie in Schritt 17 erstellt haben (z. B. **AzureSAML_Admin_AuthProfile**).

 ![Feld „Authentication Profile“ (Authentifizierungsprofil)](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsettings.png)

1. Klicken Sie auf **OK**.

1. Klicken Sie zum Committen der Konfiguration auf **Commit** (Committen).


> [!TIP]
> Zum Einrichten der App können Sie eine Kurzversion der vorstehenden Anweisungen im [Azure-Portal](https://portal.azure.com) lesen. Nachdem Sie diese App über den Abschnitt **Active Directory** > **Unternehmensanwendungen** hinzugefügt haben, navigieren Sie zur Registerkarte **Einmaliges Anmelden**, und rufen Sie unten im Abschnitt **Konfiguration** die eingebettete Dokumentation auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie unter [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal wie folgt einen Testbenutzer mit dem Namen Britta Simon:

![Erstellen eines Azure AD-Testbenutzers][100]

1. Klicken Sie im linken Bereich des Azure-Portals auf **Azure Active Directory**.

    ![Link „Azure Active Directory“](./media/paloaltoadmin-tutorial/create_aaduser_01.png)

1. Um die Liste der aktuellen Benutzer anzuzeigen, wählen Sie **Benutzer und Gruppen** > **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/paloaltoadmin-tutorial/create_aaduser_02.png)

1. Wählen Sie oben im Fenster **Alle Benutzer** die Option **Hinzufügen** aus.

    ![Schaltfläche „Hinzufügen“](./media/paloaltoadmin-tutorial/create_aaduser_03.png)
    
    Das Fenster **Benutzer** wird geöffnet.

1. Gehen Sie im Fenster **Benutzer** wie folgt vor:

    ![Fenster „Benutzer“](./media/paloaltoadmin-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-palo-alto-networks---admin-ui-test-user"></a>Erstellen eines Palo Alto Networks - Admin UI-Testbenutzers

Palo Alto Networks – Admin UI unterstützt die Just-in-Time-Benutzerbereitstellung. Wenn ein Benutzer nicht bereits vorhanden ist, wird er nach der erfolgreichen Authentifizierung automatisch im System erstellt. Zum Erstellen des Benutzers ist keine Aktion Ihrerseits erforderlich.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie der Benutzerin Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Palo Alto Networks – Admin UI gewähren. Führen Sie hierzu folgende Schritte aus:

![Zuweisen der Benutzerrolle][200] 

1. Öffnen Sie im Azure-Portal die **Anwendungsansicht**, navigieren Sie zur **Verzeichnisansicht**, und wählen Sie dann **Unternehmensanwendungen** > **Alle Anwendungen** aus.

    ![Links „Unternehmensanwendungen“ und „Alle Anwendungen“][201] 

1. Wählen Sie in der Liste **Anwendungen** den Eintrag **Palo Alto Networks – Admin UI** aus.

    ![Link „Palo Alto Networks – Admin UI“](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_app.png)  

1. Wählen Sie auf der linken Seite **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

1. Wählen Sie **Hinzufügen** und dann im Bereich **Zuweisung hinzufügen** die Option **Benutzer und Gruppen**.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Fenster **Benutzer und Gruppen** in der Liste **Benutzer** die Option **Britta Simon** aus.

1. Wählen Sie die Schaltfläche **Auswählen** aus.

1. Wählen Sie auf der Seite **Zuweisung hinzufügen** die Option **Zuweisen** aus.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Palo Alto Networks – Admin UI“ klicken, sollten Sie automatisch bei Ihrer Palo Alto Networks – Admin UI-Anwendung angemeldet werden.

Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/paloaltoadmin-tutorial/tutorial_general_01.png
[2]: ./media/paloaltoadmin-tutorial/tutorial_general_02.png
[3]: ./media/paloaltoadmin-tutorial/tutorial_general_03.png
[4]: ./media/paloaltoadmin-tutorial/tutorial_general_04.png

[100]: ./media/paloaltoadmin-tutorial/tutorial_general_100.png

[200]: ./media/paloaltoadmin-tutorial/tutorial_general_200.png
[201]: ./media/paloaltoadmin-tutorial/tutorial_general_201.png
[202]: ./media/paloaltoadmin-tutorial/tutorial_general_202.png
[203]: ./media/paloaltoadmin-tutorial/tutorial_general_203.png

