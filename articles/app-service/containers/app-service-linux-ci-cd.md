---
title: Continuous Deployment aus einer Docker-Containerregistrierung mit Web-App für Container – Azure | Microsoft-Dokumentation
description: 'Gewusst wie: Einrichten von Continuous Deployment aus einer Docker-Containerregistrierung in Web-App für Container'
keywords: Azure App Service, Linux, Docker, ACR, OSS
services: app-service
documentationcenter: ''
author: ahmedelnably
manager: cfowler
editor: ''
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: aelnably;msangapu
ms.openlocfilehash: ac35dbd041de50ab8aae1a0fb4c00fe3917a7297
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="continuous-deployment-with-web-app-for-containers"></a>Continuous Deployment mit Web-App für Container

In diesem Tutorial konfigurieren Sie Continuous Deployment für ein benutzerdefiniertes Containerimage aus verwalteten [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)-Repositorys oder aus [Docker Hub](https://hub.docker.com).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="enable-the-continuous-deployment-feature"></a>Aktivieren des Continuous Deployment-Features

Aktivieren Sie das Feature Continuous Deployment mit der [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), und führen Sie den folgenden Befehl aus:

```azurecli-interactive
az webapp deployment container config --name name --resource-group myResourceGroup --enable-cd true
```

Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf der linken Seite auf die Option **App Service**.

Klicken Sie auf den Namen der App, für die Sie Continuous Deployment mit Docker Hub konfigurieren möchten.

Wählen Sie auf der Seite **Docker-Container** die Option **Ein** aus, und klicken Sie dann auf **Speichern**, um Continuous Deployment zu aktivieren.

![Screenshot der App-Einstellung](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="prepare-the-webhook-url"></a>Vorbereiten der Webhook-URL

Rufen Sie die Webhook-URL mit der [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) ab, und führen Sie den folgenden Befehl aus:

```azurecli-interactive
az webapp deployment container show-cd-url --name sname1 --resource-group rgname
```

Für die Webhook-URL benötigen Sie den folgenden Endpunkt: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

Sie können `publishingusername` und `publishingpwd` durch Herunterladen des Web-App-Veröffentlichungsprofils im Azure-Portal abrufen.

![Screenshot zum Hinzufügen von Webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="add-a-webhook"></a>Hinzufügen eines Webhooks

### <a name="azure-container-registry"></a>Azure-Containerregistrierung

1. Klicken Sie auf der Registrierungsportalseite auf **Webhooks**.
2. Wählen Sie zum Erstellen eines neuen Webhooks **Hinzufügen** aus. 
3. Geben Sie dem Webhook im Bereich **Webhook erstellen** einen Namen. Geben Sie für den URI-Webhook die URL an, die Sie im vorherigen Abschnitt abgerufen haben.

Legen Sie den Bereich auf das Repository fest, das Ihr Containerimage enthält.

![Screenshot des Webhooks](./media/app-service-webapp-service-linux-ci-cd/step3ACRWebhook-1.png)

Wenn Sie das Image aktualisieren, wird die Web-App automatisch mit dem neuen Image aktualisiert.

### <a name="docker-hub"></a>Docker Hub

Klicken Sie auf der Docker Hub-Seite auf **Webhooks** und anschließend auf **CREATE A WEBHOOK** (WEBHOOK ERSTELLEN).

![Screenshot zum Hinzufügen von Webhook 1](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

Geben Sie für die Webhook-URL die URL an, die Sie zuvor abgerufen haben.

![Screenshot zum Hinzufügen von Webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

Wenn Sie das Image aktualisieren, wird die Web-App automatisch mit dem neuen Image aktualisiert.

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure App Service unter Linux](./app-service-linux-intro.md)
* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
* [Erstellen einer .NET Core-Web-App in App Service unter Linux](quickstart-dotnetcore.md)
* [Erstellen einer Ruby-App in App Service unter Linux](quickstart-ruby.md)
* [Bereitstellen einer Docker/Go-Web-App über die Web-App für Container](quickstart-docker-go.md)
* [Häufig gestellte Fragen (FAQ) zu Azure App Service unter Linux](./app-service-linux-faq.md)
* [Verwalten von Web-App für Container mithilfe der Azure CLI](./app-service-linux-cli.md)
