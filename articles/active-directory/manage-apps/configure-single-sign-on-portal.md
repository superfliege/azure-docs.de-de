---
title: Konfigurieren des einmaligen Anmeldens – Azure Active Directory | Microsoft-Dokumentation
description: In diesem Tutorial wird das Azure-Portal verwendet, um SAML-basiertes einmaliges Anmelden (Single Sign-On, SSO) für eine Anwendung mit Azure Active Directory (Azure AD) zu konfigurieren.
services: active-directory
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.topic: tutorial
ms.workload: identity
ms.date: 12/06/2018
ms.author: barbkess
ms.reviewer: arvinh,luleon
ms.openlocfilehash: aceacdea8b3c86a5c4f26a5f082f4c6cf0b3805d
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53011976"
---
# <a name="tutorial-configure-saml-based-single-sign-on-for-an-application-with-azure-active-directory"></a>Tutorial: Konfigurieren des SAML-basierten einmaligen Anmeldens für eine Anwendung mit Azure Active Directory

In diesem Tutorial wird das [Azure-Portal](https://portal.azure.com) verwendet, um SAML-basiertes einmaliges Anmelden für eine Anwendung mit Azure Active Directory (Azure AD) zu konfigurieren. Verwenden Sie dieses Tutorial, wenn kein [anwendungsspezifisches Tutorial](../saas-apps/tutorial-list.md) verfügbar ist. 

In diesem Tutorial verwenden Sie das Azure-Portal zu folgenden Zwecken:

> [!div class="checklist"]
> * Auswählen des Modus für SAML-basiertes einmaliges Anmelden
> * Konfigurieren von anwendungsspezifischen Domänen und URLs
> * Konfigurieren von Benutzerattributen
> * Erstellen eines SAML-Signaturzertifikats
> * Zuweisen von Benutzern zur Anwendung
> * Konfigurieren der Anwendung für SAML-basiertes einmaliges Anmelden
> * Testen der SAML-Einstellungen

## <a name="before-you-begin"></a>Voraussetzungen

1. Wenn die Anwendung Ihrem Azure AD-Mandanten nicht hinzugefügt wurde, finden Sie unter [Schnellstart: Hinzufügen einer Anwendung zum Azure AD-Mandanten](add-application-portal.md) weitere Informationen.

2. Bitten Sie den Anwendungshersteller um die unter [Konfigurieren von Domäne und URLs](#configure-domain-and-urls) beschriebenen Informationen.

3. Es wird empfohlen, zum Testen der Schritte in diesem Tutorial keine Produktionsumgebung zu verwenden. Wenn Sie keine solche Umgebung besitzen, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

4. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Cloudanwendungsadministrator oder Anwendungsadministrator für Ihren Azure AD-Mandanten an.

## <a name="select-a-single-sign-on-mode"></a>Auswählen eines Modus für einmaliges Anmelden

Nach dem Hinzufügen einer Anwendung zum Azure AD-Mandanten können Sie einmaliges Anmelden für die Anwendung konfigurieren.

So öffnen Sie die Einstellungen für einmaliges Anmelden:

1. Klicken Sie im linken Navigationsbereich des [Azure-Portals](https://portal.azure.com) auf **Azure Active Directory**. 

2. Klicken Sie auf dem Blatt **Azure Active Directory** auf **Unternehmensanwendungen**. Das Blatt **Alle Anwendungen** wird mit einer nach dem Zufallsprinzip ausgewählten Gruppe von Anwendungen in Ihrem Azure AD-Mandanten geöffnet. 

3. Wählen Sie im Menü **Anwendungstyp** die Option **Alle Anwendungen** aus, und klicken Sie auf **Anwenden**.

4. Geben Sie den Namen der Anwendung ein, für die Sie einmaliges Anmelden konfigurieren möchten. Wählen Sie Ihre eigene Anwendung aus, oder geben Sie **GitHub-test** ein, um die im Schnellstart zum [Hinzufügen einer Anwendung](add-application-portal.md) hinzugefügte Anwendung zu konfigurieren.

5. Klicken Sie auf **Einmaliges Anmelden**. Unter **SSO-Modus** wird als Standardoption **SAML-basierte Anmeldung** angezeigt. 

    ![Konfigurationsoptionen](media/configure-single-sign-on-portal/config-options.png)

6. Klicken Sie oben auf dem Blatt auf **Speichern** . 

## <a name="configure-domain-and-urls"></a>Konfigurieren von Domäne und URLs

So konfigurieren Sie die Domäne und URLs:

1. Wenden Sie sich an den Anwendungshersteller, um die richtigen Informationen für die folgenden Einstellungen zu erhalten:

    | Konfigurationseinstellung | Vom Dienstanbieter initiiert | Vom Identitätsanbieter initiiert | BESCHREIBUNG |
    |:--|:--|:--|:--|
    | Anmelde-URL | Erforderlich | Nicht angeben | Wenn ein Benutzer diese URL öffnet, wird er vom Dienstanbieter zur Authentifizierung und Anmeldung an Azure AD umgeleitet. Azure AD verwendet die URL, um die Anwendung über Office 365 oder den Azure AD-Zugriffsbereich zu starten. Wird keine URL angegeben, verwendet Azure AD den Identitätsanbieter, um einmaliges Anmelden zu initiieren, wenn ein Benutzer die Anwendung startet.|
    | Bezeichner (Entitäts-ID) | Für einige Apps erforderlich | Für einige Apps erforderlich | Er identifiziert eindeutig die Anwendung, für die einmaliges Anmelden konfiguriert wird. Azure AD sendet den Bezeichner als Audience-Parameter des SAML-Tokens an die Anwendung. Von der Anwendung wird erwartet, dass sie diesen Parameter überprüft. Dieser Wert ist auch als Entitäts-ID in SAML-Metadaten enthalten, die von der Anwendung bereitgestellt werden.|
    | Antwort-URL | Optional | Erforderlich | Gibt an, ob die Anwendung den Empfang des SAML-Tokens erwartet. Die Antwort-URL wird auch als „Assertionsverbraucherdienst-URL“ (Assertion Consumer Service, ACS) bezeichnet. |
    | Relayzustand | Optional | Optional | Mit dieser Option wird die Anwendung darüber informiert, wohin der Benutzer nach der Authentifizierung umgeleitet werden soll. Der Wert ist in der Regel eine gültige URL für die Anwendung. Einige Anwendungen nutzen dieses Feld jedoch anders. Weitere Informationen erhalten Sie vom Anwendungshersteller.

2. Geben Sie die Informationen ein. Klicken Sie zum Anzeigen aller Einstellungen auf **Erweiterte URL-Einstellungen anzeigen**.

    ![Konfigurationsoptionen](media/configure-single-sign-on-portal/config-urls.png)

3. Klicken Sie oben auf dem Blatt auf **Speichern**.

4. Dieser Abschnitt enthält eine Schaltfläche **SAML-Einstellungen testen**. Führen Sie diesen Test später im Tutorial im Abschnitt [Testen des einmaligen Anmeldens](#test-single-sign-on) aus.

## <a name="configure-user-attributes"></a>Konfigurieren von Benutzerattributen

Mithilfe von Benutzerattributen können Sie steuern, welche Informationen Azure AD bei jeder Anmeldung eines Benutzers im SAML-Token an die Anwendung sendet. Azure AD kann beispielsweise den Namen, die E-Mail-Adresse und die Mitarbeiter-ID des Benutzers an die Anwendung senden. 

Diese Attribute können erforderlich oder optional sein, damit einmaliges Anmelden ordnungsgemäß funktioniert. Weitere Informationen finden Sie im [anwendungsspezifischen Tutorial](../saas-apps/tutorial-list.md), oder wenden Sie sich an den Anwendungshersteller, um diese Informationen zu erhalten.

1. Aktivieren Sie zum Anzeigen aller Optionen die Option **Alle weiteren Benutzerattribute anzeigen und bearbeiten**.

    ![Konfigurieren von Benutzerattributen](media/configure-single-sign-on-portal/config-user-attributes.png)

2. Geben Sie ins Feld **Benutzer-ID** einen Wert ein.

    Mit der Benutzer-ID wird jeder Benutzer in der Anwendung eindeutig identifiziert. Beispiel: Ist die E-Mail-Adresse sowohl der Benutzername als auch der eindeutige Bezeichner, legen Sie den Wert auf *user.mail* fest.

3. Aktivieren Sie zum Anzeigen weiterer SAML-Tokenattribute die Option **Alle weiteren Benutzerattribute anzeigen und bearbeiten**.

4. Wenn Sie ein Attribut zu **SAML-Tokenattribute** hinzufügen möchten, klicken Sie auf **Attribut hinzufügen**. Geben Sie unter **Name** einen Namen ein, und wählen Sie im Menü den **Wert** aus.

5. Klicken Sie auf **Speichern**. Das neue Attribut wird in der Tabelle angezeigt.
 
## <a name="create-a-saml-signing-certificate"></a>Erstellen eines SAML-Signaturzertifikats

Azure AD verwendet ein Zertifikat zum Signieren der SAML-Token, die an die Anwendung gesendet werden. 

1. Aktivieren Sie zum Anzeigen aller Optionen das Kontrollkästchen **Erweiterte Einstellungen für die Zertifikatsignatur**.

    ![Konfigurieren von Zertifikaten](media/configure-single-sign-on-portal/config-certificate.png)

2. Klicken Sie zum Konfigurieren eines Zertifikats auf **Neues Zertifikat erstellen**.

3. Legen Sie auf dem Blatt **Neues Zertifikat erstellen** das **Ablaufdatum** fest, und klicken Sie auf **Speichern**.

4. Aktivieren Sie das Kontrollkästchen **Neues Zertifikat aktivieren**.

5. Weitere Informationen finden Sie unter [Erweiterte Optionen für die Zertifikatsignatur im SAML-Token für Katalog-Apps in Azure Active Directory](certificate-signing-options.md).

6. Wenn Sie die bisher vorgenommenen Änderungen beibehalten möchten, klicken Sie oben auf dem Blatt **Einmaliges Anmelden** auf **Speichern**. 

## <a name="assign-users-to-the-application"></a>Zuweisen von Benutzern zur Anwendung

Microsoft empfiehlt, das einmalige Anmelden für mehrere Benutzer oder Gruppen zu testen, bevor Sie die Anwendung in Ihrer Organisation einführen.

So weisen Sie einen Benutzer oder eine Gruppe zur Anwendung zu:

1. Öffnen Sie die Anwendung im Portal, sofern sie noch nicht geöffnet ist.
2. Klicken Sie auf dem linken Anwendungsblatt auf **Benutzer und Gruppen**.
3. Klicken Sie auf **Benutzer hinzufügen**.
4. Klicken Sie auf dem Blatt **Zuweisung hinzufügen** auf **Benutzer und Gruppen**.
5. Wenn Sie einen bestimmten Benutzer suchen möchten, geben Sie den Benutzernamen ins Feld **Auswählen** ein. Aktivieren Sie dann das Kontrollkästchen neben dem Profilbild oder -logo des Benutzers, und klicken Sie auf **Auswählen**. 
6. Suchen Sie den aktuellen Benutzernamen, und wählen Sie ihn aus. Sie können optional weitere Benutzer auswählen.
7. Klicken Sie auf dem Blatt **Zuweisung hinzufügen** auf **Zuweisen**. Anschließend wird der ausgewählte Benutzer in der Liste **Benutzer und Gruppen** angezeigt.

## <a name="configure-the-application-to-use-azure-ad"></a>Konfigurieren der Anwendung für die Verwendung von Azure AD

Sie sind fast fertig.  Im letzten Schritt müssen Sie die Anwendung so konfigurieren, dass Sie Azure AD als SAML-Identitätsanbieter verwendet. 

1. Scrollen Sie ans Ende des Blatts **Einmaliges Anmelden** für Ihre Anwendung. 

    ![Konfigurieren der Anwendung](media/configure-single-sign-on-portal/configure-app.png)

2. Klicken Sie im Portal auf **Anwendung konfigurieren**, und befolgen Sie die Anweisungen.
3. Erstellen Sie manuell Benutzerkonten in der Anwendung, um einmaliges Anmelden zu testen. Erstellen Sie die Benutzerkonten, die Sie der Anwendung im [vorherigen Abschnitt](#assign-users-to-the-application) zugewiesen haben. 

## <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

Sie können Ihre Einstellungen jetzt testen.  

1. Öffnen Sie die SSO-Einstellungen für Ihre Anwendung. 
2. Scrollen Sie zum Abschnitt **Konfigurieren von Domäne und URLs**.
2. Klicken Sie auf **SAML-Einstellungen testen**. Die Testoptionen werden angezeigt.

    ![Testen des einmaligen Anmeldens](media/configure-single-sign-on-portal/test-single-sign-on.png) 

3. Klicken Sie auf **Als aktueller Benutzer anmelden**. Dadurch können Sie zunächst überprüfen, ob einmaliges Anmelden für Sie als Administrator funktioniert.
4. Wenn ein Fehler vorliegt, wird eine Fehlermeldung angezeigt. Kopieren Sie die Details, und fügen Sie sie ins Feld **Wie äußert sich der Fehler?** ein.

    ![Leitfaden zur Problemlösung abrufen](media/configure-single-sign-on-portal/error-guidance.png)

5. Klicken Sie auf **Leitfaden zur Problemlösung abrufen**. Die Grundursache und Informationen zur Problemlösung werden angezeigt.  In diesem Beispiel wurde der Benutzer nicht der Anwendung zugewiesen.

    ![Beheben des Fehlers](media/configure-single-sign-on-portal/fix-error.png)

6. Lesen Sie den Leitfaden zur Problemlösung, und klicken Sie dann ggf. auf **Korrigieren**.

7. Führen Sie den Test erneut aus, bis der Vorgang erfolgreich abgeschlossen wird.



## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie die Einstellungen für einmaliges Anmelden für eine Anwendung konfiguriert. Nach Abschluss der Konfiguration haben Sie der Anwendung einen Benutzer zugewiesen und die Anwendung zur Verwendung des SAML-basierten einmaligen Anmeldens konfiguriert. Nach Ausführung dieser Aufgaben haben Sie überprüft, ob SAML-basiertes einmaliges Anmelden ordnungsgemäß funktioniert.

Sie haben folgende Schritte ausgeführt:
> [!div class="checklist"]
> * Sie haben SAML als SSO-Modus ausgewählt.
> * Sie haben den Anwendungshersteller kontaktiert, um die Domäne und die URLs zu konfigurieren.
> * Sie haben Benutzerattribute konfiguriert.
> * Sie haben ein SAML-Signaturzertifikat erstellt.
> * Sie haben der Anwendung manuell Benutzer oder Gruppen zugewiesen.
> * Sie haben die Anwendung zur Verwendung von Azure AD als SAML-Identitätsanbieter konfiguriert.
> * Sie haben das SAML-basierte einmalige Anmelden getestet.

Es wird empfohlen, die automatische Benutzerbereitstellung zu verwenden, wenn Sie die Anwendung für weitere Benutzer in der Organisation einführen möchten.

> [!div class="nextstepaction"]
>[Erfahren Sie, wie Sie Benutzer mithilfe der automatischen Bereitstellung zuweisen.](configure-automatic-user-provisioning-portal.md)


