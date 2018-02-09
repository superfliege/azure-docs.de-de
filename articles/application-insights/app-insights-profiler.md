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
ms.openlocfilehash: 5f691fb88c6764309bf012dfc65b561ec87afede
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2018
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Profilerstellung für Live-Azure-Web-Apps mit Application Insights

*Dieses Feature von Application Insights ist für Azure App Service allgemein und für Azure-Computeressourcen als Vorschauversion verfügbar.*

Ermitteln Sie bei der Verwendung von [Application Insights](app-insights-overview.md) den Zeitaufwand für die einzelnen Methoden in Ihrer Live-Webanwendung. Das Application Insights-Profilerstellungstool zeigt detaillierte Profile der aktiven Anforderungen, die von Ihrer App verarbeitet wurden, und hebt den *langsamsten Pfad* hervor, der die meiste Zeit beansprucht. Für Anforderungen mit anderen Antwortzeiten wird anhand von Stichproben ein Profil erstellt. Zusatzaufwand für Ihre Anwendung wird mithilfe verschiedener Verfahren minimiert.

Der Profiler kann momentan für ASP.NET- und ASP.NET Core-Web-Apps verwendet werden, die unter Azure App Service und mindestens mit Diensttarif **Basic** ausgeführt werden.

## <a id="installation"></a> Aktivieren des Profilers für App Services-Web-Apps
Wenn Sie die Anwendung bereits in einer App Services-Instanz veröffentlicht, im Quellcode aber nicht die Verwendung von Application Insights festgelegt haben, navigieren Sie im Azure-Portal zum App Services-Bereich, und klicken Sie auf **Überwachung > Application Insights**. Befolgen Sie die Anweisungen in diesem Bereich, um zur Überwachung der Web-App eine neue Application Insights-Ressource zu erstellen oder eine vorhandene Ressource auszuwählen. Beachten Sie, dass Profiler nur mit dem App Services-Tarif **Basic** oder einem höheren Tarif verwendet werden kann.

![Aktivieren von App Insights im App Services-Portal][appinsights-in-appservices]

Wenn Sie Zugriff auf den Projektquellcode besitzen, [installieren Sie Application Insights](app-insights-asp-net.md). Sollte es bereits installiert sein, vergewissern Sie sich, dass Sie über die neueste Version verfügen. Um auf die neueste Version zu prüfen, klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie dann **NuGet-Pakete verwalten** > **Updates** > **Alle Pakete aktualisieren** aus. Stellen Sie dann Ihre App bereit.

Eine ASP.NET Core-Anwendung muss „Microsoft.ApplicationInsights.AspNetCore NuGet-Paket 2.1.0-beta6“ oder höher installieren, um mit dem Profiler arbeiten zu können. Seit dem 27. Juni 2017 unterstützen wir keine früheren Versionen mehr.

Öffnen Sie im [Azure-Portal](https://portal.azure.com) die Application Insights-Ressource für Ihre Web-App. Wählen Sie **Leistung** > **Application Insights Profiler aktivieren** aus.

![Wählen Sie das Banner zum Aktivieren des Profilers aus][enable-profiler-banner]

Alternativ können Sie auch die Konfiguration für **Profiler** auswählen, um den Status anzuzeigen und den Profiler zu aktivieren bzw. zu deaktivieren.

![Auswählen der Profilerkonfiguration unter „Leistung“][performance-blade]

Mit Application Insights konfigurierte Web-Apps sind im Konfigurationsbereich für **Profiler** aufgeführt. Wenn Sie die obigen Schritte ausgeführt haben, sollte der Profileragent bereits installiert sein. Aktivieren Sie im Konfigurationsbereich für **Profiler** die Option **Profiler aktivieren**.

Befolgen Sie ggf. die Anweisungen zum Installieren des Profiler-Agents. Falls noch keine Web-Apps mit Application Insights konfiguriert wurden, wählen Sie **Verknüpfte Apps hinzufügen** aus.

![Optionen im Bereich „Konfigurieren“][linked app services]

Im Gegensatz zu Web-Apps, die über App Service-Pläne gehostet werden, werden auf Azure-Computeressourcen gehostete Anwendungen (z.B. Azure Virtual Machine, VM-Skalierungsgruppen, Azure Service Fabric oder Azure Cloud Services) nicht direkt von Azure verwaltet. In diesem Fall ist keine Web-App vorhanden, mit der eine Verknüpfung hergestellt werden kann. Statt eine Verknüpfung mit einer App herzustellen, wählen Sie die Schaltfläche **Profiler aktivieren** aus.

### <a name="enable-the-profiler-for-azure-compute-resources-preview"></a>Aktivieren des Profilers für Azure-Computeressourcen (Vorschauversion)

Informieren Sie sich über die [Vorschauversion von Profiler für Azure-Computeressourcen](https://go.microsoft.com/fwlink/?linkid=848155).

## <a name="view-profiler-data"></a>Anzeigen von Profiler-Daten

**Stellen Sie sicher, dass Ihre Anwendung Datenverkehr empfängt.** Beim Ausführen eines Experiments können Sie mithilfe der [Application Insights-Leistungstests](https://docs.microsoft.com/en-us/vsts/load-test/app-service-web-app-performance-test) Anforderungen an die Web-App generieren. Wenn Sie Profiler gerade erst aktiviert haben, können Sie einen kurzen Auslastungstest von ca. 15 Minuten ausführen und sollten daraufhin Profiler-Ablaufverfolgungen erhalten. Falls Profiler bereits eine Weile aktiviert ist, wird Profiler nach dem Zufallsprinzip zweimal stündlich jeweils zwei Minuten lang ausgeführt. Es wird empfohlen, einen einstündigen Auslastungstest auszuführen, um sicherzustellen, dass Sie Profiler-Beispielablaufverfolgungen erhalten.

Nachdem Ihre Anwendung Datenverkehr empfangen hat, navigieren Sie zum Blatt **Leistung** und dann zum Abschnitt **Aktion ausführen** der Seite, um Profiler-Ablaufverfolgungen anzuzeigen. Wählen Sie die Schaltfläche **Profiler-Ablaufverfolgungen** aus.

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
**clr!JIT\_New** und **clr!JIT\_Newarr1** sind Hilfsfunktionen in .NET Framework, die Arbeitsspeicher von einem verwalteten Heap zuweisen. **clr!JIT\_New** wird aufgerufen, wenn ein Objekt zugeordnet wird. **clr!JIT\_Newarr1** wird aufgerufen, wenn ein Objektarray zugeordnet wird. Diese beiden Funktionen sind in der Regel schnell und benötigen relativ wenig Zeit. Sollte **clr!JIT\_New** oder **clr!JIT\_Newarr1** auf Ihrer Zeitachse viel Zeit beanspruchen, deutet das darauf hin, dass der Code viele Objekte zuordnet und eine erhebliche Menge an Arbeitsspeicher beansprucht.

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

## <a name="limitations"></a>Einschränkungen

Die Standardaufbewahrung von Daten beträgt fünf Tage. Die maximale pro Tag erfasste Datenmenge beträgt 10 GB.

Es fallen keine Gebühren für die Verwendung des Profiler-Diensts an. Für die Nutzung des Profiler-Diensts muss Ihre Web-App mindestens im Basic-Tarif von App Service gehostet werden.

## <a name="overhead-and-sampling-algorithm"></a>Auslastung und Algorithmus für Stichprobenerstellung

Der Profiler wird stündlich nach dem Zufallsprinzip zwei Minuten auf jedem virtuellen Computer ausgeführt, der die Anwendung hostet und auf dem der Profiler für die Erfassung von Ablaufverfolgungen aktiviert ist. Während der Ausführung des Profilers erhöht sich die CPU-Auslastung des Servers um 5 % bis 15 %.
Je mehr Server zum Hosten der Anwendung zur Verfügung stehen, desto weniger wirkt sich der Profiler auf die Gesamtleistung der Anwendung aus. Die Ursache ist, dass der Profiler durch den Samplingalgorithmus immer nur auf 5 % der Server ausgeführt wird. Mehr Server sind zum Verarbeiten von Webanforderungen verfügbar, um den Servermehraufwand auszugleichen, der durch die Ausführung des Profilers verursacht wird.

## <a name="disable-the-profiler"></a>Deaktivieren von Profiler
Wenn Sie den Profiler für eine einzelne App Service-Instanz beenden oder neu starten möchten, wechseln Sie unter **Webaufträge** zur App Service-Ressource. Wenn Sie den Profiler löschen möchten, wechseln Sie zu **Erweiterungen**.

![Deaktivieren von Profiler für einen Webauftrag][disable-profiler-webjob]

Es wird empfohlen, den Profiler für alle Web-Apps zu aktivieren, um Leistungsprobleme möglichst schnell zu ermitteln.

Wenn Sie Änderungen an Ihrer Webanwendung mithilfe von WebDeploy bereitstellen, achten Sie darauf, dass der Ordner „App_Data“ bei der Bereitstellung nicht gelöscht wird. Andernfalls werden die Dateien der Profiler-Erweiterung gelöscht, wenn Sie die Webanwendung das nächste Mal in Azure bereitstellen.


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

## <a id="profileondemand"></a> Manuelles Auslösen des Profilers
Bei der Entwicklung des Profilers haben wir eine Befehlszeilenschnittstelle hinzugefügt, damit wir den Profiler für App Services testen konnten. Über diese Schnittstelle können Benutzer auch anpassen, wie der Profiler gestartet wird. Im Allgemeinen nutzt der Profiler das Kudu-System von App Service zum Verwalten der Profilerstellung im Hintergrund. Wenn Sie die Application Insights-Erweiterung installieren, erstellen wir einen fortlaufenden Webauftrag zum Hosten des Profilers. Wir nutzen dieselbe Technologie zum Erstellen eines neuen Webauftrags, den Sie an Ihre Anforderungen anpassen können.

In diesem Abschnitt wird Folgendes beschrieben:

1.  Erstellen eines Webauftrags, über den der Profiler mit einem Klick für die Dauer von zwei Minuten gestartet werden kann
2.  Erstellen eines Webauftrags, mit dem die Ausführung des Profilers geplant werden kann
3.  Festlegen von Argumenten für den Profiler


### <a name="set-up"></a>Einrichtung
Zuerst machen wir uns mit dem Dashboard des Webauftrags vertraut. Klicken Sie unter „Einstellungen“ auf die Registerkarte „Webaufträge“.

![Blatt „Webaufträge“](./media/app-insights-profiler/webjobs-blade.png)

Sie sehen, dass in diesem Dashboard alle Webaufträge angezeigt werden, die auf Ihrer Site derzeit installiert sind. Sie können den Webauftrag „ApplicationInsightsProfiler2“ anzeigen, für den der Profilerauftrag ausgeführt wird. An diesem Punkt erstellen wir unsere neuen Webaufträge für die manuelle und geplante Profilerstellung.

Zuerst rufen wir die erforderlichen Binärdateien ab.

1.  Navigieren Sie zunächst zur Kudu-Website. Klicken Sie unter der Registerkarte mit den Entwicklungstools auf die Registerkarte „Advanced Tools“ (Erweiterte Tools) mit dem Kudu-Logo. Klicken Sie auf „Go“ (Los). Eine neue Site wird geöffnet, und Sie werden automatisch angemeldet.
2.  Als Nächstes müssen wir die Profiler-Binärdateien herunterladen. Navigieren Sie oben auf der Seite über „Debug Console“ (Debugging-Konsole) > „CMD“ zum Datei-Explorer.
3.  Klicken Sie auf „site“ > „wwwroot“ > „App_Data“ > „jobs“ > „continuous“ („Site“ > „wwwroot“ > „App_Data“ > „Aufträge“ > „Fortlaufend“). Der Ordner „ApplicationInsightsProfiler2“ wird angezeigt. Klicken Sie links vom Ordner auf das Downloadsymbol. Die Datei „ApplicationInsightsProfiler2.zip“ wird heruntergeladen.
4.  Es werden alle Dateien heruntergeladen, die Sie benötigen. Es ist ratsam, vor dem Fortfahren ein sauberes Verzeichnis zu erstellen, in das dieses ZIP-Archiv verschoben werden kann.

### <a name="setting-up-the-web-job-archive"></a>Einrichten des Archivs für Webaufträge
Wenn Sie der Azure-Website einen neuen Webauftrag hinzufügen, erstellen Sie praktisch ein ZIP-Archiv, in dem „run.cmd“ enthalten ist. Mit „run.cmd“ wird das Webauftragssystem angewiesen, was beim Ausführen des Webauftrags zu tun ist. Es gibt noch andere Optionen, zu denen die Dokumentation zu Webaufträgen Informationen enthält, aber für unsere Zwecke benötigen wir keine weiteren Optionen.

1.  Erstellen Sie als Erstes einen neuen Ordner. Meinem Ordner habe ich den Namen „RunProfiler2Minutes“ gegeben.
2.  Kopieren Sie die Dateien aus dem extrahierten Ordner „ApplicationInsightProfiler2“ in diesen neuen Ordner.
3.  Erstellen Sie eine neue Datei „run.cmd“. (Ich habe diesen Arbeitsordner der Einfachheit halber zuvor in VS Code geöffnet.)
4.  Fügen Sie den Befehl `ApplicationInsightsProfiler.exe start --engine-mode immediate --single --immediate-profiling-duration 120` hinzu, und speichern Sie die Datei.
a.  Der Befehl `start` bewirkt den Start des Profilers.
b.  Mit `--engine-mode immediate` wird der Profiler angewiesen, dass sofort mit der Profilerstellung begonnen werden soll.
c.  `--single` bedeutet, dass die Ausführung gestartet und dann automatisch beendet werden soll. d.  `--immediate-profiling-duration 120` bedeutet, dass der Profiler 120 Sekunden bzw. 2 Minuten lang ausgeführt werden soll.
5.  Speichern Sie diese Datei.
6.  Archivieren Sie diesen Ordner. Sie können mit der rechten Maustaste auf den Ordner klicken und „Senden an“ > „ZIP-komprimierter Ordner“ wählen. Es wird eine ZIP-Datei mit dem Namen Ihres Ordners erstellt.

![Befehl zum Starten der Profilerstellung](./media/app-insights-profiler/start-profiler-command.png)

Wir verfügen nun über eine ZIP-Datei für Webaufträge, die wir zum Einrichten von Webaufträgen auf unserer Site verwenden können.

### <a name="add-a-new-web-job"></a>Hinzufügen eines neuen Webauftrags
Als Nächstes fügen wir auf unserer Site einen neuen Webauftrag hinzu. In diesem Beispiel wird veranschaulicht, wie Sie einen manuell ausgelösten Webauftrag hinzufügen. Nachdem Sie sich mit diesem Vorgang vertraut gemacht haben, können Sie fast die gleichen Schritte zum Hinzufügen eines geplanten Auftrags ausführen. Sie können sich selbst eingehender über nach Zeitplan ausgelöste Aufträge informieren.

1.  Navigieren Sie zum Dashboard für Webaufträge.
2.  Klicken Sie in der Symbolleiste auf den Befehl „Hinzufügen“.
3.  Geben Sie Ihrem Webauftrag einen Namen. Ich habe zur besseren Verständlichkeit einen Namen gewählt, der zum Namen des Archivs passt. Er kann unterschiedliche Versionen von „run.cmd“ umfassen.
4.  Klicken Sie im Formular im Abschnitt für den Dateiupload auf das Symbol zum Öffnen einer Datei, und suchen Sie nach der ZIP-Datei, die Sie oben erstellt haben.
5.  Wählen Sie als Typ die Option „Ausgelöst“.
6.  Wählen Sie unter „Trigger“ die Option „Manuell“.
7.  Wählen Sie „OK“, um zu speichern.

![Befehl zum Starten der Profilerstellung](./media/app-insights-profiler/create-webjob.png)

### <a name="run-the-profiler"></a>Ausführen des Profilers

Da wir jetzt über einen neuen Webauftrag verfügen, der manuell ausgelöst werden kann, können wir versuchen, ihn auszuführen.

1.  Auf einem Computer kann jeweils nur ein Prozess vom Typ „ApplicationInsightsProfiler.exe“ ausgeführt werden. Überprüfen Sie als Erstes also über dieses Dashboard, ob fortlaufende Webaufträge deaktiviert sind. Klicken Sie auf die Zeile, und wählen Sie „Beenden“. Wählen Sie in der Symbolleiste die Option „Aktualisieren“, und vergewissern Sie sich, dass für den Auftrag der Status „Beendet“ angezeigt wird.
2.  Klicken Sie auf die Zeile mit dem neuen Webauftrag, den Sie hinzugefügt haben, und wählen Sie „Ausführen“.
3.  Klicken Sie bei weiterhin ausgewählter Zeile in der Symbolleiste auf den Befehl „Protokolle“. Sie gelangen zu einem Dashboard für den Webauftrag, den Sie gestartet haben. Die aktuellen Ausführungen und die dazugehörigen Ergebnisse werden angezeigt.
4.  Klicken Sie auf die Ausführung, die Sie gerade gestartet haben.
5.  Wenn alles problemlos verläuft, sollte zu sehen sein, dass einige Diagnoseprotokolle vom Profiler eingehen. Dies ist die Bestätigung, dass die Profilerstellung gestartet wurde.

### <a name="things-to-consider"></a>Zu beachtende Aspekte

Diese Vorgehensweise ist zwar relativ einfach, aber es müssen einige Punkte beachtet werden.

1.  Da dieser Vorgang nicht von unserem Dienst verwaltet wird, haben wir keine Möglichkeit, die Binärdateien für den Agent für Ihren Webauftrag zu aktualisieren. Derzeit verfügen wir nicht über eine stabile Downloadseite für unsere Binärdateien. Wir können die aktuellen Dateien also nur abrufen, indem wir die Erweiterung aktualisieren und das Abrufen aus dem fortlaufenden Ordner wie oben durchführen.
2.  Da hierbei Befehlszeilenargumente verwendet werden, die ursprünglich für die Nutzung durch Entwickler und nicht durch Endbenutzer entworfen wurden, können sich diese Argumente in Zukunft ändern. Denken Sie beim Durchführen von Upgrades also daran. Dies sollte kein Problem darstellen, da Sie einen Webauftrag hinzufügen und ausführen und die Funktionsfähigkeit testen können. In Zukunft soll hierfür eine Benutzeroberfläche erstellt werden, damit der manuelle Prozess nicht mehr erforderlich ist, aber dieser Punkt sollte in diesem Zusammenhang beachtet werden.
3.  Das Feature „Webaufträge“ für App Services ist eindeutig: Bei der Ausführung des Webauftrags wird sichergestellt, dass Ihr Prozess über die gleichen Umgebungsvariablen und App-Einstellungen verfügt, die auch für Ihre Website gelten sollen. Dies bedeutet Folgendes: Es ist nicht erforderlich, dass Sie den Instrumentierungsschlüssel über die Befehlszeile an den Profiler übergeben, da er einfach aus der Umgebung abgerufen werden kann. Sie müssen aber einen Instrumentierungsschlüssel angeben, wenn Sie den Profiler in Ihrer Dev Box oder auf einem Computer außerhalb von App Services ausführen möchten. Hierzu können Sie das Argument `--ikey <instrumentation-key>` übergeben. Beachten Sie, dass dieser Wert mit dem Instrumentierungsschlüssel übereinstimmen muss, der von Ihrer Anwendung verwendet wird. Der Protokollausgabe des Profilers können Sie entnehmen, mit welchem iKey der Profiler gestartet wurde und ob über diesen Instrumentierungsschlüssel während der Profilerstellung Aktivitäten erkannt wurden.
4.  Die manuell ausgelösten Webaufträge können per Webhook ausgelöst werden. Sie können diese URL abrufen, indem Sie im Dashboard mit der rechten Maustaste auf den Webauftrag klicken und die Eigenschaften anzeigen oder die Eigenschaften auf der Symbolleiste auswählen, nachdem Sie den Webauftrag in der Tabelle ausgewählt haben. Da online viele Artikel zu diesem Thema verfügbar sind, gehe ich hier nicht weiter ins Detail. Es besteht aber die Möglichkeit, den Profiler über Ihre CI/CD-Pipeline (z.B. VSTS) oder beispielsweise Microsoft Flow (https://flow.microsoft.com/de-de/) auszulösen. Die Möglichkeiten sind vielfältig, je nachdem, wie komplex Sie Ihre Datei „run.cmd“ gestalten möchten (übrigens kann auch „run.ps1“ verwendet werden).  

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von Azure Application Insights in Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[appinsights-in-appservices]:./media/app-insights-profiler/AppInsights-AppServices.png
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
