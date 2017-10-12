---
title: 'Tutorial: Azure Active Directory-Integration mit Absorb LMS | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Absorb LMS konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: jeedes
ms.openlocfilehash: 549e353bb41f1f31bc0441294d0a8066779402e6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a>Tutorial: Azure Active Directory-Integration mit Absorb LMS

In diesem Tutorial erfahren Sie, wie Sie Absorb LMS in Azure Active Directory (Azure AD) integrieren.

Die Integration von Absorb LMS in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Absorb LMS hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Absorb LMS anzumelden (per einmaligem Anmelden).
- Sie können Ihre Konten an einem zentralen Ort, im Azure-Portal, verwalten.

Weitere Informationen zur Integration von SaaS-Apps (Software-as-a-Service) in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Absorb LMS konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Absorb LMS-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Wir empfehlen Ihnen, für dieses Tutorial keine Produktionsumgebung zu verwenden.

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

* Hinzufügen von Absorb LMS über den Katalog
* Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="add-absorb-lms-from-the-gallery"></a>Hinzufügen von Absorb LMS über den Katalog
Zum Konfigurieren der Integration von Absorb LMS in Azure AD fügen Sie Absorb LMS aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzu.

Um Absorb LMS aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:

1. Klicken Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) auf das Symbol **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Bereich „Unternehmensanwendungen“][2]
    
3. Wählen Sie zum Hinzufügen einer Anwendung die Schaltfläche **Neue Anwendung** aus.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld den Suchbegriff **Absorb LMS** ein, wählen Sie im Ergebnisbereich **Absorb LMS** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**.

    ![Absorb LMS in der Ergebnisliste](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Absorb LMS mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Absorb LMS als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Sie müssen zwischen dem Azure AD-Benutzer und dem entsprechenden Absorb LMS-Benutzer eine Linkbeziehung einrichten.

Sie stellen die Linkbeziehung her, indem Sie den *Benutzernamen* in Azure AD als *Benutzernamen* in Absorb LMS zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD mit Absorb LMS führen Sie die Bausteine in den nächsten fünf Abschnitten aus.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Absorb LMS-Anwendung.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit Absorb LMS die folgenden Schritte aus:

1. Wählen Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Absorb LMS** die Option **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** im Feld **Modus** die Option **SAML-basierte Anmeldung** aus, um das einmalige Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Absorb LMS** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Absorb LMS](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_url.png)

    a. Geben Sie im Feld **Bezeichner** eine URL mit folgender Syntax ein: `https://<subdomain>.myabsorb.com/Account/SAML`.

    b. Geben Sie im Feld **Antwort-URL** eine URL mit folgender Syntax ein: `https://<subdomain>.myabsorb.com/Account/SAML`.
     
    > [!NOTE] 
    > Bei diesen URLs handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und den Antwort-URLs. Wenden Sie sich an das [Clientsupportteam von Absorb LMS](https://www.absorblms.com/support), um diese Werte zu erhalten. 

4. Wählen Sie im Abschnitt **SAML-Signaturzertifikat** in der Spalte **Download** die Option **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Signaturzertifikat](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_certificate.png) 

5. Wählen Sie **Speichern** aus.

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-absorblms-tutorial/tutorial_general_400.png)
    
6. Wählen Sie im Abschnitt **Absorb LMS-Konfiguration** die Option **Absorb LMS konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen, und kopieren Sie dann die **Abmelde-URL** im Abschnitt **Kurzübersicht**.

    ![Bereich für Absorb LMS-Konfiguration](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_configure.png) 

7. Melden Sie sich in einem neuen Webbrowserfenster bei der Absorb LMS-Unternehmenswebsite als Administrator an.

8. Wählen Sie oben rechts die Schaltfläche **Konto**. 

    ![Schaltfläche „Konto“](./media/active-directory-saas-absorblms-tutorial/1.png)

9. Wählen Sie im Bereich „Konto“ die Option **Portaleinstellungen**.

    ![Link „Portaleinstellungen“](./media/active-directory-saas-absorblms-tutorial/2.png)
    
10. Klicken Sie auf die Registerkarte **Benutzer** .

    ![Die Registerkarte „Benutzer“](./media/active-directory-saas-absorblms-tutorial/3.png)

11. Führen Sie auf der Konfigurationsseite für einmaliges Anmelden die folgenden Schritte aus:

    ![Seite für die Konfiguration des einmaligen Anmeldens](./media/active-directory-saas-absorblms-tutorial/4.png)

    a. Wählen Sie im Feld **Modus** die Option **Identity Provider Initiated** (Vom Identitätsanbieter initiiert).

    b. Öffnen Sie im Editor das Zertifikat, das Sie aus dem Azure-Portal heruntergeladen haben. Entfernen Sie die Tags **---BEGIN CERTIFICATE---** und **---END CERTIFICATE---**. Fügen Sie anschließend im Feld **Schlüssel** den verbleibenden Inhalt ein.
    
    c. Wählen Sie im Feld **Id Property** (ID-Eigenschaft) das Attribut aus, das Sie in Azure AD als Benutzerbezeichner konfiguriert haben. Wählen Sie beispielsweise **Username** aus, wenn in Azure AD *userPrincipalName* ausgewählt ist.

    d. Fügen Sie im Feld **Anmelde-URL** die **URL für den Benutzerzugriff** von der Seite **Eigenschaften** der Anwendung im Azure-Portal hinzu.

    e. Fügen Sie als **Abmelde-URL** den Wert für **Abmelde-URL** ein, den Sie im Azure-Portal im Fenster **Anmelden konfigurieren** kopiert haben.

12. Schalten Sie **Only Allow SSO Login** (Nur SSO-Anmeldung zulassen) auf **Ein** um.

    ![Umschalter „Only Allow SSO Login“ (Nur SSO-Anmeldung zulassen)](./media/active-directory-saas-absorblms-tutorial/5.png)

13. Wählen Sie **Speichern** aus.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen. Gehen Sie wie folgt vor, nachdem Sie die App über den Abschnitt **Active Directory** > **Unternehmensanwendungen** hinzugefügt haben: Navigieren Sie zur Registerkarte **Einmaliges Anmelden**, und rufen Sie am unteren Rand im Abschnitt **Konfiguration** die eingebettete Dokumentation auf. Weitere Informationen finden Sie unter [Verwalten des einmaligen Anmeldens für Unternehmens-Apps]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal eine Testbenutzerin mit dem Namen Britta Simon.

![Erstellen eines Azure AD-Testbenutzers][100]

Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:

1. Klicken Sie im linken Bereich des Azure-Portals auf **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-absorblms-tutorial/create_aaduser_01.png) 

2. Um die Liste der Benutzer anzuzeigen, wählen Sie **Benutzer und Gruppen** > **Alle Benutzer**.
    
    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-absorblms-tutorial/create_aaduser_02.png) 

3. Wählen Sie oben im Dialogfeld die Option **Hinzufügen**.
 
    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-absorblms-tutorial/create_aaduser_03.png) 

4. Führen Sie im Dialogfeld **Benutzer** folgende Schritte aus:
 
    ![Dialogfeld „Benutzer“](./media/active-directory-saas-absorblms-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Textfeld **Benutzername** die E-Mail-Adresse von Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert im Feld **Kennwort**.

    d. Klicken Sie auf **Erstellen**.

### <a name="create-an-absorb-lms-test-user"></a>Erstellen eines Absorb LMS-Testbenutzers

Damit sich Azure AD-Benutzer an Absorb LMS anmelden können, müssen sie in Absorb LMS eingerichtet werden.  

Für Absorb LMS ist die Einrichtung eine manuelle Aufgabe.

Gehen Sie wie folgt vor, um ein Benutzerkonto einzurichten:

1. Melden Sie sich bei der Absorb LMS-Unternehmenswebsite als Administrator an.

2. Wählen Sie im linken Bereich die Option **Benutzer**.

    ![Link für Absorb LMS-Benutzer](./media/active-directory-saas-absorblms-tutorial/absorblms_users.png)

3. Wählen Sie im Bereich **Benutzer** die Option **Benutzer**.

    ![Link „Benutzer“](./media/active-directory-saas-absorblms-tutorial/absorblms_userssub.png)

4. Wählen Sie in der Dropdownliste **Neu hinzufügen** die Option **Benutzer**.

    ![Dropdownliste „Neu hinzufügen“](./media/active-directory-saas-absorblms-tutorial/absorblms_createuser.png)

5. Gehen Sie auf der Seite **Benutzer hinzufügen** wie folgt vor:

    ![Seite „Benutzer hinzufügen“](./media/active-directory-saas-absorblms-tutorial/user.png)

    a. Geben Sie im Feld **Vorname** den Vornamen ein, z.B. **Britta**.

    b. Geben Sie im Feld **Nachname** den Nachnamen ein, z.B. **Simon**.
    
    c. Geben Sie im Feld **Benutzername** einen vollständigen Namen ein, z.B. **Britta Simon**.

    d. Geben Sie im Feld **Kennwort** das Kennwort von Britta Simon ein.

    e. Geben Sie das Kennwort im Feld **Kennwort bestätigen** noch einmal ein.
    
    f. Legen Sie den Umschalter **Ist aktiv** auf **Aktiv** fest.  

6. Wählen Sie **Speichern** aus.
 
### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie der Benutzerin Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf Absorb LMS gewähren.

![Zuweisen der Benutzerrolle][200]

Gehen Sie wie folgt vor, um die Zuweisung von Britta Simon zu Absorb LMS durchzuführen:

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, und wählen Sie dann **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Der Link „Alle Anwendungen“][201] 

2. Wählen Sie in der Liste **Anwendungen** die Option **Absorb LMS** aus.

    ![Absorb LMS-Link in der Anwendungsliste](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_app.png) 

3. Wählen Sie auf der linken Seite **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202] 

4. Wählen Sie **Hinzufügen** und dann im Bereich **Zuweisung hinzufügen** die Option **Benutzer und Gruppen**.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste **Benutzer** die Option **Britta Simon** aus.

6. Wählen Sie im Dialogfeld **Benutzer und Gruppen** die Schaltfläche **Auswählen**.

7. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel **Absorb LMS** wählen, werden Sie automatisch an Ihrer Absorb LMS-Anwendung angemeldet. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://msdn.microsoft.com/library/dn308586).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_203.png

