---
title: "Häufig gestellte Fragen zum bedingten Zugriff mit Azure Active Directory | Microsoft-Dokumentation"
description: "Enthält Antworten auf häufig gestellte Fragen zum bedingten Zugriff in Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 14f7fc83-f4bb-41bf-b6f1-a9bb97717c34
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 5eb16a92feee0937edeff1c00e0babe85176a79f
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2017
---
# <a name="azure-active-directory-conditional-access-faqs"></a>Häufig gestellte Fragen zum bedingten Zugriff mit Azure Active Directory

## <a name="which-applications-work-with-conditional-access-policies"></a>Welche Anwendungen arbeiten mit Richtlinien für den bedingten Zugriff?

Informationen zu Anwendungen, von denen Richtlinien für den bedingten Zugriff verwendet werden, finden Sie unter [Anwendungen und Browser, die Regeln für den bedingten Zugriff in Azure Active Directory verwenden](active-directory-conditional-access-supported-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Werden Richtlinien für den bedingten Zugriff für B2B-Kollaborations- und Gastbenutzer erzwungen?

Richtlinien werden für B2B-Kollaborationsbenutzer (Business-to-Business) erzwungen. In einigen Fällen kann es aber vorkommen, dass ein Benutzer die Richtlinienanforderungen nicht erfüllen kann. Beispielsweise kann es sein, dass die Organisation eines Gastbenutzers die mehrstufige Authentifizierung nicht unterstützt. 



## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>Gelten SharePoint Online-Richtlinien auch für OneDrive for Business?

Ja. Eine SharePoint Online-Richtlinie gilt auch für OneDrive for Business.


## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Warum kann ich keine Richtlinien für Clientanwendungen wie Word oder Outlook festlegen?

Mit einer Richtlinie für bedingten Zugriff werden die Anforderungen für den Zugriff auf einen Dienst festgelegt. Die Richtlinie wird erzwungen, wenn die Authentifizierung für diesen Dienst durchgeführt wird. Die Richtlinie wird nicht direkt für eine Clientanwendung festgelegt. Stattdessen wird sie angewendet, wenn ein Client einen Dienst aufruft. Eine Richtlinie, die für SharePoint festgelegt wurde, gilt beispielsweise für Clients, die SharePoint aufrufen. Eine Richtlinie, die für Exchange festgelegt wurde, gilt für Outlook.

## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Gilt eine Richtlinie für bedingten Zugriff für Dienstkonten?

Richtlinien für bedingten Zugriff gelten für alle Benutzerkonten. Dies schließt auch Benutzerkonten ein, die als Dienstkonten verwendet werden. Häufig kann ein Dienstkonto, das unbeaufsichtigt ausgeführt wird, die Anforderungen einer Richtlinie für bedingten Zugriff nicht erfüllen. Beispielsweise ist unter Umständen die mehrstufige Authentifizierung erforderlich. Dienstkonten können über die Einstellungen zur Verwaltung der Richtlinie für bedingten Zugriff von einer Richtlinie ausgeschlossen werden. 

## <a name="are-graph-apis-available-for-configuring-conditional-access-policies"></a>Sind Graph-APIs zum Konfigurieren von Richtlinien für bedingten Zugriff verfügbar?

Derzeit nicht. 

## <a name="what-is-the-default-exclusion-policy-for-unsupported-device-platforms"></a>Wie lautet die standardmäßige Ausschlussrichtlinie für nicht unterstützte Geräteplattformen?

Derzeit werden Richtlinien für den bedingten Zugriff für Benutzer von iOS- und Android-Geräten selektiv erzwungen. Anwendungen auf anderen Geräteplattformen sind standardmäßig von der Richtlinie für den bedingten Zugriff für iOS- und Android-Geräte ausgenommen. Ein Mandantenadministrator kann die globale Richtlinie außer Kraft setzen, um Benutzern auf nicht unterstützten Plattformen den Zugriff zu verweigern.


## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a>Wie funktionieren Richtlinien für bedingten Zugriff für Microsoft Teams?

Microsoft Teams basiert in Bezug auf wichtige Produktivitätsszenarien, z.B. Besprechungen, Kalender und Dateifreigabe, vor allem auf Exchange Online und SharePoint Online. Richtlinien für bedingten Zugriff, die für diese Cloud-Apps festgelegt werden, gelten für Microsoft Teams, wenn sich ein Benutzer direkt bei Microsoft Teams anmeldet.

Microsoft Teams wird für Richtlinien für bedingten Zugriff in Azure Active Directory auch separat als Cloud-App unterstützt. Richtlinien für bedingten Zugriff, die für eine Cloud-App festgelegt werden, gelten für Microsoft Teams, wenn sich ein Benutzer anmeldet. Allerdings können ohne korrekte Richtlinien für andere Apps wie Exchange Online und SharePoint Online Benutzer möglicherweise weiterhin direkt auf diese Ressourcen zugreifen.

Microsoft Teams-Desktopclients für Windows und Mac unterstützen die moderne Authentifizierung. Die moderne Authentifizierung ermöglicht die ADAL-basierte Anmeldung (Azure Active Directory Authentication Library) plattformübergreifend für Microsoft Office-Clientanwendungen.