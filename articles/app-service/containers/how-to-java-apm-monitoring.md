---
title: Konfigurieren von Java-APM- und Überwachungstools mit Azure App Service unter Linux
description: Es wird beschrieben, wie Sie Protokolle und Metriken für Ihre Java-Anwendungen, die unter App Service Linux ausgeführt werden, an NewRelic- und App Dynamics-APM-Anbieter senden.
services: app-service\web
author: rloutlaw
manager: angerobe
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 11/27/2018
ms.author: astay;routlaw
ms.openlocfilehash: 06ae71fea1b85a74d87588d2635038c64c8540cc
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52577103"
---
# <a name="how-to-application-performance-monitoring-tools-with-java-apps-on-azure-app-service-on-linux"></a>Gewusst wie: Tools für die Überwachung der Anwendungsleistung mit Java-Apps in Azure App Service unter Linux

In diesem Artikel wird veranschaulicht, wie Sie Java-Anwendungen, die in Azure App Service unter Linux bereitgestellt werden, mit den Plattformen NewRelic und AppDynamics für die Überwachung der Anwendungsleistung (Application Performance Monitoring, APM) verbinden.

## <a name="configure-new-relic"></a>Konfigurieren von NewRelic
1. Erstellen Sie ein NewRelic-Konto unter [NewRelic.com](https://newrelic.com/signup).
1. Laden Sie den Java-Agent von NewRelic herunter. Der Dateiname hat etwa das Format `newrelic-java-x.x.x.zip`.
1. Kopieren Sie Ihren Lizenzschlüssel. Sie benötigen ihn später zum Konfigurieren des Agents.
1. [Stellen Sie eine SSH-Verbindung mit Ihrer App Service-Instanz her](/azure/app-service/containers/app-service-linux-ssh-support), und erstellen Sie das neue Verzeichnis `/home/site/wwwroot/apm`. 
1. Laden Sie die entpackten Dateien für den NewRelic-Java-Agent in ein Verzeichnis unter `/home/site/wwwroot/apm` herunter. Die Dateien für Ihren Agent sollten sich in `/home/site/wwwroot/apm/newrelic` befinden.
1. Ändern Sie die YAML-Datei unter `/home/site/wwwroot/apm/newrelic/newrelic.yml`, und ersetzen Sie den Platzhalter-Lizenzwert durch Ihren eigenen Lizenzschlüssel.
1. Navigieren Sie im Azure-Portal zu Ihrer Anwendung in App Service, und erstellen Sie eine neue Anwendungseinstellung.
    - Wenn für Ihre App **Java SE** verwendet wird, sollten Sie die Umgebungsvariable `JAVA_OPTS` mit dem Wert `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar` erstellen.
    - Wenn **Tomcat** verwendet wird, sollten Sie die Umgebungsvariable `CATALINA_OPTS` mit dem Wert `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar` erstellen.
    - Falls Sie bereits über eine Umgebungsvariable für `JAVA_OPTS` oder `CATALINA_OPTS` verfügen, können Sie die Option `javaagent` am Ende des aktuellen Werts anhängen.

## <a name="configure-appdynamics"></a>Konfigurieren von AppDynamics
1. Erstellen Sie unter [AppDynamics.com](https://www.appdynamics.com/community/register/) ein AppDynamics-Konto.
1. Laden Sie den Java-Agent von der AppDynamics-Website herunter. Der Dateiname hat etwa das Format `AppServerAgent-x.x.x.xxxxx.zip`.
1. [Stellen Sie eine SSH-Verbindung mit Ihrer App Service-Instanz her](/azure/app-service/containers/app-service-linux-ssh-support), und erstellen Sie das neue Verzeichnis `/home/site/wwwroot/apm`. 
1. Laden Sie die Dateien des Java-Agents in ein Verzeichnis unter `/home/site/wwwroot/apm` hoch. Die Dateien für Ihren Agent sollten sich in `/home/site/wwwroot/apm/appdynamics` befinden.
1. Navigieren Sie im Azure-Portal zu Ihrer Anwendung in App Service, und erstellen Sie eine neue Anwendungseinstellung.
    - Erstellen Sie bei Verwendung von **Java SE** die Umgebungsvariable `JAVA_OPTS` mit dem Wert `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<YOUR_SITE_NAME>`, wobei `<YOUR_SITE_NAME>` für Ihren App Service-Namen steht.
    - Erstellen Sie bei Verwendung von **Tomcat** die Umgebungsvariable `CATALINA_OPTS` mit dem Wert `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<YOUR_SITE_NAME>`, wobei `<YOUR_SITE_NAME>` für Ihren App Service-Namen steht.
