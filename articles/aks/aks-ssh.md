---
title: Zugreifen per SSH auf Azure Kubernetes Service-Clusterknoten (AKS)
description: Erstellen einer SSH-Verbindung mit einem Azure Kubernetes Service-Clusterknoten (AKS)
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/06/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 8f4c70814566a963d86c119044f9ed3ef3238483
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37099397"
---
# <a name="ssh-into-azure-kubernetes-service-aks-cluster-nodes"></a>Zugreifen per SSH auf Azure Kubernetes Service-Clusterknoten (AKS)

Gelegentlich müssen Sie auf einen AKS-Knoten (Azure Kubernetes Service) zugreifen, um Wartungsarbeiten, Protokollsammlungen oder andere Vorgänge zur Problembehandlung auszuführen. AKS-Knoten werden nicht im Internet verfügbar gemacht. Führen Sie die in diesem Dokument aufgeführten Schritte aus, um eine SSH-Verbindung mit einem AKS-Knoten zu erstellen.

## <a name="reset-ssh-keys"></a>Zurücksetzen der SSH-Schlüssel

Wenn Sie AKS ohne SSH-Schlüssel installiert haben oder keinen Zugriff auf die richtigen SSH-Schlüssel haben, können diese über das Azure-Portal zurückgesetzt werden.

Wechseln Sie zu Ihrem AKS-Cluster, wählen Sie einen AKS-Knoten (virtueller Computer) aus, und wählen Sie **Kennwort zurücksetzen** aus, um den öffentlichen SSH-Schlüssel zurückzusetzen.

![AKS-VM mit Schaltfläche „Kennwort zurücksetzen“](media/aks-ssh/reset-password.png)

Wählen Sie **Öffentlichen SSH-Schlüssel zurücksetzen** aus, geben Sie den Benutzernamen des AKS-Clusters ein, der standardmäßig **azueruser** lautet, und kopieren Sie ihn in einen öffentlichen SSH-Schlüssel. Wählen Sie anschließend die Option **Aktualisieren**.

![AKS-Portal-VM mit Schaltfläche „Kennwort zurücksetzen“](media/aks-ssh/reset-password-2.png)

Sobald der SSH-Schlüssel zurückgesetzt wurde, können Sie eine SSH-Verbindung mit dem entsprechenden privaten Schlüssel erstellen.

## <a name="get-aks-node-address"></a>Abrufen der AKS-Knotenadresse

Rufen Sie die IP-Adresse eines AKS-Clusterknotens mithilfe des Befehls `az vm list-ip-addresses` ab. Ersetzen Sie den Ressourcengruppennamen durch den Namen Ihrer AKS-Ressourcengruppe.

```console
$ az vm list-ip-addresses --resource-group MC_myAKSCluster_myAKSCluster_eastus -o table

VirtualMachine            PrivateIPAddresses
------------------------  --------------------
aks-nodepool1-42032720-0  10.240.0.6
aks-nodepool1-42032720-1  10.240.0.5
aks-nodepool1-42032720-2  10.240.0.4
```

## <a name="create-ssh-connection"></a>Erstellen einer SSH-Verbindung

Führen Sie das Containerimage `debian` aus, und fügen Sie eine Terminalsitzung daran an. Der Container kann dann zum Erstellen einer SSH-Sitzung mit einem beliebigen Knoten im AKS-Cluster verwendet werden.

```console
kubectl run -it --rm aks-ssh --image=debian
```

Installieren Sie einen SSH-Client im Container.

```console
apt-get update && apt-get install openssh-client -y
```

Öffnen Sie ein zweites Terminal, und listen Sie alle Pods auf, um den Namen des neu erstellten Pods abzurufen.

```console
$ kubectl get pods

NAME                       READY     STATUS    RESTARTS   AGE
aks-ssh-554b746bcf-kbwvf   1/1       Running   0          1m
```

Kopieren Sie den privaten SSH-Schlüssel in den Pod, und ersetzen Sie den Podnamen durch den tatsächlichen Wert.

```console
kubectl cp ~/.ssh/id_rsa aks-ssh-554b746bcf-kbwvf:/id_rsa
```

Aktualisieren Sie die Datei `id_rsa`, sodass sie schreibgeschützt ist.

```console
chmod 0600 id_rsa
```

Erstellen Sie jetzt eine SSH-Verbindung zum AKS-Knoten. Der Standardbenutzername für einen AKS-Cluster lautet `azureuser`. Wenn dieses Konto zum Zeitpunkt der Clustererstellung geändert wurde, ersetzen Sie den richtigen Administratorbenutzernamen.

```console
$ ssh -i id_rsa azureuser@10.240.0.6

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

Wenn Sie fertig sind, beenden Sie die SSH-Sitzung und danach die interaktive Containersitzung. Diese Aktion löscht den Pod, der für den SSH-Zugriff vom AKS-Cluster verwendet wurde.
