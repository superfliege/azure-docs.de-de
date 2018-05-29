---
title: Externe Office 365-Freigaben und Azure Active Directory B2B-Zusammenarbeit | Microsoft-Dokumentation
description: Erläutert das Freigeben von Ressourcen mit externen Partnern, die Office 365- und Azure Active Directory B2B-Zusammenarbeit verwenden.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/24/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: d55d587022b763a6890c098dd0a1b9bef2a3b7fb
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
ms.locfileid: "34267088"
---
# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a>Externe Office 365-Freigaben und Azure Active Directory B2B-Zusammenarbeit

Externe Office 365-Freigaben (OneDrive, SharePoint Online, einheitliche Gruppen usw.) und die Verwendung der Azure AD B2B-Zusammenarbeit sind technisch gesehen das Gleiche. Bei allen externen Freigabetools (außer OneDrive/SharePoint Online), einschließlich Gästen in Office 365-Gruppen, werden bereits die Einladungs-APIs der Azure AD B2B-Zusammenarbeit für die Freigabe verwendet.

OneDrive/SharePoint Online hat einen separaten Einladung-Manager. Unterstützung für die externe Freigabe in OneDrive/SharePoint Online begann, bevor die Unterstützung durch Azure AD entwickelt wurde. Im Lauf der Zeit wurde die externe OneDrive/SharePoint Online-Freigabe um eine Vielzahl von Features erweitert, und viele Millionen Benutzer verwenden das integrierte Freigabemuster des Produkts. Es gibt jedoch einige feine Unterschiede zwischen der Funktionsweise der externen OneDrive-/SharePoint Online-Freigaben und der Azure AD B2B-Zusammenarbeit:

- OneDrive/SharePoint Online fügt Benutzer dem Verzeichnis hinzu, nachdem sie ihre Einladungen eingelöst haben. Deshalb werden die Benutzer vor dem Einlösen nicht im Azure AD-Portal angezeigt. Wenn der Benutzer in der Zwischenzeit von einer anderen Website eingeladen wird, wird eine neue Einladung generiert. Wenn Sie jedoch Azure AD B2B-Zusammenarbeit verwenden, werden Benutzer sofort bei Einladung hinzugefügt, sodass sie überall angezeigt werden.

- Die Einlösungserfahrung in OneDrive/SharePoint Online unterscheidet sich von der Erfahrung in Azure AD B2B-Zusammenarbeit. Nachdem ein Benutzer eine Einladung eingelöst hat, sind die Erfahrungen gleich.

- In Azure AD B2B-Zusammenarbeit eingeladene Benutzer können in Freigabedialogfeldern von OneDrive/SharePoint Online ausgewählt werden. Benutzer, die über OneDrive/SharePoint Online eingeladen wurden, werden nach dem Einlösen der Einladung auch in Azure AD angezeigt.

- Um externe Freigaben in OneDrive/SharePoint Online mit der Azure AD B2B-Zusammenarbeit zu verwalten, legen Sie die Einstellung für externe OneDrive-/SharePoint Online-Freigaben auf **Freigabe nur für externe Benutzer zulassen, die sich bereits im Verzeichnis befinden** fest. Benutzer können zu extern freigegebenen Websites wechseln und aus externen Projektmitarbeitern auswählen, die der Administrator hinzugefügt hat. Ein Administrator kann die externen Projektmitarbeiter über die Einladungs-APIs der B2B-Zusammenarbeit hinzufügen.

![Die Einstellung der externen OneDrive/SharePoint Online-Freigabe](media/o365-external-user/odsp-sharing-setting.png)

## <a name="next-steps"></a>Nächste Schritte

* [Was ist die Azure AD B2B-Zusammenarbeit?](what-is-b2b.md)
* [Hinzufügen eines B2B-Zusammenarbeitsbenutzers zu einer Rolle](add-guest-to-role.md)
* [Delegieren von Einladungen zur B2B-Zusammenarbeit](delegate-invitations.md)
* [Dynamische Gruppen und B2B-Zusammenarbeit](use-dynamic-groups.md)