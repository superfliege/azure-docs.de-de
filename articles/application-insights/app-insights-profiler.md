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
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 07/13/2018
ms.author: mbullwin
ms.openlocfilehash: e4712b94be94eb6d4cf363fc120b72c74f29f0a2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39057922"
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Profilerstellung für Live-Azure-Web-Apps mit Application Insights

Dieses Feature von Azure Application Insights ist normalerweise für das Feature „Web-Apps“ von Azure App Service verfügbar und befindet sich in der Vorschau für Azure-Computeressourcen. Informationen zur [lokalen Verwendung von Profiler](https://docs.microsoft.com/azure/application-insights/enable-profiler-compute#enable-profiler-on-on-premises-servers) finden Sie hier.

In diesem Artikel wird erläutert, wie viel Zeit die einzelnen Methoden Ihrer Live-Webanwendung in Anspruch nehmen, wenn Sie [Application Insights](app-insights-overview.md) verwenden. Das Tool Application Insights Profiler zeigt detaillierte Profile von Liveanforderungen, die von Ihrer Anwendung verarbeitet wurden. Profiler hebt den *langsamsten Pfad* hervor, der die meiste Zeit benötigt. Für Anforderungen mit unterschiedlichen Antwortzeiten wird anhand von Stichproben ein Profil erstellt. Indem Sie mehrere unterschiedliche Verfahren verwenden, können Sie den Mehraufwand im Zusammenhang mit der Anwendung minimieren.

Profiler kann derzeit für ASP.NET- und ASP.NET Core-Web-Apps verwendet werden, die unter Web-Apps ausgeführt werden. Zur Verwendung von Profiler ist der Diensttarif „Basic“ oder höher erforderlich.

## <a id="installation"></a> Aktivieren von Profiler für Ihre Web-Apps

Nachdem Sie eine Web-App unabhängig davon bereitgestellt haben, ob Sie das App-Insights-SDK im Quellcode berücksichtigt haben, führen Sie folgende Schritte aus:

1. Wechseln Sie zum Bereich **App Services** im Azure-Portal.
2. Navigieren Sie zum Bereich **Einstellungen > Überwachung**.

   ![Aktivieren von App Insights im App Services-Portal](./media/app-insights-profiler/AppInsights-AppServices.png)

3. Befolgen Sie entweder die Anweisungen im Bereich zum Erstellen einer neuen Ressource, oder wählen Sie eine vorhandene App Insights-Ressource zum Überwachen Ihrer Webs-App aus. Übernehmen Sie alle Standardoptionen. **Diagnose auf Codeebene** ist standardmäßig eingeschaltet und aktiviert Profiler.

   ![Hinzufügen der App Insights-Websiteerweiterung][Enablement UI]

4. Profiler ist jetzt mit der App Insights-Websiteerweiterung installiert und wurde mit einer App Services-App-Einstellung aktiviert.

    ![App-Einstellung für Profiler][profiler-app-setting]

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

**clr!JITutil\_MonContention** oder **clr!JITutil\_MonEnterWorker** gibt an, dass der aktuelle Thread auf die Aufhebung einer Sperre wartet. Dieser Text wird häufig angezeigt, wenn Sie eine C#-Anweisung vom Typ **LOCK** ausführen, die Methode **Monitor.Enter** aufrufen oder eine Methode mit dem Attribut **MethodImplOptions.Synchronized** aufrufen. Ein Sperrkonflikt ist meist darauf zurückzuführen, dass Thread _A_ eine Sperre abruft und Thread _B_ versucht, die gleiche Sperre abzurufen, bevor sie von Thread _A_ wieder aufgehoben wurde.

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

Wenn Sie Profiler für eine einzelne Instanz einer Web-App beenden oder neu starten möchten, wechseln Sie unter **Webaufträge** zur Web-Apps-Ressource. Wenn Sie Profiler löschen möchten, wechseln Sie zu **Erweiterungen**.

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

Profiler kann manuell mit einem Klick auf eine Schaltfläche ausgelöst werden. Nehmen wir an, dass Sie einen Webleistungstest ausführen. Sie benötigen Ablaufverfolgungen, um besser zu verstehen, wie Ihre Web-App unter Last ausgeführt wird. Es ist von entscheidender Bedeutung, dass Sie kontrollieren können, wann Ablaufverfolgungen erfasst werden, da Sie wissen, wann der Auslastungstest ausgeführt wird, aber das Intervall für zufällige Stichproben diesen möglicherweise verpasst.
Die folgenden Schritte veranschaulichen das Szenario:

### <a name="optional-step-1-generate-traffic-to-your-web-app-by-starting-a-web-performance-test"></a>(Optional) Schritt 1: Generieren Sie Datenverkehr zu Ihrer Web-App, indem Sie einen Webleistungstest starten

Wenn Ihre Web-App bereits über eingehenden Datenverkehr verfügt, oder wenn Sie einfach nur manuell Datenverkehr generieren möchten, überspringen Sie diesen Abschnitt, und fahren Sie mit Schritt 2 fort.

Navigieren Sie zum Application Insights-Portal, und klicken Sie auf **Konfigurieren > Leistungstests**. Klicken Sie auf „Neu“, um einen neuen Leistungstest zu starten.
![Neuen Leistungstest erstellen][create-performance-test]

Konfigurieren Sie im Bereich **Neuer Leistungstest** die Testziel-URL. Übernehmen Sie alle Standardeinstellungen, und starten Sie die Ausführung des Auslastungstests.

![Konfigurieren von Auslastungstests][configure-performance-test]

Sie sehen, dass der neue Test zuerst in die Warteschlange eingereiht wird. Anschließend folgt der Status „In Bearbeitung“.

![Auslastungstest übermittelt und in die Warteschlange gestellt][load-test-queued]

![Ausführung des Auslastungstests ist in Bearbeitung][load-test-in-progress]

### <a name="step-2-start-profiler-on-demand"></a>Schritt 2: Starten von Profiler auf Anforderung

Sobald der Auslastungstest ausgeführt wird, können wir Profiler starten, um Ablaufverfolgungen in der Web-App zu erfassen, während diese die Last empfängt.
Navigieren Sie zum Bereich „Profiler konfigurieren“:

![Bereichseintrag „Profiler konfigurieren“][configure-profiler-entry]

Der Bereich **Profiler konfigurieren** enthält eine Schaltfläche **Jetzt Profil erstellen** zum Auslösen von Profiler für alle Instanzen der verknüpften Web-Apps. Außerdem können Sie nachvollziehen, wann Profiler in der Vergangenheit ausgeführt wurde.

![Profiler auf Anforderung][profiler-on-demand]

Sie sehen Benachrichtigungen und Statusänderungen im Hinblick auf den Profiler-Ausführungsstatus.

### <a name="step-3-view-traces"></a>Schritt 3: Anzeigen von Ablaufverfolgungen

Wenn die Profiler-Ausführung abgeschlossen ist, folgen Sie den Anweisungen zur Benachrichtigung, um zum Blatt „Leistung“ zu wechseln und Ablaufverfolgungen anzuzeigen.

### <a name="troubleshooting-on-demand-profiler"></a>Problembehandlung für den bedarfsgesteuerten Profiler

Im einigen Fällen werden nach einer bedarfsgesteuerten Sitzung möglicherweise Meldungen zu Profiler-Timeoutfehlern angezeigt:

![Profiler-Timeoutfehler][profiler-timeout]

Möglicherweise gibt es zwei Gründe dafür, dass der Fehler angezeigt wird:

1. Die bedarfsgesteuerte Profiler-Sitzung war erfolgreich, aber Application Insights hat mehr Zeit zum Verarbeiten der gesammelten Daten benötigt. Wenn die Datenverarbeitung nicht innerhalb von 15 Minuten beendet wurde, zeigt das Portal eine Timeoutmeldung an. Kurze Zeit später werden jedoch Profiler-Ablaufverfolgungen angezeigt. Wenn dies geschieht, ignorieren Sie die Fehlermeldung zunächst einmal. Wir arbeiten mit Hochdruck an der Lösung dieses Problems

2. Ihre Web-App verfügt über eine ältere Version des Profiler-Agent, der nicht über das On-Demand-Feature verfügt. Wenn Sie das Application Insights-Profil bereits aktiviert haben, ist es wahrscheinlich, dass Sie Ihren Profiler-Agent aktualisieren müssen, um das On-Demand-Feature zu verwenden.
  
So überprüfen und installieren Sie den neuesten Profiler:

1. Wechseln Sie zu den App-Einstellungen von App Services, und prüfen Sie, ob die folgenden Einstellungen festgelegt sind:
    * **APPINSIGHTS_INSTRUMENTATIONKEY**: Ersetzen Sie die Einstellung durch den richtigen Instrumentierungsschlüssel für Application Insights.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0 Wenn nicht alle Einstellungen festgelegt werden, wechseln Sie zum Application Insights-Aktivierungsbereich, um die neueste Websiteerweiterung zu installieren.

2. Wechseln Sie zum Application Insights-Bereich im App Services-Portal.

    ![Aktivieren von Application Insights im App Services-Portal][enable-app-insights]

3. Wenn auf der folgenden Seite eine Schaltfläche „Aktualisieren“ angezeigt wird, klicken Sie darauf, um die Application Insights-Websiteerweiterung zu aktualisieren. Anschließend wird der neueste Profiler-Agent installiert.
![Websiteerweiterung aktualisieren][update-site-extension]

4. Klicken Sie auf **Ändern**, um sicherzustellen, dass Profiler aktiviert ist, und anschließend auf **OK**, um die Änderungen zu speichern.

    ![Ändern und Speichern von App Insights][change-and-save-appinsights]

5. Kehren Sie zurück zur Registerkarte **App-Einstellungen** Registerkarte für den App Service, um zu überprüfen, ob die folgenden Elemente der App-Einstellungen festgelegt sind:
    * **APPINSIGHTS_INSTRUMENTATIONKEY**: Ersetzen Sie den Wert durch den ordnungsgemäßen Instrumentierungsschlüssel für Application Insights.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0

    ![App-Einstellungen für Profiler][app-settings-for-profiler]

6. Aktivieren Sie optional die Erweiterungsversion, und stellen Sie sicher, dass kein Update verfügbar ist.

    ![Auf Erweiterungsupdate prüfen][check-for-extension-update]

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von Azure Application Insights in Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[appinsights-in-appservices]:./media/app-insights-profiler/AppInsights-AppServices.png
[Enablement UI]: ./media/app-insights-profiler/Enablement_UI.png
[profiler-app-setting]:./media/app-insights-profiler/profiler-app-setting.png
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
[create-performance-test]: ./media/app-insights-profiler/new-performance-test.png
[configure-performance-test]: ./media/app-insights-profiler/configure-performance-test.png
[load-test-queued]: ./media/app-insights-profiler/load-test-queued.png
[load-test-in-progress]: ./media/app-insights-profiler/load-test-inprogress.png
[profiler-on-demand]: ./media/app-insights-profiler/Profiler-on-demand.png
[configure-profiler-entry]: ./media/app-insights-profiler/configure-profiler-entry.png
[enable-app-insights]: ./media/app-insights-profiler/enable-app-insights-blade-01.png
[update-site-extension]: ./media/app-insights-profiler/update-site-extension-01.png
[change-and-save-appinsights]: ./media/app-insights-profiler/change-and-save-appinsights-01.png
[app-settings-for-profiler]: ./media/app-insights-profiler/appsettings-for-profiler-01.png
[check-for-extension-update]: ./media/app-insights-profiler/check-extension-update-01.png
[profiler-timeout]: ./media/app-insights-profiler/profiler-timeout.png
