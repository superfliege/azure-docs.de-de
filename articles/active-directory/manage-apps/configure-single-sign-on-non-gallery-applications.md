---
title: Einmaliges Anmelden bei Nicht-Kataloganwendungen – Azure Active Directory | Microsoft-Dokumentation
description: Konfigurieren des einmaligen Anmeldens für Nicht-Kataloganwendungen in Azure Active Directory (Azure AD)
services: active-directory
author: barbkess
manager: daveba
ms.service: active-directory
ms.component: app-mgmt
ms.topic: article
ms.workload: identity
ms.date: 01/08/2019
ms.author: barbkess
ms.reviewer: asmalser,luleon
ms.openlocfilehash: c1472a78b0d1fbc287b151d9760fa549ad9b087e
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54474080"
---
# <a name="configure-single-sign-on-to-non-gallery-applications-in-azure-active-directory"></a>Konfigurieren des einmaligen Anmeldens für Nicht-Kataloganwendungen in Azure Active Directory

In diesem Artikel geht es um eine Funktion, die Administratoren das Konfigurieren der einmaligen Anmeldung bei Anwendungen, die im Azure Active Directory-App-Katalog nicht vorhanden sind, *ohne Schreiben von Code* ermöglicht. Wenn Sie stattdessen nach einer Anleitung für Entwickler zum Integrieren von benutzerdefinierten Apps in Azure AD mithilfe von Code suchen, finden Sie diese unter [Authentifizierungsszenarien für Azure AD](../develop/authentication-scenarios.md).

Der Azure Active Directory-Anwendungskatalog enthält eine Liste der Anwendungen, die bekanntermaßen eine Form der einmaligen Anmeldung mit Azure Active Directory unterstützen, wie [in diesem Artikel](what-is-single-sign-on.md)beschrieben. Wenn Sie (als IT-Spezialist oder Systemintegrator in Ihrer Organisation) die Anwendung gefunden haben, mit der Sie eine Verbindung herstellen möchten, können Sie mithilfe der Schritt-für-Schritt-Anleitungen im Azure-Portal das einmalige Anmelden aktivieren.

Diese Funktionen sind gemäß Ihrem Lizenzvertrag ebenfalls verfügbar. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/active-directory/). 

- Self-Service-Integration für Anwendungen, die SAML 2.0-Identitätsanbieter unterstützen (SP-initiiert oder IdP-initiiert)
- Self-Service-Integration für Webanwendungen, die über eine HTML-basierte Anmeldeseite mit [kennwortbasierter einmaliger Anmeldung (Single Sign-On, SSO)](what-is-single-sign-on.md#password-based-sso)
* Self-Service-Verbindung für Anwendungen, die das SCIM-Protokoll für die Benutzerbereitstellung verwenden ([wie hier beschrieben](use-scim-to-provision-users-and-groups.md))
* Möglichkeit zum Hinzufügen von Links zu Anwendungen im [Office 365-App-Startfeld](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) oder im [Azure AD-Zugriffsbereich](what-is-single-sign-on.md#linked-sso)

Diese Möglichkeiten beziehen sich nicht nur SaaS-Anwendungen, die Sie bereits nutzen, die aber noch nicht per Onboarding in den Azure AD-Anwendungskatalog aufgenommen wurden, sondern auch auf Drittanbieter-Webanwendungen, die Ihre Organisation auf von Ihnen kontrollierten Servern in der Cloud oder lokal bereitgestellt hat.

Diese Funktionen, auch als *App-Integrationsvorlagen*bezeichnet, bieten standardbasierte Verbindungspunkte für Apps, die SAML, SCIM oder formularbasierte Authentifizierung unterstützen, und enthalten flexible Optionen und Einstellungen für die Kompatibilität mit einer umfassenden Reihe von Anwendungen. 

## <a name="adding-an-unlisted-application"></a>Hinzufügen einer nicht aufgeführten Anwendung
Um eine Anwendung über eine App-Integrationsvorlage zu verbinden, melden Sie sich mit Ihrem Azure Active Directory-Administratorkonto beim Azure-Portal an. Navigieren Sie zum Abschnitt unter **Active Directory > Unternehmensanwendungen > Nicht-Kataloganwendung**, wählen Sie **Hinzufügen** aus, und klicken Sie dann auf **Anwendung aus dem Katalog hinzufügen**.

  ![Anwendung hinzufügen](./media/configure-single-sign-on-non-gallery-applications/customapp1.png)

Im App-Katalog können Sie eine nicht aufgeführte App hinzufügen, indem Sie auf die Kachel **Nicht-Kataloganwendung** klicken, die in den Suchergebnissen angezeigt wird, wenn die gewünschte App nicht gefunden wurde. Nachdem Sie den Namen für Ihre Anwendung eingegeben haben, können Sie die Optionen für das einmalige Anmelden und das entsprechende Verhalten festlegen. 

**Tipp**:  Es hat sich bewährt, zuerst mit der Suchfunktion zu prüfen, ob die Anwendung bereits im Anwendungskatalog vorhanden ist. Wenn die App gefunden und in deren Beschreibung das einmalige Anmelden erwähnt wird, wird die einmalige Verbundanmeldung für die Anwendung bereits unterstützt.

  ![Suchen,](./media/configure-single-sign-on-non-gallery-applications/customapp2.png)

Das Hinzufügen einer Anwendung mit dieser Methode gestaltet sich ähnlich benutzerfreundlich wie bei vorab integrierten Anwendungen. Wählen Sie im ersten Schritt **Einmaliges Anmelden konfigurieren**, oder klicken Sie im Navigationsmenü auf der linken Seite der Anwendung auf **Einmaliges Anmelden**. In der nächsten Anzeige sehen Sie die Optionen für die Konfiguration des einmaligen Anmeldens. Die Optionen werden in den nächsten Abschnitten dieses Artikels beschrieben.
  
![Konfigurationsoptionen](./media/configure-single-sign-on-non-gallery-applications/customapp3.png)

## <a name="saml-based-single-sign-on"></a>SAML-basiertes einmaliges Anmelden
Wählen Sie diese Option aus, um die SAML-basierte Authentifizierung für die Anwendung zu konfigurieren. Dies setzt voraus, dass die Anwendung SAML 2.0 unterstützt. Sie sollten sich vor dem Ausführen der nächsten Schritte informieren, wie die SAML-Funktionen der Anwendung genutzt werden können. Füllen Sie die folgenden Abschnitte aus, um einmaliges Anmelden zwischen der Anwendung und Azure AD zu konfigurieren.

### <a name="enter-basic-saml-configuration"></a>Eingabe der SAML-Basiskonfiguration

Geben Sie zum Einrichten von Azure AD die SAML-Basiskonfiguration ein. Sie können die Werte manuell eingeben oder eine Metadatendatei hochladen, um die Werte der Felder zu extrahieren.

  ![Litware-Domäne und -URLs](./media/configure-single-sign-on-non-gallery-applications/customapp4.png)

- **Anmelde-URL (nur SP-initiiert):** Hiermit melden sich Benutzer an dieser Anwendung an. Sofern die Anwendung für die Durchführung des vom Dienstanbieter (Service Provider, SP) initiierten einmaligen Anmeldens konfiguriert ist, passiert Folgendes, wenn ein Benutzer zu dieser URL navigiert: Der Dienstanbieter führt die Umleitung an Azure AD durch, um den Benutzer zu authentifizieren und anzumelden. Wenn dieses Feld ausgefüllt wird, verwendet Azure AD diese URL zum Starten der Anwendung aus Office 365 und über den Azure AD-Zugriffsbereich. Falls Sie dieses Feld leer lassen, führt Azure AD stattdessen eine vom Identitätsanbieter initiierte Anmeldung aus, wenn die App über Office 365, den Azure AD-Zugriffsbereich oder die Azure AD-URL für einmaliges Anmelden (kann von der Registerkarte „Dashboard“ kopiert werden) gestartet wird.
- **Bezeichner:** Er muss die Anwendung, für die das einmalige Anmelden konfiguriert wird, eindeutig identifiziern. Sie finden diesen Wert als Zertifikatausstellerelement in AuthRequest (SAML-Anforderung), die von der Anwendung gesendet wird. Dieser Wert ist auch als **Entitäts-ID** in SAML-Metadaten enthalten, die von der Anwendung bereitgestellt werden. Die SAML-Dokumentation der Anwendung enthält Details dazu, wie die Entitäts-ID bzw. der Wert für „Audience“ lautet. 

    Das folgende Beispiel zeigt, wie der Bezeichner oder der Zertifikataussteller in der SAML-Anforderung aussieht, die von der Anwendung an Azure AD gesendet wird:

    ```
    <samlp:AuthnRequest
    xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
    ID="id6c1c178c166d486687be4aaf5e482730"
    Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
    </samlp:AuthnRequest>
    ```

- **Antwort-URL:** Unter der Antwort-URL erwartet die Anwendung den Empfang des SAML-Tokens. Sie wird auch als „Assertionsverbraucherdienst-URL“ (Assertion Consumer Service, ACS) bezeichnet. In der SAML-Dokumentation der Anwendung finden Sie Details dazu, wie die Antwort-URL bzw. ACS-URL des SAML-Tokens lautet. 

    Zur Konfiguration mehrerer replyURLs-Elemente können Sie das folgende PowerShell-Skript verwenden.

    ```PowerShell
    $sp = Get-AzureADServicePrincipal -SearchString "<Exact App  name>"
    $app = Get-AzureADApplication -SearchString "<Exact app name>"
    Set-AzureADApplication -ObjectId $app.ObjectId -ReplyUrls "<ReplyURLs>"
    Set-AzureADServicePrincipal -ObjectId $sp.ObjectId -ReplyUrls "<ReplyURLs>"
    ```

Weitere Informationen finden Sie unter [Von Azure Active Directory (Azure AD) unterstützte SAML 2.0-Authentifizierungsanforderungen und -Antworten](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference?/?WT.mc_id=DOC_AAD_How_to_Debug_SAML).


### <a name="review-or-customize-the-claims-issued-in-the-saml-token"></a>Überprüfen oder Anpassen der im SAML-Token ausgestellten Ansprüche

Wenn sich ein Benutzer bei der Anwendung authentifiziert, stellt Azure AD ein SAML-Token für die App aus, das Informationen über den Benutzer (oder „Ansprüche“) enthält, die ihn eindeutig identifizieren. Dazu gehören normalerweise der Benutzername, die E-Mail-Adresse, der Vorname und der Nachname des Benutzers. 

Sie können die im SAML-Token an die Anwendung übermittelten Ansprüche auf der Registerkarte **Attribute** anzeigen oder bearbeiten.

  ![Attribute](./media/configure-single-sign-on-non-gallery-applications/customapp7.png)

Es gibt zwei Gründe dafür, dass Sie die im SAML-Token ausgestellten Ansprüche ggf. bearbeiten müssen:

- Die Anwendung wurde so geschrieben, dass sie einen anderen Satz an Anspruchs-URIs oder Anspruchswerten erfordert.
- Ihre Anwendung wurde auf eine Weise bereitgestellt, die erfordert, dass es sich beim NameIdentifier-Anspruch nicht um den in Azure Active Directory gespeicherten Benutzernamen (also Benutzerprinzipalnamen) handelt. 

Weitere Informationen finden Sie unter [Anpassen ausgestellter Ansprüche im SAML-Token für Unternehmensanwendungen in Azure Active Directory](./../develop/../develop/active-directory-saml-claims-customization.md). 



### <a name="review-certificate-expiration-data-status-and-email-notification"></a>Überprüfen von Zertifikatablaufdaten, Status und E-Mail-Benachrichtigung

Sobald Sie eine Katalog- oder eine Nicht-Katalog-Anwendung erstellen, erstellt Azure AD ein anwendungsspezifisches Zertifikat mit einem Ablaufdatum von 3 Jahren ab dem Datum der Erstellung. Sie benötigen dieses Zertifikat, um die Vertrauensstellung zwischen Azure AD und der Anwendung einzurichten. Einzelheiten zum Zertifikatsformat finden Sie in der SAML-Dokumentation der Anwendung. 

Von Azure AD können Sie das Zertifikat im Base64- oder Raw-Format herunterladen. Darüber hinaus können Sie das Zertifikat erhalten, indem Sie die XML-Datei mit den Anwendungsmetadaten herunterladen oder die App-Verbundmetadaten-URL verwenden.

  ![Zertifikat](./media/configure-single-sign-on-non-gallery-applications/certificate.png)

Stellen Sie sicher, dass das Zertifikat über Folgendes verfügt:

- Das gewünschte Ablaufdatum. Das von Ihnen konfigurierte Ablaufdatum darf höchstens drei Jahre in der Zukunft liegen.
- Den Status „Aktiv“. Wenn der Status auf „Inaktiv“ festgelegt ist, ändern Sie diesen in „Aktiv“. Aktivieren Sie zum Ändern des Status **Aktiv**, und speichern Sie dann die Konfiguration. 
- Die richtige E-Mail-Adresse für Benachrichtigungen. Wenn sich das aktive Zertifikat dem Ablaufdatum nähert, sendet Azure AD eine Benachrichtigung an die in diesem Feld konfigurierte E-Mail-Adresse.  

Weitere Informationen finden Sie unter [Verwalten von Zertifikaten für die einmalige Verbundanmeldung in Azure Active Directory](manage-certificates-for-federated-single-sign-on.md).

### <a name="set-up-target-application"></a>Einrichten der Zielanwendung

Um die Anwendung für einmaliges Anmelden zu konfigurieren, suchen Sie zunächst die Dokumentation der Anwendung. Scrollen Sie hierzu bis zum Ende der Konfigurationsseite für die SAML-basierte Anmeldung, und klicken Sie dann auf **Konfigurieren<application name>**. 

Die erforderlichen Werte variieren je nach Anwendung. Einzelheiten finden Sie in der SAML-Dokumentation der Anwendung. Die Dienst-URLs für Anmeldung und Abmeldung werden beide auf den gleichen Endpunkt aufgelöst, wobei es sich um den Endpunkt zur Verarbeitung von SAML-Anforderungen für Ihre Azure AD-Instanz handelt. Die SAML-Entitäts-ID ist der Wert, der in dem für die Anwendung ausgestellten SAML-Token als Zertifikataussteller angezeigt wird.


### <a name="assign-users-and-groups-to-your-saml-application"></a>Zuweisen von Benutzern und Gruppen zu Ihrer SAML-Anwendung

Nachdem Sie Ihre Anwendung zur Verwendung von Azure AD als SAML-Identitätsanbieter konfiguriert haben, ist sie fast zum Testen bereit. Als Sicherheitsmaßnahme stellt Azure AD nur dann ein Token aus, das dem Benutzer die Anmeldung bei der Anwendung erlaubt, wenn Azure AD dem Benutzer zuvor den Zugriff gewährt hat. Den Benutzern kann der Zugriff direkt oder über eine Gruppenmitgliedschaft gewährt werden. 

Um der Anwendung einen Benutzer oder eine Gruppe zuzuweisen, klicken Sie auf die Schaltfläche **Benutzer zuweisen** . Wählen Sie den gewünschten Benutzer oder die gewünschte Gruppe aus, und klicken Sie auf die Schaltfläche **Zuweisen** .

  ![Benutzer zuweisen](./media/configure-single-sign-on-non-gallery-applications/customapp6.png)

Durch die Zuweisung eines Benutzers kann Azure AD ein Token für den Benutzer ausstellen. Zudem wird dann im Zugriffsbereich des Benutzers eine Kachel für diese Anwendung angezeigt. Wenn der Benutzer Office 365 verwendet, wird die Anwendungskachel auch im Office 365-App-Startfeld angezeigt. 

> [!NOTE] 
> Sie können ein Kachellogo für die Anwendung hochladen. Verwenden Sie hierzu die Schaltfläche **Logo hochladen** auf der Registerkarte **Konfigurieren** für die Anwendung. 


### <a name="test-the-saml-application"></a>Testen der SAML-Anwendung

Vor dem Testen der SAML-Anwendung müssen Sie die Anwendung mit Azure AD eingerichtet und der Anwendung Benutzer oder Gruppen zugewiesen haben. Informationen zum Testen der SAML-Anwendung finden Sie unter [Debuggen des SAML-basierten einmaligen Anmeldens bei Anwendungen in Azure Active Directory](../develop/howto-v1-debug-saml-sso-issues.md).

## <a name="password-single-sign-on"></a>Einmaliges Anmelden per Kennwort

Wählen Sie diese Option aus, um das [kennwortbasierte einmalige Anmelden](what-is-single-sign-on.md) für eine Webanwendung zu konfigurieren, die über eine HTML-Anmeldeseite verfügt. Das kennwortbasierte einmalige Anmelden (auch als „Password Vaulting“ oder „Kennworttresor“ bezeichnet) ermöglicht es Ihnen, den Benutzerzugriff und die Kennwörter für Webanwendungen zu verwalten, die keinen Identitätsverbund unterstützen. Es ist auch für Szenarien nützlich, in denen mehrere Benutzer ein Konto gemeinsam verwenden müssen, wie z. B. bei den App-Konten für die sozialen Medien Ihrer Organisation. 

Nachdem Sie auf **Weiter**geklickt haben, werden Sie aufgefordert, die URL für die webbasierte Anmeldeseite der Anwendung einzugeben. Beachten Sie, dass es sich hierbei um die Seite handeln muss, welche die Eingabefelder für Benutzername und Kennwort enthält. Nach der Eingabe startet Azure AD einen Prozess, bei dem die Anmeldeseite im Hinblick auf eine Benutzernamenseingabe und eine Kennworteingabe analysiert wird. Wenn dieser Prozess nicht erfolgreich ist, wird Ihnen eine Anleitung für einen alternativen Prozess angezeigt, bei dem Sie eine Browsererweiterung installieren (erfordert Internet Explorer, Chrome oder Firefox) und damit die Felder manuell erfassen.

Nachdem die Anmeldeseite erfasst ist, können Sie wie bei regulären [Apps mit einmaligem Anmelden per Kennwort](what-is-single-sign-on.md)Benutzer und Gruppen zuweisen und Anmeldeinformationsrichtlinien festlegen.

> [!NOTE] 
> Sie können ein Kachellogo für die Anwendung hochladen. Verwenden Sie hierzu die Schaltfläche **Logo hochladen** auf der Registerkarte **Konfigurieren** für die Anwendung. 
>

## <a name="existing-single-sign-on"></a>Vorhandenes einmaliges Anmelden
Wählen Sie diese Option, um im Azure AD-Zugriffsbereich oder im Office 365-Portal einen Link auf eine Anwendung hinzufügen. Sie können mit dieser Option Links zu benutzerdefinierten Web-Apps hinzufügen, die derzeit für die Authentifizierung Active Directory-Verbunddienste (oder andere Verbunddienste) anstelle von Azure AD verwenden. Oder Sie können Deep-Links auf spezifische SharePoint-Seiten oder andere Webseiten hinzufügen, die in den Zugriffsbereichen der betreffenden Benutzer angezeigt werden sollen. 

Nachdem Sie auf **Weiter** geklickt haben, werden Sie aufgefordert, die URL für die Anwendung einzugeben, für die der Link angezeigt werden soll. Wenn Sie dies abgeschlossen haben, können Sie der Anwendung Benutzer und Gruppen zuweisen. Dadurch wird die Anwendung im [Office 365-App-Startfeld](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) oder im [Azure AD-Zugriffsbereich](end-user-experiences.md) für die betreffenden Benutzer angezeigt.

> [!NOTE] 
> Sie können ein Kachellogo für die Anwendung hochladen. Verwenden Sie hierzu die Schaltfläche **Logo hochladen** auf der Registerkarte **Konfigurieren** für die Anwendung. 
>

## <a name="related-articles"></a>Verwandte Artikel
- [Anpassen ausgestellter Ansprüche im SAML-Token für vorintegrierte Apps](../develop/active-directory-saml-claims-customization.md)
- [Problembehandlung bei SAML-basiertem einmaligem Anmelden](../develop/howto-v1-debug-saml-sso-issues.md)

