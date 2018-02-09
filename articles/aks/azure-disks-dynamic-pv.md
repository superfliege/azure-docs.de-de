---
title: "Verwenden von Azure-Datenträgern mit AKS"
description: "Verwenden von Azure-Datenträgern mit AKS"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 1/25/2018
ms.author: nepeters
ms.openlocfilehash: e1f5b68d5d39dd846ebec525d1e83a6c0ef4971a
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2018
---
# <a name="persistent-volumes-with-azure-disks---dynamic-provisioning"></a>Persistente Volumes mit Azure-Datenträgern: dynamische Bereitstellung

Ein persistentes Volume stellt ein Speicherelement dar, das für die Verwendung in einem Kubernetes-Cluster bereitgestellt wurde. Ein persistentes Volume kann von einem oder mehreren Pods verwendet und dynamisch oder statisch bereitgestellt werden. Dieses Dokument beschreibt die dynamische Bereitstellung eines Azure-Datenträgers als persistentes Kubernetes-Volume in einem AKS-Cluster. 

Weitere Informationen zu persistenten Kubernetes-Volumes finden Sie unter [Persistente Kubernetes-Volumes][kubernetes-volumes].

## <a name="built-in-storage-classes"></a>Integrierte Speicherklassen

Eine Speicherklasse dient zum Definieren der Konfiguration eines dynamisch erstellten persistenten Volumes. Weitere Informationen zu Kubernetes-Speicherklassen finden Sie unter [Kubernetes-Speicherklassen][kubernetes-storage-classes].

Jeder AKS-Cluster schließt zwei vorab erstellte Speicherklassen ein, die beide für die Arbeit mit Azure-Datenträgern konfiguriert sind. Verwenden Sie den Befehl `kubectl get storageclass`, um sie anzuzeigen.

```console
NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

Wenn diese Speicherklassen Ihre Anforderungen erfüllen, müssen Sie keine neuen erstellen.

## <a name="create-storage-class"></a>Erstellen einer Speicherklasse

Wenn Sie eine neue Speicherklasse erstellen möchten, die für Azure-Datenträger konfiguriert ist, können Sie dazu das folgende Beispielmanifest verwenden. 

Der Wert `storageaccounttype` für `Standard_LRS` gibt an, dass ein Standarddatenträger erstellt wird. Dieser Wert kann in `Premium_LRS` geändert werden, um einen [Premium-Datenträger][premium-storage] zu erstellen. Um Premium-Datenträger zu verwenden, muss der virtuelle Computer des AKS-Knotens eine Größe aufweisen, die mit Premium-Datenträgern kompatibel ist. In [diesem Dokument][premium-storage] finden Sie eine Liste der kompatiblen Größen.

```yaml
apiVersion: storage.k8s.io/v1beta1
kind: StorageClass
metadata:
  name: azure-managed-disk
provisioner: kubernetes.io/azure-disk
parameters:
  kind: Managed
  storageaccounttype: Standard_LRS
```

## <a name="create-persistent-volume-claim"></a>Erstellen eines Anspruchs auf ein persistentes Volume

Ein Anspruch auf ein persistentes Volume verwendet das Speicherklassenobjekt, um eine Speicherkomponente dynamisch bereitzustellen. Wenn Sie einen Azure-Datenträger verwenden, wird der Datenträger in derselben Ressourcengruppe wie die AKS-Ressourcen erstellt.

Dieses Beispielmanifest erstellt einen persistenten Volumeanspruch mit der Speicherklasse `azure-managed-disk`, um einen Datenträger mit der Größe `5GB` und dem Zugriff `ReadWriteOnce` zu erstellen. Weitere Informationen zu PVC-Zugriffsmodi finden Sie unter [Zugriffsmodi][access-modes].

> [!NOTE]
> Ein Azure-Datenträger kann nur mit dem Zugriffsmodustyp „ReadWriteOnce“ eingebunden werden. Dadurch ist er nur für einen einzelnen AKS-Knoten verfügbar. Wenn Sie ein persistentes Volume für mehrere Knoten freigeben möchten, ziehen Sie die Verwendung von [Azure Files][azure-files-pvc] in Erwägung. 

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
  annotations:
    volume.beta.kubernetes.io/storage-class: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

## <a name="using-the-persistent-volume"></a>Verwenden des persistenten Volumes

Nachdem der Anspruch auf ein persistentes Volumes erstellt und der Datenträger erfolgreich bereitgestellt wurde, kann ein Pod mit Zugriff auf den Datenträger erstellt werden. Das folgende Manifest erstellt einen Pod, der den Anspruch auf das persistente Volume `azure-managed-disk` verwendet, um den Azure-Datenträger im Pfad `/var/www/html` einzubinden. 

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über persistente Kubernetes-Volumes bei Verwendung von Azure-Datenträgern.

> [!div class="nextstepaction"]
> [Kubernetes-Plug-In für Azure-Datenträger][kubernetes-disk]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubernetes-disk]: https://kubernetes.io/docs/concepts/storage/storage-classes/#new-azure-disk-storage-class-starting-from-v172
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/premium-storage.md