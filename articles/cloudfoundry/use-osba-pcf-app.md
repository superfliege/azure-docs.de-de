---
title: Verwenden einer Open Service Broker-Instanz für eine Azure-Datenbank mit einer Anwendung in Pivotal Cloud Foundry
author: zr-msft
manager: jeconnoc
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.author: zarhoads
ms.date: 01/11/2019
ms.topic: tutorial
description: Hier erfahren Sie, wie Sie eine Pivotal Cloud Foundry-Anwendung für die Verwendung einer Open Service Broker-Instanz für eine Azure-Datenbank konfigurieren.
keywords: Pivotal Cloud Foundry, Cloud Foundry, Open Service Broker, Open Service Broker für Azure
tags: Cloud-Foundry
ms.openlocfilehash: d553cd09ef42e47e3a10fb96039063b8aae665cb
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2019
ms.locfileid: "54258869"
---
# <a name="tutorial-use-an-open-service-broker-for-azure-database-with-an-application-on-pivotal-cloud-foundry"></a>Tutorial: Verwenden einer Open Service Broker-Instanz für eine Azure-Datenbank mit einer Anwendung in Pivotal Cloud Foundry

Die Verwendung von Open Service Broker für Azure mit einer Pivotal Cloud Foundry-Instanz ermöglicht es, Dienste (etwa Datenbanken) in Azure direkt über die Cloud Foundry-Befehlszeilenschnittstelle und Ihre Pivotal Cloud Foundry-Instanz bereitzustellen. Darüber hinaus können Sie diese Dienste an eine Anwendung binden, die in Ihrer Pivotal Cloud Foundry-Instanz ausgeführt wird. Wenn Sie eine Anwendung auf diese Weise an einen Dienst binden, müssen Sie weder Code noch die Konfiguration in Ihrer Anwendung aktualisieren. In diesem Artikel erfahren Sie, wie Sie Open Service Broker für Azure für eine Datenbank mit einer Anwendung in Pivotal Cloud Foundry verwenden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Vorbereiten Ihres Anwendungsbereichs in Pivotal Cloud Foundry
> * Klonen einer Beispielanwendungsquelle von GitHub
> * Vorbereiten der Anwendung für die Bereitstellung
> * Bereitstellen der Anwendung
> * Erstellen einer Datenbank unter Verwendung von Open Service Broker für Azure
> * Binden der Datenbank an Ihre Anwendung

## <a name="prerequisites"></a>Voraussetzungen

Für die weiteren Schritte wird Folgendes vorausgesetzt:

* [Sie haben Pivotal Cloud Foundry installiert und konfiguriert.](create-cloud-foundry-on-azure.md)
* [Sie haben Open Service Broker für Azure installiert und konfiguriert](https://network.pivotal.io/products/azure-open-service-broker-pcf) (mit Ihrer Cloud Foundry-Instanz).

Die folgende Beispielabbildung zeigt den Ops Manager-Bildschirm von Pivotal Cloud Foundry mit installierter und konfigurierter OSBA-Kachel (Open Service Broker für Azure):

![Pivotal Cloud Foundry mit installierter Instanz von Open Service Broker für Azure](media/pcf-ops-manager.png)

## <a name="prepare-your-application-space-in-pivotal-cloud-foundry"></a>Vorbereiten Ihres Anwendungsbereichs in Pivotal Cloud Foundry

Damit die Anwendung in Ihrer Pivotal Cloud Foundry-Instanz bereitgestellt werden kann, müssen Sie mit dem Befehlszeilentool `cf` angemeldet sein. Darüber hinaus müssen als Ziel Ihre gewünschte Organisation und Ihr gewünschter Bereich verwendet werden.

Verwenden Sie `cf target`, um sich zu vergewissern, dass Sie angemeldet sind, und um den Zielbereich anzuzeigen. Im folgenden Beispiel ist der Benutzer bereits als *admin* angemeldet und verwendet die Organisation *myorg* sowie den Zielbereich *dev*:

```cmd
$ cf target

api endpoint:   https://api.system.40.85.111.222.cf.pcfazure.com
api version:    2.120.0
user:           admin
org:            myorg
space:          dev
```

Verwenden Sie `cf login`, um sich anzumelden:

```cmd
cf login -a https://api.SYSTEMDOMAINURL --skip-ssl-validation
```

Verwenden Sie `cf create-org` und `cf create-space`, um eine neue Organisation und einen neuen Bereich zu erstellen:

```cmd 
cf create-org myorg
cf create-space dev -o myorg
```

Verwenden Sie `cf target`, um einen Bereich als Ziel festzulegen:

```cmd
cf target -o myorg -s dev
```

## <a name="get-application-code"></a>Abrufen von Anwendungscode

Damit eine Anwendung Open Service Broker für Azure nutzen kann, muss sie eine Datenquelle (etwa eine Datenbank) verwenden. In diesem Artikel verwenden wir die [Spring-Musikbeispielanwendung von Cloud Foundry](https://github.com/cloudfoundry-samples/spring-music) zur Veranschaulichung einer Anwendung mit Datenquelle.

Klonen Sie die Anwendung von GitHub, und navigieren Sie zum Verzeichnis der Anwendung:

```cmd
git clone https://github.com/cloudfoundry-samples/spring-music
cd spring music
```

> [!TIP]
> Öffnen Sie die Datei [src/main/resources/application.yml](https://github.com/cloudfoundry-samples/spring-music/blob/master/src/main/resources/application.yml), um die Datenquellen für diese Anwendung anzuzeigen.


## <a name="prepare-the-application-for-deployment"></a>Vorbereiten der Anwendung für die Bereitstellung

Bevor Sie die Anwendung in Ihrer Pivotal Cloud Foundry-Instanz bereitstellen können, müssen Sie sie erstellen. Zu Veranschaulichungszwecken aktivieren wir außerdem die *DEBUG*-Protokollierung, um die Verbindungsinformationen für die Datenquelle zu protokollieren.

Fügen Sie zum Aktivieren der *DEBUG*-Protokollierung von Datenbankverbindungsdetails am Ende der Datei *application.yml* die folgende YAML-Eigenschaft hinzu:

```yaml
---
logging:
  level:
    com.zaxxer.hikari: DEBUG
```

Die Beispielanwendung verwendet Gradle, um die Anwendung als in Spring Boot ausführbare JAR-Datei zu erstellen. Die ausführbare JAR-Datei wird in Ihrer Pivotal Cloud Foundry-Instanz bereitgestellt. So erstellen Sie die Anwendung:

```cmd
./gradlew clean assemble

Starting a Gradle Daemon (subsequent builds will be faster)

BUILD SUCCESSFUL in 10s
4 actionable tasks: 4 executed
```


## <a name="deploy-your-application"></a>Bereitstellen der Anwendung

Verwenden Sie den Befehl `cf push`, um die Anwendung in Ihrer Pivotal Cloud Foundry-Instanz bereitzustellen:

```cmd
cf push

Pushing from manifest to org myorg / space dev as admin...
Using manifest file /path/to/spring-music/manifest.yml
Getting app info...
Creating app with these attributes...
+ name:       spring-music
...
Waiting for app to start...

name:              spring-music
requested state:   started
routes:            spring-music-wacky-oribi.app.40.85.111.222.cf.pcfazure.com
...
     state     since                  cpu    memory         disk           details
#0   running   2018-12-06T21:24:06Z   0.0%   313.3M of 1G   170.7M of 1G
```

Kopieren Sie den Wert von *routes* (Routen) aus der Ausgabe von `cf push`. Bei der Route handelt es sich um die URL für den Zugriff auf die ausgeführte Anwendung. Beispiel: 

```cmd
...
routes:            spring-music-wacky-oribi.app.40.85.111.222.cf.pcfazure.com
...
```


Nachdem die Anwendung bereitgestellt wurde, finden Sie in den Protokollen der Anwendung die von der Anwendung verwendete Verbindungs-URL. Der folgende Befehl zeigt die Protokolle der Anwendung an und verwendet `grep`, um nach der Konfiguration *jdbcUrl* zu suchen.

```cmd
cf logs spring-music --recent | grep jdbcUrl

2018-12-07T14:44:30.57-0600 [APP/PROC/WEB/0] OUT 2018-12-07 20:44:30.574 DEBUG 24 --- [           main] com.zaxxer.hikari.HikariConfig           : jdbcUrl.........................jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
```

Beachten Sie, dass die Anwendung *h2:mem:testdb* verwendet. Hierbei handelt es sich um die In-Memory Database. Spring-Anwendungen werden automatisch für die Verwendung einer In-Memory Database konfiguriert, wenn der Klassenpfad eine entsprechende Abhängigkeit enthält und die [automatische Konfiguration](https://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-auto-configuration.html) aktiviert ist. Für die Beispielanwendung ist die [In-Memory Database-Abhängigkeit „h2“ konfiguriert](https://github.com/cloudfoundry-samples/spring-music/blob/master/build.gradle#L49) und die automatische Konfiguration in [src/main/java/org/cloudfoundry/samples/music/Application.java](https://github.com/cloudfoundry-samples/spring-music/blob/master/src/main/java/org/cloudfoundry/samples/music/Application.java#L8) aktiviert.

Verwenden Sie die Route der Anwendung, um in einem Browser zu ihr zu navigieren. Die Route (oder URL) wird in der Ausgabe des Befehls `cf push` angezeigt.

> [!TIP]
> Die URL der Anwendung kann aber auch durch Ausführen von `cf apps` angezeigt werden.

Interagieren Sie mit der Anwendung in Ihrem Browser, indem Sie einige vorhandene Alben löschen und einige neue Alben erstellen. Die Beispielanwendung verwendet die In-Memory Database, um Ihre Änderungen zu speichern. Wie Sie sehen, enthält die Anwendung bereits einige [Standarddaten](https://github.com/cloudfoundry-samples/spring-music/blob/master/src/main/resources/albums.json). 

![Spring-Musik-App mit Standarddaten](media/music-app.png)

Da Ihre Anwendung eine In-Memory Database verwendet, gehen Ihre Änderungen verloren, wenn Ihre Anwendung neu gestartet oder erneut bereitgestellt wird. Führen Sie zur Veranschaulichung dieses Verhaltens `cf restage` aus, nachdem Sie einige Änderungen vorgenommen haben, um Ihre Anwendung erneut bereitzustellen:

```cmd
cf restage spring-music
```

Navigieren Sie anschließend in einem Browser unter Verwendung der gleichen URL zu der erneut bereitgestellten Anwendung. Wie Sie sehen, wurden die Änderungen nicht gespeichert, und es werden wieder die Standarddaten angezeigt.

![Spring-Musik-App mit Standarddaten](media/music-app.png)

## <a name="create-a-database"></a>Erstellen einer Datenbank

Eine dauerhafte Datenbank in Azure mit Open Service Broker kann mithilfe des Befehls `cf create-service` erstellt werden. Der unten angegebene Befehl stellt eine PostgreSQL-Datenbank in Azure in der Ressourcengruppe *MyResourceGroup* in der Region *eastus* bereit. Weitere Informationen zu *resourceGroup*, *location* und anderen Azure-spezifischen JSON-Parametern finden Sie in der [Referenzdokumentation für das PostgreSQL-Modul](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/postgresql.md#provision):

```cmd
cf create-service azure-postgresql-9-6 general-purpose mypgsql -c '{"resourceGroup":"MyResourceGroup", "location":"eastus"}'
```

Die Datenbank wird in Ihrer Pivotal Cloud Foundry-Instanz als Dienst namens *mypgsql* verfügbar gemacht. Die Bereitstellung Ihrer Datenbank dauert voraussichtlich ein paar Minuten. Nach Abschluss des Vorgangs können Sie sie an Ihre Anwendung binden. Vergewissern Sie sich mithilfe des Befehls `cf services`, dass die Bereitstellung Ihrer Datenbank abgeschlossen ist:

```cmd
cf services

Getting services in org myorg / space dev as admin...

name      service                plan              bound apps     last operation
mypgsql   azure-postgresql-9-6   general-purpose                  create succeeded
```

Ist die Bereitstellung des Diensts abgeschlossen, hat *last operation* (letzter Vorgang) den Wert *create succeeded* (erfolgreich erstellt).

## <a name="bind-the-database-to-your-application"></a>Binden der Datenbank an Ihre Anwendung

Verwenden Sie den Befehl `bind-service`, um den Dienst an Ihre Anwendung zu binden.

```cmd
cf bind-service spring-music mypgsql
```

Nachdem Sie den Dienst an Ihre Anwendung gebunden haben, müssen Sie die Anwendung erneut bereitstellen, damit die Änderungen wirksam werden.

```cmd
cf restage spring-music
```

Die Anwendung ist [für die Verwendung von Spring-Cloudconnectors konfiguriert](https://github.com/cloudfoundry-samples/spring-music/blob/master/build.gradle#L45...L46), was Ihrer Pivotal Cloud Foundry-Instanz die Verwendung der [automatischen Neukonfiguration](https://docs.cloudfoundry.org/buildpacks/java/configuring-service-connections/spring-service-bindings.html#auto) für die Anwendungsdatenquelle ermöglicht. In diesem Fall konfiguriert Ihre Pivotal Cloud Foundry-Instanz die Anwendung automatisch so, dass sie einen gebundenen Dienst als Datenquelle verwendet, wenn Ihre Anwendung erneut bereitgestellt wird.

Nach der erneuten Bereitstellung speichert Ihre Anwendung Daten nicht mehr in der In-Memory Database, sondern in *mypgsql*.

Sämtliche Änderungen, die Sie vornehmen, bleiben nun auch über Neustarts und erneute Bereitstellungen hinweg erhalten. Die von der Anwendung verwendete Verbindungs-URL ist außerdem in den Anwendungsprotokollen enthalten:

```cmd
cf logs spring-music --recent | grep jdbcUrl

2018-12-07T14:44:30.57-0600 [APP/PROC/WEB/0] OUT 2018-12-07 20:44:30.574 DEBUG 24 --- [           main] com.zaxxer.hikari.HikariConfig           : jdbcUrl.........................jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
2018-12-07T14:48:58.10-0600 [APP/PROC/WEB/0] OUT 2018-12-07 20:48:58.107 DEBUG 16 --- [           main] com.zaxxer.hikari.HikariConfig           : jdbcUrl.........................jdbc:postgresql://12345678-aaaa-bbbb-cccc-1234567890ab.postgres.database.azure.com:5432/123456789?user=aabbcc1122@12345678-aaaa-bbbb-cccc-1234567890ab&password=<masked>&&sslmode=require
```

Wie Sie sehen, gibt es zwei Einträge:

* Den ursprünglichen Wert von *h2:mem:testdb*
* Den aktualisierten Wert für Ihre PostgreSQL-Datenbank nach der erneuten Bereitstellung Ihrer Anwendung

Vergewissern Sie sich, dass die Daten in der PostgreSQL-Datenbank dauerhaft gespeichert werden. Navigieren Sie hierzu in Ihrem Browser zu der Anwendung, nehmen Sie einige Änderungen vor, und stellen Sie Ihre Anwendung anschließend erneut bereit:

```cmd
cf restage spring-music
```

Navigieren Sie anschließend in einem Browser unter Verwendung der gleichen URL zu der erneut bereitgestellten Anwendung. Wie Sie sehen, sind die vorgenommenen Änderungen weiterhin vorhanden.

## <a name="cleanup"></a>Bereinigen

Wenn Sie die Verbindung zwischen Ihrer Anwendung und der Datenbank trennen möchten, können Sie die Bindung mithilfe des Befehls `cf unbind-service` aufheben.

```cmd
cf unbind-service spring-music mypgsql
```

Die Anwendung muss ähnlich wie beim Binden der Anwendung an einen Dienst erneut bereitgestellt werden, damit die Änderungen wirksam werden. *mypgsql* und Ihre Anwendung bleiben bei dieser Aktion intakt, Ihre Anwendung verwendet jedoch wieder die In-Memory Database anstelle von *mypgsql*.

Die Datenbank kann mithilfe des Befehls `cf delete-service` gelöscht werden. *Diese Aktion kann nicht rückgängig gemacht werden. Überlegen Sie sich daher gut, ob Sie Ihre Datenbank löschen möchten, bevor Sie den Vorgang fortsetzen.*

```cmd
cf delete-service mypgsql
```

So entfernen Sie Ihre Anwendung aus Ihrer Pivotal Cloud Foundry-Instanz:
 
```cmd
cf delete spring-music
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie eine Anwendung in Pivotal Cloud Foundry bereitstellen und eine Datenbank unter Verwendung von Open Service Broker für Azure erstellen. Darüber hinaus wurde gezeigt, wie Sie die Datenbank an Ihre Anwendung in der Pivotal Cloud Foundry-Instanz binden. Weitere Informationen zur Anwendungsbereitstellung in Cloud Foundry in Azure finden Sie hier:

* [Cloud Foundry in Azure](../virtual-machines/linux/cloudfoundry-get-started.md)
* [Bereitstellen Ihrer ersten App in Cloud Foundry in Microsoft Azure](../virtual-machines/linux/cloudfoundry-deploy-your-first-app.md)