---
title: Azure Automation DSC – Übersicht
description: Eine Übersicht über Azure Automation Desired State Configuration (DSC), die verwendeten Begriffe und bekannte Fehler.
keywords: PowerShell DSC, Desired State Configuration, Konfiguration des gewünschten Zustands, PowerShell DSC Azure
services: automation
ms.service: automation
ms.component: dsc
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 83d5b89422a0181c06dbfe3b2bd8975ef7214b9d
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
---
# <a name="azure-automation-dsc-overview"></a>Azure Automation DSC – Übersicht

Azure Automation DSC ist ein Azure-Dienst, mit dem Sie PowerShell-[Konfigurationen](https://msdn.microsoft.com/powershell/dsc/configurations) mit Desired State Configuration (DSC) schreiben, verwalten und kompilieren, [DSC-Ressourcen](https://msdn.microsoft.com/powershell/dsc/resources) importieren und Konfigurationen Zielknoten zuweisen können – und all dies in der Cloud.

## <a name="why-use-azure-automation-dsc"></a>Gründe für die Verwendung von Azure Automation DSC

Azure Automation DSC bietet verschiedene Vorteile gegenüber der Verwendung von DSC außerhalb von Azure.

### <a name="built-in-pull-server"></a>Integrierter Pullserver

Azure Automation bietet einen [DSC-Pullserver](https://msdn.microsoft.com/powershell/dsc/pullserver), sodass Zielknoten Konfigurationen automatisch empfangen, dem gewünschten Zustand entsprechen und ihre Konformität melden.
Durch den integrierten Pullserver in Azure Automation entfällt die Notwendigkeit zur Einrichtung und Verwaltung Ihres eigenen Pullservers.
Azure Automation eignet sich für virtuelle oder physische Windows- oder Linux-Computer in Cloud- oder lokalen Umgebungen.

### <a name="management-of-all-your-dsc-artifacts"></a>Verwalten aller DSC-Artefakte

Azure Automation DSC führt dieselbe Verwaltungsschicht in [PowerShell Desired State Configuration](https://msdn.microsoft.com/powershell/dsc/overview) ein, wie Azure Automation sie für die PowerShell-Skripterstellung bietet.

Über das Azure-Portal oder über PowerShell können Sie Ihre gesamten DSC-Konfigurationen, -Ressourcen und -Zielknoten verwalten.

![Screenshot des Blatts „Azure Automation“](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a>Importieren von Berichtsdaten in Log Analytics

Mit Azure Automation DSC verwaltete Knoten senden detaillierte Statusberichtdaten an den integrierten Pullserver.
Sie können Azure Automation DSC so konfigurieren, dass diese Daten an Ihren Log Analytics-Arbeitsbereich gesendet werden.
Eine Anleitung zum Senden von DSC-Statusdaten an Ihren Log Analytics-Arbeitsbereich finden Sie unter [Weiterleiten von Azure Automation DSC-Berichtsdaten an Log Analytics](automation-dsc-diagnostics.md).

## <a name="introduction-video"></a>Einführungsvideo

Möchten Sie sich lieber ein Video ansehen? Betrachten Sie das folgende Video vom Mai 2015, als Azure Automation DSC erstmals angekündigt wurde.

>[!NOTE]
>Die Konzepte und der Lebenszyklus, die in diesem Video erläutert werden, sind noch zutreffend, jedoch wurde Azure Automation DSC seit der Aufzeichnung des Videos beträchtlich weiterentwickelt.
>Es ist jetzt allgemeine verfügbar, besitzt eine umfangreichere Benutzeroberfläche im Azure-Portal und unterstützt viele zusätzliche Funktionen.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a>Nächste Schritte

* Eine Anleitung zum Integrieren von Knoten, die mit Azure Automation DSC verwaltet werden sollen, finden Sie unter [Integrieren von Computern für die Verwaltung durch Azure Automation DSC](automation-dsc-onboarding.md).
* Informationen zu den ersten Schritten mit Azure Automation DSC finden Sie unter [Erste Schritte mit Azure Automation DSC](automation-dsc-getting-started.md).
* Informationen zum Kompilieren von DSC-Konfigurationen und der anschließenden Zuweisung an Zielknoten finden Sie unter [Kompilieren von Konfigurationen in Azure Automation DSC](automation-dsc-compile.md).
* Informationen zur PowerShell-Cmdlet-Referenz für Azure Automation DSC finden Sie unter [Azure Automation DSC-Cmdlets](/powershell/module/azurerm.automation/#automation).
* Preisinformationen finden Sie unter [Azure Automation DSC – Preise](https://azure.microsoft.com/pricing/details/automation/).
* Ein Beispiel zur Verwendung von Azure Automation DSC in einer Continuous Deployment-Pipeline finden Sie unter [Continuous Deployment für IaaS-VMs mit Azure Automation DSC und Chocolatey](automation-dsc-cd-chocolatey.md).
