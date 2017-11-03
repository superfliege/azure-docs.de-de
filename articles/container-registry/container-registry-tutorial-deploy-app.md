---
title: "Azure Container Registry-Tutorial: Bereitstellen einer Web-App über Azure Container Registry"
description: Stellen Sie eine Linux-basierte Web-App unter Verwendung eines Containerimages aus einer georeplizierten Azure-Containerregistrierung bereit. Dieses Tutorial ist der zweite Teil einer dreiteiligen Reihe.
services: container-registry
documentationcenter: 
author: mmacy
manager: timlt
editor: neilpeterson
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
ms.openlocfilehash: 749a536fea50d0f45b021cfeaffe8e493faa9b3f
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="deploy-web-app-from-azure-container-registry"></a>Bereitstellen einer Web-App über Azure Container Registry

Dies ist der zweite Teil eines dreiteiligen Tutorials. In [Teil 1](container-registry-tutorial-prepare-registry.md) wurde eine private georeplizierte Containerregistrierung erstellt. Außerdem wurde ein quellcodebasiertes Containerimage erstellt und mithilfe von Push an die Registrierung übertragen. In diesem Artikel stellen Sie den Container in zwei Web-App-Instanzen in zwei verschiedenen Azure-Regionen bereit, um von der Netzwerknähe der georeplizierten Registrierung zu profitieren.

Dieser zweite Teil der Tutorialreihe umfasst Folgendes:

> [!div class="checklist"]
> * Bereitstellen eines Containerimages in zwei Instanzen vom Typ *Web-App für Container*
> * Überprüfen der bereitgestellten Anwendung

Wenn Sie noch keine georeplizierte Registrierung erstellt und noch kein Image der containerbasierten Beispielanwendung mithilfe von Push an die Registrierung übertragen haben, kehren Sie zum vorherigen Tutorial der Reihe zurück: [Prepare a geo-replicated Azure container registry](container-registry-tutorial-prepare-registry.md) (Vorbereiten einer georeplizierten Azure-Containerregistrierung).

Im nächsten Teil der Reihe aktualisieren Sie die Anwendung und übertragen anschließend mithilfe von Push ein neues Containerimage an die Registrierung. Danach navigieren Sie zu den beiden ausgeführten Web-App-Instanzen, um zu sehen, dass die Änderung jeweils automatisch übernommen wurde. Dies zeigt die Georeplikation und Webhooks von Azure Container Registry in Aktion.

## <a name="automatic-deployment-to-web-app-for-containers"></a>Automatische Bereitstellung in Web-App für Container

Azure Container Registry unterstützt die direkte Bereitstellung containerbasierter Anwendungen in [Web-Apps für Container](../app-service/containers/index.yml). In diesem Tutorial verwenden Sie das Azure-Portal, um das im vorherigen Tutorial erstellte Containerimage in zwei Web-App-Plänen in unterschiedlichen Azure-Regionen bereitzustellen.

Wenn Sie eine Web-App auf der Grundlage eines Containerimages in Ihrer Registrierung bereitstellen und in der gleichen Region über eine georeplizierte Registrierung verfügen, erstellt Azure Container Registry automatisch einen [Webhook](container-registry-webhook.md) für die Imagebereitstellung. Wenn Sie ein neues Image mithilfe von Push an Ihr Containerrepository übertragen, übernimmt der Webhook die Änderung und stellt automatisch das neue Containerimage für Ihre Web-App bereit.

## <a name="deploy-a-web-app-for-containers-instance"></a>Bereitstellen einer Instanz von „Web-App für Container“

In diesem Schritt erstellen Sie eine Instanz von „Web-App für Container“ in der Region *USA, Westen*.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu der Registrierung, die Sie im vorherigen Tutorial erstellt haben.

Klicken Sie auf **Repositorys** > **acr-helloworld**, klicken Sie unter **Tags** mit der rechten Maustaste auf das Tag **v1**, und klicken Sie anschließend auf **In App Service bereitstellen**.

![„In App Service bereitstellen“ im Azure-Portal][deploy-app-portal-01]

Geben Sie unter **Web-App in Linux (Vorschau)** die folgenden Werte für die einzelnen Einstellungen an:

| Einstellung | Wert |
|---|---|
| **Standortname** | Ein global eindeutiger Name für die Web-App. In diesem Beispiel verwenden wir das Format `<acrName>-westus`, um ganz einfach erkennen zu können, aus welcher Registrierung und Region die Web-App bereitgestellt wird. |
| **Ressourcengruppe** | **Vorhandene verwenden** > `myResourceGroup` |
| **App Service-Plan/Standort** | Erstellen Sie einen neuen Plan namens `plan-westus` in der Region **USA, Westen**. |
| **Image** | `acr-helloworld:v1`

Klicken Sie auf **Erstellen**, um die Web-App in der Region *USA, Westen* bereitzustellen.

![Konfiguration der Web-App unter Linux im Azure-Portal][deploy-app-portal-02]

## <a name="view-the-deployed-web-app"></a>Anzeigen der bereitgestellten Web-App

Nach Abschluss der Bereitstellung können Sie die ausgeführte Anwendung anzeigen, indem Sie in Ihrem Browser zur URL der Anwendung navigieren.

Klicken Sie im Portal auf **App Services** und anschließend auf die Web-App, die Sie im vorherigen Schritt bereitgestellt haben. In diesem Beispiel hat die Web-App den Namen *uniqueregistryname-westus*.

Klicken Sie rechts oben in der **App Service-Übersicht** auf die als Link dargestellte URL der Web-App, um die ausgeführte Anwendung in Ihrem Browser anzuzeigen.

![Konfiguration der Web-App unter Linux im Azure-Portal][deploy-app-portal-04]

Nachdem das Docker-Image über Ihre georeplizierte Containerregistrierung bereitgestellt wurde, wird ein Image angezeigt, das die Azure-Region darstellt, in der die Containerregistrierung gehostet wird.

![Bereitgestellte Webanwendung, angezeigt in einem Browser][deployed-app-westus]

## <a name="deploy-second-web-app-for-containers-instance"></a>Bereitstellen der zweiten Instanz von „Web-App für Container“

Gehen Sie wie im vorherigen Abschnitt beschrieben vor, um eine zweite Web-App in der Region *USA, Osten* bereitzustellen. Geben Sie unter **Web-App unter Linux (Vorschau)** die folgenden Werte an:

| Einstellung | Wert |
|---|---|
| **Standortname** | Ein global eindeutiger Name für die Web-App. In diesem Beispiel verwenden wir das Format `<acrName>-eastus`, um ganz einfach erkennen zu können, aus welcher Registrierung und Region die Web-App bereitgestellt wird. |
| **Ressourcengruppe** | **Vorhandene verwenden** > `myResourceGroup` |
| **App Service-Plan/Standort** | Erstellen Sie einen neuen Plan namens `plan-eastus` in der Region **USA, Osten**. |
| **Image** | `acr-helloworld:v1`

Klicken Sie auf **Erstellen**, um die Web-App in der Region *USA, Osten* bereitzustellen.

![Konfiguration der Web-App unter Linux im Azure-Portal][deploy-app-portal-06]

## <a name="view-the-deployed-web-app"></a>Anzeigen der bereitgestellten Web-App

Auch hier können Sie die ausgeführte Anwendung anzeigen, indem Sie in Ihrem Browser zur URL der Anwendung navigieren.

Klicken Sie im Portal auf **App Services** und anschließend auf die Web-App, die Sie im vorherigen Schritt bereitgestellt haben. In diesem Beispiel hat die Web-App den Namen *uniqueregistryname-eastus*.

Klicken Sie rechts oben in der **App Service-Übersicht** auf die als Link dargestellte URL der Web-App, um die ausgeführte Anwendung in Ihrem Browser anzuzeigen.

![Konfiguration der Web-App unter Linux im Azure-Portal][deploy-app-portal-07]

Nachdem das Docker-Image über Ihre georeplizierte Containerregistrierung bereitgestellt wurde, wird ein Image angezeigt, das die Azure-Region darstellt, in der die Containerregistrierung gehostet wird.

![Bereitgestellte Webanwendung, angezeigt in einem Browser][deployed-app-eastus]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie zwei Instanzen von „Web-App für Container“ über eine georeplizierte Azure-Containerregistrierung bereitgestellt. Mithilfe der Schritte in diesem Tutorial haben Sie folgende Aktionen durchgeführt:

> [!div class="checklist"]
> * Bereitstellen eines Containerimages in zwei Instanzen vom Typ *Web-App für Container*
> * Überprüfen der bereitgestellten Anwendung

Im nächsten Tutorial aktualisieren Sie die Anwendung, stellen ein neues Containerimage in der Containerregistrierung bereit und vergewissern sich anschließend, dass die in beiden Regionen ausgeführten Web-Apps automatisch aktualisiert wurden.

> [!div class="nextstepaction"]
> [Push an updated image to regional deployments](./container-registry-tutorial-deploy-update.md) (Übertragen eines aktualisierten Images an regionale Bereitstellungen mithilfe von Push)

<!-- IMAGES -->
[deploy-app-portal-01]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-01.png
[deploy-app-portal-02]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-02.png
[deploy-app-portal-03]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-03.png
[deploy-app-portal-04]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-04.png
[deploy-app-portal-05]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-05.png
[deploy-app-portal-06]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-06.png
[deploy-app-portal-07]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-07.png
[deployed-app-westus]: ./media/container-registry-tutorial-deploy-app/deployed-app-westus.png
[deployed-app-eastus]: ./media/container-registry-tutorial-deploy-app/deployed-app-eastus.png