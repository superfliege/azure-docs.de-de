---
title: "Azure App Service-Web-App für Container – FAQs | Microsoft-Dokumentation"
description: "Azure App Service-Web-App für Container – FAQs"
keywords: "Azure App Service, Web-App, häufig gestellte Fragen, Linux, OSS"
services: app-service
documentationCenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: aelnably;wesmc
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: b783f7e8cfef991e7028ba4b4c7b1d0935397580
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---
# <a name="azure-app-service-web-app-for-containers-faq"></a>Azure App Service-Web-App für Container – FAQs

Mit der Veröffentlichung von Web-App für Container erweitern wir unsere Plattform um Funktionen und Verbesserungen. Dieser Artikel enthält Antworten auf Fragen, die uns kürzlich von Kunden gestellt wurden.

Wenn Sie eine Frage haben, kommentieren Sie den Artikel, und wir werden so bald wie möglich antworten.

## <a name="built-in-images"></a>Integrierte Images

 **Ich möchte die integrierten Docker-Container nutzen, die von der Plattform bereitgestellt werden. Wo finde ich diese Dateien?**

Sie finden alle Docker-Dateien auf [GitHub](https://github.com/azure-app-service). Sie finden alle Docker-Container auf [Docker Hub](https://hub.docker.com/u/appsvc/).

**Welche Werte sind beim Konfigurieren des Laufzeitstapels im Abschnitt „Startdatei“ anzugeben?**

Für Node.js geben Sie die PM2-Konfigurationsdatei oder Ihre Skriptdatei an. Für .NET Core geben Sie den Namen der kompilierten DLL-Datei an. Für Ruby können Sie das Ruby-Skript angeben, mit dem Ihre App initialisiert werden soll.

## <a name="management"></a>Verwaltung

**Was geschieht, wenn ich auf die Schaltfläche „Neu starten“ im Azure-Portal klicke?**

Diese Aktion ist mit dem Docker-Neustart identisch.

**Kann ich über Secure Shell (SSH) eine Verbindung mit dem virtuellen Computer des App-Containers herstellen?**

Ja, Sie können dazu die Website für die Quellcodeverwaltung (Source Control Management, SCM) verwenden.

**Wie kann ich einen Linux-App Service-Plan über ein SDK oder eine Azure Resource Manager-Vorlage erstellen?**

Sie müssen das Feld **reserviert** des App-Diensts auf *true* festlegen.

## <a name="continuous-integration-and-deployment"></a>Continuous Integration und Continuous Deployment

**Meine Web-App verwendet nach dem Aktualisieren des Images auf Docker Hub noch ein altes Docker-Containerimage. Unterstützen Sie Continuous Integration/Deployment von benutzerdefinierten Containern?**

**A:** Informationen zum Einrichten von Continuous Integration/Deployment für Azure Container Registry- oder Docker Hub-Images finden Sie im Artikel [Continuous Deployment mit Azure-Web-App für Container](./app-service-linux-ci-cd.md). Bei privaten Registrierungen können Sie den Container aktualisieren, indem Sie Ihre Web-App beenden und dann wieder starten. Sie können auch eine Dummyeinstellung in der Anwendung ändern oder hinzufügen, um die Aktualisierung Ihres Containers zu erzwingen.

**Werden Stagingumgebungen unterstützt?**

Ja.

**Kann ich *Web Deploy* zur Bereitstellung meiner Web-App verwenden?**

Ja, Sie müssen die App-Einstellung `WEBSITE_WEBDEPLOY_USE_SCM` auf *false* festlegen.

## <a name="language-support"></a>Sprachunterstützung

**Unterstützen Sie nicht kompilierte .NET Core-Apps?**

Ja.

**Unterstützen Sie Composer als Abhängigkeits-Manager für PHP-Apps?**

Ja. Während einer Git-Bereitstellung sollte Kudu erkennen, dass Sie eine PHP-Anwendung bereitstellen (durch das Vorhandensein einer Datei „composer.lock“), und dann eine Composer-Installation für Sie auslösen.

## <a name="custom-containers"></a>Benutzerdefinierte Container

**Ich verwende meinen eigenen benutzerdefinierten Container. Ich möchte, dass die Plattform eine SMB-Freigabe im Verzeichnis `/home/` bereitstellt.**

Dies können Sie tun, indem Sie die App-Einstellung `WEBSITES_ENABLE_APP_SERVICE_STORAGE` auf *true* festlegen oder die App-Einstellung vollständig entfernen. Denken Sie daran, dass der Container bei einer Änderung des Plattformspeichers neu gestartet wird. 

>[!NOTE]
>Wenn die Einstellung `WEBSITES_ENABLE_APP_SERVICE_STORAGE` auf *false* festgelegt ist, wird das Verzeichnis `/home/` nicht über Skalierungsinstanzen freigegeben, und dort geschriebene Dateien werden nicht über Neustarts hinweg beibehalten.

**Mein benutzerdefinierter Container benötigt für den Start sehr lange, und die Plattform startet den Container neu, bevor er den Startvorgang abgeschlossen hat.**

Sie können den Zeitraum konfigurieren, den die Plattform vor dem Neustarten Ihres Containers wartet. Legen Sie hierzu die App-Einstellung `WEBSITES_CONTAINER_START_TIME_LIMIT` auf den gewünschten Wert fest. Der Standardwert ist 230 Sekunden, und der zulässige Maximalwert ist 600 Sekunden.

**Welches Format hat die Server-URL der privaten Registrierung?**

Geben Sie die vollständige Registrierungs-URL samt `http://` oder `https://` ein.

**Welches Format hat der Imagename in der Option zur privaten Registrierung?**

Fügen Sie den vollständigen Namen hinzu, einschließlich der URL der privaten Registrierung (Beispiel: myacr.azurecr.io/dotnet:latest). Namen von Images, die einen benutzerdefinierten Port verwenden, [können nicht über das Portal eingegeben werden](https://feedback.azure.com/forums/169385-web-apps/suggestions/31304650). Verwenden Sie das [Befehlszeilentool](https://docs.microsoft.com/en-us/cli/azure/webapp/config/container?view=azure-cli-latest#az_webapp_config_container_set) `az`, um `docker-custom-image-name` festzulegen.

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

* [Was sind Azure-Web-Apps für Container?](app-service-linux-intro.md)
* [Einrichten von Stagingumgebungen in Azure App Service](../../app-service/web-sites-staged-publishing.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Continuous Deployment mit Azure-Web-Apps für Container](./app-service-linux-ci-cd.md)

