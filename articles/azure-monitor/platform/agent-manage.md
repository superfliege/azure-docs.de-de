---
title: Verwalten des Azure Log Analytics-Agents | Microsoft Dokumentation
description: In diesem Artikel werden die verschiedenen Verwaltungsaufgaben beschrieben, die Sie typischerweise während des Lebenszyklus des Microsoft Monitoring Agents (MMA) ausführen, der auf einem Computer bereitgestellt wird.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/30/2018
ms.author: magoedte
ms.openlocfilehash: 0680140d9f4f4e77b5029c30c37f4c531652c6f2
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2019
ms.locfileid: "54119117"
---
# <a name="managing-and-maintaining-the-log-analytics-agent-for-windows-and-linux"></a>Verwalten und Warten des Log Analytics-Agents für Windows und Linux

Nach der erstmaligen Bereitstellung des Windows- oder Linux-Agents für Log Analytics müssen Sie den Agent neu konfigurieren oder ihn vom Computer entfernen, wenn er in seinem Lebenszyklus die Deaktivierungsphase erreicht hat. Sie können diese Routinewartungsaufgaben einfach manuell oder durch Automatisierung bewältigen, wodurch sowohl Betriebsfehler als auch Kosten reduziert werden.

## <a name="adding-or-removing-a-workspace"></a>Hinzufügen oder Entfernen von Arbeitsbereichen

### <a name="windows-agent"></a>Windows-Agent

#### <a name="update-settings-from-control-panel"></a>Aktualisieren von Einstellungen über die Systemsteuerung

1. Melden Sie sich auf dem Computer mit einem Konto an, das über Administratorrechte verfügt.
2. Öffnen Sie die **Systemsteuerung**.
3. Wählen Sie **Microsoft Monitoring Agent** aus, und klicken Sie dann auf die Registerkarte **Azure Log Analytics**.
4. Wenn Sie einen Arbeitsbereich entfernen, wählen Sie ihn aus, und klicken Sie dann auf **Entfernen**. Wiederholen Sie diesen Schritt für alle anderen Arbeitsbereiche, für die der Agent keine Berichte mehr erstellen soll.
5. Wenn Sie einen Arbeitsbereich hinzufügen, klicken Sie auf **Hinzufügen**. Fügen Sie dann im Dialogfeld **Log Analytics-Arbeitsbereich hinzufügen** die Arbeitsbereichs-ID und den Arbeitsbereichsschlüssel (Primärschlüssel) ein. Wenn der Computer in einen Log Analytics-Arbeitsbereich in Azure Government Cloud melden soll, wählen Sie „Azure US-Regierung“ aus der Dropdownliste „Azure Cloud“ aus.
6. Klicken Sie zum Speichern der Änderungen auf **OK** .

#### <a name="remove-a-workspace-using-powershell"></a>Entfernen eines Arbeitsbereichs mithilfe von PowerShell

```PowerShell
$workspaceId = "<Your workspace Id>"
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.RemoveCloudWorkspace($workspaceId)
$mma.ReloadConfiguration()
```

#### <a name="add-a-workspace-in-azure-commercial-using-powershell"></a>Hinzufügen eines Arbeitsbereichs in Azure Commercial mithilfe von PowerShell

```PowerShell
$workspaceId = "<Your workspace Id>"
$workspaceKey = "<Your workspace Key>"
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

#### <a name="add-a-workspace-in-azure-for-us-government-using-powershell"></a>Hinzufügen eines Arbeitsbereichs in Azure für US-Regierungsbehörden mithilfe von PowerShell

```PowerShell
$workspaceId = "<Your workspace Id>"
$workspaceKey = "<Your workspace Key>"
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
>Wenn Sie bereits die Befehlszeile oder Skripts zum Installieren oder Konfigurieren des Agents verwendet haben, sollten Sie wissen, dass `EnableAzureOperationalInsights` durch `AddCloudWorkspace` und `RemoveCloudWorkspace` ersetzt wurde.
>

### <a name="linux-agent"></a>Linux-Agent
Die folgenden Schritte veranschaulichen, wie Sie den Linux-Agent neu konfigurieren, wenn Sie sich entscheiden, ihn mit einem anderen Arbeitsbereich zu registrieren, oder einen Arbeitsbereich aus seiner Konfiguration entfernen möchten.

1. Um sicherzustellen, dass er bei einem Arbeitsbereich registriert ist, führen Sie den folgenden Befehl aus:

    `/opt/microsoft/omsagent/bin/omsadmin.sh -l`

    Es sollte ungefähr folgender Status zurückgegeben werden:

    `Primary Workspace: <workspaceId>   Status: Onboarded(OMSAgent Running)`

    Es ist wichtig, dass der Status auch zeigt, dass der Agent ausgeführt wird, da andernfalls die folgenden Schritte zum Neukonfigurieren des Agents nicht erfolgreich abgeschlossen werden.

2. Wenn er bereits mit einem Arbeitsbereich registriert ist, entfernen Sie den registrierten Arbeitsbereich mit dem folgenden Befehl. Falls er noch nicht registriert ist, fahren Sie mit dem nächsten Schritt fort.

    `/opt/microsoft/omsagent/bin/omsadmin.sh -X`

3. Führen Sie zur Registrierung bei einem anderen Arbeitsbereich den folgenden Befehl aus:

    `/opt/microsoft/omsagent/bin/omsadmin.sh -w <workspace id> -s <shared key> [-d <top level domain>]`
    
4. Führen Sie den folgenden Befehl aus, um sicherzustellen, dass Ihre Änderungen wirksam sind:

    `/opt/microsoft/omsagent/bin/omsadmin.sh -l`

    Es sollte ungefähr folgender Status zurückgegeben werden:

    `Primary Workspace: <workspaceId>   Status: Onboarded(OMSAgent Running)`

Der Agent-Dienst muss nicht neu gestartet werden, damit die Änderungen wirksam werden.

## <a name="update-proxy-settings"></a>Aktualisieren der Proxyeinstellungen
Verwenden Sie eine der folgenden Methoden, um den Agent so zu konfigurieren, dass er mit dem Dienst nach der Bereitstellung über einen Proxyserver oder ein [Log Analytics-Gateway](gateway.md) kommuniziert.

### <a name="windows-agent"></a>Windows-Agent

#### <a name="update-settings-using-control-panel"></a>Aktualisieren von Einstellungen über die Systemsteuerung

1. Melden Sie sich auf dem Computer mit einem Konto an, das über Administratorrechte verfügt.
2. Öffnen Sie die **Systemsteuerung**.
3. Wählen Sie **Microsoft Monitoring Agent** aus, und klicken Sie dann auf die Registerkarte **Proxyeinstellungen**.
4. Klicken Sie auf **Proxyserver verwenden**, und geben Sie die URL und die Portnummer des Proxyservers oder Gateways an. Wenn der Proxyserver oder das Log Analytics-Gateway eine Authentifizierung erfordert, geben Sie den Benutzernamen und das Kennwort für die Authentifizierung ein, und klicken Sie dann auf **OK**.

#### <a name="update-settings-using-powershell"></a>Aktualisieren von Einstellungen mithilfe von PowerShell

Kopieren Sie den folgenden PowerShell-Beispielcode, aktualisieren Sie ihn mit umgebungsspezifischen Informationen, und speichern Sie ihn mit einer PS1-Erweiterung. Führen Sie das Skript auf jedem Computer aus, der sich direkt mit dem Log Analytics-Dienst verbindet.

```PowerShell
param($ProxyDomainName="https://proxy.contoso.com:30443", $cred=(Get-Credential))

# First we get the Health Service configuration object. We need to determine if we
#have the right update rollup with the API we need. If not, no need to run the rest of the script.
$healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

$proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

if (!$proxyMethod)
{
    Write-Output 'Health Service proxy API not present, will not update settings.'
    return
}

Write-Output "Clearing proxy settings."
$healthServiceSettings.SetProxyInfo('', '', '')

$ProxyUserName = $cred.username

Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
$healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)
```

### <a name="linux-agent"></a>Linux-Agent
Führen Sie die folgenden Schritte aus, wenn Ihre Linux-Computer über einen Proxyserver oder ein Log Analytics-Gateway kommunizieren müssen. Der Wert für die Proxykonfiguration weist die folgende Syntax auf: `[protocol://][user:password@]proxyhost[:port]`. Für die *proxyhost*-Eigenschaft kann ein vollqualifizierter Domänenname oder eine IP-Adresse des Proxyservers verwendet werden.

1. Bearbeiten Sie die Datei `/etc/opt/microsoft/omsagent/proxy.conf`, indem Sie die folgenden Befehle ausführen. Ändern Sie die Werte, damit Sie Ihren spezifischen Einstellungen entsprechen.

    ```
    proxyconf="https://proxyuser:proxypassword@proxyserver01:30443"
    sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
    sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf
    ```

2. Starten Sie den Agent neu, indem Sie den folgenden Befehl ausführen:

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ```

## <a name="uninstall-agent"></a>Uninstall agent
Verwenden Sie eines der folgenden Verfahren, um den Windows- oder Linux-Agent über die Befehlszeile oder mithilfe des Setup-Assistenten zu deinstallieren.

### <a name="windows-agent"></a>Windows-Agent

#### <a name="uninstall-from-control-panel"></a>Deinstallieren über die Systemsteuerung
1. Melden Sie sich auf dem Computer mit einem Konto an, das über Administratorrechte verfügt.
2. Klicken Sie in der **Systemsteuerung** auf **Programs und Features**.
3. Klicken Sie in **Programme und Features** auf die Option **Microsoft Monitoring Agent**, dann auf **Deinstallieren** und anschließend auf **Ja**.

>[!NOTE]
>Der Agent-Setup-Assistent kann auch durch Doppelklicken auf die Datei **MMASetup-\<Plattform\>.exe** ausgeführt werden, die aus einem Arbeitsbereich im Azure-Portal heruntergeladen werden kann.

#### <a name="uninstall-from-the-command-line"></a>Deinstallieren über die Befehlszeile
Die heruntergeladene Datei für den Agent ist ein eigenständiges Installationspaket, das mit IExpress erstellt wurde. Das Setup-Programm für den Agent und die unterstützenden Dateien sind im Paket enthalten und müssen extrahiert werden, um eine ordnungsgemäße Deinstallation mit der im folgenden Beispiel gezeigten Befehlszeile durchführen zu können.

1. Melden Sie sich auf dem Computer mit einem Konto an, das über Administratorrechte verfügt.
2. Um die Installationsdateien des Agent zu extrahieren, führen Sie `extract MMASetup-<platform>.exe` von einer Eingabeaufforderung mit erhöhten Rechten aus. Sie werden dann nach dem Pfad gefragt, in den die Dateien extrahiert werden sollen. Alternativ können Sie den Pfad angeben, indem Sie die Argumente `extract MMASetup-<platform>.exe /c:<Path> /t:<Path>` übergeben. Weitere Informationen zu den von IExpress unterstützten Befehlszeilenswitches finden Sie unter [Befehlszeilenoptionen für IExpress-Softwareupdatepakete](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages). Sie können anschließend das Beispiel aktualisieren, um es an Ihre Bedürfnisse anzupassen.
3. Geben Sie an der Eingabeaufforderung `%WinDir%\System32\msiexec.exe /x <Path>:\MOMAgent.msi /qb` ein.

### <a name="linux-agent"></a>Linux-Agent
Führen Sie den folgenden Befehl auf dem Linux-Computer aus, um den Agent zu entfernen. Mit dem Argument *--purge* werden der Agent und die dazugehörige Konfiguration vollständig entfernt.

   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh --purge`

## <a name="configure-agent-to-report-to-an-operations-manager-management-group"></a>Konfigurieren von Agents zum Berichten an eine Operations Manager-Verwaltungsgruppe

### <a name="windows-agent"></a>Windows-Agent
Führen Sie die folgenden Schritte aus, um den Log Analytics-Agent für Windows so zu konfigurieren, dass Meldungen an eine System Center Operations Manager-Verwaltungsgruppe gesendet werden.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

1. Melden Sie sich auf dem Computer mit einem Konto an, das über Administratorrechte verfügt.
2. Öffnen Sie die **Systemsteuerung**.
3. Klicken Sie auf **Microsoft Monitoring Agent** und dann auf die Registerkarte **Operations Manager**.
4. Wenn Ihre Operations Manager-Server über eine Integration mit Active Directory verfügen, klicken Sie auf **Verwaltungsgruppenzuweisungen aus AD DS automatisch aktualisieren**.
5. Klicken Sie auf **Hinzufügen**, um das Dialogfeld **Verwaltungsgruppe hinzufügen** zu öffnen.
6. Geben Sie im Feld **Verwaltungsgruppenname** den Namen Ihrer Verwaltungsgruppe ein.
7. Geben Sie im Feld **Primärer Verwaltungsserver** den Computernamen des primären Verwaltungsservers ein.
8. Geben Sie im Feld **Verwaltungsserverport** die TCP-Portnummer ein.
9. Wählen Sie unter **Agentaktionskonto**das lokale Systemkonto oder ein lokales Domänenkonto aus.
10. Klicken Sie zum Schließen des Dialogfelds **Verwaltungsgruppe hinzufügen** auf **OK**. Klicken Sie anschließend zum Schließen des Dialogfelds **Eigenschaften von Microsoft Monitoring Agent** auf **OK**.

### <a name="linux-agent"></a>Linux-Agent
Führen Sie die folgenden Schritte aus, um den Log Analytics-Agent für Linux so zu konfigurieren, dass Meldungen an eine System Center Operations Manager-Verwaltungsgruppe gesendet werden.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

1. Bearbeiten Sie die Datei `/etc/opt/omi/conf/omiserver.conf`
2. Stellen Sie sicher, dass die Zeile, die mit `httpsport=` beginnt, den Port 1270 definiert. Beispiel: `httpsport=1270`
3. Starten Sie den OMI-Server neu: `sudo /opt/omi/bin/service_control restart`

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie [Problembehandlung für den Linux-Agent](agent-linux-troubleshoot.md), wenn bei der Installation oder Verwaltung des Agent Probleme auftreten.
