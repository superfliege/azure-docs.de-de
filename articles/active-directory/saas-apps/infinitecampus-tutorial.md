---
title: 'Tutorial: Integration von Infinite Campus in Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Infinite Campus konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3995b544-e751-4e0f-ab8b-c9a3862da6ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 53eb0c4ad5c0a21f46985062ef8202a87dc0d5e6
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56189360"
---
# <a name="tutorial-azure-active-directory-integration-with-infinite-campus"></a>Tutorial: Integration von Infinite Campus in Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie Infinite Campus in Azure Active Directory (Azure AD) integrieren.

Die Integration von Infinite Campus in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Infinite Campus hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Infinite Campus anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Integration von Infinite Campus in Azure AD konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Infinite Campus-Abonnement, das für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).
- Sie müssen mindestens ein Azure Active Directory-Administrator sein, um die Konfiguration durchzuführen.

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Infinite Campus aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-infinite-campus-from-the-gallery"></a>Hinzufügen von Infinite Campus aus dem Katalog

Zum Konfigurieren der Integration von Infinite Campus in Azure AD müssen Sie Infinite Campus aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Infinite Campus aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Infinite Campus** ein, wählen Sie im Ergebnisbereich **Infinite Campus** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Infinite Campus in der Ergebnisliste](./media/infinitecampus-tutorial/tutorial_infinitecampus_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Infinite Campus basierend auf einer Testbenutzerin mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Infinite Campus als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Infinite Campus muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Infinite Campus müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. **[Erstellen eines Infinite Campus-Testbenutzers](#creating-a-infinite-campus-test-user)**, um eine Entsprechung für Britta Simon in Infinite Campus zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Infinite Campus-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Infinite Campus die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Infinite Campus** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Klicken Sie im Dialogfeld **SSO-Methode auswählen** für den Modus **SAML** auf **Auswählen**, um einmaliges Anmelden zu aktivieren.

    ![Configure single sign-on](common/tutorial_general_301.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Configure single sign-on](common/editconfigure.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Schritte 4.a bis 4.d aus, wenn Sie über eine **Dienstanbieter-Metadatendatei** verfügen, und fahren Sie dann mit Schritt 11.c fort. Wenn Sie keine Dienstanbieter-Metadatendatei haben, fahren Sie mit Schritt 5 fort.

    a. Klicken Sie auf **Metadatendatei hochladen**.

        ![image](common/b9_saml.png)

    b. Klicken Sie auf das **Ordnerlogo**, wählen Sie die Metadatendatei aus, und klicken Sie auf **Hochladen**.

    ![image](common/b9(1)_saml.png)

    c. Nach dem erfolgreichen Upload der Metadatendatei werden die Werte **Bezeichner** und **Antwort-URL** im Abschnitt **Grundlegende SAML-Konfiguration** in den entsprechenden Textfeldern automatisch ausgefüllt (wie unten dargestellt):

    ![image](./media/infinitecampus-tutorial/tutorial_infinitecampus_url.png)

    d. Geben Sie im Textfeld **Anmelde-URL** eine URL nach folgendem Muster ein (die Domäne variiert je nach Hostingmodell): `https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>/SIS`

5. Führen Sie die folgenden Schritte aus, wenn Sie keine **Dienstanbieter-Metadatendatei** haben (die Domäne variiert je nach Hostingmodell):

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>/SIS`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<DOMAIN>.infinitecampus.com/campus/<DISTRICTNAME>`

    c. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>`

    ![SSO-Informationen zur Domäne und zu den URLs für Infinite Campus](./media/infinitecampus-tutorial/tutorial_infinitecampus_url1.png)

6. Klicken Sie auf der Seite **SAML-Signaturzertifikat** im Abschnitt **SAML-Signaturzertifikat** auf das **Symbol** „Kopieren“, um die  **Verbundmetadaten-URL der App**  zu kopieren, und fügen Sie die URL dann in den Editor ein.

    ![Downloadlink für das Zertifikat](./media/infinitecampus-tutorial/tutorial_infinitecampus_certificate.png) 

7. Verwenden Sie im Abschnitt **Infinite Campus einrichten** die folgenden Werte zur Validierung, wenn Sie die Azure-Metadatendatei/-URL hochladen oder verwenden.

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

    ![Infinite Campus-Konfiguration](common/configuresection.png)

8. Melden Sie sich in einem anderen Webbrowserfenster als Sicherheitsadministrator bei Infinite Campus an.

9. Klicken Sie links im Menü auf **System Administration** (Systemverwaltung).

    ![Administrator](./media/infinitecampus-tutorial/tutorial_infinitecampus_admin.png)

10. Navigieren Sie zu **User Security** > **SAML Management** > **SSO Service Provider Configuration** (Benutzersicherheit -> SAML-Verwaltung -> SSO-Dienstanbieterkonfiguration).

    ![SAML](./media/infinitecampus-tutorial/tutorial_infinitecampus_saml.png)

11. Führen Sie auf der Seite **SSO Service Provider Configuration** (SSO-Dienstanbieterkonfiguration) die folgenden Schritte aus:

    ![SSO](./media/infinitecampus-tutorial/tutorial_infinitecampus_sso.png)

    a. Wählen Sie **Enable SAML Single Sign On** (Einmaliges Anmelden per SAML aktivieren) aus.
    
    b. Wählen Sie im Bereich **Select an option to retrieve Identity Provider (IDP) server data** (Option zum Abrufen von IDP-Dienstanbieterdaten auswählen) die Option **Metadata URL** (Metadaten-URL), fügen Sie die **Verbundmetadaten-URL der App** in das Feld ein, und klicken Sie auf **Sync** (Synchronisieren).

    c. Klicken Sie auf den Link **Service Provider Metadata** (Metadaten des Dienstanbieters), um die Datei mit den **Metadaten des Dienstanbieters** auf Ihrem Computer zu speichern. Laden Sie die Datei im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** hoch, damit die Werte **Bezeichner** und **Antwort-URL** automatisch eingefügt werden (Schritt 4: Upload und automatisches Einfügen von Werten, Schritt 5: Manuelle Eingabe).

    d. Nach dem Klicken auf **Sync** (Synchronisieren) werden die Werte auf der Seite **SSO Service Provider Configuration** (SSO-Dienstanbieterkonfiguration) automatisch aufgefüllt.

    e. Klicken Sie auf **Speichern**.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines _einzelnen_ Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Azure AD-Benutzer erstellen][100]

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Erstellen eines Azure AD-Testbenutzers](common/create_aaduser_01.png) 

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Erstellen eines Azure AD-Testbenutzers](common/create_aaduser_02.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** den Namen **brittasimon@yourcompanydomain.extension** ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich dann den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

### <a name="creating-a-infinite-campus-test-user"></a>Erstellen eines Infinite Campus-Testbenutzers

Infinite Campus besitzt eine demografiezentrierte Architektur. Wenden Sie sich an das [Supportteam von Infinite Campus](mailto:sales@infinitecampus.com), um die Benutzer zur Infinite Campus-Plattform hinzuzufügen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Infinite Campus gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Infinite Campus** aus.

    ![Configure single sign-on](./media/infinitecampus-tutorial/tutorial_infinitecampus_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Infinite Campus“ klicken, sollten Sie automatisch bei Ihrer Infinite Campus-Anwendung angemeldet werden. Wenn Sie sich bei der Infinite Campus-Anwendung im selben Browser anmelden, in dem Sie auch Azure AD verwalten, stellen Sie sicher, dass Sie in Azure AD als der Testbenutzer angemeldet sind. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
