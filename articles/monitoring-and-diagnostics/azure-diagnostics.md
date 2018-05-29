---
title: Übersicht über Azure-Diagnose | Microsoft-Dokumentation
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
ms.date: 03/18/2017
ms.author: robb
ms.openlocfilehash: 0231a6c1d78818b948bb24d0c406fb2f2da17a0f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32169134"
---
# <a name="what-is-azure-diagnostics"></a>Was ist die Azure-Diagnose?
Die Azure-Diagnose ist eine Funktion in Azure, mit der Diagnosedaten für eine bereitgestellte Anwendung erfasst werden können. Sie können die Diagnoseerweiterung von einer Reihe verschiedener Quellen aus verwenden. Derzeit werden die Web- und Workerrollen des Azure-Clouddiensts (klassisch), Virtual Machines, Virtual Machine Scale Sets und Service Fabric unterstützt. Andere Azure-Dienste haben unterschiedliche Diagnosemethoden. Siehe [Übersicht über die Überwachung in Microsoft Azure](monitoring-overview.md). 

## <a name="data-you-can-collect"></a>Erfassbare Daten
Mit der Azure-Diagnose können Sie die folgenden Arten von Daten erfassen:

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

Die Azure-Diagnoseerweiterung kann diese Daten an ein Azure-Speicherkonto übertragen oder an [Application Insights](../application-insights/app-insights-cloudservices.md) senden. Sie können sie auch an [Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md) streamen, was Ihnen wiederum ermöglicht, sie an Überwachungsdienste außerhalb von Azure zu senden. Die Daten können zum Debuggen und Beheben von Fehlern, Messen der Leistung, Überwachen der Ressourcenauslastung, Analysieren des Datenverkehrs, Planen der Kapazität und Durchführen der Überwachung verwendet werden.

## <a name="versioning"></a>Versionsverwaltung
Informationen finden Sie unter [Azure-Diagnose – Versionsverlauf](azure-diagnostics-versioning-history.md)

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

## <a name="virtual-machines-using-azure-diagnostics"></a>Virtual Machines mit der Azure-Diagnose
* Wenn Sie Visual Studio verwenden, finden Sie unter [Verfolgen von Azure Virtual Machines mit Visual Studio](../vs-azure-tools-debug-cloud-services-virtual-machines.md) Informationen für den Einstieg. Andernfalls finden Sie weitere Informationen in folgenden Artikeln:
* [Einrichten der Azure-Diagnose auf einem virtuellen Azure-Computer](../virtual-machines-dotnet-diagnostics.md)

Fortgeschrittenere Themen finden Sie in folgenden Artikeln:

* [Verwenden von PowerShell zum Einrichten der Diagnose für Azure Virtual Machines](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Erstellen eines virtuellen Windows-Computers mit Überwachung und Diagnose mithilfe von Azure Resource Manager-Vorlagen](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric-using-azure-diagnostics"></a>Service Fabric mit der Azure-Diagnose
Erste Schritte finden Sie unter [Überwachen einer Service Fabric-Anwendung](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Viele weitere Artikel zur Service Fabric-Diagnose stehen in der Navigationsstruktur auf der linken Seite zur Verfügung, nachdem Sie diesen Artikel aufgerufen haben.

## <a name="general-azure-diagnostics-articles"></a>Allgemeine Artikel zur Azure-Diagnose
* [Schemakonfiguration der Azure-Diagnose](https://msdn.microsoft.com/library/azure/mt634524.aspx) : Erfahren Sie, wie Sie die Schemadatei so ändern, dass Diagnosedaten gesammelt und weitergeleitet werden. Beachten Sie, dass Sie die Schemadatei auch mit Visual Studio ändern können.
* [Speichern von Azure-Diagnosedaten in Azure Storage](../cloud-services/cloud-services-dotnet-diagnostics-storage.md): Lernen Sie die Namen der Tabellen und Blobs kennen, in die die Diagnosedaten geschrieben werden.
* Hier erhalten Sie Informationen zur Verwendung von [Leistungsindikatoren in der Azure-Diagnose](../cloud-services/diagnostics-performance-counters.md).
* Erlernen Sie das [Weiterleiten von Azure-Diagnoseinformationen an Application Insights](azure-diagnostics-configure-application-insights.md).
* Wenn bei der Diagnose Probleme mit dem Starten oder dem Finden von Daten in Azure Storage-Tabellen auftreten, finden Sie weitere Informationen unter [Problembehandlung mit Azure-Diagnose](azure-diagnostics-troubleshooting.md).
