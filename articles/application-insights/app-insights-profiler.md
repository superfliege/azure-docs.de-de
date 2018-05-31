---
title: Profilerstellung für Live-Web-Apps in Azure mit Application Insights Profiler | Microsoft-Dokumentation
description: Identifizieren Sie den langsamsten Pfad in Ihrem Webservercode mithilfe eines kompakten Profilers.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: mbullwin
ms.openlocfilehash: 34824401ec8d21949c5c5036a11197a09e240bd7
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "33936724"
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Profilerstellung für Live-Azure-Web-Apps mit Application Insights

*Dieses Feature von Azure Application Insights ist für das Web-Apps-Feature von Azure App Service allgemein und für Azure-Computeressourcen als Vorschauversion verfügbar. Informationen zur lokalen Verwendung von Profiler finden Sie [hier](https://docs.microsoft.com/azure/application-insights/enable-profiler-compute#enable-profiler-on-on-premises-servers).*

In diesem Artikel wird erläutert, wie viel Zeit die einzelnen Methoden Ihrer Live-Webanwendung in Anspruch nehmen, wenn Sie [Application Insights](app-insights-overview.md) verwenden. Das Tool Application Insights Profiler zeigt detaillierte Profile von Liveanforderungen, die von Ihrer Anwendung verarbeitet wurden. Profiler hebt den *langsamsten Pfad* hervor, der die meiste Zeit benötigt. Für Anforderungen mit unterschiedlichen Antwortzeiten wird anhand von Stichproben ein Profil erstellt. Indem Sie mehrere unterschiedliche Verfahren verwenden, können Sie den Mehraufwand im Zusammenhang mit der Anwendung minimieren.

Profiler kann derzeit für ASP.NET- und ASP.NET Core-Web-Apps verwendet werden, die unter Web-Apps ausgeführt werden. Zur Verwendung von Profiler ist der Diensttarif „Basic“ oder höher erforderlich.

## <a id="installation"></a> Aktivieren von Profiler für Ihre Web-Apps-Web-App
Wenn Sie die Anwendung in einer Web-App veröffentlicht haben, aber noch keine Maßnahmen im Quellcode zur Verwendung von Application Insights ergriffen haben, führen Sie folgende Schritte aus:
1. Wechseln Sie zum Bereich **App Services** im Azure-Portal.
2. Wählen Sie unter **Überwachung** die Option **Application Insights** aus, und befolgen Sie dann entweder die Anweisungen im Bereich zum Erstellen einer neuen Ressource, oder wählen Sie eine vorhandene Application Insights-Ressource zum Überwachen Ihrer Webs-App aus.

   ![Aktivieren von App Insights im App Services-Portal][appinsights-in-appservices]

3. Wenn Sie Zugriff auf den Projektquellcode besitzen, [installieren Sie Application Insights](app-insights-asp-net.md).  
   Sollte es bereits installiert sein, vergewissern Sie sich, dass Sie über die neueste Version verfügen. Um auf die neueste Version zu prüfen, klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie dann **NuGet-Pakete verwalten** > **Updates** > **Alle Pakete aktualisieren** aus. Stellen Sie dann Ihre App bereit.

Eine ASP.NET Core-Anwendung erfordert die Installation des NuGet-Pakets Microsoft.ApplicationInsights.AspNetCore 2.1.0-beta6 oder höher, um mit Profiler arbeiten zu können. Ab dem 27. Juni 2017 werden frühere Versionen werden nicht mehr unterstützt.

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) die Application Insights-Ressource für Ihre Web-App. 
2. Wählen Sie **Leistung** > **Application Insights Profiler aktivieren** aus.

   ![Wählen Sie das Banner zum Aktivieren des Profilers aus][enable-profiler-banner]

3. Alternativ können Sie auch die Konfiguration **Profiler** auswählen, um den Status anzuzeigen und Profiler zu aktivieren bzw. zu deaktivieren.

   ![Auswählen der Konfiguration „Profiler“][performance-blade]

   Mit Application Insights konfigurierte Web-Apps sind im Konfigurationsbereich für **Profiler** aufgeführt. Wenn Sie die vorherigen Schritte ausgeführt haben, sollte der Profiler-Agent installiert sein. 

4. Wählen Sie im Konfigurationsbereich **Profiler** die Option **Profiler aktivieren** aus.

5. Befolgen Sie ggf. die Anweisungen zum Installieren des Profiler-Agents. Falls noch keine Web-Apps mit Application Insights konfiguriert wurden, wählen Sie **Verknüpfte Apps hinzufügen** aus.

   ![Optionen im Bereich „Konfigurieren“][linked app services]

Im Gegensatz zu Web-Apps, die über Web-Apps-Pläne gehostet werden, werden auf Azure-Computeressourcen gehostete Anwendungen (z.B. Azure Virtual Machines, VM-Skalierungsgruppen, Azure Service Fabric oder Azure Cloud Services) nicht direkt von Azure verwaltet. In diesem Fall ist keine Web-App vorhanden, mit der eine Verknüpfung hergestellt werden kann. Statt eine Verknüpfung mit einer App herzustellen, wählen Sie die Schaltfläche **Profiler aktivieren** aus.

### <a name="enable-profiler-for-azure-compute-resources-preview"></a>Aktivieren von Profiler für Azure-Computeressourcen (Vorschau)

Informationen finden Sie in der [Vorschauversion von Profiler für Azure-Computeressourcen](https://go.microsoft.com/fwlink/?linkid=848155).

## <a name="view-profiler-data"></a>Anzeigen von Profiler-Daten

Stellen Sie sicher, dass Ihre Anwendung Datenverkehr empfängt. Beim Ausführen eines Experiments können Sie mithilfe der [Application Insights-Leistungstests](https://docs.microsoft.com/vsts/load-test/app-service-web-app-performance-test) Anforderungen an die Web-App generieren. Wenn Sie Profiler gerade erst aktiviert haben, können Sie einen kurzen Auslastungstest von ca. 15 Minuten ausführen. Dabei sollten Profiler-Ablaufverfolgungen generiert werden. Falls Profiler bereits eine Zeit lang aktiviert ist, wird Profiler nach dem Zufallsprinzip zweimal stündlich jeweils zwei Minuten lang ausgeführt. Es wird empfohlen, einen einstündigen Auslastungstest auszuführen, um sicherzustellen, dass Sie Profiler-Beispielablaufverfolgungen erhalten.

Nachdem Ihre Anwendung Datenverkehr empfangen hat, wechseln Sie zum Bereich **Leistung**, wählen Sie **Aktion ausführen** aus, um Profiler-Ablaufverfolgungen anzuzeigen, und wählen Sie dann die Schaltfläche **Profiler-Überwachungen** aus.

![Profiler-Ablaufverfolgungen im Application Insights-Bereich für die Leistungsvorschau][performance-blade-v2-examples]

Wählen Sie ein Beispiel aus, um eine Aufschlüsselung des Zeitaufwands für die Anforderung auf Codeebene anzuzeigen.

![Ablaufverfolgungs-Explorer von Application Insights][trace-explorer]

Im Ablaufverfolgungs-Explorer werden die folgenden Informationen angezeigt:

* **Langsamsten Pfad anzeigen**: Öffnet den größten Blattknoten (oder etwas in der Nähe). In den meisten Fällen befindet sich dieser Knoten in der Nähe eines Leistungsengpasses.
* **Bezeichnung**: Der Name der Funktion oder des Ereignisses. Die Struktur enthält eine Mischung aus Code und aufgetretenen Ereignissen (wie SQL- und HTTP-Ereignissen). Das oberste Ereignis stellt die Gesamtdauer der Anforderung dar.
* **Verstrichen**: Das Zeitintervall zwischen Beginn und Ende des Vorgangs.
* **Wann:** der Zeitpunkt, an dem die Funktion oder das Ereignis relativ zu anderen Funktionen aktiv war.

## <a name="how-to-read-performance-data"></a>Lesen von Leistungsdaten

Der Dienst-Profiler von Microsoft analysiert die Leistung Ihrer Anwendung mithilfe einer Kombination aus Samplingmethoden und Instrumentierung. Bei der ausführlichen Erfassung nimmt der Dienstprofiler jede Millisekunde Stichproben des Anweisungszeigers der einzelnen Computer-CPUs. Jede Stichprobe erfasst die vollständige Aufrufliste des Threads, der gerade ausgeführt wird. Sie enthält ausführliche Informationen zu diesem Thread, sowohl auf hoher als auch auf niedriger Abstraktionsebene. Darüber hinaus erfasst der Dienst-Profiler auch andere Ereignisse zur Nachverfolgung von Aktivitätskorrelation und Kausalität, dazu gehören Kontextwechsel-, TPL- (Task Parallel Library) und Threadpoolereignisse.

Die Aufrufliste in der Zeitachsenansicht ist das Ergebnis des oben erwähnten Samplings und der Instrumentierung. Da jede Stichprobe die vollständige Aufrufliste des Threads erfasst, enthält sie auch Code aus Microsoft .NET Framework sowie aus anderen Frameworks, auf die Sie verweisen.

### <a id="jitnewobj"></a>Objektzuordnung („clr!JIT\_New“ oder „clr!JIT\_Newarr1“)
**clr!JIT\_New** und **clr!JIT\_Newarr1** sind Hilfsfunktionen in .NET Framework, die Arbeitsspeicher von einem verwalteten Heap zuweisen. **clr!JIT\_New** wird aufgerufen, wenn ein Objekt zugeordnet wird. **clr!JIT\_Newarr1** wird aufgerufen, wenn ein Objektarray zugeordnet wird. Diese beiden Funktionen sind in der Regel schnell und benötigen relativ wenig Zeit. Sollte **clr!JIT\_New** oder **clr!JIT\_Newarr1** auf Ihrer Zeitachse viel Zeit beanspruchen, deutet das darauf hin, dass der Code viele Objekte zuordnet und eine erhebliche Menge an Arbeitsspeicher beansprucht.

### <a id="theprestub"></a>Laden von Code (clr!ThePreStub)
**clr!ThePreStub** ist eine Hilfsfunktion in .NET Framework, die den Code für die erstmalige Ausführung vorbereitet. Das schließt normalerweise auch die JIT-Kompilierung (Just-In-Time) ein. Während der Lebensdauer eines Prozesses sollte **clr!ThePreStub** für jede C#-Methode höchstens einmal aufgerufen werden.

Wenn **clr!ThePreStub** für eine Anforderung viel Zeit beansprucht, ist dies ein Hinweis darauf, dass die Anforderung der erste ist, die diese Methode ausführt. Das Laden der ersten Methode durch .NET Framework Runtime dauert relativ lange. Verwenden Sie ggf. einen Vorbereitungsprozess, der diesen Teil des Codes ausführt, bevor Ihre Benutzer darauf zugreifen, oder führen Sie Native Image Generator (ngen.exe) für Ihre Assemblys aus.

### <a id="lockcontention"></a> („clr!JITutil\_MonContention“ oder „clr!JITutil\_MonEnterWorker“)
**clr!JITutil\_MonContention** oder **clr!JITutil\_MonEnterWorker** gibt an, dass der aktuelle Thread auf die Aufhebung einer Sperre wartet. Dieser Text wird üblicherweise angezeigt, wenn Sie eine C#-Anweisung vom Typ **LOCK** ausführen, die **Monitor.Enter**-Methode aufrufen oder eine Methode mit dem **MethodImplOptions.Synchronized**-Attribut aufrufen. Ein Sperrkonflikt ist meist darauf zurückzuführen, dass Thread _A_ eine Sperre abruft und Thread _B_ versucht, die gleiche Sperre abzurufen, bevor sie von Thread _A_ wieder aufgehoben wurde.

### <a id="ngencold"></a>Laden von Code ([COLD])
Falls der Methodenname **[COLD]** enthält (Beispiel: **mscorlib.ni![COLD]System.Reflection.CustomAttribute.IsDefined**), führt .NET Framework Runtime zum ersten Mal Code aus, der nicht durch die <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">profilgesteuerte Optimierung</a> optimiert wurde. Während der Lebensdauer des Prozesses sollte dies für jede Methode höchstens einmal angezeigt werden.

Falls das Laden von Code bei einer Anforderung sehr lange dauert, deutet das darauf hin, dass die Anforderung die erste Anforderung ist, die den nicht optimierten Teil der Methode ausführt. Verwenden Sie ggf. einen Vorbereitungsprozess, der diesen Teil des Codes ausführt, bevor Ihre Benutzer darauf zugreifen.

### <a id="httpclientsend"></a>Senden von HTTP-Anforderungen
Methoden wie **HttpClient.Send** deuten darauf hin, dass der Code auf den Abschluss einer HTTP-Anforderung wartet.

### <a id="sqlcommand"></a>Datenbankvorgang
Methoden wie **SqlCommand.Execute** deuten darauf hin, dass der Code auf den Abschluss eines Datenbankvorgangs wartet.

### <a id="await"></a>Warten (AWAIT\_TIME)
**AWAIT\_TIME** deutet darauf hin, dass der Code auf den Abschluss einer anderen Aufgabe wartet. Das ist normalerweise bei C#-Anweisungen vom Typ **AWAIT** der Fall. Wenn der Code eine C#-Anweisung vom Typ **AWAIT** ausführt, wird der Thread entladen und übergibt die Steuerung wieder an den Threadpool, sodass kein Thread auf den Abschluss der **AWAIT**-Anweisung warten muss und dadurch blockiert wird. Logisch betrachtet ist allerdings der Thread, der die **AWAIT**-Anweisung ausgeführt hat, blockiert und wartet auf den Abschluss des Vorgangs. Die **AWAIT\_TIME**-Anweisung gibt die blockierte Zeit an, für die auf den Abschluss der Aufgabe gewartet wird.

### <a id="block"></a>Blockierte Zeit
**BLOCKED_TIME** gibt an, dass der Code darauf wartet, dass eine andere Ressource verfügbar ist. Beispielsweise könnte er auf ein Synchronisierungsobjekt warten, darauf, dass ein Thread verfügbar ist, oder auf den Abschluss einer Anforderung.

### <a id="cpu"></a>CPU-Zeit
Die CPU ist mit der Ausführung der Anweisungen beschäftigt.

### <a id="disk"></a>Datenträgerzeit
Die Anwendung führt Datenträgervorgänge aus.

### <a id="network"></a>Netzwerkzeit
Die Anwendung führt Netzwerkvorgänge aus.

### <a id="when"></a>Wann-Spalte
Die Spalte **Wann** ist eine Visualisierung der Abweichungen INKLUSIVER Stichproben für einen Knoten im Zeitverlauf. Der Gesamtbereich der Anforderung wird in 32 Zeitrahmen unterteilt. Die inklusiven Stichproben für diesen Knoten werden in diesen 32 Zeitrahmen akkumuliert. Jeder Zeitrahmen wird als Balken dargestellt. Die Höhe der Balken stellt einen skalierten Wert dar. Bei Knoten, die mit **CPU_TIME** oder **BLOCKED_TIME** gekennzeichnet sind oder bei denen ein offensichtlicher Zusammenhang mit der Nutzung einer Ressource (CPU, Datenträger, Thread) besteht, stellt der Balken die Nutzung einer dieser Ressourcen für den Zeitraum des entsprechenden Zeitrahmens dar. Für diese Metriken ist es möglich, durch die Nutzung mehrerer Ressourcen einen Wert von mehr als 100 % zu erhalten. Wenn Sie also beispielsweise innerhalb eines Intervalls im Schnitt zwei CPUs verwenden, erhalten Sie 200 %.

## <a name="limitations"></a>Einschränkungen

Die Standarddauer für die Datenaufbewahrung beträgt fünf Tage. Die maximal pro Tag erfasste Datenmenge beträgt 10 GB.

Es fallen keine Gebühren für die Verwendung des Profiler-Diensts an. Für die Nutzung des Profiler-Diensts muss Ihre Web-App mindestens im Tarif „Basic“ von Web-Apps gehostet werden.

## <a name="overhead-and-sampling-algorithm"></a>Auslastung und Algorithmus für Stichprobenerstellung

Profiler wird stündlich nach dem Zufallsprinzip zwei Minuten auf jedem virtuellen Computer ausgeführt, der die Anwendung hostet und auf dem Profiler für die Erfassung von Ablaufverfolgungen aktiviert ist. Wenn Profiler ausgeführt wird, tritt auf dem Server eine zusätzliche CPU-Auslastung von 5 % bis 15 % auf.

Je mehr Server zum Hosten der Anwendung zur Verfügung stehen, desto weniger wirkt sich Profiler auf die Gesamtleistung der Anwendung aus. Die Ursache ist, dass Profiler durch den Samplingalgorithmus immer nur auf 5 % der Server ausgeführt wird. Damit stehen mehr Server zum Verarbeiten von Webanforderungen zur Verfügung, um den Servermehraufwand durch die Ausführung von Profiler auszugleichen.

## <a name="disable-profiler"></a>Deaktivieren von Profiler
Wenn Sie Profiler für eine einzelne Web-Apps-Instanz beenden oder neu starten möchten, wechseln Sie unter **Webaufträge** zur Web-Apps-Ressource. Wenn Sie Profiler löschen möchten, wechseln Sie zu **Erweiterungen**.

![Deaktivieren von Profiler für einen Webauftrag][disable-profiler-webjob]

Es wird empfohlen, Profiler für alle Web-Apps zu aktivieren, um Leistungsprobleme möglichst schnell zu ermitteln.

Wenn Sie Änderungen an Ihrer Webanwendung mithilfe von WebDeploy bereitstellen, achten Sie darauf, dass der Ordner „App_Data“ bei der Bereitstellung nicht gelöscht wird. Andernfalls werden die Dateien der Profiler-Erweiterung gelöscht, wenn Sie die Webanwendung das nächste Mal in Azure bereitstellen.


## <a id="troubleshooting"></a>Problembehandlung

### <a name="too-many-active-profiling-sessions"></a>Zu viele aktive Profilerstellungssitzungen

Zurzeit können Sie Profiler für maximal vier Azure Web-Apps und Bereitstellungsslots aktivieren, die im selben Serviceplan ausgeführt werden. Wenn der Profiler-Webauftrag zu viele aktive Profilerstellungssitzungen meldet, verschieben Sie einige Web-Apps in einen anderen Serviceplan.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Wie kann ich feststellen, ob Application Insights Profiler ausgeführt wird?

Profiler wird als fortlaufender Webauftrag in der Web-App ausgeführt. Sie können die Web-App-Ressource im [Azure-Portal](https://portal.azure.com) öffnen. Überprüfen Sie im Bereich **WebJobs** den Status von **ApplicationInsightsProfiler**. Wenn er nicht ausgeführt wird, öffnen Sie **Protokolle**, um weitere Informationen zu erhalten.

### <a name="why-cant-i-find-any-stack-examples-even-though-profiler-is-running"></a>Warum finde ich keine Stapelbeispiele, obwohl Profiler ausgeführt wird?

Überprüfen Sie Folgendes:

* Vergewissern Sie sich, dass Sie für Ihre Web-App mindestens einen Service-Plan mit Basic-Tarif verwenden.
* Vergewissern Sie sich, dass für Ihre Web-App mindestens Application Insights SDK 2.2 Beta aktiviert ist.
* Vergewissern Sie sich, dass für die **APPINSIGHTS_INSTRUMENTATIONKEY**-Einstellung Ihrer Web-App der gleiche Instrumentierungsschlüssel konfiguriert ist, der auch vom Application Insights SDK verwendet wird.
* Vergewissern Sie sich, dass Ihre Web-App unter .NET Framework 4.6 ausgeführt wird.
* Falls Ihre Web-App eine ASP.NET Core-Anwendung ist, prüfen Sie die [erforderlichen Abhängigkeiten](#aspnetcore).

Nach dem Start von Profiler folgt eine kurze Vorbereitungsphase, in der Profiler aktiv mehrere Leistungsnachverfolgungen erfasst. Danach werden Leistungsnachverfolgungen jeweils stündlich für zwei Minuten erfasst.

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Ich habe Azure Service Profiler verwendet. Was ist damit passiert?

Wenn Sie Application Insights Profiler aktivieren, wird der Azure Service Profiler-Agent deaktiviert.

### <a id="double-counting"></a>Doppelte Erfassung in parallelen Threads

Manchmal übersteigt die Gesamtzeitmetrik im Stapel-Viewer die Dauer der Anforderung.

Diese Situation kann eintreten, wenn einer Anforderung mehrere Threads zugeordnet sind, die parallel ausgeführt werden. In diesem Fall übersteigt die Gesamtzeit der Threads die verstrichene Zeit. Ein Thread wartet möglicherweise auf den Abschluss eines anderen Threads. Der Viewer versucht, solche Fälle zu erkennen und die nicht relevante Wartezeit zu ignorieren. Im Zweifelsfall werden jedoch eher zu viele Informationen angezeigt, um zu vermeiden, dass womöglich entscheidende Informationen ausgelassen werden.

Wenn Ihnen in Ihren Ablaufverfolgungen parallele Threads auffallen, bestimmen Sie die wartenden Threads, um den kritischen Pfad für die Anforderung ermitteln zu können. In den meisten Fällen wartet der Thread, der schnell in einen Wartezustand wechselt, einfach auf den Abschluss der anderen Threads. Konzentrieren Sie sich auf die anderen Threads, und ignorieren Sie die Zeit in den wartenden Threads.

### <a id="issue-loading-trace-in-viewer"></a>Keine Daten für die Profilerstellung

Überprüfen Sie Folgendes:

* Falls die Daten, die Sie anzeigen möchten, bereits mehrere Wochen alt sind, schränken Sie Ihren Zeitfilter ein, und versuchen Sie es noch mal.
* Stellen Sie sicher, dass der Zugriff auf https://gateway.azureserviceprofiler.net nicht durch Proxys oder durch eine Firewall blockiert wird.
* Stellen Sie sicher, dass der in der App verwendete Application Insights-Instrumentierungsschlüssel dem Instrumentierungsschlüssel der Application Insights-Ressource entspricht, mit der Sie die Profilerstellung aktiviert haben. Der Schlüssel befindet sich in der Regel in der Datei „ApplicationInsights.config“, er kann sich aber auch in einer der Dateien „web.config“ oder „app.config“ befinden.

### <a name="error-report-in-the-profiling-viewer"></a>Fehlerbericht im Profilerstellungs-Viewer

Senden Sie ein Supportticket über das Portal. Geben Sie dabei die Korrelations-ID aus der Fehlermeldung an.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Bereitstellungsfehler: Verzeichnis nicht leer 'D:\\home\\site\\wwwroot\\App_Data\\jobs'

Wenn Sie Ihre Web-App für eine Web-Apps-Ressource erneut bereitstellen und Profiler aktiviert ist, wird möglicherweise eine Meldung wie die folgende angezeigt:

*Verzeichnis nicht leer „D:\\home\\site\\wwwroot\\App_Data\\jobs“*

Dieser Fehler tritt auf, wenn Sie Web Deploy über Skripts oder die Visual Studio Team Services-Bereitstellungspipeline ausführen. Als Lösung fügen Sie die folgenden zusätzlichen Bereitstellungsparameter zum Web Deploy-Task hinzu:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Diese Parameter löschen den Ordner, der von Application Insights Profiler verwendet wird, und heben die Sperre der erneuten Bereitstellung auf. Sie haben keine Auswirkungen auf die Profiler-Instanz, die gerade ausgeführt wird.


## <a name="manual-installation"></a>Manuelle Installation

Wenn Sie Profiler konfigurieren, werden an den Einstellungen der Web-App Aktualisierungen vorgenommen. Sie können die Updates manuell anwenden, wenn dies für Ihre Umgebung erforderlich ist. Beispiel: Ihre Anwendung wird in einer Web-Apps-Umgebung für PowerApps ausgeführt.

1. Öffnen Sie im **Steuerungsbereich der Web-App** die Option **Einstellungen**.
2. Legen Sie die **.NET Framework-Version** auf **v4.6** fest.
3. Legen Sie **Immer bereit** auf **Ein** fest.
4. Fügen Sie die App-Einstellung **APPINSIGHTS_INSTRUMENTATIONKEY** hinzu, und legen Sie den Wert auf den Instrumentierungsschlüssel des SDK fest.
5. Öffnen Sie **Erweiterte Tools**.
6. Wählen Sie **Los** aus, um die Kudu-Website zu öffnen.
7. Wählen Sie auf der Kudu-Website **Site extensions** (Websiteerweiterungen) aus.
8. Installieren Sie **Application Insights** aus dem Katalog der Azure-Web-Apps.
9. Starten Sie die Web-App neu.

## <a id="profileondemand"></a> Manuelles Auslösen von Profiler
Bei der Entwicklung von Profiler wurde eine Befehlszeilenschnittstelle hinzugefügt, damit Profiler für App Services getestet werden konnte. Über diese Schnittstelle können Benutzer auch anpassen, wie Profiler gestartet wird. Im Allgemeinen nutzt Profiler das Kudu-System von Web-Apps zum Verwalten der Profilerstellung im Hintergrund. Wenn Sie die Application Insights-Erweiterung installieren, erstellen wir einen fortlaufenden Webauftrag zum Hosten von Profiler. Wir nutzen dieselbe Technologie zum Erstellen eines neuen Webauftrags, den Sie an Ihre Anforderungen anpassen können.

In diesem Abschnitt wird Folgendes beschrieben:

* Erstellen eines Webauftrags, über den Profiler mit einem Klick für die Dauer von zwei Minuten gestartet werden kann
* Erstellen eines Webauftrags, mit dem die Ausführung von Profiler geplant werden kann
* Festlegen von Argumenten für Profiler


### <a name="set-up"></a>Einrichtung
Machen Sie sich zuerst mit dem Dashboard des Webauftrags vertraut. Klicken Sie unter **Einstellungen** auf die Registerkarte **WebJobs**.

![Blatt „Webaufträge“](./media/app-insights-profiler/webjobs-blade.png)

Sie sehen, dass in diesem Dashboard alle Webaufträge angezeigt werden, die in Ihrer Website derzeit installiert sind. Sie können den Webauftrag „ApplicationInsightsProfiler2“ anzeigen, für den der Profiler-Auftrag ausgeführt wird. An diesem Punkt erstellen wir neue Webaufträge für die manuelle und geplante Profilerstellung.

Um die benötigten Binärdateien zu erhalten, führen Sie folgende Schritte aus:

1.  Klicken Sie auf der Kudu-Website unter der Registerkarte **Development tools** (Entwicklungstools) auf die Registerkarte **Advanced Tools** (Erweiterte Tools) mit dem Kudu-Logo, und wählen Sie dann **Go** (Los) aus.  
   Eine neue Website wird geöffnet, und Sie werden automatisch angemeldet.
2.  Um die Profiler-Binärdateien herunterzuladen, wechseln Sie über **Debugging-Konsole** > **CMD** im oberen Bereich der Seite zum Datei-Explorer.
3.  Wählen Sie **Site** > **wwwroot** > **App_Data** > **Jobs** > **Continuous** aus.  
   Der Ordner *ApplicationInsightsProfiler2* wird angezeigt. 
4. Wählen Sie links vom Ordner das Symbol **Herunterladen** aus.  
   Dadurch wird die Datei *ApplicationInsightsProfiler2.zip* heruntergeladen. Es ist ratsam, ein leeres Verzeichnis zu erstellen, in das dieses ZIP-Archiv verschoben werden kann.

### <a name="setting-up-the-web-job-archive"></a>Einrichten des Archivs für Webaufträge
Wenn Sie der Azure-Website einen neuen Webauftrag hinzufügen, erstellen Sie letztlich ein ZIP-Archiv, in dem die Datei *run.cmd* enthalten ist. Mit der Datei *run.cmd* wird das Webauftragssystem angewiesen, was beim Ausführen des Webauftrags zu erledigen ist.

1.  Erstellen Sie einen neuen Ordner (z.B. *RunProfiler2Minutes*).
2.  Kopieren Sie die Dateien aus dem extrahierten Ordner *ApplicationInsightProfiler2* in diesen neuen Ordner.
3.  Erstellen Sie eine neue Datei *run.cmd*.  
    Der Einfachheit halber können Sie den Arbeitsordner in Visual Studio Code öffnen, bevor Sie beginnen.
4.  Fügen Sie in der Datei den Befehl `ApplicationInsightsProfiler.exe start --engine-mode immediate --single --immediate-profiling-duration 120` hinzu. Die Befehle lassen sich wie folgt beschreiben:

    * `start`: weist Profiler an, zu starten.  
    * `--engine-mode immediate`: weist Profiler an, sofort mit der Profilerstellung zu beginnen.  
    * `--single`: weist Profiler an, die Ausführung zu beginnen und dann automatisch zu beenden.  
    * `--immediate-profiling-duration 120`: weist Profiler an, die Ausführung für 120 Sekunden (zwei Minuten) durchzuführen.

5.  Speichern Sie die Änderungen.
6.  Archivieren Sie den Ordner, indem Sie mit der rechten Maustaste auf ihn klicken und dann **Senden an** > **ZIP-komprimierter Ordner** auszuwählen.  
   Hierdurch wird eine ZIP-Datei mit dem Namen Ihres Ordners erstellt.

![Befehl zum Starten von Profiler](./media/app-insights-profiler/start-profiler-command.png)

Sie haben nun eine ZIP-Datei für einen Webauftrag erstellt, die Sie zum Einrichten von Webaufträgen in Ihrer Website verwenden können.

### <a name="add-a-new-web-job"></a>Hinzufügen eines neuen Webauftrags
In diesem Abschnitt fügen Sie auf der Website einen neuen Webauftrag hinzu. Im folgenden Beispiel wird veranschaulicht, wie Sie einen manuell ausgelösten Webauftrag hinzufügen. Nachdem Sie den manuell ausgelösten Webauftrag hinzugefügt haben, ist der Prozess mit dem für einen geplanten Webauftrag nahezu identisch.

1.  Navigieren Sie zum Dashboard **Webaufträge**.
2.  Wählen Sie auf der Symbolleiste **Hinzufügen** aus.
3.  Geben Sie einen Namen für den Webauftrag ein.  
    Aus Gründen der Übersichtlichkeit kann es hilfreich sein, den Namen Ihres Archivs anzugeben und dieses zu öffnen, wenn Sie über verschiedene Versionen der Datei *run.cmd* verfügen.
4.  Wählen Sie im Bereich **Dateiupload** des Formulars das Symbol **Datei öffnen** aus, und suchen Sie nach der ZIP-Datei, die Sie im vorherigen Abschnitt erstellt haben.

    a.  Wählen Sie im Feld **Typ** die Option **Ausgelöst** aus.  
    b.  Wählen Sie im Feld **Trigger** die Option **Manuell** aus.

5.  Klicken Sie auf **OK**.

![Befehl zum Starten von Profiler](./media/app-insights-profiler/create-webjob.png)

### <a name="run-profiler"></a>Ausführen von Profiler

Nun verfügen Sie über einen neuen Webauftrag, den Sie manuell auslösen können, und können versuchen, ihn mithilfe der Anweisungen in diesem Abschnitt auszuführen.

Auf einem Computer kann jeweils nur ein Prozess vom Typ *ApplicationInsightsProfiler.exe* ausgeführt werden. Deaktivieren Sie daher vor dem Beginn über dieses Dashboard den Webauftrag *Continuous*. 
1. Wählen Sie die Zeile mit dem neuen Webauftrag aus, und wählen Sie dann **Beenden** aus. 
2. Wählen Sie dann auf der Symbolleiste die Option **Aktualisieren** aus, und vergewissern Sie sich, dass für den Auftrag der Status „Beendet“ angezeigt wird.
3. Wählen Sie die Zeile mit dem neuen Webauftrag und dann **Ausführen** aus.
4. Wählen Sie mit der weiterhin ausgewählten Zeile auf der Symbolleiste den Befehl **Protokolle** aus.  
    Diese Aktion öffnet ein Webauftrags-Dashboard für den neuen Webauftrag und listet die letzten Ausführungen und ihre Ergebnisse auf.
5. Wählen Sie die Instanz der Ausführung aus, die Sie gerade gestartet haben.  
    Wenn Sie den neuen Webauftrag erfolgreich ausgelöst haben, können Sie einige Diagnoseprotokolle von Profiler anzeigen, die bestätigen, dass die Profilerstellung gestartet wurde.

### <a name="things-to-consider"></a>Zu beachtende Aspekte

Obwohl diese Vorgehensweise relativ einfach ist, sollten Sie folgende Punkte beachten:

* Da der Webauftrag nicht von unserem Dienst verwaltet wird, können wir die Binärdateien für den Agent für Ihren Webauftrag nicht aktualisieren. Derzeit steht keine stabile Downloadseite für unsere Binärdateien zur Verfügung. Die aktuellen Binärdateien können also nur durch Aktualisieren Ihrer Erweiterung und Abrufen aus dem Ordner *Continuous* abgerufen werden, wie Sie dies in den vorherigen Schritten erledigt haben.

* Da in diesem Prozess Befehlszeilenargumente genutzt werden, die ursprünglich für Entwickler und nicht für Endbenutzer entwickelt wurden, können sich die Argumente in der Zukunft ändern. Achten Sie auf mögliche Änderungen, wenn Sie ein Upgrade durchführen. Dies sollte kein allzu großes Problem darstellen, da Sie einen Webauftrag hinzufügen, ausführen und testen können, um sicherzustellen, dass er funktioniert. Wir werden letztlich eine Benutzeroberfläche entwickeln, die diese Aufgaben ohne manuelles Eingreifen übernimmt.

* Das Webauftragsfeature von Web-Apps ist auf Eindeutigkeit ausgelegt. Bei der Ausführung des Webauftrags wird sichergestellt, dass Ihr Prozess über die gleichen Umgebungsvariablen und App-Einstellungen verfügt, die auch für Ihre Website gelten sollen. Dies bedeutet, dass Sie den Instrumentierungsschlüssel nicht über die Befehlszeile an Profiler übergeben müssen. Profiler sollte den Instrumentierungsschlüssel aus der Umgebung abrufen. Sie müssen jedoch einen Instrumentierungsschlüssel angeben, wenn Sie den Profiler auf Ihrem Entwicklungscomputer oder auf einem Computer außerhalb von Web-Apps ausführen möchten. Hierzu können Sie das Argument `--ikey <instrumentation-key>` übergeben. Dieser Wert muss mit dem Instrumentierungsschlüssel übereinstimmen, der von Ihrer Anwendung verwendet wird. Der Protokollausgabe von Profiler können Sie entnehmen, mit welchem Instrumentierungsschlüssel Profiler gestartet wurde und ob über diesen Schlüssel während der Profilerstellung Aktivitäten erkannt wurden.

* Manuell ausgelöste Webaufträge können per Webhook ausgelöst werden. Sie können diese URL nun abrufen, indem Sie auf dem Dashboard mit der rechten Maustaste auf den Webauftrag klicken und die Eigenschaften anzeigen. Alternativ können Sie auch auf der Symbolleiste **Eigenschaften** auswählen, nachdem Sie den Webauftrag in der Tabelle ausgewählt haben. Diese Herangehensweise eröffnet endlose Möglichkeiten – etwa das Auslösen von Profiler über Ihre CI/CD-Pipeline (wie VSTS) oder über eine Lösung wie Microsoft Flow (https://flow.microsoft.com/en-us/). Letztlich hängt Ihre Auswahl davon ab, wie komplex Sie Ihre Datei *run.cmd* gestalten möchten (bei der es sich auch um die Datei *run.ps1* handeln kann), aber die Flexibilität ist gegeben.

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
