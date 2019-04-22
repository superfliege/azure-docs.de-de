---
title: Ändern der Standardwerte für die Tokengültigkeitsdauer für eine benutzerdefiniert entwickelte Anwendung | Microsoft-Dokumentation
description: Aktualisieren der Richtlinien für die Tokengültigkeitsdauer für Ihre Anwendung, die Sie in Azure AD entwickeln
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: celested
ms.custom: seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: 04abdedf5ac19be3d5a43e7502cbc97f8f5fee43
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59360310"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Ändern der Standardwerte für die Tokengültigkeitsdauer für eine benutzerdefiniert entwickelte Anwendung

In diesem Artikel wird erläutert, wie PowerShell von Azure AD verwendet werden kann, um eine Richtlinie für die Tokengültigkeitsdauer festzulegen. Azure AD Premium ermöglicht App-Entwicklern und Mandantenadministratoren die Konfiguration der Gültigkeitsdauer von Token, die für nicht vertrauliche Clients ausgegeben werden. Die Richtlinien für die Tokengültigkeitsdauer werden mandantenübergreifend oder auf Basis des Ressourcenzugriffs festgelegt.

1. Um eine Richtlinie für die Tokengültigkeitsdauer festzulegen, müssen Sie das [Azure AD PowerShell-Modul](https://www.powershellgallery.com/packages/AzureADPreview) herunterladen.
1. Führen Sie den Befehl **Connect-AzureAD -Confirm** aus.

    Im Folgenden finden Sie eine Beispielrichtlinie, die die maximale Alterung für das Einzelfaktor-Aktualisierungstoken festlegt. Erstellen der Richtlinie:
  ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Konfigurieren der Gültigkeitsdauer der von Azure AD ausgestellten Token finden Sie unter [Konfigurierbare Gültigkeitsdauer in Azure AD (Vorschau)](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes). Dort erfahren Sie unter anderem, wie Sie die Tokengültigkeitsdauer für alle Apps in Ihrer Organisation, für eine mehrinstanzenfähige App oder für einen bestimmten Dienstprinzipal in Ihrer Organisation festlegen. 
* [Azure AD-Tokenreferenz](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)

