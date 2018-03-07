---
title: Zugreifen per SSH auf Azure Container Service-Clusterknoten (AKS-Clusterknoten)
description: Erstellen einer SSH-Verbindung mit einem Azure Container Service-Clusterknoten (AKS-Clusterknoten)
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/28/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 00affc3d1c02c477826261aeac6e092934037e81
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="ssh-into-azure-container-service-aks-cluster-nodes"></a>Zugreifen per SSH auf Azure Container Service-Clusterknoten (AKS-Clusterknoten)

Gelegentlich müssen Sie auf einen AKS-Knoten (Azure Container Service) zugreifen, um Wartungsarbeiten, Protokollsammlungen oder andere Vorgänge zur Problembehandlung auszuführen. AKS-Knoten werden nicht im Internet verfügbar gemacht. Führen Sie die in diesem Dokument aufgeführten Schritte aus, um eine SSH-Verbindung mit einem AKS-Knoten zu erstellen.

## <a name="configure-ssh-access"></a>Konfigurieren des SSH-Zugriffs

 Um per SSH auf einen bestimmten Knoten zuzugreifen, wird ein Pod mit `hostNetwork`-Zugriff erstellt. Außerdem wird ein Dienst für den Pod-Zugriff erstellt. Diese Konfiguration ist privilegiert und sollte nach der Verwendung entfernt werden.

Erstellen Sie eine Datei mit dem Namen `aks-ssh.yaml`, und kopieren Sie sie in dieses Manifest. Aktualisieren Sie den Namen des Knotens mit dem Namen des AKS-Zielknotens.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: aks-ssh
spec:
  selector:
    app: aks-ssh
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 22
    targetPort: 22
---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: aks-ssh
  labels:
    app: aks-ssh
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aks-ssh
  template:
    metadata:
      labels:
        app: aks-ssh
    spec:
      containers:
      - name: alpine
        image: alpine:latest
        ports:
        - containerPort: 22
        command: ["/bin/sh", "-c", "--"]
        args: ["while true; do sleep 30; done;"]
      hostNetwork: true
      nodeName: aks-nodepool1-42032720-0
```

Führen Sie das Manifest aus, um den Pod und den Dienst zu erstellen.

```azurecli-interactive
$ kubectl apply -f aks-ssh.yaml
```

Rufen Sie die externe IP-Adresse des verfügbar gemachten Diensts ab. Es kann eine Minute dauern, bis die Konfiguration der IP-Adresse abgeschlossen ist. 

```azurecli-interactive
$ kubectl get service

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)        AGE
kubernetes         ClusterIP      10.0.0.1      <none>          443/TCP        1d
aks-ssh            LoadBalancer   10.0.51.173   13.92.154.191   22:31898/TCP   17m
```

Erstellen Sie die SSH-Verbindung. 

Der Standardbenutzername für einen AKS-Cluster lautet `azureuser`. Wenn dieses Konto zum Zeitpunkt der Clustererstellung geändert wurde, ersetzen Sie den richtigen Administratorbenutzernamen. 

Wenn sich Ihr Schlüssel nicht unter `~/ssh/id_rsa` befindet, geben Sie den richtigen Speicherort mit dem Argument `ssh -i` an.

```azurecli-interactive
$ ssh azureuser@13.92.154.191

Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.11.0-1016-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

48 packages can be updated.
0 updates are security updates.


*** System restart required ***
Last login: Tue Feb 27 20:10:38 2018 from 167.220.99.8
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@aks-nodepool1-42032720-0:~$
```

## <a name="remove-ssh-access"></a>Entfernen des SSH-Zugriffs

Wenn Sie fertig sind, löschen Sie den Pod und den Dienst für den SSH-Zugriff.

```azurecli-interactive
kubectl delete -f aks-ssh.yaml
```