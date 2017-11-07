---
title: "Tutorial zu Azure Container Registry – Aktualisiertes Image per Push für regionale Bereitstellungen übertragen"
description: "Übertragen Sie ein geändertes Docker-Image per Push zu Ihrer georeplizierten Azure Container Registry. Betrachten Sie dann die automatische Bereitstellung der Änderungen für Web-Apps, die in mehreren Regionen ausgeführt werden. Dieses Tutorial ist der dritte Teil einer dreiteiligen Reihe."
services: container-registry
documentationcenter: 
author: mmacy
manager: timlt
editor: mmacy
tags: acr, azure-container-registry, geo-replication
keywords: Docker, Container, Registrierung, Azure
ms.service: container-registry
ms.devlang: 
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 76e6e1b826f37bfea7a8463808566191753e4f2d
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="push-an-updated-image-to-regional-deployments"></a>Pushübertragung eines aktualisierten Images an regionale Bereitstellungen

Dies ist der dritte Teil eines dreiteiligen Tutorials. Im [vorherigen Tutorial](container-registry-tutorial-deploy-app.md) war die Georeplikation für zwei verschiedene regionale Web-App-Bereitstellungen konfiguriert. In diesem Tutorial ändern Sie zuerst die Anwendung, erstellen dann ein neues Containerimage und übertragen es per Push an Ihre georeplizierte Registrierung. Abschließend überprüfen Sie die Änderung, die automatisch von Webhooks der Azure Container Registry bereitgestellt wird, in beiden Web-App-Instanzen.

Dieser letzte Teil der Tutorialreihe umfasst Folgendes:

> [!div class="checklist"]
> * Ändern des HTML-Codes der Webanwendung
> * Erstellen und Kennzeichnen des Docker-Images
> * Übertragen der Änderung zu Azure Container Registry per Push
> * Anzeigen der aktualisierten App in zwei verschiedenen Regionen

Wenn Sie die beiden regionalen Bereitstellungen für *Web-App für Container* noch nicht konfiguriert haben, kehren Sie zum vorherigen Tutorial in dieser Reihe zurück: [Bereitstellen einer Web-App über Azure Container Registry](container-registry-tutorial-deploy-app.md).

## <a name="modify-the-web-application"></a>Ändern der Webanwendung

Nehmen Sie in diesem Schritt eine Änderung an der Webanwendung vor, die leicht zu erkennen ist, nachdem Sie das aktualisierte Containerimage per Push zu Azure Container Registry übertragen haben.

Suchen Sie die Datei `AcrHelloworld/Views/Home/Index.cshtml` in der Anwendungsquelle, die Sie in einem vorherigen Tutorial [von GitHub geklont](container-registry-tutorial-prepare-registry.md#get-application-code) haben, und öffnen Sie sie in Ihrem bevorzugten Text-Editor. Fügen Sie über der `<img>`-Zeile die folgende Zeile hinzu:

```html
<h1>MODIFIED</h1>
```

Ihre geänderte `Index.cshtml`-Datei sollte in etwa wie folgt aussehen:

```html
@{
    ViewData["Title"] = "Azure Container Registry :: Geo-replication";
}
<h1>MODIFIED</h1>
<img width="700" src="~/images/@ViewData["MAPIMAGE"]" />
<ul>
<li>Registry URL: @ViewData["REGISTRYURL"]</li>
<li>Registry IP: @ViewData["REGISTRYIP"]</li>
<li>HostEntry: @ViewData["HOSTENTRY"]</li>
<li>Region: @ViewData["REGION"]</li>
<li>Map: @ViewData["MAPIMAGE"]</li>
</ul>
```

## <a name="rebuild-the-image"></a>Neuerstellen des Images

Nachdem Sie die Webanwendung aktualisiert haben, erstellen Sie deren Containerimage neu. Verwenden Sie wie zuvor den vollqualifizierten Imagenamen, einschließlich der Anmeldeserver-URL für das Tag:

```bash
docker build . -f ./AcrHelloworld/Dockerfile -t <acrName>.azurecr.io/acr-helloworld:v1
```

## <a name="run-the-container-locally"></a>Lokales Ausführen des Containers

Führen Sie vor der Bereitstellung in Azure Container Registry das Image lokal aus, um sicherzustellen, dass die Erstellung erfolgreich war.

```bash
docker run -d -p 8080:80 <acrName>.azurecr.io/acr-helloworld:v1
```

Navigieren Sie in Ihrem Webbrowser zu „http://localhost:8080“, um sicherzustellen, dass der Container ausgeführt und die Änderung angezeigt wird.

![LOKALES CONTAINERIMAGE][local-container-01]

## <a name="push-image-to-azure-container-registry"></a>Übertragen des Images zu Azure Container Registry mithilfe von Push

Übertragen Sie jetzt das aktualisierte *acr-helloworld*-Containerimage per Push zu Ihrer georeplizierten Registrierung. Hier führen Sie einen einzelnen `docker push`-Befehl aus, um das aktualisierte Image für die Registrierungsreplikate in den Regionen *USA, Westen* und *USA, Osten* bereitzustellen.

```bash
docker push <acrName>.azurecr.io/acr-helloworld:v1
```

## <a name="view-the-webhook-logs"></a>Anzeigen der Webhook-Protokolle

Während das Image repliziert wird, können Sie sehen, wie die Azure Container Registry-Webhooks ausgelöst werden.

Um die regionalen Webhooks anzuzeigen, die während der Bereitstellung des Containers für *Web-Apps für Container* in einem vorherigen Tutorial erstellt wurden, navigieren Sie zu Ihrer Containerregistrierung im Azure-Portal, und wählen Sie dann **Webhooks** unter **DIENSTE** aus.

![Webhooks der Containerregistrierung im Azure-Portal][tutorial-portal-01]

Wählen Sie die einzelnen Webhooks aus, um den Verlauf der zugehörigen Aufrufe und Antworten anzuzeigen. Daraufhin sollte eine Zeile für die **Push**-Aktion in den Protokollen der beiden Webhooks angezeigt werden. Hier zeigt das Protokoll für den Webhook in der Region *USA, Westen* an, dass die **Push**-Aktion im vorherigen Schritt durch `docker push` ausgelöst wurde:

![Webhook-Protokoll der Containerregistrierung im Azure-Portal (USA, Westen)][tutorial-portal-02]

## <a name="view-the-updated-web-app"></a>Anzeigen der aktualisierten Web-App

Die Webhooks benachrichtigen die Web-Apps, dass ein neues Image per Push an die Registrierung übertragen wurde, wodurch der aktualisierte Container automatisch für die beiden regionalen Web-Apps bereitgestellt wird.

Stellen Sie sicher, dass die Anwendung in beiden Bereitstellungen aktualisiert wurde, indem Sie in Ihrem Webbrowser zu beiden regionalen Web-App-Bereitstellungen navigieren. Zur Erinnerung: Sie können die URL für die bereitgestellte Web-App in der oberen rechten Ecke der einzelnen Registerkarten für die App Service-Übersicht finden.

![App Service-Übersicht im Azure-Portal.][tutorial-portal-03]

Wählen Sie den Link in der App Service-Übersicht aus, um die aktualisierte Anwendung anzuzeigen. Hier folgt eine Beispielansicht für die Ausführung der App in *USA, Westen*:

![Browseransicht zur Ausführung der geänderten Web-App in der Region USA (Westen)][deployed-app-westus-modified]

Stellen Sie sicher, dass das aktualisierte Containerimage auch in der Region *USA, Osten* bereitgestellt wurde, indem Sie es im Browser anzeigen.

![Browseransicht zur Ausführung der geänderten Web-App in der Region USA (Osten)][deployed-app-eastus-modified]

Mit einem einzelnen `docker push` haben Sie beide regionalen Web-App-Bereitstellungen aktualisiert, und Azure Container Registry hat die Containerimages über netzwerknahe Repositorys bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Version des Containers der Webanwendung aktualisiert und die neue Version per Push an Ihre georeplizierte Registrierung übertragen. App Services wurden von Webhooks in Azure Container Registry benachrichtigt, wodurch ein lokaler Pullvorgang von den replizierten Registrierungen ausgelöst wurde.

In diesem letzten Teil der Tutorialreihe haben Sie Folgendes erreicht:

> [!div class="checklist"]
> * HTML-Code der Webanwendung aktualisiert
> * Docker-Image erstellt und gekennzeichnet
> * Änderung per Push zu Azure Container Registry übertragen
> * Aktualisierte App in zwei verschiedenen Regionen angezeigt

<!-- IMAGES -->
[deployed-app-eastus-modified]: ./media/container-registry-tutorial-deploy-update/deployed-app-eastus-modified.png
[deployed-app-westus-modified]: ./media/container-registry-tutorial-deploy-update/deployed-app-westus-modified.png
[local-container-01]: ./media/container-registry-tutorial-deploy-update/local-container-01.png
[tutorial-portal-01]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-01.png
[tutorial-portal-02]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-02.png
[tutorial-portal-03]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-03.png