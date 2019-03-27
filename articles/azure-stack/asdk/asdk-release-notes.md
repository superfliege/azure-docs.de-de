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
ms.date: 03/18/2019
ms.author: sethm
ms.reviewer: misainat
ms.lastreviewed: 03/18/2019
ms.openlocfilehash: 33f1ccf3f1c7bc657cc66efe7c5025356c954ad6
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58187760"
---
# <a name="asdk-release-notes"></a>Versionshinweise zum ASDK

Dieser Artikel bietet Informationen zu Änderungen, Fehlerbehebungen und bekannten Problemen im Azure Stack Development Kit (ASDK). Wenn Sie nicht sicher sind, welche Version ausgeführt wird, können Sie diese im [Portal überprüfen](../azure-stack-updates.md#determine-the-current-version).

Abonnieren Sie den [![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#)-[Feed](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#), um in Bezug auf Neuerungen im ASDK auf dem Laufenden zu bleiben.

## <a name="build-11902069"></a>Build 1.1902.0.69

### <a name="new-features"></a>Neue Funktionen

- In Build 1902 wird eine neue Benutzeroberfläche im Azure Stack-Administratorportal zum Erstellen von Plänen, Angeboten, Kontingenten und Add-On-Plänen eingeführt. Weitere Informationen (einschließlich Screenshots) finden Sie unter [Erstellen von Plänen, Angeboten und Kontingenten](../azure-stack-create-plan.md).

- Eine Liste der weiteren Änderungen und Verbesserungen in diesem Release finden Sie [in diesem Abschnitt](../azure-stack-update-1902.md#improvements) der Versionshinweise zu Azure Stack.

<!-- ### New features

- For a list of new features in this release, see [this section](../azure-stack-update-1902.md#new-features) of the Azure Stack release notes.

### Fixed and known issues

- For a list of issues fixed in this release, see [this section](../azure-stack-update-1902.md#fixed-issues) of the Azure Stack release notes. For a list of known issues, see [this section](../azure-stack-update-1902.md#known-issues-post-installation).
- Note that [available Azure Stack hotfixes](../azure-stack-update-1902.md#azure-stack-hotfixes) are not applicable to the Azure Stack ASDK. -->

### <a name="known-issues"></a>Bekannte Probleme

- Es wurde ein Problem festgestellt, bei dem an einen internen Lastenausgleich (Internal Load Balancer, ILB) gesendete Pakete mit einer Größe von mehr als 1.450 Bytes gelöscht werden. Dieses Problem ist darauf zurückzuführen, dass die MTU-Einstellung auf dem Host zu niedrig ist, um VXLAN-gekapselte Pakete zu verarbeiten, die die seit Update 1901 auf den Host verschobene Rolle durchlaufen. Das Problem wurde in den folgenden zwei Szenarien beobachtet, kann jedoch auch in weiteren Szenarien auftreten:

  - SQL-Abfragen an SQL AlwaysOn, die hinter einem internen Lastenausgleich ausgeführt werden und größer als 660 Bytes sind.
  - Bei Kubernetes-Bereitstellungen tritt ein Fehler auf, wenn Sie versuchen, mehrere Master zu aktivieren.  

  Das Problem tritt auf, wenn eine VM und ein interner Lastenausgleich, die sich im gleichen virtuellen Netzwerk, aber in unterschiedlichen Subnetzen befinden, miteinander kommunizieren. Sie können das Problem umgehen, indem Sie auf dem ASDK-Host die folgenden Befehle an einer Eingabeaufforderung mit erhöhten Rechten ausführen:

  ```shell
  netsh interface ipv4 set sub "hostnic" mtu=1660
  netsh interface ipv4 set sub "management" mtu=1660
  ```

## <a name="build-11901095"></a>Build 1.1901.0.95

Lesen Sie die [wichtigen Buildinformationen in den Versionshinweisen zu Azure Stack](../azure-stack-update-1901.md#build-reference).

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
