---
title: Sammeln von Datei aus Ihrer Umgebung mit Azure Log Analytics | Microsoft-Dokumentation
description: "In diesem Thema erfahren Sie, wie Sie mit Log Analytics entsprechende Daten sammeln und Computer überwachen, die in Ihrer lokalen oder anderen Cloudumgebung gehostet werden."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: magoedte
ms.openlocfilehash: 85fde471f0d99b976e319d552c6a031d63854cf4
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2018
---
# <a name="collect-data-from-computers-in-your-environment-with-log-analytics"></a>Sammeln von Daten von Computern in Ihrer Umgebung mit Log Analytics

Azure Log Analytics kann Daten von Windows- oder Linux-Computern sammeln und darauf reagieren, die sich an folgenden Positionen befinden:

* In [Azure Virtual Machines](log-analytics-quick-collect-azurevm.md), indem die Log Analytics-VM-Erweiterung verwendet wird 
* In Ihrem Rechenzentrum als physischer Server oder virtueller Computer
* Auf virtuellen Computern in einem in der Cloud gehosteten Dienst wie Amazon Web Services (AWS)

In Ihrer Umgebung gehostete Computer können direkt mit Log Analytics verbunden werden. Wenn Sie diese Computer bereits mit System Center Operations Manager 2012 R2 oder 2016 überwachen, können Sie Ihre Operations Manager-Verwaltungsgruppe auch in Log Analytics integrieren und Ihre Prozesse und Strategie für Dienstvorgänge weiterführen.  

## <a name="overview"></a>Übersicht

![log-analytics-agent-direct-connect-diagram](media/log-analytics-concept-hybrid/log-analytics-on-prem-comms.png)

Bevor Sie die gesammelten Daten analysieren und bearbeiten können, müssen Sie zunächst Agents für alle Computer installieren und diese verbinden, die Daten an den Log Analytics-Dienst senden sollen. Sie können Agents über das Setup, eine Befehlszeile oder durch eine Konfiguration des gewünschten Zustands in Azure Automation auf Ihren lokalen Computern installieren. 

Der Agent für Linux und Windows kommuniziert ausgehend über TCP-Port 443 mit dem Log Analytics-Dienst. Wenn der Computer für die Kommunikation über das Internet eine Verbindung mit einer Firewall oder einem Proxyserver herstellt, finden Sie unter [Konfigurieren des Agents für die Verwendung mit einem Proxyserver oder dem OMS-Gateway](#configuring-the-agent-for-use-with-a-proxy-server-or-oms-gateway) weitere Informationen zu den Konfigurationsänderungen, die angewendet werden müssen. Wenn Sie den Computer mit System Center 2016 – Operations Manager oder Operations Manager 2012 R2 überwachen, kann er mit dem Log Analytics-Dienst mehrfach vernetzt werden, um Daten zu sammeln und an den Dienst weiterzuleiten, wobei er weiterhin von [Operations Manager](log-analytics-om-agents.md) überwacht werden kann. Linux-Computer, die durch eine in Log Analytics integrierte Operations Manager-Verwaltungsgruppe überwacht werden, empfangen keine Konfiguration für Datenquellen und weitergeleitete Daten über die Verwaltungsgruppe. Der Windows-Agent kann bis zu vier Arbeitsbereiche melden, während der Linux-Agent nur die Berichterstellung für einen einzelnen Arbeitsbereich unterstützt.  

Der Agent für Linux und Windows dient nicht nur zur Anbindung an Log Analytics, sondern unterstützt auch die Anbindung an Azure Automation, um die Hybrid Runbook Workerrolle und Verwaltungslösungen wie Änderungsnachverfolgung und Updateverwaltung zu hosten.  Weitere Informationen zur Hybrid Runbook Workerrolle finden Sie unter [Azure Automation Hybrid Runbook Worker](../automation/automation-offering-get-started.md#automation-architecture-overview).  

Sofern nach Ihren IT-Sicherheitsrichtlinien Computer in Ihrem Netzwerk keine Internetverbindung herstellen dürfen, kann der Agent so konfiguriert werden, dass er eine Verbindung mit dem OMS-Gateway herstellt, um Konfigurationsinformationen zu empfangen und gesammelte Daten abhängig von der aktivierten Lösung zu senden. Weitere Informationen und Anleitungen zum Konfigurieren Ihres Linux- oder Windows-Agents für die Kommunikation mit dem Log Analytics-Dienst über ein OMS-Gateway finden Sie unter [Verbinden von Computern mit OMS über das OMS-Gateway](log-analytics-oms-gateway.md). 

> [!NOTE]
> Der Agent für Windows unterstützt nur Transport Layer Security (TLS) 1.0 und 1.1.  
> 

## <a name="prerequisites"></a>Voraussetzungen
Prüfen Sie zunächst anhand der folgenden Informationen, ob die Mindestanforderungen des Systems erfüllt sind.

### <a name="windows-operating-system"></a>Windows-Betriebssystem
Die folgenden Versionen des Windows-Betriebssystems werden für den Windows-Agent offiziell unterstützt:

* Windows Server 2008 Service Pack 1 (SP1) oder höher
* Windows 7 SP1 und höher

#### <a name="network-configuration"></a>Network Configuration
Die Aufstellung unten enthält die Proxy- und Firewall-Konfigurationsinformationen, die der Windows-Agent benötigt, um mit Log Analytics zu kommunizieren. Ausgehender Datenverkehr von Ihrem Netzwerk zum Log Analytics-Dienst. 

| Agent-Ressource | Ports | Umgehung der HTTPS-Überprüfung|
|----------------|-------|------------------------|
|*.ods.opinsights.azure.com |443 | Ja |
|*.oms.opinsights.azure.com | 443 | Ja | 
|*.blob.core.windows.net | 443 | Ja | 
|*.azure-automation.net | 443 | Ja | 

### <a name="linux-operating-systems"></a>Linux-Betriebssysteme
Die folgenden Linux-Distributionen werden offiziell unterstützt.  Der Linux-Agent kann jedoch auch auf anderen Distributionen ausgeführt werden, die hier nicht aufgeführt sind.  Sofern nicht anders angegeben, werden alle Nebenversionen für jede aufgeführte Hauptversion unterstützt.  

* Amazon Linux 2012.09 bis 2015.09 (x86/x64)
* CentOS Linux 5, 6 und 7 (x86/x64)  
* Oracle Linux 5, 6 und 7 (x86/x64) 
* Red Hat Enterprise Linux Server 5, 6 und 7 (x86/x64)
* Debian GNU/Linux 6, 7 und 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 und 12 (x86/x64)

#### <a name="network-configuration"></a>Network Configuration
Die Aufstellung unten enthält die Proxy- und Firewall-Konfigurationsinformationen, die der Linux-Agent benötigt, um mit Log Analytics zu kommunizieren. Ausgehender Datenverkehr von Ihrem Netzwerk zum Log Analytics-Dienst. 

|Agent-Ressource| Ports |  
|------|---------|  
|*.ods.opinsights.azure.com | Port 443|   
|*.oms.opinsights.azure.com | Port 443|   
|*.blob.core.windows.net | Port 443|   
|*.azure-automation.net | Port 443|  

Der Linux-Agent unterstützt die Kommunikation mit dem Log Analytics-Dienst über einen Proxyserver oder ein OMS-Gateway mithilfe des HTTPS-Protokolls.  Es wird sowohl die anonyme als auch die Standardauthentifizierung (Benutzername und Kennwort) unterstützt.  Der Proxyserver kann während der Installation oder durch Ändern der Konfigurationsdatei „proxy.conf“ nach der Installation angegeben werden.  

Der Wert für die Proxykonfiguration weist die folgende Syntax auf:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Wenn Ihr Proxyserver keine Authentifizierung erfordert, muss der Linux-Agent trotzdem einen Pseudo-Benutzernamen und -Kennwort angeben. Dies kann ein beliebiger Benutzername oder ein beliebiges Kennwort sein.

|Eigenschaft| BESCHREIBUNG |
|--------|-------------|
|Protokoll | https |
|user | Optionaler Benutzername für die Proxyauthentifizierung |
|password | Optionales Kennwort für die Proxyauthentifizierung |
|proxyhost | Adresse oder FQDN des Proxyservers/OMS-Gateways |
|port | Optionale Portnummer für den Proxyserver/das OMS-Gateway |

Beispiel: `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Wenn Sie in Ihrem Kennwort Sonderzeichen wie „@“ verwenden, erhalten Sie einen Proxyverbindungsfehler, da der Wert falsch analysiert wird.  Zur Umgehung dieses Problems codieren Sie das Kennwort in der URL mit einem Tool wie [URLDecode](https://www.urldecoder.org/).  

## <a name="install-and-configure-agent"></a>Installieren und Konfigurieren des Agents 
Die direkte Verbindung Ihrer lokalen Computer mit Log Analytics kann je nach Ihren Anforderungen mithilfe verschiedener Methoden durchgeführt werden. Die folgende Tabelle hebt die einzelnen Methoden hervor, um festzustellen, welche Methode in Ihrer Organisation am besten funktioniert.

|Quelle | Methode | BESCHREIBUNG|
|-------|-------------|-------------|
| Windows-Computer|- [Manuelle Installation](log-analytics-agent-windows.md)<br>- [Azure Automation DSC](log-analytics-agent-windows.md#install-the-agent-using-dsc-in-azure-automation)<br>- [Resource Manager-Vorlage mit Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) |Installieren Sie den Microsoft Monitoring Agent über die Befehlszeile oder mit einer automatisierten Methode wie Azure Automation DSC, [System Center Configuration Manager](https://docs.microsoft.com/sccm/apps/deploy-use/deploy-applications) oder mit einer Azure Resource Manager-Vorlage, wenn Sie Microsoft Azure Stack in Ihrem Rechenzentrum bereitgestellt haben.| 
|Linux-Computer| [Manuelle Installation](log-analytics-quick-collect-linux-computer.md)|Installieren Sie den Agent für Linux durch Aufrufen eines Wrapperskripts, das auf GitHub gehostet wird. | 
| System Center Operations Manager|[Integrieren von Operations Manager mit Log Analytics](log-analytics-om-agents.md) | Konfigurieren Sie die Integration zwischen Operations Manager und Log Analytics, um die gesammelten Daten von Linux- und Windows-Computern weiterzuleiten, die Berichte für eine Verwaltungsgruppe erstellen.|  

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie [Datenquellen](log-analytics-data-sources.md), um die Datenquellen zu verstehen, die für die Erfassung von Daten aus Ihrem Windows- oder Linux-System zur Verfügung stehen. 

* Erfahren Sie mehr über [Protokollsuchvorgänge](log-analytics-log-searches.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten. 

* Erfahren Sie mehr über [Lösungen](log-analytics-add-solutions.md) , die Log Analytics um zusätzliche Funktionen erweitern und ebenfalls Daten für das OMS-Repository sammeln.