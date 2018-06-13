---
title: Hinzufügen einer Java-Anwendung in Azure App Service-Web-Apps
description: In diesem Lernprogramm erfahren Sie, wie Sie eine Seite oder Anwendung zu Ihrer Instanz von Azure-App Service Web-Apps hinzufügen, die bereits für die Verwendung von Java konfiguriert ist.
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: mbaldwin
ms.assetid: 9b46528b-e2d0-4f26-b8d7-af94bd8c31ef
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/11/2018
ms.author: robmcm
ms.openlocfilehash: 71009370282fcfbd71c00b30d4ea22802035714d
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
ms.locfileid: "31513634"
---
# <a name="add-a-java-application-to-azure-app-service-web-apps"></a>Hinzufügen einer Java-Anwendung in Azure App Service-Web-Apps
Nachdem Sie Ihre Java Web-App in [Azure App Service][Azure App Service] wie unter [Erstellen einer Java-Web-App in Azure App Service](app-service-web-get-started-java.md) beschrieben initialisiert haben, können Sie die Anwendung hochladen, indem Sie Ihre WAR-Datei im Ordner **webapps** speichern.

iiDer Navigationspfad zum Ordner **webapps** hängt davon ab, wie Sie Ihre Web-Apps-Instanz einrichten.

* Wenn Sie Ihre Web-App über den Azure Marketplace einrichten, hat der Pfad zum Ordner **webapps** das Format **D:\home\site\wwwroot\bin\application\_server\webapps**, wobei **application\_server** für den Namen des Anwendungsservers steht, der für Ihre Web-Apps-Instanz verwendet wird. 
* Wenn Sie Ihre Web-App über die Azure-Konfigurationsbenutzeroberfläche einrichten, hat der Pfad zum Ordner **webapps** das Format **D:\home\site\wwwroot\webapps**. 

Beachten Sie, dass Sie die Quellcodeverwaltung zum Hochladen Ihrer Anwendung oder Webseiten verwenden können, auch in [Szenarien mit fortlaufender Integration](app-service-continuous-deployment.md). FTP ist auch eine Option für das Hochladen Ihrer Anwendung oder Webseiten. Weitere Informationen zum Bereitstellen von Anwendungen über FTP finden Sie unter [Deploy your app using FTP (Bereitstellen Ihrer App mithilfe von FTP)](app-service-deploy-ftp.md).

Hinweis für Tomcat-Web-Apps: Nachdem Sie Ihre WAR-Datei in den Ordner **webapps** hochgeladen haben, erkennt der Tomcat-Anwendungsserver die neu hinzugefügte Datei und lädt diese automatisch. Beachten Sie, dass Sie beim Kopieren von Dateien in das ROOT-Verzeichnis den Anwendungsserver neu starten müssen, damit diese Dateien verwendet werden. Dies gilt nicht für WAR-Dateien. Die Funktion für automatisches Laden für in Azure ausgeführte Tomcat-Java-Web-Apps basiert auf einer neuen, hinzugefügten WAR-Datei oder neuen Dateien oder Verzeichnissen, die dem Ordner **webapps** hinzugefügt wurden. 

<a name="see-also"></a>

## <a name="see-also"></a>Siehe auch
Weitere Informationen zum Verwenden von Azure mit Java finden Sie im [Azure Java Developer Center].

[application-insights-app-insights-java-get-started](../application-insights/app-insights-java-get-started.md)

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
