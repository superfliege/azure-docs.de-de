---
title: Kompilieren von Java-Web-Apps unter Linux – Azure App Service
description: Kompilieren, Bereitstellen und Skalieren von Spring Boot-Java-Web-Apps mit Azure App Service unter Linux und Azure Cosmos DB.
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.service: app-service-web
ms.devlang: java
ms.topic: tutorial
ms.date: 12/10/2018
ms.custom: seodec18
ms.openlocfilehash: e148165afd91d76917e06577198950ee7552995f
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54107705"
---
# <a name="tutorial-build-a-java-web-app-using-spring-and-azure-cosmos-db"></a>Tutorial: Kompilieren einer Java-Web-App mithilfe von Spring und Azure Cosmos DB

Dieses Tutorial führt Sie durch den Prozess der Kompilierung, Konfiguration, Bereitstellung und Skalierung Java-Web-Apps in Azure. Anschließend besitzen Sie eine [Spring Boot](https://projects.spring.io/spring-boot/)-Anwendung, die Daten in [Azure Cosmos DB](/azure/cosmos-db) speichert und mit [Azure App Service unter Linux](/azure/app-service/containers) ausgeführt wird.

![In Azure App Service ausgeführte Java-App](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-locally.jpg)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eine Cosmos DB-Datenbank
> * Herstellen einer Verbindung zwischen einer Beispiel-App und der Datenbank und das lokale Testen
> * Bereitstellen der Beispiel-App in Azure
> * Streamen von Diagnoseprotokollen aus Azure Service
> * Hinzufügen zusätzlicher Instanzen zum horizontalen Hochskalieren der Beispiel-App

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Installation der [Azure CLI](https://docs.microsoft.com/cli/azure/overview) auf Ihrem eigenen Computer. 
* [Git-Client](https://git-scm.com/)
* [Java JDK](https://aka.ms/azure-jdks)
* [Maven](https://maven.apache.org)

## <a name="clone-the-sample-todo-app-and-prepare-the-repo"></a>Klonen der TODO-Beispiel-App und Vorbereiten des Repositorys

Dieses Tutorial verwendet eine TODO-Beispiel-Listen-App mit einer Web-Benutzeroberfläche, die eine Spring REST-API aufruft, die durch [Spring Data Azure Cosmos DB](https://github.com/Microsoft/spring-data-cosmosdb) unterstützt wird. Den Code für die App finden Sie auf [GitHub](https://github.com/Microsoft/spring-todo-app). Weitere Informationen zum Schreiben von Java-Apps mit Spring und Cosmos DB finden Sie in [Verwendung von Spring Boot Starter mit der SQL-API von Azure Cosmos DB](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db ) und im [Schnellstart zu Spring Data Azure Cosmos DB](https://github.com/Microsoft/spring-data-cosmosdb#quick-start).


Führen Sie die folgenden Befehle in Ihrem Terminal aus, um das Beispielrepository zu klonen und die Beispiel-App-Umgebung einzurichten.

```bash
git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
cd e2e-java-experience-in-app-service-linux-part-2
yes | cp -rf .prep/* .
```

## <a name="create-an-azure-cosmos-db"></a>Erstellen einer Azure Cosmos DB-Instanz

Führen Sie diese Schritte aus, um eine Azure Cosmos DB-Datenbank in Ihrem Abonnement zu erstellen. Die TODO-Listen-App verbindet sich mit dieser Datenbank und speichert ihre Daten während der Ausführung, wobei der Anwendungszustand erhalten bleibt, unabhängig davon, wo Sie die Anwendung ausführen.

1. Melden Sie sich mit Ihrer Azure CLI an und legen Sie optional Ihr Abonnement fest, wenn Sie mehr als eines mit Ihren Anmeldeinformationen verbunden haben.

    ```bash
    az login
    az account set -s <your-subscription-id>
    ```   

2. Erstellen Sie eine Azure-Ressourcengruppe, und notieren Sie den Namen der Ressourcengruppe.

    ```bash
    az group create -n <your-azure-group-name> \
        -l <your-resource-group-region>
    ```

3. Erstellen Sie eine Azure Cosmos DB mit der Art `GlobalDocumentDB`. Der Name der Cosmos DB darf nur Kleinbuchstaben enthalten. Notieren Sie sich das Feld `documentEndpoint` in der Antwort des Befehls.

    ```bash
    az cosmosdb create --kind GlobalDocumentDB \
        -g <your-azure-group-name> \
        -n <your-azure-COSMOS-DB-name-in-lower-case-letters>
    ```

4. Rufen Sie Ihren Azure Cosmos DB-Schlüssel für die Verbindung zur App ab. Halten Sie die Werte für `primaryMasterKey`, `documentEndpoint` bereit, da Sie sie im nächsten Schritt brauchen.

    ```bash
    az cosmosdb list-keys -g <your-azure-group-name> -n <your-azure-COSMOSDB-name>
    ```

## <a name="configure-the-todo-app-properties"></a>Konfigurieren der TODO-App-Eigenschaften

Öffnen Sie ein Terminal auf Ihrem Computer. Kopieren Sie die Beispielskriptdatei in das geklonte Repository, damit Sie sie für Ihre gerade erstellte Cosmos DB-Datenbank anpassen können.

```bash
cd initial/spring-todo-app
cp set-env-variables-template.sh .scripts/set-env-variables.sh
```
 
Bearbeiten Sie `.scripts/set-env-variables.sh` in Ihrem bevorzugten Editor und stellen Sie die Azure Cosmos DB-Verbindungsinformationen bereit. Verwenden Sie für die App Service-Linux-Konfiguration die gleiche Region wie zuvor (`your-resource-group-region`) und die Ressourcengruppe (`your-azure-group-name`), die beim Erstellen der Cosmos DB-Datenbank verwendet wurden. Wählen Sie einen eindeutigen WEBAPP_NAMEN, da er keinen Web-App-Namen in einer Azure-Bereitstellung duplizieren kann.

```bash
export COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
export COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
export COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>

// App Service Linux Configuration
export RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
export WEBAPP_NAME=<put-your-Webapp-name-here>
export REGION=<put-your-REGION-here>
```

Führen Sie dann das Skript aus:

```bash
source .scripts/set-env-variables.sh
```
   
Diese Umgebungsvariablen werden in `application.properties` in der TODO-Listen-App verwendet. Die Felder in der Eigenschaftsdatei richten eine Standardrepositorykonfiguration für Spring Data ein:

```properties
azure.cosmosdb.uri=${COSMOSDB_URI}
azure.cosmosdb.key=${COSMOSDB_KEY}
azure.cosmosdb.database=${COSMOSDB_DBNAME}
```

```java
@Repository
public interface TodoItemRepository extends DocumentDbRepository<TodoItem, String> {
}
```

Dann verwendet die Beispiel-App die aus `com.microsoft.azure.spring.data.cosmosdb.core.mapping.Document` importierte `@Document`-Anmerkung, um einen Entitätstyp einzurichten, der von Cosmos DB gespeichert und verwaltet wird:

```java
@Document
public class TodoItem {
    private String id;
    private String description;
    private String owner;
    private boolean finished;
```

## <a name="run-the-sample-app"></a>Ausführen der Beispiel-App

Verwenden Sie Maven, um das Beispiel auszuführen.

```bash
mvn package spring-boot:run
```

Die Ausgabe sollte wie im folgenden Beispiel aussehen.

```bash
bash-3.2$ mvn package spring-boot:run
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 


[INFO] SimpleUrlHandlerMapping - Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
[INFO] SimpleUrlHandlerMapping - Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
[INFO] WelcomePageHandlerMapping - Adding welcome page: class path resource [static/index.html]
2018-10-28 15:04:32.101  INFO 7673 --- [           main] c.m.azure.documentdb.DocumentClient      : Initializing DocumentClient with serviceEndpoint [https://sample-cosmos-db-westus.documents.azure.com:443/], ConnectionPolicy [ConnectionPolicy [requestTimeout=60, mediaRequestTimeout=300, connectionMode=Gateway, mediaReadMode=Buffered, maxPoolSize=800, idleConnectionTimeout=60, userAgentSuffix=;spring-data/2.0.6;098063be661ab767976bd5a2ec350e978faba99348207e8627375e8033277cb2, retryOptions=com.microsoft.azure.documentdb.RetryOptions@6b9fb84d, enableEndpointDiscovery=true, preferredLocations=null]], ConsistencyLevel [null]
[INFO] AnnotationMBeanExporter - Registering beans for JMX exposure on startup
[INFO] TomcatWebServer - Tomcat started on port(s): 8080 (http) with context path ''
[INFO] TodoApplication - Started TodoApplication in 45.573 seconds (JVM running for 76.534)
```

Sie können die Spring-TODO-App lokal über diesen Link aufrufen, sobald die App gestartet ist: [http://localhost:8080/](http://localhost:8080/).

 ![](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-locally.jpg)

Wenn Ausnahmen anstelle der Meldung „Started TodoApplication“ angezeigt werden, überprüfen Sie, ob das Skript `bash` im vorherigen Schritt die Umgebungsvariablen korrekt exportiert hat, und ob die Werte für die von Ihnen erstellte Azure Cosmos DB-Datenbank korrekt sind.

## <a name="configure-azure-deployment"></a>Konfigurieren der Azure-Bereitstellung

Öffnen Sie die Datei `pom.xml` im Verzeichnis `initial/spring-boot-todo` und fügen Sie die folgende Konfiguration [Maven-Plug-In für Azure App Service](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md) hinzu.

```xml    
<plugins> 

    <!--*************************************************-->
    <!-- Deploy to Java SE in App Service Linux           -->
    <!--*************************************************-->
       
    <plugin>
        <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-webapp-maven-plugin</artifactId>
            <version>1.4.0</version>
            <configuration>
            <deploymentType>jar</deploymentType>
            
            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
            
            <!-- Java Runtime Stack for Web App on Linux-->
            <linuxRuntime>jre8</linuxRuntime>
            
            <appSettings>
                <property>
                    <name>COSMOSDB_URI</name>
                    <value>${COSMOSDB_URI}</value>
                </property>
                <property>
                    <name>COSMOSDB_KEY</name>
                    <value>${COSMOSDB_KEY}</value>
                </property>
                <property>
                    <name>COSMOSDB_DBNAME</name>
                    <value>${COSMOSDB_DBNAME}</value>
                </property>
                <property>
                    <name>JAVA_OPTS</name>
                    <value>-Dserver.port=80</value>
                </property>
            </appSettings>
            
        </configuration>
    </plugin>            
    ...
</plugins>
```

## <a name="deploy-to-app-service-on-linux"></a>Bereitstellen für Azure App Service unter Linux

Verwenden Sie das Maven-Ziel `azure-webapp:deploy`, um die TODO-App in Azure App Service unter Linux bereitzustellen.

```bash

// Deploy
bash-3.2$ mvn azure-webapp:deploy
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- azure-webapp-maven-plugin:1.4.0:deploy (default-cli) @ spring-todo-app ---
[INFO] Authenticate with Azure CLI 2.0
[INFO] Target Web App doesn't exist. Creating a new one...
[INFO] Creating App Service Plan 'ServicePlanb6ba8178-5bbb-49e7'...
[INFO] Successfully created App Service Plan.
[INFO] Successfully created Web App.
[INFO] Trying to deploy artifact to spring-todo-app...
[INFO] Successfully deployed the artifact to https://spring-todo-app.azurewebsites.net
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 02:19 min
[INFO] Finished at: 2018-10-28T15:32:03-07:00
[INFO] Final Memory: 50M/574M
[INFO] ------------------------------------------------------------------------
```

Die Ausgabe enthält die URL zu Ihrer bereitgestellten Anwendung (in diesem Beispiel `https://spring-todo-app.azurewebsites.net`). Sie können diese URL in Ihren Webbrowser kopieren oder den folgenden Befehl in Ihrem Terminalfenster ausführen, um Ihre App zu laden.

```bash
open https://spring-todo-app.azurewebsites.net
```

Sie sollten sehen, dass die App mit der Remote-URL in der Adressleiste ausgeführt wird:

 ![](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-in-app-service.jpg)

## <a name="view-logs-to-troubleshoot-the-app"></a>Anzeigen von Protokollen zur Problembehandlung für die App

Aktivieren Sie die Protokollierung für die eingesetzte Java-Web-App im App Service unter Linux:

```bash
az webapp log config --name ${WEBAPP_NAME} \
 --resource-group ${RESOURCEGROUP_NAME} \
  --web-server-logging filesystem
```

Streamen Sie anschließend die Web-App-Protokolle an Ihren Terminal:

```bash
az webapp log tail --name ${WEBAPP_NAME} \
 --resource-group ${RESOURCEGROUP_NAME}
```

Sie sehen die neuesten Ausgabezeilen und wenn neue Anforderungen an die TODO-App gestellt werden, werden sie auf der Konsole eingeblendet. Um die Konsole zu schließen, verwenden Sie STRG + C.

```bash
bash-3.2$ az webapp log tail --name ${WEBAPP_NAME}  --resource-group ${RESOURCEGROUP_NAME}
2018-10-28T22:50:17  Welcome, you are now connected to log-streaming service.
2018-10-28T22:44:56.265890407Z   _____                               
2018-10-28T22:44:56.265930308Z   /  _  \ __________ _________   ____  
2018-10-28T22:44:56.265936008Z  /  /_\  \___   /  |  \_  __ \_/ __ \ 
2018-10-28T22:44:56.265940308Z /    |    \/    /|  |  /|  | \/\  ___/ 
2018-10-28T22:44:56.265944408Z \____|__  /_____ \____/ |__|    \___  >
2018-10-28T22:44:56.265948508Z         \/      \/                  \/ 
2018-10-28T22:44:56.265952508Z A P P   S E R V I C E   O N   L I N U X
2018-10-28T22:44:56.265956408Z Documentation: https://aka.ms/webapp-linux
2018-10-28T22:44:56.266260910Z Setup openrc ...
2018-10-28T22:44:57.396926506Z Service `hwdrivers' needs non existent service `dev'
2018-10-28T22:44:57.397294409Z  * Caching service dependencies ... [ ok ]
2018-10-28T22:44:57.474152273Z Starting ssh service...
...
...
2018-10-28T22:46:13.432160734Z [INFO] AnnotationMBeanExporter - Registering beans for JMX exposure on startup
2018-10-28T22:46:13.744859424Z [INFO] TomcatWebServer - Tomcat started on port(s): 80 (http) with context path ''
2018-10-28T22:46:13.783230205Z [INFO] TodoApplication - Started TodoApplication in 57.209 seconds (JVM running for 70.815)
2018-10-28T22:46:14.887366993Z 2018-10-28 22:46:14.887  INFO 198 --- [p-nio-80-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring FrameworkServlet 'dispatcherServlet'
2018-10-28T22:46:14.887637695Z [INFO] DispatcherServlet - FrameworkServlet 'dispatcherServlet': initialization started
2018-10-28T22:46:14.998479907Z [INFO] DispatcherServlet - FrameworkServlet 'dispatcherServlet': initialization completed in 111 ms

2018-10-28T22:49:20.572059062Z Sun Oct 28 22:49:20 GMT 2018 GET ======= /api/todolist =======
2018-10-28T22:49:25.850543080Z Sun Oct 28 22:49:25 GMT 2018 DELETE ======= /api/todolist/{4f41ab03-1b12-4131-a920-fe5dfec106ca} ======= 
2018-10-28T22:49:26.047126614Z Sun Oct 28 22:49:26 GMT 2018 GET ======= /api/todolist =======
2018-10-28T22:49:30.201740227Z Sun Oct 28 22:49:30 GMT 2018 POST ======= /api/todolist ======= Milk
2018-10-28T22:49:30.413468872Z Sun Oct 28 22:49:30 GMT 2018 GET ======= /api/todolist =======
```

## <a name="scale-out-the-todo-app"></a>Horizontales Hochskalieren der TODO-App

Die Anwendung kann durch das Hinzufügen eines weiteren Workers horizontal hochskaliert werden:

```bash
az appservice plan update --number-of-workers 2 \
   --name ${WEBAPP_PLAN_NAME} \
   --resource-group <your-azure-group-name>
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese Ressourcen nicht für ein anderes Tutorial benötigen (siehe [Nächste Schritte](#next)), können Sie sie löschen, indem Sie den folgenden Befehl in Cloud Shell ausführen: 
  
```bash
az group delete --name your-azure-group-name
``` 

<a name="next"></a>

## Next steps

[Azure for Java Developers](/java/azure/)
[Spring Boot](https://spring.io/projects/spring-boot), 
[Spring Data for Cosmos DB](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db?view=azure-java-stable), 
[Azure Cosmos DB](/azure/cosmos-db/sql-api-introduction)
and
[App Service Linux](/azure/app-service/containers/app-service-linux-intro).

Learn more about running Java apps on App Service on Linux in the developer guide.

> [!div class="nextstepaction"] 
> [Java in App Service Linux dev guide](/azure/app-service/containers/app-service-linux-java)
