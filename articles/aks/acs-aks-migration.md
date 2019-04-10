---
title: Migration von Azure Container Service (ACS) zu Azure Kubernetes Service (AKS)
description: Migration von Azure Container Service (ACS) zu Azure Kubernetes Service (AKS)
services: container-service
author: noelbundick
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/13/2018
ms.author: nobun
ms.custom: mvc
ms.openlocfilehash: 910c96988ec0a8b8aa7b6ac8ce287c4fdc59e177
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58649967"
---
# <a name="migrating-from-azure-container-service-acs-to-azure-kubernetes-service-aks"></a>Migration von Azure Container Service (ACS) zu Azure Kubernetes Service (AKS)

Ziel dieses Dokuments ist es, Sie bei der erfolgreichen Planung und Durchführung einer Migration von Azure Container Service (ACS) zu Azure Kubernetes Service (AKS) zu unterstützen. Dieser Leitfaden erläutert die Unterschiede zwischen ACS und AKS, bietet eine Übersicht über den Migrationsprozess und soll Sie bei wichtigen Entscheidungen unterstützen.

## <a name="differences-between-acs-and-aks"></a>Unterschiede zwischen ACS und AKS

ACS und AKS unterscheiden sich in einigen wichtigen Bereichen, die sich auf die Migration auswirken. Vor einer Migration müssen Sie sich mit den folgenden Unterschieden vertraut machen und diese in Ihre Planung einbeziehen.

* AKS-Knoten verwenden [verwaltete Datenträger](../virtual-machines/windows/managed-disks-overview.md).
    * Nicht verwaltete Datenträger müssen möglicherweise konvertiert werden, bevor sie an AKS-Knoten angefügt werden können.
    * Benutzerdefinierte `StorageClass`-Objekte für Azure-Datenträger müssen von `unmanaged` in `managed` geändert werden.
    * Alle `PersistentVolumes` müssen `kind: Managed` verwenden.
* AKS unterstützt aktuell nur einen Agentpool.
* Windows Server-basierte Knoten befinden sich derzeit in der [privaten Vorschau](https://azure.microsoft.com/blog/kubernetes-on-azure/).
* Überprüfen Sie die Liste der von AKS [unterstützten Regionen](https://docs.microsoft.com/azure/aks/container-service-quotas).
* AKS ist ein verwalteter Dienst mit einer gehosteten Kubernetes-Steuerungsebene. Sie müssen Ihre Anwendungen möglicherweise ändern, wenn Sie die Konfiguration Ihrer ACS-Master zuvor geändert haben.

### <a name="differences-between-kubernetes-versions"></a>Unterschiede zwischen Kubernetes-Versionen

Wenn Sie zu einer neueren Version von Kubernetes migrieren (z.B.  von 1.7.x zu 1.9.x), gelten einige Änderungen für die k8s-API, die Ihre Aufmerksamkeit erfordern.

* [Migration einer ThirdPartyResource zu CustomResourceDefinition](https://kubernetes.io/docs/tasks/access-kubernetes-api/migrate-third-party-resource/)
* [Änderungen an der Workloads-API in den Versionen 1.8 und 1.9](https://kubernetes.io/docs/reference/workloads-18-19/).

## <a name="migration-considerations"></a>Überlegungen zur Migration

### <a name="agent-pools"></a>Agentpools

Wenngleich AKS die Kubernetes-Steuerungsebene verwaltet, müssen Sie dennoch die Größe und Anzahl von Knoten angeben, die Sie in Ihren neuen Cluster einschließen möchten. Wenn Sie eine 1:1-Zuordnung von ACS zu AKS erzielen möchten, müssen Sie die Informationen zu Ihren vorhandenen ACS-Knoten erfassen. Sie verwenden diese Daten, wenn Sie Ihren neuen AKS-Cluster erstellen.

Beispiel:

| NAME | Count | Größe des virtuellen Computers | Betriebssystem |
| --- | --- | --- | --- |
| agentpool0 | 3 | Standard_D8_v2 | Linux |
| agentpool1 | 1 | Standard_D2_v2 | Windows |

Da während der Migration zusätzliche VMs in Ihrem Abonnement bereitgestellt werden, müssen Sie überprüfen, ob Ihre Kontingente und Limits für diese Ressourcen ausreichen. Weitere Informationen erhalten Sie im Artikel zu [ Azure-Abonnement- und Dienstlimits](https://docs.microsoft.com/azure/azure-subscription-service-limits). Um Ihre aktuellen Kontingente zu überprüfen, wechseln Sie zum [Blatt „Abonnements“](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) im Azure-Portal, wählen Ihr Abonnement aus und klicken dann auf `Usage + quotas`.

### <a name="networking"></a>Netzwerk

Für komplexe Anwendungen führen Sie die Migration typischerweise nach und nach durch, statt alle Anwendungen gleichzeitig zu migrieren. Dies bedeutet, dass die alten und neuen Umgebungen möglicherweise über das Netzwerk kommunizieren müssen. Anwendungen, die zuvor möglicherweise `ClusterIP`-Dienste für die Kommunikation nutzen konnten, müssen als Typ `LoadBalancer` verfügbar gemacht und in geeigneter Weise geschützt werden.

Um die Migration durchzuführen, müssen Clients auf die neuen Dienste zeigen, die in AKS ausgeführt werden. Die empfohlene Methode zum Umleiten des Datenverkehrs besteht darin, DNS zu aktualisieren, damit auf den Load Balancer verwiesen wird, der vor Ihrem AKS-Cluster platziert ist.

### <a name="stateless-applications"></a>Zustandslose Anwendungen

Die Migration von zustandslosen Anwendung ist der einfachste Fall. Sie wenden Ihre YAML-Definitionen auf den neuen Cluster an und überprüfen, ob alles wie erwartet funktioniert. Anschließend leiten Sie den Datenverkehr um, damit Ihr neuer Cluster aktiviert wird.

### <a name="stateful-applications"></a>Zustandsbehaftete Anwendungen

Die Migration von zustandsbehafteten Anwendungen erfordert eine sorgfältige Planung, um einen Datenverlust oder eine unerwartete Downtime zu vermeiden.

#### <a name="highly-available-applications"></a>Hoch verfügbare Anwendungen

Einige zustandsbehaftete Anwendungen können in hoch verfügbaren Konfiguration bereitgestellt werden und können Daten replikatübergreifend kopieren. Wenn dies auf Ihre aktuelle Bereitstellung zutrifft, ist es unter Umständen möglich, ein neues Mitglied im neuen AKS-Cluster zu erstellen und die Migration mit minimaler Auswirkung auf die nachgelagerten Aufrufer zu migrieren. Für dieses Szenario gelten die folgenden allgemeinen Migrationsschritte:

1. Erstellen eines neuen sekundären Replikats in AKS
2. Warten auf die Datenreplikation
3. Failover, um das sekundäre Replikat als neues primäres Replikat festzulegen
4. Umleiten des Datenverkehrs an den AKS-Cluster

#### <a name="migrating-persistent-volumes"></a>Migration persistenter Volumes

Wenn Sie ein vorhandenes persistentes Volume zu AKS migrieren, müssen verschiedene Faktoren berücksichtigt werden. Im Allgemeinen müssen die folgenden Schritte ausgeführt werden:

1. (Optional) Stilllegen von Schreibvorgängen in die Anwendung (erfordert Downtime)
2. Erstellen von Momentaufnahmen der Datenträger
3. Erstellen neuer verwalteter Datenträger aus den Momentaufnahmen
4. Erstellen von persistenten Volumes in AKS
5. Aktualisieren der Podspezifikationen zur [Verwendung vorhandener Volumes](https://docs.microsoft.com/azure/aks/azure-disk-volume) anstelle von „PersistentVolumeClaims“ (statische Bereitstellung)
6. Bereitstellen der Anwendung in AKS
7. Überprüfen
8. Umleiten des Datenverkehrs an den AKS-Cluster

> **Wichtig**: Wenn Sie die Schreibvorgänge nicht stilllegen, müssen Sie Daten zur neuen Bereitstellung replizieren, weil ansonsten Daten fehlen, die seit dem Erstellen der Datenträgermomentaufnahme geschrieben wurden.

Es sind Open Source-Tools verfügbar, mit denen Sie verwaltete Datenträger erstellen und Volumes zwischen Kubernetes-Clustern migrieren können.

* [noelbundick/azure-cli-disk-extension](https://github.com/noelbundick/azure-cli-disk-copy-extension): Kopieren und Konvertieren von Datenträgern zwischen Ressourcengruppen und Azure-Regionen
* [yaron2/azure-kube-cli](https://github.com/yaron2/azure-kube-cli): Auflisten von ACS Kubernetes -Volumes und Migrieren der Volumes zu einem AKS-Cluster

#### <a name="azure-files"></a>Azure Files

Im Gegensatz zu Datenträgern können Azure Files-Instanzen gleichzeitig auf mehreren Hosts bereitgestellt werden. Sowohl in Azure als auch in Kubernetes ist es möglich, einen Pod in Ihrem AKS-Cluster zu erstellen, der weiterhin von Ihrem ACS-Cluster verwendet wird. Um einen Datenverlust und ein unerwartetes Verhalten zu vermeiden, müssen Sie sicherstellen, dass die beiden Cluster nicht gleichzeitig in dieselben Dateien schreiben.

Wenn Ihre Anwendung mehrere Replikate hosten kann, die auf dieselbe Dateifreigabe verweisen, können Sie die Schritte für die Migration zustandsloser Anwendungen befolgen und Ihre YAML-Definitionen auf Ihren neuen Cluster anwenden.

Andernfalls umfasst ein möglicher Ansatz für die Migration die folgenden Schritte:

1. Bereitstellen Ihrer Anwendung in AKS mit einer Replikatanzahl von 0
2. Skalieren der Anwendung in ACS auf 0 (erfordert Downtime)
3. Skalieren der Anwendung in AKS auf bis zu 1
4. Überprüfen
5. Umleiten des Datenverkehrs an den AKS-Cluster

In Fällen, in denen Sie mit einer leeren Freigabe beginnen und dann eine Kopie der Quelldaten erstellen möchten, können Sie die Befehle [`az storage file copy`](https://docs.microsoft.com/cli/azure/storage/file/copy?view=azure-cli-latest) zum Migrieren Ihrer Daten verwenden.

### <a name="deployment-strategy"></a>Bereitstellungsstrategie

Die empfohlen Methode besteht darin, Ihre vorhandene CI/CD-Pipeline zum Bereitstellen einer als funktionierend bekannten Konfiguration in AKS zu verwenden. Sie klonen Ihre vorhandenen Bereitstellungsaufgaben und stellen sicher, dass Ihre `kubeconfig` auf den neuen AKS-Cluster zeigt.

In Fällen, in denen dies nicht möglich ist, müssen Sie die Ressourcendefinition aus ACS exportieren und anschließend auf AKS anwenden. Sie können `kubectl` zum Exportieren von Objekten verwenden.

```console
kubectl get deployment -o=yaml --export > deployments.yaml
```

Außerdem können Sie abhängig von Ihren Anforderungen auf verschiedene Open Source-Tools zurückgreifen:

* [heptio/ark](https://github.com/heptio/ark): erfordert k8s 1.7
* [yaron2/azure-kube-cli](https://github.com/yaron2/azure-kube-cli)
* [mhausenblas/reshifter](https://github.com/mhausenblas/reshifter)

## <a name="migration-steps"></a>Schritte bei der Migration

### <a name="1-create-an-aks-cluster"></a>1. Erstellen eines AKS-Clusters

Sie können den Anweisungen in der Dokumentation folgen, um über das Azure-Portal, mithilfe der Azure CLI oder unter Verwendung einer Resource Manager-Vorlage [einen AKS-Cluster zu erstellen](https://docs.microsoft.com/azure/aks/create-cluster).

> Sie finden Azure Resource Manager-Beispielvorlagen für AKS im Repository [Azure/AKS](https://github.com/Azure/AKS/tree/master/examples/vnet) in GitHub.

### <a name="2-modify-applications"></a>2. Ändern von Anwendungen

Nehmen Sie eventuell erforderliche Änderungen an Ihren YAML-Definitionen vor. Beispiel: Ersetzen Sie für `Deployments` `apps/v1beta1` durch `apps/v1`.

### <a name="3-optional-migrate-volumes"></a>3. (Optional) Migrieren von Volumes

Migrieren Sie Volumes von Ihrem ACS-Cluster zu Ihrem AKS-Cluster. Weitere Informationen finden Sie im Abschnitt [Migrieren persistenter Volumes](#migrating-persistent-volumes).

### <a name="4-deploy-applications"></a>4. Bereitstellen von Anwendungen

Verwenden Sie Ihr CI/CD-System, um Anwendungen in AKS bereitzustellen, oder verwenden Sie „kubectl“, um die YAML-Definitionen anzuwenden.

### <a name="5-validate"></a>5. Überprüfen

Stellen Sie sicher, dass Ihre Anwendungen wie erwartet funktionieren und migrierte Daten an das Ziel kopiert wurden.

### <a name="6-redirect-traffic"></a>6. Umleiten von Datenverkehr

Aktualisieren Sie DNS, um Clients an Ihre AKS-Bereitstellung zu verweisen.

### <a name="7-post-migration-tasks"></a>7. Aufgaben nach der Migration

Wenn Sie Volumes migriert haben und Schreibvorgänge nicht stillgelegt haben, müssen Sie diese Daten in den neuen Cluster kopieren.
