---
title: Self-Service- oder virale Registrierung in Azure Active Directory | Microsoft-Dokumentation
description: Verwenden der Self-Service-Registrierung in einem Azure Active Directory-Mandanten (Azure AD)
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: b9f01876-29d1-4ab8-8b74-04d43d532f4b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/03/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: 762627a1d743922d680d90d1065ae80fbd3f4420
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2017
---
# <a name="what-is-self-service-signup-for-azure-active-directory"></a>Was ist die Self-Service-Registrierung für Azure Active Directory?
Dieser Artikel beschreibt die Self-Service-Registrierung und deren Unterstützung in Azure Active Directory (Azure AD). Wenn Sie einen Domänennamen von einem nicht verwalteten Azure AD-Mandanten übernehmen möchten, lesen Sie [Übernehmen eines nicht verwalteten Verzeichnisses als Administrator](domains-admin-takeover.md).

## <a name="why-use-self-service-signup"></a>Gründe für das Verwenden der Self-Service-Registrierung
* Kunden erhalten schneller die gewünschten Dienste.
* Sie können E-Mail-basierte Angebote für einen Dienst erstellen.
* Sie können E-Mail-basierte Registrierungsabläufe erstellen, mit denen Benutzer schnell Identitäten mithilfe ihrer einfach zu merkenden geschäftlichen E-Mail-Aliase erstellen können.
* Ein per Self-Service erstelltes Azure AD-Verzeichnis kann in ein verwaltetes Verzeichnis konvertiert werden, das für andere Dienste verwendet werden kann.

## <a name="terms-and-definitions"></a>Begriffe und Definitionen
* **Self-Service-Registrierung**: Dies ist die Methode, mit der sich ein Benutzer für einen Clouddienst registriert, wobei für ihn basierend auf seiner E-Mail-Domäne automatisch eine Identität in Azure AD erstellt wird.
* **Nicht verwaltetes Azure AD-Verzeichnis**: Dies ist das Verzeichnis, in dem diese Identität erstellt wird. Ein nicht verwaltetes Verzeichnis ist ein Verzeichnis ohne globalen Administrator.
* **Über E-Mail verifizierter Benutzer**: Dies ist ein Typ von Benutzerkonto in Azure AD. Ein Benutzer, für den nach der Registrierung für ein Self-Service-Angebot automatisch eine Identität erstellt wird, wird als über E-Mail verifizierter Benutzer bezeichnet. Ein über E-Mail verifizierter Benutzer ist ein normales Mitglied eines Verzeichnisses mit der Kennzeichnung "creationmethod=EmailVerified".

## <a name="how-do-i-control-self-service-settings"></a>Wie steuere ich Self-Service-Einstellungen?
Administratoren stehen derzeit zwei Self-Service-Steuerungsmöglichkeiten zur Verfügung. Sie können steuern, ob:

* Benutzer dem Verzeichnis per E-Mail beitreten können.
* Benutzer sich selbst für Anwendungen und Dienste lizenzieren können.

### <a name="how-can-i-control-these-capabilities"></a>Wie können diese Funktionen gesteuert werden?
Ein Administrator kann diese Funktionen mit den folgenden Parametern des Azure AD-Cmdlets „Set-MsolCompanySettings“ konfigurieren:

* **AllowEmailVerifiedUsers** steuert, ob ein Benutzer ein nicht verwaltetes Verzeichnis erstellen oder diesem beitreten kann. Wenn Sie diesen Parameter auf „$false“ festlegen, können dem Verzeichnis keine über E-Mail verifizierten Benutzer beitreten.
* **AllowAdHocSubscriptions** steuert, ob Benutzern das Ausführen der Self-Service-Registrierung erlaubt ist. Wenn Sie diesen Parameter auf "$false" festlegen, können Benutzer keine Self-Service-Registrierung ausführen.

### <a name="how-do-the-controls-work-together"></a>Wie funktionieren diese Steuerungsmöglichkeiten zusammen?
Diese beiden Parameter können zusammen verwendet werden, um eine genauere Steuerung der Self-Service-Registrierung zu definieren. Der folgende Befehl erlaubt Benutzern beispielsweise die Self-Service-Registrierung, jedoch nur, wenn die Benutzer bereits über ein Konto in Azure AD verfügen (d.h. dass für Benutzer, für die zuerst ein über E-Mail verifiziertes Konto erstellt werden müsste, die Self-Service-Registrierung nicht erlaubt ist):

````
    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true
````
Im folgenden Flussdiagramm werden die verschiedenen Kombinationen für diese Parameter und die resultierenden Bedingungen für das Verzeichnis und die Self-Service-Registrierung erläutert.

![][1]

Weitere Informationen und Beispiele zum Verwenden dieser Parameter finden Sie unter [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="next-steps"></a>Nächste Schritte
* [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure AD](add-custom-domain.md)
* [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure-Cmdlet-Referenz](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
