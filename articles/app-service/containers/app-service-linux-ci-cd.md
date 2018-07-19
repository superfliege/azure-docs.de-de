---
title: Continuous Deployment aus einer Docker-Containerregistrierung mit Web-App für Container – Azure | Microsoft-Dokumentation
description: 'Gewusst wie: Einrichten von Continuous Deployment aus einer Docker-Containerregistrierung in Web-App für Container'
keywords: Azure App Service, Linux, Docker, ACR, OSS
services: app-service
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2018
ms.author: msangapu
ms.openlocfilehash: 0f2d4626308eed376b71f1b3df2f9e43f1b2a4f7
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130964"
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

Notieren Sie sich die Webhook-URL. Sie benötigen sie im nächsten Abschnitt.
`https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist.

Sie können `publishingusername` und `publishingpwd` durch Herunterladen des Web-App-Veröffentlichungsprofils im Azure-Portal abrufen.

![Screenshot zum Hinzufügen von Webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="add-a-webhook"></a>Hinzufügen eines Webhooks

Führen Sie zum Hinzufügen eines Webhooks die Schritte in den folgenden Anleitungen aus:

- [Azure Container Registry](../../container-registry/container-registry-webhook.md) mithilfe der Webhook-URL
- [Webhooks für Docker Hub](https://docs.docker.com/docker-hub/webhooks/)

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure App Service unter Linux](./app-service-linux-intro.md)
* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
* [Erstellen einer .NET Core-Web-App in App Service unter Linux](quickstart-dotnetcore.md)
* [Erstellen einer Ruby-App in App Service unter Linux](quickstart-ruby.md)
* [Bereitstellen einer Docker/Go-Web-App über die Web-App für Container](quickstart-docker-go.md)
* [Häufig gestellte Fragen (FAQ) zu Azure App Service unter Linux](./app-service-linux-faq.md)
* [Verwalten von Web-App für Container mithilfe der Azure CLI](./app-service-linux-cli.md)
