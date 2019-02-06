---
title: Verwenden der Autoskalierung für Cluster in Azure Kubernetes Service (AKS)
description: In diesem Artikel erfahren Sie, wie Sie Ihren Cluster mithilfe der Autoskalierung automatisch zur Erfüllung der Anwendungsanforderungen in einem Azure Kubernetes Service-Cluster (AKS) skalieren.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 01/29/2019
ms.author: iainfou
ms.openlocfilehash: 6b2302e69c9412170b55df4bfd8c1df5a9f75ef3
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55479138"
---
# <a name="automatically-scale-a-cluster-to-meet-application-demands-on-azure-kubernetes-service-aks"></a>Automatisches Skalieren eines Clusters zur Erfüllung von Anwendungsanforderungen in Azure Kubernetes Service (AKS)

Um die Anwendungsanforderungen in Azure Kubernetes Service (AKS) zu erfüllen, müssen Sie möglicherweise die Anzahl von Knoten anpassen, die Ihre Workloads ausführen. Die Komponente für die automatische Clusterskalierung kann auf Pods in Ihrem Cluster überprüfen, die aufgrund von Ressourceneinschränkungen nicht geplant werden können. Sobald Probleme erkannt werden, wird die Anzahl von Knoten erhöht, um den Anwendungsanforderungen gerecht zu werden. Die Knoten werden außerdem regelmäßig auf einen Mangel an ausgeführten Pods überprüft, und bei Bedarf wird die Anzahl von Knoten verringert. Diese Fähigkeit zur automatischen Erhöhung oder Verringerung der Knotenanzahl in Ihrem AKS-Cluster ermöglicht es Ihnen, einen effizienten, kostengünstigen Cluster zu betreiben.

In diesem Artikel wird gezeigt, wie Sie die Autoskalierung für Cluster in einem AKS-Cluster aktivieren und verwalten.

> [!IMPORTANT]
> Diese Funktion steht derzeit als Vorschau zur Verfügung. Wenn Sie Vorschauversionen nutzen möchten, müssen Sie die [zusätzlichen Nutzungsbedingungen][terms-of-use] akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.

## <a name="before-you-begin"></a>Voraussetzungen

Für den Artikel müssen Sie mindestens Version 2.0.55 der Azure-Befehlszeilenschnittstelle (Azure CLI) ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0][azure-cli-install] Informationen dazu.

AKS-Cluster mit Unterstützung der Autoskalierung für Cluster müssen VM-Skalierungsgruppen verwenden und Kubernetes-Version *1.12.4* oder höher ausführen. Die Unterstützung von Skalierungsgruppen befindet sich in der Vorschau. Um dieses Feature zu aktivieren und Cluster mit Verwendung von Skalierungsgruppen zu erstellen, installieren Sie die Azure CLI-Erweiterung *aks-preview* mit dem Befehl [az extension add][az-extension-add], wie im folgenden Beispiel gezeigt:

```azurecli-interactive
az extension add --name aks-preview
```

> [!NOTE]
> Wenn Sie die Erweiterung *aks-preview* installieren, verwendet jeder erstellte AKS-Cluster das Vorschaubereitstellungsmodell für Skalierungsgruppen. Wenn Sie dieses Feature nicht nutzen und stattdessen reguläre, vollständig unterstützte Cluster verwenden möchten, entfernen Sie die Erweiterung mithilfe von `az extension remove --name aks-preview`.

## <a name="about-the-cluster-autoscaler"></a>Grundlegendes zur Autoskalierung für Cluster

Für eine Anpassung an sich ändernde Anwendungsanforderungen, z.B. zwischen Arbeitstag und Abend oder an einem Wochenende, benötigen Cluster oft eine Möglichkeit zur automatischen Skalierung. AKS-Cluster können auf zwei Arten skaliert werden:

* Die **Autoskalierung für Cluster** überprüft auf Pods, die aufgrund von Ressourceneinschränkungen nicht auf Knoten geplant werden können. Der Cluster erhöht in diesem Fall automatisch die Anzahl von Knoten.
* Bei der **horizontalen automatischen Podskalierung** überwacht der Metrikserver in einem Kubernetes-Cluster die Ressourcenanforderungen der Pods. Wenn ein Dienst mehr Ressourcen benötigt, wird die Anzahl von Pods automatisch erhöht, um den Bedarf zu decken.

![Die Autoskalierung für Cluster und die horizontale automatische Podskalierung arbeiten häufig zusammen, um die Anwendungsanforderungen zu erfüllen.](media/autoscaler/cluster-autoscaler.png)

Sowohl über die horizontale automatische Podskalierung als auch über die Autoskalierung für Cluster kann bei Bedarf die Anzahl von Pods und Knoten verringert werden. Die Autoskalierung für Cluster verringert die Anzahl von Knoten, wenn für einen längeren Zeitraum Kapazität ungenutzt bleibt. Pods auf einem Knoten, der durch die Autoskalierung für Cluster entfernt werden soll, werden sicher an anderer Stelle im Cluster geplant. Die Autoskalierung für Cluster kann Pods möglicherweise nicht zentral herunterskalieren, wenn ein Verschieben nicht möglich ist. Dies kann in folgenden Situationen der Fall sein:

* Ein Pod wird direkt erstellt und nicht von einem Controllerobjekt wie z.B. einer Bereitstellungs- oder Replikatgruppe unterstützt.
* Ein Budget für die Unterbrechung von Pods (Pod Disruption Budget, PDB) ist zu restriktiv und lässt nicht zu, dass die Anzahl von Pods unter einen bestimmten Schwellenwert fällt.
* Ein Pod verwendet Knotenselektoren oder Anti-Affinität, die nicht berücksichtigt werden können, wenn der Pod auf einem anderen Knoten geplant wird.

Weitere Informationen dazu, warum ein zentrales Herunterskalieren über die Autoskalierung für Cluster möglicherweise nicht möglich ist, finden Sie unter [What types of pods can prevent the cluster autoscaler from removing a node?][autoscaler-scaledown] (Welche Podtypen können das Entfernen eines Knotens durch die Autoskalierung für Cluster verhindern?).

Die Autoskalierung für Cluster verwendet Startparameter, um z.B. Zeitintervalle zwischen Skalierungsereignissen und Ressourcenschwellenwerte zu steuern. Diese Parameter werden über die Azure-Plattform definiert und können aktuell nicht von Ihnen angepasst werden. Weitere Informationen zu den von der Autoskalierung für Cluster verwendeten Parametern finden Sie unter [What are the cluster autoscaler parameters?][autoscaler-parameters] (Welche Parameter verwendet die Autoskalierung für Cluster?).

Die zwei Methoden für die automatische Skalierung können zusammenarbeiten und werden häufig gemeinsam in einem Cluster bereitgestellt. In Kombination konzentriert sich die horizontale automatische Podskalierung auf die Anzahl von Pods, die zum Erfüllen der Anwendungsanforderungen benötigt werden. Die Autoskalierung für Cluster konzentriert sich auf die Anzahl von Knoten, die zum Unterstützen der geplanten Pods erforderlich sind.

> [!NOTE]
> Bei Verwendung der Autoskalierung für Cluster ist die manuelle Skalierung deaktiviert. Überlassen Sie der Autoskalierung für Cluster das Bestimmen der erforderlichen Anzahl von Knoten. Wenn Sie Ihren Cluster manuell skalieren möchten, [deaktivieren Sie die Autoskalierung für Cluster](#disable-the-cluster-autoscaler).

## <a name="create-an-aks-cluster-and-enable-the-cluster-autoscaler"></a>Erstellen eines AKS-Clusters und Aktivieren der Autoskalierung für Cluster

Wenn Sie einen AKS-Cluster erstellen müssen, verwenden Sie den Befehl [az aks create][az-aks-create]. Geben Sie eine *--kubernetes-version* an, die der mindestens erforderlichen Version entspricht oder diese übersteigt, wie im vorstehenden Abschnitt [Bevor Sie beginnen](#before-you-begin) beschrieben. Um Autoskalierung für Cluster zu aktivieren und zu konfigurieren, verwenden Sie den Parameter *--enable-cluster-autoscaler* und geben einen *--min-count*- und einen *--max-count*-Wert für die Knoten an.

Im folgenden Beispiel wird ein AKS-Cluster erstellt, in dem VM-Skalierungsgruppen und die Autoskalierung für Cluster aktiviert sind. Darüber hinaus wird ein Mindestwert von *1* Knoten und ein Höchstwert von *3* Knoten festgelegt:

```azurecli-interactive
# First create a resource group
az group create --name myResourceGroup --location canadaeast

# Now create the AKS cluster and enable the cluster autoscaler
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --kubernetes-version 1.12.4 \
  --node-count 1 \
  --enable-vmss \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

Die Erstellung des Clusters und die Konfiguration der Einstellungen für die Autoskalierung für Cluster dauert einige Minuten.

### <a name="enable-the-cluster-autoscaler-on-an-existing-aks-cluster"></a>Aktivieren der Autoskalierung für Cluster in einem vorhandenen AKS-Cluster

Sie können die Autoskalierung für Cluster für einen vorhandenen AKS-Cluster aktivieren, der die im vorstehenden Abschnitt [Voraussetzungen](#before-you-begin) beschriebenen Anforderungen erfüllt. Verwenden Sie den Befehl [az aks update][az-aks-update], und aktivieren Sie die Option *--enable-cluster-autoscaler*. Geben Sie anschließend einen Wert für *--min-count* und *--max-count* für die Knoten an. Das folgende Beispiel aktiviert die Autoskalierung für Cluster auf einem vorhandenen Cluster, der mindestens *1* und höchstens *3* Knoten verwendet:

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

Wenn die Mindestanzahl von Knoten größer ist als die vorhandene Anzahl von Knoten im Cluster, dauert es einige Minuten, bis die zusätzlichen Knoten angelegt sind.

## <a name="change-the-cluster-autoscaler-settings"></a>Ändern der Einstellungen zur Autoskalierung für Cluster

Im vorherigen Schritt zum Erstellen oder Aktualisieren eines vorhandenen AKS-Clusters wurde die Mindestanzahl von Knoten zur Autoskalierung für Cluster auf *1* und die maximale Knotenanzahl auf *3* festgelegt. Wenn sich die Anforderungen Ihrer Anwendung ändern, müssen Sie möglicherweise die Knotenanzahl für die Autoskalierung für Cluster anpassen.

Um die Knotenanzahl zu ändern, verwenden Sie den Befehl [az aks update][az-aks-update] und legen einen Mindest- und einen Höchstwert fest. Im folgenden Beispiel wird *--min-count* auf *1* und *--max-count* auf *5* festgelegt:

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --update-cluster-autoscaler \
  --min-count 1 \
  --max-count 5
```

> [!NOTE]
> Während der Vorschau können Sie keine höhere Mindestanzahl von Knoten einstellen, als aktuell für den Cluster festgelegt ist. Wenn beispielsweise die Mindestanzahl von Knoten aktuell auf *1* festgelegt ist, können Sie die Mindestanzahl nicht auf *3* aktualisieren.

Überwachen Sie die Leistung Ihrer Anwendungen und Dienste, und passen Sie die Anzahl von Knoten zur Autoskalierung für Cluster an die erforderliche Leistung an.

## <a name="disable-the-cluster-autoscaler"></a>Deaktivieren der Autoskalierung für Cluster

Wenn Sie Autoskalierung für Cluster nicht länger verwenden möchten, können Sie sie mit dem Befehl [az aks update][az-aks-update] deaktivieren. Beim Deaktivieren der Autoskalierung für Cluster werden keine Knoten entfernt.

Um die Autoskalierung für Cluster zu entfernen, geben Sie den Parameter *--disable-cluster-autoscaler* an, wie im folgenden Beispiel gezeigt:

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --disable-cluster-autoscaler
```

Sie können Ihren Cluster mithilfe des Befehls [az aks scale][az-aks-scale] manuell skalieren. Wenn Sie die horizontale automatische Podskalierung verwenden, wird dieses Feature nach dem Deaktivieren der Autoskalierung für Cluster weiterhin ausgeführt, aber Pods können möglicherweise nicht geplant werden, wenn alle Knotenressourcen verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde gezeigt, wie Sie die Anzahl von AKS-Knoten automatisch skalieren können. Außerdem können Sie mit der horizontalen automatischen Podskalierung automatisch die Anzahl von Pods anpassen, auf denen Ihre Anwendung ausgeführt wird. Die Schritte zum Verwenden der horizontalen automatischen Podskalierung finden Sie unter [Skalieren von Anwendungen in Azure Kubernetes Service (AKS)][aks-scale-apps].

<!-- LINKS - internal -->
[aks-upgrade]: upgrade-cluster.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-extension-add]: /cli/azure/extension#az-extension-add
[aks-scale-apps]: tutorial-kubernetes-scale.md
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-scale]: /cli/azure/aks#az-aks-scale

<!-- LINKS - external -->
[az-aks-update]: https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[autoscaler-scaledown]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-types-of-pods-can-prevent-ca-from-removing-a-node
[autoscaler-parameters]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-are-the-parameters-to-ca
