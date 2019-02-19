---
title: Microsoft Azure Stack Development Kit – Versionshinweise | Microsoft-Dokumentation
description: Verbesserungen, Fehlerbehebungen und bekannte Probleme in Azure Stack Development Kit
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2019
ms.author: sethm
ms.reviewer: misainat
ms.lastreviewed: 02/09/2019
ms.openlocfilehash: 485d88a4765d7cedcb171a5b325fe5f366fff1f9
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2019
ms.locfileid: "56004753"
---
# <a name="asdk-release-notes"></a>Versionshinweise zum ASDK

Dieser Artikel bietet Informationen zu Änderungen, Fehlerbehebungen und bekannten Problemen im Azure Stack Development Kit (ASDK). Wenn Sie nicht sicher sind, welche Version ausgeführt wird, können Sie diese im [Portal überprüfen](../azure-stack-updates.md#determine-the-current-version).

Abonnieren Sie den [![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#)-[Feed](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#), um in Bezug auf Neuerungen im ASDK auf dem Laufenden zu bleiben.

## <a name="build-11901095"></a>Build 1.1901.0.95

### <a name="changes"></a>Änderungen

Dieser Build enthält die folgenden Verbesserungen für Azure Stack:

- BGP- und NAT-Komponenten werden nun auf dem physischen Host bereitgestellt. Dadurch entfällt die Notwendigkeit, zwei öffentliche oder Unternehmens-IP-Adressen für die Bereitstellung des ASDK bereitzustellen, was zusätzlich die Bereitstellung vereinfacht.
- Integrierte Azure Stack-Systemsicherungen können jetzt mithilfe des PowerShell-Skripts **asdk-installer. ps1** [überprüft werden](asdk-validate-backup.md).

### <a name="new-features"></a>Neue Funktionen

- Eine Liste der neuen Funktionen in diesem Release finden Sie [in diesem Abschnitt](../azure-stack-update-1901.md#new-features) der Anmerkungen zu dieser Version für Azure Stack.

### <a name="fixed-and-known-issues"></a>Behobene und bekannte Probleme

- Eine Liste der in diesem Release behobenen Probleme finden Sie [in diesem Abschnitt](../azure-stack-update-1901.md#fixed-issues) der Anmerkungen zu dieser Version für Azure Stack. Eine Liste der bekannten Probleme finden Sie [in diesem Abschnitt](../azure-stack-update-1901.md#known-issues-post-installation).
- Beachten Sie, dass [verfügbare Hotfixes für Azure Stack](../azure-stack-update-1901.md#azure-stack-hotfixes) nicht auf das Azure Stack ASDK anwendbar sind.

## <a name="build-118110101"></a>Build 1.1811.0.101

### <a name="changes"></a>Änderungen

Dieser Build enthält die folgenden Verbesserungen für Azure Stack:  

- Es gibt einen Satz von neuen Mindestanforderungen und Empfehlungen für Hardware und Software für das ASDK. Diese neuen empfohlenen Spezifikationen sind in [Überlegungen zur Planung der Azure Stack-Bereitstellung](asdk-deploy-considerations.md) dokumentiert. Da sich die Azure Stack-Plattform weiterentwickelt hat, sind jetzt mehr Dienste verfügbar, wodurch möglicherweise auch mehr Ressourcen erforderlich werden. Die gestiegenen Spezifikationen spiegeln diese überarbeiteten Empfehlungen wider.

### <a name="new-features"></a>Neue Funktionen

Eine Liste der neuen Funktionen in diesem Release finden Sie [in diesem Abschnitt](../azure-stack-update-1811.md#new-features) der Anmerkungen zu dieser Version für Azure Stack.

### <a name="fixed-and-known-issues"></a>Behobene und bekannte Probleme

Eine Liste der in diesem Release behobenen Probleme finden Sie [in diesem Abschnitt](../azure-stack-update-1811.md#fixed-issues) der Anmerkungen zu dieser Version für Azure Stack. Eine Liste der bekannten Probleme finden Sie [in diesem Abschnitt](../azure-stack-update-1811.md#known-issues-post-installation).

## <a name="build-11809090"></a>Build 1.1809.0.90

### <a name="new-features"></a>Neue Funktionen

Eine Liste der neuen Funktionen in diesem Release finden Sie [in diesem Abschnitt](../azure-stack-update-1809.md#new-features) der Anmerkungen zu dieser Version für Azure Stack.

### <a name="fixed-issues"></a>Behobene Probleme

Eine Liste der in diesem Release behobenen Probleme finden Sie [in diesem Abschnitt](../azure-stack-update-1809.md#fixed-issues).

### <a name="known-issues"></a>Bekannte Probleme

Eine Liste der bekannten Probleme in diesem Release finden Sie [in diesem Abschnitt](../azure-stack-update-1809.md#known-issues-post-installation).