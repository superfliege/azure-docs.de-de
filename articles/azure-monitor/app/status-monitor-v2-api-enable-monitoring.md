---
title: 'API-Referenz für Azure-Statusmonitor v2: Aktivieren der Überwachung | Microsoft-Dokumentation'
description: 'API-Referenz für Statusmonitor v2: Enable-ApplicationInsightsMonitoring. Überwachen Sie die Websiteleistung ohne erneute Bereitstellung der Website. Funktioniert mit ASP.NET-Web-Apps, die lokal, auf virtuellen Computern oder in Azure gehostet werden.'
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
ms.openlocfilehash: 17798c78b167821f5f30a11996ac90cf67fb0179
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65144945"
---
# <a name="status-monitor-v2-api-enable-applicationinsightsmonitoring-v021-alpha"></a>API für Statusmonitor v2: Enable-ApplicationInsightsMonitoring (v0.2.1-alpha)

In diesem Dokument wird ein Cmdlet beschrieben, das als Teil des [PowerShell-Moduls „Az.ApplicationMonitor“](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/) zur Verfügung gestellt wird.

> [!IMPORTANT]
> Statusmonitor v2 ist derzeit als öffentliche Vorschauversion verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="description"></a>BESCHREIBUNG

Aktivieren Sie die codefreie Anfügungsüberwachung von IIS-Anwendungen auf einem Zielcomputer.
Dieses Cmdlet ändert die IIS-Datei „applicationHost.config“ und legt einige Registrierungsschlüssel fest.
Dieses Cmdlet erstellt ferner die Datei „applicationinsights.ikey.config“, die definiert, welcher Instrumentierungsschlüssel von der Anwendung verwendet wird.
IIS lädt beim Start das RedfieldModule, welches das Application Insights SDK in Anwendungen einfügt, während diese Anwendungen gestartet werden.
Starten Sie IIS neu, damit die Änderungen wirksam werden.

Es wird empfohlen, nach der Aktivierung der Überwachung [Livemetriken](live-stream.md) zu verwenden, um schnell herauszufinden, ob Ihre Anwendung uns Telemetriedaten sendet.


> [!NOTE] 
> Für den Einstieg benötigen Sie einen Instrumentierungsschlüssel. Weitere Informationen finden unter [Erstellen einer neuen Ressource](create-new-resource.md#copy-the-instrumentation-key).


> [!IMPORTANT] 
> Dieses Cmdlet erfordert eine PowerShell-Sitzung mit Administratorrechten und einer Ausführungsrichtlinie mit höheren Rechten. Weitere Informationen finden Sie [hier](status-monitor-v2-detailed-instructions.md#run-powershell-as-administrator-with-an-elevated-execution-policy).

> [!NOTE] 
> Dieses Cmdlet erfordert, dass Sie unsere Lizenz- und Datenschutzbestimmungen durchlesen und akzeptieren.


## <a name="examples"></a>Beispiele

### <a name="example-with-single-instrumentation-key"></a>Beispiel mit einem einzigen Instrumentierungsschlüssel
In diesem Beispiel werden alle Anwendungen auf dem aktuellen Computer einem einzigen Instrumentierungsschlüssel zugewiesen.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### <a name="example-with-instrumentation-key-map"></a>Beispiel mit Instrumentierungsschlüsselzuordnung
In diesem Beispiel 
- findet `MachineFilter` den aktuellen Computer unter Verwendung des Platzhalters `'.*'`.
- stellt `AppFilter='WebAppExclude'` einen `null`-Instrumentierungsschlüssel bereit. Diese App wird nicht instrumentiert.
- weist `AppFilter='WebAppOne'` diese spezifische App einem eindeutigen Instrumentierungsschlüssel zu.
- weist `AppFilter='WebAppTwo'` ebenfalls diese spezifische App einem eindeutigen Instrumentierungsschlüssel zu.
- Abschließend verwendet `AppFilter` auch den Platzhalter `'.*'`, um alle anderen Web-Apps zu finden, die nicht von den früheren Regeln gefunden wurden, und weist einen Standard-Instrumentierungsschlüssel zu.
- Leerzeichen nur zur besseren Lesbarkeit hinzugefügt.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKeyMap 
    @(@{MachineFilter='.*';AppFilter='WebAppExclude'},
      @{MachineFilter='.*';AppFilter='WebAppOne';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1'},
      @{MachineFilter='.*';AppFilter='WebAppTwo';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2'},
      @{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault'})

```


## <a name="parameters"></a>Parameter 

### <a name="-instrumentationkey"></a>-InstrumentationKey
**Erforderlich.** Verwenden Sie diesen Parameter, um einen einzelnen iKey für die Verwendung durch alle Anwendungen auf dem Zielcomputer bereitzustellen.

### <a name="-instrumentationkeymap"></a>-InstrumentationKeyMap
**Erforderlich.** Verwenden Sie diesen Parameter, um mehrere iKeys sowie eine Zuordnung bereitzustellen, welche Apps welchen iKey verwenden. Sie können ein einzelnes Installationsskript für mehrere Computer erstellen, indem Sie den MachineFilter festlegen. 

> [!IMPORTANT] 
> Anwendungen werden anhand der Regeln in der Reihenfolge gesucht, in der diese Regeln bereitgestellt werden. Daher sollten Sie die spezifischsten Regeln zuerst und die generischsten Regeln zuletzt angeben.

#### <a name="schema"></a>Schema
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'})`

- **MachineFilter** ist ein erforderliches c#-RegEx des Computer- oder VM-Namens.
    - „.*“ findet alle
    - „ComputerName“ findet nur Computer mit exakt diesem Namen.
- **AppFilter** ist ein erforderliches c#-RegEx des Computer- oder VM-Namens.
    - „.*“ findet alle
    - „ApplicationName“ findet nur IIS-Anwendungen mit exakt diesem Namen.
- **InstrumentationKey** ist erforderlich, um die Überwachung der Anwendungen zu aktivieren, die den beiden obigen Filtern entsprechen.
    - Behalten Sie für diesen Wert Null bei, wenn Sie Regeln definieren möchten, um die Überwachung auszuschließen.


### <a name="-enableinstrumentationengine"></a>-EnableInstrumentationEngine
**Optional.** Verwenden Sie diesen Schalter, um die Instrumentierungs-Engine zu aktivieren, sodass sie Ereignisse und Nachrichten darüber sammelt, was während der Ausführung eines verwalteten Prozesses passiert. Dies betrifft unter anderem Abhängigkeitsergebniscodes, HTTP-Verben und SQL-Befehlstext. Die Instrumentierungs-Engine erfordert zusätzlichen Aufwand und ist standardmäßig deaktiviert.

### <a name="-acceptlicense"></a>-AcceptLicense
**Optional.** Verwenden Sie diesen Schalter, um die Lizenz- und Datenschutzbestimmungen in monitorlosen Installationen anzunehmen.

### <a name="-verbose"></a>-Verbose
**Allgemeiner Parameter.** Verwenden Sie diesen Parameter, um detaillierte Protokolle auszugeben.

### <a name="-whatif"></a>-WhatIf 
**Allgemeiner Parameter.** Verwenden Sie diesen Parameter, um Ihre Eingabeparameter zu testen und zu überprüfen, ohne die Überwachung tatsächlich zu aktivieren.

## <a name="output"></a>Output


#### <a name="example-output-from-a-successful-enablement"></a>Beispielausgabe für eine erfolgreiche Aktivierung

```
Initiating Disable Process
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-52z'
in :1,237
No element in the source document matches '/configuration/location[@path='']/system.webServer/modules/add[@name='ManagedHttpModuleHelper']'
Not executing RemoveAll (transform line 1, 546)
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'disable'
GAC Module will not be removed, since this operation might cause IIS instabilities
Configuring IIS Environment for codeless attach...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring IIS Environment for instrumentation engine...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring registry for instrumentation engine...
Successfully disabled Application Insights Status Monitor
Installing GAC module 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.0\content\Runtime\Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll'
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
Found GAC module Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.ManagedHttpModuleHelper, Microsoft.AppInsights.IIS.ManagedHttpModuleHelper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-52z_1'
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'enable'
Configuring IIS Environment for codeless attach...
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
Updating app pool permissions...
Successfully enabled Application Insights Status Monitor
```

## <a name="next-steps"></a>Nächste Schritte

  Anzeigen der Telemetrie:
 - [Untersuchen Sie Metriken](../../azure-monitor/app/metrics-explorer.md) für die Überwachung von Leistung und Auslastung.
- [Durchsuchen Sie Ereignisse und Protokolle](../../azure-monitor/app/diagnostic-search.md), um Probleme zu diagnostizieren.
- [Verwenden Sie Analytics](../../azure-monitor/app/analytics.md) für erweiterte Abfragen.
- [Erstellen Sie Dashboards.](../../azure-monitor/app/app-insights-dashboards.md)
 
 Hinzufügen weiterer Telemetrieelemente:
 - [Erstellen Sie Webtests](monitor-web-app-availability.md), um sicherzustellen, dass Ihre Website live bleibt.
- [Fügen Sie Webclient-Telemetriedaten hinzu](../../azure-monitor/app/javascript.md), um Ausnahmen von Webseitencode anzuzeigen und Ablaufverfolgungsaufrufe einfügen zu können.
- [Fügen Sie Ihrem Code das Application Insights SDK hinzu](../../azure-monitor/app/asp-net.md), um Ablaufverfolgungs- und Protokollaufrufe einfügen zu können.
 
 Weitere Möglichkeiten zur Verwendung von Statusmonitor v2:
 - Verwenden Sie unsere Anleitung für die [Problembehandlung](status-monitor-v2-troubleshoot.md) von Statusmonitor v2.
 - [Rufen Sie die Konfiguration ab](status-monitor-v2-api-get-config.md), um sicherzustellen, dass Ihre Einstellungen korrekt erfasst wurden.
 - [Rufen Sie den Status ab](status-monitor-v2-api-get-status.md), um die Überwachung zu überprüfen.
