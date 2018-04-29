---
title: Continuous Deployment mit Jenkins über Kubernetes in Azure Container Service
description: Automatisieren eines Continuous Deployment-Prozesses mit Jenkins zum Bereitstellen und Aktualisieren einer Container-App über Kubernetes in Azure Container Service
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/26/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7ebe7a88fcb0a0785b72c512e64a2d9aeb5fc506
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="continuous-deployment-with-jenkins-and-azure-container-service"></a>Continuous Deployment mit Jenkins und Azure Container Service

In diesem Dokument wird beschrieben, wie Sie einen einfachen Continuous Deployment-Workflow zwischen Jenkins und einem AKS-Cluster (Azure Container Service) einrichten.

Der Beispielworkflow umfasst die folgenden Schritte:

> [!div class="checklist"]
> * Bereitstellen der Azure Vote-Anwendung in Ihrem Kubernetes-Cluster
> * Aktualisieren des Codes der Azure Vote-Anwendung und Durchführen der Übertragung in ein GitHub-Repository per Pushvorgang, um den Continuous Deployment-Prozess zu starten
> * Jenkins klont das Repository und erstellt ein neues Containerimage mit dem aktualisierten Code.
> * Dieses Image wird per Pushvorgang in Azure Container Registry (ACR) übertragen.
> * Die Anwendung, die im AKS-Cluster ausgeführt wird, wird mit dem neuen Containerimage aktualisiert.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um die Schritte in diesem Artikel ausführen zu können:

- Grundkenntnisse in Bezug auf Kubernetes, Git, CI/CD und Azure Container Registry (ACR).
- Einen [AKS-Cluster (Azure Container Service)][aks-quickstart] und [konfigurierte AKS-Anmeldeinformationen][aks-credentials] in Ihrem Entwicklungssystem.
- Eine [Registrierung vom Typ „Azure Container Registry (ACR)“][acr-quickstart], den ACR-Anmeldeservernamen und [ACR-Anmeldeinformationen][acr-authentication] mit Push- und Pullzugriff.
- Eine Installation der Azure CLI auf Ihrem Entwicklungssystem.
- Eine Installation von Docker auf Ihrem Entwicklungssystem.
- Ein GitHub-Konto, das [persönliche Zugriffstoken für GitHub][git-access-token] und eine Installation des Git-Clients auf Ihrem Entwicklungssystem.

## <a name="prepare-application"></a>Vorbereiten der Anwendung

Die in diesem Dokument verwendete Azure Vote-Anwendung enthält eine Webschnittstelle, die in einem oder mehreren Pods gehostet wird, und einen zusätzlichen Pod für die temporäre Datenspeicherung.

Bereiten Sie die Azure Vote-Anwendung vor, und stellen Sie sie in Ihrem AKS-Cluster bereit, bevor Sie die Jenkins/AKS-Integration erstellen. Stellen Sie sich dies als erste Version der Anwendung vor.

Forken Sie das folgende GitHub-Repository.

```
https://github.com/Azure-Samples/azure-voting-app-redis
```

Nachdem der Fork erstellt wurde, können Sie das Klonen für Ihr Entwicklungssystem durchführen. Stellen Sie sicher, dass Sie beim Klonen dieses Repositorys die URL Ihres Forks verwenden.

```bash
git clone https://github.com/<your-github-account>/azure-voting-app-redis.git
```

Wechseln Sie in das Verzeichnis, sodass Sie im geklonten Verzeichnis arbeiten.

```bash
cd azure-voting-app-redis
```

Führen Sie die Datei `docker-compose.yaml` aus, um das Containerimage `azure-vote-front` zu erstellen und die Anwendung zu starten.

```bash
docker-compose up -d
```

Verwenden Sie anschließend den Befehl [docker-images][docker-images], um das erstellte Image anzuzeigen.

Beachten Sie, dass drei Images heruntergeladen oder erstellt wurden. Das `azure-vote-front`-Image enthält die Anwendung und verwendet das `nginx-flask`-Image als Grundlage. Das `redis`-Image wird zum Starten einer Redis-Instanz verwendet.

```console
$ docker images

REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Rufen Sie den ACR-Anmeldeserver mit dem Befehl [az acr list][az-acr-list] auf. Stellen Sie sicher, dass Sie den Ressourcengruppennamen mit der Ressourcengruppe aktualisieren, von der die ACR gehostet wird.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Verwenden Sie den Befehl [docker tag][docker-tag], um das Image mit dem Anmeldeservernamen und der Versionsnummer `v1` zu versehen.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Aktualisieren Sie den Wert für den ACR-Anmeldeserver mit Ihrem ACR-Anmeldeservernamen, und übertragen Sie das Image `azure-vote-front` in die Registrierung.

```bash
docker push <acrLoginServer>/azure-vote-front:v1
```

## <a name="deploy-application-to-kubernetes"></a>Bereitstellen einer Anwendung in Kubernetes

Im Stammverzeichnis des Azure Vote-Repositorys befindet sich eine Kubernetes-Manifestdatei, die zum Bereitstellen der Anwendung in Ihrem Kubernetes-Cluster verwendet werden kann.

Aktualisieren Sie zuerst die Manifestdatei **azure-vote-all-in-one-redis.yaml** mit dem Standort Ihrer ACR. Öffnen Sie die Datei mit einem Text-Editor, und ersetzen Sie `microsoft` durch den Namen des ACR-Anmeldeservers. Dieser Wert befindet sich in Zeile **47** der Manifestdatei.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Führen Sie als Nächstes den Befehl [kubectl create][kubectl-create] aus, um die Anwendung auszuführen. Dieser Befehl analysiert die Manifestdatei und erstellt die definierten Kubernetes-Objekte.

```bash
kubectl create -f azure-vote-all-in-one-redis.yaml
```

Es wird ein [Kubernetes-Dienst][kubernetes-service] erstellt, um die Anwendung im Internet verfügbar zu machen. Dieser Vorgang kann einige Minuten dauern.

Verwenden Sie zum Überwachen des Fortschritts den Befehl [kubectl get service][kubectl-get] mit dem Argument `--watch`.

```bash
kubectl get service azure-vote-front --watch
```

Die *externe IP-Adresse* für den Dienst *azure-vote-front* wird zunächst als *ausstehend* angezeigt.

```
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
```

Sobald die *externe IP-Adresse* nicht mehr *ausstehend* ist, sondern eine *IP-Adresse* angezeigt wird, verwenden Sie `control+c`, um die kubectl-Überwachung zu beenden.

```
azure-vote-front   10.0.34.242   13.90.150.118   80:30676/TCP   2m
```

Um die Anwendung anzuzeigen, navigieren Sie zu der externen IP-Adresse.

![Abbildung: Kubernetes-Cluster in Azure](media/aks-jenkins/azure-vote-safari.png)

## <a name="deploy-jenkins-to-vm"></a>Bereitstellen von Jenkins auf einer VM

Es wurde ein Skript erstellt, mit dem ein virtueller Computer bereitgestellt, der Netzwerkzugriff konfiguriert und eine einfache Installation von Jenkins durchgeführt wird. Außerdem bewirkt das Skript, dass Ihre Kubernetes-Konfigurationsdatei aus Ihrem Entwicklungssystem auf das Jenkins-System kopiert wird. Diese Datei wird für die Authentifizierung zwischen Jenkins und dem AKS-Cluster verwendet.

Führen Sie die folgenden Befehle aus, um das Skript herunterzuladen und auszuführen. Die unten angegebene URL kann auch verwendet werden, um den Inhalt des Skripts zu prüfen.

```console
curl https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh > azure-jenkins.sh
sh azure-jenkins.sh
```

Nachdem das Skript abgeschlossen wurde, werden eine Adresse für den Jenkins-Server und ein Schlüssel zum Entsperren von Jenkins ausgegeben. Navigieren Sie zur URL, geben Sie den Schlüssel ein, und befolgen Sie die angezeigten Aufforderungen, um die Jenkins-Konfiguration durchzuführen.

```console
Open a browser to http://52.166.118.64:8080
Enter the following to Unlock Jenkins:
667e24bba78f4de6b51d330ad89ec6c6
```

## <a name="jenkins-environment-variables"></a>Jenkins-Umgebungsvariablen

Eine Jenkins-Umgebungsvariable wird verwendet, um den Namen des ACR-Anmeldeservers (Azure Container Registry) zu speichern. Auf diese Variable wird während der Ausführung eines Jenkins-Continuous Deployment-Auftrags verwiesen.

Klicken Sie im Jenkins-Verwaltungsportal auf **Manage Jenkins** > **Configure System**.

Wählen Sie unter **Globale Eigenschaften** die Option **Environment variables**, und fügen Sie eine Variable mit dem Namen `ACR_LOGINSERVER` und dem Wert Ihres ACR-Anmeldeservers hinzu.

![Jenkins-Umgebungsvariablen](media/aks-jenkins/env-variables.png)

Klicken Sie nach Abschluss des Vorgangs auf der Seite für die Jenkins-Konfiguration auf **Save**.

## <a name="jenkins-credentials"></a>Jenkins-Anmeldeinformationen

Speichern Sie Ihre ACR-Anmeldeinformationen jetzt in einem Objekt für Jenkins-Anmeldeinformationen. Auf diese Anmeldeinformationen wird während eines Jenkins-Erstellungsauftrags verwiesen.

Klicken Sie im Jenkins-Verwaltungsportal auf **Credentials** > **Jenkins** > **Global credentials (unrestricted)** > **Add Credentials**.

Stellen Sie sicher, dass als Typ für die Anmeldeinformationen **Username with password** ausgewählt ist, und geben Sie Folgendes ein:

- **Username**: ID des Dienstprinzipals für die Authentifizierung bei Ihrer ACR.
- **Password**: Geheimer Clientschlüssel des Dienstprinzipals für die Authentifizierung bei Ihrer ACR.
- **ID**: Bezeichner für die Anmeldeinformationen, z.B. `acr-credentials`.

Nach Abschluss des Vorgangs sollte das Formular mit den Anmeldeinformationen in etwa wie hier dargestellt aussehen:

![ACR-Anmeldeinformationen](media/aks-jenkins/acr-credentials.png)

Klicken Sie auf **OK**, und wechseln Sie zurück zum Jenkins-Verwaltungsportal.

## <a name="create-jenkins-project"></a>Erstellen eines Jenkins-Projekts

Klicken Sie im Jenkins-Verwaltungsportal auf **New Item**.

Geben Sie einen Namen für das Projekt ein, z.B. `azure-vote`, wählen Sie **Freestyle Project**, und klicken Sie auf **OK**.

![Jenkins-Projekt](media/aks-jenkins/jenkins-project.png)

Wählen Sie unter **General** die Option **GitHub project**, und geben Sie die URL zu Ihrem Fork des Azure Vote-GitHub-Projekts ein.

![GitHub-Projekt](media/aks-jenkins/github-project.png)

Wählen Sie unter **Source Code Management** die Option **Git**, und geben Sie die URL zu Ihrem Fork des Azure Vote-GitHub-Repositorys ein.

Klicken Sie auf **Add** > **Jenkins**, um die Anmeldeinformationen zu erhalten. Wählen Sie unter **Kind** die Option **Secret text**, und geben Sie Ihr persönliches GitHub-Zugriffstoken ([GitHub personal access token][git-access-token]) als Geheimnis ein.

Wählen Sie **Add**, wenn Sie fertig sind.

![GitHub-Anmeldeinformationen](media/aks-jenkins/github-creds.png)

Wählen Sie unter **Build Triggers** die Option **GitHub hook trigger for GITScm polling**.

![Jenkins – Erstellen von Triggern](media/aks-jenkins/build-triggers.png)

Wählen Sie unter **Build Environment** die Option **Use secret texts or files**.

![Jenkins-Buildumgebung](media/aks-jenkins/build-environment.png)

Wählen Sie unter **Bindings** die Option **Add** > **Username and password (separated)**.

Geben Sie `ACR_ID` unter **Username Variable** und `ACR_PASSWORD` unter **Password Variable** ein.

![Jenkins-Bindungen](media/aks-jenkins/bindings.png)

Fügen Sie einen **Build Step** vom Typ **Execute shell** hinzu, und verwenden Sie den folgenden Text. Mit diesem Skript wird ein neues Containerimage erstellt und per Pushvorgang in Ihre ACR übertragen.

```bash
# Build new image and push to ACR.
WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
docker build -t $WEB_IMAGE_NAME ./azure-vote
docker login ${ACR_LOGINSERVER} -u ${ACR_ID} -p ${ACR_PASSWORD}
docker push $WEB_IMAGE_NAME
```

Fügen Sie einen weiteren **Build Step** vom Typ **Execute shell** hinzu, und verwenden Sie den folgenden Text. Mit diesem Skript wird die Kubernetes-Bereitstellung aktualisiert.

```bash
# Update kubernetes deployment with new image.
WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
kubectl set image deployment/azure-vote-front azure-vote-front=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
```

Klicken Sie nach Abschluss des Vorgangs auf **Save**.

## <a name="test-the-jenkins-build"></a>Testen des Jenkins-Builds

Testen Sie den Jenkins-Build, bevor Sie fortfahren. Hierbei wird überprüft, ob der Buildauftrag richtig konfiguriert wurde, die richtige Kubernetes-Authentifizierungsdatei vorhanden ist und die richtigen ACR-Anmeldeinformationen angegeben wurden.

Klicken Sie im Menü auf der linken Seite des Projekts auf **Build Now**.

![Jenkins-Testbuild](media/aks-jenkins/test-build.png)

Während dieses Vorgangs wird das GitHub-Repository auf dem Jenkins-Buildserver geklont. Ein neues Containerimage wird erstellt und per Pushvorgang in die ACR übertragen. Abschließend wird die Azure Vote-Anwendung, die im AKS-Cluster ausgeführt wird, aktualisiert, damit das neue Image verwendet wird. Da am Anwendungscode keine Änderungen vorgenommen wurden, wird die Anwendung nicht geändert.

Nachdem der Prozess abgeschlossen ist, klicken Sie unter „build history“ auf **build #1** und wählen die Option **Console Output**, um die gesamte Ausgabe des Buildvorgangs anzuzeigen. In der letzten Zeile sollte die erfolgreiche Durchführung des Builds angezeigt werden.

## <a name="create-github-webhook"></a>Erstellen eines GitHub-Webhooks

Hängen Sie als Nächstes das Anwendungsrepository an den Jenkins-Buildserver an, damit bei jedem Commit ein neuer Buildvorgang ausgelöst wird.

1. Navigieren Sie zum geforkten GitHub-Repository.
2. Wählen Sie **Einstellungen** und anschließend **Integrations & Services** (Integrationen und Dienste) auf der linken Seite aus.
3. Wählen Sie die Option **Add Service**, geben Sie im Filterfeld `Jenkins (GitHub plugin)` ein, und wählen Sie das Plug-In aus.
4. Geben Sie als Jenkins-Hook-URL `http://<publicIp:8080>/github-webhook/` ein, wobei `publicIp` die IP-Adresse des Jenkins-Servers ist. Stellen Sie sicher, dass Sie den nachgestellten Schrägstrich (/) hinzufügen.
5. Wählen Sie „Add service“.

![GitHub-Webhook](media/aks-jenkins/webhook.png)

## <a name="test-cicd-process-end-to-end"></a>Durchführen eines Ende-zu-Ende-Tests des CI/CD-Prozesses

Öffnen Sie auf dem Entwicklungscomputer die geklonte Anwendung mit einem Code-Editor.

Im Verzeichnis **/azure-vote/azure-vote** ist eine Datei mit dem Namen **config_file.cfg** enthalten. Aktualisieren Sie die Vote-Werte in dieser Datei auf andere Werte als „cats“ und „dogs“.

Im folgenden Beispiel ist die aktualisierte Datei **config_file.cfg** dargestellt.

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Nach Abschluss des Vorgangs können Sie die Datei speichern und die Änderungen committen und per Pushvorgang an Ihren Fork des GitHub-Repositorys übertragen. Wenn der Commit-Vorgang beendet ist, löst der GitHub-Webhook einen neuen Jenkins-Buildvorgang aus, mit dem das Containerimage und die AKS-Bereitstellung aktualisiert werden. Überwachen Sie den Buildprozess auf der Jenkins-Verwaltungskonsole.

Navigieren Sie nach Abschluss des Buildvorgangs zurück zum Anwendungsendpunkt, um die Änderungen zu prüfen.

![Azure Vote – Aktualisiert](media/aks-jenkins/azure-vote-updated-safari.png)

Der einfache Continuous Deployment-Prozess ist nun abgeschlossen. Die in diesem Beispiel gezeigten Schritte und Konfigurationen können verwendet werden, um eine stabilere und für die Produktion geeignete fortlaufende Automatisierung des Buildvorgangs zu erzielen.

<!-- LINKS - external -->
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[git-access-token]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[az-acr-list]: /cli/azure/acr#az_acr_list
[acr-authentication]: ../container-registry/container-registry-auth-aks.md
[acr-quickstart]: ../container-registry/container-registry-get-started-azure-cli.md
[aks-credentials]: /cli/azure/aks#az_aks_get_credentials
[aks-quickstart]: kubernetes-walkthrough.md
[azure-cli-install]: /cli/azure/install-azure-cli