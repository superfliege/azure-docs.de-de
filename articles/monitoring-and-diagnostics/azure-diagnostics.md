---
title: Übersicht über die Azure-Diagnoseerweiterung | Microsoft-Dokumentation
description: Verwenden Sie die Azure-Diagnose zur Problembehandlung, zur Leistungsmessung, zur Überwachung und zur Datenverkehrsanalyse in Clouddiensten, virtuellen Maschinen und Service Fabric.
services: multiple
documentationcenter: .net
author: rboucher
manager: ''
editor: ''
ms.assetid: baad40d8-c915-4f93-b486-8b160bf33463
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/01/2018
ms.author: robb
ms.openlocfilehash: daeaddefa461e71fcc62af4efc4fb7084b237cf9
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
ms.locfileid: "33886394"
---
# <a name="what-is-azure-diagnostics-extension"></a>Was ist die Azure-Diagnoseerweiterung?
Die Azure-Diagnoseerweiterung ist ein Agent innerhalb von Azure, mit dem Diagnosedaten für eine bereitgestellte Anwendung erfasst werden können. Sie können die Diagnoseerweiterung von einer Reihe verschiedener Quellen aus verwenden. Derzeit werden die Web- und Workerrollen des Azure-Clouddiensts (klassisch), Virtual Machines, Virtual Machine Scale Sets und Service Fabric unterstützt. Andere Azure-Dienste haben unterschiedliche Diagnosemethoden. Siehe [Übersicht über die Überwachung in Microsoft Azure](monitoring-overview.md). 

## <a name="linux-agent"></a>Linux-Agent
Für virtuelle Computer mit Linux ist eine [Linux-Version der Erweiterung](../virtual-machines/linux/diagnostic-extension.md) verfügbar. Die gesammelten Statistiken und das Verhalten unterscheiden sich von der Windows-Version. 

## <a name="data-you-can-collect"></a>Erfassbare Daten
Die Azure-Diagnoseerweiterung kann die folgenden Datentypen erfassen:

| Data source | BESCHREIBUNG |
| --- | --- |
| Leistungsindikatoren |Leistungsindikatoren des Betriebssystems und benutzerdefinierte Leistungsindikatoren |
| Anwendungsprotokolle |Von Ihrer Anwendung geschriebene Ablaufverfolgungsmeldungen |
| Windows-Ereignisprotokolle |An das Windows-System für die Ereignisprotokollierung gesendete Informationen |
| .NET-Ereignisquelle |Code zum Schreiben von Ereignissen mit der .NET-Klasse [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) |
| IIS-Protokolle |Informationen zu IIS-Websites |
| Manifestbasiertes ETW |Von einem beliebigen Prozess generierte Ereignisse der Ereignisablaufverfolgung für Windows |
| Absturzabbilder |Informationen zum Status des Prozesses im Fall eines Anwendungsabsturzes |
| Benutzerdefinierte Fehlerprotokolle |Von Ihrer Anwendung oder Ihrem Dienst erstellte Protokolle |
| Infrastrukturprotokolle der Azure-Diagnose |Informationen zur Diagnose selbst |

## <a name="data-storage"></a>Datenspeicher
Die Erweiterung speichert ihre Daten in einem von Ihnen angegebenen [Azure Storage-Konto](azure-diagnostics-storage.md). 

Sie können auch an [Application Insights](../application-insights/app-insights-cloudservices.md) gesendet werden. Als weitere Option können sie auch an [Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md) gestreamt werden, was Ihnen wiederum ermöglicht, sie an Überwachungsdienste außerhalb von Azure zu senden. 


## <a name="versioning-and-configuration-schema"></a>Schema für Versionsverwaltung und Konfiguration
Siehe [Azure-Diagnoseerweiterung – Versionen und Verlauf des Konfigurationsschemas](azure-diagnostics-versioning-history.md).


## <a name="next-steps"></a>Nächste Schritte
Wählen Sie, zu welchem Dienst Sie Diagnoseinformationen erfassen möchten, und lesen Sie zum Einstieg die folgenden Artikel. Verwenden Sie die allgemeinen Links zur Azure-Diagnose als Referenz für bestimmte Aufgaben.

## <a name="cloud-services-using-azure-diagnostics"></a>Clouddienste mit der Azure-Diagnose
* Wenn Sie Visual Studio verwenden, finden Sie unter [Verfolgen einer Cloud Services-Anwendung mit Visual Studio](../vs-azure-tools-debug-cloud-services-virtual-machines.md) Informationen für den Einstieg. Andernfalls finden Sie weitere Informationen in folgenden Artikeln:
* [Überwachen von Clouddiensten mit der Azure-Diagnose](../cloud-services/cloud-services-how-to-monitor.md)
* [Einrichten der Azure-Diagnose in einer Cloud Services-Anwendung](../cloud-services/cloud-services-dotnet-diagnostics.md)

Fortgeschrittenere Themen finden Sie in folgenden Artikeln:

* [Verwenden der Azure-Diagnose mit Application Insights für Cloud Services](../application-insights/app-insights-cloudservices.md)
* [Verfolgen des Ablaufs einer Cloud Services-Anwendung mit der Azure-Diagnose](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
* [Verwenden von PowerShell zum Einrichten der Diagnose für Cloud Services](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="virtual-machines"></a>Virtual Machines
* Wenn Sie Visual Studio verwenden, finden Sie unter [Verfolgen von Azure Virtual Machines mit Visual Studio](../vs-azure-tools-debug-cloud-services-virtual-machines.md) Informationen für den Einstieg. Andernfalls finden Sie weitere Informationen in folgenden Artikeln:
* [Einrichten der Azure-Diagnose auf einem virtuellen Azure-Computer](../virtual-machines-dotnet-diagnostics.md)

Fortgeschrittenere Themen finden Sie in folgenden Artikeln:

* [Verwenden von PowerShell zum Einrichten der Diagnose für Azure Virtual Machines](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Erstellen eines virtuellen Windows-Computers mit Überwachung und Diagnose mithilfe von Azure Resource Manager-Vorlagen](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric"></a>Service Fabric
Erste Schritte finden Sie unter [Überwachen einer Service Fabric-Anwendung](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Viele weitere Artikel zur Service Fabric-Diagnose stehen in der Navigationsstruktur auf der linken Seite zur Verfügung, nachdem Sie diesen Artikel aufgerufen haben.

## <a name="general-articles"></a>Allgemeine Artikel
* Hier erhalten Sie Informationen zur Verwendung von [Leistungsindikatoren in der Azure-Diagnose](../cloud-services/diagnostics-performance-counters.md).
* Wenn bei der Diagnose Probleme mit dem Starten oder dem Finden von Daten in Azure-Speichertabellen auftreten, finden Sie weitere Informationen unter [Problembehandlung mit Azure-Diagnose](azure-diagnostics-troubleshooting.md).
