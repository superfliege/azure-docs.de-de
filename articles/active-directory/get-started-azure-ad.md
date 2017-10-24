---
title: Erste Schritte mit Azure Active Directory | Microsoft-Dokumentation
description: 
keywords: 
author: jeffgilb
manager: femila
ms.author: jeffgilb
ms.reviewer: jsnow
ms.date: 10/04/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
service: active-directory
custom: it-pro
ms.openlocfilehash: 2e9d9955810c01eb916df2580f623373aebb9d1d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-ad"></a>Erste Schritte mit Azure AD
Eine moderne Identitätsverwaltung muss skalierbar, konsistent und zuverlässig sein, um zu gewährleisten, dass Anwendungen und Dienste nur authentifizierten Benutzern zur Verfügung stehen. Zur angemessenen Unterstützung der Identitätsverwaltungsanforderungen von Benutzern muss die IT in der Lage sein, den Zugriff auf genehmigte, öffentliche SaaS-Apps (Software-as-a-Service) zu ermöglichen, interne Branchen-Apps zu hosten und die lokale App-Entwicklung und -Verwendung zu verbessern. Diese Anforderungen lassen sich alle mit einer cloudbasierten Identitätsverwaltungslösung erfüllen.      

Azure Active Directory (Azure AD) ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft. Azure AD kombiniert grundlegende Verwaltungsdienste mit Identitätsgovernance und Zugriffsverwaltung für Anwendungen. Dank der mehrinstanzenfähigen, geografisch verteilten Azure AD-Struktur mit hoher Verfügbarkeit können Sie sich bei Ihren wichtigsten Geschäftsanforderungen auf Azure AD verlassen.

Azure AD enthält eine vollständige Suite mit Funktionen zur Identitätsverwaltung. Hierzu zählen unter anderem die Synchronisierung von Informationen zu lokalen Ressourcen, anpassbares Unternehmensbranding, eine unkomplizierte Lizenzverwaltung und sogar eine Self-Service-Kennwortverwaltung.  Mit diesen einfach einzurichtenden Funktionen können Sie Ihre ersten Schritte mit Azure AD unternehmen und cloudbasierte Anwendungen schützen, IT-Prozesse optimieren, Kosten senken und die Erfüllung unternehmensweiter Kompatibilitätsziele sicherstellen.

![Azure AD ](./media/get-started-azure-ad/Azure_Active_Directory.png)

Im weiteren Verlauf des Artikels finden Sie Informationen zu den ersten Schritten mit Azure AD. 

## <a name="sign-up-for-azure-active-directory-premium"></a>Registrieren für Azure Active Directory Premium
Für Ihre [ersten Schritte mit Azure Active Directory (Azure AD) Premium](active-directory-get-started-premium.md) können Sie Lizenzen erwerben und Ihrem Azure-Abonnement zuordnen. Wenn Sie ein neues Azure-Abonnement erstellen, müssen Sie außerdem Ihren Lizenzplan und den Azure AD-Dienstzugriff aktivieren. 

Es gibt mehrere Möglichkeiten, sich für Active Directory Premium zu registrieren: 

- Ihr Azure- oder Office 365-Abonnement
- Enterprise Mobility + Security-Lizenzplan
- Microsoft-Volumenlizenzierungsplan

> ### <a name="verification-step"></a>Überprüfungsschritt
> Vergewissern Sie sich nach der Aktivierung des Abonnements, dass Sie sich bei dem Dienst anmelden können.

## <a name="add-a-custom-domain-name"></a>Hinzufügen eines benutzerdefinierten Domänennamens
Jedes Azure AD-Verzeichnis enthält einen anfänglichen Domänennamen in der Form *domänenname*.onmicrosoft.com. Der Name der Anfangsdomäne kann nicht geändert oder gelöscht werden. Sie können jedoch auch [Azure AD den Namen Ihrer Unternehmensdomäne hinzufügen](add-custom-domain.md). Beispielsweise verfügt Ihre Organisation wahrscheinlich über andere Domänennamen, die sie für ihre Geschäftstätigkeit verwendet, sowie über Benutzer, die sich mit dem Domänennamen des Unternehmens anmelden. Indem Sie Azure AD benutzerdefinierte Domänennamen hinzufügen, können Sie Benutzernamen im Verzeichnis zuweisen, die Ihren Benutzern vertraut sind, z.B. „alice@contoso.com“ anstelle von „alice@.onmicrosoft.com“. Der Prozess ist ganz einfach:

1. Fügen Sie Ihrem Verzeichnis den benutzerdefinierten Domänennamen hinzu.
2. Fügen Sie bei der Domänennamen-Registrierungsstelle einen DNS-Eintrag für den Domänennamen hinzu.
3. Überprüfen Sie den benutzerdefinierten Domänennamen in Azure AD.

> ### <a name="verification-step"></a>Überprüfungsschritt
> Vergewissern Sie sich nach dem Hinzufügen einer benutzerdefinierten Domäne, dass im Azure AD-Portal auf dem Blatt **Domänennamen** der Status **Bestätigt** angezeigt wird.

## <a name="add-company-branding-to-your-sign-in-page"></a>Hinzufügen eines Unternehmensbrandings zu Ihrer Anmeldeseite 
Um Verwirrung zu vermeiden, möchten viele Unternehmen über alle verwalteten Websites und Dienste hinweg für ein einheitliches Erscheinungsbild sorgen. Dies ist in Azure Active Directory (Azure AD) möglich, da Sie die [Darstellung der Anmeldeseite mit Ihrem Firmenlogo und Ihren benutzerdefinierten Farbschemas anpassen](customize-branding.md) können. Die Anmeldeseite wird angezeigt, wenn Sie sich bei Office 365 oder anderen webbasierten Anwendungen anmelden, die Azure AD als Identitätsanbieter verwenden. Sie interagieren mit dieser Seite, um Ihre Anmeldeinformationen einzugeben.

> ### <a name="verification-step"></a>Überprüfungsschritt
> Melden Sie sich beim Azure-Portal an, und vergewissern Sie sich, dass Ihre benutzerdefinierte Anmeldeseite sowie ggf. zusätzliche Sprachanpassungen ordnungsgemäß konfiguriert sind. 

## <a name="add-new-users"></a>Hinzufügen neuer Benutzer
Sie können [dem Azure AD Ihrer Organisation neue Benutzer hinzufügen](add-users-azure-active-directory.md) – entweder einzeln über das Azure-Portal oder mittels Synchronisierung Ihrer lokalen Windows Server AD-Ressourcendaten. Sie können cloudbasierte Benutzer direkt über das Azure AD-Portal hinzufügen oder lokale Benutzerinformationen synchronisieren.

Um die lokale Identitätssynchronisierung mit Azure AD zu aktivieren, müssen Sie [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) auf einem Server Ihrer Organisation installieren und konfigurieren. Mit dieser Anwendung wird die Synchronisierung von Benutzern und Gruppen von Ihrer vorhandenen Identitätsquelle mit Ihrem Azure AD-Mandanten durchgeführt.

> ### <a name="verification-step"></a>Überprüfungsschritt
> Vergewissern Sie sich nach dem Erstellen oder Synchronisieren neuer Benutzer, dass sie in Azure AD angezeigt werden.

## <a name="assign-licenses"></a>Zuweisen von Lizenzen
Zum Konfigurieren kostenpflichtiger Funktionen benötigen Sie zwar lediglich ein Abonnement, für kostenpflichtige Azure AD Premium-Features müssen Sie jedoch weiterhin [Benutzerlizenzen zuweisen](license-users-groups.md). Allen Benutzern, die Zugriff haben sollen oder über kostenpflichtige Azure AD-Features verwaltet werden, muss eine Lizenz zugewiesen werden. Eine Lizenzzuweisung ist eine Zuordnung zwischen einem Benutzer und einem erworbenen Dienst wie Azure AD Premium, Basic oder Enterprise Mobility + Security.

Mit gruppenbasierter Lizenzzuweisung können Sie Regeln wie die Folgenden einrichten:

- Alle Benutzer in Ihrem Verzeichnis erhalten automatisch eine Lizenz.
- Alle Benutzer mit der entsprechenden Position erhalten eine Lizenz.
- Sie können die Entscheidung an andere Manager in der Organisation delegieren (mithilfe von Self-Service-Gruppen).

> ### <a name="verification-step"></a>Überprüfungsschritt
> Prüfen Sie unter **Azure Active Directory** > **Lizenzen** > **Alle Produkte** die zugewiesenen und verfügbaren Lizenzen.

## <a name="configure-self-service-password-reset"></a>Konfigurieren der Self-Service-Kennwortzurücksetzung
Die [Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR)](active-directory-passwords-getting-started.md) ist für IT-Administratoren eine einfache Möglichkeit, Benutzern das Zurücksetzen oder Entsperren ihrer Kennwörter oder Konten zu ermöglichen. Das System verfügt über eine ausführliche Berichterstellung zur Nutzung des Systems durch Benutzer sowie über eine Benachrichtigungsfunktion, damit Sie über eine fehlerhafte oder missbräuchliche Nutzung informiert sind.

> ### <a name="verification-step"></a>Überprüfungsschritt
> Prüfen Sie unter **Azure Active Directory** > **Kennwortzurücksetzung** die aktivierten SSPR-Eigenschaften, um sicherzustellen, dass die Benutzer- und Gruppenzuweisungen korrekt sind. 


### <a name="learn-more"></a>Weitere Informationen
[Azure Active Directory – Produktseite](https://azure.microsoft.com/services/active-directory/)

[Azure Active Directory – Preisinformationen](https://azure.microsoft.com/pricing/details/active-directory/)