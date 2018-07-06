---
title: Bereitstellen von Gruppen mit mehreren Containern in Azure Container Instances
description: Hier erfahren Sie, wie Sie eine Containergruppe mit mehreren Containern in Azure Container Instances bereitstellen.
services: container-instances
author: iainfoulds
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 06/08/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 6d337c9ed23ac9af884f4113b046a8e9756fd441
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37097103"
---
# <a name="deploy-a-container-group"></a>Bereitstellen einer Containergruppe

Azure Container Instances unterstützt die Bereitstellung von mehreren Containern auf einem einzelnen Host mit einer [Containergruppe](container-instances-container-groups.md). Dies ist nützlich, wenn eine Sidecar-Anwendung für die Protokollierung, die Überwachung oder irgendeine andere Konfiguration, bei der ein Dienst einen zweiten angefügten Prozess benötigt, erstellt wird.

Es gibt zwei Möglichkeiten zum Bereitstellen von Gruppen mit mehreren Containern über die Azure CLI:

* Resource Manager-Vorlagenbereitstellung (dieser Artikel)
* [YAML-Dateibereitstellung](container-instances-multi-container-yaml.md)

Wenn Sie bei der Bereitstellung von Containerinstanzen zusätzliche Azure-Dienstressourcen (z.B. eine Azure Files-Freigabe) bereitstellen müssen, wird die Bereitstellung mit einer Resource Manager-Vorlage empfohlen. Das YAML-Format ist präziser, daher wird die Bereitstellung mit einer YAML-Datei empfohlen, wenn Ihre Bereitstellung *nur* Containerinstanzen enthält.

> [!NOTE]
> Gruppen mit mehreren Containern sind aktuell auf Linux-Container beschränkt. Bis alle Features auch für Windows-Container verfügbar sind, finden Sie die aktuellen Plattformunterschiede unter [Kontingente und Regionsverfügbarkeit für Azure Container Instances](container-instances-quotas.md).

## <a name="configure-the-template"></a>Konfigurieren der Vorlage

In den Abschnitten in diesem Artikel wird das Ausführen einer einfachen Sidecar-Konfiguration mit mehreren Containern durch das Bereitstellen einer Azure Resource Manager-Vorlage beschrieben.

Erstellen Sie zunächst eine Datei mit dem Namen `azuredeploy.json`, und fügen Sie dann den folgende JSON-Code ein.

Diese Resource Manager-Vorlage definiert eine Containergruppe mit zwei Containern, einer öffentlichen IP-Adresse und zwei verfügbar gemachten Ports. Der erste Container in der Gruppe führt eine Anwendung mit Internetverbindung aus. Der zweite Container, der Sidecar, stellt eine HTTP-Anforderung an die Hauptwebanwendung über das lokale Netzwerk der Gruppe.

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerGroupName": {
      "type": "string",
      "defaultValue": "myContainerGroup",
      "metadata": {
        "description": "Container Group name."
      }
    }
  },
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "microsoft/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",
    "container2image": "microsoft/aci-tutorial-sidecar"
  },
  "resources": [
    {
      "name": "[parameters('containerGroupName')]",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2018-04-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "containers": [
          {
            "name": "[variables('container1name')]",
            "properties": {
              "image": "[variables('container1image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "port": 80
                },
                {
                  "port": 8080
                }
              ]
            }
          },
          {
            "name": "[variables('container2name')]",
            "properties": {
              "image": "[variables('container2image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              }
            }
          }
        ],
        "osType": "Linux",
        "ipAddress": {
          "type": "Public",
          "ports": [
            {
              "protocol": "tcp",
              "port": "80"
            },
            {
                "protocol": "tcp",
                "port": "8080"
            }
          ]
        }
      }
    }
  ],
  "outputs": {
    "containerIPv4Address": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', parameters('containerGroupName'))).ipAddress.ip]"
    }
  }
}
```

Fügen Sie ein Objekt zum JSON-Dokument mit dem folgenden Format hinzu, um einen Container der privaten Imageregistrierung verwenden zu können. Eine Beispielimplementierung dieser Konfiguration finden Sie in der Dokumentation [Referenz zu ACI Resource Manager-Vorlagen][template-reference].

```JSON
"imageRegistryCredentials": [
  {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
  }
]
```

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Erstellen Sie mit dem Befehl [az group create][az-group-create] eine Ressourcengruppe.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Stellen Sie mit dem Befehl [az group deployment create][az-group-deployment-create] die Vorlage bereit.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file azuredeploy.json
```

Innerhalb weniger Sekunden sollten Sie eine erste Antwort von Azure erhalten.

## <a name="view-deployment-state"></a>Zusammenfassung der Bereitstellungen anzeigen

Um den Bereitstellungsstatus anzuzeigen, verwenden Sie den folgenden Befehl [az container show][az-container-show]:

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

Wenn Sie die laufende Anwendung anzeigen möchten, navigieren Sie zu ihrer IP-Adresse in Ihrem Browser. In dieser Beispielausgabe lautet die IP `52.168.26.124`:

```bash
Name              ResourceGroup    ProvisioningState    Image                                                           IP:ports               CPU/Memory       OsType    Location
----------------  ---------------  -------------------  --------------------------------------------------------------  ---------------------  ---------------  --------  ----------
myContainerGroup  myResourceGroup  Succeeded            microsoft/aci-helloworld:latest,microsoft/aci-tutorial-sidecar  52.168.26.124:80,8080  1.0 core/1.5 gb  Linux     westus
```

## <a name="view-logs"></a>Anzeigen von Protokollen

Zeigen Sie die Protokollausgabe eines Containers mit dem Befehl [az container logs][az-container-logs] an. Das `--container-name`-Argument gibt den Container an, aus dem Protokolle erhalten werden können. In diesem Beispiel wird der erste Container angegeben.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Ausgabe:

```bash
listening on port 80
::1 - - [09/Jan/2018:23:17:48 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [09/Jan/2018:23:17:51 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [09/Jan/2018:23:17:54 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Führen Sie den gleichen Befehl aus, und geben Sie den zweiten Containernamen an, um die Protokolle für den Sidecar-Container anzuzeigen.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-sidecar
```

Ausgabe:

```bash
Every 3s: curl -I http://localhost                          2018-01-09 23:25:11

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
Date: Tue, 09 Jan 2018 23:25:11 GMT
Connection: keep-alive
```

Wie Sie sehen können, erfolgt vom Sidecar in regelmäßigen Abständen eine HTTP-Anforderung an die Hauptwebanwendung über das lokale Netzwerk der Gruppe, um sicherzustellen, dass er ausgeführt wird. Dieses Sidecar-Beispiel kann erweitert werden, um eine Warnung auszulösen, wenn ein anderer HTTP-Antwortcode als 200 OK empfangen wird.

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel behandelt die für die Bereitstellung einer Azure-Instanz mit mehreren Containern erforderlichen Schritte. Ein umfassende Beschreibung von Azure Container Instances finden Sie im Azure Container Instances-Tutorial.

> [!div class="nextstepaction"]
> [Azure Container Instances-Tutorial][aci-tutorial]

<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-logs]: /cli/azure/container#az_container_logs
[az-container-show]: /cli/azure/container#az_container_show
[az-group-create]: /cli/azure/group#az_group_create
[az-group-deployment-create]: /cli/azure/group/deployment#az_group_deployment_create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
