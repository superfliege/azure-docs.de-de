---
title: Übersicht über Azure Automation State Configuration
description: Übersicht über Azure Automation State Configuration (DSC, Desired State Configuration), die verwendeten Begriffe und bekannte Fehler
keywords: PowerShell DSC, Desired State Configuration, Konfiguration des gewünschten Zustands, PowerShell DSC Azure
services: automation
ms.service: automation
ms.component: dsc
author: bobbytreed
ms.author: robreed
ms.date: 08/08/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: ef55e6ca6fc913710bae68a7423369b33f26c009
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2018
ms.locfileid: "45628997"
---
# <a name="azure-automation-state-configuration-overview"></a>Übersicht über Azure Automation State Configuration

Azure Automation State Configuration ist ein Azure-Dienst, mit dem Sie PowerShell DSC-[Konfigurationen](/powershell/dsc/configurations) schreiben, verwalten und kompilieren, [DSC-Ressourcen](/powershell/dsc/resources) importieren und Konfigurationen Zielknoten zuweisen können – und all dies in der Cloud.

## <a name="why-use-azure-automation-state-configuration"></a>Gründe für Azure Automation State Configuration

Azure Automation State Configuration bietet verschiedene Vorteile gegenüber der Verwendung von DSC außerhalb von Azure.

### <a name="built-in-pull-server"></a>Integrierter Pullserver

Azure Automation State Configuration bietet einen DSC-Pullserver, der dem [Windows-Dienst DSC](/powershell/dsc/pullserver) ähnelt, sodass Zielknoten Konfigurationen automatisch empfangen, dem gewünschten Zustand entsprechen und ihre Konformität melden. Durch den integrierten Pullserver in Azure Automation entfällt die Notwendigkeit zur Einrichtung und Verwaltung Ihres eigenen Pullservers. Azure Automation eignet sich für virtuelle oder physische Windows- oder Linux-Computer in Cloud- oder lokalen Umgebungen.

### <a name="management-of-all-your-dsc-artifacts"></a>Verwalten aller DSC-Artefakte

Azure Automation State Configuration führt dieselbe Verwaltungsschicht in [PowerShell Desired State Configuration](/powershell/dsc/overview) ein, wie Azure Automation sie für die PowerShell-Skripterstellung bietet.

Über das Azure-Portal oder über PowerShell können Sie Ihre gesamten DSC-Konfigurationen, -Ressourcen und -Zielknoten verwalten.

![Screenshot des Blatts „Azure Automation“](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a>Importieren von Berichtsdaten in Log Analytics

Mit Azure Automation State Configuration verwaltete Knoten senden detaillierte Statusberichtsdaten an den integrierten Pullserver. Sie können Azure Automation State Configuration so konfigurieren, dass diese Daten an Ihren Log Analytics-Arbeitsbereich gesendet werden. Eine Anleitung zum Senden von Statusdaten der Zustandskonfiguration an Ihren Log Analytics-Arbeitsbereich finden Sie unter [Weiterleiten von Azure Automation DSC-Berichtsdaten an Log Analytics](automation-dsc-diagnostics.md).

## <a name="introduction-video"></a>Einführungsvideo

Möchten Sie sich lieber ein Video ansehen? Sehen Sie sich das folgende Video vom Mai 2015 an, als Azure Automation State Configuration erstmals angekündigt wurde.

> [!NOTE]
> Die Konzepte und der Lebenszyklus, die in diesem Video erläutert werden, sind noch zutreffend, jedoch wurde Azure Automation State Configuration seit der Aufzeichnung des Videos beträchtlich weiterentwickelt. Es ist jetzt allgemeine verfügbar, besitzt eine umfangreichere Benutzeroberfläche im Azure-Portal und unterstützt viele zusätzliche Funktionen.

[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung finden Sie unter [Erste Schritte mit Azure Automation State Configuration](automation-dsc-getting-started.md).
- Weitere Informationen zum Integrieren von Knoten finden Sie unter [Onboarding von Computern zur Verwaltung durch Azure Automation DSC](automation-dsc-onboarding.md).
- Wie Sie DSC-Konfigurationen kompilieren und anschließend Zielknoten zuweisen, erfahren Sie unter [Kompilieren von DSC-Konfigurationen in Azure Automation DSC](automation-dsc-compile.md).
- Eine PowerShell-Cmdlet-Referenz ist unter [Azure Automation State Configuration-Cmdlets](/powershell/module/azurerm.automation/#automation) verfügbar.
- Eine Preisübersicht finden Sie unter [Automation – Preise](https://azure.microsoft.com/pricing/details/automation/).
- Ein Verwendungsbeispiel für Azure Automation State Configuration in einer Continuous Deployment-Pipeline finden Sie unter [Continuous Deployment mit Azure Automation State Configuration und Chocolatey](automation-dsc-cd-chocolatey.md).