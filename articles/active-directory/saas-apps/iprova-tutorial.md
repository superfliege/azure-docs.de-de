---
title: 'Tutorial: Azure Active Directory-Integration mit iProva | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und iProva konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 1eaeef9b-4479-4a9f-b1b2-bc13b857c75c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 395d3887e35d6e9c043d4d947b605e71eb58bd6b
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57897332"
---
# <a name="tutorial-azure-active-directory-integration-with-iprova"></a>Tutorial: Azure Active Directory-Integration mit iProva

In diesem Tutorial erfahren Sie, wie Sie iProva in Azure Active Directory (Azure AD) integrieren.
Die Integration von iProva in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf iProva hat.
* Sie können Ihren Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei iProva anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten an einem zentralen Ort, im Azure-Portal, verwalten.

Weitere Informationen zur Integration von SaaS-Apps (Software-as-a-Service) in Azure AD finden Sie unter [Einmaliges Anmelden bei Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit iProva konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über keine Azure AD-Umgebung verfügen, erhalten Sie auf der [Microsoft Azure-Website](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion.
* Ein SSO-fähiges iProva-Abonnement.

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung:

* iProva unterstützt SP-initiiertes einmaliges Anmelden.

## <a name="add-iprova-from-the-gallery"></a>Hinzufügen von iProva über den Katalog

Zum Konfigurieren der Integration von iProva in Azure AD müssen Sie iProva über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

Gehen Sie wie folgt vor, um iProva über den Katalog hinzuzufügen:

1. Wählen Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) die Option **Azure Active Directory** aus.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Wählen Sie oben im Dialogfeld **Neue Anwendung** aus, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie **iProva** in das Suchfeld ein. Wählen Sie im Ergebnisbereich **iProva** und anschließend **Hinzufügen** aus, um die Anwendung hinzuzufügen.

     ![iProva in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei iProva mithilfe eines Testbenutzers namens Britta Simon.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in iProva eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit iProva die folgenden Schritte aus:

- [Abrufen von Konfigurationsinformationen aus iProva](#retrieve-configuration-information-from-iprova) als Vorbereitung für die nächsten Schritte.
- [Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on), um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
- [Konfigurieren des einmaligen Anmeldens für iProva](#configure-iprova-single-sign-on), um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
- [Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user), um das einmalige Anmelden mit Azure AD mit der Testbenutzerin Britta Simon zu testen.
- [Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user), um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
- [Erstellen eines iProva-Testbenutzers](#create-an-iprova-test-user), um in iProva ein Pendant von Britta Simon zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
- [Testen der einmaligen Anmeldung](#test-single-sign-on), um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="retrieve-configuration-information-from-iprova"></a>Abrufen von Konfigurationsinformationen von iProva

In diesem Abschnitt rufen Sie Informationen aus iProva ab, um das einmalige Anmelden von Azure AD zu konfigurieren.

1. Öffnen Sie einen Webbrowser, und verwenden Sie das folgende URL-Muster, um zur **SAML2-Informationsseite** in iProva zu gelangen:

    | | |
    |-|-|
    | `https://SUBDOMAIN.iprova.nl/saml2info`|
    | `https://SUBDOMAIN.iprova.be/saml2info`|
    | | |

     ![Anzeigen der iProva SAML2-Informationsseite](media/iprova-tutorial/iprova-saml2-info.png)

2. Lassen Sie die Registerkarte Browser geöffnet, während Sie mit den nächsten Schritten in einer anderen Registerkarte des Browsers fortfahren.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit iProva zu konfigurieren:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **iProva** die Option **Einmaliges Anmelden** aus.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Wählen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** das Symbol **Bearbeiten** aus, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeitungssymbol in der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    a. Füllen Sie das Feld **Bezeichner** mit dem Wert aus, der hinter der Bezeichnung **EntityID** auf der **iProva SAML2-Informationsseite** angezeigt wird. Diese Seite ist auf Ihrer anderen Browserregisterkarte weiterhin geöffnet.

    b. Füllen Sie das Feld **Antwort-URL** mit dem Wert aus, der hinter der Bezeichnung **Antwort-URL** auf der **iProva SAML2-Informationsseite** angezeigt wird. Diese Seite ist auf Ihrer anderen Browserregisterkarte weiterhin geöffnet.

    c. Füllen Sie das Feld **Anmelde-URL** mit dem Wert aus, der hinter der Bezeichnung **Anmelde-URL** auf der **iProva SAML2-Informationsseite** angezeigt wird. Diese Seite ist auf Ihrer anderen Browserregisterkarte weiterhin geöffnet.

    ![SSO-Informationen zur Domäne und zu den URLs für iProva](common/sp-identifier-reply.png)

5. Die iProva-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Wählen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** das Symbol **Bearbeiten** aus, um das Dialogfeld **Benutzerattribute** zu öffnen.

    ![Dialogfeld „Benutzerattribute“](common/edit-attribute.png)

6. Konfigurieren Sie im Dialogfeld **Benutzerattribute** im Abschnitt **Benutzeransprüche** das SAML-Tokenattribut, wie in der obigen Abbildung gezeigt. Führen Sie folgende Schritte durch:

    | NAME | Quellattribut| Namespace |
    | ---------------| -------- | -----|
    | `samaccountname` | `user.onpremisessamaccountname`| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|
    | | |

    a. Wählen Sie **Neuen Anspruch hinzufügen** aus, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    ![Benutzeransprüche](common/new-save-attribute.png)

    ![Dialogfeld „Benutzeransprüche verwalten“](common/new-attribute-details.png)

    b. Geben Sie im Feld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Geben Sie in der Liste **Namespace** den für diese Zeile angezeigten Namespacewert ein.

    d. Wählen Sie für **Quelle** die Option **Attribut** aus.

    e. Geben Sie in der Liste **Quellattribut** den für diese Zeile angezeigten Attributwert ein.

    f. Klicken Sie auf **OK**.

    g. Wählen Sie **Speichern** aus.

7. Wählen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** das **Kopiersymbol** aus, um die **App-Verbundmetadaten-URL** zu kopieren, und speichern Sie sie auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](common/copy-metadataurl.png)

### <a name="configure-iprova-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für iProva

1. Melden Sie sich mit dem **Administratorkonto** bei iProva an.

2. Öffnen Sie das Menü **Gehe zu**.

3. Wählen Sie **Anwendungsverwaltung** aus.

4. Wählen Sie im Bereich **Systemeinstellungen** die Option **Allgemein** aus.

5. Wählen Sie **Bearbeiten** aus.

6. Scrollen Sie zu **Zugriffssteuerung**.

    ![Einstellungen für die iProva-Zugriffssteuerung](media/iprova-tutorial/iprova-accesscontrol.png)

7. Suchen Sie die Einstellung **Benutzer werden automatisch mit ihren Netzwerkkonten angemeldet**, und ändern Sie sie in **Ja, Authentifizierung über SAML**. Daraufhin werden zusätzliche Optionen angezeigt.

8. Wählen Sie **Einrichten** aus.

9. Klicken Sie auf **Weiter**.

10. Sie werden von iProva gefragt, ob Sie Verbunddaten von einer URL herunterladen oder aus einer Datei hochladen möchten. Wählen Sie die Option **Von URL** aus.

    ![Herunterladen von Azure AD-Metadaten](media/iprova-tutorial/iprova-download-metadata.png)

11. Fügen Sie die Metadaten-URL ein, die Sie im letzten Schritt des Abschnitts „Konfigurieren des einmaliges Anmeldens bei Azure AD“ gespeichert haben.

12. Wählen Sie die pfeilförmige Schaltfläche aus, um die Metadaten aus Azure AD herunterzuladen.

13. Nach Abschluss des Downloadvorgangs wird die Bestätigungsmeldung **Gültige Verbunddatendatei heruntergeladen** angezeigt.

14. Klicken Sie auf **Weiter**.

15. Überspringen Sie vorerst die Option **Anmeldung testen**, und wählen Sie **Weiter** aus.

16. Wählen Sie in der Dropdownliste **Verwendung beanspruchen** die Option **windowsaccountname** aus.

17. Wählen Sie **Fertig stellen** aus.

18. Sie kehren jetzt zum Bildschirm **Allgemeine Einstellungen bearbeiten** zurück. Scrollen Sie zum unteren Seitenrand, und wählen Sie **OK** aus, um die Konfiguration zu speichern.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer namens Britta Simon.

1. Wählen Sie im linken Bereich des Azure-Portals **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](common/users.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Schaltfläche „Neuer Benutzer“](common/new-user.png)

3. Gehen Sie im Dialogfeld **Benutzer** wie folgt vor:

    ![Dialogfeld „Benutzer“](common/user-properties.png)

    a. Geben Sie im Feld **Name** einen Namen ein (etwa **BrittaSimon**).
  
    b. Geben Sie im Feld **Benutzername** Folgendes ein: *IhrName\@ihreunternehmensdomäne.erweiterung* ein. 
    Ein Beispiel ist BrittaSimon@contoso.com.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert aus dem Feld **Kennwort**.

    d. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie den Zugriff auf iProva gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** > **iProva** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **iProva** aus.

    ![iProva-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Wählen Sie die Schaltfläche **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Dialogfeld „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste **Benutzer** den Eintrag **Britta Simon** und anschließend am unteren Bildschirmrand **Auswählen** aus.

6. Falls Sie in der SAML-Assertion einen Rollenwert erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer aus der Liste aus. Wählen Sie am unteren Bildschirmrand **Auswählen** aus.

7. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Option **Zuweisen** aus.

### <a name="create-an-iprova-test-user"></a>Erstellen eines iProva-Testbenutzers

1. Melden Sie sich mit dem **Administratorkonto** bei iProva an.

2. Öffnen Sie das Menü **Gehe zu**.

3. Wählen Sie **Anwendungsverwaltung** aus.

4. Wählen Sie im Bereich **Benutzer und Benutzergruppen** die Option **Benutzer** aus.

5. Wählen Sie **Hinzufügen**.

6. Geben Sie im Feld **Benutzername** Folgendes ein: *brittasimon\@ihreunternehmensdomäne.erweiterung*. 
    Ein Beispiel ist BrittaSimon@contoso.com.

7. Geben Sie im Feld **Vollständiger Name** einen vollständigen Namen ein (etwa **BrittaSimon**).

8. Wählen Sie die Option **Kein Kennwort (einmaliges Anmelden verwenden)** aus.

9. Geben Sie im Feld **E-Mail-Adresse** Folgendes ein: *IhrName\@ihreunternehmensdomäne.erweiterung* ein. 
   Ein Beispiel ist BrittaSimon@contoso.com.

10. Scrollen Sie zum Seitenende, und wählen Sie **Fertig stellen** aus.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „iProva“ klicken, sollten Sie automatisch bei der iProva-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [iProva – Konfigurieren des einmaligen Anmeldens für SAML2](https://webshare.iprova.nl/0wqwm45yn09f5poh/Document.aspx)
