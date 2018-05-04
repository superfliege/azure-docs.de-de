---
title: Gewähren des Zugriffs auf lokale Anwendungen für B2B-Benutzer in Azure AD | Microsoft-Dokumentation
description: Beschreibt, wie Sie B2B-Cloudbenutzern mit Azure AD B2B-Zusammenarbeit Zugriff auf lokale Apps gewähren.
services: active-directory
documentationcenter: ''
author: twooley
manager: mtillman
editor: ''
tags: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.date: 04/20/2018
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 0eb567c8587b0eec367160facc86f163ef6b3c15
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications"></a>Gewähren des Zugriffs auf lokale Anwendungen für B2B-Benutzer in Azure AD

Wenn Ihr Unternehmen Funktionen der Azure Active Directory (Azure AD) B2B-Zusammenarbeit verwendet, um Gastbenutzer aus Partnerunternehmen zu Ihrem Azure AD-Verzeichnis einzuladen, können Sie für diese B2B-Benutzer jetzt Zugriff auf lokale Apps bereitstellen. Diese lokalen Apps können die SAML-basierte Authentifizierung oder die integrierte Windows-Authentifizierung (IWA) mit eingeschränkter Kerberos-Delegierung (Kerberos Constrained Delegation, KCD) verwenden.

## <a name="access-to-saml-apps"></a>Zugriff auf SAML-Apps

Wenn Ihre lokalen Apps die SAML-basierte Authentifizierung verwenden, können Sie diese Apps ganz einfach über das Azure-Portal für Ihre Benutzer in Azure AD B2B-Zusammenarbeit verfügbar machen.

Dazu müssen Sie die folgenden zwei Schritte ausführen:

- Integrieren Sie die SAML-App wie unter [Konfigurieren des einmaligen Anmeldens für Anwendungen, die nicht im Azure Active Directory-Anwendungskatalog enthalten sind](active-directory-saas-custom-apps.md) beschrieben mithilfe der Vorlage „Nicht-Kataloganwendung“. Notieren Sie unbedingt den Wert, den Sie für **Anmelde-URL** angeben.
-  Verwenden Sie den Azure AD-Anwendungsproxy, um die lokale App mit **Azure Active Directory** als Authentifizierungsquelle zu veröffentlichen. Anweisungen dazu finden Sie unter [Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy](application-proxy-publish-azure-portal.md). 

   Verwenden Sie beim Konfigurieren der Einstellung **Interne URL** die Anmelde-URL, die Sie in der Vorlage „Nicht-Kataloganwendung“ angegeben haben. Auf diese Weise können Benutzer außerhalb der Unternehmensgrenzen auf die App zugreifen. Der Anwendungsproxy führt das SAML-basierte einmalige Anmelden für die lokale App aus.
 
   ![Zeigt die Einstellungen der lokalen App für die interne URL und Authentifizierung](media/active-directory-b2b-hybrid-cloud-to-on-premises/OnPremAppSettings.PNG)

## <a name="access-to-iwa-and-kcd-apps"></a>Zugriff auf IWA- und KCD-Apps

Um B2B-Benutzern den Zugriff auf lokale Anwendungen zu ermöglichen, die durch die integrierte Windows-Authentifizierung und eingeschränkte Kerberos-Delegierung geschützt sind, benötigen Sie die folgenden Komponenten:

- **Authentifizierung über den Azure AD-Anwendungsproxy**. B2B-Benutzer müssen sich bei der lokalen Anwendung authentifizieren können. Zu diesem Zweck müssen Sie die lokale App über den Azure AD-Anwendungsproxy veröffentlichen. Weitere Informationen finden Sie unter [Erste Schritte mit dem Anwendungsproxy und Installieren des Connectors](active-directory-application-proxy-enable.md) und [Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy](application-proxy-publish-azure-portal.md).
- **Autorisierung über ein B2B-Benutzerobjekt im lokalen Verzeichnis**. Die Anwendung muss Zugriffsüberprüfungen für Benutzer ausführen können und ihnen Zugriff auf die richtigen Ressourcen gewähren. IWA und KCD erfordern für diese Autorisierung ein Benutzerobjekt im lokalen Windows Server Active Directory-Verzeichnis. Der Anwendungsproxy benötigt dieses Benutzerobjekt, um die Identität des Benutzers anzunehmen und ein Kerberos-Token für die App abzurufen (siehe [So funktioniert das einmalige Anmelden mit KCD](active-directory-application-proxy-sso-using-kcd.md#how-single-sign-on-with-kcd-works)). 

   Für das B2B-Benutzerszenario sind zwei Methoden verfügbar, die Sie zum Erstellen der für die Autorisierung erforderlichen Gastbenutzerobjekte im lokalen Verzeichnis verwenden können:

   - [Microsoft Identity Manager (MIM) und MIM-Verwaltungs-Agent für Microsoft Graph](#create-b2b-guest-user-objects-through-mim-preview). Diese Lösung erfordert, dass Sie ein Azure AD Premium P1-Abonnement besitzen. 
   - [Ein PowerShell-Skript](#create-b2b-guest-user-objects-through-a-script-preview). Die Verwendung des Skripts ist die einfachere Lösung und erfordert weder MIM noch Azure AD Premium. 

Das folgende Diagramm bietet einen allgemeinen Überblick darüber, wie der Azure AD-Anwendungsproxy und die Generierung des B2B-Benutzerobjekts im lokalen Verzeichnis zusammenarbeiten, um B2B-Benutzern Zugriff auf Ihre lokalen IWA- und KCD-Apps zu gewähren. Die nummerierten Schritte werden unter dem Diagramm detailliert beschrieben.

![Diagramm von Lösungen mit MIM und B2B-Skript](media/active-directory-b2b-hybrid-cloud-to-on-premises/MIMScriptSolution.PNG)

1.  Ein Benutzer aus einem Partnerunternehmen (der Mandant „Fabrikam“) wird zum Mandanten „Contoso“ eingeladen.
2.  Ein Gastbenutzerobjekt wird im Mandanten „Contoso“ erstellt (beispielsweise ein Benutzerobjekt mit dem UPN „guest_fabrikam.com#EXT#“@contoso.onmicrosoft.com).
3.  Der Gast „Fabrikam“ wird über MIM oder das B2B-PowerShell-Skript aus Contoso importiert.
4.  Eine Darstellung bzw. ein „Profil“ des Gastbenutzerobjekts „Fabrikam“ (Guest#EXT#) wird im lokalen Verzeichnis (contoso.com) über MIM oder das B2B-PowerShell-Skript erstellt.
5.  Der Gastbenutzer greift auf die lokale Anwendung „app.contoso.com“ zu.
6.  Die Authentifizierungsanforderung wird mithilfe der eingeschränkten Kerberos-Delegierung über den Anwendungsproxy autorisiert. 
7.  Da das Gastbenutzerobjekt lokal vorhanden ist, ist die Authentifizierung erfolgreich.

### <a name="lifecycle-management-policies"></a>Richtlinien für die Lebenszyklusverwaltung

Sie können die lokalen B2B-Benutzerobjekte mithilfe von Richtlinien für die Lebenszyklusverwaltung verwalten. Beispiel: 

- Sie können Richtlinien für die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) für den Gastbenutzer einrichten, damit während der Anwendungsproxy-Authentifizierung MFA verwendet wird. Weitere Informationen finden Sie unter [Bedingter Zugriff für Benutzer der B2B-Zusammenarbeit](active-directory-b2b-mfa-instructions.md).
- Alle Sponsorships, Zugriffsüberprüfungen, Kontoüberprüfungen usw., die für die B2B-Cloudbenutzer ausgeführt werden, gelten für die lokalen Benutzer. Wenn der Cloudbenutzer beispielsweise durch Ihre Richtlinien für die Lebenszyklusverwaltung gelöscht wird, wird der lokale Benutzer ebenfalls durch die MIM-Synchronisierung oder Azure AD Connect-Synchronisierung gelöscht. Weitere Informationen finden Sie unter [Verwalten des Gastzugriffs mit Azure AD-Zugriffsüberprüfungen](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md).

### <a name="create-b2b-guest-user-objects-through-mim-preview"></a>Erstellen von B2B-Gastbenutzerobjekten über MIM (Vorschau)

Informationen zur Verwendung von MIM 2016 Service Pack 1 und des MIM-Verwaltungs-Agents für Microsoft Graph zum Erstellen von Gastbenutzerobjekten im lokalen Verzeichnis finden Sie unter [Azure AD business-to-business (B2B) collaboration with Microsoft Identity Manager (MIM) 2016 SP1 with Azure Application Proxy (Public Preview)](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-graph-b2b-scenario) (Azure AD B2B-Zusammenarbeit mit Microsoft Identity Manager (MIM) 2016 SP1 mit dem Azure-Anwendungsproxy, Public Preview).

### <a name="create-b2b-guest-user-objects-through-a-script-preview"></a>Erstellen von B2B-Gastbenutzerobjekten über ein Skript (Vorschau)

Es ist ein PowerShell-Beispielskript verfügbar, das Sie als Ausgangspunkt für die Erstellung von Gastbenutzerobjekten in Ihrem lokalen Active Directory-Verzeichnis verwenden können.

Sie können das Skript und die Infodatei aus dem [Download Center](https://www.microsoft.com/download/details.aspx?id=51495) herunterladen. Wählen Sie die Datei **Script and Readme to pull Azure AD B2B users on-prem.zip** aus.

Lesen Sie vor der Verwendung des Skripts unbedingt die Voraussetzungen und wichtigen Hinweise in der zugehörigen Infodatei. Beachten Sie außerdem, dass das Skript nur als Beispiel zur Verfügung gestellt wird. Ihr Entwicklerteam oder ein Partner muss das Skript vor der Ausführung anpassen und überprüfen.

## <a name="license-considerations"></a>Hinweise zur Lizenz

Stellen Sie sicher, dass Sie über die richtigen Clientzugriffslizenzen (Client Access License, CAL) für externe Gastbenutzer verfügen, die auf lokale Apps zugreifen. Weitere Informationen finden Sie im Abschnitt zu externen Connector-Lizenzen unter [Client Access-Lizenzen und Management-Lizenzen](https://www.microsoft.com/en-us/licensing/product-licensing/client-access-license.aspx). Wenden Sie sich bezüglich Ihrer spezifischen Lizenzierungsanforderungen an den für Sie zuständigen Microsoft-Kundendienstmitarbeiter oder Ihren Handelspartner vor Ort.

## <a name="next-steps"></a>Nächste Schritte

- [Azure Active Directory B2B-Zusammenarbeit für Hybridorganisationen](active-directory-b2b-hybrid-organizations.md)
- [Gewähren des Zugriffs auf Cloudressourcen für lokal verwaltete Partnerkonten mit Azure AD B2B-Zusammenarbeit](active-directory-b2b-hybrid-on-premises-to-cloud.md)
- Eine Übersicht zu Azure AD Connect finden Sie unter [Integrieren Ihrer lokalen Verzeichnisse in Azure Active Directory](connect/active-directory-aadconnect.md).

