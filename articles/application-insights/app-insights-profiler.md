---
title: "Profilerstellung für Live-Web-Apps in Azure mit Application Insights Profiler | Microsoft-Dokumentation"
description: Identifizieren Sie den langsamsten Pfad in Ihrem Webservercode mithilfe eines kompakten Profilers.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: mbullwin
ms.openlocfilehash: e66dc2af18785c6c8e83815129c8bca5b877d25b
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2017
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Profilerstellung für Live-Azure-Web-Apps mit Application Insights

*Dieses Feature von Application Insights ist für Azure App Service allgemein und für Azure-Computeressourcen als Vorschauversion verfügbar.*

Ermitteln Sie den Zeitaufwand für die einzelnen Methoden in Ihrer Live-Webanwendung mithilfe von [Application Insights Profiler](app-insights-overview.md). Das Application Insights-Profilerstellungstool zeigt detaillierte Profile der aktiven Anforderungen, die von Ihrer App verarbeitet wurden, und hebt den *langsamsten Pfad* hervor, der die meiste Zeit beansprucht. Der Profiler wählt automatisch Beispiele mit unterschiedlichen Antwortzeiten aus, und verwendet anschließend verschiedene Techniken, um den Mehraufwand zu minimieren.

Der Profiler kann momentan für ASP.NET-Web-Apps verwendet werden, die unter Azure App Services und mindestens mit der Basic-Dienstebene ausgeführt werden.

## <a id="installation"></a> Aktivieren des Profilers

[Installieren Sie Application Insights](app-insights-asp-net.md) in Ihrem Code. Sollte es bereits installiert sein, vergewissern Sie sich, dass Sie über die neueste Version verfügen. Um auf die neueste Version zu prüfen, klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie dann **NuGet-Pakete verwalten** > **Updates** > **Alle Pakete aktualisieren** aus. Stellen Sie Ihre App dann erneut bereit.

*Sie verwenden ASP.NET Core? Hier erhalten Sie [weitere Informationen](#aspnetcore).*

Öffnen Sie im [Azure-Portal](https://portal.azure.com) die Application Insights-Ressource für Ihre Web-App. Wählen Sie **Leistung** > **Application Insights Profiler aktivieren** aus.

![Wählen Sie das Banner zum Aktivieren des Profilers aus][enable-profiler-banner]

Alternativ können Sie auch **Konfigurieren** auswählen, um den Status anzuzeigen und den Profiler zu aktivieren bzw. zu deaktivieren.

![Wählen Sie unter „Leistung“ die Option „Konfigurieren“ aus][performance-blade]

Mit Application Insights konfigurierte Web-Apps sind auf dem Blatt **Konfigurieren** aufgeführt. Befolgen Sie ggf. die Anweisungen zum Installieren des Profiler-Agents. Falls noch keine Web-Apps mit Application Insights konfiguriert wurden, wählen Sie **Verknüpfte Apps hinzufügen** aus.

Steuern Sie den Profiler für Ihre verknüpften Web-Apps, indem Sie im Bereich **Konfigurieren** die Option **Profiler aktivieren** oder **Profiler deaktivieren** auswählen.

![Optionen im Bereich „Konfigurieren“][linked app services]

Im Gegensatz zu Web-Apps, die über App Service-Pläne gehostet werden, werden auf Azure-Computeressourcen gehostete Anwendungen (z.B. Azure Virtual Machine, VM-Skalierungsgruppen, Azure Service Fabric oder Azure Cloud Services) nicht direkt von Azure verwaltet. In diesem Fall ist keine Web-App vorhanden, mit der eine Verknüpfung hergestellt werden kann. Statt eine Verknüpfung mit einer App herzustellen, wählen Sie die Schaltfläche **Profiler aktivieren** aus.

## <a name="disable-the-profiler"></a>Deaktivieren von Profiler
Wenn Sie den Profiler für eine einzelne App Service-Instanz beenden oder neu starten möchten, wechseln Sie unter **Webaufträge** zur App Service-Ressource. Wenn Sie den Profiler löschen möchten, wechseln Sie zu **Erweiterungen**.

![Deaktivieren von Profiler für einen Webauftrag][disable-profiler-webjob]

Es wird empfohlen, den Profiler für alle Web-Apps zu aktivieren, um Leistungsprobleme möglichst schnell zu ermitteln.

Wenn Sie Änderungen an Ihrer Webanwendung mithilfe von WebDeploy bereitstellen, achten Sie darauf, dass der Ordner „App_Data“ bei der Bereitstellung nicht gelöscht wird. Andernfalls werden die Dateien der Profiler-Erweiterung gelöscht, wenn Sie die Webanwendung das nächste Mal in Azure bereitstellen.

### <a name="using-profiler-with-azure-vms-and-azure-compute-resources-preview"></a>Verwenden des Profilers mit virtuellen Azure-Computern und Azure-Computeressourcen (Vorschau)

Wenn Sie [Application Insights für Azure App Services zur Laufzeit aktivieren](app-insights-azure-web-apps.md#run-time-instrumentation-with-application-insights), ist Application Insights Profiler automatisch verfügbar. Wenn Sie Application Insights bereits für die Ressource aktiviert haben, müssen Sie mit dem Konfigurations-Assistenten möglicherweise ein Update auf die aktuelle Version ausführen.

Informieren Sie sich über die [Vorschauversion von Profiler für Azure-Computeressourcen](https://go.microsoft.com/fwlink/?linkid=848155).


## <a name="limitations"></a>Einschränkungen

Die Standardaufbewahrung von Daten beträgt fünf Tage. Die maximale pro Tag erfasste Datenmenge beträgt 10 GB.

Es fallen keine Gebühren für die Verwendung des Profiler-Diensts an. Für die Nutzung des Profiler-Diensts muss Ihre Web-App mindestens im Basic-Tarif von App Service gehostet werden.

## <a name="overhead-and-sampling-algorithm"></a>Auslastung und Algorithmus für Stichprobenerstellung

Der Profiler wird stündlich nach dem Zufallsprinzip zwei Minuten auf jedem virtuellen Computer ausgeführt, der die Anwendung hostet und auf dem der Profiler für die Erfassung von Ablaufverfolgungen aktiviert ist. Während der Ausführung des Profilers erhöht sich die CPU-Auslastung des Servers um 5 % bis 15 %.
Je mehr Server zum Hosten der Anwendung zur Verfügung stehen, desto weniger wirkt sich der Profiler auf die Gesamtleistung der Anwendung aus. Die Ursache ist, dass der Profiler durch den Samplingalgorithmus immer nur auf 5 % der Server ausgeführt wird. Mehr Server sind zum Verarbeiten von Webanforderungen verfügbar, um den Servermehraufwand auszugleichen, der durch die Ausführung des Profilers verursacht wird.


## <a name="view-profiler-data"></a>Anzeigen von Profiler-Daten

Wechseln Sie zum Bereich **Leistung**, und scrollen Sie dann nach unter zur Liste der Vorgänge.

![Application Insights-Bereich „Leistung“, Spalte „Beispiele“][performance-blade-examples]

Die Tabelle der Vorgänge enthält folgende Spalten:

* **ANZAHL**: Die Anzahl dieser Anforderungen im Zeitbereich des Bereichs **ANZAHL**.
* **MEDIAN**: Die typische Reaktionszeit Ihrer App bei einer Anforderung. Die Hälfte aller Reaktionen war schneller als dieser Wert.
* **95. PERZENTIL**: 95 % der Reaktionen waren schneller als dieser Wert. Sollte sich dieser Wert deutlich vom Median unterscheiden, liegt bei Ihrer App unter Umständen ein vorübergehendes Problem vor. (Eine andere denkbare Erklärung wäre ein Designfeature wie Caching.)
* **PROFILER-ABLAUFVERFOLGUNGEN**: Ein Symbol gibt an, dass der Profiler Stapelüberwachungen für diesen Vorgang erfasst hat.

Wählen Sie **Ansicht** aus, um den Ablaufverfolgungs-Explorer zu öffnen. Der Explorer zeigt mehrere vom Profiler erfasste Beispiele, klassifiziert nach Reaktionszeit.

Wenn Sie den Bereich für die **Leistungsvorschau** verwenden, wechseln Sie zum Abschnitt **Aktion ausführen** auf der Seite, um Profiler-Ablaufverfolgungen anzuzeigen. Wählen Sie die Schaltfläche **Profiler-Ablaufverfolgungen** aus.

![Profiler-Ablaufverfolgungen im Application Insights-Bereich für die Leistungsvorschau][performance-blade-v2-examples]

Wählen Sie ein Beispiel aus, um eine Aufschlüsselung des Zeitaufwands für die Anforderung auf Codeebene anzuzeigen.

![Ablaufverfolgungs-Explorer von Application Insights][trace-explorer]

Im Ablaufverfolgungs-Explorer werden die folgenden Informationen angezeigt:

* **Langsamsten Pfad anzeigen**: Öffnet den größten Blattknoten (oder etwas in der Nähe). In den meisten Fällen befindet sich dieser Knoten in der Nähe eines Leistungsengpasses.
* **Bezeichnung**: Der Name der Funktion oder des Ereignisses. Die Struktur enthält eine Mischung aus Code und aufgetretenen Ereignissen (wie SQL- und HTTP-Ereignisse). Das oberste Ereignis stellt die Gesamtdauer der Anforderung dar.
* **Verstrichen**: Das Zeitintervall zwischen Beginn und Ende des Vorgangs.
* **Wann**: Wann die Funktion oder das Ereignis aktiv war (relativ zu anderen Funktionen).

## <a name="how-to-read-performance-data"></a>Lesen von Leistungsdaten

Der Dienst-Profiler von Microsoft analysiert die Leistung Ihrer Anwendung mithilfe einer Kombination aus Samplingmethoden und Instrumentierung. Während der ausführlichen Erfassung nimmt der Dienst-Profiler jede Millisekunde Stichproben des Anweisungszeigers der einzelnen CPUs des Computers. Jede Stichprobe erfasst die vollständige Aufrufliste des Threads, der gerade ausgeführt wird. Sie enthält ausführliche und nützliche Informationen zu diesem Thread, sowohl auf hoher als auch auf niedriger Abstraktionsebene. Darüber hinaus erfasst der Dienst-Profiler auch andere Ereignisse zur Nachverfolgung von Aktivitätskorrelation und Kausalität, dazu gehören Kontextwechsel-, TPL- (Task Parallel Library) und Threadpoolereignisse.

Die Aufrufliste in der Zeitachsenansicht ist das Ergebnis des oben erwähnten Samplings und der Instrumentierung. Da jede Stichprobe die vollständige Aufrufliste des Threads erfasst, enthält sie auch Code aus Microsoft .NET Framework sowie aus anderen Frameworks, auf die Sie verweisen.

### <a id="jitnewobj"></a>Objektzuordnung („clr!JIT\_New“ oder „clr!JIT\_Newarr1“)
**clr!JIT\_New** und **clr!JIT\_Newarr1** sind Hilfsfunktionen in .NET Framework, die Arbeitsspeicher von einem verwalteten Heap zuweisen. **clr!JIT\_New** wird aufgerufen, wenn ein Objekt zugeordnet wird. **clr!JIT\_Newarr1** wird aufgerufen, wenn ein Objektarray zugeordnet wird. Diese beiden Funktionen sind in der Regel sehr schnell und benötigen relativ wenig Zeit. Sollte **clr!JIT\_New** oder **clr!JIT\_Newarr1** auf Ihrer Zeitachse viel Zeit beanspruchen, deutet das darauf hin, dass der Code viele Objekte zuordnet und eine erhebliche Menge an Arbeitsspeicher beansprucht.

### <a id="theprestub"></a>Laden von Code (clr!ThePreStub)
**clr!ThePreStub** ist eine Hilfsfunktion in .NET Framework, die den Code für die erstmalige Ausführung vorbereitet. Das schließt in der Regel auch die JIT-Kompilierung (Just-In-Time) ein. Während der Lebensdauer eines Prozesses sollte **clr!ThePreStub** für jede C#-Methode höchstens einmal aufgerufen werden.

Wenn **clr!ThePreStub** für eine Anforderung viel Zeit beansprucht, ist dies ein Hinweis darauf, dass die Anforderung der erste ist, die diese Methode ausführt. Das Laden dieser Methode durch .NET Framework Runtime dauert relativ lange. Verwenden Sie ggf. einen Vorbereitungsprozess, der diesen Teil des Codes ausführt, bevor Ihre Benutzer darauf zugreifen, oder führen Sie Native Image Generator (ngen.exe) für Ihre Assemblys aus.

### <a id="lockcontention"></a> („clr!JITutil\_MonContention“ oder „clr!JITutil\_MonEnterWorker“)
**clr!JITutil\_MonContention** oder **clr!JITutil\_MonEnterWorker** gibt an, dass der aktuelle Thread auf die Aufhebung einer Sperre wartet. Das ist üblicherweise der Fall, wenn eine C#-Anweisung vom Typ **LOCK** ausgeführt wird, die **Monitor.Enter**-Methode aufgerufen wird oder eine Methode mit dem **MethodImplOptions.Synchronized**-Attribut aufgerufen wird. Ein Sperrkonflikt ist in der Regel darauf zurückzuführen, dass Thread A eine Sperre abruft und Thread B versucht, die gleiche Sperre abzurufen, bevor sie von Thread A wieder aufgehoben wurde.

### <a id="ngencold"></a>Laden von Code ([COLD])
Falls der Methodenname **[COLD]** enthält (Beispiel: **mscorlib.ni![COLD]System.Reflection.CustomAttribute.IsDefined**), führt .NET Framework Runtime zum ersten Mal Code aus, der nicht durch die <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">profilgesteuerte Optimierung</a> optimiert wurde. Während der Lebensdauer des Prozesses sollte dies für jede Methode höchstens einmal erscheinen.

Falls das Laden von Code bei einer Anforderung sehr lange dauert, deutet das darauf hin, dass die Anforderung die erste Anforderung ist, die den nicht optimierten Teil der Methode ausführt. Verwenden Sie ggf. einen Vorbereitungsprozess, der diesen Teil des Codes ausführt, bevor Ihre Benutzer darauf zugreifen.

### <a id="httpclientsend"></a>Senden von HTTP-Anforderungen
Methoden wie **HttpClient.Send** deuten darauf hin, dass der Code auf den Abschluss einer HTTP-Anforderung wartet.

### <a id="sqlcommand"></a>Datenbankvorgang
Methoden wie **SqlCommand.Execute** deuten darauf hin, dass der Code auf den Abschluss eines Datenbankvorgangs wartet.

### <a id="await"></a>Warten (AWAIT\_TIME)
**AWAIT\_TIME** deutet darauf hin, dass der Code auf den Abschluss einer anderen Aufgabe wartet. Das ist in der Regel bei C#-Anweisungen vom Typ **AWAIT** der Fall. Wenn der Code eine C#-Anweisung vom Typ **AWAIT** ausführt, wird der Thread entladen und übergibt die Steuerung wieder an den Threadpool, sodass kein Thread auf den Abschluss der **AWAIT**-Anweisung warten muss und dadurch blockiert wird. Logisch betrachtet ist allerdings der Thread, der die **AWAIT**-Anweisung ausgeführt hat, blockiert und wartet auf den Abschluss des Vorgangs. Die **AWAIT\_TIME**-Anweisung gibt die blockierte Zeit an, für die auf den Abschluss der Aufgabe gewartet wird.

### <a id="block"></a>Blockierte Zeit
**BLOCKED_TIME** gibt an, dass der Code darauf wartet, dass eine andere Ressource verfügbar ist. Beispielsweise könnte er auf ein Synchronisierungsobjekt warten, darauf, dass ein Thread verfügbar ist, oder auf den Abschluss einer Anforderung.

### <a id="cpu"></a>CPU-Zeit
Die CPU ist mit der Ausführung der Anweisungen beschäftigt.

### <a id="disk"></a>Datenträgerzeit
Die Anwendung führt Datenträgervorgänge aus.

### <a id="network"></a>Netzwerkzeit
Die Anwendung führt Netzwerkvorgänge aus.

### <a id="when"></a>Wann-Spalte
Die Spalte **Wann** ist eine Visualisierung der Abweichungen INKLUSIVER Stichproben für einen Knoten im Zeitverlauf. Der Gesamtbereich der Anforderung wird in 32 Zeitrahmen unterteilt. Die inklusiven Stichproben für diesen Knoten werden in diesen 32 Zeitrahmen akkumuliert. Jeder Zeitrahmen wird als Balken dargestellt. Die Höhe der Balken stellt einen skalierten Wert dar. Bei Knoten, die mit **CPU_TIME** oder **BLOCKED_TIME** gekennzeichnet sind oder bei denen ein offensichtlicher Zusammenhang mit der Nutzung einer Ressource (CPU, Datenträger, Thread) besteht, stellt der Balken die Nutzung einer dieser Ressourcen für den Zeitraum des entsprechenden Zeitrahmens dar. Für diese Metrik ist es möglich, durch die Nutzung mehrerer Ressourcen einen Wert von mehr als 100 % zu erhalten. Wenn Sie also beispielsweise innerhalb eines Intervalls im Schnitt zwei CPUs verwenden, erhalten Sie 200 %.


## <a id="troubleshooting"></a>Problembehandlung

### <a name="too-many-active-profiling-sessions"></a>Zu viele aktive Profilerstellungssitzungen

Zurzeit können Sie den Profiler für maximal vier Azure Web-Apps und Bereitstellungsslots aktivieren, die im gleichen Serviceplan ausgeführt werden. Wenn der Profiler-Webauftrag zu viele aktive Profilerstellungssitzungen meldet, verschieben Sie einige Web-Apps in einen anderen Serviceplan.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Wie kann ich feststellen, ob Application Insights Profiler ausgeführt wird?

Der Profiler wird als fortlaufender Webauftrag in der Web-App ausgeführt. Sie können die Web-App-Ressource im [Azure-Portal](https://portal.azure.com) öffnen. Überprüfen Sie im Bereich **WebJobs** den Status von **ApplicationInsightsProfiler**. Wenn er nicht ausgeführt wird, öffnen Sie **Protokolle**, um weitere Informationen zu erhalten.

### <a name="why-cant-i-find-any-stack-examples-even-though-the-profiler-is-running"></a>Warum finde ich keine Stapelbeispiele, obwohl der Profiler ausgeführt wird?

Überprüfen Sie Folgendes:

* Vergewissern Sie sich, dass Sie für Ihre Web-App mindestens einen Service-Plan mit Basic-Tarif verwenden.
* Vergewissern Sie sich, dass für Ihre Web-App mindestens Application Insights SDK 2.2 Beta aktiviert ist.
* Vergewissern Sie sich, dass für die **APPINSIGHTS_INSTRUMENTATIONKEY**-Einstellung Ihrer Web-App der gleiche Instrumentierungsschlüssel konfiguriert ist, der auch vom Application Insights SDK verwendet wird.
* Vergewissern Sie sich, dass Ihre Web-App unter .NET Framework 4.6 ausgeführt wird.
* Falls Ihre Web-App eine ASP.NET Core-Anwendung ist, prüfen Sie die [erforderlichen Abhängigkeiten](#aspnetcore).

Nach dem Start des Profilers folgt eine kurze Vorbereitungsphase, in der der Profiler aktiv mehrere Leistungsnachverfolgungen erfasst. Danach werden Leistungsnachverfolgungen vom Profiler jeweils stündlich für zwei Minuten erfasst.  

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Ich habe Azure Service Profiler verwendet. Was ist damit passiert?  

Wenn Sie Application Insights Profiler aktivieren, wird der Azure Service Profiler-Agent deaktiviert.

### <a id="double-counting"></a>Doppelte Erfassung in parallelen Threads

Manchmal übersteigt die Gesamtzeitmetrik im Stapel-Viewer die Dauer der Anforderung.

Dieser Fall kann eintreten, wenn einer Anforderung mehrere Threads zugeordnet sind, die parallel ausgeführt werden. In diesem Fall übersteigt die Gesamtzeit der Threads die verstrichene Zeit. Ein Thread wartet möglicherweise auf den Abschluss eines anderen Threads. Der Viewer versucht, solche Fälle zu erkennen und die nicht relevante Wartezeit zu ignorieren. Im Zweifelsfall wird jedoch eher zu viel angezeigt, um zu vermeiden, dass womöglich entscheidende Informationen ignoriert werden.  

Wenn Ihnen in Ihren Ablaufverfolgungen parallele Threads begegnen, bestimmen Sie die wartenden Threads, um den kritischen Pfad für die Anforderung ermitteln zu können. In den meisten Fällen wartet der Thread, der schnell in einen Wartezustand wechselt, einfach auf den Abschluss der anderen Threads. Konzentrieren Sie sich auf die anderen Threads, und ignorieren Sie die Zeit in den wartenden Threads.

### <a id="issue-loading-trace-in-viewer"></a>Keine Daten für die Profilerstellung

Überprüfen Sie Folgendes:

* Falls die Daten, die Sie anzeigen möchten, bereits mehrere Wochen alt sind, schränken Sie Ihren Zeitfilter ein, und versuchen Sie es noch mal.
* Vergewissern Sie sich, dass der Zugriff auf „https://gateway.azureserviceprofiler.net“ nicht durch Proxys oder durch eine Firewall blockiert wird.
* Vergewissern Sie sich, dass der in der App verwendete Application Insights-Instrumentierungsschlüssel dem Instrumentierungsschlüssel der Application Insights-Ressource entspricht, mit der Sie die Profilerstellung aktiviert haben. Der Schlüssel befindet sich in der Regel in „ApplicationInsights.config“, er kann sich aber auch in den Dateien „web.config“ oder „app.config“ befinden.

### <a name="error-report-in-the-profiling-viewer"></a>Fehlerbericht im Profilerstellungs-Viewer

Senden Sie ein Supportticket über das Portal. Geben Sie dabei die Korrelations-ID aus der Fehlermeldung an.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Bereitstellungsfehler: Verzeichnis nicht leer 'D:\\home\\site\\wwwroot\\App_Data\\jobs'

Wenn Sie Ihre Web-App für eine App Service-Ressource erneut bereitstellen und der Profiler aktiviert ist, wird möglicherweise eine Meldung wie die folgende angezeigt:

Verzeichnis nicht leer 'D:\\home\\site\\wwwroot\\App_Data\\jobs'

Dieser Fehler tritt auf, wenn Sie Web Deploy über Skripts oder die Visual Studio Team Services-Bereitstellungspipeline ausführen. Als Lösung fügen Sie die folgenden zusätzlichen Bereitstellungsparameter zum Web Deploy-Task hinzu:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Diese Parameter löschen den Ordner, der von Application Insights Profiler verwendet wird, und heben die Sperre der erneuten Bereitstellung auf. Sie haben keine Auswirkungen auf die Profiler-Instanz, die gerade ausgeführt wird.


## <a name="manual-installation"></a>Manuelle Installation

Wenn Sie den Profiler konfigurieren, werden an den Einstellungen der Web-App Aktualisierungen vorgenommen. Sie können die Updates manuell anwenden, wenn dies für Ihre Umgebung erforderlich ist. Angenommen, Ihre Anwendung wird in der App Service-Umgebung für PowerApps ausgeführt.

1. Öffnen Sie im Steuerungsbereich der Web-App **Einstellungen**.
2. Legen Sie die **.NET Framework-Version** auf **v4.6** fest.
3. Legen Sie **Immer bereit** auf **Ein** fest.
4. Fügen Sie die App-Einstellung __APPINSIGHTS_INSTRUMENTATIONKEY__ hinzu, und legen Sie den Wert auf den Instrumentierungsschlüssel des SDK fest.
5. Öffnen Sie **Erweiterte Tools**.
6. Wählen Sie **Los** aus, um die Kudu-Website zu öffnen.
7. Wählen Sie auf der Kudu-Website **Site extensions** (Websiteerweiterungen) aus.
8. Installieren Sie __Application Insights__ aus dem Katalog der Azure-Web-Apps.
9. Starten Sie die Web-App neu.

## <a id="aspnetcore"></a>Unterstützung von ASP.NET Core

Eine ASP.NET Core-Anwendung muss „Microsoft.ApplicationInsights.AspNetCore NuGet-Paket 2.1.0-beta6“ oder höher installieren, um mit dem Profiler arbeiten zu können. Seit dem 27. Juni 2017 unterstützen wir keine früheren Versionen mehr.

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von Azure Application Insights in Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[performance-blade-v2-examples]:./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png
[enable-profiler-banner]: ./media/app-insights-profiler/enable-profiler-banner.png
[disable-profiler-webjob]: ./media/app-insights-profiler/disable-profiler-webjob.png
[linked app services]: ./media/app-insights-profiler/linked-app-services.png
