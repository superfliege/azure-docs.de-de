---
title: "Bewährte Methoden und Problembehandlungsschritte für Node-Anwendungen in Azure-Web-Apps"
description: "Es werden die bewährten Methoden und Problembehandlungsschritte für Node-Anwendungen in Azure-Web-Apps beschrieben."
services: app-service\web
documentationcenter: nodejs
author: ranjithr
manager: wadeh
editor: 
ms.assetid: 387ea217-7910-4468-8987-9a1022a99bef
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 11/09/2017
ms.author: ranjithr
ms.openlocfilehash: f7f3186ba93670e566a10f97dde86a977101e8fc
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Bewährte Methoden und Problembehandlungsschritte für Node-Anwendungen in Azure-Web-Apps
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

In diesem Artikel werden die bewährten Methoden und Problembehandlungsschritte für [Node-Anwendungen](app-service-web-get-started-nodejs.md) beschrieben, die auf Azure-Web-Apps (mit [iisnode](https://github.com/azure/iisnode)) ausgeführt werden.

> [!WARNING]
> Gehen Sie mit Bedacht vor, wenn Sie für Ihre Produktionswebsite Schritte zur Problembehandlung ausführen. Es wird empfohlen, die Problembehandlung für Ihre App nicht in der Produktionsumgebung durchzuführen, sondern z.B. in Ihrem Stagingslot. Nachdem das Problem behoben wurde, können Sie den Stagingslot dann durch Ihren Produktionsslot austauschen.
> 
> 

## <a name="iisnode-configuration"></a>IISNODE-Konfiguration
Diese [Schemadatei](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) enthält alle Einstellungen, die Sie für iisnode konfigurieren können. Einige Einstellungen, die für Ihre Anwendung nützlich sind, lauten:

* nodeProcessCountPerApplication
  
    Mit dieser Einstellung wird die Anzahl von Knotenprozessen gesteuert, die pro IIS-Anwendung gestartet werden. Der Standardwert ist 1. Sie können so viele Instanzen von „node.exe“ starten, wie VM-vCPUs vorhanden sind, indem Sie diese Einstellung auf 0 festlegen. Für die meisten Anwendungen ist der empfohlene Wert 0, damit Sie alle vCPUs des Computers nutzen können. Für „node.exe“ gilt das Singlethread-Verfahren, sodass eine „node.exe“-Instanz maximal eine vCPU nutzt. Sie möchten erreichen, dass alle vCPUs genutzt werden, um für Ihre Knotenanwendung die höchste Leistung zu erzielen.
* nodeProcessCommandLine
  
    Mit dieser Einstellung wird der Pfad zu „node.exe“ gesteuert. Sie können diesen Wert so festlegen, dass er auf Ihre „node.exe“-Version zeigt.
* maxConcurrentRequestsPerProcess
  
    Mit dieser Einstellung wird die maximale Anzahl von gleichzeitigen Anforderungen gesteuert, die von iisnode an jede Instanz von „node.exe“ gesendet werden. Bei Azure-Web-Apps lautet der Standardwert hierfür „Infinite“. Sie müssen sich um diese Einstellung nicht kümmern. Außerhalb von Azure-Web-Apps lautet der Standardwert 1024. Sie können dies in Abhängigkeit davon konfigurieren, wie viele Anforderungen Ihre Anwendung erhält und wie schnell Ihre Anwendung die einzelnen Anforderungen verarbeitet.
* maxNamedPipeConnectionRetry
  
    Mit dieser Einstellung wird gesteuert, wie oft iisnode maximal versucht, die Verbindung für die benannte Pip erneut herzustellen, um die Anforderung an „node.exe“ zu senden. Diese Einstellung in Kombination mit namedPipeConnectionRetryDelay bestimmt den Gesamttimeout jeder Anforderung in iisnode. Für Azure-Web-Apps lautet der Standardwert 200. Gesamttimeout in Sekunden = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay)/1000
* namedPipeConnectionRetryDelay
  
    Mit dieser Einstellung wird gesteuert, wie lange (in ms) iisnode vor jedem erneuten Versuch zum Senden der Anforderung an „node.exe“ über die benannte Pipe wartet. Der Standardwert ist 250 ms.
    Gesamttimeout in Sekunden = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay)/1000
  
    Standardmäßig beträgt der Gesamttimeout in iisnode für Azure-Web-Apps 200 \* 250 ms = 50 Sekunden.
* logDirectory
  
    Mit dieser Einstellung wird das Verzeichnis gesteuert, in dem iisnode die Protokollierung für stdout/stderr durchführt. Der Standardwert ist „iisnode“, der relativ zum Skripthauptverzeichnis gilt (Verzeichnis, in dem die Hauptversion von „main server.js“ enthalten ist).
* debuggerExtensionDll
  
    Mit dieser Einstellung wird gesteuert, welche Version von Node-Inspector von iisnode beim Debuggen der Node-Anwendung verwendet wird. Derzeit sind „iisnode-inspector-0.7.3.dll“ und „iisnode-inspector.dll“ die einzigen beiden gültigen Werte für diese Einstellung. Der Standardwert lautet „iisnode-inspector-0.7.3.dll“. Für die Version „iisnode-inspector-0.7.3.dll“ werden „node-inspector-0.7.3“ und WebSockets verwendet. Sie müssen WebSockets in Ihrer Azure-Web-App aktivieren, um diese Version nutzen zu können. Ausführlichere Informationen zur Konfiguration von iisnode für die Nutzung der neuen Node-Inspector-Version finden Sie unter <http://www.ranjithr.com/?p=98>.
* flushResponse
  
    Das Standardverhalten von IIS besteht darin, dass Antwortdaten von bis zu 4 MB vor dem Leeren oder bis zum Ende der Antwort (je nachdem, was früher eintritt) gepuffert werden. iisnode enthält eine Konfigurationseinstellung zum Außerkraftsetzen dieses Verhaltens. Um ein Fragment des Antwortentität-Textkörpers zu leeren, sobald iisnode es von „node.exe“ empfängt, müssen Sie das iisnode/@flushResponse-Attribut in „web.config“ auf „true“ festlegen:
  
    ```
    <configuration>    
        <system.webServer>    
            <!-- ... -->    
            <iisnode flushResponse="true" />    
        </system.webServer>    
    </configuration>
    ```
  
    Durch das Ermöglichen der Leerung jedes Fragments des Antwortentität-Textkörpers wird der Leistungsaufwand erhöht und der Durchsatz des Systems um ca. 5% reduziert (ab v0.1.13). Es ist also am besten, diese Einstellung nur für Endpunkte festzulegen, für die das Streaming von Antworten erforderlich ist (z.B. mit dem <location>-Element in „web.config“).
  
    Für das Streaming von Anwendungen müssen Sie außerdem responseBufferLimit für Ihren iisnode-Handler auf 0 festlegen.
  
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```
* watchedFiles
  
    Dies ist eine durch Semikolons getrennte Liste mit Dateien, die auf Änderungen überprüft werden. Eine Änderung einer Datei bewirkt, dass die Anwendung „recycelt“ wird. Jeder Eintrag besteht aus einem optionalen Verzeichnisnamen und einem erforderlichen Dateinamen. Diese gelten relativ zu dem Verzeichnis, in dem sich der Einstiegspunkt der Hauptanwendung befindet. Platzhalter sind nur im Dateinamenteil zulässig. Der Standardwert ist „\*.js;web.config“.
* recycleSignalEnabled
  
    Der Standardwert ist „false“. Wenn diese Einstellung aktiviert ist, kann die Node-Anwendung eine Verbindung mit einer benannten Pipe herstellen (Umgebungsvariable IISNODE\_CONTROL\_PIPE) und eine „recycle“-Nachricht senden. Dies bewirkt, dass w3wp korrekt recycelt wird.
* idlePageOutTimePeriod
  
    Der Standardwert ist 0. Dies bedeutet, dass das Feature aktiviert ist. Wenn ein höherer Wert als 0 angegeben ist, lagert iisnode alle untergeordneten Prozesse jeweils nach Ablauf der „idlePageOutTimePeriod“ (in Millisekunden) aus. Grundlegende Informationen zur Auslagerung finden Sie in der entsprechenden [Dokumentation](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx). Diese Einstellung ist für Anwendungen nützlich, die viel Arbeitsspeicher verbrauchen und für die Arbeitsspeicher von Zeit zu Zeit auf den Datenträger ausgelagert werden soll, um RAM freizugeben.

> [!WARNING]
> Gehen Sie mit Bedacht vor, wenn Sie die folgenden Konfigurationseigenschaften für Produktionsanwendungen aktivieren. Es wird empfohlen, die Aktivierung nicht für aktive Produktionsanwendungen durchzuführen.
> 
> 

* debugHeaderEnabled
  
    Der Standardwert ist „false“. Wenn „true“ festgelegt ist, fügt iisnode jeder gesendeten HTTP-Antwort den HTTP-Antwortheader „iisnode-debug“ hinzu. Der „iisnode-debug“-Headerwert ist eine URL. Sie können einzelne Teile der Diagnoseinformationen ermitteln, indem Sie sich das URL-Fragment ansehen. Sie erreichen aber eine deutlich bessere Visualisierung, indem Sie die URL im Browser öffnen.
* loggingEnabled
  
    Mit dieser Einstellung wird die Protokollierung von stdout und stderr durch iisnode gesteuert. iisnode erfasst stdout/stderr von Knotenprozessen, die gestartet werden, und führt Schreibvorgänge in das Verzeichnis durch, das unter der Einstellung „logDirectory“ angegeben ist. Nach der Aktivierung schreibt Ihre Anwendung Protokolle in das Dateisystem. Je nach Umfang der von der Anwendung durchgeführten Protokollierung kann es hierbei auch zu Leistungseinbußen kommen.
* devErrorsEnabled
  
   Der Standardwert ist „false“. Bei der Festlegung auf „true“ zeigt iisnode den HTTP-Statuscode und win32-Fehlercode in Ihrem Browser an. Der win32-Code ist hilfreich beim Debuggen bestimmter Problemtypen.
* debuggingEnabled (nicht für aktive Produktionswebsite aktivieren)
  
    Mit dieser Einstellung wird die Debugfunktion gesteuert. iisnode ist in Node-Inspector integriert. Indem Sie diese Einstellung aktivieren, aktivieren Sie das Debuggen Ihrer Node-Anwendung. Nach der Aktivierung dieser Einstellung stellt iisnode die erforderlichen Node-Inspector-Dateien nach der ersten Debuganforderung an Ihre Node-Anwendung im Verzeichnis „debuggerVirtualDir“ bereit. Sie können den Node-Inspector laden, indem Sie eine Anforderung an „http://yoursite/server.js/debug“ senden. Sie können das Debug-URL-Segment mit der Einstellung „debuggerPathSegment“ steuern. Standardmäßig lautet sie „debuggerPathSegment=’debug’“. Sie können sie beispielsweise auf eine GUID festlegen, damit die Ermittlung durch andere Personen erschwert wird.
  
    Weitere Informationen zum Debuggen finden Sie unter diesem [Link](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) .

## <a name="scenarios-and-recommendationstroubleshooting"></a>Szenarien und Empfehlungen/Problembehandlung
### <a name="my-node-application-is-making-too-many-outbound-calls"></a>Meine Node-Anwendung führt zu viele ausgehende Aufrufe durch
Viele Anwendungen stellen im Rahmen ihres normalen Betriebs ausgehende Verbindungen her. Wenn beispielsweise eine Anforderung empfangen wird, nimmt Ihre Knoten-App in der Regel Kontakt mit einer REST-API an einem anderen Ort auf, um Informationen zur Verarbeitung der Anforderung zu erhalten. Es ist ratsam, für http- oder https-Aufrufe einen Keep-Alive-Agent zu verwenden. Beispielsweise können Sie zum Durchführen dieser ausgehenden Aufrufe das agentkeepalive-Modul als Keep-Alive-Agent verwenden. So wird sichergestellt, dass die Sockets auf der Azure-Web-App-VM wiederverwendet werden und der Mehraufwand vermieden wird, der mit der Erstellung neuer Sockets für jede ausgehende Anforderung verbunden ist. Außerdem wird sichergestellt, dass Sie eine geringere Anzahl von Sockets für ausgehende Anforderungen verwenden und daher nicht den maxSockets-Wert überschreiten, der pro VM zugeordnet ist. Die Empfehlung für Azure-Web-Apps lautet, den maxSockets-Wert für agentKeepAlive auf insgesamt 160 Sockets pro VM festzulegen. Dies bedeutet Folgendes: Wenn auf der VM vier „node.exe“-Instanzen ausgeführt werden, sollten Sie den maxSockets-Wert für agentKeepAlive auf 40 pro „node.exe“-Instanz festlegen, also insgesamt 160 pro VM.

Beispielkonfiguration für [agentKeepAlive](https://www.npmjs.com/package/agentkeepalive):

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

In diesem Beispiel wird davon ausgegangen, dass Sie vier „node.exe“-Instanzen auf Ihrer VM ausführen. Wenn Sie eine andere Anzahl von „node.exe“-Instanzen auf der VM ausführen, müssen Sie die maxSockets-Einstellung entsprechend ändern.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>Der CPU-Verbrauch meiner Node-Anwendung ist zu hoch
Wahrscheinlich erhalten Sie von Azure-Web-Apps in Ihrem Portal eine Empfehlung zum hohen CPU-Verbrauch. Sie können auch Monitore einrichten, mit denen bestimmte [Metriken](web-sites-monitor.md) überwacht werden. Sehen Sie sich beim Überprüfen der CPU-Nutzung im [Azure-Portal-Dashboard](../application-insights/app-insights-web-monitor-performance.md)die MAX-Werte für die CPU an, damit Ihnen die Spitzenwerte nicht entgehen.
Falls Sie der Meinung sind, dass der CPU-Verbrauch Ihrer Anwendung zu hoch ist, und den Grund dafür nicht kennen, können Sie zur Ermittlung für Ihre Node-Anwendung ein Profil erstellen.

### 
#### <a name="profiling-your-node-application-on-azure-web-apps-with-v8-profiler"></a>Profilerstellung für die Node-Anwendung in Azure-Web-Apps mit V8-Profiler
Angenommen, Sie verfügen über eine Hello World-App, für die Sie wie folgt ein Profil erstellen möchten:

```
var http = require('http');    
function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    WriteConsoleLog();    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Wechseln Sie zur SCM-Website „https://yoursite.scm.azurewebsites.net/DebugConsole“.

Sie sehen eine Befehlszeile wie in der Darstellung unten. Wechseln Sie in das Verzeichnis „site/wwwroot“.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Führen Sie den Befehl „npm install v8-profiler“ aus.

Der „v8-profiler“ wird im Verzeichnis „node\_modules“ und für alle Abhängigkeiten installiert.
Bearbeiten Sie nun die Datei „server.js“, um das Profil für Ihre Anwendung zu erstellen.

```
var http = require('http');    
var profiler = require('v8-profiler');    
var fs = require('fs');

function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    profiler.startProfiling('HandleRequest');    
    WriteConsoleLog();    
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Mit dem obigen Code wird das Profil für die WriteConsoleLog-Funktion erstellt, und anschließend wird die Profilausgabe in die Datei „profile.cpuprofile“ im Verzeichnis „site/wwwroot“ geschrieben. Senden Sie eine Anforderung an die Anwendung. Sie sehen, dass unter „site/wwwroot“ die Datei „profile.cpuprofile“ erstellt wird.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Laden Sie diese Datei herunter, und öffnen Sie sie mit den F12-Tools von Chrome. Drücken Sie in Chrome die F12-TASTE, und wählen Sie anschließend die Registerkarte **Profile**. Wählen Sie die Schaltfläche **Laden**. Wählen Sie die Datei „profile.cpuprofile“ aus, die Sie heruntergeladen haben. Klicken Sie auf das Profil, das Sie gerade geladen haben.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

Sie sehen, dass 95% der Zeit von der WriteConsoleLog-Funktion verbraucht wurde. Es werden auch die genauen Zeilennummern und Quelldateien angezeigt, die das Problem verursacht haben.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Der Arbeitsspeicherverbrauch meiner Node-Anwendung ist zu hoch
Wenn Ihre Anwendung zu viel Arbeitsspeicher verbraucht, wird für Azure-Web-Apps im Portal ein entsprechender Hinweis angezeigt. Sie können Monitore einrichten, mit denen bestimmte [Metriken](web-sites-monitor.md) überwacht werden. Sehen Sie sich beim Überprüfen der Arbeitsspeichernutzung im [Azure-Portal-Dashboard](../application-insights/app-insights-web-monitor-performance.md) die MAX-Werte für den Arbeitsspeicher an, damit Ihnen die Spitzenwerte nicht entgehen.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>Speicherverlusterkennung und Heapvergleich für „node.js“
Sie können [node-memwatch](https://github.com/lloyd/node-memwatch) als Unterstützung beim Identifizieren von Speicherverlusten verwenden.
Sie können memwatch genau wie v8-profiler installieren und Ihren Code bearbeiten, um Heaps zu erfassen und zu vergleichen und so die Speicherverluste der Anwendung zu identifizieren.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>Meine „node.exe“-Instanzen werden nach dem Zufallsprinzip beendet
Es gibt einige Gründe, warum dies passieren kann:

1. Ihre Anwendung löst nicht abgefangene Ausnahmen aus. Suchen Sie in der Datei „d:\\home\\LogFiles\\Application\\logging-errors.txt“ nach Details zur ausgelösten Ausnahme. Diese Datei verfügt über Stapelüberwachung, und Sie können den Anwendungsfehler auf dieser Grundlage beheben.
2. Ihre Anwendung verbraucht zu viel Arbeitsspeicher, und dies wirkt sich auf das Starten anderer Prozesse aus. Wenn der gesamte VM-Arbeitsspeicher nahe bei 100% liegt, werden Ihre „node.exe“-Instanzen vom Prozess-Manager ggf. beendet, damit auch andere Prozesse die Chance zum Ausführen von Schritten erhalten. Gehen Sie wie folgt vor, um dies zu beheben: Stellen Sie entweder sicher, dass es für die Anwendung nicht zu einem Speicherverlust kommt, ODER skalieren Sie zentral hoch auf eine größere VM mit deutlich mehr RAM, falls Ihre Anwendung eine große Menge an Arbeitsspeicher benötigt.

### <a name="my-node-application-does-not-start"></a>Meine Node-Anwendung startet nicht
Wenn die Anwendung beim Starten Fehler vom Typ 500 zurückgibt, kann dies verschiedene Gründe haben:

1. „node.exe“ ist nicht am richtigen Speicherort vorhanden. Überprüfen Sie die Einstellung nodeProcessCommandLine.
2. Die Hauptskriptdatei ist nicht am richtigen Speicherort vorhanden. Überprüfen Sie die Datei „web.config“, und stellen Sie sicher, dass der Name der Hauptskriptdatei im Abschnitt „handlers“ mit der Hauptskriptdatei übereinstimmt.
3. Die web.config-Konfiguration ist nicht korrekt. Überprüfen Sie die Namen und Werte der Einstellungen.
4. Kaltstart: Der Startvorgang Ihrer Anwendung dauert zu lange. Wenn die Anwendung länger als (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay)/1000 Sekunden benötigt, gibt iisnode Fehler 500 zurück. Erhöhen Sie die Werte dieser Einstellungen, um sie an die Startdauer der Anwendung anzupassen. So wird verhindert, dass für iisnode ein Timeout auftritt und der Fehler 500 zurückgegeben wird.

### <a name="my-node-application-crashed"></a>Meine Node-Anwendung ist abgestürzt
Ihre Anwendung löst nicht abgefangene Ausnahmen aus. Suchen Sie in der Datei „d:\\home\\LogFiles\\Application\\logging-errors.txt“ nach Details zur ausgelösten Ausnahme. Diese Datei verfügt über Stapelüberwachung, und Sie können den Anwendungsfehler auf dieser Grundlage beheben.

### <a name="my-node-application-takes-too-much-time-to-start-cold-start"></a>Meine Node-Anwendung benötigt zu viel Zeit zum Starten (Kaltstart)
Die häufigste Ursache dafür, dass eine Anwendung zu viel Zeit zum Starten benötigt, ist eine große Zahl von Dateien unter „node\_modules“. Die Anwendung versucht, die meisten dieser Dateien beim Starten zu laden. Standardmäßig kann das Laden von vielen Dateien lange dauern, da sich Ihre Dateien auf der Netzwerkfreigabe in Azure-Web-Apps befinden.
Beispiele für Lösungen, um diesen Prozess zu beschleunigen:

1. Stellen Sie sicher, dass Sie über eine flache Abhängigkeitsstruktur verfügen und doppelte Abhängigkeiten vermeiden, indem Sie npm3 zum Installieren der Module verwenden.
2. Versuchen Sie, Ihre node\_modules nach und nach und nicht auf einmal beim Starten der Anwendung zu laden. Dies bedeutet, dass der Aufruf von „require(‘module’)“ erst erfolgen sollte, wenn dies in der Funktion bei Verwendung des Moduls wirklich erforderlich ist.
3. Azure-Web-Apps verfügen über ein Feature mit dem Namen „Lokaler Cache“. Bei diesem Feature wird Ihr Inhalt von der Netzwerkfreigabe auf den lokalen Datenträger auf der VM kopiert. Da die Dateien lokal vorhanden sind, geht das Laden von node\_modules deutlich schneller.

## <a name="iisnode-http-status-and-substatus"></a>IISNODE-http-Status und -Unterstatus
In dieser [Quelldatei](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) sind alle möglichen Status/Unterstatus-Kombinationen aufgeführt, die von iisnode bei einem Fehler zurückgegeben werden können.

Aktivieren Sie FREB für Ihre Anwendung, um den win32-Fehlercode anzuzeigen. (Stellen Sie aus Leistungsgründen sicher, dass Sie FREB nur für Websites aktivieren, die nicht für die Produktion bestimmt sind.)

| Http-Status | Http-Unterstatus | Möglicher Grund? |
| --- | --- | --- |
| 500 |1000 |Es besteht ein Problem mit der Zustellung der Anforderung an IISNODE. Überprüfen Sie, ob „node.exe“ gestartet wurde. Unter Umständen ist „node.exe“ beim Starten abgestürzt. Überprüfen Sie die web.config-Konfiguration auf Fehler. |
| 500 |1001 |- Win32Error 0x2 – App reagiert nicht auf die URL. Überprüfen Sie die URL Rewrite-Regeln bzw. die Richtigkeit der Routendefinition Ihrer Express-App. - Win32Error 0x6d – Benannte Pipe ist beschäftigt – „node.exe“ akzeptiert keine Anforderungen, weil die Pipe beschäftigt ist. Überprüfen Sie den hohen CPU-Verbrauch. - Andere Fehler – Überprüfen Sie, ob „node.exe“ abgestürzt ist. |
| 500 |1002 |„node.exe“ abgestürzt: Überprüfen Sie „d:\\home\\LogFiles\\logging-errors.txt“ in Bezug auf die Stapelüberwachung. |
| 500 |1003 |Problem mit der Pipekonfiguration: Dies sollte eigentlich nicht auftreten. Falls doch, ist die Konfiguration der benannten Pipe fehlerhaft. |
| 500 |1004 - 1018 |Es ist ein Fehler aufgetreten, während die Anforderung gesendet oder die Antwort an bzw. von „node.exe“ verarbeitet wurde. Überprüfen Sie, ob „node.exe“ abgestürzt ist. Überprüfen Sie „d:\\home\\LogFiles\\logging-errors.txt“ in Bezug auf die Stapelüberwachung. |
| 503 |1000 |Nicht genügend Arbeitsspeicher zum Zuordnen von weiteren Verbindungen vom Typ „Benannte Pipe“. Überprüfen Sie, warum Ihre App so viel Arbeitsspeicher verbraucht. Überprüfen Sie den Wert der Einstellung maxConcurrentRequestsPerProcess. Wenn nicht „infinite“ (unendlich) festgelegt ist und Sie über eine hohe Zahl von Anforderungen verfügen, können Sie diesen Wert erhöhen, um den Fehler zu verhindern. |
| 503 |1001 |Die Anforderung konnte nicht an „node.exe“ zugestellt werden, da für die Anwendung gerade der Vorgang für die Wiederverwendung durchgeführt wird. Nach Abschluss des Vorgangs sollten Anforderungen wieder normal bereitgestellt werden. |
| 503 |1002 |Überprüfen Sie den win32-Fehlercode auf den tatsächlichen Grund. Die Anforderung konnte nicht für „node.exe“ zugestellt werden. |
| 503 |1003 |Benannte Pipe ist zu stark ausgelastet. Überprüfen Sie, ob der Knoten einen hohen CPU-Verbrauch aufweist. |

„NODE.exe“ enthält eine Einstellung mit dem Namen NODE\_PENDING\_PIPE\_INSTANCES. Dieser Wert lautet außerhalb von Azure-Web-Apps standardmäßig 4. Dies bedeutet, dass „node.exe“ für die benannte Pipe nur vier Anforderungen auf einmal akzeptieren kann. In Azure-Web-Apps ist dieser Wert auf 5000 festgelegt. Dieser Wert sollte für die meisten Knotenanwendungen ausreichen, die unter Azure-Web-Apps ausgeführt werden. 503.1003 sollte für Azure-Web-Apps nicht erscheinen, da für NODE\_PENDING\_PIPE\_INSTANCES ein hoher Wert gilt.  |

## <a name="more-resources"></a>Weitere Ressourcen
Unter den unten angegebenen Links finden Sie weitere Informationen zu „node.js“-Anwendungen für Azure App Service.

* [Erste Schritte mit Node.js-Web-Apps in Azure App Service](app-service-web-get-started-nodejs.md)
* [Debuggen einer Node.js-Web-App in Azure App Service](app-service-web-tutorial-nodejs-mongodb-app.md)
* [Verwenden von Node.js-Modulen mit Azure-Anwendungen](../nodejs-use-node-modules-azure-apps.md)
* [Azure App Service-Web-Apps: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Node.js Developer Center (in englischer Sprache)](../nodejs-use-node-modules-azure-apps.md)
* [Exploring the Super Secret Kudu Debug Console (Erkunden der geheimen Kudu-Debugkonsole)](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)

