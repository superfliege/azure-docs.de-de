---
title: Verwalten von Azure API Management mit Azure Automation
description: Erfahren Sie, wie der Azure Automation-Dienst zum Verwalten von Azure API Management verwendet werden kann.
services: api-management, automation
documentationcenter: ''
author: vladvino
manager: eamono
editor: ''
ms.assetid: 2e53c9af-f738-47f8-b1b6-593050a7c51b
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2018
ms.author: apimpm
ms.openlocfilehash: fee93d01f0462cbee1a3e1110c56fb57220f8004
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49956856"
---
# <a name="managing-azure-api-management-using-azure-automation"></a>Verwalten von Azure API Management mit Azure Automation
Dieser Leitfaden bietet eine Einführung in den Azure Automation-Dienst und zeigt, wie dieser zur Vereinfachung der Verwaltung von Azure API Management genutzt werden kann.

## <a name="what-is-azure-automation"></a>Was ist Azure Automation?
[Azure Automation](https://azure.microsoft.com/services/automation/) ist ein Azure-Dienst für die Vereinfachung der Cloudverwaltung durch eine  Prozessautomatisierung. Mithilfe von Azure Automation können manuelle, sich wiederholende, lang andauernde und fehleranfällige Aufgaben automatisiert werden, um die Zuverlässigkeit und Effizienz für Ihre Organisation zu erhöhen und die Amortisierungszeit zu verkürzen.

Azure Automation bietet eine äußerst zuverlässige, hoch verfügbare Engine für die Workflowausführung, die sich Ihren Anforderungen entsprechend skalieren lässt. In Azure Automation können Prozesse manuell, durch Drittanbietersysteme oder in geplanten Intervallen gestartet werden, sodass Aufgaben genau nach Bedarf ausgeführt werden.

Indem Sie die Aufgaben im Zusammenhang mit der Cloudverwaltung mit Azure Automation automatisieren, verringern Sie den Verwaltungsaufwand und ermöglichen es Ihren IT- und DevOps-Mitarbeitern, sich Aufgaben zu widmen, die einen Mehrwert für Ihr Unternehmen liefern.

## <a name="how-can-azure-automation-help-manage-azure-api-management"></a>Wie kann Azure Automation beim Verwalten von Azure API Management nützlich sein?
API Management kann in Azure Automation mithilfe von [Windows PowerShell-Cmdlets für Azure API Management](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/?view=azurermps-5.5.0#api_management/)verwaltet werden. In Azure Automation können Sie PowerShell-Workflowskripts schreiben, um viele Ihrer API Management-Aufgaben mithilfe von Cmdlets auszuführen. Sie können diese Cmdlets in Azure Automation auch an die Cmdlets für andere Azure-Dienste koppeln, um komplexe Aufgaben über Azure-Dienste und Drittanbietersysteme hinweg zu automatisieren.

Hier sind einige Beispiele für die Verwendung von API Management mit PowerShell:

* [Azure PowerShell-Beispiele für API Management](https://docs.microsoft.com/azure/api-management/powershell-samples)

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich nun mit den Grundlagen von Azure Automation und dessen Einsatz zur Verwaltung von Azure API Management vertraut gemacht haben, folgen Sie diesen Links, um weitere Informationen zu erhalten.

* Siehe das Tutorial [Erste Schritte](../automation/automation-first-runbook-graphical.md)zu Azure Automation.

