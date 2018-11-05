---
title: Verwenden von GPUs in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie GPUs für Hochleistungscompute- oder grafikintensive Workloads in Azure Kubernetes Service (AKS) verwendet werden können.
services: container-service
author: lachie83
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 10/25/2018
ms.author: laevenso
ms.openlocfilehash: 683abd9bad93bff51bea84c8081d2b8f9d300cd4
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50419247"
---
# <a name="use-gpus-for-compute-intensive-workloads-on-azure-kubernetes-service-aks"></a>Verwenden von GPUs für computeintensive Workloads in Azure Kubernetes Service (AKS)

GPUs (Graphical Processing Units) werden häufig für computeintensive Workloads (also etwa für grafikintensive und visualisierungsorientierte Workloads) verwendet. AKS unterstützt die Erstellung von GPU-fähigen Knotenpools, um diese computrintensiven Workloads in Kubernetes auszuführen. Weitere Informationen zu verfügbaren GPU-fähigen virtuellen Computern finden Sie unter [GPU-optimierte VM-Größen in Azure][gpu-skus]. Für die AKS-Knoten empfehlen wir eine Mindestgröße von *Standard_NC6*.

> [!NOTE]
> GPU-fähige virtuelle Computer verfügen über spezielle Hardware, für die höhere Preise gelten und die möglicherweise nicht in allen Regionen verfügbar ist. Weitere Informationen finden Sie in der [Preisübersicht][azure-pricing] sowie unter [regionale Verfügbarkeit][azure-availability].

## <a name="before-you-begin"></a>Voraussetzungen

Es wird vorausgesetzt, dass Sie über einen AKS-Cluster mit Knoten verfügen, die AKS unterstützen. Ihr AKS-Cluster muss Kubernetes 1.10 oder höher ausführen. Wenn Sie einen AKS-Cluster benötigen, der diese Anforderungen erfüllt, lesen Sie den ersten Abschnitt dieses Artikels zum [Erstellen eines AKS-Clusters](#create-an-aks-cluster).

Außerdem muss die Version 2.0.49 oder höher der Azure CLI installiert und konfiguriert sein. Führen Sie  `az --version` aus, um die Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie weitere Informationen unter  [Installieren der Azure CLI][install-azure-cli].

## <a name="create-an-aks-cluster"></a>Erstellen eines AKS-Clusters

Wenn Sie einen AKS-Cluster benötigen, der die Mindestanforderungen erfüllt (GPU-fähiger Knoten und Kubernetes Version 1.10 oder höher), führen Sie die folgenden Schritte aus. Wenn Sie bereits über einen AKS-Cluster verfügen, der diese Anforderungen erfüllt, fahren Sie mit dem nächsten Abschnitt fort.

Erstellen Sie zunächst mit dem Befehl [az group create][az-group-create] eine Ressourcengruppe für den Cluster. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* in der Region *eastus* erstellt:

```azurecli
az group create --name myResourceGroup --location eastus
```

Erstellen Sie nun mit dem Befehl [az aks create][az-aks-create] einen AKS-Cluster. Das folgende Beispiel erstellt einen Cluster mit einem einzelnen Knoten der Größe `Standard_NC6` und führt Kubernetes Version 1.10.8 aus:

```azurecli
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-vm-size Standard_NC6 \
    --node-count 1 \
    --kubernetes-version 1.10.8
```

Rufen Sie die Anmeldeinformationen für Ihren AKS-Cluster mit dem Befehl [az aks get-credentials][az-aks-get-credentials] ab:

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="confirm-that-gpus-are-schedulable"></a>Bestätigen, dass GPUs planbar sind

Nachdem Ihr AKS-Cluster erstellt wurde, bestätigen Sie, dass GPUs in Kubernetes planbar sind. Listen Sie zunächst die Knoten in Ihrem Cluster mit dem Befehl [kubectl get nodes][kubectl-get] auf:

```
$ kubectl get nodes

NAME                       STATUS   ROLES   AGE   VERSION
aks-nodepool1-18821093-0   Ready    agent   6m    v1.10.8
```

Verwenden Sie nun den Befehl [kubectl describe node][kubectl-describe], um zu bestätigen, dass die GPUs planbar sind. Unter dem Abschnitt *Capacity* (Kapazität) sollte die GPU als `nvidia.com/gpu:  1` aufgelistet werden. Wenn die GPUs nicht angezeigt werden, finden Sie weitere Informationen im Abschnitt [Problembehandlung der GPU-Verfügbarkeit](#troubleshoot-gpu-availability).

Das folgende verkürzte Beispiel zeigt, dass eine GPU für den Knoten *aks-nodepool1-18821093-0* verfügbar ist:

```
$ kubectl describe node aks-nodepool1-18821093-0

Name:               aks-nodepool1-18821093-0
Roles:              agent
Labels:             accelerator=nvidia

[...]

Capacity:
 cpu:                6
 ephemeral-storage:  30428648Ki
 hugepages-1Gi:      0
 hugepages-2Mi:      0
 memory:             57713824Ki
 nvidia.com/gpu:     1
 pods:               110
Allocatable:
 cpu:                5940m
 ephemeral-storage:  28043041951
 hugepages-1Gi:      0
 hugepages-2Mi:      0
 memory:             53417120Ki
 nvidia.com/gpu:     1
 pods:               110
System Info:
 Machine ID:                 688e083d19554d4a9563bd138f4ca98b
 System UUID:                08162568-B987-A84D-8865-98D6EFC64B32
 Boot ID:                    7b440249-8a96-42eb-950f-08c9a3c530b7
 Kernel Version:             4.15.0-1023-azure
 OS Image:                   Ubuntu 16.04.5 LTS
 Operating System:           linux
 Architecture:               amd64
 Container Runtime Version:  docker://1.13.1
 Kubelet Version:            v1.10.8
 Kube-Proxy Version:         v1.10.8
PodCIDR:                     10.244.0.0/24
ProviderID:                  azure:///subscriptions/19da35d3-9a1a-4f3b-9b9c-3c56ef409565/resourceGroups/MC_myGPUCluster_myGPUCluster_eastus/providers/Microsoft.Compute/virtualMachines/aks-nodepool1-18821093-0
Non-terminated Pods:         (9 in total)
  Namespace                  Name                                    CPU Requests  CPU Limits  Memory Requests  Memory Limits
  ---------                  ----                                    ------------  ----------  ---------------  -------------
  gpu-resources              nvidia-device-plugin-9cfcf              0 (0%)        0 (0%)      0 (0%)           0 (0%)

[...]
```

## <a name="run-a-gpu-enabled-workload"></a>Ausführen einer GPU-fähigen Workload

Um die GPU in Aktion zu sehen, planen Sie eine GPU-fähige Workload mit der entsprechenden Ressourcenanforderung. In diesem Beispiel wird ein [TensorFlow](https://www.tensorflow.org/versions/r1.1/get_started/mnist/beginners)-Auftrag für das [MNIST-Dataset](http://yann.lecun.com/exdb/mnist/) ausgeführt.

Erstellen Sie eine Datei mit dem Namen *samples-tf-mnist-demo.yaml*, und fügen Sie das folgende YAML-Manifest ein. Das folgende Auftragsmanifest enthält ein Ressourcenlimit von `nvidia.com/gpu: 1`:

> [!NOTE]
> Wenn Sie beim Aufruf von Treibern einen Versionsfehler erhalten (z.B., dass die CUDA-Treiberversion für die CUDA-Laufzeitversion nicht ausreicht), überprüfen Sie das nVidia-Treibermatrix-Kompatibilitätsdiagramm: [https://docs.nvidia.com/deploy/cuda-compatibility/index.html](https://docs.nvidia.com/deploy/cuda-compatibility/index.html)

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  labels:
    app: samples-tf-mnist-demo
  name: samples-tf-mnist-demo
spec:
  template:
    metadata:
      labels:
        app: samples-tf-mnist-demo
    spec:
      containers:
      - name: samples-tf-mnist-demo
        image: microsoft/samples-tf-mnist-demo:gpu
        args: ["--max_steps", "500"]
        imagePullPolicy: IfNotPresent
        resources:
          limits:
           nvidia.com/gpu: 1
      restartPolicy: OnFailure
```

Führen Sie den Auftrag mithilfe des Befehls [kubectl apply][kubectl-apply] aus. Dieser Befehl analysiert die Manifestdatei und erstellt die definierten Kubernetes-Objekte:

```console
kubectl apply -f samples-tf-mnist-demo.yaml
```

## <a name="view-the-status-and-output-of-the-gpu-enabled-workload"></a>Anzeigen des Status und der Ausgabe der GPU-fähigen Workload

Überwachen Sie den Status des Auftrags mithilfe des Befehls [kubectl get jobs][kubectl-get] mit dem Argument `--watch`. Es kann einige Minuten dauern, um zunächst das Image zu pullen und das Dataset zu verarbeiten. Wenn die Spalte *ABSCHLÜSSE* die Angabe *1/1* anzeigt, wurde der Auftrag erfolgreich abgeschlossen:

```
$ kubectl get jobs samples-tf-mnist-demo --watch

NAME                    COMPLETIONS   DURATION   AGE

samples-tf-mnist-demo   0/1           3m29s      3m29s
samples-tf-mnist-demo   1/1   3m10s   3m36s
```

Um die Ausgabe der GPU-fähigen Workload anzuzeigen, rufen Sie zunächst den Namen der Pods mit dem Befehl [kubectl get pods][kubectl-get] auf:

```
$ kubectl get pods --selector app=samples-tf-mnist-demo

NAME                          READY   STATUS      RESTARTS   AGE
samples-tf-mnist-demo-smnr6   0/1     Completed   0          3m
```

Nun verwenden Sie den Befehl [kubectl logs][kubectl-logs], um die Podprotokolle anzuzeigen. Die folgenden Beispielpodprotokolle bestätigen, dass das entsprechende GPU-Gerät ermittelt wurde: `Tesla K80`. Geben Sie den Namen für Ihren eigenen Pod an:

```
$ kubectl logs samples-tf-mnist-demo-smnr6

2018-10-25 18:31:10.155010: I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX AVX2 FMA
2018-10-25 18:31:10.305937: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1030] Found device 0 with properties:
name: Tesla K80 major: 3 minor: 7 memoryClockRate(GHz): 0.8235
pciBusID: ccb6:00:00.0
totalMemory: 11.92GiB freeMemory: 11.85GiB
2018-10-25 18:31:10.305981: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1120] Creating TensorFlow device (/device:GPU:0) -> (device: 0, name: Tesla K80, pci bus id: ccb6:00:00.0, compute capability: 3.7)
2018-10-25 18:31:14.941723: I tensorflow/stream_executor/dso_loader.cc:139] successfully opened CUDA library libcupti.so.8.0 locally
Successfully downloaded train-images-idx3-ubyte.gz 9912422 bytes.
Extracting /tmp/tensorflow/input_data/train-images-idx3-ubyte.gz
Successfully downloaded train-labels-idx1-ubyte.gz 28881 bytes.
Extracting /tmp/tensorflow/input_data/train-labels-idx1-ubyte.gz
Successfully downloaded t10k-images-idx3-ubyte.gz 1648877 bytes.
Extracting /tmp/tensorflow/input_data/t10k-images-idx3-ubyte.gz
Successfully downloaded t10k-labels-idx1-ubyte.gz 4542 bytes.
Extracting /tmp/tensorflow/input_data/t10k-labels-idx1-ubyte.gz
Accuracy at step 0: 0.097
Accuracy at step 10: 0.6993
Accuracy at step 20: 0.8208
Accuracy at step 30: 0.8594
Accuracy at step 40: 0.8685
Accuracy at step 50: 0.8864
Accuracy at step 60: 0.901
Accuracy at step 70: 0.905
Accuracy at step 80: 0.9103
Accuracy at step 90: 0.9126
Adding run metadata for 99
Accuracy at step 100: 0.9176
Accuracy at step 110: 0.9149
Accuracy at step 120: 0.9187
Accuracy at step 130: 0.9253
Accuracy at step 140: 0.9252
Accuracy at step 150: 0.9266
Accuracy at step 160: 0.9255
Accuracy at step 170: 0.9267
Accuracy at step 180: 0.9257
Accuracy at step 190: 0.9309
Adding run metadata for 199
Accuracy at step 200: 0.9272
Accuracy at step 210: 0.9321
Accuracy at step 220: 0.9343
Accuracy at step 230: 0.9388
Accuracy at step 240: 0.9408
Accuracy at step 250: 0.9394
Accuracy at step 260: 0.9412
Accuracy at step 270: 0.9422
Accuracy at step 280: 0.9436
Accuracy at step 290: 0.9411
Adding run metadata for 299
Accuracy at step 300: 0.9426
Accuracy at step 310: 0.9466
Accuracy at step 320: 0.9458
Accuracy at step 330: 0.9407
Accuracy at step 340: 0.9445
Accuracy at step 350: 0.9486
Accuracy at step 360: 0.9475
Accuracy at step 370: 0.948
Accuracy at step 380: 0.9516
Accuracy at step 390: 0.9534
Adding run metadata for 399
Accuracy at step 400: 0.9501
Accuracy at step 410: 0.9552
Accuracy at step 420: 0.9535
Accuracy at step 430: 0.9545
Accuracy at step 440: 0.9533
Accuracy at step 450: 0.9526
Accuracy at step 460: 0.9566
Accuracy at step 470: 0.9547
Accuracy at step 480: 0.9548
Accuracy at step 490: 0.9545
Adding run metadata for 499
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Verwenden Sie zum Entfernen der zugehörigen Kubernetes-Objekte, die in diesem Artikel erstellt wurden, den Befehl [kubectl delete job][kubectl delete] wie folgt:

```console
kubectl delete jobs samples-tf-mnist-demo
```

## <a name="troubleshoot-gpu-availability"></a>Problembehandlung der GPU-Verfügbarkeit

Wenn GPUs für Ihre Knoten nicht als verfügbar angezeigt werden, müssen Sie möglicherweise ein DaemonSet für das nVidia-Geräte-Plug-In bereitstellen. Diese DaemonSet führt einen Pod für jeden Knoten aus, um die erforderlichen Treiber für die GPUs bereitzustellen.

Erstellen Sie zunächst einen Namespace mit dem Befehl [kubectl create namespace][kubectl-create], z.B. *gpu-resources*:

```console
kubectl create namespace gpu-resources
```

Erstellen Sie eine Datei mit dem Namen *nvidia-device-plugin-ds.yaml*, und fügen Sie das folgende YAML-Manifest ein. Aktualisieren Sie das `image: nvidia/k8s-device-plugin:1.10` in der Mitte des Manifests, um es an Ihre Kubernetes-Version anzupassen. Wenn Ihr AKS-Cluster beispielsweise Kubernetes Version 1.11 ausführt, aktualisieren Sie das Tag auf `image: nvidia/k8s-device-plugin:1.11`.

```yaml
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  labels:
    kubernetes.io/cluster-service: "true"
  name: nvidia-device-plugin
  namespace: gpu-resources
spec:
  template:
    metadata:
      # Mark this pod as a critical add-on; when enabled, the critical add-on scheduler
      # reserves resources for critical add-on pods so that they can be rescheduled after
      # a failure.  This annotation works in tandem with the toleration below.
      annotations:
        scheduler.alpha.kubernetes.io/critical-pod: ""
      labels:
        name: nvidia-device-plugin-ds
    spec:
      tolerations:
      # Allow this pod to be rescheduled while the node is in "critical add-ons only" mode.
      # This, along with the annotation above marks this pod as a critical add-on.
      - key: CriticalAddonsOnly
        operator: Exists
      containers:
      - image: nvidia/k8s-device-plugin:1.10 # Update this tag to match your Kubernetes version
        name: nvidia-device-plugin-ctr
        securityContext:
          allowPrivilegeEscalation: false
          capabilities:
            drop: ["ALL"]
        volumeMounts:
          - name: device-plugin
            mountPath: /var/lib/kubelet/device-plugins
      volumes:
        - name: device-plugin
          hostPath:
            path: /var/lib/kubelet/device-plugins
      nodeSelector:
        beta.kubernetes.io/os: linux
        accelerator: nvidia
```

Verwenden Sie nun den Befehl [kubectl apply][kubectl-apply], um das DaemonSet zu erstellen:

```
$ kubectl apply -f nvidia-device-plugin-ds.yaml

daemonset "nvidia-device-plugin" created
```

Führen Sie den Befehl [kubectl describe node][kubectl-describe] erneut aus, um sich zu vergewissern, dass die GPU nun für den Knoten verfügbar ist.

## <a name="next-steps"></a>Nächste Schritte

Um Apache Spark-Aufträge auszuführen, lesen Sie [Ausführen von Apache Spark-Aufträgen in AKS][aks-spark].

Weitere Informationen zum Ausführen von Workloads für Machine Learning (ML) in Kubernetes finden Sie unter [Kubeflow-Labs][kubeflow-labs].

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubeflow-labs]: https://github.com/Azure/kubeflow-labs
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[kubectl delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[azure-pricing]: https://azure.microsoft.com/pricing/
[azure-availability]: https://azure.microsoft.com/global-infrastructure/services/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[aks-spark]: spark-job.md
[gpu-skus]: ../virtual-machines/linux/sizes-gpu.md
[install-azure-cli]: /cli/azure/install-azure-cli
