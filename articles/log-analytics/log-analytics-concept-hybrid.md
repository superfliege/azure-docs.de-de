---
title: Sammeln von Daten in einer Hybridumgebung mit dem Azure Log Analytics-Agent | Microsoft-Dokumentation
description: In diesem Thema erfahren Sie, wie Sie mit Log Analytics entsprechende Daten sammeln und Computer überwachen, die in Ihrer lokalen oder anderen Cloudumgebung gehostet werden.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: b50e6de9de6498f57fd9b599c45a35f2367b84ba
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51012138"
---
# <a name="collect-data-in-a-hybrid-environment-with-log-analytics-agent"></a>Sammeln von Daten in einer Hybridumgebung mit dem Log Analytics-Agent

Azure Log Analytics kann Daten von Computern mit dem Windows- oder Linux-Betriebssystem, die an folgenden Orten ausgeführt werden, erfassen und Aktionen dafür durchführen:

* In [Azure Virtual Machines](log-analytics-quick-collect-azurevm.md), indem die Log Analytics-VM-Erweiterung verwendet wird 
* In Ihrem Rechenzentrum als physischer Server oder virtueller Computer
* Auf virtuellen Computern in einem in der Cloud gehosteten Dienst wie Amazon Web Services (AWS)

In Ihrer Umgebung gehostete Computer können direkt mit Log Analytics verbunden werden. Wenn Sie diese Computer bereits mit System Center Operations Manager 2012 R2 oder höher überwachen, können Sie Ihre Operations Manager-Verwaltungsgruppe auch in Log Analytics integrieren und die Prozesse Ihrer IT-Dienstvorgänge weiterführen.  

## <a name="overview"></a>Übersicht

![log-analytics-agent-direct-connect-diagram](media/log-analytics-concept-hybrid/log-analytics-on-prem-comms.png)

Bevor Sie die gesammelten Daten analysieren und bearbeiten können, müssen Sie zunächst Agents für alle Computer installieren und diese verbinden, die Daten an den Log Analytics-Dienst senden sollen. Sie können Agents über das Setup, eine Befehlszeile oder durch eine Konfiguration des gewünschten Zustands in Azure Automation auf Ihren lokalen Computern installieren. 

Der Agent für Linux und Windows kommuniziert in ausgehender Richtung über TCP-Port 443 mit dem Log Analytics-Dienst. Wenn der Computer für die Kommunikation über das Internet eine Verbindung mit einer Firewall oder einem Proxyserver herstellt, sollten Sie sich die unten angegebenen Anforderungen ansehen, um sich mit der erforderlichen Netzwerkkonfiguration vertraut zu machen.  Wenn Ihre IT-Sicherheitsrichtlinien es nicht zulassen, dass Computer im Netzwerk eine Verbindung mit dem Internet herstellen, können Sie ein [Log Analytics-Gateway](log-analytics-oms-gateway.md) einrichten und dann den Agent so konfigurieren, dass er die Verbindung mit Log Analytics über das Gateway herstellt. Der Agent kann dann Konfigurationsinformationen empfangen und Daten senden, die je nach aktivierten Datensammlungsregeln und -lösungen gesammelt werden. 

Wenn Sie den Computer mit System Center Operations Manager 2012 R2 oder höher überwachen, kann er mit dem Log Analytics-Dienst mehrfach vernetzt werden, um Daten zu sammeln und an den Dienst weiterzuleiten. Hierbei kann er weiterhin von [Operations Manager](log-analytics-om-agents.md) überwacht werden. Linux-Computer, die durch eine in Log Analytics integrierte Operations Manager-Verwaltungsgruppe überwacht werden, empfangen keine Konfiguration für Datenquellen und weitergeleitete Daten über die Verwaltungsgruppe. Der Windows-Agent kann bis zu vier Arbeitsbereiche melden, während der Linux-Agent nur die Berichterstellung für einen einzelnen Arbeitsbereich unterstützt.  

Der Agent für Linux und Windows dient nicht nur für die Verbindung mit Log Analytics, sondern unterstützt auch Azure Automation, um die Hybrid Runbook-Workerrolle und Verwaltungslösungen wie Änderungsnachverfolgung und Updateverwaltung zu hosten.  Weitere Informationen zur Hybrid Runbook Workerrolle finden Sie unter [Azure Automation Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md).  

## <a name="supported-windows-operating-systems"></a>Unterstützte Windows-Betriebssysteme
Die folgenden Versionen des Windows-Betriebssystems werden für den Windows-Agent offiziell unterstützt:

* Windows Server 2008 Service Pack 1 (SP1) oder höher
* Windows 7 SP1 und höher

## <a name="supported-linux-operating-systems"></a>Unterstützte Linux-Betriebssysteme
Dieser Abschnitt enthält Details zu den unterstützten Linux-Distributionen.    

Beginnend mit den nach August 2018 veröffentlichten Versionen gelten folgende Änderungen für unser Supportmodell:  

* Es werden nur die Serverversionen und keine Clientversionen unterstützt.  
* Neue Versionen der [von Azure unterstützten Linux-Distributionen](../virtual-machines/linux/endorsed-distros.md) werden immer unterstützt.  
* Alle Nebenversionen werden für jede aufgeführte Hauptversion unterstützt.
* Versionen, für die der Support des Herstellers abgelaufen ist, werden nicht unterstützt.  
* Neue Versionen von AMI werden nicht unterstützt.  
* Nur Versionen, die standardmäßig SSL 1.x ausführen, werden unterstützt.

Wenn Sie eine Distribution oder eine Version verwenden, die derzeit nicht unterstützt wird und nicht auf unser Supportmodell abgestimmt ist, wird empfohlen, dieses Repository zu forken und dabei anzuerkennen, dass der Microsoft-Support bei geforkten Agent-Versionen keine Unterstützung bietet.

* Amazon Linux 2017.09 (x64)
* CentOS Linux 6 (x86/x64) und 7 (x64)  
* Oracle Linux 6 und 7 (x86/x64) 
* Red Hat Enterprise Linux Server 6 (x86/x64) und 7 (x64)
* Debian GNU/Linux 8 und 9 (x86/x64)
* Ubuntu 14.04 LTS (x86/x64), 16.04 LTS (x86/x64) und 18.04 LTS (x64)
* SUSE Linux Enterprise Server 12 (x64)

>[!NOTE]
>OpenSSL 1.1.0 wird nur auf x86_x64-Plattformen (64 Bit) unterstützt. OpenSSL vor Version 1.x wird auf keiner Plattform unterstützt.
>

## <a name="tls-12-protocol"></a>TLS 1.2-Protokoll
Um die Sicherheit von Daten bei der Übertragung an Log Analytics sicherzustellen, wird dringend empfohlen, den Agent so zu konfigurieren, dass mindestens Transport Layer Security (TLS) 1.2 verwendet wird. Bei älteren Versionen von TLS/Secure Sockets Layer (SSL) wurde ein Sicherheitsrisiko festgestellt. Sie funktionieren aus Gründen der Abwärtskompatibilität zwar noch, werden jedoch **nicht empfohlen**.  Weitere Informationen finden Sie unter [Senden von Daten über TLS 1.2](log-analytics-data-security.md#sending-data-securely-using-tls-12). 

## <a name="network-firewall-requirements"></a>Netzwerkfirewallanforderungen
Die Aufstellung unten enthält die Proxy- und Firewall-Konfigurationsinformationen, die der Linux- und Windows-Agent benötigt, um mit Log Analytics zu kommunizieren.  

|Agent-Ressource|Ports |Richtung |Umgehung der HTTPS-Überprüfung|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Port 443 |Eingehend und ausgehend|JA |  
|*.oms.opinsights.azure.com |Port 443 |Eingehend und ausgehend|JA |  
|*.blob.core.windows.net |Port 443 |Eingehend und ausgehend|JA |  
|*.azure-automation.net |Port 443 |Eingehend und ausgehend|JA |  


Wenn Sie planen, den Azure Automation Hybrid Runbook Worker zu verwenden, um eine Verbindung zum Automatisierungsdienst herzustellen und sich bei diesem zu registrieren, um Runbooks in Ihrer Umgebung zu verwenden, muss dieser Zugriff auf die Portnummer und die unter [Konfigurieren Ihres Netzwerks für den Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md#network-planning) beschriebenen URLs besitzen. 

Der Windows- und Linux-Agent unterstützt die Kommunikation mit dem Log Analytics-Dienst über einen Proxyserver oder ein Log Analytics-Gateway mithilfe des HTTPS-Protokolls.  Es wird sowohl die anonyme als auch die Standardauthentifizierung (Benutzername und Kennwort) unterstützt.  Für den Windows-Agent, der direkt mit dem Dienst verbunden ist, wird die Proxykonfiguration während der Installation oder [nach der Bereitstellung](log-analytics-agent-manage.md#update-proxy-settings) über die Systemsteuerung oder mit PowerShell angegeben.  

Für den Linux-Agent wird der Proxyserver während der Installation oder [nach der Installation](log-analytics-agent-manage.md#update-proxy-settings) durch Ändern der Konfigurationsdatei „proxy.conf“ angegeben.  Der Wert für die Proxykonfiguration des Linux-Agents weist die folgende Syntax auf:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Wenn Ihr Proxyserver keine Authentifizierung erfordert, muss der Linux-Agent trotzdem einen Pseudo-Benutzernamen und -Kennwort angeben. Dies kann ein beliebiger Benutzername oder ein beliebiges Kennwort sein.

|Eigenschaft| BESCHREIBUNG |
|--------|-------------|
|Protokoll | https |
|user | Optionaler Benutzername für die Proxyauthentifizierung |
|password | Optionales Kennwort für die Proxyauthentifizierung |
|proxyhost | Adresse oder FQDN des Proxyservers oder Log Analytics-Gateways |
|port | Optionale Portnummer für den Proxyserver oder das Log Analytics-Gateway |

Beispiel: `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Wenn Sie in Ihrem Kennwort Sonderzeichen wie „\@“ verwenden, erhalten Sie einen Proxyverbindungsfehler, da der Wert falsch analysiert wird.  Zur Umgehung dieses Problems codieren Sie das Kennwort in der URL mit einem Tool wie [URLDecode](https://www.urldecoder.org/).  

## <a name="install-and-configure-agent"></a>Installieren und Konfigurieren des Agents 
Die direkte Verbindung Ihrer lokalen Computer mit Log Analytics kann je nach Ihren Anforderungen mithilfe verschiedener Methoden durchgeführt werden. Die folgende Tabelle hebt die einzelnen Methoden hervor, um festzustellen, welche Methode in Ihrer Organisation am besten funktioniert.

|Quelle | Methode | BESCHREIBUNG|
|-------|-------------|-------------|
| Windows-Computer|- [Manuelle Installation](log-analytics-agent-windows.md)<br>- [Azure Automation DSC](log-analytics-agent-windows.md#install-the-agent-using-dsc-in-azure-automation)<br>- [Resource Manager-Vorlage mit Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) |Installieren Sie den Microsoft Monitoring Agent über die Befehlszeile oder mit einer automatisierten Methode wie Azure Automation DSC, [System Center Configuration Manager](https://docs.microsoft.com/sccm/apps/deploy-use/deploy-applications) oder mit einer Azure Resource Manager-Vorlage, wenn Sie Microsoft Azure Stack in Ihrem Rechenzentrum bereitgestellt haben.| 
|Linux-Computer| [Manuelle Installation](log-analytics-quick-collect-linux-computer.md)|Installieren Sie den Agent für Linux durch Aufrufen eines Wrapperskripts, das auf GitHub gehostet wird. | 
| System Center Operations Manager|[Integrieren von Operations Manager mit Log Analytics](log-analytics-om-agents.md) | Konfigurieren Sie die Integration zwischen Operations Manager und Log Analytics, um die gesammelten Daten von Linux- und Windows-Computern weiterzuleiten, die Berichte für eine Verwaltungsgruppe erstellen.|  

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie [Datenquellen](log-analytics-data-sources.md), um die Datenquellen zu verstehen, die für die Erfassung von Daten aus Ihrem Windows- oder Linux-System zur Verfügung stehen. 

* Erfahren Sie mehr über [Protokollsuchvorgänge](log-analytics-queries.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten. 

* Erfahren Sie mehr über [Lösungen](../monitoring/monitoring-solutions.md), die Log Analytics um zusätzliche Funktionen erweitern und ebenfalls Daten für den Log Analytics-Arbeitsbereich sammeln.
