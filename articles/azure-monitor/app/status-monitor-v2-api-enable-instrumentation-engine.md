---
title: 'API-Referenz für Azure-Statusmonitor v2: Aktivieren der Instrumentierungs-Engine | Microsoft-Dokumentation'
description: 'API-Referenz für Statusmonitor v2: Enable-InstrumentationEngine Überwachen der Websiteleistung ohne erneute Bereitstellung der Website Funktioniert mit ASP.NET-Web-Apps, die lokal, auf virtuellen Computern oder in Azure gehostet werden.'
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
ms.openlocfilehash: e993378634262de25449975431c0a9e3145ca9fb
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2019
ms.locfileid: "66255242"
---
# <a name="status-monitor-v2-api-enable-instrumentationengine-v021-alpha"></a>API für Statusmonitor v2: Enable-InstrumentationEngine (v0.2.1-alpha)

In diesem Dokument wird ein Cmdlet beschrieben, das als Teil des [PowerShell-Moduls „Az.ApplicationMonitor“](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/) zur Verfügung gestellt wird.

> [!IMPORTANT]
> Statusmonitor v2 ist derzeit als öffentliche Vorschauversion verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="description"></a>BESCHREIBUNG

Mit diesem Cmdlet wird die Instrumentierungs-Engine durch Festlegen von Registrierungsschlüsseln aktiviert.
Starten Sie IIS neu, damit die Änderungen wirksam werden.

Die Instrumentierungs-Engine kann Daten ergänzen, die mit .NET SDKs gesammelt wurden.
Ereignisse und Nachrichten, die die Ausführung eines verwalteten Prozesses beschreiben, werden gesammelt. Dies schließt u. a. Abhängigkeitsergebniscodes, HTTP-Verben und SQL-Befehlstext ein. 

Aktivieren Sie die Instrumentierungs-Engine in den folgenden Fällen:
- Sie haben mithilfe des Cmdlets „Enable“ die Überwachung bereits aktiviert, jedoch nicht die Instrumentierungs-Engine.
- Sie haben mithilfe der .NET SDKs die Anwendung manuell instrumentiert und möchten zusätzliche Telemetriedaten sammeln.

> [!IMPORTANT] 
> Dieses Cmdlet erfordert eine PowerShell-Sitzung mit Administratorberechtigungen.

> [!NOTE] 
> Dieses Cmdlet erfordert, dass Sie unsere Lizenz- und Datenschutzbestimmungen durchlesen und akzeptieren.

> [!NOTE] 
> Die Instrumentierungs-Engine erfordert zusätzlichen Aufwand und ist standardmäßig deaktiviert.

## <a name="examples"></a>Beispiele

```powershell
PS C:\> Enable-InstrumentationEngine
```

## <a name="parameters"></a>Parameter 

### <a name="-acceptlicense"></a>-AcceptLicense
**Optional.** Verwenden Sie diesen Schalter, um die Lizenz- und Datenschutzbestimmungen in monitorlosen Installationen anzunehmen.

### <a name="-verbose"></a>-Verbose
**Allgemeiner Parameter.** Verwenden Sie diesen Parameter, um detaillierte Protokolle auszugeben.

## <a name="output"></a>Output


#### <a name="example-output-from-successfully-enabling-the-instrumentation-engine"></a>Beispielausgabe nach erfolgreicher Aktivierung der Instrumentierungs-Engine

```
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
```

## <a name="next-steps"></a>Nächste Schritte

  Anzeigen der Telemetrie:
 - [Untersuchen Sie Metriken](../../azure-monitor/app/metrics-explorer.md) für die Überwachung von Leistung und Auslastung.
- [Durchsuchen Sie Ereignisse und Protokolle](../../azure-monitor/app/diagnostic-search.md), um Probleme zu diagnostizieren.
- [Verwenden Sie Analytics](../../azure-monitor/app/analytics.md) für erweiterte Abfragen.
- [Erstellen Sie Dashboards.](../../azure-monitor/app/overview-dashboard.md)
 
 Hinzufügen weiterer Telemetrieelemente:
 - [Erstellen Sie Webtests](monitor-web-app-availability.md), um sicherzustellen, dass Ihre Website live bleibt.
- [Fügen Sie Webclient-Telemetriedaten hinzu](../../azure-monitor/app/javascript.md), um Ausnahmen von Webseitencode anzuzeigen und Ablaufverfolgungsaufrufe einfügen zu können.
- [Fügen Sie Ihrem Code das Application Insights SDK hinzu](../../azure-monitor/app/asp-net.md), um Ablaufverfolgungs- und Protokollaufrufe einfügen zu können.
 
 Weitere Möglichkeiten zur Verwendung von Statusmonitor v2:
 - Verwenden Sie unsere Anleitung für die [Problembehandlung](status-monitor-v2-troubleshoot.md) von Statusmonitor v2.
 - [Rufen Sie die Konfiguration ab](status-monitor-v2-api-get-config.md), um sicherzustellen, dass Ihre Einstellungen korrekt erfasst wurden.
 - [Rufen Sie den Status ab](status-monitor-v2-api-get-status.md), um die Überwachung zu überprüfen.
