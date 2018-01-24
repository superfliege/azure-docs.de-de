---
title: "Problembehandlung für den Azure Log Analytics-Linux-Agent | Microsoft-Dokumentation"
description: "Beschrieben werden die Symptome, Ursachen und Lösungen für die häufigsten Probleme, die beim Log Analytics-Linux-Agent auftreten."
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
ms.date: 12/17/2017
ms.author: magoedte
ms.openlocfilehash: 5f598da9b82b4425ca509a26a2e6e366ba4c3394
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2017
---
# <a name="how-to-troubleshoot-issues-with-the-linux-agent-for-log-analytics"></a>Behandeln von Problemen beim Linux-Agent für Log Analytics

Dieser Artikel bietet Unterstützung bei der Problembehandlung von Fehlern, die beim Linux-Agent für Log Analytics auftreten können, und enthält mögliche Lösungen zum Beheben dieser Fehler.

## <a name="issue-unable-to-connect-through-proxy-to-log-analytics"></a>Problem: keine Verbindung über den Proxy mit Log Analytics möglich

### <a name="probable-causes"></a>Mögliche Ursachen
* Der während der Integration angegebene Proxy war falsch.
* Die Dienstendpunkte für Log Analytics und Azure Automation sind nicht in der Whitelist in Ihrem Rechenzentrum enthalten. 

### <a name="resolutions"></a>Lösungen
1. Führen Sie das Onboarding des Log Analytics-Diensts mit dem OMS-Agent für Linux erneut durch, indem Sie dem folgenden Befehl mit der Option `-v` ausführen. Dadurch wird die ausführliche Ausgabe des Agents über eine Proxy-Verbindung an den OMS-Dienst möglich. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. Lesen Sie den Abschnitt [Konfigurieren des Agents für die Verwendung mit einem Proxyserver oder OMS-Gateway](#configuring the-agent-for-use-with-a-proxy-server-or-oms-gateway), um sicherzustellen, dass der Agent ordnungsgemäß für die Kommunikation über einen Proxyserver konfiguriert wurde.    
* Vergewissern Sie sich erneut, dass die folgenden Log Analytics-Dienstendpunkte in der Whitelist enthalten sind:

    |Agent-Ressource| Ports |  
    |------|---------|  
    |*.ods.opinsights.azure.com | Port 443|   
    |*.oms.opinsights.azure.com | Port 443|   
    |ods.systemcenteradvisor.com | Port 443|   
    |*.blob.core.windows.net/ | Port 443|   

## <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Problem: Beim Versuch der Integration erhalten Sie einen 403-Fehler.

### <a name="probable-causes"></a>Mögliche Ursachen
* Das Datum und die Uhrzeit auf dem Linux-Server sind falsch. 
* Die Arbeitsbereichs-ID und der Arbeitsbereichsschlüssel sind falsch.

### <a name="resolution"></a>Lösung

1. Überprüfen Sie die Uhrzeit auf dem Linux-Server mit dem Befehl „date“. Wenn die Zeit um +/-15 Minuten von der aktuellen Uhrzeit abweicht ist, tritt bei der Integration ein Fehler auf. Aktualisieren Sie zur Behebung dieses Problems das Datum und/oder die Zeitzone des Linux-Servers. 
2. Überprüfen Sie, ob die neueste Version des OMS-Agents für Linux installiert ist.  Die neueste Version benachrichtigt Sie nun darüber, ob der Onboardingfehler durch die Zeitabweichung verursacht wird.
3. Führen Sie das Onboarding gemäß den obigen Installationsanweisungen in diesem Thema mit korrekten Daten für die Arbeitsbereich-ID und den Arbeitsbereichsschlüssel erneut durch.

## <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Problem: In der Protokolldatei sind direkt nach der Integration die Fehler 500 und 404 enthalten.
Dies ist ein bekanntes Problem, das beim ersten Hochladen von Linux-Daten in einen Log Analytics-Arbeitsbereich auftritt. Gesendete Daten und die Ausführung des Diensts sind davon nicht betroffen.

## <a name="issue-you-are-not-seeing-any-data-in-the-azure-portal"></a>Problem: Im Azure-Portal werden keine Daten angezeigt.

### <a name="probable-causes"></a>Mögliche Ursachen

- Fehler beim Onboarding zum Log Analytics-Dienst
- Die Verbindung mit dem Log Analytics-Dienst ist blockiert.
- Der OMS-Agent für Linux-Daten ist überlastet.

### <a name="resolutions"></a>Lösungen
1. Überprüfen Sie, ob die Integration des Log Analytics-Diensts erfolgreich war, indem Sie überprüfen, ob die folgende Datei vorhanden ist: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Erneutes Onboarding mithilfe der Befehlszeilenanweisungen `omsadmin.sh`
3. Wenn Sie einen Proxy verwenden, lesen Sie die Proxylösungsschritte weiter oben.
4. In einigen Fällen, wenn der OMS-Agent für Linux nicht mit dem Dienst kommunizieren kann, werden Daten auf dem Agent bis zur vollständigen Puffergröße von 50 MB in eine Warteschlange gestellt. Der OMS-Agent für Linux sollte über den folgenden Befehl neu gestartet werden: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Dieses Problem wurde ab der Agent-Version 1.1.0-28 behoben.

