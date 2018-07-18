---
title: Schnellstartanleitung zum Erstellen einer Java-Web-App in Azure App Service unter Linux
description: In dieser Schnellstartanleitung stellen Sie in wenigen Minuten Ihre erste Java-App „Hallo Welt“ in Azure App Service unter Linux bereit.
services: app-service\web
documentationcenter: ''
author: msangapu
manager: cfowler
editor: ''
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: quickstart
ms.date: 03/07/2018
ms.author: msangapu
ms.custom: mvc
ms.openlocfilehash: 49702349b1c2476f5743122b33cb3375e54df191
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37930095"
---
# <a name="quickstart-create-a-java-web-app-in-app-service-on-linux"></a>Schnellstart: Erstellen einer Java-Web-App in App Service unter Linux

In App Service unter Linux wird derzeit eine Vorschaufunktion für die Unterstützung von Java-Web-Apps bereitgestellt. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

[App Service unter Linux](app-service-linux-intro.md) bietet einen hochgradig skalierbaren Webhostingdienst mit Self-Patching unter dem Linux-Betriebssystem. In dieser Schnellstartanleitung wird gezeigt, wie Sie mit der [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) und dem [Maven-Plug-In für Azure-Web-Apps (Vorschauversion)](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) eine Java-Web-App mit einem integrierten Linux-Image bereitstellen.

![In Azure ausgeführte Beispiel-App](media/quickstart-java/java-hello-world-in-browser.png)

Unter [Bereitstellen einer Web-App vom Typ „Hallo Welt“ in einem Linux-Container in der Cloud mit dem Azure-Toolkit für IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-hello-world-web-app-linux) ist ein alternativer Ansatz für die Bereitstellung der Java-App in Ihrem eigenen Container dargestellt.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch: 

* Lokale Installation von [Azure CLI 2.0 oder höher](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* [Apache Maven](http://maven.apache.org/).



## <a name="create-a-java-app"></a>Erstellen einer Java-App

Führen Sie den folgenden Befehl mit Maven aus, um eine neue Web-App vom Typ *helloworld* zu erstellen:  

    mvn archetype:generate -DgroupId=example.demo -DartifactId=helloworld -DarchetypeArtifactId=maven-archetype-webapp

Wechseln Sie zum neuen Projektverzeichnis von *helloworld*, und erstellen Sie mithilfe des folgenden Befehls alle Module:

    mvn verify

Mit diesem Befehl werden alle Module (einschließlich der Datei *helloworld.war* im Unterverzeichnis *helloworld/target*) überprüft und erstellt.


## <a name="deploying-the-java-app-to-app-service-on-linux"></a>Bereitstellen der Java-App für App Service unter Linux

Für die Bereitstellung Ihrer Java-Web-Apps in App Service unter Linux gibt es mehrere Bereitstellungsoptionen. Die Optionen umfassen:

* [Bereitstellen mithilfe des Maven-Plug-Ins für Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin)
* [Bereitstellen mit einer ZIP- oder WAR-Datei](https://docs.microsoft.com/azure/app-service/app-service-deploy-zip)
* [Bereitstellen über FTP](https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp)

In dieser Schnellstartanleitung verwenden Sie das Maven-Plug-In für Azure-Web-Apps. Der Vorteil dabei ist, dass es einfach über Maven verwendet werden kann und die erforderlichen Azure-Ressourcen (Ressourcengruppe, App Service-Plan und Web-App) für Sie erstellt.

### <a name="deploy-with-maven"></a>Bereitstellen mit Maven

Fügen Sie zum Bereitstellen über Maven die folgende Plug-In-Definition im `<build>`-Element der Datei *pom.xml* hinzu:

```xml
    <plugins>
      <plugin>
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-webapp-maven-plugin</artifactId> 
        <version>1.2.0</version>
        <configuration> 
          <resourceGroup>YOUR_RESOURCE_GROUP</resourceGroup> 
          <appName>YOUR_WEB_APP</appName> 
          <linuxRuntime>tomcat 9.0-jre8</linuxRuntime>
          <deploymentType>ftp</deploymentType> 
          <resources> 
              <resource> 
                  <directory>${project.basedir}/target</directory> 
                  <targetPath>webapps</targetPath> 
                  <includes> 
                      <include>*.war</include> 
                  </includes> 
                  <excludes> 
                      <exclude>*.xml</exclude> 
                  </excludes> 
              </resource> 
          </resources> 
        </configuration>
      </plugin>
    </plugins>
```    

Aktualisieren Sie die folgenden Platzhalter in der Plug-In-Konfiguration:

| Platzhalter | BESCHREIBUNG |
| ----------- | ----------- |
| `YOUR_RESOURCE_GROUP` | Der Name der neuen Ressourcengruppe, in der Ihre Web-App erstellt wird. Indem Sie alle Ressourcen für eine App in einer Gruppe zusammenfassen, können Sie sie zusammen verwalten. Wenn Sie beispielsweise die Ressourcengruppe löschen, werden alle Ressourcen gelöscht, die der App zugeordnet sind. Aktualisieren Sie diesen Wert mit einem neuen eindeutigen Ressourcengruppennamen, z.B. *TestResources*. In einem Abschnitt weiter unten verwenden Sie diesen Ressourcengruppennamen zum Bereinigen aller Azure-Ressourcen. |
| `YOUR_WEB_APP` | Der App-Name wird bei der Bereitstellung in Azure als Teil des Hostnamens für die Web-App (YOUR_WEB_APP.azurewebsites.net) verwendet. Aktualisieren Sie diesen Wert mit einem eindeutigen Namen für die neue Azure-Web-App, die Ihre Java-App hostet (z.B. *contoso*). |

Das `linuxRuntime`-Element der Konfiguration steuert, welches integrierte Linux-Image mit Ihrer Anwendung verwendet wird. Alle unterstützten Runtimestapel sind unter [diesem Link](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin#runtime-stacks) aufgeführt. 


> [!NOTE] 
> In diesem Artikel arbeiten wir nur mit WAR-Dateien. Das Plug-In unterstützt jedoch die JAR-Webanwendungen und verwendet die folgende Plug-In-Definition im `<build>`-Element einer *pom.xml*-Datei:
>
>```xml
>    <plugins>
>      <plugin>
>        <groupId>com.microsoft.azure</groupId> 
>        <artifactId>azure-webapp-maven-plugin</artifactId> 
>        <version>1.2.0</version>
>        <configuration> 
>          <resourceGroup>YOUR_RESOURCE_GROUP</resourceGroup> 
>          <appName>YOUR_WEB_APP</appName> 
>          <linuxRuntime>jre8</linuxRuntime>   
>          <!-- This is to make sure the jar file will not be occupied during the deployment -->
>          <stopAppDuringDeployment>true</stopAppDuringDeployment>
>          <deploymentType>ftp</deploymentType> 
>          <resources> 
>              <resource> 
>                  <directory>${project.basedir}/target</directory> 
>                  <targetPath>webapps</targetPath> 
>                  <includes> 
>                      <!-- Currently it is required to set as app.jar -->
>                      <include>app.jar</include> 
>                  </includes>  
>              </resource> 
>          </resources> 
>        </configuration>
>      </plugin>
>    </plugins>
>```    

Führen Sie den folgenden Befehl aus, und befolgen Sie alle Anweisungen zum Authentifizieren mit der Azure CLI:

    az login

Stellen Sie mit dem folgenden Befehl Ihre Java-App in der Web-App bereit:

    mvn clean package azure-webapp:deploy


Navigieren Sie nach Abschluss der Bereitstellung zur bereitgestellten Anwendung, indem Sie in Ihrem Webbrowser die folgende URL verwenden:

```bash
http://<app_name>.azurewebsites.net/helloworld
```

Der Java-Beispielcode wird in einer Web-App mit integriertem Image ausgeführt.

![In Azure ausgeführte Beispiel-App](media/quickstart-java/java-hello-world-in-browser-curl.png)

**Glückwunsch!** Sie haben Ihre erste Java-App für App Service unter Linux bereitgestellt.


[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]


## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Maven zum Erstellen einer Java-Web-App verwendet und dann die Java-Web-App in App Service unter Linux bereitgestellt. Weitere Informationen zur Verwendung von Java mit Azure finden Sie unter folgendem Link:

> [!div class="nextstepaction"]
> [Azure für Java-Entwickler](https://docs.microsoft.com/java/azure/)

