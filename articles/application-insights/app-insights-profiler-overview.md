---
title: Profilerstellung für Produktionsanwendungen in Azure mit Application Insights Profiler | Microsoft-Dokumentation
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
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: bba15ee901e7ccecc513f526339bde2fcc6277fa
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142859"
---
# <a name="profile-production-applications-in-azure-with-application-insights"></a>Profilerstellung für Produktionsanwendungen in Azure mit Application Insights Profiler
## <a name="enable-profiler-for-your-application"></a>Aktivieren von Profiler für Ihre Anwendung

Application Insights Profiler bietet Leistungsablaufverfolgungen für Anwendungen, die in der Produktion in Azure ausgeführt werden. Die Daten werden automatisch im vollen Umfang erfasst, ohne Ihre Endbenutzer zu beeinträchtigen. Profiler unterstützt Sie beim Identifizieren des langsamsten Codepfads, der beim Verarbeiten einer bestimmten Webanforderung am längsten dauert. 

Profiler funktioniert mit .NET-Anwendungen, die für die folgenden Azure-Dienste bereitgestellt werden. Spezifische Anweisungen zum Aktivieren des Profilers für die einzelnen Diensttypen finden Sie unter folgenden Links.

* [App Services](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Cloud Services](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric-Anwendungen](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Virtual Machines und VM Scale Sets](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Wenn Sie Profiler aktiviert haben, aber keine Ablaufverfolgungen angezeigt werden, lesen Sie unseren [Leitfaden zur Problembehandlung](app-insights-profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).

Die lokale Ausführung von Profiler wird offiziell nicht unterstützt. Allerdings gibt es einige [Anweisungen, die Sie ausprobieren können](https://docs.microsoft.com/azure/application-insights/enable-profiler-compute#enable-profiler-on-on-premises-servers).

## <a name="view-profiler-data"></a>Anzeigen von Profiler-Daten

Damit Profiler Ablaufverfolgungen hochladen kann, muss Ihre Anwendung aktiv Anforderungen verarbeiten. Beim Ausführen eines Experiments können Sie mithilfe der [Application Insights-Leistungstests](https://docs.microsoft.com/vsts/load-test/app-service-web-app-performance-test) Anforderungen an die Web-App generieren. Wenn Sie Profiler gerade erst aktiviert haben, können Sie einen kurzen Auslastungstest ausführen. Während der Auslastungstest ausgeführt wird, klicken Sie auf der Seite mit den [**Profiler-Einstellungen**]() auf die Schaltfläche **Jetzt Profil erstellen**. Wenn Profiler ausgeführt wird, erfolgt die Profilerstellung nach dem Zufallsprinzip etwa einmal pro Stunde für eine Dauer von zwei Minuten. Wenn Ihre Anwendung einen stetigen Strom an Anforderungen verarbeitet, lädt Profiler jede Stunde Ablaufverfolgungen hoch.

Sobald die Anwendung Datenverkehr empfangen hat und Profiler Zeit hatte, die Ablaufverfolgungen hochzuladen, sollten Ihnen Ablaufverfolgungen angezeigt werden. Dieser Vorgang kann 5 bis 10 Minuten dauern. Zum Anzeigen von Ablaufverfolgungen wechseln Sie zum Bereich **Leistung**, wählen **Aktion ausführen** aus, um Profiler-Ablaufverfolgungen anzuzeigen, und wählen dann die Schaltfläche **Profiler-Ablaufverfolgungen** aus.

![Profiler-Ablaufverfolgungen im Application Insights-Bereich für die Leistungsvorschau][performance-blade]

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

## <a name="next-steps"></a>Nächste Schritte
Aktivieren von Application Insights Profiler für Ihre Azure-Anwendung
* [App Services](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Cloud Services](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric-Anwendungen](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Virtual Machines und VM Scale Sets](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)


[performance-blade]: ./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
