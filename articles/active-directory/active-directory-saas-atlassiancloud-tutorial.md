---
title: 'Tutorial: Azure Active Directory-Integration mit Atlassian Cloud | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Atlassian Cloud konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/06/2017
ms.author: jeedes
ms.openlocfilehash: 25a7694cefde344b34d3a46535bf09209b306593
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Tutorial: Azure Active Directory-Integration mit Atlassian Cloud

In diesem Tutorial erfahren Sie, wie Sie Atlassian Cloud in Azure Active Directory (Azure AD) integrieren.

Die Integration von Atlassian Cloud in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Atlassian Cloud hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Atlassian Cloud anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Für das Konfigurieren der Azure AD-Integration mit Atlassian Cloud benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Um SAML-SSO für Atlassian Cloud-Produkte zu ermöglichen, müssen Sie Identity Manager einrichten. Weitere Informationen zu [Identity Manager]( https://www.atlassian.com/enterprise/cloud/identity-manager)

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Atlassian Cloud aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-atlassian-cloud-from-the-gallery"></a>Hinzufügen von Atlassian Cloud aus dem Katalog
Zum Konfigurieren der Integration von Atlassian Cloud in Azure AD müssen Sie Atlassian Cloud aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Atlassian Cloud aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Atlassian Cloud** ein, wählen Sie im Ergebnisbereich **Atlassian Cloud** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Atlassian Cloud in der Ergebnisliste](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden in Azure AD mit Atlassian Cloud.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Atlassian Cloud als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Atlassian Cloud muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Atlassian Cloud den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD mit Atlassian Cloud müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Atlassian Cloud-Testbenutzers](#create-an-atlassian-cloud-test-user)**, um eine Entsprechung von Britta Simon in Atlassian Cloud zu erhalten, die mit ihrer Benutzerdarstellung in Azure AD verknüpft ist
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Atlassian Cloud-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit Atlassian Cloud die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Atlassian Cloud** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Atlassian Cloud** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Atlassian Cloud](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url_new.png)
    
    a. Geben Sie im Textfeld **Bezeichner** die folgende URL ein: `https://id.atlassian.com/login`
    
    b. Geben Sie im Textfeld **Antwort-URL** folgende URL ein: `https://id.atlassian.com/login/saml/acs`

    c. Geben Sie im Textfeld **Relayzustand** eine URL nach folgendem Muster ein: `https://<instancename>.atlassian.net`

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für Atlassian Cloud](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL nach folgendem Muster ein: `https://<instancename>.atlassian.net`.

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Diese Werte können Sie dem Bildschirm für die Atlassian Cloud-SAML-Konfiguration entnehmen. Dies wird weiter unten im Tutorial erläutert.

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

7. Klicken Sie im Abschnitt **Atlassian Cloud-Konfiguration** auf **Atlassian Cloud konfigurieren**, um das Fenster **Anmelden konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Atlassian Cloud-Konfiguration](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

8. Melden Sie sich zum Konfigurieren von SSO für Ihre Anwendung mit Administratorrechten beim Atlassian-Portal an.

9. Navigieren Sie zu **Atlassian Site Administration** > **Organizations & Security** (Atlassian-Websiteverwaltung > Organisationen und Sicherheit). Erstellen und benennen Sie Ihre Organisation, sofern noch nicht geschehen. Klicken Sie dann im linken Navigationsbereich auf **Domains** (Domänen).

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

10. Wählen Sie die Methode zum Überprüfen Ihrer Domäne: **DNS** oder **HTTPS**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_17.png)

11. Klicken Sie für die DNS-Überprüfung auf der Seite **Domains** (Domänen) auf die Registerkarte **DNS**, und führen Sie die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_18.png)

    a. Klicken Sie auf **Copy** (Kopieren), um den Wert für Ihren TXT-Eintrag zu kopieren.

    b. Navigieren Sie in DNS zur Einstellungsseite zum Hinzufügen eines neuen Eintrags.

    c. Wählen Sie die Option zum Hinzufügen eines neuen Eintrags, und fügen Sie den Wert, den Sie auf der Seite **Domains** (Domänen) kopiert haben, ins Feld **Wert** ein. In Ihrem DNS kann dieses Feld auch **Antwort** oder **Beschreibung** heißen.

    d. Ihr DNS-Eintrag kann darüber hinaus die folgenden Felder enthalten:
    
    * **Eintragstyp**: Geben Sie **TXT** ein.
    * **Name/Host/Alias**: Übernehmen Sie den Standardwert (@ oder leer).
    * **Gültigkeitsdauer (TTL)**: Geben Sie **86400** ein.
    
    e.  Speichern Sie den Eintrag.

12. Kehren Sie zurück zur Seite **Domains** (Domänen) in der Organisationsverwaltung, und klicken Sie auf die Schaltfläche **Verify domain** (Domäne überprüfen). Geben Sie im Popupfeld Ihren Domänennamen ein, und klicken Sie auf die Schaltfläche **Verify domain** (Domäne überprüfen).

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_19.png)
    
    > [!NOTE]
    > Es kann bis zu 72 Stunden dauern, bis Änderungen an TXT-Einträgen übernommen werden. Sie wissen nicht direkt, ob die Domänenüberprüfung erfolgreich war. Überprüfen Sie kurz nach dem Ausführen dieser Schritte den Überprüfungsstatus auf der Seite **Domains** (Domänen). Auf dem folgenden Bildschirm wird als aktualisierter Status **VERIFIED** (ÜBERPRÜFT) angezeigt.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_20.png)

13. Klicken Sie für die HTTPS-Überprüfung auf der Seite **Domains** (Domänen) auf die Registerkarte **HTTPS**, und führen Sie die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_21.png)

    a.  Klicken Sie auf **Download file** (Datei herunterladen), um die HTML-Datei herunterzuladen.

    b.  Laden Sie die HTML-Datei in das Stammverzeichnis Ihrer Domäne hoch.

14. Kehren Sie zurück zur Seite **Domains** (Domänen) in der Organisationsverwaltung, und klicken Sie auf die Schaltfläche **Verify domain** (Domäne überprüfen). Geben Sie im Popupfeld Ihren Wert für **Domänenname** ein, und klicken Sie auf die Schaltfläche **Verify domain** (Domäne überprüfen).

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_22.png)

15. Falls bei der Überprüfung die ins Stammverzeichnis hochgeladene Datei gefunden wird, ändert sich der Domänenstatus in **Verified** (Überprüft).

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_23.png)

    > [!NOTE]
    > Weitere Informationen finden Sie in der [Atlassian-Dokumentation zur Domänenüberprüfung](https://confluence.atlassian.com/cloud/domain-verification-873871234.html).

16. Klicken Sie in der linken Navigationsleiste auf **SAML single sign-on** (SAML-SSO). Wenn nicht bereits geschehen, abonnieren Sie Identity Manager von Atlassian.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

17. Fügen Sie im Dialogfeld **Add SAML configuration** (SAML-Konfiguration hinzufügen) die Identitätsanbietereinstellungen wie folgt hinzu:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. Fügen Sie in das Textfeld **Entitäts-ID des Identitätsanbieters** den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.

    b. Fügen Sie in das Textfeld **SSO-URL des Identitätsanbieters** den Wert der **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Öffnen Sie das heruntergeladene Zertifikat im Editor, kopieren Sie die Werte ohne die Anfangs- und Endzeilen, und fügen Sie sie in das Feld **Public X509 certificate** (Öffentliches X509-Zertifikat) ein.
    
    d. Klicken Sie auf **Konfiguration speichern**, um die Einstellungen zu speichern.
     
18. Aktualisieren Sie die Azure AD-Einstellungen, um sicherzustellen, dass Sie die richtigen URLs eingerichtet haben.
  
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. Kopieren Sie auf dem SAML-Bildschirm die **SP-Identitäts-ID**, und fügen Sie den Wert im Azure-Portal im Atlassian Cloud-Abschnitt **Domäne und URLs** ins Feld **Bezeichner** ein.
    
    b. Kopieren Sie auf dem SAML-Bildschirm die **SP-Assertionsverbraucherdienst-URL**, und fügen Sie den Wert im Azure-Portal im Atlassian Cloud-Abschnitt **Domäne und URLs** ins Feld **Antwort-URL** ein.
    
    c. Die Anmelde-URL ist die Mandanten-URL von Atlassian Cloud. 
    
19. Klicken Sie im Azure-Portal auf die Schaltfläche **Speichern**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
  
### <a name="create-an-atlassian-cloud-test-user"></a>Erstellen eines Atlassian Cloud-Testbenutzers

Damit sich Azure AD-Benutzer bei Atlassian Cloud anmelden können, müssen sie in Atlassian Cloud bereitgestellt werden. Im Fall von Atlassian Cloud ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Klicken Sie im Abschnitt „Site Administration“ (Websiteverwaltung) des Atlassian-Portals auf die Schaltfläche **User** (Benutzer).

    ![Atlassian Cloud-Benutzer erstellen](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 

2. Klicken Sie auf die Schaltfläche **Invite user** (Benutzer einladen), um einen Benutzer in Atlassian Cloud zu erstellen.

    ![Atlassian Cloud-Benutzer erstellen](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 

3. Geben Sie die **E-Mail-Adresse** des Benutzers ein, und weisen Sie den Anwendungszugriff zu. 

    ![Atlassian Cloud-Benutzer erstellen](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
4. Klicken Sie auf die Schaltfläche **Invite users** (Benutzer einladen) Dadurch wird eine E-Mail-Einladung an den Benutzer gesendet, und nach der Annahme der Einladung ist der Benutzer im System aktiv. 

>[!NOTE] 
>Sie können auch mehrere Benutzer erstellen, indem Sie im Abschnitt „Benutzer“ auf die Schaltfläche **Bulk Create** (Massenerstellung) klicken.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie Zugriff auf Atlassian Cloud gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Atlassian Cloud zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Atlassian Cloud** aus.

    ![Wählen Sie in der Anwendungsliste Atlassian Cloud aus.](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Atlassian Cloud“ klicken, sollten Sie automatisch bei Ihrer Atlassian Cloud-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png

