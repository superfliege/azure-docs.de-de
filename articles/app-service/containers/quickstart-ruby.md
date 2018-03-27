---
title: Erstellen einer Ruby-App und Bereitstellen dieser in App Service unter Linux | Microsoft Docs
description: Erfahren Sie, wie Sie Ruby-Apps mit App Service unter Linux erstellen.
keywords: Azure App Service, Linux, OSS, Ruby
services: app-service
documentationcenter: ''
author: SyntaxC4
manager: cfowler
editor: ''
ms.assetid: 6d00c73c-13cb-446f-8926-923db4101afa
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/10/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 6668f02bb7ac9588e1bb11b3848d0a3e25cbed67
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="create-a-ruby-app-in-app-service-on-linux"></a>Erstellen einer Ruby-App in App Service unter Linux

[App Service unter Linux](app-service-linux-intro.md) bietet einen hochgradig skalierbaren Webhostingdienst mit Self-Patching. In dieser Schnellstartanleitung erfahren Sie, wie Sie eine grundlegende Ruby on Rails-Anwendung erstellen und in Azure unter als Web-App unter Linux bereitstellen.

![Hello-world](./media/quickstart-ruby/hello-world-updated.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* <a href="https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller" target="_blank">Installieren von Ruby 2.4.1 oder höher</a>
* <a href="https://git-scm.com/" target="_blank">Installation von Git</a>

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Führen Sie in einem Terminalfenster den folgenden Befehl aus, um das Beispiel-App-Repository auf Ihren lokalen Computer zu klonen:

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

## <a name="run-the-application-locally"></a>Lokales Ausführen der Anwendung

Führen Sie den Rails-Server aus, damit die Anwendung funktioniert. Wechseln Sie zum Verzeichnis *hello-world*, und starten Sie den Server mit dem Befehl `rails server`.

```bash
cd hello-world\bin
rails server
```

Navigieren Sie in Ihrem Webbrowser zu `http://localhost:3000`, um die App lokal zu testen.

![Hello-world](./media/quickstart-ruby/hello-world.png)

## <a name="modify-app-to-display-welcome-message"></a>Ändern der App zum Anzeigen einer Begrüßungsnachricht

Ändern Sie die Anwendung, sodass diese eine Begrüßungsnachricht anzeigt. Sie müssen zuerst eine Route einrichten, indem Sie die Datei *~/workspace/ruby-docs-hello-world/config/routes.rb* so ändern, dass sie eine Route mit dem Namen `hello` enthält.

  ```ruby
  Rails.application.routes.draw do
      #For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html
      root 'application#hello'
  end
  ```

Ändern Sie den Controller für die Anwendung, sodass die Nachricht als HTML an den Browser zurückgegeben wird. 

Öffnen Sie *~/workspace/hello-world/app/controllers/application_controller.rb* zur Bearbeitung. Ändern Sie die `ApplicationController`-Klasse wie im folgenden Codebeispiel gezeigt:

  ```ruby
  class ApplicationController > ActionController :: base
    protect_from_forgery with: :exception
    def hello
      render html: "Hello, world from Azure Web App on Linux!"
    end
  end
  ```

Ihre App ist jetzt konfiguriert. Navigieren Sie in Ihrem Webbrowser zu `http://localhost:3000`, um die Stamm-Startseite zu bestätigen.

![Hallo Welt (konfiguriert)](./media/quickstart-ruby/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app"></a>Erstellen einer Web-App

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-ruby-linux-no-h.md)] 

Browsen Sie zu der Website, um Ihre neu erstellte Web-App mit integriertem Image anzuzeigen. Ersetzen Sie _&lt;App-Name>_ durch Ihren Web-App-Namen.

```bash
http://<app_name>.azurewebsites.net
```

Ihre neue Web-App sollte nun wie folgt aussehen:

![Begrüßungsseite](./media/quickstart-ruby/splash-page.png)

## <a name="deploy-your-application"></a>Bereitstellen der Anwendung

Führen Sie die folgenden Befehle aus, um die lokale Anwendung auf Ihrer Azure-Website bereitzustellen:

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

Vergewissern Sie sich, dass die Remotebereitstellungsvorgänge erfolgreich waren. Der Befehl erzeugt eine Ausgabe ähnlich dem folgenden Text:

```bash
remote: Using sass-rails 5.0.6
remote: Updating files in vendor/cache
remote: Bundle gems are installed into ./vendor/bundle
remote: Updating files in vendor/cache
remote: ~site/repository
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
  579ccb....2ca5f31  master -> master
myuser@ubuntu1234:~workspace/<app name>$
```

Starten Sie Ihre Web-App nach Abschluss der Bereitstellung neu, damit die Bereitstellung wirksam wird. Verwenden Sie dazu den Befehl [`az webapp restart`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_restart), wie hier gezeigt:

```azurecli-interactive
az webapp restart --name <app name> --resource-group myResourceGroup
```

Navigieren Sie zu Ihrer Website, und überprüfen Sie die Ergebnisse.

```bash
http://<app name>.azurewebsites.net
```

![Aktualisierte Web-App](./media/quickstart-ruby/hello-world-updated.png)

> [!NOTE]
> Wenn Sie während des Neustarts der App versuchen, zur Website zu browsen, wird der HTTP-Statuscode `Error 503 Server unavailable` ausgegeben. Der vollständige Neustart kann einige Minuten in Anspruch nehmen.
>

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Ruby on Rails mit MySQL](tutorial-ruby-mysql-app.md)
