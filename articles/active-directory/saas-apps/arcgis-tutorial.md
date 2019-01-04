---
title: 'Tutorial: Azure Active Directory-Integration von ArcGIS Online | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und ArcGIS Online konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: 3284202ffaa6767a8dd4a6a5050dbdc928075237
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52846113"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Tutorial: Azure Active Directory-Integration von ArcGIS Online

In diesem Tutorial erfahren Sie, wie Sie ArcGIS Online in Azure Active Directory (Azure AD) integrieren.

Die Integration von Azure AD in ArcGIS Online bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf ArcGIS Online Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei ArcGIS Online anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Integration von Azure AD in ArcGIS Online konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein ArcGIS Online-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.
Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von ArcGIS Online aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-arcgis-online-from-the-gallery"></a>Hinzufügen von ArcGIS Online aus dem Katalog

Zum Konfigurieren der Integration von Azure AD in ArcGIS Online müssen Sie Ihrer Liste der verwalteten SaaS-Apps ArcGIS Online aus dem Katalog hinzufügen.

**Um ArcGIS Online aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte durch:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![image](./media/arcgis-tutorial/selectazuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![image](./media/arcgis-tutorial/a_select_app.png)
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![image](./media/arcgis-tutorial/a_new_app.png)

4. Geben Sie im Suchfeld **ArcGIS Online** ein, wählen Sie im Ergebnisbereich **ArcGIS Online** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![image](./media/arcgis-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei ArcGIS Online mithilfe einer Testbenutzerin namens „Britta Simon“.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in ArcGIS Online als Pendant für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in ArcGIS Online muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in ArcGIS Online den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD in ArcGIS Online müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines ArcGIS Online-Testbenutzers](#create-an-arcgis-online-test-user)**, um eine Entsprechung von Britta Simon in ArcGIS Online zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer ArcGIS Online-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in ArcGIS Online die folgenden Schritte durch:**

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **ArcGIS Online** die Option **Einmaliges Anmelden** aus.

    ![image](./media/arcgis-tutorial/b1_b2_select_sso.png)

2. Klicken Sie oben im Bildschirm auf **Modus für einmaliges Anmelden ändern**, um den Modus **SAML** auszuwählen.

      ![image](./media/arcgis-tutorial/b1_b2_saml_ssso.png)

3. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![image](./media/arcgis-tutorial/b1_b2_saml_sso.png)

4. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![image](./media/arcgis-tutorial/b1-domains_and_urlsedit.png)

5. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<companyname>.maps.arcgis.com`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `<companyname>.maps.arcgis.com`.

    ![image](./media/arcgis-tutorial/b1-domains_and_urls.png)

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam für den ArcGIS Online-Client](https://support.esri.com/en/), um diese Werte zu erhalten.

6. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um die Datei **Verbundmetadaten-XML** herunterzuladen, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![image](./media/arcgis-tutorial/federationxml.png)

7. Wenn Sie die Konfiguration in **ArcGIS Online** automatisieren möchten, müssen Sie die **Browsererweiterung „Meine Apps“ für die sichere Anmeldung** installieren, indem Sie auf **Erweiterung installieren** klicken.

    ![image](./media/arcgis-tutorial/install_extension.png)

8. Klicken Sie nach dem Hinzufügen der Erweiterung zum Browser auf **ArcGIS Online einrichten**, um zur Anwendung ArcGIS Online weitergeleitet zu werden. Geben Sie dort die Administratoranmeldeinformationen ein, um sich bei ArcGIS Online anzumelden. Die Browsererweiterung konfiguriert die Anwendung automatisch für Sie und automatisiert die Schritte 9 bis 13.

9. Wenn Sie ArcGIS Online manuell einrichten möchten, öffnen Sie ein neues Webbrowserfenster, melden Sie sich bei der ArcGIS-Unternehmenswebsite als Administrator an, und führen Sie die folgenden Schritte aus:

10. Klicken Sie auf **EINSTELLUNGEN BEARBEITEN**.

    ![Einstellungen bearbeiten](./media/arcgis-tutorial/ic784742.png "Einstellungen bearbeiten")

11. Klicken Sie auf **Sicherheit**.

    ![Sicherheit](./media/arcgis-tutorial/ic784743.png "Sicherheit")

12. Klicken Sie unter **Unternehmensanmeldungen** auf **IDENTITÄTSANBIETER FESTLEGEN**.

    ![Unternehmensanmeldungen](./media/arcgis-tutorial/ic784744.png "Unternehmensanmeldungen")

13. Führen Sie auf der Konfigurationsseite **Set Identity Provider** die folgenden Schritte aus.

    ![Identitätsanbieter festlegen](./media/arcgis-tutorial/ic784745.png "Identitätsanbieter festlegen")

    a. Geben Sie in das Textfeld **Name** den Namen Ihrer Organisation ein.

    b. Wählen Sie für **Metadaten für Unternehmensidentitätsanbieter werden bereitgestellt durch** die Option **eine Datei** aus.

    c. Klicken Sie auf **Datei auswählen**, um die heruntergeladene Metadatendatei hochzuladen.

    d. Klicken Sie auf **IDENTITÄTSANBIETER FESTLEGEN**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers 

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![image](./media/arcgis-tutorial/d_users_and_groups.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![image](./media/arcgis-tutorial/d_adduser.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![image](./media/arcgis-tutorial/d_userproperties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

### <a name="create-an-arcgis-online-test-user"></a>Erstellen eines ArcGIS Online-Testbenutzers

Damit sich Azure AD-Benutzer bei ArcGIS Online anmelden können, müssen sie in ArcGIS Online bereitgestellt werden.  
Im Fall von ArcGIS Online ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrem **ArcGIS** -Mandanten an.

2. Klicken Sie auf **MITGLIEDER EINLADEN**.
   
    ![Mitglieder einladen](./media/arcgis-tutorial/ic784747.png "Mitglieder einladen")

3. Wählen Sie **Mitglieder automatisch hinzufügen, ohne eine E-Mail zu senden** aus, und klicken Sie dann auf **WEITER**.
   
    ![Mitglieder automatisch hinzufügen](./media/arcgis-tutorial/ic784748.png "Mitglieder automatisch hinzufügen")

4. Führen Sie auf der Dialogfeldseite **Members** die folgenden Schritte aus:
   
     ![Hinzufügen und überprüfen Sie](./media/arcgis-tutorial/ic784749.png "hinzufügen und überprüfen")
    
     a. Geben Sie **E-Mail-Adresse**, **Vorname** und **Nachname** eines gültigen AAD-Kontos ein, das Sie bereitstellen möchten.
  
     b. Klicken Sie auf **HINZUFÜGEN UND ÜBERPRÜFEN**.
5. Überprüfen Sie die eingegebenen Daten, und klicken Sie dann auf **MITGLIEDER HINZUFÜGEN**.
   
    ![Mitglied hinzufügen](./media/arcgis-tutorial/ic784750.png "Mitglied hinzufügen")
        
    > [!NOTE]
    > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail und folgt einem Link zur Bestätigung des Kontos, bevor es aktiv wird.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf ArcGIS Online gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![image](./media/arcgis-tutorial/d_all_applications.png)

2. Wählen Sie in der Anwendungsliste **ArcGIS Online** aus.

    ![image](./media/arcgis-tutorial/d_all_application.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![image](./media/arcgis-tutorial/d_leftpaneusers.png)

4. Wählen Sie die Schaltfläche **Hinzufügen** und dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![image](./media/arcgis-tutorial/d_assign_user.png)

4. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

5. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „ArcGIS Online“ klicken, sollten Sie automatisch bei Ihrer ArcGIS Online-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



