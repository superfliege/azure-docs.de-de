---
title: Übersicht über die Azure Automation-Zustandskonfiguration
description: Übersicht über die Azure Automation-Zustandskonfiguration (DSC, Desired State Configuration), die verwendeten Begriffe und bekannte Fehler
keywords: PowerShell DSC, Desired State Configuration, Konfiguration des gewünschten Zustands, PowerShell DSC Azure
services: automation
ms.service: automation
ms.component: dsc
author: DCtheGeek
ms.author: dacoulte
ms.date: 08/08/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d2d46803f81b369f8f24a6f0e3c7f32955503e4a
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2018
ms.locfileid: "40006060"
---
# <a name="azure-automation-state-configuration-overview"></a>Übersicht über die Azure Automation-Zustandskonfiguration

Die Azure Automation-Zustandskonfiguration ist ein Azure-Dienst, mit dem Sie PowerShell DSC-[Konfigurationen](/powershell/dsc/configurations) schreiben, verwalten und kompilieren, [DSC-Ressourcen](/powershell/dsc/resources) importieren und Konfigurationen Zielknoten zuweisen können – und all dies in der Cloud.

## <a name="why-use-azure-automation-state-configuration"></a>Gründe für die Azure Automation-Zustandskonfiguration

Die Azure Automation-Zustandskonfiguration bietet verschiedene Vorteile gegenüber der Verwendung von DSC außerhalb von Azure.

### <a name="built-in-pull-server"></a>Integrierter Pullserver

Die Azure Automation-Zustandskonfiguration bietet einen DSC-Pullserver, der dem [Windows-Dienst DSC](/powershell/dsc/pullserver) ähnelt, sodass Zielknoten Konfigurationen automatisch empfangen, dem gewünschten Zustand entsprechen und ihre Konformität melden. Durch den integrierten Pullserver in Azure Automation entfällt die Notwendigkeit zur Einrichtung und Verwaltung Ihres eigenen Pullservers. Azure Automation eignet sich für virtuelle oder physische Windows- oder Linux-Computer in Cloud- oder lokalen Umgebungen.

### <a name="management-of-all-your-dsc-artifacts"></a>Verwalten aller DSC-Artefakte

Die Azure Automation-Zustandskonfiguration führt dieselbe Verwaltungsschicht in [PowerShell Desired State Configuration](/powershell/dsc/overview) ein, wie Azure Automation sie für die PowerShell-Skripterstellung bietet.

Über das Azure-Portal oder über PowerShell können Sie Ihre gesamten DSC-Konfigurationen, -Ressourcen und -Zielknoten verwalten.

![Screenshot des Blatts „Azure Automation“](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a>Importieren von Berichtsdaten in Log Analytics

Mit der Azure Automation-Zustandskonfiguration verwaltete Knoten senden detaillierte Statusberichtsdaten an den integrierten Pullserver. Sie können die Azure Automation-Zustandskonfiguration so konfigurieren, dass diese Daten an Ihren Log Analytics-Arbeitsbereich gesendet werden. Eine Anleitung zum Senden von Statusdaten der Zustandskonfiguration an Ihren Log Analytics-Arbeitsbereich finden Sie unter [Weiterleiten von Berichtsdaten der Azure Automation-Zustandskonfiguration an Log Analytics](automation-dsc-diagnostics.md).

## <a name="introduction-video"></a>Einführungsvideo

Möchten Sie sich lieber ein Video ansehen? Sehen Sie sich das folgende Video vom Mai 2015 an, als die Azure Automation-Zustandskonfiguration erstmals angekündigt wurde.

> [!NOTE]
> Die Konzepte und der Lebenszyklus, die in diesem Video erläutert werden, sind noch zutreffend, jedoch wurde die Azure Automation-Zustandskonfiguration seit der Aufzeichnung des Videos beträchtlich weiterentwickelt. Es ist jetzt allgemeine verfügbar, besitzt eine umfangreichere Benutzeroberfläche im Azure-Portal und unterstützt viele zusätzliche Funktionen.

[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung finden Sie unter [Erste Schritte mit der Azure Automation-Zustandskonfiguration](automation-dsc-getting-started.md).
- Weitere Informationen zum Integrieren von Knoten finden Sie unter [Integrieren von Computern für die Verwaltung durch die Azure Automation-Zustandskonfiguration](automation-dsc-onboarding.md).
- Wie Sie DSC-Konfigurationen kompilieren und anschließend Zielknoten zuweisen, erfahren Sie unter [Kompilieren von DSC-Konfigurationen in der Azure Automation-Zustandskonfiguration](automation-dsc-compile.md).
- Eine PowerShell-Cmdlet-Referenz ist unter [Cmdlets der Azure Automation-Zustandskonfiguration](/powershell/module/azurerm.automation/#automation) verfügbar.
- Eine Preisübersicht finden Sie unter [Azure Automation-Zustandskonfiguration: Preise](https://azure.microsoft.com/pricing/details/automation/).
- Ein Verwendungsbeispiel für die Azure Automation-Zustandskonfiguration in einer Continuous Deployment-Pipeline finden Sie unter [Continuous Deployment mit der Azure Automation-Zustandskonfiguration und Chocolatey](automation-dsc-cd-chocolatey.md).