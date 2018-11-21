---
title: Java Enterprise-Unterstützung für Azure App Service unter Linux | Microsoft-Dokumentation
description: Entwicklerleitfaden für die Bereitstellung von Java Enterprise-Apps unter Verwendung von WildFly mit Azure App Service unter Linux
keywords: Azure App Service, Web-App, Linux, OSS, Java, WildFly, Enterprise
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
ms.openlocfilehash: 6613def8891109e3a0ddf818111898a893a8035d
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628645"
---
# <a name="java-enterprise-guide-for-app-service-on-linux"></a>Java Enterprise-Leitfaden für App Service unter Linux

Mit Azure App Service unter Linux können Java-Entwickler Java Enterprise-Anwendungen (JEE) in einem vollständig verwalteten Linux-basierten Dienst erstellen, bereitstellen und skalieren.  Der Java Enterprise Runtime-Umgebung liegt der Open-Source-Anwendungsserver [WildFly](http://wildfly.org/) zugrunde.

Dieser Leitfaden enthält die wichtigsten Konzepte und Anweisungen für Java Enterprise-Entwickler, die App Service für Linux verwenden. Wenn Sie noch nie Java-Anwendungen mit Azure App Service für Linux bereitgestellt haben, sollten Sie zunächst den [Java-Schnellstart](quickstart-java.md) absolvieren. Antworten auf Fragen zu App Service für Linux, die sich nicht speziell auf Java Enterprise beziehen, finden Sie unter [Java-Entwicklerleitfaden](app-service-linux-java.md) und [Häufig gestellte Fragen (FAQ) zu Azure App Service unter Linux](app-service-linux-faq.md).

## <a name="scale-with-app-service"></a>Skalierung mit App Service 

Der in App Service unter Linux ausgeführte WildFly-Anwendungsserver wird im eigenständigen Modus und nicht in einer Domänenkonfiguration ausgeführt. 

 Sie können Ihre Anwendung mit [Skalierungsregeln](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-autoscale-get-started?toc=%2Fazure%2Fapp-service%2Fcontainers%2Ftoc.json) und durch [Erhöhen der Anzahl der Instanzen](https://docs.microsoft.com/azure/app-service/web-sites-scale?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) vertikal oder horizontal skalieren.

## <a name="customize-application-server-configuration"></a>Anpassen der Anwendungsserverkonfiguration

Entwickler können ein Bash-Startskript schreiben, um zusätzliche Konfigurationsschritte auszuführen, die für die Anwendung erforderlich sind, z.B.:

- Einrichten von Datenquellen
- Konfigurieren von Messaginganbietern
- Hinzufügen von anderen Modulen und Abhängigkeiten zur WildFly-Serverkonfiguration

 Das Skript wird ausgeführt, wenn WildFly betriebsbereit ist, jedoch vor dem Start der Anwendung. Im Skript sollte die von `/opt/jboss/wildfly/bin/jboss-cli.sh` aufgerufene [JBOSS CLI](https://docs.jboss.org/author/display/WFLY/Command+Line+Interface) verwendet werden, um den Anwendungsserver mit allen Konfigurationen oder Änderungen zu konfigurieren, die nach dem Start des Servers erforderlich sind. 

Konfigurieren Sie WildFly nicht mit dem interaktiven Modus der CLI. Stattdessen können Sie mit dem Befehl `--file` ein Skript von Befehlen für die JBoss CLI bereitstellen, z.B.:

```bash
/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli
```

Laden Sie das Startskript in `/home/site/deployments/tools` in Ihrer App Service-Instanz hoch. Anweisungen zum Abrufen der FTP-Anmeldeinformationen finden Sie in [diesem Dokument](/azure/app-service/app-service-deployment-credentials#userscope). 

Legen Sie das Feld **Startskript** im Azure-Portal auf den Speicherort Ihres Startshellskripts fest, z.B. `/home/site/deployments/tools/your-startup-script.sh`.

Verwenden Sie [Anwendungseinstellungen](/azure/app-service/web-sites-configure#application-settings), um Umgebungsvariablen zur Verwendung im Skript festzulegen. Diese Einstellungen werden in der Startskriptumgebung zur Verfügung gestellt und halten Verbindungszeichenfolgen und andere Geheimnisse aus der Versionskontrolle heraus.

## <a name="modules-and-dependencies"></a>Module und Abhängigkeiten

Um Module und die zugehörigen Abhängigkeiten über die JBoss CLI im WildFly-Klassenpfad zu installieren, müssen Sie die folgenden Dateien in Ihrem Verzeichnis erstellen.  Für einige Module und Abhängigkeiten sind möglicherweise zusätzliche Konfigurationsschritte erforderlich, z.B. JNDI-Benennung oder andere API-spezifische Konfigurationen. In der folgenden Liste sind daher die Komponenten aufgeführt, die Sie in den meisten Fällen zum Konfigurieren einer Abhängigkeit mindestens benötigen.

- Ein [XML-Moduldeskriptor](https://jboss-modules.github.io/jboss-modules/manual/#descriptors). Diese XML-Datei definiert den Namen, die Attribute und die Abhängigkeiten Ihres Moduls. Diese [module.xml-Beispieldatei](https://access.redhat.com/documentation/en-us/jboss_enterprise_application_platform/6/html/administration_and_configuration_guide/example_postgresql_xa_datasource) definiert ein Postgres-Modul, die zugehörige JAR-Datei für die JDBC-Abhängigkeit und andere erforderliche Modulabhängigkeiten.
- Alle erforderlichen JAR-Dateiabhängigkeiten für Ihr Modul.
- Ein Skript mit den JBoss CLI-Befehlen zum Konfigurieren des neuen Moduls. Diese Datei enthält die über die JBoss CLI auszuführenden Befehle, um den Server zur Verwendung der Abhängigkeit zu konfigurieren. Informationen zu den Befehlen zum Hinzufügen von Modulen, Datenquellen und Messaginganbietern finden Sie in [diesem Dokument](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.0/html-single/management_cli_guide/#how_to_cli).
-  Ein Bash-Startskript zum Aufrufen der JBoss CLI und zum Ausführen des Skripts im vorherigen Schritt. Diese Datei wird ausgeführt, wenn Ihre App Service-Instanz neu gestartet wird oder wenn neue Instanzen bei einer horizontalen Skalierung bereitgestellt werden.  In diesem Startskript können Sie andere Konfigurationen für Ihre Anwendung ausführen, während die JBoss-Befehle an die JBoss CLI übergeben werden. Diese Datei kann mindestens ein einzelner Befehl zum Übergeben des JBoss CLI-Befehlsskripts an die JBoss CLI sein: 
   
```bash
`/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli` 
``` 

Nachdem Sie die Dateien und Inhalte für Ihr Modul definiert haben, führen Sie die folgenden Schritte aus, um das Modul dem WildFly-Anwendungsserver hinzuzufügen. 

1. Übertragen Sie Ihre Dateien per FTP nach `/home/site/deployments/tools` in Ihrer App Service-Instanz. Anweisungen zum Abrufen der FTP-Anmeldeinformationen finden Sie in diesem Dokument. 
2. Legen Sie auf dem Blatt „Anwendungseinstellungen“ des Azure-Portals das Feld „Startskript“ auf den Speicherort Ihres Startshellskripts fest, z.B. `/home/site/deployments/tools/your-startup-script.sh`.
3. Starten Sie Ihre App Service-Instanz neu, indem Sie im Abschnitt **Übersicht** des Portals auf die Schaltfläche **Neu starten** klicken oder die Azure-Befehlszeilenschnittstelle verwenden.

## <a name="data-sources"></a>Datenquellen

Um WildFly für eine Datenquellenverbindung zu konfigurieren, führen Sie die gleichen Schritte aus, die oben im Abschnitt „Module und Abhängigkeiten“ beschrieben sind. Sie können die gleichen Schritte für alle Azure-Datenbankdienste ausführen.

1. Laden Sie den JDBC-Treiber für Ihren Datenbanktyp herunter. Der Einfachheit halber finden Sie hier die Treiber für [Postgres](https://jdbc.postgresql.org/download.html) und [MySQL](https://dev.mysql.com/downloads/connector/j/). Entpacken Sie den Download, um die JAR-Datei zu erhalten.
2. Führen Sie die unter „Module und Abhängigkeiten“ beschriebenen Schritte aus, um den XML-Moduldeskriptor, das JBoss CLI-Skript, das Startskript und die JDBC-JAR-Abhängigkeit zu erstellen und hochzuladen.


Weitere Informationen zum Konfigurieren von WildFly mit [PostgreSQL](https://developer.jboss.org/blogs/amartin-blog/2012/02/08/how-to-set-up-a-postgresql-jdbc-driver-on-jboss-7), [MySQL](https://dev.mysql.com/doc/connector-j/5.1/connector-j-usagenotes-jboss.html) und [SQL-Datenbank](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#d0e3898) stehen zur Verfügung. Anhand dieser angepassten Anweisungen zusammen mit den generalisierten Anweisungen oben können Sie auf dem Server Definitionen von Datenquellen hinzufügen.

## <a name="messaging-providers"></a>Messaginganbieter

So aktivieren Sie nachrichtengesteuerte Beans mit Service Bus als Messagingmechanismus

1. Verwenden Sie die [Apache QPId JMS-Messagingbibliothek](https://qpid.apache.org/proton/index.html). Fügen Sie diese Abhängigkeit in der Datei „pom.xml“ (oder einer anderen Builddatei) für die Anwendung ein.

2.  Erstellen Sie [Service Bus-Ressourcen](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp). Erstellen Sie einen Azure Service Bus-Namespace und eine Warteschlange in diesem Namespace und eine SAS-Richtlinie mit Funktionen zum Senden und Empfangen.

3. Übergeben Sie den Schlüssel für die SAS-Richtlinie an den Code, entweder durch URL-Codierung des Primärschlüssels der Richtlinie oder durch [Verwenden des Service Bus SDK](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp#setup-jndi-context-and-configure-the-connectionfactory).

4. Führen Sie die im Abschnitt „Module und Abhängigkeiten“ beschriebenen Schritte mit dem XML-Moduldeskriptor, den JAR-Abhängigkeiten, den JBoss CLI-Befehlen und dem Startskript für den JMS-Anbieter aus. Zusätzlich zu diesen vier Dateien müssen Sie auch eine XML-Datei erstellen, die den JNDI-Namen für die JMS-Warteschlange und das JMS-Thema definiert. Referenzkonfigurationsdateien finden Sie in [diesem Repository](https://github.com/JasonFreeberg/widlfly-server-configs/tree/master/appconfig).


## <a name="configure-session-management-caching"></a>Konfigurieren der Zwischenspeicherung der Sitzungsverwaltung

Standardmäßig werden in App Service unter Linux Sitzungsaffinitätscookies verwendet, um sicherzustellen, dass Clientanforderungen mit vorhandenen Sitzungen an die gleiche Instanz der Anwendung weitergeleitet werden. Dieses Standardverhalten erfordert keine Konfiguration, es gelten jedoch einige Einschränkungen:

- Wenn eine Anwendungsinstanz neu gestartet oder zentral herunterskaliert wird, geht der Zustand der Benutzersitzung im Anwendungsserver verloren.
- Wenn für Anwendungen Einstellungen für lange Sitzungstimeouts oder eine festgelegte Anzahl von Benutzern festgelegt sind, kann es einige Zeit dauern, bis automatisch skalierte neue Instanzen Lasten empfangen, da nur neue Sitzungen an die neu gestarteten Instanzen weitergeleitet werden.

Sie können WildFly zur Verwendung eines externen Sitzungsspeichers konfigurieren, z.B. [Redis Cache](/azure/redis-cache/). Sie müssen die Konfiguration [der vorhandenen ARR-Instanzaffinität deaktivieren](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/), um die cookiebasierte Weiterleitung der Sitzung zu deaktivieren und zu ermöglichen, dass der konfigurierte WildFly-Sitzungsspeicher ohne Beeinträchtigung ausgeführt wird.

## <a name="enable-web-sockets"></a>Aktivieren von WebSockets

WebSockets sind in App Service standardmäßig aktiviert. Informationen zu den ersten Schritten mit WebSockets in Ihrer Anwendung finden Sie in [diesem Schnellstart](https://github.com/wildfly/quickstart/tree/master/websocket-hello).

## <a name="logs-and-troubleshooting"></a>Protokolle und Problembehandlung

App Service umfasst Tools, mit denen Sie Probleme in Ihrer Anwendung beheben können.

-   Aktivieren Sie die Protokollierung, indem Sie im linken Navigationsbereich auf **Diagnoseprotokolle** klicken. Klicken Sie auf **Dateisystem**, um das Speicherkontingent und den Aufbewahrungszeitraum festzulegen und die Änderungen zu speichern. Diese Protokolle befinden sich unter `/home/LogFiles/`.
-   [Stellen Sie über SSH eine Verbindung mit der Anwendungsinstanz her](/app-service-linux-ssh-support), um die Protokolle für ausgeführte Anwendungen anzuzeigen.
-   Überprüfen Sie die Diagnoseprotokolle im Bereich **Diagnoseprotokolle** des Portals oder durch Ausführen des folgenden Azure CLI-Befehls: ` az webapp log tail --name <your-app-name> --resource-group <your-apps-resource-group> `
