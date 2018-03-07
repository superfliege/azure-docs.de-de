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
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: jeedes
ms.openlocfilehash: 60430f08f54232db619efd054ca3a7d9a44f4cdc
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2018
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---admin-ui"></a>Tutorial: Azure Active Directory-Integration mit Palo Alto Networks - Admin UI

In diesem Tutorial erfahren Sie, wie Sie Palo Alto Networks - Admin UI in Azure Active Directory (Azure AD) integrieren.

Die Integration von Palo Alto Networks - Admin UI in Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Palo Alto Networks - Admin UI hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Palo Alto Networks - Admin UI anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Palo Alto Networks - Admin UI konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Eine Firewall der nächsten Generation oder Panorama von Palo Alto Networks (zentralisiertes Verwaltungssystem für die Firewalls)

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Palo Alto Networks - Admin UI aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-palo-alto-networks---admin-ui-from-the-gallery"></a>Hinzufügen von Palo Alto Networks - Admin UI aus dem Katalog
Zum Konfigurieren der Integration von Palo Alto Networks - Admin UI in Azure AD müssen Sie Palo Alto Networks - Admin UI aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie zum Hinzufügen von Palo Alto Networks - Admin UI aus dem Katalog die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Palo Alto Networks - Admin UI** ein, wählen Sie im Ergebnisbereich **Palo Alto Networks - Admin UI** aus, und klicken Sie auf **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Palo Alto Networks - Admin UI in der Ergebnisliste](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_step4-add-from-the-gallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Palo Alto Networks - Admin UI mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Palo Alto Networks - Admin UI als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Palo Alto Networks - Admin UI muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Palo Alto Networks - Admin UI den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Palo Alto Networks - Admin UI müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Palo Alto Networks - Admin UI-Testbenutzers](#create-a-palo-alto-networks---admin-ui-test-user)**, um ein Pendant von Britta Simon in Palo Alto Networks - Admin UI zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden bei Ihrer Palo Alto Networks - Admin UI-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Palo Alto Networks - Admin UI die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Palo Alto Networks - Admin UI** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Palo Alto Networks - Admin UI** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Palo Alto Networks - Admin UI](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_show_advanced_url.png)
    
    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<Customer Firewall FQDN>/php/login.php`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<Customer Firewall FQDN>:443/SAML20/SP`
    
    c. Geben Sie im Textfeld **Antwort-URL** die Assertionsverbraucherdienst-URL (Assertion Consumer Service, ACS) in folgendem Format ein: `https://<Customer Firewall FQDN>:443/SAML20/SP/ACS`.
    

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam für den Palo Alto Networks - Admin UI-Client](https://support.paloaltonetworks.com/support), um diese Werte zu erhalten. 
 
4. Die Palo Alto Networks - Admin UI-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie für diese Anwendung die folgenden Ansprüche. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:
    
    ![Configure Single Sign-On](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_attribute.png)
    
5. Konfigurieren Sie das SAML-Tokenattribut im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute** wie in der obigen Abbildung gezeigt, und führen Sie die folgenden Schritte aus. Beachten Sie, dass es sich bei den Attributwerten lediglich um Beispiele handelt. Verwenden Sie die entsprechenden Werte für den Benutzernamen und die Administratorrolle. Es ist ein weiteres optionales Attribut (accessdomain) vorhanden. Mit diesem Attribut wird der Administratorzugriff auf bestimmte virtuelle Systeme in der Firewall beschränkt.
        
    | Attributname | Attributwert |
    | --- | --- |    
    | username | user.userprincipalname |
    | adminrole | customadmin |

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Configure Single Sign-On](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_attribute_04.png)

    ![Configure Single Sign-On](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_attribute_05.png)
    
    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.
    
    d. Klicken Sie auf **OK**.

    > [!NOTE]
    > Ausführlichere Informationen zu den Attributen finden Sie in den folgenden Artikeln:
    > 1. Administratorrollenprofil für Admin UI (adminrole): https://www.paloaltonetworks.com/documentation/80/pan-os/pan-os/firewall-administration/manage-firewall-administrators/configure-an-admin-role-profile
    > 2. Gerätezugriffsdomäne für Admin UI (accessdomain): https://www.paloaltonetworks.com/documentation/80/pan-os/web-interface-help/device/device-access-domain

6. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_certificate.png) 

7. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_400.png)

8. Öffnen Sie als Administrator die Administratoroberfläche für die Palo Alto Networks-Firewall in einem anderen Browserfenster.

9. Klicken Sie auf **Device** (Gerät).

    ![Konfigurieren des einmaligen Anmeldens für Palo Alto](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin1.png)

10. Klicken Sie in der linken Navigationsleiste auf **SAML-Identitätsanbieter** und anschließend auf „Importieren“, um die Metadatendatei zu importieren.

    ![Konfigurieren des einmaligen Anmeldens für Palo Alto](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin2.png)

11. Führen Sie im Fenster zum Importieren die folgenden Aktionen aus:

    ![Konfigurieren des einmaligen Anmeldens für Palo Alto](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp.png)

    a. Geben Sie im Textfeld **Profile Name** (Profilname) einen Namen ein, etwa „AzureAD Admin UI“.
    
    b. Klicken Sie unter **Identity Provider Metadata** (Metadaten des Identitätsanbieters) auf **Browse** (Durchsuchen), und wählen Sie die Datei „metadata.xml“ aus, die Sie aus dem Azure-Portal heruntergeladen haben.
    
    c. Deaktivieren Sie **Validate Identity Provider Certificate** (Zertifikat des Identitätsanbieters überprüfen).
    
    d. Klicken Sie auf **OK**
    
    e. Committen Sie die Konfigurationen der Firewall, indem Sie auf die Schaltfläche **Commit** (Committen) klicken.

12. Klicken Sie links auf der Navigationsleiste auf **SAML Identity Provider** (SAML-Identitätsanbieter), und klicken Sie dann auf das im vorherigen Schritt erstellte SAML-Identitätsanbieterprofil (z.B. „AzureAD Admin UI“). 
    
  ![Konfigurieren des einmaligen Anmeldens für Palo Alto Networks](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp_select.png)

13. Führen Sie im Fenster **SAML Identity Provider Server Profile** (Profil des SAML-Identitätsanbieterservers) die folgenden Aktionen aus:

  ![Konfigurieren des einmaligen Abmeldens für Palo Alto Networks](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_slo.png)
  
  a. Entfernen Sie im Textfeld **Identity Provider SLO URL** (SLO-URL des Identitätsanbieters) die zuvor importierte SLO-URL, und fügen Sie die folgende URL hinzu: `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0`
  
  b. Klicken Sie auf **OK**


14. Klicken Sie auf der Administratoroberfläche für die Palo Alto Networks-Firewall auf **Device** (Gerät) und dann auf **Admin Roles** (Administratorrollen).

15. Klicken Sie auf die Schaltfläche **Hinzufügen** . Geben Sie im Fenster für das Administratorrollenprofil einen Namen für die Administratorrolle ein (etwa „fwadmin“). Dieser Administratorrollenname sollte mit dem vom Identitätsanbieter gesendeten Attributnamen für die SAML-Administratorrolle übereinstimmen. In Schritt 5 wurden der Name und der Wert für die Administratorrolle erstellt. 

  ![Konfigurieren der Administratorrolle für Palo Alto Networks](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_adminrole.png)
  
16. Klicken Sie auf der Administratoroberfläche der Firewall auf **Device** (Gerät) und dann auf **Authentication Profile** (Authentifizierungsprofil).

17. Klicken Sie auf die Schaltfläche **Hinzufügen** . Führen Sie im Fenster „Authentication Profile“ (Authentifizierungsprofil) die folgenden Schritte aus: 

 ![Konfigurieren des Palo Alto Networks-Authentifizierungsprofils](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_authentication_profile.png)

   a. Geben Sie im Textfeld **Name** einen Namen ein, etwa „AzureSAML_Admin_AuthProfile“.
    
   b. Wählen Sie in der Dropdownliste **Type** (Typ) die Option **SAML** aus. 
   
   c. Wählen Sie in der Dropdownliste „IdP Server Profile“ (IdP-Serverprofil) das entsprechende SAML-Identitätsanbieterserver-Profil aus (etwa „AzureAD Admin UI“).
   
   c. Aktivieren Sie das Kontrollkästchen **Enable Single Logout** (Einmaliges Abmelden ermöglichen).
    
   d. Geben Sie im Textfeld „Admin Role Attribute“ (Administratorrollenattribut) den Attributnamen (z.B. „adminrole“) ein. 
   
   e. Klicken Sie auf die Registerkarte „Advanced“ (Erweitert) und dann im Bereich „Allow List“ (Zulassungsliste) auf die Schaltfläche **Add** (Hinzufügen). Wählen Sie alle oder bestimmte Benutzer und Gruppen aus, die sich mit diesem Profil authentifizieren können. Bei der Authentifizierung eines Benutzers gleicht die Firewall den zugewiesenen Benutzernamen bzw. die zugewiesene Gruppe mit den Einträgen in der Liste ab. Wenn Sie keine Einträge hinzufügen, können sich keine Benutzer authentifizieren.
   
   ![Konfigurieren des Palo Alto Networks-Authentifizierungsprofils](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_allowlist.png)
   
   f. Klicken Sie auf **OK**

18. Damit Administratoren SAML-SSO mit Azure verwenden können, klicken Sie auf **Device** (Gerät) und dann auf **Setup**. Klicken Sie im Bereich „Setup“ auf die Registerkarte **Management** (Verwaltung) und dann unter **Authentication Settings** (Authentifizierungseinstellungen) auf das Zahnradsymbol. 

 ![Konfigurieren der Palo Alto Networks-Authentifizierungseinstellungen](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsetup.png)

19. Wählen Sie das in Schritt 17 erstellte SAML-Authentifizierungsprofil aus. (Beispiel: AzureSAML_Admin_AuthProfile)

 ![Konfigurieren der Palo Alto Networks-Authentifizierungseinstellungen](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsettings.png)

20. Klicken Sie auf **OK**

21. Committen Sie die Konfiguration, indem Sie auf die Schaltfläche **Commit** (Committen) klicken.


> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-palo-alto-networks---admin-ui-test-user"></a>Erstellen eines Palo Alto Networks - Admin UI-Testbenutzers

Palo Alto Networks - Admin UI unterstützt die Just-In-Time-Bereitstellung von Benutzern. Daher wird ein Benutzer nach der erfolgreichen Authentifizierung automatisch im System erstellt, sofern er noch nicht vorhanden ist. Sie müssen keine Aktion ausführen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf Palo Alto Networks - Admin UI gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie zum Hinzufügen von Britta Simon zu Palo Alto Networks - Admin UI die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Palo Alto Networks - Admin UI** aus.

    ![Der Link für Palo Alto Networks - Admin UI in der Anwendungsliste](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Palo Alto Networks - Admin UI“ klicken, sollten Sie automatisch bei Ihrer Palo Alto Networks - Admin UI-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_203.png

