---
title: "Tutorial für Azure Container Instances – Bereitstellungs-App"
description: "Tutorial für Azure Container Instances (Teil 3 von 3) – Bereitstellen der Anwendung"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 99bd03bf4c3ca2d7b1ced51ebfe8be669f271c1c
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2018
---
# <a name="deploy-a-container-to-azure-container-instances"></a>Bereitstellen eines Containers für Azure Container Instances

Dies ist das letzte Tutorial einer dreiteiligen Reihe. In den vorhergehenden Teilen der Reihe wurde [ein Containerimage erstellt](container-instances-tutorial-prepare-app.md) und [mithilfe von Push an eine Azure Container Registry-Instanz übermittelt](container-instances-tutorial-prepare-acr.md). Zum Abschluss der Tutorialreihe wird der Container in diesem Tutorial für Azure Container Instances bereitgestellt.

In diesem Tutorial haben Sie Folgendes durchgeführt:

> [!div class="checklist"]
> * Bereitstellen des Containers aus der Azure Container Registry-Instanz mithilfe der Azure-Befehlszeilenschnittstelle
> * Anzeigen der Anwendung im Browser
> * Anzeigen der Containerprotokolle

## <a name="before-you-begin"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie mindestens Version 2.0.27 der Azure-Befehlszeilenschnittstelle. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI 2.0][azure-cli-install].

Für dieses Tutorial ist eine lokal installierte Docker-Entwicklungsumgebung erforderlich. Für Docker sind Pakete erhältlich, mit denen Docker problemlos auf einem [Mac-][docker-mac], [Windows-][docker-windows] oder [Linux-][docker-linux]System konfiguriert werden kann.

Azure Cloud Shell umfasst keine Docker-Komponenten, die zum Abschließen der einzelnen Schritte dieses Tutorials erforderlich sind. Die Azure-Befehlszeilenschnittstelle und die Docker-Entwicklungsumgebung müssen zur Absolvierung dieses Tutorials auf Ihrem lokalen Computer installiert sein.

## <a name="deploy-the-container-using-the-azure-cli"></a>Bereitstellen des Containers mithilfe der Azure-Befehlszeilenschnittstelle

Mithilfe der Azure-Befehlszeilenschnittstelle kann ein Container mit einem einzelnen Befehl in Azure Container Instances bereitgestellt werden. Da das Containerimage in der privaten Azure Container Registry gehostet wird, müssen Sie die erforderlichen Anmeldeinformationen einschließen, um darauf zugreifen zu können. Rufen Sie die Anmeldeinformationen mit den folgenden Azure-CLI-Befehlen ab.

Anmeldeserver für die Containerregistrierung (durch den Namen Ihrer Registrierung ersetzen):

```azurecli
az acr show --name <acrName> --query loginServer
```

Kennwort der Containerregistrierung:

```azurecli
az acr credential show --name <acrName> --query "passwords[0].value"
```

Um das Containerimage aus der Containerregistrierung mit einer Ressourcenanforderung von einem CPU-Kern und 1 GB Speicher bereitzustellen, führen Sie den folgenden Befehl aus. Ersetzen Sie `<acrLoginServer>` und `<acrPassword>` durch die Werte, die Sie mit den beiden vorhergehenden Befehlen abgerufen haben. Ersetzen Sie `<acrName>` durch den Namen Ihrer Containerregistrierung.

```azurecli
az container create --resource-group myResourceGroup --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-username <acrName> --registry-password <acrPassword> --dns-name-label aci-demo --ports 80
```

Innerhalb weniger Sekunden sollten Sie eine erste Antwort vom Azure Resource Manager erhalten. Der Wert `--dns-name-label` muss innerhalb der Azure-Region, in der Sie die Containerinstanz erstellen, eindeutig sein. Aktualisieren Sie den Wert im vorherigen Beispiel, wenn Sie beim Ausführen des Befehls eine Fehlermeldung bezüglich der **DNS-Namensbezeichnung** erhalten.

Verwenden Sie zum Anzeigen des Bereitstellungsstatus den Befehl [az container show][az-container-show]:

```azurecli
az container show --resource-group myResourceGroup --name aci-tutorial-app --query instanceView.state
```

Führen Sie den Befehl [az container show][az-container-show] erneut aus, bis der Status nicht mehr *Ausstehend*, sondern *Wird ausgeführt* lautet. Das sollte nach maximal einer Minute der Fall sein. Wenn für den Container *Wird ausgeführt* angezeigt wird, fahren Sie mit dem nächsten Schritt fort.

## <a name="view-the-application-and-container-logs"></a>Anzeigen der Anwendungs- und Containerprotokolle

Wenn die Bereitstellung erfolgreich war, zeigen Sie mit dem Befehl [az container show][az-container-show] den vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) des Containers an:

```bash
az container show --resource-group myResourceGroup --name aci-tutorial-app --query ipAddress.fqdn
```

Beispielausgabe: `"aci-demo.eastus.azurecontainer.io"`

Navigieren Sie zum Anzeigen der ausgeführten Anwendung in Ihrem bevorzugten Browser zum angezeigten DNS-Namen:

![„Hello World“-Anwendung im Browser][aci-app-browser]

Sie können auch die Protokollausgabe des Containers anzeigen:

```azurecli
az container logs --resource-group myResourceGroup --name aci-tutorial-app
```

Ausgabe:

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die in dieser Tutorialreihe erstellten Ressourcen nicht mehr benötigen, können Sie den Befehl [az group delete][az-group-delete] ausführen, um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu entfernen. Dieser Befehl löscht die von Ihnen erstellte Containerregistrierung sowie den ausgeführten Container und alle zugehörigen Ressourcen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Ihre Container in Azure Container Instances bereitgestellt. Die folgenden Schritte wurden durchgeführt:

> [!div class="checklist"]
> * Bereitstellen des Containers aus der Azure Container Registry-Instanz mithilfe der Azure-Befehlszeilenschnittstelle
> * Anzeigen der Anwendung im Browser
> * Anzeigen der Containerprotokolle

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-container-show]: /cli/azure/container#az_container_show
[az-group-delete]: /cli/azure/group#az_group_delete
[azure-cli-install]: /cli/azure/install-azure-cli
[prepare-app]: ./container-instances-tutorial-prepare-app.md
