---
title: Verbinden von Azure Kubernetes Service (AKS) mit Azure Database for PostgreSQL
description: Es wird beschrieben, wie Sie Azure Kubernetes Service mit Azure Database for PostgreSQL verbinden.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.date: 11/27/2018
ms.topic: conceptual
ms.openlocfilehash: f25d87c7c557404071d777f4efcf22e53886d96d
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55242619"
---
# <a name="connecting-azure-kubernetes-service-and-azure-database-for-postgresql"></a>Verbinden von Azure Kubernetes Service und Azure Database for PostgreSQL

Azure Kubernetes Service (AKS) stellt einen verwalteten Kubernetes-Cluster bereit, den Sie in Azure nutzen können. Unten sind einige Optionen angegeben, die Sie berücksichtigen sollten, wenn Sie AKS und Azure Database for PostgreSQL zusammen zum Erstellen einer Anwendung verwenden.


## <a name="accelerated-networking"></a>Beschleunigte Netzwerke
Verwenden Sie in Ihrem AKS-Cluster zugrunde liegende virtuelle Computer, für die der beschleunigte Netzwerkbetrieb aktiviert ist. Wenn der beschleunigte Netzwerkbetrieb auf einem virtuellen Computer aktiviert ist, ist die Latenz geringer, Jitter reduziert und die CPU-Auslastung des virtuellen Computers niedriger. Informieren Sie sich eingehender über die Funktionsweise des beschleunigten Netzwerkbetriebs, die unterstützten Betriebssystemversionen und die unterstützten VM-Instanzen für [Linux](../virtual-network/create-vm-accelerated-networking-cli.md).

Ab November 2018 unterstützt AKS den beschleunigten Netzwerkbetrieb auf diesen unterstützen VM-Instanzen. Der beschleunigte Netzwerkbetrieb ist für neue AKS-Cluster, in denen diese virtuellen Computer verwendet werden, standardmäßig aktiviert.

Sie können überprüfen, ob Ihr AKS-Cluster über den beschleunigten Netzwerkbetrieb verfügt:
1. Navigieren Sie zum Azure-Portal, und wählen Sie Ihren AKS-Cluster aus.
2. Wählen Sie die Registerkarte „Eigenschaften“.
3. Kopieren Sie den Namen der **Infrastrukturressourcengruppe**.
4. Suchen Sie über die Suchleiste des Portals nach der Infrastrukturressourcengruppe, und öffnen Sie sie.
5. Wählen Sie in dieser Ressourcengruppe einen virtuellen Computer aus.
6. Navigieren Sie zur Registerkarte **Netzwerk** des virtuellen Computers.
7. Überprüfen Sie, ob die Option **Beschleunigter Netzwerkbetrieb** auf „Aktiviert“ festgelegt ist.

Alternativ über die Azure-Befehlszeilenschnittstelle mithilfe der folgenden beiden Befehle:
```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --query "nodeResourceGroup"
```
Die Ausgabe ist die von AKS erstellte generierte Ressourcengruppe, die die Netzwerkschnittstelle enthält. Verwenden Sie den Namen „nodeResourceGroup“ im nächsten Befehl. **EnableAcceleratedNetworking** ist entweder „true“ oder „false“:
```azurecli
az network nic list --resource-group nodeResourceGroup -o table
```

## <a name="open-service-broker-for-azure"></a>Installieren von Service Broker für Azure 
Mit [Open Service Broker für Azure](https://github.com/Azure/open-service-broker-azure/blob/master/README.md) (OSBA) können Sie Azure-Dienste direkt über Kubernetes oder Cloud Foundry bereitstellen. Es handelt sich um eine [Open Service Broker-API](https://www.openservicebrokerapi.org/)-Implementierung für Azure.

Mit OSBA können Sie einen Azure Database for PostgreSQL-Server erstellen und per nativer Kubernetes-Sprache an Ihren AKS-Cluster binden. Informieren Sie sich auf der [GitHub-Seite zu OSBA](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/postgresql.md), wie Sie OSBA und Azure Database for PostgreSQL zusammen nutzen können. 


## <a name="connection-pooling"></a>Verbindungspool
Eine Verbindungspoolfunktion minimiert die Kosten und die Zeit für das Erstellen neuer Verbindungen mit der Datenbank und das Schließen der Verbindungen. Der Pool ist eine Sammlung von Verbindungen, die wiederverwendet werden können. 

Sie können mehrere Verbindungspoolfunktionen mit PostgreSQL verwenden. Eine davon ist [PgBouncer](https://pgbouncer.github.io/). In der Microsoft-Containerregistrierung bieten wir einen einfachen PgBouncer im Container, der in einem Sidecar verwendet werden kann, um Verbindungen von AKS mit Azure Database for PostgreSQL in einem Pool zusammenzufassen. Besuchen Sie die [Docker-Hubseite](https://hub.docker.com/r/microsoft/azureossdb-tools-pgbouncer/), um zu erfahren, wie Sie darauf zugreifen und dieses Image verwenden. 


## <a name="next-steps"></a>Nächste Schritte
-  [Erstellen eines Azure Kubernetes Service-Clusters](../aks/kubernetes-walkthrough.md)
