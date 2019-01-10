---
title: Java-Web-App-Analyse mit Azure Application Insights | Microsoft-Dokumentation
description: 'Überwachung der Anwendungsleistung für Java-Web-Apps mithilfe von Application Insights. '
services: application-insights
documentationcenter: java
author: lgayhardt
manager: carmonm
ms.assetid: 051d4285-f38a-45d8-ad8a-45c3be828d91
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: lagayhar
ms.openlocfilehash: b662f5c4baa9c4c6e7689f3f463761dbd456f33f
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54074174"
---
# <a name="get-started-with-application-insights-in-a-java-web-project"></a>Erste Schritte mit Application Insights in einem Java-Webprojekt


[Application Insights](https://azure.microsoft.com/services/application-insights/) ist ein erweiterbarer Analysedienst für Webentwickler, der Ihnen dabei hilft, die Leistung und die Verwendung der Liveanwendung zu verstehen. Verwenden Sie den Dienst, um [Leistungsprobleme und Ausnahmen zu erkennen und zu diagnostizieren](../../azure-monitor/app/detect-triage-diagnose.md), und [schreiben Sie Code][api], um zu verfolgen, welche Aktionen Benutzer mit Ihrer App ausführen.

![Screenshot der Übersicht über die Beispieldaten](./media/java-get-started/overview-graphs.png)

Application Insights unterstützt Java-Apps, die unter Linux, Unix oder Windows ausgeführt werden.

Erforderlich:

* JRE-Version 1.7 oder 1.8
* Ein Abonnement für [Microsoft Azure](https://azure.microsoft.com/).

*Wenn Sie eine Web-App haben, die bereits aktiv ist, könnten Sie das alternative Verfahren nutzen, [um das SDK zur Laufzeit auf dem Webserver hinzuzufügen](java-live.md). Diese Alternative vermeidet das Neuerstellen des Codes, jedoch erhalten Sie nicht die Möglichkeit, Code schreiben, um die Benutzeraktivität nachzuverfolgen.*

Wenn Sie das Spring-Framework bevorzugen, lesen Sie den Leitfaden zum [Konfigurieren einer Spring Boot Initializer-App für die Verwendung von Application Insights](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-java-applicationinsights).

## <a name="1-get-an-application-insights-instrumentation-key"></a>1. Abrufen eines Application Insights-Instrumentationsschlüssels
1. Melden Sie sich beim [Microsoft Azure-Portal](https://portal.azure.com)an.
2. Erstellen Sie eine Application Insights-Ressource. Legen Sie den Anwendungstyp auf "Java-Webanwendung" fest.

    ![Geben Sie einen Namen ein, wählen Sie "Java-Web-App", und klicken Sie auf "Erstellen"](./media/java-get-started/02-create.png)
3. Suchen Sie den Instrumentationsschlüssel der neuen Ressource. Dieser Schlüssel muss in Kürze in das Codeprojekt eingefügt werden.

    ![Klicken Sie in der Übersicht über neue Ressourcen auf "Eigenschaften", und kopieren Sie den Instrumentationsschlüssel](./media/java-get-started/03-key.png)

## <a name="2-add-the-application-insights-sdk-for-java-to-your-project"></a>2. Hinzufügen des Application Insights SDK für Java zu Ihrem Projekt
*Wählen Sie die geeignete Methode für Ihr Projekt.*

#### <a name="if-youre-using-maven-a-namemaven-setup-"></a>Wenn Sie Maven verwenden: <a name="maven-setup" />
Wenn Ihr Projekt bereits für die Verwendung von Maven für den Buildprozess eingerichtet ist, fügen Sie Ihrer Datei „pom.xml“ den folgenden Code hinzu:

Aktualisieren Sie dann die Projektabhängigkeiten, damit die Binärdateien heruntergeladen werden.

```XML

    <repositories>
       <repository>
          <id>central</id>
          <name>Central</name>
          <url>http://repo1.maven.org/maven2</url>
       </repository>
    </repositories>

    <dependencies>
      <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-web</artifactId>
        <!-- or applicationinsights-core for bare API -->
        <version>[2.0,)</version>
      </dependency>
    </dependencies>
```

* *Build- oder Prüfsummenvalidierungsfehler?* Versuchen Sie es mit einer bestimmten Version, z. B.: `<version>2.0.n</version>`. Die neueste Version finden Sie in den [SDK-Versionshinweisen](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) oder in den [Maven-Artefakten](https://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).
* *Sie müssen auf ein neues SDK aktualisieren?*  Aktualisieren Sie die Abhängigkeiten des Projekts.

#### <a name="if-youre-using-gradle-a-namegradle-setup-"></a>Wenn Sie Gradle verwenden: <a name="gradle-setup" />
Wenn Ihr Projekt bereits für die Verwendung von Gradle für den Buildprozess eingerichtet ist, fügen Sie Ihrer Datei „build.gradle“ den folgenden Code hinzu:

Aktualisieren Sie dann die Projektabhängigkeiten, damit die Binärdateien heruntergeladen werden.

```gradle

    repositories {
      mavenCentral()
    }

    dependencies {
      compile group: 'com.microsoft.azure', name: 'applicationinsights-web', version: '2.+'
      // or applicationinsights-core for bare API
    }
```

#### <a name="if-youre-using-eclipse-to-create-a-dynamic-web-project-"></a>Wenn Sie Eclipse zum Erstellen eines dynamischen Webprojekts verwenden:
Verwenden Sie das [Application Insights SDK für Java-Plug-In][eclipse]. Hinweis: Dieses Plug-In unterstützt Sie zwar bei der Einrichtung von Application Insights (sofern Sie nicht Maven/Gradle verwenden), es ist jedoch kein Abhängigkeitsverwaltungssystem. Durch die Aktualisierung des Plug-Ins werden also nicht automatisch auch die Application Insights-Bibliotheken in Ihrem Projekt aktualisiert.

* *Build- oder Prüfsummenvalidierungsfehler?* Versuchen Sie es mit einer bestimmten Version, z. B.: `version:'2.0.n'`. Die neueste Version finden Sie in den [SDK-Versionshinweisen](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) oder in den [Maven-Artefakten](https://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).
* Aktualisieren Sie die Abhängigkeiten Ihres Projekts, um ein *Update auf ein neues SDK* durchzuführen.

#### <a name="otherwise-if-you-are-manually-managing-dependencies-"></a>Wenn Sie Abhängigkeiten manuell verwalten:
Laden Sie die [neueste Version](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest) herunter, kopieren Sie die erforderlichen Dateien in Ihr Projekt, und ersetzen Sie dabei alle früheren Versionen.

### <a name="questions"></a>Fragen...
* *Welche Beziehung besteht zwischen den Komponenten `-core` und `-web`?*
  * `applicationinsights-core` erhalten Sie die bloße API. Diese Komponente ist immer erforderlich.
  * Mit `applicationinsights-web` erhalten Sie Metriken zum Verfolgen der Anzahl der HTTP-Anforderungen und der Antwortzeiten. Diese Komponente können Sie weglassen, wenn die entsprechenden Telemetriedaten nicht automatisch gesammelt werden sollen – beispielsweise, wenn Sie den Code selbst schreiben möchten.
  
* *Wie aktualisiere ich das SDK auf die neueste Version?*
  * Wenn Sie Gradle oder Maven verwenden:
    * Aktualisieren Sie die Builddatei mit der neuesten Version, oder verwenden Sie die Platzhaltersyntax von Gradle/Maven zur automatischen Angabe der neuesten Version. Aktualisieren Sie anschließend die Abhängigkeiten Ihres Projekts. Die Platzhaltersyntax können Sie den obigen Beispielen für [Gradle](#gradle-setup) und [Maven](#maven-setup) entnehmen.
  * Wenn Sie Abhängigkeiten manuell verwalten:
    * Laden Sie das aktuelle [Application Insights-SDK für Java](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest) herunter, und ersetzen Sie die alte Version. Änderungen werden in den [SDK-Versionshinweisen](https://github.com/Microsoft/ApplicationInsights-Java#release-notes)beschrieben.

## <a name="3-add-an-applicationinsightsxml-file"></a>3. Hinzufügen einer Datei vom Typ „ApplicationInsights.xml“
Fügen Sie dem Ressourcenordner in Ihrem Projekt die Datei „ApplicationInsights.xml“ hinzu, oder stellen Sie sicher, dass sie dem Bereitstellungsklassenpfad Ihres Projekts hinzugefügt wird. Kopieren Sie den folgenden XML-Code in die Datei.

Fügen Sie den Instrumentationsschlüssel ein, den Sie aus dem Azure-Portal abgerufen haben.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- The key from the portal: -->
      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->
      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data to each event -->

      <TelemetryInitializers>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>
```

Die Konfigurationsdatei kann sich optional an einem beliebigen Speicherort befinden, auf den Ihre Anwendung Zugriff hat.  Die Systemeigenschaft `-Dapplicationinsights.configurationDirectory` gibt das Verzeichnis an, in dem sich die Datei „ApplicationInsights.xml“ befindet. Eine Konfigurationsdatei unter `E:\myconfigs\appinsights\ApplicationInsights.xml` wird beispielsweise mit der Eigenschaft `-Dapplicationinsights.configurationDirectory="E:\myconfigs\appinsights"` konfiguriert.

* Der Instrumentationsschlüssel wird zusammen mit jedem Telemetrieelement übermittelt und weist Application Insights an, ihn in Ihrer Ressource anzuzeigen.
* Die Komponente "HTTP-Anforderung" ist optional. Sie sendet automatisch Telemetriedaten zu Anforderungen und Antwortzeiten zum Portal.
* Die Ereigniskorrelation ist eine Ergänzung der HTTP-Anforderungskomponente. Sie weist den einzelnen Anforderungen, die vom Server empfangen wurden, einen Bezeichner zu und fügt ihn als Operation.Id-Eigenschaft jedem Telemetrieelement hinzu. Diese Eigenschaft ermöglicht das Korrelieren der jeder Anforderung zugeordneten Telemetriedaten, indem in [Diagnosesuche][diagnostic] ein Filter festgelegt wird.

### <a name="alternative-ways-to-set-the-instrumentation-key"></a>Alternative Methoden zum Festlegen des Instrumentationsschlüssels
Das Application Insights SDK sucht in dieser Reihenfolge nach dem Schlüssel:

1. Systemeigenschaft: -DAPPLICATION_INSIGHTS_IKEY=your_ikey
2. Umgebungsvariable: APPLICATION_INSIGHTS_IKEY
3. Konfigurationsdatei: ApplicationInsights.xml

Sie können dies auch [per Code festlegen](../../azure-monitor/app/api-custom-events-metrics.md#ikey):

```Java
    TelemetryConfiguration.getActive().setInstrumentationKey(iKey);
```

## <a name="4-add-an-http-filter"></a>4. Hinzufügen eines HTTP-Filters
Der letzte Konfigurationsschritt ermöglicht der HTTP-Anforderungskomponente das Protokollieren jeder Webanforderung. (Nicht erforderlich, wenn nur die bloße API wünschen.)

### <a name="spring-boot-applications"></a>Spring Boot-Anwendungen
Registrieren Sie `WebRequestTrackingFilter` für Application Insights in Ihrer Configuration-Klasse:

```Java
package <yourpackagename>.configurations;

import javax.servlet.Filter;

import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.core.Ordered;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Configuration;
import com.microsoft.applicationinsights.TelemetryConfiguration;
import com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter;

@Configuration
public class AppInsightsConfig {

    @Bean
    public String telemetryConfig() {
        String telemetryKey = System.getenv("<instrumentation key>");
        if (telemetryKey != null) {
            TelemetryConfiguration.getActive().setInstrumentationKey(telemetryKey);
        }
        return telemetryKey;
    }

    /**
     * Programmatically registers a FilterRegistrationBean to register WebRequestTrackingFilter
     * @param webRequestTrackingFilter
     * @return Bean of type {@link FilterRegistrationBean}
     */
    @Bean
    public FilterRegistrationBean webRequestTrackingFilterRegistrationBean(WebRequestTrackingFilter webRequestTrackingFilter) {
        FilterRegistrationBean registration = new FilterRegistrationBean();
        registration.setFilter(webRequestTrackingFilter);
        registration.addUrlPatterns("/*");
        registration.setOrder(Ordered.HIGHEST_PRECEDENCE + 10);
        return registration;
    }


    /**
     * Creates bean of type WebRequestTrackingFilter for request tracking
     * @param applicationName Name of the application to bind filter to
     * @return {@link Bean} of type {@link WebRequestTrackingFilter}
     */
    @Bean
    @ConditionalOnMissingBean

    public WebRequestTrackingFilter webRequestTrackingFilter(@Value("${spring.application.name:application}") String applicationName) {
        return new WebRequestTrackingFilter(applicationName);
    }


}
```

> [!NOTE]
> Ersetzen Sie bei Verwendung von Spring Boot 1.3.8 oder einer älteren Version die FilterRegistrationBean durch die folgende Zeile.

```Java
    import org.springframework.boot.context.embedded.FilterRegistrationBean;
```

Diese Klasse konfiguriert `WebRequestTrackingFilter` als ersten Filter der HTTP-Filterkette. Darüber hinaus ruft sie den Instrumentierungsschlüssel aus der Betriebssystem-Umgebungsvariablen ab (sofern verfügbar).

> Wir verwenden nicht die Spring-MVC-Konfiguration, sondern die Web-HTTP-Filterkonfiguration, da es sich hierbei um eine Spring Boot-Anwendung mit eigener Spring-MVC-Konfiguration handelt. Informationen zur Spring-MVC-spezifischen Konfiguration finden Sie in den Abschnitten weiter unten.

### <a name="applications-using-webxml"></a>Anwendungen mit „web.xml“
Suchen und öffnen Sie die Datei „web.xml“ in Ihrem Projekt, und führen Sie den folgenden Code unter dem Web-App-Knoten zusammen, in dem Ihre Anwendungsfilter konfiguriert sind.

Um möglichst genaue Ergebnisse zu erhalten, muss der Filter vor allen anderen Filtern zugeordnet werden.

```XML

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>

   <!-- This listener handles shutting down the TelemetryClient when an application/servlet is undeployed. -->
    <listener>
      <listener-class>com.microsoft.applicationinsights.web.internal.ApplicationInsightsServletContextListener</listener-class>
    </listener>
```

#### <a name="if-youre-using-spring-web-mvc-31-or-later"></a>Vorgehensweise bei Verwendung von Spring Web MVC 3.1 oder einer höheren Version
Bearbeiten Sie diese Elemente in „*-servlet.xml“ so, dass das Application Insights-Paket einbezogen wird:

```XML

    <context:component-scan base-package=" com.springapp.mvc, com.microsoft.applicationinsights.web.spring"/>

    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.microsoft.applicationinsights.web.spring.RequestNameHandlerInterceptorAdapter" />
        </mvc:interceptor>
    </mvc:interceptors>
```

#### <a name="if-youre-using-struts-2"></a>Wenn Sie Struts 2 verwenden...
Fügen Sie dieses Element der Struts-Konfigurationsdatei (die in der Regel "struts.xml" oder "struts-default.xml" heißt) hinzu:

```XML

     <interceptors>
       <interceptor name="ApplicationInsightsRequestNameInterceptor" class="com.microsoft.applicationinsights.web.struts.RequestNameInterceptor" />
     </interceptors>
     <default-interceptor-ref name="ApplicationInsightsRequestNameInterceptor" />
```

Wenn Sie in einem Standardstapel Interceptors definiert haben, kann der Interceptor diesem Stapel hinzugefügt werden.

## <a name="5-run-your-application"></a>5. Ausführen der Anwendung
Führen Sie sie entweder im Debugmodus auf dem Entwicklungscomputer aus, oder veröffentlichen Sie sie auf Ihrem Server.

## <a name="6-view-your-telemetry-in-application-insights"></a>6. Anzeigen Ihrer Telemetriedaten in Application Insights
Kehren Sie zur Application Insights-Ressource im [Microsoft Azure-Portal](https://portal.azure.com)zurück.

HTTP-Anforderungsdaten werden auf dem Übersichtsblatt angezeigt. (Wenn sie nicht vorhanden sind, warten Sie einige Sekunden, und klicken Sie dann auf "Aktualisieren".)

![Beispieldaten](./media/java-get-started/5-results.png)

[Weitere Informationen zu Metriken.][metrics]

Klicken Sie sich durch ein beliebiges Diagramm, um ausführliche aggregierte Metriken anzuzeigen.

![](./media/java-get-started/6-barchart.png)

> Application Insights geht bei HTTP-Anforderungen für MVC-Anwendungen von folgendem Format aus: `VERB controller/action`. `GET Home/Product/f9anuh81`, `GET Home/Product/2dffwrf5` und `GET Home/Product/sdf96vws` werden beispielsweise zu `GET Home/Product` gruppiert. Diese Gruppierung ermöglicht aussagekräftige Aggregationen von Anforderungen (etwa die Anzahl und die durchschnittliche Ausführungszeit von Anforderungen).
>
>

### <a name="instance-data"></a>Instanzdaten
Klicken Sie sich durch einen bestimmten Anforderungstyp, um einzelne Instanzen anzuzeigen.

In Application Insights werden zwei Arten von Daten angezeigt: aggregierte Daten, die als Mittelwerte, Anzahl und Summen gespeichert und angezeigt werden, sowie Instanzdaten, bei denen es sich um einzelne Berichte von HTTP-Anforderungen, Ausnahmen, Seitenansichten oder benutzerdefinierten Ereignissen handelt.

Beim Anzeigen der Eigenschaften einer Anforderung können Sie die damit verbundenen Telemetrieereignisse erkennen, wie z.B. Anforderungen und Ausnahmen.

![](./media/java-get-started/7-instance.png)

### <a name="analytics-powerful-query-language"></a>Analytics: Leistungsfähige Abfragesprache
Wenn sich mehr Daten ansammeln, können Sie Abfragen sowohl zum Aggregieren von Daten als auch zum Ermitteln einzelner Instanzen ausführen.  [Analytics](../../azure-monitor/app/analytics.md) ist ein leistungsfähiges Tool zum Nachvollziehen der Leistung und Nutzung sowie für Diagnosezwecke.

![Analytics-Beispiel](./media/java-get-started/025.png)

## <a name="7-install-your-app-on-the-server"></a>7. Installieren der App auf dem Server
Jetzt veröffentlichen Sie Ihre App auf dem Server, erlauben deren Benutzung und sehen sich an, wie die Telemetrie im Portal angezeigt wird.

* Stellen Sie sicher, dass die Firewall der Anwendung das Senden von Telemetrie an die folgenden Ports erlaubt:

  * dc.services.visualstudio.com:443
  * f5.services.visualstudio.com:443

* Wenn ausgehender Datenverkehr über eine Firewall geleitet werden muss, legen Sie Systemeigenschaften wie `http.proxyHost` und `http.proxyPort` fest.

* Installieren Sie auf Windows-Servern:

  * [Microsoft Visual C++ Redistributable](https://www.microsoft.com/download/details.aspx?id=40784)

    (Diese Komponente aktiviert Leistungsindikatoren.)


## <a name="exceptions-and-request-failures"></a>Ausnahmen und Anforderungsfehler
Nicht behandelte Ausnahmen werden automatisch gesammelt:

![Öffnen Sie „Einstellungen“ > „Fehler“.](./media/java-get-started/21-exceptions.png)

Um Daten zu anderen Ausnahmen zu erfassen, haben Sie zwei Möglichkeiten:

* [Fügen Sie trackException()-Aufrufe in den Code ein.][apiexceptions]
* [Installieren Sie den Java-Agent auf dem Server](java-agent.md). Sie geben die Methoden an, die Sie überwachen möchten.

## <a name="monitor-method-calls-and-external-dependencies"></a>Überwachen von Methodenaufrufen und externen Abhängigkeiten
[Installieren Sie den Java-Agent](java-agent.md) , um die angegebenen internen Methoden und Aufrufe über JDBC mit Zeitdaten zu protokollieren.

## <a name="performance-counters"></a>Leistungsindikatoren
Unter **Einstellungen** >**Server** finden Sie eine Reihe von Leistungsindikatoren.

![](./media/java-get-started/11-perf-counters.png)

### <a name="customize-performance-counter-collection"></a>Anpassen der Erfassung von Leistungsindikatoren
Um die Erfassung der Standardgruppe von Leistungsindikatoren zu deaktivieren, fügen Sie unter dem Stammknoten der Datei „ApplicationInsights.xml“ den folgenden Code hinzu:

```XML
    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>
```

### <a name="collect-additional-performance-counters"></a>Erfassen weiterer Leistungsindikatoren
Sie können weitere Leistungsindikatoren angeben, die erfasst werden sollen.

#### <a name="jmx-counters-exposed-by-the-java-virtual-machine"></a>JMX-Leistungsindikatoren (von der Java Virtual Machine bereitgestellt)

```XML
    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>
```

* `displayName` – Der im Application Insights-Portal angezeigte Name.
* `objectName` – Der JMX-Objektname.
* `attribute` – Das Attribut des abzurufenden JMX-Objektnamens
* `type` (optional) – Der Typ des Attributs des JMX-Objekts:
  * Standard: ein einfacher Typ wie "int" oder "long".
  * `composite`: Die Leistungsindikatordaten haben das Format 'Attribut.Daten'.
  * `tabular`: Die Leistungsindikatordaten haben das Format einer Tabellenzeile.

#### <a name="windows-performance-counters"></a>Windows-Leistungsindikatoren
Jeder [Windows-Leistungsindikator](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) gehört zu einer Kategorie (genauso wie ein Feld zu einer Klasse gehört). Kategorien können entweder global sein oder nummerierte oder benannte Instanzen haben.

```XML
    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>
```

* displayName – Der im Application Insights-Portal angezeigte Name
* categoryName – Die Leistungsindikatorkategorie (Leistungsobjekt), der dieser Leistungsindikator zugeordnet ist
* counterName – Der Name des Leistungsindikators
* instanceName – Der Name der Instanz der Leistungsindikatorkategorie oder eine leere Zeichenfolge (""), wenn die Kategorie eine einzelne Instanz enthält. Wenn "categoryName" auf "Process" festgelegt ist und der Leistungsindikator, den Sie erfassen möchten, aus dem aktuellen JVM-Prozess stammt, in dem Ihre Anwendung ausgeführt wird, geben Sie `"__SELF__"`an.

Ihre Leistungsindikatoren werden im [Metrik-Explorer][metrics] als benutzerdefinierte Metriken angezeigt.

![](./media/java-get-started/12-custom-perfs.png)

### <a name="unix-performance-counters"></a>Unix-Leistungsindikatoren
* [Installieren Sie collectd mit dem Application Insights-Plug-In](java-collectd.md) , um eine Vielzahl von System- und Netzwerkdaten abzurufen.

## <a name="local-forwarder"></a>Lokale Weiterleitung

[Die lokale Weiterleitung](https://docs.microsoft.com/azure/application-insights/local-forwarder) ist ein Agent, der Application Insights- oder [OpenCensus](https://opencensus.io/)-Telemetriedaten aus einer Vielzahl von SDKs und Frameworks erfasst und an Application Insights weiterleitet. Sie kann unter Windows und Linux ausgeführt werden.

```xml
<Channel type="com.microsoft.applicationinsights.channel.concrete.localforwarder.LocalForwarderTelemetryChannel">
<DeveloperMode>false</DeveloperMode>
<EndpointAddress><!-- put the hostname:port of your LocalForwarder instance here --></EndpointAddress>
<!-- The properties below are optional. The values shown are the defaults for each property -->
<FlushIntervalInSeconds>5</FlushIntervalInSeconds><!-- must be between [1, 500]. values outside the bound will be rounded to nearest bound -->
<MaxTelemetryBufferCapacity>500</MaxTelemetryBufferCapacity><!-- units=number of telemetry items; must be between [1, 1000] -->
</Channel>
```

Wenn Sie das SpringBoot-Startprogramm verwenden, fügen Sie Folgendes in Ihre Konfigurationsdatei (application.properies) ein:

```yml
azure.application-insights.channel.local-forwarder.endpoint-address=<!--put the hostname:port of your LocalForwarder instance here-->
azure.application-insights.channel.local-forwarder.flush-interval-in-seconds=<!--optional-->
azure.application-insights.channel.local-forwarder.max-telemetry-buffer-capacity=<!--optional-->
```

Die Standardwerte sind für die Konfiguration von SpringBoot „application.properties“ und „applicationinsights.xml“ identisch.

## <a name="get-user-and-session-data"></a>Abrufen von Benutzer- und Sitzungsdaten
Sie senden also Telemetriedaten vom Webserver. Um jetzt eine Rundum-Ansicht Ihrer Anwendung zu erhalten, können Sie weitere Überwachungsfunktionen hinzufügen:

* [Fügen Sie Ihren Webseiten Telemetrie hinzu][usage], um Seitenaufrufe und Benutzermetriken zu überwachen.
* [Richten Sie Webtests ein][availability], um sicherzustellen, dass die Anwendung online und reaktionsfähig bleibt.

## <a name="capture-log-traces"></a>Erfassen von Protokollablaufverfolgungen
Sie können Application Insights verwenden, um Protokolle aus Log4J, Logback oder anderen Frameworks zu segmentieren. Sie können die Protokolle mit HTTP-Anforderungen und anderer Telemetrie in Beziehung setzen. [Weitere Informationen][javalogs]

## <a name="send-your-own-telemetry"></a>Senden eigener Telemetriedaten
Nachdem Sie das SDK installiert haben, können Sie die API verwenden, um eigene Telemetriedaten senden.

* [Verfolgen Sie benutzerdefinierte Ereignisse und Metriken nach][api], um zu erfahren, welche Aktionen Benutzer ausführen.
* [Durchsuchen Sie Ereignisse und Protokolle][diagnostic], um Probleme besser zu diagnostizieren.

## <a name="availability-web-tests"></a>Verfügbarkeitswebtests
Application Insights kann Ihre Website in regelmäßigen Abständen testen, um zu überprüfen, ob sie betriebsbereit ist und gut reagiert. Klicken Sie zum [Einrichten][availability] auf „Webtests“.

![Klicken Sie auf „Webtests“ und anschließend auf „Webtest hinzufügen“.](./media/java-get-started/31-config-web-test.png)

Sie erhalten Diagramme von Reaktionszeiten sowie E-Mail-Benachrichtigungen, falls Ihre Website ausfällt.

![Beispiel für Webtest](./media/java-get-started/appinsights-10webtestresult.png)

[Weitere Informationen zu Verfügbarkeitswebtests][availability]

## <a name="questions-problems"></a>Fragen? Probleme?
[Problembehandlung für Java](java-troubleshoot.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Nächste Schritte
* [Überwachen von Abhängigkeitsaufrufen](java-agent.md)
* [Überwachen von Unix-Leistungsindikatoren](java-collectd.md)
* Richten Sie die [Überwachung für Ihre Webseiten ein](javascript.md), um Seitenladezeiten, AJAX-Aufrufe und Browserausnahmen zu überwachen.
* Schreiben Sie [benutzerdefinierte Telemetriedaten](../../azure-monitor/app/api-custom-events-metrics.md), um die Nutzung im Browser oder auf dem Server nachzuverfolgen.
* Erstellen Sie [Dashboards](../../azure-monitor/app/app-insights-dashboards.md), um die wichtigsten Diagramme zur Überwachung Ihres Systems zusammenzuführen.
* Verwenden Sie [Analytics](../../azure-monitor/app/analytics.md) für leistungsfähige Abfragen über Telemetriedaten in Ihrer App.
* Weitere Informationen finden Sie im Artikel [Azure für Java-Entwickler](/java/azure).

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiexceptions]: ../../azure-monitor/app/api-custom-events-metrics.md#trackexception
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[eclipse]: ../../azure-monitor/learn/java-quick-start.md
[javalogs]: java-trace-logs.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
[usage]: javascript.md
