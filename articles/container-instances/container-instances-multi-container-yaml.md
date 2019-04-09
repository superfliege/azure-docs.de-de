---
title: Bereitstellen von Containergruppe mit mehreren Containern in Azure Container Instances mit Azure CLI und YAML
description: Hier erfahren Sie, wie Sie eine Containergruppe mit mehreren Containern mit Azure CLI und einer YAML-Datei in Azure Container Instances bereitstellen.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 03/21/2019
ms.author: danlep
ms.openlocfilehash: 10f2340bd85da3dabcd50d51a4dd56d58d31675b
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2019
ms.locfileid: "58372436"
---
# <a name="deploy-a-multi-container-container-group-with-yaml"></a>Bereitstellen einer Containergruppe mit mehreren Containern mit YAML

Azure Container Instances unterstützt die Bereitstellung von mehreren Containern auf einem einzelnen Host mit einer [Containergruppe](container-instances-container-groups.md). Containergruppe mit mehreren Containern sind nützlich, wenn eine Sidecar-Anwendung für die Protokollierung, die Überwachung oder irgendeine andere Konfiguration, bei der ein Dienst einen zweiten angefügten Prozess benötigt, erstellt wird.

Es gibt zwei Möglichkeiten zum Bereitstellen von Gruppen mit mehreren Containern über die Azure CLI:

* Bereitstellung mit YAML-Datei (in diesem Artikel)
* [Resource Manager-Vorlagenbereitstellung](container-instances-multi-container-group.md)

Das YAML-Format ist präziser, daher wird die Bereitstellung mit einer YAML-Datei empfohlen, wenn Ihre Bereitstellung *nur* Containerinstanzen enthält. Wenn Sie bei der Bereitstellung von Containerinstanzen zusätzliche Azure-Dienstressourcen (z.B. eine Azure Files-Freigabe) bereitstellen müssen, wird die Bereitstellung mit einer Resource Manager-Vorlage empfohlen.

> [!NOTE]
> Gruppen mit mehreren Containern sind aktuell auf Linux-Container beschränkt. Bis alle Features auch für Windows-Container verfügbar sind, finden Sie die aktuellen Plattformunterschiede unter [Kontingente und Regionsverfügbarkeit für Azure Container Instances](container-instances-quotas.md).

## <a name="configure-the-yaml-file"></a>Konfigurieren der YAML-Datei

Um eine Containergruppe mit mehreren Containern mit dem Befehl [az container create][az-container-create] in der Azure CLI bereitzustellen, müssen Sie in der YAML-Datei die Containergruppenkonfiguration angeben, und die YAML-Datei anschließend als Parameter an den Befehl weitergeben.

Starten Sie, indem Sie den folgenden YAML-Inhalt in eine neue Datei mit dem Namen **deploy-aci.yaml** kopieren.

Diese YAML-Datei definiert eine Containergruppe namens „myContainerGroup“ mit zwei Containern, einer öffentlichen IP-Adresse und zwei verfügbar gemachten Ports. Die Container werden aus öffentlichen Microsoft-Images bereitgestellt. Der erste Container in der Gruppe führt eine Webanwendung mit Internetverbindung aus. Vom zweiten Container, dem Sidecar, erfolgen in regelmäßigen Abständen HTTP-Anforderungen an die Webanwendung, die im ersten Container über das lokale Netzwerk der Containergruppe ausgeführt wird.

```YAML
apiVersion: 2018-10-01
location: eastus
name: myContainerGroup
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      image: mcr.microsoft.com/azuredocs/aci-helloworld:latest
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
      ports:
      - port: 80
      - port: 8080
  - name: aci-tutorial-sidecar
    properties:
      image: mcr.microsoft.com/azuredocs/aci-tutorial-sidecar
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
  osType: Linux
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: '80'
    - protocol: tcp
      port: '8080'
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

## <a name="deploy-the-container-group"></a>Bereitstellen der Containergruppe

Erstellen Sie mit dem Befehl [az group create][az-group-create] eine Ressourcengruppe:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Stellen Sie die Containergruppe mit dem Befehl [az container create] [ az-container-create] bereit, und geben Sie die YAML-Datei als Argument weiter:

```azurecli-interactive
az container create --resource-group myResourceGroup --file deploy-aci.yaml
```

Innerhalb weniger Sekunden sollten Sie eine erste Antwort von Azure erhalten.

## <a name="view-deployment-state"></a>Zusammenfassung der Bereitstellungen anzeigen

Um den Bereitstellungsstatus anzuzeigen, verwenden Sie den folgenden Befehl [az container show][az-container-show]:

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

Wenn Sie die laufende Anwendung anzeigen möchten, navigieren Sie zu ihrer IP-Adresse in Ihrem Browser. In dieser Beispielausgabe lautet die IP `52.168.26.124`:

```bash
Name              ResourceGroup    Status    Image                                                                                               IP:ports              Network    CPU/Memory       OsType    Location
----------------  ---------------  --------  --------------------------------------------------------------------------------------------------  --------------------  ---------  ---------------  --------  ----------
myContainerGroup  danlep0318r      Running   mcr.microsoft.com/azuredocs/aci-tutorial-sidecar,mcr.microsoft.com/azuredocs/aci-helloworld:latest  20.42.26.114:80,8080  Public     1.0 core/1.5 gb  Linux     eastus
```

## <a name="view-logs"></a>Anzeigen von Protokollen

Zeigen Sie die Protokollausgabe eines Containers mit dem Befehl [az container logs][az-container-logs] an. Das `--container-name`-Argument gibt den Container an, aus dem Protokolle erhalten werden können. In diesem Beispiel wird der erste Container angegeben.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Ausgabe:

```console
listening on port 80
::1 - - [21/Mar/2019:23:17:48 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:51 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:54 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Führen Sie den gleichen Befehl aus, und geben Sie den zweiten Containernamen an, um die Protokolle für den Sidecar-Container anzuzeigen.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-sidecar
```

Ausgabe:

```console
Every 3s: curl -I http://localhost                          2019-03-21 20:36:41

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
X-Powered-By: Express
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Wed, 29 Nov 2017 06:40:40 GMT
ETag: W/"67f-16006818640"
Content-Type: text/html; charset=UTF-8
Content-Length: 1663
Date: Thu, 21 Mar 2019 20:36:41 GMT
Connection: keep-alive
```

Wie Sie sehen können, erfolgt vom Sidecar in regelmäßigen Abständen eine HTTP-Anforderung an die Hauptwebanwendung über das lokale Netzwerk der Gruppe, um sicherzustellen, dass er ausgeführt wird. Dieses Sidecar-Beispiel kann erweitert werden, um eine Warnung auszulösen, wenn ein anderer HTTP-Antwortcode als 200 OK empfangen wird.

## <a name="deploy-from-private-registry"></a>Bereitstellen über eine private Registrierung

Um eine private Containerimageregistrierung zu verwenden, integrieren Sie den folgenden YAML-Inhalt mit entsprechend für Ihre Umgebung modifizierten Werten:

```YAML
  imageRegistryCredentials:
  - server: imageRegistryLoginServer
    username: imageRegistryUsername
    password: imageRegistryPassword
```

Beispielsweise stellt der folgende YAML-Inhalt eine Containergruppe mit einem einzelnen Container bereit, dessen Image aus einer privaten Azure-Containerregistrierung mit dem Namen“myregistry“ abgerufen wird:

```YAML
apiVersion: 2018-10-01
location: eastus
name: myContainerGroup2
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      image: myregistry.azurecr.io/aci-helloworld:latest
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
      ports:
      - port: 80
  osType: Linux
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: '80'
  imageRegistryCredentials:
  - server: myregistry.azurecr.io
    username: myregistry
    password: REGISTRY_PASSWORD
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

## <a name="export-container-group-to-yaml"></a>Exportieren der Containergruppe nach YAML

Mit dem Azure CLI-Befehl [az container export][az-container-export] können Sie die Konfiguration einer vorhandenen Containergruppe in eine YAML-Datei exportieren.

Durch den Export können Sie die Konfiguration einer Containergruppe beibehalten und diese in der Versionskontrolle für „Konfiguration als Code“ speichern. Oder Sie können die exportierte Datei als Ausgangspunkt bei der Entwicklung einer neuen Konfiguration in YAML verwenden.

Exportieren Sie die Konfiguration für die schon erstellte Containergruppe mithilfe des folgenden Befehls [az container export][az-container-export].

```azurecli-interactive
az container export --resource-group myResourceGroup --name myContainerGroup --file deployed-aci.yaml
```

Es wird keine Ausgabe angezeigt, wenn der Befehl erfolgreich ist. Um das Ergebnis zu sehen, können Sie jedoch den Inhalt der Datei aufrufen. Beispielsweise die ersten Zeilen mit `head`:

```console
$ head deployed-aci.yaml
additional_properties: {}
apiVersion: '2018-06-01'
location: eastus
name: myContainerGroup
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      environmentVariables: []
      image: mcr.microsoft.com/azuredocs/aci-helloworld:latest
```

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel behandelt die für die Bereitstellung einer Azure-Instanz mit mehreren Containern erforderlichen Schritte. Ein umfassende Beschreibung von Azure Container Instances, auch zur Verwendung einer privaten Azure-Containerregistrierung, finden Sie im Azure Container Instances-Tutorial.

> [!div class="nextstepaction"]
> [Azure Container Instances-Tutorial][aci-tutorial]

<!-- LINKS - External -->
[cli-issue-6525]: https://github.com/Azure/azure-cli/issues/6525

<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-create]: /cli/azure/container#az-container-create
[az-container-export]: /cli/azure/container#az-container-export
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
