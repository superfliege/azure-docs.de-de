---
title: Bericht zu nicht lizenzierter Nutzung | Microsoft Docs
description: Der Bericht zu nicht lizenzierter Nutzung hilft Ihnen dabei, nicht lizenzierte Benutzer zu finden, die kostenpflichtige Azure AD-Funktionen nutzen.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 92138f43-9528-4c8a-b834-66a47da476e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.openlocfilehash: 8bcd814ee7b7bfc158e62ad26e6cc23781f5352d
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2018
---
# <a name="unlicensed-usage-report"></a>Bericht zu nicht lizenzierter Nutzung
Der Bericht zu nicht lizenzierter Nutzung hilft Ihnen dabei, nicht lizenzierte Benutzer zu finden, die kostenpflichtige Azure AD-Funktionen nutzen. Somit können Sie Ihre gekauften Lizenzen besser nutzen und leichter ermitteln, wann Sie weitere Lizenzen benötigen. 

Der Bericht zeigt, welche kostenpflichtigen Funktionen in den letzten 30 Tagen aktiv genutzt wurden. 

## <a name="report-structure"></a>Berichtsstruktur
| Spaltenname | BESCHREIBUNG |
|:--- |:--- |
| Nicht lizenzierter Benutzer |Name des Benutzers |
| Feature |Name des Features. Beispiel: Bedingter Zugriff |
| Genutzte Anwendung |Der Name der Anwendung, auf die mit der Funktion zugegriffen wird. Beispiel: Office 365 SharePoint Online |

> [!NOTE]
> Wenn ein Benutzerkonto gelöscht wurde, wird eine ID wie z. B. 1003000090D8B285 in der Spalte „Nicht lizenzierte Benutzer“ angezeigt.
> 
> 

## <a name="conditional-access-feature"></a>Feature für den bedingten Zugriff
Nicht lizenzierte Benutzer werden beim Zugriff auf einen Dienst, auf den eine Richtlinie für den bedingten Zugriff angewendet wird, gekennzeichnet, wenn sie nicht über eine Azure AD Premium-Lizenz verfügen. 

Dies gilt für MFA-/Standortrichtlinien sowie Geräterichtlinien, die Intune verwenden.

## <a name="see-also"></a>Weitere Informationen
* [Verwenden von bedingtem Zugriff mit Office 365 und anderen verbundenen Azure Active Directory-Apps](active-directory-conditional-access-azure-portal.md)
* [Erste Schritte mit bedingtem Zugriff auf Azure AD](active-directory-conditional-access-azure-portal-get-started.md) 

