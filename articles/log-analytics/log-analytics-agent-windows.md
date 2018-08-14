---
title: Verbinden von Windows-Computern mit Azure Log Analytics | Microsoft Docs
description: In diesem Artikel wird beschrieben, wie Windows-Computer, die in anderen Clouds oder lokal gehostet werden, über den Microsoft Monitoring Agent (MMA) mit Log Analytics verbunden werden können.
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
ms.date: 03/12/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 975b3d30f242b877512312f8a5a030bde9349287
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003646"
---
# <a name="connect-windows-computers-to-the-log-analytics-service-in-azure"></a>Verbinden von Windows-Computern mit dem Log Analytics-Dienst in Azure

Zum Überwachen und Verwalten virtueller oder physischer Computer in Ihrem lokalen Rechenzentrum oder einer anderen Cloudumgebung mit Log Analytics müssen Sie den Microsoft Monitoring Agent (MMA) bereitstellen und so konfigurieren, dass Berichte an einen oder mehrere Log Analytics-Arbeitsbereiche übermittelt werden.  Der Agent unterstützt auch die Rolle „Hybrid Runbook Worker“ für Azure Automation.  

Der Agent ist auf einem überwachten Windows-Computer als Microsoft Monitoring Agent-Dienst aufgeführt. Der Microsoft Monitoring Agent-Dienst sammelt Ereignisse aus Protokolldateien und dem Windows-Ereignisprotokoll sowie Leistungsdaten und andere Telemetriedaten. Selbst wenn der Agent nicht mit dem Log Analytics-Dienst kommunizieren kann, an den er Berichte übermittelt, wird er weiterhin ausgeführt und stellt die gesammelten Daten in die Warteschlange auf dem Datenträger des überwachten Computers. Wenn die Verbindung wiederhergestellt wird, sendet der Microsoft Monitoring Agent-Dienst die gesammelten Daten an den Dienst.

Der Agent kann mit einer der folgenden Methoden installiert werden. Bei den meisten Installationen wird zum Installieren verschiedener Gruppen von Computern je nach Bedarf eine Kombination dieser Methoden verwendet.  Details zur Verwendung der einzelnen Methoden finden Sie weiter unten im Artikel.

* Manuelle Installation: Das Setup wird mithilfe des Setup-Assistenten über die Befehlszeile auf dem Computer installiert oder mithilfe eines vorhandenen Tools zur Softwareverteilung bereitgestellt.
* Azure Automation Desired State Configuration (DSC): Dabei wird DSC in Azure Automation mit einem Skript für Windows-Computer verwendet, die bereits in Ihrer Umgebung bereitgestellt sind.  
* PowerShell-Skript:
* Resource Manager-Vorlage für virtuelle Computer, auf denen Windows lokal in Azure Stack ausgeführt wird.  

Informationen zur unterstützten Konfiguration finden Sie in den Abschnitten zu [unterstützten Windows-Betriebssystemen](log-analytics-concept-hybrid.md#supported-windows-operating-systems) und zur [Netzwerkfirewallkonfiguration](log-analytics-concept-hybrid.md#network-firewall-requirements).

## <a name="obtain-workspace-id-and-key"></a>Abrufen von Arbeitsbereichs-ID und -Schlüssel
Vor der Installation von Microsoft Monitoring Agent für Windows benötigen Sie die Arbeitsbereichs-ID und den Schlüssel für Ihren Log Analytics-Arbeitsbereich.  Diese Informationen sind mit jeder Installationsmethode beim Setup erforderlich, um den Agent ordnungsgemäß zu konfigurieren und sicherzustellen, dass er erfolgreich mit Log Analytics in der kommerziellen Azure-Cloud und der US Government-Cloud kommunizieren kann.  

1. Klicken Sie im Azure-Portal auf **Alle Dienste**. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.
2. Wählen Sie in der Liste der Log Analytics-Arbeitsbereiche den Arbeitsbereich aus, für den der Agent für die Übermittlung von Berichten konfiguriert werden soll.
3. Wählen Sie **Erweiterte Einstellungen**.<br><br> ![Erweiterte Einstellungen für Log Analytics](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Wählen Sie **Verbundene Quellen** und dann **Windows Server** aus.   
5. Kopieren Sie die **Arbeitsbereichs-ID** und den **Primärschlüssel**, und fügen Sie diese Angaben in Ihren bevorzugten Texteditor ein.    
   
## <a name="install-the-agent-using-setup-wizard"></a>Installieren des Agents mithilfe des Setup-Assistenten
Mit den folgenden Schritten wird der Agent für Log Analytics in Azure und Azure Government Cloud mithilfe des Setup-Assistenten für Microsoft Monitoring Agent auf Ihrem Computer installiert und konfiguriert.  

1. Wählen Sie in Ihrem Log Analyics-Arbeitsbereich auf der Seite **Windows-Server**, auf die Sie zuvor navigiert sind, die entsprechende Version für **Windows-Agent herunterladen** aus, um den Download abhängig von der Prozessorarchitektur des Windows-Betriebssystems auszuführen.   
2. Führen Sie Setup aus, um den Agent auf Ihrem Computer zu installieren.
2. Klicken Sie auf der Seite **Willkommen**auf **Weiter**.
3. Lesen Sie die Seite **Lizenzbedingungen** durch, und klicken Sie anschließend auf **Ich stimme zu**.
4. Auf der Seite **Zielordner** können Sie den Standardinstallationsordner entweder ändern oder beibehalten. Klicken Sie anschließend auf **Weiter**.
5. Wählen Sie auf der Seite **Agent-Setupoptionen** aus, dass der Agent mit Azure Log Analytics (OMS) verbunden wird, und klicken Sie dann auf **Weiter**.   
6. Führen Sie auf der Seite **Azure Log Analytics** die folgenden Schritte aus:
   1. Fügen Sie die **Arbeitsbereichs-ID** und den **Arbeitsbereichsschlüssel (Primärschlüssel)** ein, die Sie zuvor kopiert haben.  Wenn der Computer in einen Log Analytics-Arbeitsbereich in Azure Government Cloud melden soll, wählen Sie **Azure US-Regierung** aus der Dropdownliste **Azure Cloud** aus.  
   2. Wenn der Computer über einen Proxyserver mit dem Log Analytics-Dienst kommunizieren muss, klicken Sie auf **Erweitert**, und stellen Sie die URL sowie die Portnummer des Proxyservers bereit.  Wenn der Proxyserver eine Authentifizierung erfordert, geben Sie den Benutzernamen und das Kennwort für die Authentifizierung mit dem Proxyserver ein, und klicken Sie dann auf **Weiter**.  
7. Klicken Sie auf **Weiter**, nachdem Sie die Bereitstellung der erforderlichen Konfigurationseinstellungen abgeschlossen haben.<br><br> ![Arbeitsbereichs-ID und Primärschlüssel einfügen](media/log-analytics-quick-collect-windows-computer/log-analytics-mma-setup-laworkspace.png)<br><br>
8. Überprüfen Sie Ihre Auswahl auf der Seite **Bereit zum Installieren**, und klicken Sie dann auf **Installieren**.
9. Klicken Sie auf der Seite **Die Konfiguration wurde erfolgreich abgeschlossen** auf **Fertig stellen**.

Nach Abschluss wird der **Microsoft Monitoring Agent** in der **Systemsteuerung** angezeigt. Informationen zum Überprüfen, ob er Berichte an Log Analytics übermittelt, finden Sie unter [Überprüfen der Agent-Konnektivität mit Log Analytics](#verify-agent-connectivity-to-log-analytics). 

## <a name="install-the-agent-using-the-command-line"></a>Installieren des Agents über die Befehlszeile
Die heruntergeladene Datei für den Agent ist ein eigenständiges Installationspaket.  Das Setupprogramm für den Agent und die unterstützenden Dateien sind im Paket enthalten und müssen extrahiert werden, um eine ordnungsgemäße Installation über die in den folgenden Beispielen gezeigte Befehlszeile durchführen zu können.    

>[!NOTE]
>Wenn Sie einen Agent aktualisieren möchten, müssen Sie die Skripting-API für Log Analytics verwenden. Weitere Informationen finden Sie im Thema [Verwalten und Warten des Log Analytics-Agents für Windows und Linux](log-analytics-agent-manage.md).

In der folgenden Tabelle sind die spezifischen Log Analytics-Parameter aufgeführt, die im Setup (auch bei der Bereitstellung mit Automation DSC) für den Agent unterstützt werden.

|MMA-spezifische Optionen                   |Notizen         |
|---------------------------------------|--------------|
| NOAPM=1                               | Dieser Parameter ist optional. Installiert den Agent ohne .NET-Anwendungsleistungsüberwachung.|   
|ADD_OPINSIGHTS_WORKSPACE               | 1 = Konfigurieren des Agents, sodass Berichte an einen Arbeitsbereich übermittelt werden                |
|OPINSIGHTS_WORKSPACE_ID                | Arbeitsbereichs-ID (GUID) für den hinzuzufügenden Arbeitsbereich                    |
|OPINSIGHTS_WORKSPACE_KEY               | Arbeitsbereichsschlüssel, der für die erste Authentifizierung beim Arbeitsbereich verwendet wird |
|OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE  | Angabe der Cloudumgebung, in der sich der Arbeitsbereich befindet <br> 0 = Azure Commercial Cloud (Standard) <br> 1 = Azure Government |
|OPINSIGHTS_PROXY_URL               | URI für den zu verwendenden Proxy |
|OPINSIGHTS_PROXY_USERNAME               | Benutzername für den Zugriff auf einen authentifizierten Proxy |
|OPINSIGHTS_PROXY_PASSWORD               | Kennwort für den Zugriff auf einen authentifizierten Proxy |

1. Um die Installationsdateien des Agent zu extrahieren, führen Sie `MMASetup-<platform>.exe /c` von einer Eingabeaufforderung mit erhöhten Rechten aus. Sie werden dann nach dem Pfad gefragt, in den die Dateien extrahiert werden sollen.  Alternativ können Sie den Pfad angeben, indem Sie die Argumente `MMASetup-<platform>.exe /c /t:<Path>` übergeben.  
2. Für die Installation des Agents im Hintergrund und seine Konfiguration zum Übermitteln von Berichten an einen Arbeitsbereich in der Azure Commercial-Cloud extrahieren Sie die Setupdateien aus dem Ordner und geben Folgendes ein: 
   
     ```dos
    setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=0 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
    ```

   Zum Konfigurieren des Agents zum Übermitteln von Berichten an die Azure US Government-Cloud geben Sie Folgendes ein: 

     ```dos
    setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
    ```

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Installieren des Agents mithilfe von DSC in Azure Automation

Sie können das folgende Skriptbeispiel verwenden, um den Agent mit Azure Automation DSC zu installieren.   Wenn Sie über kein Automation-Konto verfügen, finden Sie unter [Erste Schritte mit Azure Automation](../automation/automation-offering-get-started.md) Informationen zu den Anforderungen und Schritten zum Erstellen eines Automation-Kontos, die vor der Verwendung von Automation DSC erforderlich sind.  Wenn Sie nicht mit Automation DSC vertraut sind, finden Sie entsprechende Informationen unter [Erste Schritte mit Azure Automation DSC](../automation/automation-dsc-getting-started.md).

Im folgenden Beispiel wird der vom `URI`-Wert identifizierte 64-Bit-Agent installiert. Sie können auch die 32-Bit-Version installieren, indem Sie den URI-Wert ersetzen. Die URIs für beide Versionen lauten:

- Windows-64-Bit-Agent: https://go.microsoft.com/fwlink/?LinkId=828603
- Windows-32-Bit-Agent: https://go.microsoft.com/fwlink/?LinkId=828604


>[!NOTE]
>Mit diesem Verfahren und dem Skriptbeispiel kann der bereits auf einem Windows-Computer bereitgestellte Agent nicht aktualisiert werden.

Die 32-Bit- und die 64-Bit-Version des Agent-Pakets weisen unterschiedliche Produktcodes auf. Auch neu veröffentlichten Versionen ist ein eindeutiger Wert zugewiesen.  Der Produktcode ist eine GUID, die als hauptsächliche Identifikation einer Anwendung oder eines Produkts dient und durch die Windows Installer-Eigenschaft **ProductCode** angegeben wird.  `ProductId value` im Skript **MMAgent.ps1** muss mit dem Produktcode aus dem Installationspaket für den 32-Bit- oder 64-Bit-Agent übereinstimmen.

Den Produktcode können Sie mithilfe von „Orca.exe“ aus [Windows SDK Components for Windows Installer Developers](https://msdn.microsoft.com/library/windows/desktop/aa370834%28v=vs.85%29.aspx), bei dem es sich um eine Komponente des Windows Software Development Kit handelt, oder über PowerShell entsprechend einem von einem MVP (Microsoft Valuable Professional) geschriebenen [Beispielskript](http://www.scconfigmgr.com/2014/08/22/how-to-get-msi-file-information-with-powershell/) direkt aus dem Installationspaket für den Agent abrufen.  Für beide Vorgehensweisen müssen Sie zuerst die Datei **MOMAgent.msi** aus dem MMASetup-Installationspaket extrahieren.  Dies wird weiter oben im ersten Schritt im Abschnitt [Installieren des Agents über die Befehlszeile](#install-the-agent-using-the-command-line) gezeigt.  

1. Importieren Sie das DSC-Modul „xPSDesiredStateConfiguration“ aus [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) in Azure Automation.  
2.  Erstellen Sie Variablenassets in Azure Automation für *OPSINSIGHTS_WS_ID* und *OPSINSIGHTS_WS_KEY*. Legen Sie für *OPSINSIGHTS_WS_ID* Ihre Log Analytics-Arbeitsbereichs-ID und für *OPSINSIGHTS_WS_KEY* den Primärschlüssel Ihres Arbeitsbereichs fest.
3.  Kopieren Sie das Skript, und speichern Sie es unter „MMAgent.ps1“.

    ```PowerShell
    Configuration MMAgent
    {
        $OIPackageLocalPath = "C:\Deploy\MMASetup-AMD64.exe"
        $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
        $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"

        Import-DscResource -ModuleName xPSDesiredStateConfiguration
        Import-DscResource -ModuleName PSDesiredStateConfiguration

        Node OMSnode {
            Service OIService
            {
                Name = "HealthService"
                State = "Running"
                DependsOn = "[Package]OI"
            }

            xRemoteFile OIPackage {
                Uri = "https://go.microsoft.com/fwlink/?LinkId=828603"
                DestinationPath = $OIPackageLocalPath
            }

            Package OI {
                Ensure = "Present"
                Path  = $OIPackageLocalPath
                Name = "Microsoft Monitoring Agent"
                ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
                Arguments = '/C:"setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
                DependsOn = "[xRemoteFile]OIPackage"
            }
        }
    }

    ```

4. [Importieren Sie das Konfigurationsskript „MMAgent.ps1“](../automation/automation-dsc-getting-started.md#importing-a-configuration-into-azure-automation) in Ihr Automation-Konto. 
5. [Weisen Sie einen Windows-Computer oder -Knoten](../automation/automation-dsc-getting-started.md#onboarding-an-azure-vm-for-management-with-azure-automation-state-configuration) der Konfiguration hinzu. Innerhalb von 15 Minuten prüft der Knoten die Konfiguration, und der Agent wird auf den Knoten gepusht.

## <a name="verify-agent-connectivity-to-log-analytics"></a>Überprüfen der Agent-Konnektivität mit Log Analytics

Nachdem die Installation des Agents abgeschlossen ist, kann auf zwei Arten überprüft werden, ob er verbunden ist und Berichte übermittelt.  

Suchen Sie auf dem Computer in der **Systemsteuerung** das Element **Microsoft Monitoring Agent**.  Wählen Sie das Element aus. Auf der Registerkarte **Azure Log Analytics (OMS)** sollte für den Agent die folgende Meldung angezeigt werden: **The Microsoft Monitoring Agent has successfully connected to the Microsoft Operations Management Suite service.** (Für den Microsoft Monitoring Agent wurde die Verbindung mit dem Microsoft Operations Management Suite-Dienst erfolgreich hergestellt.)<br><br> ![MMA-Verbindungsstatus mit Log Analytics](media/log-analytics-quick-collect-windows-computer/log-analytics-mma-laworkspace-status.png)

Außerdem können Sie eine einfache Protokollsuche im Azure-Portal durchführen.  

1. Klicken Sie im Azure-Portal auf **Alle Dienste**. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.  
2. Wählen Sie auf der Seite der Log Analytics-Arbeitsbereiche den Zielarbeitsbereich und dann die Kachel **Protokollsuche** aus. 
2. Geben Sie im Bereich „Protokollsuche“ im Abfragefeld Folgendes ein:  

    ```
    search * 
    | where Type == "Heartbeat" 
    | where Category == "Direct Agent" 
    | where TimeGenerated > ago(30m)  
    ```

In den zurückgegebenen Suchergebnissen sollten Heartbeat-Datensätze für den Computer angezeigt werden, die angeben, dass dieser verbunden ist und Berichte an den Dienst übermittelt.   

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Verwaltung des Agents während seines Bereitstellungslebenszyklus auf Ihren Computern finden Sie unter [Verwalten und Warten des Log Analytics-Agents für Windows und Linux](log-analytics-agent-manage.md).  
