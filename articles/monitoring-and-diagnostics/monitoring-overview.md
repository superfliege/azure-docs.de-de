---
title: "Überwachung in Microsoft Azure | Microsoft-Dokumentation"
description: "Optionen bei der Überwachung von Elemente in Microsoft Azure. Azure Monitor, Application Insights und Log Analytics"
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: robb
ms.openlocfilehash: e164cbd910ccc38610c7aef37d25ff1b4413038d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-monitoring-in-microsoft-azure"></a>Überblick über die Überwachung in Microsoft Azure
Dieser Artikel enthält eine Übersicht über die Tools und Dienste, die die ganzheitliche Überwachung von Microsoft Azure umfasst. Folgendes wird behandelt:
- Verwenden von Azure-Diensten, zum Überwachen der Azure-Infrastruktur und Azure-Anwendungen
- Verwenden von Azure-Diensten zum Überwachen von hybriden und Nicht-Azure-Infrastrukturen und -Anwendungen
- Verwenden von Nicht-Azure-Diensten zum Überwachen der Azure-Infrastruktur und Azure-Anwendungen

Der Artikel erläutert die verschiedenen verfügbaren Produkte und Dienste und wie sie zusammen funktionieren. Er unterstützt Sie bei der Festlegung der Tools, die für Sie in den jeweiligen Fällen am besten geeignet sind.  

## <a name="why-use-azures-monitoring-services"></a>Warum sollten Sie die Azure-Überwachungsdienste verwenden?

Leistungsprobleme in Ihrer Cloud-App können Ihr Unternehmen beeinträchtigen. Durch mehrere miteinander verbundene Komponenten und häufige Versionswechsel können Leistungseinbußen jederzeit vorkommen. Bei der Entwicklung einer App werden von Ihren Benutzern in der Regel Probleme entdeckt, die Sie beim Testen nicht gefunden haben. Sie sollten sofort über diese Probleme Bescheid wissen und über Tools zu deren Diagnose und Beseitigung verfügen. Darüber hinaus resultieren Probleme in Ihrer Anwendung sind oft das Ergebnis der zugrunde liegenden Infrastruktur, in der diese Anwendungen ausgeführt werden. Eine ganzheitliche Übersicht über Ihre Anwendung und Infrastruktur ist also der Schlüssel zum Überwachen Ihrer Azure-Umgebung. Microsoft Azure umfasst eine Reihe von Tools zum Identifizieren und Beheben solcher Probleme.

## <a name="how-do-i-monitor-my-azure-environment"></a>Wie überwache ich meine Azure-Umgebung?

Es gibt eine Reihe von Tools zum Überwachen Ihrer Azure-Umgebung, die vom Anwendungscode, der in Azure ausgeführt wird, bis zu den Diensten und der Infrastruktur reichen, die diesen Code ausführen. Diese Tools arbeiten zusammen, um eine umfassende Cloudüberwachung anzubieten und umfassen Folgendes:

-   **Azure Monitor**: Der Azure-Dienst, der als konsolidierte Pipeline für alle Überwachungsdaten von Azure-Diensten fungiert. Sie erhalten Zugriff auf die Leistungsmetriken und Ereignisse, die den Betrieb der Azure-Infrastruktur und aller Azure-Dienste beschreiben, die Sie verwenden. Azure Monitor ist eine Datenpipeline für die Überwachung Ihrer Azure-Umgebung, die diese Daten direkt für Log Analytics und Tools von Drittanbietern anbietet, mit denen Sie einen Einblick in die Daten erhalten und diese mit Daten von lokalen oder Cloudressourcen kombinieren können.

-   **Application Insights**: Der Azure-Dienst, der die Überwachung der Anwendungsleistung und Benutzeranalysen anbietet. Der von Ihnen geschriebene Code und die Anwendungen, die Sie in Azure oder auf lokalen/anderen Clouds bereitgestellt haben, werden überwacht. Indem Ihre Anwendung mit dem Application Insights SDK instrumentieren, können Sie Zugriff auf eine Reihe von Daten erhalten, einschließlich der Antwortzeiten von Abhängigkeiten, Ausnahmeablaufverfolgungen, Debuggingmomentaufnahmen und Ausführungsprofile. Es stellt leistungsfähige Tools zum Analysieren dieser Anwendungstelemetrie während des Entwickelns und des Betriebs Ihrer Anwendung zur Verfügung. Durch die tiefe Integration in Visual Studio können Sie direkt zu den problematischen Codezeilen gelangen, um die Fehler zu beheben. Weiterhin wird eine Nutzungsanalyse bereitgestellt, um die Nutzung Ihrer Anwendung durch Kunden für die Produkt-Manager zu analysieren.

-   **Log Analytics** (ehemals OMS Log Analytics) ist ein Azure-Dienst, der Protokoll- und Metrikdaten von Azure-Diensten (über Azure Monitor), Azure-VMs und lokalen oder anderen Cloudinfrastrukturen erfasst und eine flexible Protokollsuche sowie auf diesen Daten basierende Standardanalysen anbietet. Es stellt umfassende Tools zur quellenübergreifenden Datenanalyse bereit, erlaubt komplexe Abfragen in allen Protokollen und kann beim Eintreten angegebener Bedingungen proaktiv Warnungen ausgeben.  Sie können sogar benutzerdefinierte Daten im zugehörigen zentralen Repository sammeln, um sie abfragen und visualisieren zu können. Sie können ebenfalls die in Log Analytics integrierten Lösungen nutzen, um unmittelbare Einblicke in die Sicherheit und Funktionalität Ihrer Infrastruktur zu erhalten.

## <a name="accessing-monitoring-in-the-azure-portal"></a>Zugreifen auf die Überwachung im Azure-Portal
Alle Azure-Überwachungsdienste sind jetzt in einem zentralen Bereich der Benutzeroberfläche verfügbar. Weitere Informationen zum Zugriff auf diesen Bereich finden Sie unter [Erste Schritte mit Azure Monitor](monitoring-get-started.md). 

Auf Überwachungsfunktionen für bestimmte Ressourcen können Sie auch zugreifen, indem Sie diese Ressourcen markieren und einen Drilldown in die Überwachungsoptionen durchführen. 

## <a name="examples-of-when-to-use-which-tool"></a>Beispiele für die Verwendung der einzelnen Tools 

In den folgenden Abschnitten werden einige grundlegende Szenarien beschreiben, und es wird ausgeführt, welche Tools zusammen verwendet werden sollten. 

### <a name="scenario-1--fix-errors-in-an-azure-application-under-development"></a>Szenario 1: Beheben von Fehlern in einer Azure-Anwendung in der Entwicklungsphase   

**Die beste Option besteht darin, Application Insights, Azure Monitor und Visual Studio gemeinsam zu verwenden.**

Azure bietet jetzt die volle Leistung des Visual Studio-Debuggers in der Cloud. Konfigurieren Sie Azure Monitor für das Senden von Telemetriedaten an Application Insights. Ermöglichen Sie in Visual Studio das Einschließen des Application Insights-SDK in Ihrer Anwendung. In Application Insights können Sie die Anwendungszuordnung verwenden, um visuell zu ermitteln, welche Teile der ausgeführten Anwendung fehlerhaft sind. Für die Teile, die nicht fehlerfrei sind, stehen bereits Fehler und Ausnahmen für die Untersuchung zur Verfügung. Sie können die verschiedenen Analysefunktionen in Application Insights für eine detailliertere Analyse verwenden. Wenn Sie sich über den Fehler nicht sicher sind, können Sie den Code mit dem Visual Studio-Debugger schrittweise durchlaufen und ein Problem weiter eingrenzen. 

Weitere Informationen finden Sie unter [Überwachen von Web-Apps](../application-insights/app-insights-azure-web-apps.md). Im Inhaltsverzeichnis auf der linken Seite finden Sie Anleitungen zu verschiedenen Arten von Apps und Sprachen.  

### <a name="scenario-2--debug-an-azure-net-web-application-for-errors-that-only-show-in-production"></a>Szenario 2: Debuggen einer Azure .NET-Webanwendung auf Fehler, die nur in der Produktion auftreten 

> [!NOTE]
> Diese Features befinden sich in der Vorschauphase. 

**Die beste Option besteht in der Verwendung von Application Insights und, wenn möglich, von Visual Studio, um eine vollständige Debuggingumgebung zu erhalten.**

Verwenden Sie den Application Insights-Momentaufnahmedebugger zum Debuggen Ihrer App. Tritt bei Produktionskomponenten ein bestimmter Fehlerschwellenwert auf, erfasst das System automatisch Telemetriedaten in Zeitfenstern, die als „Momentaufnahmen“ bezeichnet werden. Die erfasste Datenmenge ist für eine Produktionscloud sicher, da sie klein genug ist, um nicht die Leistung zu beeinträchtigen, aber groß genug, um die Ablaufverfolgung zu ermöglichen.  Das System kann mehrere Momentaufnahmen erfassen. Sie können sich im Azure-Portal einen bestimmten Zeitpunkt ansehen oder in Visual Studio die vollständige Umgebung nutzen. Mit Visual Studio können Entwickler diese Momentaufnahme so durchlaufen, als fände das Debuggen in Echtzeit statt. Lokale Variablen, Parameter, Speicher und Frames sind alle verfügbar. Den Entwicklern muss der Zugriff auf diese Produktionsdaten über eine RBAC-Rolle gewährt werden.  

Weitere Informationen finden Sie unter [Debuggen mit Momentaufnahmen](../application-insights/app-insights-snapshot-debugger.md). 

### <a name="scenario-3--debug-an-azure-application-that-uses-containers-or-microservices"></a>Szenario 3: Debuggen einer Azure-Anwendung, die Container oder Microservices verwendet 

**Identisch mit Szenario 1. Verwenden Sie Application Insights, Azure Monitor und Visual Studio gemeinsam.** Application Insights unterstützt auch das Sammeln von Telemetriedaten aus Prozessen, die innerhalb von Containern ausgeführt werden, und aus Microservices wie Kubernetes, Docker oder Azure Service Fabric. Weitere Informationen [finden Sie in diesem Video zum Debuggen von Containern und Microservices](https://go.microsoft.com/fwlink/?linkid=848184). 


### <a name="scenario-4--fix-performance-issues-in-your-azure-application"></a>Szenario 4: Beheben von Leistungsproblemen in Ihrer Azure-Anwendung

Der [Application Insights-Profiler](../application-insights/app-insights-profiler.md) erleichtert die Problembehandlung für diese Art von Problemen. Sie können Leistungsprobleme für Anwendungen erkennen und beheben, die in App Services (Web-Apps, Logic Apps, Mobile Apps, API-Apps) und anderen Computeressourcen wie Virtual Machines, VM-Skalierungsgruppen (VMSS), Cloud Services und Service Fabric ausgeführt werden. 

> [!NOTE]
> Die Möglichkeit zum Profilieren von Virtual Machines, VM-Skalierungsgruppen (VMSS), Cloud Services und Service Fabric befindet sich in der Vorschau.   

Darüber hinaus werden Sie durch das Tool „Intelligente Erkennung“ proaktiv per E-Mail über bestimmte Arten von Fehlern wie z.B. langsame Seitenladezeiten benachrichtigt.  Sie müssen an diesem Tool keine Konfiguration vornehmen. Weitere Informationen finden Sie unter [Intelligente Erkennung – Leistungsabweichungen](../application-insights/app-insights-proactive-performance-diagnostics.md).



## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen

* [Azure Monitor in einem Video von der Ignite 2016](https://myignite.microsoft.com/videos/4977)
* [Erste Schritte mit Azure Monitor](monitoring-get-started.md)
* [Azure-Diagnose](../azure-diagnostics.md) für die Diagnose von Problemen mit Clouddiensten, virtuellen Computern, VM-Skalierungsgruppen oder Service Fabric-Anwendungen.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) : Diagnostizieren Sie Probleme mit Ihrer App Service-Web-App.
* [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) und die [Operations Management Suite](https://www.microsoft.com/oms/)-Lösung zur Produktionsüberwachung
