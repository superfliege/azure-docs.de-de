---
title: "Verwenden eines benutzerdefinierten Docker-Images für Web-App für Container | Microsoft Docs"
description: "Verwenden eines benutzerdefinierten Docker-Images für Web-App für Container."
keywords: Azure App Service, Web-App, Linux, Docker, Container
services: app-service
documentationcenter: 
author: SyntaxC4
manager: SyntaxC4
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 4ba53dd1239290c64907ed431d404b2d1be66c36
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2017
---
# <a name="use-a-custom-docker-image-for-web-app-for-containers"></a>Verwenden eines benutzerdefinierten Docker-Images für Web-App für Container

[Introduction to Azure Web App for Containers](app-service-linux-intro.md) (Einführung zu Azure-Web-App für Container) bietet integrierte Docker-Images unter Linux mit Unterstützung für bestimmte Versionen wie PHP 7.0 und Node.js 4.5. „Web-App für Container“ nutzt die Containertechnologie von Docker, um sowohl integrierte als auch benutzerdefinierte Images als „Platform as a Service“ zu hosten. In diesem Tutorial erfahren Sie, wie Sie ein benutzerdefiniertes Docker-Image erstellen und in Web-App für Container bereitstellen. Dieses Muster ist hilfreich, wenn die integrierten Images nicht die Sprache Ihrer Wahl verwenden oder Ihre Anwendung eine bestimmte Konfiguration erfordert, die nicht in den integrierten Images bereitgestellt wird.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* [Git](https://git-scm.com/downloads)
* Ein aktives [Azure-Abonnement](https://azure.microsoft.com/pricing/free-trial/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Docker](https://docs.docker.com/get-started/#setup)
* Ein [Docker-Hub-Konto](https://docs.docker.com/docker-id/)

[!INCLUDE [Free trial note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Führen Sie in einem Terminalfenster den folgenden Befehl aus, um das Beispiel-App-Repository auf Ihren lokalen Computer zu klonen, und navigieren Sie dann zum Verzeichnis mit dem Beispielcode.

```bash
git clone https://github.com/Azure-Samples/docker-django-webapp-linux.git --config core.autocrlf=input
cd docker-django-webapp-linux
```

## <a name="build-the-image-from-the-docker-file"></a>Erstellen des Images aus der Docker-Datei

Betrachten Sie _Dockerfile_ im Git-Repository. Diese Datei beschreibt die Python-Umgebung, die zur Ausführung Ihrer Anwendung erforderlich ist. Darüber hinaus richtet das Image einen [SSH](https://www.ssh.com/ssh/protocol/)-Server für eine sichere Kommunikation zwischen dem Container und dem Host ein.

```docker
FROM python:3.4

RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt
ADD . /code/

# ssh
ENV SSH_PASSWD "root:Docker!"
RUN apt-get update \
        && apt-get install -y --no-install-recommends dialog \
        && apt-get update \
    && apt-get install -y --no-install-recommends openssh-server \
    && echo "$SSH_PASSWD" | chpasswd 

COPY sshd_config /etc/ssh/
COPY init.sh /usr/local/bin/
    
RUN chmod u+x /usr/local/bin/init.sh
EXPOSE 8000 2222
#CMD ["python", "/code/manage.py", "runserver", "0.0.0.0:8000"]
ENTRYPOINT ["init.sh"]
```

Um das Docker-Image zu erstellen, führen Sie den `docker build`-Befehl aus, und geben Sie einen Namen (`mydockerimage`) und das Tag (`v1.0.0`) ein. Ersetzen Sie `<docker-id>` durch Ihre Docker Hub-Konto-ID.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Der Befehl erzeugt eine Ausgabe ähnlich der Folgenden:

```bash
# The output from the commands in this article has been shortened for brevity.

Sending build context to Docker daemon  5.558MB
Step 1/13 : FROM python:3.4
 ---> 9ff45ddb54e9
Step 2/13 : RUN mkdir /code
 ---> Using cache
 ---> f3f3ac01db0a
Step 3/13 : WORKDIR /code
 ---> Using cache
 ---> 38b32f15b442
.
.
.
Step 13/13 : ENTRYPOINT init.sh
 ---> Running in 5904e4c70043
 ---> e7cf08275692
Removing intermediate container 5904e4c70043
Successfully built e7cf08275692
Successfully tagged cephalin/mydockerimage:v1.0.0
```

Testen Sie, ob der Build funktioniert, indem Sie den Docker-Container ausführen. Rufen Sie den Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/) auf, und übergeben Sie ihm Namen und Tag des Images. Stellen Sie sicher, dass Sie auch den Port mit dem `-p`-Argument angeben.

```bash
docker run -p 2222:8000 <docker-ID>/mydockerimage:v1.0.0
```

Überprüfen Sie, ob Web-App und Container ordnungsgemäß funktionieren, indem Sie `http://localhost:2222` lokal aufrufen.

![Lokales Testen der Web-App](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-local.png)

## <a name="push-the-docker-image-to-docker-hub"></a>Pushübertragung eines Docker-Images auf den Docker-Hub

Eine Registrierung ist eine Anwendung, die Images hostet und Image- sowie Container-Dienste für Dienste bereitstellt. Damit Ihr Image gemeinsam verwendet werden kann, müssen Sie es per Pushübertragung an eine Registrierung senden. 

<!-- Depending on your requirements, you may have your docker images in a Public Docker Registry, such as Docker Hub, or a Private Docker Registry such as Azure Container Registry. Select the appropriate tab for your scenario below (your selection will switch multiple tabs on this page). -->

> [!NOTE]
> Pushübertragung an eine private Docker-Registrierung? Die optionalen Anweisungen hierzu finden Sie unter [Pushübertragung eines Docker-Images an eine private Registrierung (optional)](#push-a-docker-image-to-private-registry-optional).

<!--## [Docker Hub](#tab/docker-hub)-->

Docker Hub ist eine Registrierung für Docker-Images, die Ihnen ermöglicht, Ihre eigenen Repositorys – entweder öffentlich oder privat – zu hosten. Verwenden Sie für die Pushübertragung eines benutzerdefinierten Docker-Images an den öffentlichen Docker-Hub den [docker push](https://docs.docker.com/engine/reference/commandline/push/)-Befehl, und geben Sie Image-Name und -Tag vollständig an. Ein vollständiger Name und ein vollständiges Tag des Images sehen wie folgt aus:

```bash
<docker-id>/image-name:tag
```

Wenn Sie sich noch nicht bei Docker-Hub angemeldet haben, tun Sie dies mit dem [docker login](https://docs.docker.com/engine/reference/commandline/login/)-Befehl, bevor Sie die Pushübertragung eines Images versuchen.

```bash
docker login --username <docker-id> --password <docker-hub-password>
```

Eine „login succeeded“-Meldung bestätigt Ihre erfolgreiche Anmeldung. Wenn Sie angemeldet sind, können Sie das Image mit dem [docker Push](https://docs.docker.com/engine/reference/commandline/push/)-Befehl per Pushübertragung an Docker-Hub senden.

```bash
docker push <docker-id>/mydockerimage:v1.0.0 .
```

Überprüfen Sie anhand der Befehlsausgabe, ob der Pushvorgang erfolgreich war.

```bash
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
c33197c3f6d4: Pushed
ccd2c850ee43: Pushed
02dff2853466: Pushed
6ce78153632a: Pushed
efef3f03cc58: Pushed
3439624d77fb: Pushed
3a07adfb35c5: Pushed
2fcec228e1b7: Mounted from library/python
97d2d3bae505: Mounted from library/python
95aadeabf504: Mounted from library/python
b456afdc9996: Mounted from library/python
d752a0310ee4: Mounted from library/python
db64edce4b5b: Mounted from library/python
d5d60fc34309: Mounted from library/python
c01c63c6823d: Mounted from library/python
v1.0.0: digest: sha256:21f2798b20555f4143f2ca0591a43b4f6c8138406041f2d32ec908974feced66 size: 3676
```

<!--
# [Private Registry](#tab/private-registry)

// Move Private Registry instructions here when Tabbed Conceptual bug is fixed

---
-->

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-app-to-azure"></a>Bereitstellen von Apps in Azure

Mithilfe von Azure-Web-Apps können Sie native Linux-Anwendungen in der Cloud hosten. Um eine Web-App für Container zu erstellen, müssen Sie Azure CLI-Befehle ausführen, die eine Gruppe erstellen, dann einen Dienstplan und schließlich die Web-App selbst. 

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-linux-app-service-plan"></a>Erstellen eines Linux-App Service-Plans

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)] 

### <a name="create-a-web-app"></a>Erstellen einer Web-App

Erstellen Sie in der Cloud Shell im App Service-Plan `myAppServicePlan` mit dem Befehl [az webapp create](/cli/azure/webapp#create) eine [Web-App](app-service-linux-intro.md). Vergessen Sie nicht, `<app_name>` durch einen eindeutigen Anwendungsnamen und <Docker-ID> durch Ihre Docker-ID zu ersetzen.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --deployment-container-image-name <docker-ID>/mydockerimage:v1.0.0
```

Nach Erstellung der Web-App zeigt die Azure CLI eine Ausgabe wie im folgenden Beispiel an:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="configure-environment-variables"></a>Konfigurieren von Umgebungsvariablen

Die meisten Docker-Images weisen Umgebungsvariablen auf, die konfiguriert werden müssen. Wenn Sie ein vorhandenes Docker-Image nutzen, das von einer anderen Person erstellt wurde, verwendet das Image unter Umständen einen anderen Port als Port 80. Sie teilen Azure den von Ihrem Image verwendeten Port über die App-Einstellung `WEBSITES_PORT` mit. Die GitHub-Seite für das [Python-Beispiel in diesem Tutorial](https://github.com/Azure-Samples/docker-django-webapp-linux) zeigt an, dass Sie für `WEBSITES_PORT` den Wert _8000_ festlegen müssen.

Verwenden Sie zum Festlegen der App-Einstellungen den [az webapp config appsettings update](/cli/azure/webapp/config/appsettings#update)-Befehl in der Cloud Shell. Bei App-Einstellungen wird die Groß-/Kleinschreibung berücksichtigt und die Einstellungen sind durch Leerzeichen getrennt.

```azurecli-interactive
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WEBSITES_PORT=8000
```

<!-- Depending on your requirements, you may have your docker images in a Public Docker Registry, such as Docker Hub, or a Private Docker Registry, such as Azure Container Registry. Select the appropriate tab for your scenario below: -->

> [!NOTE]
> Bereitstellung aus einer privaten Docker-Registrierung? Die optionalen Anweisungen hierzu finden Sie unter [Konfigurieren einer Web-App zur Verwendung eines Docker-Containers aus einer privaten Registrierung (optional)](#configure-web-app-to-use-docker-container-from-a-private-registry-optional).

<!-- # [Docker Hub](#tab/docker-hub)-->

<!-- # [Private Registry](#tab/private-registry)

// Place Private Registry text back here once Tabbed Conceptual bug is fixed

---
-->

### <a name="test-the-web-app"></a>Testen der Web-App

Stellen Sie sicher, dass die Web-App funktioniert, indem Sie zu ihr navigieren (`http://<app_name>azurewebsites.net`). 

![Testen der Konfiguration des Web-App-Ports](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-azure.png)

## <a name="change-web-app-and-redeploy"></a>Ändern der Web-App und erneutes Bereitstellen

Öffnen Sie „app/templates/app/index.html“ in Ihrem lokalen Git-Repository. Suchen Sie das erste HTML-Element und passen Sie es an.

```python
<nav class="navbar navbar-inverse navbar-fixed-top">
    <div class="container">
      <div class="navbar-header">         
        <a class="navbar-brand" href="#">Azure App Service - Updated Here!</a>       
      </div>            
    </div>
  </nav> 
```

Nachdem Sie die Python-Datei geändert und gespeichert haben, müssen Sie sie neu erstellen und das neue Docker-Image per Pushübertragung senden. Starten Sie dann die Web-App neu, damit die Änderungen wirksam werden. Verwenden Sie die gleichen Befehle, die Sie zuvor in diesem Tutorial verwendet haben. Weitere Informationen finden Sie unter [Erstellen des Images aus der Docker-Datei](#build-the-image-from-the-docker-file) und [Pushübertragung des Docker-Images zu Docker Hub](#push-the-docker-image-to-docker-hub). Testen Sie die Web-App, indem Sie die Anweisungen in [Testen der Web-App](#test-the-web-app) befolgen.

## <a name="connect-to-web-app-for-containers-using-ssh"></a>Herstellen einer Verbindung mit der Web-App für Container mithilfe von SSH

SSH ermöglicht die sichere Kommunikation zwischen einem Container und einem Client. Damit ein benutzerdefiniertes Docker-Image SSH unterstützen kann, muss es in einer Docker-Datei erstellt werden. Sie aktivieren SSH in der Docker-Datei selbst. Die SSH-Anweisungen wurden bereits der Docker-Beispieldatei hinzugefügt, sodass Sie diese Anweisungen mit Ihrem eigenen benutzerdefinierten Image ausführen können:

* Eine [RUN](https://docs.docker.com/engine/reference/builder/#run)-Anweisung, die `apt-get` aufruft und dann das Kennwort für das Stammkonto auf `"Docker!"` festlegt.

    ```docker
    ENV SSH_PASSWD "root:Docker!"
    RUN apt-get update \
            && apt-get install -y --no-install-recommends dialog \
            && apt-get update \
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "$SSH_PASSWD" | chpasswd 
    ```

    > [!NOTE]
    > Diese Konfiguration erlaubt keine externen Verbindungen zum Container. SSH ist nur über die Kudu/SCM-Website verfügbar. Die Kudu/SCM-Website wird durch die Anmeldeinformationen für die Veröffentlichung authentifiziert.

* Eine [COPY](https://docs.docker.com/engine/reference/builder/#copy)-Anweisung, die das Docker-Modul anweist, die [sshd_config](http://man.openbsd.org/sshd_config)-Datei in das */etc/ssh/*-Verzeichnis zu kopieren. Die Konfigurationsdatei sollte auf [dieser sshd_config-Datei](https://github.com/Azure-App-Service/node/blob/master/6.11.1/sshd_config) basieren.

    ```docker
    COPY sshd_config /etc/ssh/
    ```

    > [!NOTE]
    > Die *sshd_config*-Datei muss Folgendes enthalten: 
    > * `Ciphers` muss mindestens ein Element aus dieser Liste enthalten: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs` muss mindestens ein Element aus dieser Liste enthalten: `hmac-sha1,hmac-sha1-96`.

* Eine [EXPOSE](https://docs.docker.com/engine/reference/builder/#expose)-Anweisung, die Port 2222 im Container verfügbar macht. Selbst wenn das Root-Kennwort bekannt ist, kann auf Port 2222 nicht aus dem Internet zugegriffen werden. Es handelt sich um einen internen Port, auf den nur von Containern innerhalb des Brückennetzwerks eines privaten virtuellen Netzwerks zugegriffen werden kann. Danach kopieren Befehle Details der SSH-Konfiguration und starten den `ssh`-Dienst.

    ```docker
    EXPOSE 8000 2222

    RUN service ssh start
    ```

### <a name="open-ssh-connection-to-container"></a>Öffnen einer SSH-Verbindung mit einem Container

Web-App für Container erlaubt keine externen Verbindungen zum Container. SSH ist nur über die Kudu-Website unter `https://<app_name>.scm.azurewebsites.net` verfügbar.

Um eine Verbindung herzustellen, navigieren Sie zu `https://<app_name>.scm.azurewebsites.net/webssh/host`, und melden Sie sich mit Ihrem Azure-Konto an.

Sie werden dann zu einer Seite umgeleitet, die eine interaktive Konsole anzeigt. 

Sie möchten möglicherweise sicherstellen, dass bestimmte Anwendungen im Container ausgeführt werden. Geben Sie zum Überprüfen des Containers und der ausgeführten Prozesse den `top`-Befehl an der Eingabeaufforderung ein.

```bash
top
```

Der `top`-Befehl macht alle aktiven Prozesse in einem Container verfügbar.

```bash
PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
 1 root      20   0  945616  35372  15348 S  0.0  2.1   0:04.63 node
20 root      20   0   55180   2776   2516 S  0.0  0.2   0:00.00 sshd
42 root      20   0  944596  33340  15352 S  0.0  1.9   0:05.80 node /opt/s+
56 root      20   0   59812   5244   4512 S  0.0  0.3   0:00.93 sshd
58 root      20   0   20228   3128   2664 S  0.0  0.2   0:00.00 bash
62 root      20   0   21916   2272   1944 S  0.0  0.1   0:03.15 top
63 root      20   0   59812   5344   4612 S  0.0  0.3   0:00.03 sshd
65 root      20   0   20228   3140   2672 S  0.0  0.2   0:00.00 bash
71 root      20   0   59812   5380   4648 S  0.0  0.3   0:00.02 sshd
73 root      20   0   20228   3160   2696 S  0.0  0.2   0:00.00 bash
77 root      20   0   21920   2304   1972 R  0.0  0.1   0:00.00 top
```

Glückwunsch! Sie haben ein benutzerdefiniertes Docker-Image für eine Web-App für Container konfiguriert.

## <a name="use-a-private-image-from-docker-hub-optional"></a>Verwenden eines privaten Images von Docker Hub (optional)

In [Erstellen einer Web-App](#create-a-web-app) haben Sie ein Image auf Docker Hub im `az webapp create`-Befehl angegebenen. Dies ist für ein öffentliches Image ausreichend. Um ein privates Image zu verwenden, müssen Sie die Docker-Konto-ID und das zugehörige Kennwort in Azure Web App konfigurieren.

Folgen Sie in der Cloud Shell denM Befehl `az webapp create` mit [az webapp config Container set](/cli/azure/webapp/config/container#az_webapp_config_container_set). Ersetzen Sie *\<App-Name>* und auch _<Docker-ID>_ und _<password>_ durch Ihre Docker-ID und das zugehörige Kennwort.

```azurecli-interactive
az webapp config container set --name <app_name> --resource-group myResourceGroup --docker-registry-server-user <docker-id> --docker-registry-server-password <password>
```

Die Ausgabe des Befehls ähnelt der folgenden JSON-Zeichenfolge und zeigt an, dass die Konfigurationsänderung erfolgreich war:

```bash
[
  {
    "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
    "slotSetting": false,
    "value": "false"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_USERNAME",
    "slotSetting": false,
    "value": "<docker-id>"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_PASSWORD",
    "slotSetting": false,
    "value": null
  },
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "value": "DOCKER|<image-name-and-tag>"
  }
]
```

## <a name="use-a-docker-image-from-any-private-registry-optional"></a>Verwenden eines Docker-Images für eine private Registrierung (optional)

In diesem Abschnitt erfahren Sie, wie Sie ein Docker-Image aus einer privaten Registrierung in Web-App für Container verwenden. Dabei wird Azure Container Registry als Beispiel verwendet. Die Schritte zum Verwenden anderer privater Registrierungen sind ähnlich. 

Azure Container Registry ist ein verwalteter Docker-Dienst von Azure zum Hosten von privaten Images. Die Bereitstellungen können beliebigen Typs sein, einschließlich [Docker Swarm](https://docs.docker.com/engine/swarm/), [Kubernetes](https://kubernetes.io/) und Web-App für Container. 

### <a name="create-an-azure-container-registry"></a>Erstellen einer Azure-Containerregistrierung

Erstellen Sie mit dem Befehl [az acr create](https://docs.microsoft.com/cli/azure/acr#az_acr_create) in der Cloud Shell eine Azure Container Registry. Übergeben Sie Namen, Ressourcengruppe und `Basic` für die SKU. Verfügbare SKUs sind `Classic`, `Basic`, `Standard` und `Premium`.

```azurecli-interactive
az acr create --name <azure-container-registry-name> --resource-group myResourceGroup --sku Basic --admin-enabled true
```

Das Erstellen eines Containers erzeugt die folgende Ausgabe:

```bash
 - Finished ..
Create a new service principal and assign access:
  az ad sp create-for-rbac --scopes /subscriptions/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<azure-container-registry-name> --role Owner --password <password>

Use an existing service principal and assign access:
  az role assignment create --scope /subscriptions/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<azure-container-registry-name> --role Owner --assignee <app-id>
{
  "adminUserEnabled": false,
  "creationDate": "2017-08-09T04:21:09.654153+00:00",
  "id": "/subscriptions/<subscriptionId>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<azure-container-registry-name>",
  "location": "westeurope",
  "loginServer": "<azure-container-registry-name>.azurecr.io",
  "name": "<azure-container-registry-name>",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "myazurecontainerre042025"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### <a name="log-in-to-azure-container-registry"></a>Anmelden bei Azure Container Registry

Zur Pushübertragung eines Images an die Registrierung müssen Sie Anmeldeinformationen angeben, damit die Registrierung den Pushvorgang akzeptiert. Sie können diese Anmeldeinformationen mit dem [az acr show](https://docs.microsoft.com/cli/azure/acr/credential#az_acr_credential_show)-Befehl in der Cloud Shell abrufen. 

```azurecli-interactive
az acr credential show --name <azure-container-registry-name>
```

Der Befehl zeigt zwei Kennwörter, die mit dem Benutzernamen verwendet werden können.

```json
<
  "passwords": [
    {
      "name": "password",
      "value": "{password}"
    },
    {
      "name": "password2",
      "value": "{password}"
    }
  ],
  "username": "<registry-username>"
}
```

Melden Sie sich über ein lokales Terminalfenster bei Azure Container Registry mit dem Befehl `docker login` an. Der Servername muss angegeben werden, um sich einzuloggen. Verwenden Sie das Format `{azure-container-registry-name>.azurecr.io`.

```bash
docker login <azure-container-registry-name>.azurecr.io --username <registry-username> --password <password> 
```

Vergewissern Sie sich, dass die Anmeldung erfolgreich war. 

### <a name="push-an-image-to-azure-container-registry"></a>Übertragen eines Images zu Azure Container Registry mithilfe von Push

Übertragen Sie das Image mithilfe des Befehls `docker push`. Kennzeichnen Sie das Image mit dem Namen der Registrierung, gefolgt von Name und Tag des Image.

```bash
docker push <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

Überprüfen Sie durch Auflisten der ACR-Repositorys, ob die Pushübertragung der Registrierung erfolgreich einen Container hinzugefügt hat. 

```azurecli-interactive
az acr repository list -n <azure-container-registry-name>
```

Durch Auflisten der Images in der Registrierung wird sichergestellt, dass `mydockerimage` sich in der Registrierung befindet.

```json
[
  "mydockerimage"
]
```

### <a name="configure-web-app-to-use-the-image-from-azure-container-registry-or-any-private-registry"></a>Konfigurieren von Web-App für die Verwendung des Image von Azure Container Registry (oder einer privaten Registrierung)

Sie können Web-App für Container so konfigurieren, dass es einen in Azure Container Registry gespeicherten Container ausführt. Die Verwendung der Azure Container Registry ist wie die Verwendung einer privaten Registrierung. Wenn Sie also Ihre eigene private Registrierung verwenden müssen, sind die Schritte zur Ausführung dieser Aufgabe ähnlich.

Führen Sie in der Cloud Shell den Befehl [az acr credential show](/cli/azure/acr/credential#az_acr_credential_show) aus, um den Benutzernamen und das Kennwort für Azure Container Registry anzuzeigen. Kopieren Sie den Benutzernamen und eines der Kennwörter zur Verwendung für die Konfiguration der Web-App im nächsten Schritt.

```bash
az acr credential show --name <azure-container-registry-name>
```

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "password"
    },
    {
      "name": "password2",
      "value": "password2"
    }
  ],
  "username": "<registry-username>"
}
```

Führen Sie in der Cloud Shell den Befehl [az webapp config container set](/cli/azure/webapp/config/container#az_webapp_config_container_set) aus, um das benutzerdefinierte Docker-Image der Web-App zuzuweisen. Ersetzen Sie *\<App-Name>*, *\<Server-URL-der-Docker-Registrierung>*, _\<Registrierung-Benutzername>_ und _\<Kennwort>_. Für Azure Container Registry hat *\<Server-URL-der-Docker-Registrierung>* das Format `https://<azure-container-registry-name>.azurecr.io`. 

```azurecli-interactive
az webapp config container set --name <app_name> --resource-group myResourceGroup --docker-custom-image-name mydockerimage --docker-registry-server-url https://<azure-container-registry-name>.azurecr.io --docker-registry-server-user <registry-username> --docker-registry-server-password <password>
```

> [!NOTE]
> `https://` ist in *\<Server-URL-der-Docker-Registrierung>* erforderlich.
>

Die Ausgabe des Befehls ähnelt der folgenden JSON-Zeichenfolge und zeigt an, dass die Konfigurationsänderung erfolgreich war:

```bash
[
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "slotSetting": false,
    "value": "mydockerimage"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_URL",
    "slotSetting": false,
    "value": "<azure-container-registry-name>.azurecr.io"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_USERNAME",
    "slotSetting": false,
    "value": "<registry-username>"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_PASSWORD",
    "slotSetting": false,
    "value": null
  }
]
```

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Docker Python- und PostgreSQL-Web-App in Azure](tutorial-docker-python-postgresql-app.md)
