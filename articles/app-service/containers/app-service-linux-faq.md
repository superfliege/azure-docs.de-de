---
title: Häufig gestellte Fragen (FAQ) zu Azure App Service unter Linux | Microsoft-Dokumentation
description: Häufig gestellte Fragen (FAQ) zu Azure App Service unter Linux.
keywords: Azure App Service, Web-App, häufig gestellte Fragen, Linux, OSS
services: app-service
documentationCenter: ''
author: ahmedelnably
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2018
ms.author: msangapu
ms.openlocfilehash: 5b3b3d3946b56ff53ad74c2ab93a646baa787d05
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36222976"
---
# <a name="azure-app-service-on-linux-faq"></a>Häufig gestellte Fragen (FAQ) zu Azure App Service unter Linux

Mit dem Release von App Service unter Linux erweitern wir unsere Plattform um Features und nehmen Verbesserungen an ihr vor. Dieser Artikel enthält Antworten auf Fragen, die uns kürzlich von Kunden gestellt wurden.

Wenn Sie eine Frage haben, schreiben Sie einen Kommentar zu diesem Artikel.

## <a name="built-in-images"></a>Integrierte Images

**Ich möchte die integrierten Docker-Container nutzen, die von der Plattform bereitgestellt werden. Wo finde ich diese Dateien?**

Sie finden alle Docker-Dateien auf [GitHub](https://github.com/azure-app-service). Sie finden alle Docker-Container auf [Docker Hub](https://hub.docker.com/u/appsvc/).

**Welche Werte sind beim Konfigurieren des Laufzeitstapels im Abschnitt „Startdatei“ anzugeben?**

Für Node.js geben Sie die PM2-Konfigurationsdatei oder Ihre Skriptdatei an. Für .NET Core geben Sie den Namen der kompilierten DLL-Datei als `dotnet <myapp>.dll` an. Für Ruby können Sie das Ruby-Skript angeben, mit dem Ihre App initialisiert werden soll.

## <a name="management"></a>Verwaltung

**Was geschieht, wenn ich auf die Schaltfläche „Neu starten“ im Azure-Portal klicke?**

Diese Aktion ist mit dem Docker-Neustart identisch.

**Kann ich über Secure Shell (SSH) eine Verbindung mit dem virtuellen Computer des App-Containers herstellen?**

Ja, Sie können dazu die Website für die Quellcodeverwaltung (Source Control Management, SCM) verwenden.

> [!NOTE]
> Sie können auch direkt über SSH, SFTP oder Visual Studio Code (zum Livedebuggen von Node.js-Apps) eine Verbindung vom lokalen Entwicklungscomputer zum App-Container herstellen. Weitere Informationen finden Sie unter [Remotedebuggen und SSH in App Service unter Linux](https://aka.ms/linux-debug).
>

**Wie kann ich einen Linux-App Service-Plan über ein SDK oder eine Azure Resource Manager-Vorlage erstellen?**

Legen Sie das App-Dienstfeld **reserviert** auf *TRUE* fest.

## <a name="continuous-integration-and-deployment"></a>Continuous Integration und Continuous Deployment

**Meine Web-App verwendet nach dem Aktualisieren des Images auf Docker Hub noch ein altes Docker-Containerimage. Unterstützen Sie Continuous Integration/Deployment von benutzerdefinierten Containern?**

Ja. Informationen zum Einrichten von Continuous Integration/Continuous Deployment für Azure Container Registry oder DockerHub finden Sie unter [Continuous Deployment mit Web-App für Container](./app-service-linux-ci-cd.md) (in englischer Sprache). Bei privaten Registrierungen können Sie den Container aktualisieren, indem Sie Ihre Web-App beenden und dann wieder starten. Sie können auch eine Dummyeinstellung in der Anwendung ändern oder hinzufügen, um die Aktualisierung Ihres Containers zu erzwingen.

**Werden Stagingumgebungen unterstützt?**

Ja.

**Kann ich *Web Deploy* zur Bereitstellung meiner Web-App verwenden?**

Ja, Sie müssen die App-Einstellung `WEBSITE_WEBDEPLOY_USE_SCM` auf *false* festlegen.

**Bei der Git-Bereitstellung meiner Anwendung tritt ein Fehler auf, wenn ich eine Linux-Web-App verwende. Wie kann ich dieses Problem umgehen?**

Wenn bei der Git-Bereitstellung in Ihrer Linux-Web-App ein Fehler auftritt, stellen Sie Anwendungscodes wie folgt bereit:

- Verwenden Sie das Feature für Continuous Delivery (Vorschauversion): Sie können den Quellcode Ihrer App in einem Team Services-Git-Repository oder einem GitHub-Repository speichern, um Azure Continuous Delivery zu verwenden. Weitere Informationen finden Sie unter [Konfigurieren von Continuous Delivery für die Linux-Web-App](https://blogs.msdn.microsoft.com/devops/2017/05/10/use-azure-portal-to-setup-continuous-delivery-for-web-app-on-linux/) (in englischer Sprache).

- Verwenden Sie die [API für die ZIP-Bereitstellung](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file):Stellen Sie zur Verwendung dieser API [eine SSH-Verbindung mit Ihrer Web-App](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-ssh-support#making-a-client-connection) her, und navigieren Sie zu dem Ordner, in dem der Code bereitgestellt werden soll. Führen Sie den folgenden Code aus:

   ```bash
   curl -X POST -u <user> --data-binary @<zipfile> https://{your-sitename}.scm.azurewebsites.net/api/zipdeploy
   ```

   Wenn eine Fehlermeldung angezeigt wird, dass der Befehl `curl` nicht gefunden wird, müssen Sie curl unbedingt mithilfe von `apt-get install curl` installieren, bevor Sie den Befehl `curl` ausführen.

## <a name="language-support"></a>Sprachunterstützung

**Ich möchte in meiner Node.js-Anwendung Websockets verwenden. Muss ich bestimmte Einstellungen oder Konfigurationen festlegen?**

Ja, deaktivieren Sie in Ihrem serverseitigen Node.js-Code `perMessageDeflate`. Verwenden Sie beispielsweise den folgenden Code, wenn Sie socket.io nutzen:

```nodejs
var io = require('socket.io')(server,{
  perMessageDeflate :false
});
```

**Unterstützen Sie nicht kompilierte .NET Core-Apps?**

Ja.

**Unterstützen Sie Composer als Abhängigkeits-Manager für PHP-Apps?**

Ja. Bei einer Git-Bereitstellung erkennt Kudu, dass Sie eine PHP-Anwendung bereitstellen (weil eine composer.lock-Datei existiert), und löst eine Composerinstallation aus.

## <a name="custom-containers"></a>Benutzerdefinierte Container

**Ich verwende meinen eigenen benutzerdefinierten Container. Ich möchte, dass die Plattform eine SMB-Freigabe im Verzeichnis `/home/` bereitstellt.**

Legen Sie die App-Einstellung `WEBSITES_ENABLE_APP_SERVICE_STORAGE` auf *TRUE* fest. Bitte beachten Sie, dass das Containerneustarts verursacht, wenn Änderungen am Plattformspeicher vorgenommen werden.

>[!NOTE]
>Wenn die Einstellung `WEBSITES_ENABLE_APP_SERVICE_STORAGE` nicht angegeben oder auf *FALSE* festgelegt ist, wird das Verzeichnis `/home/` nicht über Skalierungsinstanzen freigegeben. Außerdem werden dort geschriebene Dateien nicht über Neustarts hinweg beibehalten.

**Mein benutzerdefinierter Container benötigt für den Start sehr lange, und die Plattform startet den Container neu, bevor er den Startvorgang abgeschlossen hat.**

Sie können den Zeitraum konfigurieren, den die Plattform vor dem Neustarten Ihres Containers wartet. Legen Sie hierzu die App-Einstellung `WEBSITES_CONTAINER_START_TIME_LIMIT` auf den gewünschten Wert fest. Der Standardwert ist 230 Sekunden, und der Maximalwert ist 1800 Sekunden.

**Welches Format hat die Server-URL der privaten Registrierung?**

Geben Sie die vollständige Registrierungs-URL samt `http://` oder `https://` ein.

**Welches Format hat der Imagename in der Option zur privaten Registrierung?**

Fügen Sie den vollständigen Namen hinzu, einschließlich der URL der privaten Registrierung (Beispiel: myacr.azurecr.io/dotnet:latest). Namen von Images, die einen benutzerdefinierten Port verwenden, [können nicht über das Portal eingegeben werden](https://feedback.azure.com/forums/169385-web-apps/suggestions/31304650). Verwenden Sie das [Befehlszeilentool](https://docs.microsoft.com/cli/azure/webapp/config/container?view=azure-cli-latest#az_webapp_config_container_set) `az`, um `docker-custom-image-name` festzulegen.

**Kann ich mehr als einen Port in meinem benutzerdefinierten Containerimage verfügbar machen?**

Momentan kann nur ein Port verfügbar gemacht werden.

**Kann ich meinen eigenen Speicher verwenden?**

Die Verwendung eigener Speicher wird derzeit nicht unterstützt.

**Warum kann ich über die SCM-Website das Dateisystem meines benutzerdefinierten Containers nicht durchsuchen und keine Prozesse ausführen?**

Die SCM-Website wird in einem separaten Container ausgeführt. Sie können das Dateisystem oder aktive Prozesse des App-Containers nicht überprüfen.

**Mein benutzerdefinierter Container lauscht auf einen anderen Port als Port 80. Wie kann ich meine App so konfigurieren, dass Anforderungen an diesen Port weitergeleitet werden?**

Die Porterkennung erfolgt automatisch. Sie können auch eine Anwendungseinstellung mit dem Namen *WEBSITES_PORT* und für diese den Wert der erwarteten Portnummer angeben. Zuvor wurde von der Plattform die App-Einstellung *PORT* verwendet. Es ist geplant, diese App-Einstellung einzustellen und ausschließlich *WEBSITES_PORT* zu verwenden.

**Muss ich HTTPS in meinem benutzerdefinierten Container implementieren?**

Nein. Die Plattform handhabt die HTTPS-Beendigung an den freigegebenen Front-Ends.

## <a name="pricing-and-sla"></a>Preise und SLA

**Wie erfolgt die Abrechnung für den nun allgemein verfügbaren Dienst?**

Ihnen wird der normale Azure App Service-Preis für die Stunden berechnet, die Ihre App ausgeführt wird.

## <a name="other-questions"></a>Weitere Fragen

**Welche Zeichen werden in den Namen von Anwendungseinstellungen unterstützt?**

Sie können nur Buchstaben (A-Z, a-Z), Zahlen (0-9) und Unterstrich (_) für Anwendungseinstellungen verwenden.

**Wo kann ich neue Funktionen beantragen?**

Sie können Ihre Idee im [Web-Apps-Feedbackforum](https://aka.ms/webapps-uservoice) einreichen. Fügen Sie im Titel Ihrer Idee „[Linux]“ hinzu.

## <a name="next-steps"></a>Nächste Schritte

- [Was ist Azure App Service unter Linux?](app-service-linux-intro.md)
- [Einrichten von Stagingumgebungen in Azure App Service](../../app-service/web-sites-staged-publishing.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Continuous Deployment mit Web-App für Container](./app-service-linux-ci-cd.md)
