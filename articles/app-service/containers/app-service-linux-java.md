---
title: Java-Sprachunterstützung für Azure App Service unter Linux | Microsoft-Dokumentation
description: Entwicklerleitfaden für die Bereitstellung von Java-Apps mit Azure App Service unter Linux
keywords: Azure App Service, Web-App, Linux, OSS, Java
services: app-service
author: rloutlaw
manager: angerobe
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/29/2018
ms.author: routlaw
ms.openlocfilehash: 2b2256ef5802160dbaa66e2a098a798fcdc653d2
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47064506"
---
# <a name="java-developers-guide-for-app-service-on-linux"></a>Java-Entwicklerleitfaden für App Service unter Linux

Mit Azure App Service unter Linux können Java-Entwickler schnell ihre mit Tomcat oder Java Standard Edition (SE) paketierten Webanwendungen in einem vollständig verwalteten Linux-basierten Dienst erstellen, bereitstellen und skalieren. Stellen Sie Anwendungen mit Maven-Plug-Ins über die Befehlszeile oder in Editoren wie IntelliJ, Eclipse oder Visual Studio Code bereit.

Dieser Leitfaden enthält die wichtigsten Konzepte und Anweisungen für Java-Entwickler, die App Service für Linux verwenden. Wenn Sie Azure App Service für Linux noch nie verwendet haben, lesen Sie sich zuerst den [Java-Schnellstart](quickstart-java.md) durch. Allgemeine Fragen zur Verwendung von App Service für Linux, die sich nicht speziell auf die Java-Entwicklung beziehen, werden unter [Häufig gestellte Fragen (FAQ) zu Azure App Service unter Linux](app-service-linux-faq.md) beantwortet.

## <a name="logging-and-debugging-apps"></a>Protokollieren und Debuggen von Apps

Leistungsberichte, Datenverkehrsvisualisierungen und Integritätsprüfungen für die einzelnen Apps stehen über das Azure-Portal zur Verfügung. Weitere Informationen zum Zugriff auf diese Diagnosetools und zu deren Verwendung finden Sie unter [Übersicht über die Azure App Service-Diagnose](/azure/app-service/app-service-diagnostics).

### <a name="ssh-console-access"></a>SSH-Konsolenzugriff 

Für die Linux-Umgebung, unter der Ihre App ausgeführt wird, ist SSH-Konnektivität verfügbar. Vollständige Anweisungen zum Herstellen einer Verbindung mit dem Linux-System über Ihren Webbrowser oder ein lokales Terminal finden Sie unter [SSH-Unterstützung bei Azure App Service unter Linux](/azure/app-service/containers/app-service-linux-ssh-support).

### <a name="streaming-logs"></a>Streamingprotokolle

Für schnelles Debuggen und zur Problembehandlung können Sie Protokolle über die Azure CLI an die Konsole streamen. Konfigurieren Sie die CLI mit `az webapp log config`, um die Protokollierung zu aktivieren:

```azurecli-interactive
az webapp log config --name ${WEBAPP_NAME} \
 --resource-group ${RESOURCEGROUP_NAME} \
 --web-server-logging filesystem
```

Streamen Sie anschließend mit `az webapp log tail` Protokolle an Ihre Konsole:

```azurecli-interactive
az webapp log tail --name webappname --resource-group myResourceGroup
```

Weitere Informationen finden Sie unter [Streaming mit der Azure-Befehlszeilenschnittstelle](/azure/app-service/web-sites-enable-diagnostic-log#streaming-with-azure-command-line-interface).

### <a name="app-logging"></a>App-Protokollierung

Aktivieren Sie die [Anwendungsprotokollierung](/azure/app-service/web-sites-enable-diagnostic-log#enablediag) über das Azure-Portal oder die [Azure-Befehlszeilenschnittstelle](/cli/azure/webapp/log#az-webapp-log-config), um App Service so zu konfigurieren, dass die Streams mit der Standardkonsolenausgabe und den Standardkonsolenfehlern Ihrer Anwendung in das lokale Dateisystem oder Azure Blob Storage geschrieben werden. Die Protokollierung in der lokalen App Service-Dateisysteminstanz wird 12 Stunden nach der Konfiguration deaktiviert. Wenn Sie eine längere Beibehaltung benötigen, konfigurieren Sie die Anwendung für die Ausgabe in einen Blob Storage-Container.

Wenn Ihre Anwendung [Logback](https://logback.qos.ch/) oder [Log4j](https://logging.apache.org/log4j) für die Ablaufverfolgung verwendet, können Sie diese Ablaufverfolgungen mithilfe der Konfigurationsanweisungen für das Protokollierungsframework unter [Untersuchen von Java-Ablaufverfolgungsprotokollen in Application Insights](/azure/application-insights/app-insights-java-trace-logs) zur Überprüfung an Azure Application Insights weiterleiten. 

## <a name="customization-and-tuning"></a>Anpassung und Optimierung

Azure App Service für Linux unterstützt eine sofort durchführbare Optimierung und Anpassung über das Azure-Portal und die CLI. Lesen Sie die folgenden Artikel zur nicht Java-spezifischen Web-App-Konfiguration:

- [Konfigurieren von App Service-Einstellungen](/azure/app-service/web-sites-configure?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Einrichten einer benutzerdefinierten Domäne](/azure/app-service/app-service-web-tutorial-custom-domain?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Aktivieren von SSL](/azure/app-service/app-service-web-tutorial-custom-ssl?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Hinzufügen eines CDN](/azure/cdn/cdn-add-to-web-app?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)

### <a name="set-java-runtime-options"></a>Festlegen von Java-Runtimeoptionen

Um reservierten Arbeitsspeicher oder andere JVM-Runtimeoptionen in den Tomcat- und Java SE-Umgebungen einzustellen, legen Sie die JAVA_OPTS wie unten gezeigt als [Anwendungseinstellung](/azure/app-service/web-sites-configure#app-settings) fest. App Service für Linux übergibt diese Einstellung beim Start als Umgebungsvariable an die Java-Runtime.

Erstellen Sie im Azure-Portal unter **Anwendungseinstellungen** für die Web-App eine neue App-Einstellung namens `JAVA_OPTS`, welche die zusätzlichen Einstellungen enthält, z.B. `$JAVA_OPTS -Xms512m -Xmx1204m`.

Um die App-Einstellung über das Azure App Service-Linux-Maven-Plug-In zu konfigurieren, fügen Sie Einstellungs-/Werttags im Azure-Plug-In-Abschnitt hinzu. Im folgenden Beispiel werden bestimmte Mindest- und Höchstwerte für die Java-Heapgröße festgelegt:

```xml
<appSettings> 
    <property> 
        <name>JAVA_OPTS</name> 
        <value>$JAVA_OPTS -Xms512m -Xmx1204m</value> 
    </property> 
</appSettings> 
```

Entwickler, die eine einzige Anwendung mit einem Bereitstellungsslot in ihrem App Service-Plan ausführen, können die folgenden Optionen verwenden:

- B1- und S1-Instanzen: -Xms1024m -Xmx1024m
- B2- und S2-Instanzen: -Xms3072m -Xmx3072m
- B3- und S3-Instanzen: -Xms6144m -Xmx6144m


Überprüfen Sie beim Optimieren Ihrer Anwendungsheapeinstellungen Ihre App Service-Plandetails, und berücksichtigen Sie mehrere Anwendungs- und Bereitstellungsslotanforderungen, um die optimale Zuordnung von Arbeitsspeicher zu ermitteln.

### <a name="turn-on-web-sockets"></a>Aktivieren von Websockets

Aktivieren Sie die Unterstützung für Websockets im Azure-Portal in den **Anwendungseinstellungen** für die Anwendung. Sie müssen die Anwendung neu starten, damit die Einstellung in Kraft tritt.

Aktivieren Sie die Websocketunterstützung über die Azure CLI mithilfe des folgenden Befehls:

```azurecli-interactive
az webapp config set -n ${WEBAPP_NAME} -g ${WEBAPP_RESOURCEGROUP_NAME} --web-sockets-enabled true 
```

Starten Sie Ihre Anwendung anschließend neu:

```azurecli-interactive
az webapp stop -n ${WEBAPP_NAME} -g ${WEBAPP_RESOURCEGROUP_NAME} 
az webapp start -n ${WEBAPP_NAME} -g ${WEBAPP_RESOURCEGROUP_NAME}  
```

### <a name="set-default-character-encoding"></a>Festlegen der Standardzeichencodierung 

Erstellen Sie im Azure-Portal unter **Anwendungseinstellungen** für die Web-App eine neue App-Einstellung namens `JAVA_OPTS` mit dem Wert `$JAVA_OPTS -Dfile.encoding=UTF-8`.

Alternativ können Sie die App-Einstellung mit dem App Service-Maven-Plug-In konfigurieren. Fügen Sie die name- und value-Tags der Einstellung in der Plug-In-Konfiguration hinzu: 

```xml
<appSettings> 
    <property> 
        <name>JAVA_OPTS</name> 
        <value>$JAVA_OPTS -Dfile.encoding=UTF-8</value> 
    </property> 
</appSettings> 
```

## <a name="secure-application"></a>Sichere Anwendung

Für App Service für Linux ausgeführte Java-Anwendungen gelten dieselben [bewährten Sicherheitsmethoden](/azure/security/security-paas-applications-using-app-services) wie für andere Anwendungen. 

### <a name="authenticate-users"></a>Authentifizieren von Benutzern

Richten Sie die App-Authentifizierung im Azure-Portal über die Option **Authentifizierung und Autorisierung** ein. Von dort aus können Sie die Authentifizierung über Azure Active Directory oder soziale Netzwerke wie Facebook, Google oder GitHub aktivieren. Die Konfiguration des Azure-Portals funktioniert nur, wenn Sie einen einzelnen Authentifizierungsanbieter konfigurieren.  Weitere Informationen finden Sie unter [Konfigurieren Ihrer App Service-App zur Verwendung der Azure Active Directory-Anmeldung](/azure/app-service/app-service-mobile-how-to-configure-active-directory-authentication) und in den entsprechenden Artikeln für andere Identitätsanbieter.

Wenn Sie mehrere Anmeldungsanbieter aktivieren müssen, befolgen Sie die Anweisungen im Artikel [Anpassen der Authentifizierung und Autorisierung in Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-authentication-how-to).

 Spring Boot-Entwickler können die [Azure Active Directory-Startoption für Spring Boot](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory?view=azure-java-stable) verwenden, um Anwendungen mithilfe von vertrauten Spring Security-Anmerkungen und -APIs absichern.

### <a name="configure-tlsssl"></a>Konfigurieren von TLS/SSL

Befolgen Sie die Anweisungen unter [Tutorial: Binden eines vorhandenen benutzerdefinierten SSL-Zertifikats an Azure-Web-Apps](/azure/app-service/app-service-web-tutorial-custom-ssl), um ein vorhandenes SSL-Zertifikat hochzuladen und an den Domänennamen Ihrer Anwendung zu binden. Standardmäßig lässt Ihre Anwendung dennoch HTTP-Verbindungen zu. Befolgen Sie die spezifischen Schritte im Tutorial, um SSL und TLS zu erzwingen.

## <a name="tomcat"></a>Tomcat 

### <a name="connecting-to-data-sources"></a>Herstellen einer Verbindung mit Datenquellen

>[!NOTE]
> Wenn Ihre Anwendung das Spring Framework oder Spring Boot verwendet, können Sie Informationen zur Datenbankverbindung für die Spring Data-JPA als Umgebungsvariablen [in Ihrer Anwendungseigenschaftendatei] festlegen. Verwenden Sie dann [App-Einstellungen](/azure/app-service/web-sites-configure#app-settings), um diese Werte für Ihre Anwendung im Azure-Portal oder in der CLI zu definieren.

Um Tomcat für die Verwendung verwalteter Datenbankverbindungen über Java Database Connectivity (JDBC) oder die Java-Persistenz-API (JPA) zu konfigurieren, passen Sie zuerst die CATALINA_OPTS-Umgebungsvariable an, die von Tomcat beim Start eingelesen wird. Legen Sie diese Werte mithilfe einer App-Einstellung im App Service-Maven-Plug-In fest:

```xml
<appSettings> 
    <property> 
        <name>CATALINA_OPTS</name> 
        <value>"$CATALINA_OPTS -Dmysqluser=${mysqluser} -Dmysqlpass=${mysqlpass} -DmysqlURL=${mysqlURL}"</value> 
    </property> 
</appSettings> 
```

Verwenden Sie alternativ dazu eine entsprechende App Service-Einstellung im Azure-Portal.

Legen Sie als Nächstes fest, ob die Datenquelle nur für eine Anwendung oder für alle Anwendungen zur Verfügung gestellt werden soll, die im Rahmen des App Service-Plans ausgeführt werden.

Für Datenquellen auf Anwendungsebene: 

1. Fügen Sie eine Datei `context.xml` hinzu, wenn diese in Ihrer Web-Anwendung nicht vorhanden ist, und fügen Sie ihr bei Projekterstellung das Verzeichnis `META-INF` Ihrer WAR-Datei hinzu.

2. Fügen Sie in dieser Datei einen Pfadeintrag `Context` hinzu, um die Datenquelle mit einer JNDI-Adresse zu verknüpfen. Das 

    ```xml
    <Context>
        <Resource
            name="jdbc/mysqldb" type="javax.sql.DataSource"
            url="${mysqlURL}"
            driverClassName="com.mysql.jdbc.Driver"
            username="${mysqluser}" password="${mysqlpass}"
        />
    </Context>
    ```

3. Aktualisieren Sie die Datei `web.xml` Ihrer Anwendung, sodass die Datenquelle in Ihrer Anwendung verwendet wird.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/mysqldb</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

Für gemeinsam verwendete Ressourcen auf Serverebene:

1. Kopieren Sie den Inhalt von `/usr/local/tomcat/conf` über SSH in `/home/tomcat` in Ihrer Linux-Instanz von App Service, wenn dort noch keine Konfiguration vorliegt.

2. Fügen Sie den Kontext Ihrer Datei `server.xml` hinzu.

    ```xml
    <Context>
        <Resource
            name="jdbc/mysqldb" type="javax.sql.DataSource"
            url="${mysqlURL}"
            driverClassName="com.mysql.jdbc.Driver"
            username="${mysqluser}" password="${mysqlpass}"
        />
    </Context>
    ```

3. Aktualisieren Sie die Datei `web.xml` Ihrer Anwendung, sodass die Datenquelle in Ihrer Anwendung verwendet wird.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/mysqldb</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

4. Stellen Sie sicher, dass die JDBC-Treiberdateien für den Tomcat-Klassenladeprogramm verfügbar sind, indem Sie sie im Verzeichnis `/home/tomcat/lib` ablegen. Um diese Dateien in Ihre App Service-Instanz hochzuladen, gehen Sie folgendermaßen vor:  
    1. Installieren Sie die Azure App Service-webpp-Erweiterung:
      ```azurecli-interactive
      az extension add –name webapp
      ```
    2. Führen Sie den folgenden CLI-Befehl aus, um einen SSH-Tunnel von Ihrem lokalen System zu App Service zu erstellen:
      ```azurecli-interactive
      az webapp remote-connection create –g [resource group] -n [app name] -p [local port to open]
      ```
    3. Stellen Sie mit Ihrem SFTP-Client eine Verbindung mit dem lokalen Tunnelport her, und laden Sie die Dateien in `/home/tomcat/lib` hoch.

5. Starten Sie die App Service-Linux-Anwendung neu. Tomcat setzt `CATALINA_HOME` auf `/home/tomcat` zurück und verwendet die aktualisierte Konfiguration und die entsprechenden Klassen.

## <a name="docker-containers"></a>Docker-Container

Um das von Azure unterstützte Zulu JDK zu verwenden, das in App Service in Ihren Containern ausgeführt wird, stellen Sie sicher, dass die `Dockerfile` Ihrer Anwendung Images aus dem [Java-App Service-Docker-Image-Repository](https://github.com/Azure-App-Service/java) verwendet.

## <a name="runtime-availability-and-statement-of-support"></a>Runtimeverfügbarkeit und Unterstützungserklärung

App Service für Linux unterstützt zwei Runtimes zum verwalteten Hosten von Java-Webanwendungen:

- [Tomcat-Servlet-Container](http://tomcat.apache.org/) zum Ausführen von Anwendungen, die als Webarchivdateien (WAR) paketiert wurden. Unterstützt werden die Versionen 8.5 und 9.0.
- Java SE Runtime Environment zum Ausführen von Anwendungen, die als Java-Archivdateien (JAR) paketiert wurden. Aktuell wird als Hauptversion nur Java 8 unterstützt.

## <a name="java-runtime-statement-of-support"></a>Unterstützungserklärung für die Java-Runtime 

### <a name="jdk-versions-and-maintenance"></a>JDK-Versionen und -Wartung

Als Java Development Kit (JDK) wird das von [Azul Systems](https://www.azul.com/) bereitgestellte [Zulu](https://www.azul.com/products/zulu-and-zulu-enterprise/) von Azure unterstützt.

Größere Versionsupdates werden durch neue Runtimeoptionen in Azure App Service für Linux bereitgestellt. Kunden führen das Update auf diese neueren Versionen von Java durch die Konfiguration ihrer App Service-Bereitstellung durch und müssen durch Tests sicherstellen, dass das größere Update ihren Anforderungen entspricht.

Unterstützte JDKs werden jedes Vierteljahr im Januar, April, Juli und Oktober automatisch gepatcht.

### <a name="security-updates"></a>Sicherheitsupdates

Patches und Fixes für größere Sicherheitsrisiken werden veröffentlicht, sobald sie von Azul Systems zur Verfügung gestellt werden. Ein „größeres“ Sicherheitsrisiko wird durch eine Gesamtbewertung von 9.0 oder höher im [NIST Common Vulnerability Scoring System, Version 2](https://nvd.nist.gov/cvss.cfm), definiert.

### <a name="deprecation-and-retirement"></a>Einstellung und Außerbetriebnahme

Wenn eine unterstützte Java-Runtime eingestellt wird, erhalten Azure-Entwickler, die die betreffende Runtime verwenden, mindestens sechs Monate vor Außerbetriebnahme der Runtime eine entsprechende Benachrichtigung.

### <a name="local-development"></a>Lokale Entwicklung

Entwickler können die Production Edition des Azul Zulu Enterprise JDK zur lokalen Entwicklung von der [Azul-Downloadsite](https://www.azul.com/downloads/zulu/) herunterladen.

### <a name="development-support"></a>Entwicklungsunterstützung

Produktsupport für das Azul Zulu Enterprise JDK ist bei der Entwicklung für Azure oder [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) mit einem [qualifizierten Azure-Supportplan](https://azure.microsoft.com/support/plans/) erhältlich.

### <a name="runtime-support"></a>Laufzeitunterstützung

Entwickler können ein [Problem](/azure/azure-supportability/how-to-create-azure-support-request) mit der App Service-Linux-Java-Runtime über den Azure-Support melden, wenn sie einen [qualifizierten Supportplan](https://azure.microsoft.com/support/plans/) besitzen.

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie das Center [Azure für Java-Entwickler](/java/azure/), um Azure-Schnellstarts, Tutorials und Java-Referenzdokumentation zu finden.
