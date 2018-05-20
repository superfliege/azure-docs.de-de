---
title: Verwenden von Azure-Datenträgern mit AKS
description: Verwenden von Azure-Datenträgern mit AKS
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/06/2018
ms.author: nepeters
ms.openlocfilehash: 858961db439b28a71d3475d2608073287e02f2fd
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
---
# <a name="persistent-volumes-with-azure-disks"></a>Persistente Volumes mit Azure-Datenträgern

Ein persistentes Volume stellt ein Speicherelement dar, das für die Verwendung in Kubernetes-Pods bereitgestellt wurde. Ein persistentes Volume kann von einem oder mehreren Pods verwendet und dynamisch oder statisch bereitgestellt werden. Weitere Informationen zu persistenten Kubernetes-Volumes finden Sie unter [Persistente Kubernetes-Volumes][kubernetes-volumes].

Dieses Dokument enthält Informationen zur Verwendung von persistenten Volumes mit Azure-Datenträgern in einem AKS-Cluster (Azure Kubernetes Service).

> [!NOTE]
> Ein Azure-Datenträger kann nur mit dem Zugriffsmodustyp „ReadWriteOnce“ eingebunden werden. Dadurch ist er nur für einen einzelnen AKS-Knoten verfügbar. Wenn Sie ein persistentes Volume für mehrere Knoten freigeben möchten, ziehen Sie die Verwendung von [Azure Files][azure-files-pvc] in Erwägung.

## <a name="built-in-storage-classes"></a>Integrierte Speicherklassen

Mit einer Speicherklasse wird festgelegt, wie eine Speichereinheit dynamisch in einem persistenten Volume erstellt wird. Weitere Informationen zu Kubernetes-Speicherklassen finden Sie unter [Kubernetes-Speicherklassen][kubernetes-storage-classes].

Jeder AKS-Cluster schließt zwei vorab erstellte Speicherklassen ein, die beide für die Arbeit mit Azure-Datenträgern konfiguriert sind. Die Speicherklasse `default` stellt einen Azure Standard-Datenträger bereit. Die Speicherklasse `managed-premium` stellt einen Azure Premium-Datenträger bereit. Wenn die AKS-Knoten in Ihrem Cluster Storage Premium verwenden, wählen Sie die `managed-premium`-Klasse aus.

Mit dem Befehl [kubectl get sc][kubectl-get] können Sie die vorab erstellten Speicherklassen anzeigen.

```console
NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

## <a name="create-persistent-volume-claim"></a>Erstellen eines Anspruchs auf ein persistentes Volume

Ein Anspruch auf ein persistentes Volume (Persistent Volume Claim, PVC) wird verwendet, um basierend auf einer Speicherklasse automatisch Speicher bereitzustellen. In diesem Fall kann ein PVC eine der zuvor erstellten Speicherklassen verwenden, um einen verwalteten Azure Standard- oder Premium-Datenträger zu erstellen.

Erstellen Sie eine Datei namens `azure-premimum.yaml`, und fügen Sie das folgende Manifest ein.

Beachten Sie, dass die Speicherklasse `managed-premium` in der Anmerkung angegeben wird und dass der Anspruch einen Datenträger der Größe `5GB` mit `ReadWriteOnce`-Zugriff anfordert.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
  annotations:
    volume.beta.kubernetes.io/storage-class: managed-premium
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

Erstellen Sie mit dem Befehl [kubectl apply][kubectl-apply] einen Anspruch auf ein persistentes Volume.

```azurecli-interactive
kubectl apply -f azure-premimum.yaml
```

## <a name="using-the-persistent-volume"></a>Verwenden des persistenten Volumes

Nachdem der Anspruch auf ein persistentes Volumes erstellt und der Datenträger erfolgreich bereitgestellt wurde, kann ein Pod mit Zugriff auf den Datenträger erstellt werden. Das folgende Manifest erstellt einen Pod, der den Anspruch auf das persistente Volume `azure-managed-disk` verwendet, um den Azure-Datenträger im Pfad `/mnt/azure` einzubinden.

Erstellen Sie eine Datei namens `azure-pvc-disk.yaml`, und fügen Sie das folgende Manifest ein.

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
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk
```

Verwenden Sie den Befehl [kubectl apply][kubectl-apply] zum Erstellen des Pods.

```azurecli-interactive
kubectl apply -f azure-pvc-disk.yaml
```

Sie verfügen nun über einen ausgeführten Pod mit Ihrem Azure-Datenträger, der im Verzeichnis `/mnt/azure` eingebunden wurde. Diese Konfiguration wird angezeigt, wenn Sie Ihren Pod über `kubectl describe pod mypod` auswerten.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über persistente Kubernetes-Volumes bei Verwendung von Azure-Datenträgern.

> [!div class="nextstepaction"]
> [Kubernetes-Plug-In für Azure-Datenträger][kubernetes-disk]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-disk]: https://kubernetes.io/docs/concepts/storage/storage-classes/#new-azure-disk-storage-class-starting-from-v172
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/premium-storage.md