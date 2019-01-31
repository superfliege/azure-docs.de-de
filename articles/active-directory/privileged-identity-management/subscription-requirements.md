---
title: Lizenzanforderungen für die Verwendung von PIM – Azure | Microsoft-Dokumentation
description: Beschreibt die Lizenzierungsanforderungen für die Verwendung von Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 01/16/2019
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: ad64fba0288aa5663e008484d6f0d2cbdff0bca6
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55187282"
---
# <a name="license-requirements-to-use-pim"></a>Lizenzanforderungen für die Verwendung von PIM

Damit Sie Azure Active Directory (Azure AD) Privileged Identity Management (PIM) verwenden können, muss das Verzeichnis über eine gültige Lizenz verfügen. Darüber hinaus müssen den Administratoren und den erforderlichen Benutzern Lizenzen zugewiesen werden. In diesem Artikel werden die Lizenzanforderungen für die Verwendung von PIM beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie PIM verwenden können, muss Ihr Verzeichnis über eine der folgenden kostenpflichtigen Lizenzen oder Testlizenzen verfügen:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5

Weitere Informationen finden Sie unter [Was ist Azure Active Directory?](../fundamentals/active-directory-whatis.md).

## <a name="which-users-must-have-licenses"></a>Welche Benutzer benötigen Lizenzen?

Jeder Administrator oder Benutzer, der einen Vorteil von PIM nutzt oder erhält, muss über eine Lizenz verfügen. Beispiele:

- Administratoren mit Azure AD-Rollen, die mit PIM verwaltet werden
- Administratoren mit Azure-Ressourcenrollen, die mit PIM verwaltet werden
- Administratoren, die der Rolle „Administrator für privilegierte Rollen“ zugeordnet sind
- Benutzer mit Verzeichnisrollen, die mit PIM verwaltet werden
- Benutzer mit der Berechtigung zum Genehmigen/Ablehnen von Anforderungen in PIM
- Benutzer mit einer Azure-Ressourcenrolle mit Just-in-Time- oder direkten (zeitbasierten) Zuweisungen  
- Benutzer mit einer Zugriffsüberprüfung
- Benutzer, die Zugriffsüberprüfungen ausführen

Weitere Informationen zum Zuweisen von Lizenzen zu Ihren Benutzern finden Sie unter [Zuweisen oder Entfernen von Lizenzen im Azure Active Directory-Portal](../fundamentals/license-users-groups.md).

## <a name="what-happens-when-a-license-expires"></a>Was passiert, wenn eine Lizenz abläuft?

Wenn eine Lizenz vom Typ Azure AD Premium P2 oder EMS E5 bzw. eine Testlizenz abläuft, stehen die Funktionen von PIM in Ihrem Verzeichnis nicht mehr zur Verfügung:

- Permanente Rollenzuweisungen für Azure AD-Rollen sind nicht betroffen.
- Der PIM-Dienst im Azure-Portal sowie die Graph-API-Cmdlets und PowerShell-Schnittstellen von PIM sind für Benutzer nicht mehr verfügbar, um privilegierte Rollen zu aktivieren, den privilegierten Zugriff zu verwalten oder Zugriffsüberprüfungen privilegierter Rollen auszuführen.
- Berechtigte Rollenzuweisungen von Azure AD-Rollen werden entfernt, da Benutzer keine privilegierten Rollen mehr aktivieren können.
- Alle laufenden Zugriffsüberprüfungen von Azure AD-Rollen enden, und PIM-Konfigurationseinstellungen werden entfernt.
- PIM sendet keine E-Mails zu Änderungen an Rollenzuweisungen mehr.

## <a name="next-steps"></a>Nächste Schritte

- [Einstieg in die Verwendung von PIM](pim-getting-started.md)
- [Nicht in PIM verwaltbare Rollen](pim-roles.md)
