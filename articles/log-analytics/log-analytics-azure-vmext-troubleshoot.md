---
title: Problembehandlung für die Azure Log Analytics-VM-Erweiterung | Microsoft-Dokumentation
description: Beschrieben werden die Symptome, Ursachen und Lösungen für die häufigsten Probleme, die bei der Log Analytics-Erweiterung für virtuelle Windows und Linux Azure-Computer auftreten.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/08/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 700d6b2c3bcd39aed38bf75556bcdcb59d1ab78b
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37128825"
---
# <a name="troubleshooting-the-log-analytics-vm-extension"></a>Problembehandlung für die Log Analytics-VM-Erweiterung
Dieser Artikel enthält Hilfe zur Problembehandlung von Fehlern, die im Zusammenhang mit der Log Analytics-VM-Erweiterung für virtuelle Windows- und Linux-Computer mit Microsoft Azure auftreten können, sowie Vorschläge für mögliche Problemlösungen.

Um den Status der Erweiterung zu überprüfen, führen Sie im Azure-Portal die folgenden Schritte aus.

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com) an.
2. Klicken Sie im Azure-Portal auf **Alle Dienste**. Geben Sie in der Ressourcenliste **Virtuelle Computer** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Virtuelle Computer** aus.
3. Suchen Sie in der Liste der virtuellen Computer nach dem entsprechenden Computer, und wählen Sie ihn aus.
3. Klicken Sie auf dem virtuellen Computer auf **Erweiterungen**.
4. Überprüfen Sie in der Liste, ob die Log Analytics-Erweiterung aktiviert oder deaktiviert ist.  Bei Linux wird der Agent als **OMSAgentforLinux** und bei Windows als **MicrosoftMonitoringAgent** aufgeführt.

   ![Ansicht der VM-Erweiterung](./media/log-analytics-azure-vmext-troubleshoot/log-analytics-vmview-extensions.png)

4. Klicken Sie auf die Erweiterung, um Details anzuzeigen. 

   ![Details der VM-Erweiterung](./media/log-analytics-azure-vmext-troubleshoot/log-analytics-vmview-extensiondetails.png)

## <a name="troubleshooting-azure-windows-vm-extension"></a>Problembehandlung für die Azure Windows-VM-Erweiterung

Wenn die VM-Erweiterung *MicrosoftMonitoringAgent* nicht installiert ist oder keine Berichte erstellt, können Sie die folgenden Schritte ausführen, um das Problem zu beheben.

1. Überprüfen Sie, ob der Azure-VM-Agent installiert ist und ordnungsgemäß funktioniert, indem Sie die Schritte unter [KB 2965986](https://support.microsoft.com/kb/2965986#mt1) ausführen.
   * Sie können auch die Protokolldatei `C:\WindowsAzure\logs\WaAppAgent.log` des VM-Agents überprüfen.
   * Wenn das Protokoll nicht vorhanden ist, wurde der VM-Agent nicht installiert.
   * [Installieren des Azure-VM-Agents](log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. Vergewissern Sie sich anhand der folgenden Schritte, dass die Taktaufgabe der Microsoft Monitoring Agent-Erweiterung ausgeführt wird:
   * Melden Sie sich beim virtuellen Computer an.
   * Öffnen Sie den Taskplaner, und suchen Sie die Aufgabe `update_azureoperationalinsight_agent_heartbeat`.
   * Vergewissern Sie sich, dass die Aufgabe aktiviert ist und jede Minute ausgeführt wird.
   * Überprüfen Sie die Taktprotokolldatei in `C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`.
3. Überprüfen Sie die Protokolldateien der Microsoft Monitoring Agent-VM-Erweiterung in `C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`.
4. Stellen Sie sicher, dass der virtuelle Computer PowerShell-Skripts ausführen kann.
5. Vergewissern Sie sich, dass die Berechtigungen für „C:\Windows\temp“ nicht geändert wurden.
6. Zeigen Sie den Status des Microsoft Monitoring Agents an, indem Sie Folgendes in einem PowerShell-Fenster mit erhöhten Rechten auf dem virtuellen Computer eingeben: `  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`.
7. Überprüfen Sie die Setupprotokolldateien des Microsoft Monitoring Agents in `C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`.

Weitere Informationen finden Sie unter [Behandeln von Problemen bei Windows-Erweiterungen](../virtual-machines/windows/extensions-oms.md).

## <a name="troubleshooting-linux-vm-extension"></a>Problembehandlung für die Linux-VM-Erweiterung
Wenn die VM-Erweiterung *OMSAgentforLinux* nicht installiert ist oder keine Berichte erstellt, können Sie die folgenden Schritte ausführen, um das Problem zu beheben.

1. Wenn der Status der Erweiterung *Unbekannt* lautet, überprüfen Sie, ob der Azure-VM-Agent installiert ist und ordnungsgemäß funktioniert. Dazu überprüfen Sie die Protokolldatei des VM-Agents: `/var/log/waagent.log`.
   * Wenn das Protokoll nicht vorhanden ist, wurde der VM-Agent nicht installiert.
   * [Installieren des Azure-VM-Agents auf Linux-VMs](log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. Bei einem anderen fehlerhaften Status überprüfen Sie die Protokolldateien der VM-Erweiterung OMS-Agent für Linux in `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` und `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`.
3. Wenn der Status der Erweiterung fehlerfrei ist, aber keine Daten hochgeladen werden, überprüfen Sie Protokolldateien des OMS-Agents für Linux in `/var/opt/microsoft/omsagent/log/omsagent.log`.

Weitere Informationen finden Sie unter [Behandeln von Problemen bei Linux-Erweiterungen](../virtual-machines/linux/extensions-oms.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Anleitungen zur Fehlerbehebung im Zusammenhang mit dem OMS-Agent für Linux, der auf Computern außerhalb von Azure gehostet wird, finden Sie unter [Problembehandlung für den Linux-Agent bei Azure Log Analytics](log-analytics-agent-linux-support.md).  
