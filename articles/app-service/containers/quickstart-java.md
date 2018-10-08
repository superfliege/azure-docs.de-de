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
ms.openlocfilehash: b9e8d2b9eacfa5c427ffe3f27ea99bbd35651d57
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47165975"
---
# <a name="quickstart-create-a-java-web-app-in-app-service-on-linux"></a>Schnellstart: Erstellen einer Java-Web-App in App Service unter Linux

[App Service unter Linux](app-service-linux-intro.md) bietet einen hochgradig skalierbaren Webhostingdienst mit Self-Patching unter dem Linux-Betriebssystem. In dieser Schnellstartanleitung wird gezeigt, wie Sie mit der [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) und dem [Maven-Plug-In für Azure-Web-Apps (Vorschauversion)](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) eine Webarchivdatei (WAR) der Java-Web-App bereitstellen.

![In Azure ausgeführte Beispiel-App](media/quickstart-java/java-hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Erstellen einer Java-App

Führen Sie den folgenden Maven-Befehl über die Eingabeaufforderung der Cloud Shell aus, um eine neue Web-App namens `helloworld` zu erstellen:

```bash
mvn archetype:generate -DgroupId=example.demo -DartifactId=helloworld -DarchetypeArtifactId=maven-archetype-webapp
```

## <a name="configure-the-maven-plugin"></a>Konfigurieren des Maven-Plug-Ins

Verwenden Sie zum Bereitstellen über Maven den Code-Editor in der Cloud Shell, um die Projektdatei `pom.xml` im Verzeichnis `helloworld` zu öffnen. 

```bash
code pom.xml
```

Fügen Sie dann die folgende Plug-In-Definition im `<build>`-Element der Datei `pom.xml` hinzu.

```xml
<plugins>
    <!--*************************************************-->
    <!-- Deploy to Tomcat in App Service Linux           -->
    <!--*************************************************-->
      
    <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.4.0</version>
        <configuration>
   
            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
   
            <!-- Java Runtime Stack for Web App on Linux-->
            <linuxRuntime>tomcat 8.5-jre8</linuxRuntime>
   
        </configuration>
    </plugin>
</plugins>
```    


> [!NOTE] 
> In diesem Artikel arbeiten wir nur mit Java-Apps, die in WAR-Dateien enthalten sind. Das Plug-In unterstützt auch JAR-Webanwendungen. Verwenden Sie die folgende alternative Plug-In-Definition für diese Anwendungen. Diese Konfiguration stellt eine von Maven erstellte JAR-Datei bei `${project.build.directory}/${project.build.finalName}.jar` auf Ihrem lokalen Dateisystem bereit.
>
>```xml
><plugin>
>            <groupId>com.microsoft.azure</groupId>
>            <artifactId>azure-webapp-maven-plugin</artifactId>
>            <version>1.4.0</version>
>            <configuration>
>                <deploymentType>jar</deploymentType>
>
>           <!-- Web App information -->
>            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
>            <appName>${WEBAPP_NAME}</appName>
>            <region>${REGION}</region>  
>
>                <!-- Java Runtime Stack for Web App on Linux-->
>                <linuxRuntime>jre8</linuxRuntime>
>            </configuration>
>         </plugin>
>```    


Aktualisieren Sie die folgenden Platzhalter in der Plug-In-Konfiguration:

| Platzhalter | BESCHREIBUNG |
| ----------- | ----------- |
| `RESOURCEGROUP_NAME` | Der Name der neuen Ressourcengruppe, in der Ihre Web-App erstellt wird. Indem Sie alle Ressourcen für eine App in einer Gruppe zusammenfassen, können Sie sie zusammen verwalten. Wenn Sie beispielsweise die Ressourcengruppe löschen, werden alle Ressourcen gelöscht, die der App zugeordnet sind. Aktualisieren Sie diesen Wert mit einem neuen eindeutigen Ressourcengruppennamen, z.B. *TestResources*. In einem Abschnitt weiter unten verwenden Sie diesen Ressourcengruppennamen zum Bereinigen aller Azure-Ressourcen. |
| `WEBAPP_NAME` | Der App-Name wird bei der Bereitstellung in Azure als Teil des Hostnamens für die Web-App (WEBAPP_NAME.azurewebsites.net) verwendet. Aktualisieren Sie diesen Wert mit einem eindeutigen Namen für die neue Azure-Web-App, die Ihre Java-App hostet (z.B. *contoso*). |
| `REGION` | Eine Azure-Region, in der die Web-App gehostet wird, z. B. `westus2`. Sie können eine Liste von Regionen über die Cloud Shell oder CLI mit dem Befehl `az account list-locations` abrufen. |

## <a name="deploy-the-app"></a>Bereitstellen der App

Stellen Sie Ihre Java-App mit dem folgenden Befehl in Azure bereit:

```bash
mvn package azure-webapp:deploy
```

Navigieren Sie nach Abschluss der Bereitstellung zur bereitgestellten Anwendung, indem Sie in Ihrem Webbrowser die folgende URL verwenden, z. B. `http://<webapp>.azurewebsites.net/helloworld`. 

![In Azure ausgeführte Beispiel-App](media/quickstart-java/java-hello-world-in-browser-curl.png)

**Glückwunsch!** Sie haben Ihre erste Java-App für App Service unter Linux bereitgestellt.


[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]


## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie mit Maven eine Java-Webanwendung erstellt, das [Maven-Plug-In für Azure Web Apps (Vorschau)](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) konfiguriert und dann eine in einem Webarchiv verpackte Java-Webanwendung für den App Service unter Linux bereitgestellt. Weitere Informationen zur Verwendung von Java mit Azure finden Sie unter folgendem Link:

> [!div class="nextstepaction"]
> [Azure für Java-Entwickler](https://docs.microsoft.com/java/azure/)

