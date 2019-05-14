---
title: Problembehandlung für Azure Statusmonitor v2 und bekannte Probleme | Microsoft-Dokumentation
description: Bekannte Probleme bei Statusmonitor v2 und Beispiele für die Problembehandlung Überwachen Sie die Websiteleistung ohne erneute Bereitstellung der Website. Funktioniert mit ASP.NET-Web-Apps, die lokal, auf virtuellen Computern oder in Azure gehostet werden.
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: f8e057bf2e71d5a00c1e2a8ef72a5c9415a5a207
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65145120"
---
# <a name="troubleshooting-status-monitor-v2"></a>Problembehandlung bei Statusmonitor v2

Wenn Sie die Überwachung aktivieren, können Probleme auftreten, die die Datensammlung verhindern. In diesem Dokument werden alle bekannten Probleme sowie Beispiele für die Problembehandlung aufgeführt.
Wenn Sie auf ein Problem stoßen, das nicht in diesem Artikel aufgeführt ist, können Sie uns [hier](https://github.com/Microsoft/ApplicationInsights-Home/issues) kontaktieren.


> [!IMPORTANT]
> Statusmonitor v2 ist derzeit als öffentliche Vorschauversion verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="known-issues"></a>Bekannte Probleme

### <a name="conflicting-dlls-in-an-applications-bin-directory"></a>In Konflikt stehende DLLs im Verzeichnis „bin“ einer Anwendung

Wenn eine der folgenden DLLs im Verzeichnis „bin“ vorhanden ist, kann die Überwachung fehlschlagen.

- Microsoft.ApplicationInsights.dll
- Microsoft.AspNet.TelemetryCorrelation.dll
- System.Diagnostics.DiagnosticSource.dll

Einige dieser DLLs sind in den Standardanwendungsvorlagen von Visual Studio enthalten, und zwar auch dann, wenn Ihre Anwendung diese nicht verwendet.
Bei der Verwendung von Tools zur Problembehandlung kann es zu dem folgenden symptomatischen Verhalten kommen:

- PerfView:
    ```
    ThreadID="7,500" 
    ProcessorNumber="0" 
    msg="Found 'System.Diagnostics.DiagnosticSource, Version=4.0.2.1, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' assembly, skipping attaching redfield binaries" 
    ExtVer="2.8.13.5972" 
    SubscriptionId="" 
    AppName="" 
    FormattedMessage="Found 'System.Diagnostics.DiagnosticSource, Version=4.0.2.1, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' assembly, skipping attaching redfield binaries" 
    ```

- iisreset + app load (KEINE TELEMETRIE). Führen Sie eine Untersuchung mit Sysinternals (Handle.exe und ListDLLs.exe) durch.
    ```
    .\handle64.exe -p w3wp | findstr /I "InstrumentationEngine AI. ApplicationInsights"
    E54: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll

    .\Listdlls64.exe w3wp | findstr /I "InstrumentationEngine AI ApplicationInsights"
    0x0000000009be0000  0x127000  C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll
    0x0000000009b90000  0x4f000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll
    0x0000000004d20000  0xb2000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.Extensions.Base_x64.dll
    ```

### <a name="conflict-with-iis-shared-configuration"></a>Konflikt mit IIS-Freigabekonfiguration

Wenn Sie über einen Cluster von Webservern verfügen, verwenden Sie möglicherweise eine [Freigabekonfiguration](https://docs.microsoft.com/iis/web-hosting/configuring-servers-in-the-windows-web-platform/shared-configuration_211). Unser HTTP-Modul kann nicht automatisch in diese Freigabekonfiguration eingefügt werden. Auf jedem Webserver muss zunächst der Enable-Befehl ausgeführt werden, um unsere DLL in dessen GAC zu installieren.

Nach dem Ausführen des Enable-Befehls: 
1. Navigieren Sie zum Verzeichnis Ihrer Freigabekonfiguration, und suchen Sie Ihre Datei `applicationHost.config`.
2. Fügen Sie Ihrer Konfiguration im Modulabschnitt folgende Zeile hinzu:
    ```
    <modules>
        <!-- Registered global managed http module handler. The 'Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll' must be installed in the GAC before this config is applied. -->
        <add name="ManagedHttpModuleHelper" type="Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.ManagedHttpModuleHelper, Microsoft.AppInsights.IIS.ManagedHttpModuleHelper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" preCondition="managedHandler,runtimeVersionv4.0" />
    </modules>
    ```
    


## <a name="troubleshooting"></a>Problembehandlung
    
### <a name="troubleshooting-powershell"></a>Problembehandlung bei PowerShell

#### <a name="how-to-inspect-what-modules-are-available"></a>Wie kann ich überprüfen, welche Module verfügbar sind?
Sie können die installierten Module mit dem folgenden Befehl überprüfen: `Get-Module -ListAvailable`

#### <a name="how-to-import-a-module-into-the-current-session"></a>Wie kann ich ein Modul in die aktuelle Sitzung importieren?
Wenn das Modul noch nicht in eine PowerShell-Sitzung geladen wurde, können Sie es manuell mit dem Befehl `Import-Module <path to psd1>` laden.


### <a name="troubleshooting-the-status-monitor-v2-module"></a>Problembehandlung beim Statusmonitor v2-Modul

#### <a name="how-to-review-what-commands-are-available-in-the-status-monitor-v2-module"></a>Wie kann ich überprüfen, welche Befehle im Statusmonitor v2-Modul verfügbar sind?
- Führen Sie den Befehl `Get-Command -Module Az.ApplicationMonitor` aus, um die verfügbaren Befehle anzuzeigen:

    ```
    CommandType     Name                                               Version    Source
    -----------     ----                                               -------    ------
    Cmdlet          Disable-ApplicationInsightsMonitoring              0.2.1      Az.ApplicationMonitor
    Cmdlet          Disable-InstrumentationEngine                      0.2.1      Az.ApplicationMonitor
    Cmdlet          Enable-ApplicationInsightsMonitoring               0.2.1      Az.ApplicationMonitor
    Cmdlet          Enable-InstrumentationEngine                       0.2.1      Az.ApplicationMonitor
    Cmdlet          Get-ApplicationInsightsMonitoringConfig            0.2.1      Az.ApplicationMonitor
    Cmdlet          Get-ApplicationInsightsMonitoringStatus            0.2.1      Az.ApplicationMonitor
    Cmdlet          Set-ApplicationInsightsMonitoringConfig            0.2.1      Az.ApplicationMonitor
    ```

#### <a name="what-is-the-current-version-of-the-status-monitor-v2-module"></a>Wie lautet die aktuelle Version des Statusmonitor v2-Moduls?
- Führen Sie den Befehl `Get-ApplicationInsightsMonitoringStatus` aus, um sich Informationen zu diesem Modul anzeigen zu lassen:

    ```
    PowerShell Module version:
    0.2.1-alpha

    Application Insights SDK version:
    2.9.0.3872

    Executing PowerShell Module Assembly:
    Microsoft.ApplicationInsights.Redfield.Configurator.PowerShell, Version=2.8.14.9129, Culture=neutral, PublicKeyToken=31bf3856ad364e35

    PowerShell Module Directory:
    C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\PowerShell

    Runtime Paths:
    ParentDirectory: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content Exists: True
    ConfigurationPath: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\applicationInsights.ikey.config Exists: False
    ManagedHttpModuleHelperPath: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\Runtime\Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll Exists: True
    RedfieldIISModulePath: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll Exists: True
    InstrumentationEngine86Path: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\Instrumentation32\MicrosoftInstrumentationEngine_x86.dll Exists: True
    InstrumentationEngine64Path: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll Exists: True
    InstrumentationEngineExtensionHost86Path: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\Instrumentation32\Microsoft.ApplicationInsights.ExtensionsHost_x86.dll Exists: True
    InstrumentationEngineExtensionHost64Path: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll Exists: True
    InstrumentationEngineExtensionConfig86Path: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\Instrumentation32\Microsoft.InstrumentationEngine.Extensions.config Exists: True
    InstrumentationEngineExtensionConfig64Path: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\Instrumentation64\Microsoft.InstrumentationEngine.Extensions.config Exists: True
    ApplicationInsightsSdkPath: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\Runtime\Microsoft.ApplicationInsights.dll Exists: True
    ```




### <a name="troubleshooting-running-processes"></a>Problembehandlung bei aktuell ausgeführten Prozessen

Sie können den Prozess auf dem instrumentierten Computer überprüfen, um herauszufinden, ob alle DLLs geladen wurden.
Wenn die Überwachung funktioniert, sollten mindestens 12 DLLS geladen worden sein.

- Befehl: `Get-ApplicationInsightsMonitoringStatus -InspectProcess`

    ```
    iisreset.exe /status
    Status for IIS Admin Service ( IISADMIN ) : Running
    Status for Windows Process Activation Service ( WAS ) : Running
    Status for Net.Msmq Listener Adapter ( NetMsmqActivator ) : Running
    Status for Net.Pipe Listener Adapter ( NetPipeActivator ) : Running
    Status for Net.Tcp Listener Adapter ( NetTcpActivator ) : Running
    Status for World Wide Web Publishing Service ( W3SVC ) : Running

    handle64.exe -accepteula -p w3wp
      BF0: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.ServerTelemetryChannel.dll
      C58: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.AzureAppServices.dll
      C68: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.DependencyCollector.dll
      C78: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.WindowsServer.dll
      C98: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.Web.dll
      CBC: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.PerfCounterCollector.dll
      DB0: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.Agent.Intercept.dll
      B98: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll
      BB4: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.Contracts.dll
      BCC: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.Redfield.Lightup.dll
      BE0: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.dll

    listdlls64.exe -accepteula w3wp
    0x0000000019ac0000  0x127000  C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll
    0x00000000198b0000  0x4f000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll
    0x000000000c460000  0xb2000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.Extensions.Base_x64.dll
    0x000000000ad60000  0x108000  C:\Windows\TEMP\2.4.0.0.Microsoft.ApplicationInsights.Extensions.Intercept_x64.dll
    ```

### <a name="collect-etw-logs-with-perfview"></a>Sammeln von ETW-Protokollen mit PerfView

#### <a name="setup"></a>Einrichtung

1. Laden Sie PerfView.exe und PerfView64.exe aus https://github.com/Microsoft/perfview/releases herunter.
2. Starten Sie PerfView64.exe.
3. Zeigen Sie „Erweitere Optionen“ an.
4. Deaktivieren Sie Folgendes:
    - ZIP
    - Merge
    - .NET-Symbolsammlung
5. Legen Sie zusätzliche Anbieter fest: `61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,925fa42b-9ef6-5fa7-10b8-56449d7a2040,f7d60e07-e910-5aca-bdd2-9de45b46c560,7c739bb9-7861-412e-ba50-bf30d95eae36,61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,252e28f4-43f9-5771-197a-e8c7e750a984`


#### <a name="collecting-logs"></a>Sammeln von Protokollen

1. Führen Sie in einer Befehlszeilenkonsole mit Administratorrechten `iisreset /stop` aus, um IIS und alle Webanwendungen zu deaktivieren.
2. Klicken Sie in PerfView auf „Sammlung starten“.
3. Führen Sie in einer Befehlszeilenkonsole mit Administratorrechten `iisreset /start` aus, um IIS zu starten.
4. Versuchen Sie, zu Ihrer App zu navigieren.
5. Kehren Sie nach dem Laden Ihrer App zu PerfView zurück, und klicken Sie auf „Sammlung beenden“.



## <a name="next-steps"></a>Nächste Schritte

- Beachten Sie unsere [API-Referenz](status-monitor-v2-overview.md#powershell-api-reference), um einen Parameter zu finden, den Sie möglicherweise übersehen haben.
- Wenn Sie auf ein Problem stoßen, das nicht in diesem Artikel aufgeführt ist, können Sie uns [hier](https://github.com/Microsoft/ApplicationInsights-Home/issues) kontaktieren.
