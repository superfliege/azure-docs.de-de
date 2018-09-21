---
title: Leistungsüberwachung für Java-Web-Apps in Azure Application Insights | Microsoft-Dokumentation
description: Erweiterte Leistungs- und Nutzungsüberwachung Ihrer Java-Website mit Application Insights.
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: 84017a48-1cb3-40c8-aab1-ff68d65e2128
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 08/24/2016
ms.author: mbullwin
ms.openlocfilehash: 366e79e7a58f45f5a5eeb318d3dd08427fbec0b0
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2018
ms.locfileid: "35634675"
---
# <a name="monitor-dependencies-caught-exceptions-and-method-execution-times-in-java-web-apps"></a>Überwachen von Abhängigkeiten, abgefangene Ausnahmen und Methodenausführungszeiten in Java-Web-Apps


Wenn Sie [Ihre Java-Web-App mit Application Insights instrumentiert haben][java], können Sie den Java-Agent ohne Codeänderungen verwenden, um detailliertere Informationen zu erhalten:

* **Abhängigkeiten** : Daten über Aufrufe der Anwendung an andere Komponenten, einschließlich:
  * **REST-Aufrufe** über „HttpClient“, „OkHttp“ und „RestTemplate“ (Spring) werden erfasst.
  * **Redis-Aufrufe** über den Jedis-Client werden erfasst.
  * **[JDBC-Aufrufe:](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** MySQL-, SQL Server- and Oracle DB-Befehle werden automatisch erfasst. Für MySQL gilt: Wenn der Aufruf länger als zehn Sekunden dauert, wird der Abfrageplan gemeldet.
* **Abgefangene Ausnahmen:** Informationen zu Ausnahmen, die von Ihrem Code behandelt werden.
* **Methodenausführungszeit:** Informationen zur Ausführungsdauer bestimmter Methoden.

Um den Java-Agent zu verwenden, installieren Sie ihn auf Ihrem Server. Ihre Web-Apps müssen mit dem [Application Insights Java SDK][java] instrumentiert werden. 

## <a name="install-the-application-insights-agent-for-java"></a>Installieren des Application Insights-Agents für Java
1. Laden Sie auf dem Computer, auf dem Ihr Java-Server ausgeführt wird, [den Agent herunter](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest). Die Version des heruntergeladenen Agents muss der Version der Core- und Webpakete des Application Insights Java SDKs entsprechen.
2. Bearbeiten Sie das Startskript des Anwendungsservers, und fügen Sie die folgende JVM hinzu:
   
    `javaagent:`*Vollständiger Pfad der JAR-Datei des Agents*
   
    Beispielsweise in Tomcat auf einem Linux-Computer:
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`
3. Starten Sie den Anwendungsserver neu.

## <a name="configure-the-agent"></a>Konfigurieren des Agents
Erstellen Sie eine Datei mit dem Namen `AI-Agent.xml` , und speichern Sie sie im selben Ordner wie die JAR-Datei des Agents.

Legen Sie den Inhalt der XML-Datei fest. Bearbeiten Sie das folgende Beispiel, um Features Ihrer Wahl aufzunehmen oder nicht.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>

        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>

           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne"
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>

      </Instrumentation>
    </ApplicationInsightsAgent>

```

Sie müssen für einzelne Methoden die Berichtausnahmen und Methodenzeiten aktivieren.

`reportExecutionTime` ist standardmäßig „true“, `reportCaughtExceptions` ist standardmäßig „false“.

## <a name="view-the-data"></a>Anzeigen der Daten
In der Application Insights-Ressource werden aggregierte Remoteabhängigkeiten und Methodenausführungszeiten [auf der Kachel „Leistung“][metrics] angezeigt.

Um nach den einzelnen Instanzen der Abhängigkeits-, Ausnahmen- und Methodenberichte zu suchen, öffnen Sie die [Suche][diagnostic].

[Diagnostizieren von Problemen mit Abhängigkeiten – weitere Informationen](app-insights-asp-net-dependencies.md#diagnosis).

## <a name="questions-problems"></a>Fragen? Probleme?
* Sie sehen keine Daten? [Festlegen von Firewallausnahmen](app-insights-ip-addresses.md)
* [Problembehandlung für Java](app-insights-java-troubleshoot.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
