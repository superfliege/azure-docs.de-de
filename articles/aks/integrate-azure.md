---
title: Integrieren von mit Azure verwalteten Diensten mithilfe von Open Service Broker für Azure (OSBA)
description: Integrieren von mit Azure verwalteten Diensten mithilfe von Open Service Broker für Azure (OSBA)
services: container-service
author: zr-msft
manager: jeconnoc
ms.service: container-service
ms.topic: overview
ms.date: 12/05/2017
ms.author: zarhoads
ms.openlocfilehash: 7a887905bcb4b09c1b4ae179116b3f08c75caabd
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758287"
---
# <a name="integrate-with-azure-managed-services-using-open-service-broker-for-azure-osba"></a>Integrieren von mit Azure verwalteten Diensten mithilfe von Open Service Broker für Azure (OSBA)

Zusammen mit dem [Kubernetes Service Catalog][kubernetes-service-catalog] ermöglicht Open Service Broker für Azure (OSBA) Entwicklern die Nutzung von mit Azure verwalteten Diensten in Kubernetes. Dieser Leitfaden konzentriert sich auf die Bereitstellung von Kubernetes Service Catalog, Open Service Broker für Azure (OSBA) und Anwendungen, die von Azure verwaltete Dienste mit Kubernetes nutzen.

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Abonnement

* Azure CLI: [Lokale Installation][azure-cli-install] oder Verwendung in [Azure Cloud Shell][azure-cloud-shell]

* Helm-CLI 2.7+: [lokale Installation][helm-cli-install] oder Verwendung in [Azure Cloud Shell][azure-cloud-shell]

* Berechtigungen zum Erstellen eines Dienstprinzipals mit der Rolle „Mitwirkender“ in Ihrem Azure-Abonnement

* Ein bestehender Azure Kubernetes Service-Clusters (AKS). Befolgen Sie die Schnellstartanleitung [Erstellen eines AKS-Clusters][create-aks-cluster], wenn Sie einen AKS-Cluster benötigen.

## <a name="install-service-catalog"></a>Installieren des Dienstkatalogs

Der erste Schritt besteht darin, den Dienstkatalog in Ihrem Kubernetes-Cluster mithilfe eines Helm-Diagramms zu installieren. Führen Sie ein Upgrade Ihrer Tiller-Installation (Helm-Server) in Ihrem Cluster mit Folgendem durch:

```azurecli-interactive
helm init --upgrade
```

Fügen Sie nun das Dienstkatalogdiagramm zum Helm-Repository hinzu:

```azurecli-interactive
helm repo add svc-cat https://svc-catalog-charts.storage.googleapis.com
```

Installieren Sie abschließend den Dienstkatalog mit dem Helm-Diagramm. Führen Sie diesen Befehl aus, wenn Ihr Cluster RBAC-fähig ist.

```azurecli-interactive
helm install svc-cat/catalog --name catalog --namespace catalog --set apiserver.storage.etcd.persistence.enabled=true --set apiserver.healthcheck.enabled=false --set controllerManager.healthcheck.enabled=false --set apiserver.verbosity=2 --set controllerManager.verbosity=2
```

Führen Sie diesen Befehl aus, falls Ihr Cluster nicht RBAC-fähig ist.

```azurecli-interactive
helm install svc-cat/catalog --name catalog --namespace catalog --set rbacEnable=false --set apiserver.storage.etcd.persistence.enabled=true --set apiserver.healthcheck.enabled=false --set controllerManager.healthcheck.enabled=false --set apiserver.verbosity=2 --set controllerManager.verbosity=2
```

Nachdem das Helm-Diagramm ausgeführt wurde, vergewissern Sie sich, dass `servicecatalog` in der Ausgabe des folgenden Befehls angezeigt wird:

```azurecli-interactive
kubectl get apiservice
```

Beispielsweise sollten eine Ausgabe ähnlich der folgenden angezeigt werden (hier abgeschnitten anzeigen):

```
NAME                                 AGE
v1.                                  10m
v1.authentication.k8s.io             10m
...
v1beta1.servicecatalog.k8s.io        34s
v1beta1.storage.k8s.io               10
```

## <a name="install-open-service-broker-for-azure"></a>Installieren von Open Service Broker für Azure

Der nächste Schritt besteht darin, [Open Service Broker für Azure][open-service-broker-azure] zu installieren, worin der Katalog für die von Azure verwalteten Dienste enthalten ist. Beispiele für verfügbare Azure-Dienste sind Azure Database for PostgreSQL, Azure Database for MySQL und Azure SQL-Datenbank.

Sie beginnen mit dem Hinzufügen des Helm-Repositorys für Open Service Broker für Azure:

```azurecli-interactive
helm repo add azure https://kubernetescharts.blob.core.windows.net/azure
```

Erstellen Sie einen [Dienstprinzipal][create-service-principal] mit dem folgenden Azure CLI-Befehl:

```azurecli-interactive
az ad sp create-for-rbac
```

Die Ausgabe sollte in etwa wie folgt aussehen. Notieren Sie sich die Werte `appId`, `password` und `tenant`, die Sie im nächsten Schritt verwenden.

```JSON
{
  "appId": "7248f250-0000-0000-0000-dbdeb8400d85",
  "displayName": "azure-cli-2017-10-15-02-20-15",
  "name": "http://azure-cli-2017-10-15-02-20-15",
  "password": "77851d2c-0000-0000-0000-cb3ebc97975a",
  "tenant": "72f988bf-0000-0000-0000-2d7cd011db47"
}
```

Legen Sie die folgenden Umgebungsvariablen mit den vorhergehenden Werten fest:

```azurecli-interactive
AZURE_CLIENT_ID=<appId>
AZURE_CLIENT_SECRET=<password>
AZURE_TENANT_ID=<tenant>
```

Rufen Sie jetzt Ihre Azure-Abonnement-ID ab:

```azurecli-interactive
az account show --query id --output tsv
```

Legen Sie erneut die folgenden Umgebungsvariablen mit dem vorhergehenden Wert fest:

```azurecli-interactive
AZURE_SUBSCRIPTION_ID=[your Azure subscription ID from above]
```

Nachdem Sie diese Umgebungsvariablen ausgefüllt haben, führen Sie den folgenden Befehl aus, um den Open Service Broker für Azure mithilfe des Helm-Diagramms zu installieren:

```azurecli-interactive
helm install azure/open-service-broker-azure --name osba --namespace osba \
    --set azure.subscriptionId=$AZURE_SUBSCRIPTION_ID \
    --set azure.tenantId=$AZURE_TENANT_ID \
    --set azure.clientId=$AZURE_CLIENT_ID \
    --set azure.clientSecret=$AZURE_CLIENT_SECRET
```

Sobald die OSBA-Bereitstellung abgeschlossen ist, installieren Sie die [Dienstkatalog-CLI][service-catalog-cli], eine benutzerfreundliche Befehlszeilenschnittstelle für die Abfrage von Service Brokern, Dienstklassen, Dienstplänen und mehr.

Führen Sie die folgenden Befehle aus, um die Binärdatei der Dienstkatalog-CLI zu installieren:

```azurecli-interactive
curl -sLO https://servicecatalogcli.blob.core.windows.net/cli/latest/$(uname -s)/$(uname -m)/svcat
chmod +x ./svcat
```

Führen Sie nun die installierten Service Broker auf:

```azurecli-interactive
./svcat get brokers
```

Eine Ausgabe ähnlich der folgenden sollte angezeigt werden:

```
  NAME                               URL                                STATUS
+------+--------------------------------------------------------------+--------+
  osba   http://osba-open-service-broker-azure.osba.svc.cluster.local   Ready
```

Als nächstes listen Sie die verfügbaren Dienstklassen auf. Die angezeigten Dienstklassen sind die verfügbaren, von Azure verwalteten Dienste, die über Open Service Broker für Azure bereitgestellt werden können.

```azurecli-interactive
./svcat get classes
```

Listen Sie abschließend alle verfügbaren Dienstpläne auf. Dienstpläne sind die Dienstebenen für die von Azure verwalteten Dienste. Im Falle von Azure Database for MySQL reichen die Pläne z. B. von `basic50` für den Tarif „Basic“ mit 50 Datenübertragungseinheiten (DTUs) bis `standard800` für den Tarif „Standard“ mit 800 DTUs.

```azurecli-interactive
./svcat get plans
```

## <a name="install-wordpress-from-helm-chart-using-azure-database-for-mysql"></a>Installieren von WordPress aus Helm-Diagramm mithilfe von Azure Database for MySQL

In diesem Schritt verwenden Sie Helm, um ein aktualisiertes Helm-Diagramm für WordPress zu installieren. Das Diagramm enthält eine externe Azure Database for MySQL-Instanz, die WordPress verwenden kann. Dieser Vorgang kann einige Minuten dauern.

```azurecli-interactive
helm install azure/wordpress --name wordpress --namespace wordpress --set resources.requests.cpu=0 --set replicaCount=1
```

Listen Sie die installierten Dienstinstanzen und -bindungen auf, um zu überprüfen, ob durch die Installation die richtigen Ressourcen bereitgestellt wurden:

```azurecli-interactive
./svcat get instances -n wordpress
./svcat get bindings -n wordpress
```

Auflisten installierter geheimer Schlüssel:

```azurecli-interactive
kubectl get secrets -n wordpress -o yaml
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie sich an diesen Artikel halten, haben Sie den Dienstkatalog in einem Azure Kubernetes Service-Cluster (AKS) bereitgestellt. Sie haben Open Service Broker für Azure verwendet, um eine WordPress-Installation bereitzustellen, die von Azure verwaltete Dienste verwendet, in diesem Fall Azure Database for MySQL.

Über das Repository [Azure-/Helm-Diagramme][helm-charts] können Sie auf andere aktualisierte OSBA-basierte Helm-Diagramme zugreifen. Wenn Sie daran interessiert sind, eigene Diagramme zu erstellen, die für OSBA geeignet sind, helfen Ihnen die Informationen unter [Erstellen eines neuen Diagramms][helm-create-new-chart] weiter.

<!-- LINKS - external -->
[helm-charts]: https://github.com/Azure/helm-charts
[helm-cli-install]: https://docs.helm.sh/helm/#helm-install
[helm-create-new-chart]: https://github.com/Azure/helm-charts#creating-a-new-chart
[kubernetes-service-catalog]: https://github.com/kubernetes-incubator/service-catalog
[open-service-broker-azure]: https://github.com/Azure/open-service-broker-azure
[service-catalog-cli]: https://github.com/Azure/service-catalog-cli

<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cloud-shell]: ../cloud-shell/overview.md
[create-aks-cluster]: ./kubernetes-walkthrough.md
[create-service-principal]: ./kubernetes-service-principal.md
