---
title: Einführung in Azure Kubernetes Service
description: Azure Kubernetes Service vereinfacht die Bereitstellung und Verwaltung containerbasierter Anwendungen in Azure.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: overview
ms.date: 06/13/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: cb38285a009d8dfba175de6e3037970e6111d929
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096126"
---
# <a name="azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS)

Mit Azure Kubernetes Service (AKS) können Sie ganz einfach einen Kubernetes-Cluster in Azure bereitstellen. AKS verringert die Komplexität und den operativen Mehraufwand für die Kubernetes-Verwaltung, indem ein Großteil dieser Verantwortung an Azure übertragen wird. Azure führt als gehosteter Kubernetes-Dienst wichtige Aufgaben für Sie aus, z.B. Systemüberwachung und Wartung. Der Dienst ist zudem kostenlos. Sie zahlen nur für die Agent-Knoten in den Clustern, nicht für den Master.

Dieses Dokument enthält eine Übersicht über die Features von Azure Kubernetes Service (AKS).

## <a name="flexible-deployment-options"></a>Flexible Bereitstellungsoptionen

Azure Kubernetes Service bietet portalbasierte, befehlszeilenbasierte und vorlagenbasierte Bereitstellungsoptionen (Resource Manager-Vorlagen und Terraform). Wenn Sie einen AKS-Cluster bereitstellen, werden der Kubernetes-Master und alle Knoten für Sie bereitgestellt und konfiguriert. Zusätzliche Features wie erweiterte Netzwerke, Azure Active Directory-Integration und Überwachung können ebenfalls im Rahmen der Bereitstellung konfiguriert werden.

Weitere Informationen finden Sie unter [Schnellstart: Bereitstellen eines Azure Kubernetes Service-Clusters (AKS)][aks-portal] (Schnellstartanleitung für das Portal) sowie unter [Schnellstart: Bereitstellen eines Azure Kubernetes Service-Clusters (AKS)][aks-cli] (Schnellstartanleitung für die CLI).

## <a name="identity-and-security-management"></a>Identitäts- und Sicherheitsverwaltung

AKS-Cluster unterstützen die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)][kubernetes-rbac]. Ein AKS-Cluster kann auch für die Azure Active Directory-Integration konfiguriert werden. In dieser Konfiguration kann der Kubernetes-Zugriff auf der Grundlage von Azure Active Directory-Identität und Gruppenmitgliedschaft konfiguriert werden.

Weitere Informationen finden Sie unter [Integrieren von Azure Active Directory in AKS – Vorschauversion][aks-aad].

## <a name="integrated-logging-and-monitoring"></a>Integrierte Protokollierung und Überwachung

Die Containerintegrität erfasst Arbeitsspeicher- und Prozessormetriken von Containern, Knoten und Controllern und liefert Informationen zur Leistung. Auch Containerprotokolle werden erfasst. Diese Daten werden in Ihrem Log Analytics-Arbeitsbereich gespeichert und stehen über das Azure-Portal, über die Azure CLI und über einen REST-Endpunkt zur Verfügung.

Weitere Informationen finden Sie unter [Überwachen der Integrität von Azure Kubernetes Service-Containern (AKS) (Vorschauversion)][container-health].

## <a name="cluster-node-scaling"></a>Skalierung von Clusterknoten

Die Knoten eines AKS-Clusters können horizontal hochskaliert werden, um auf steigende Ressourcenanforderungen zu reagieren. Bei rückläufigem Ressourcenbedarf können Knoten mittels Skalierung aus dem Cluster entfernt werden. AKS-Skalierungsvorgänge können über das Azure-Portal oder über die Azure CLI ausgeführt werden.

Weitere Informationen finden Sie unter [Skalieren eines Azure Kubernetes Service-Clusters (AKS)][aks-scale].

## <a name="cluster-node-upgrades"></a>Upgrades für Clusterknoten

Azure Kubernetes Service bietet mehrere Kubernetes-Versionen. Wenn in AKS neue Versionen verfügbar werden, können Sie mithilfe des Azure-Portals oder der Azure CLI ein Clusterupgrade ausführen. Während des Upgradevorgangs werden die Knoten sorgfältig isoliert und ausgeglichen, um ausgeführte Anwendungen möglichst wenig zu beeinträchtigen.

Weitere Informationen finden Sie unter [Durchführen eines Upgrades für einen Azure Kubernetes Service-Cluster (AKS)][aks-upgrade].

## <a name="http-application-routing"></a>HTTP-Anwendungsrouting

Die Lösung für das HTTP-Anwendungsrouting ermöglicht einen einfachen Zugriff auf Anwendungen, die in Ihrem AKS-Cluster bereitgestellt sind. Bei aktiviertem HTTP-Anwendungsrouting konfiguriert die Lösung einen Eingangscontroller in Ihrem AKS-Cluster. Im Rahmen der Anwendungsbereitstellung werden automatisch öffentlich zugängliche DNS-Namen konfiguriert.

Weitere Informationen finden Sie unter [HTTP-Anwendungsrouting][aks-http-routing].

## <a name="gpu-enabled-nodes"></a>GPU-fähige Knoten

AKS unterstützt die Erstellung GPU-fähiger Knotenpools. Azure bietet derzeit virtuelle Computer mit einzelner GPU oder mit mehreren GPUs. GPU-fähige virtuelle Computer sind für rechenintensive, grafikintensive und visualisierungsorientierte Workloads vorgesehen.

Weitere Informationen finden Sie unter [Verwenden von GPUs in AKS][aks-gpu].

## <a name="development-tooling-integration"></a>Integration von Entwicklungstools

Kubernetes verfügt über ein umfangreiches Ökosystem von Entwicklungs- und Verwaltungstools. Hierzu zählen beispielsweise Helm, Draft und die Kubernetes-Erweiterung für Visual Studio Code. Diese Tools arbeiten nahtlos mit Azure Kubernetes Service zusammen.

Darüber hinaus ermöglicht Azure Dev Spaces eine schnelle, iterative Kubernetes-Bereitstellung für Teams. Container lassen sich mit minimalem Konfigurationsaufwand direkt in Azure Kubernetes Service (AKS) ausführen und debuggen.

Weitere Informationen finden Sie unter [Azure Dev Spaces][azure-dev-spaces].

Das Azure DevOps-Projekt bietet eine Lösung, mit der Sie vorhanden Code und das Git-Repository ganz einfach in Azure einbinden können. Das DevOps-Projekt erstellt automatisch Azure-Ressourcen (beispielsweise AKS) und eine Releasepipeline in VSTS mit einer Builddefinition für CI, richtet eine Releasedefinition für CD ein und erstellt anschließend eine Azure Application Insights-Ressource für die Überwachung.

Weitere Informationen finden Sie unter [Tutorial: Deploy your ASP.NET Core App to Azure Kubernetes Service (AKS) with the Azure DevOps Project][azure-devops] (Tutorial: Bereitstellen Ihrer ASP.NET Core-App in Azure Kubernetes Service (AKS) mit dem Azure DevOps-Projekt).

## <a name="virtual-network-integration"></a>Integration in ein virtuelles Netzwerk

Ein AKS-Cluster kann in einem vorhandenen VNet bereitgestellt werden. Bei dieser Konfiguration wird jedem Pod im Cluster eine IP-Adresse im VNet zugewiesen, und es ist jeweils eine direkte Kommunikation mit anderen Pods im Cluster und anderen Knoten im VNet möglich. Pods können auch eine Verbindung mit anderen Diensten eines mittels Peering verbundenen VNets sowie mit lokalen Netzwerken über ExpressRoute- und S2S-VPN-Verbindungen (Site-to-Site) herstellen.

Weitere Informationen finden Sie unter [Netzwerkkonfiguration in Azure Kubernetes Service (AKS)][aks-networking].

## <a name="private-container-registry"></a>Private Containerregistrierung

Zur privaten Speicherung Ihrer Docker-Images können Sie eine ACR-Integration (Azure Container Registry) einrichten.

Weitere Informationen finden Sie unter [Einführung in private Docker-Containerregistrierungen in Azure][acr-docs].

## <a name="storage-volume-support"></a>Unterstützung von Speichervolumes

Azure Kubernetes Service (AKS) unterstützt das Einbinden von Speichervolumes für persistente Daten. AKS-Cluster werden mit Unterstützung für Azure Files und Azure Disks erstellt.

Weitere Informationen finden Sie unter [Persistente Volumes mit Azure Files][azure-files] bzw. unter [Persistente Volumes mit Azure-Datenträgern][azure-disk].

## <a name="docker-image-support"></a>Unterstützung von Docker-Images

Azure Kubernetes Service (AKS) unterstützt das Docker-Imageformat.

## <a name="kubernetes-certification"></a>Kubernetes-Zertifizierung

Azure Kubernetes Service (AKS) wurde von der CNCF als Kubernetes-konform zertifiziert.

## <a name="regulatory-compliance"></a>Compliance

Azure Kubernetes Service (AKS) erfüllt die Anforderungen von SOC sowie von ISO/HIPAA/HITRUST.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Bereitstellung und Verwaltung von AKS mit der AKS-Schnellstartanleitung.

> [!div class="nextstepaction"]
> [Schnellstart: Bereitstellen eines Azure Kubernetes Service-Clusters (AKS)][aks-cli]

<!-- LINKS - external -->
[acs-engine]: https://github.com/Azure/acs-engine
[draft]: https://github.com/Azure/draft
[helm]: https://helm.sh/
[kubectl-overview]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[kubernetes-rbac]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - internal -->
[acr-docs]: ../container-registry/container-registry-intro.md
[aks-aad]: ./aad-integration.md
[aks-cli]: ./kubernetes-walkthrough.md
[aks-gpu]: ./gpu-cluster.md
[aks-http-routing]: ./http-application-routing.md
[aks-networking]: ./networking-overview.md
[aks-portal]: ./kubernetes-walkthrough-portal.md
[aks-scale]: ./scale-cluster.md
[aks-upgrade]: ./upgrade-cluster.md
[azure-dev-spaces]: https://docs.microsoft.com/en-us/azure/dev-spaces/azure-dev-spaces
[azure-devops]: https://docs.microsoft.com/en-us/vsts/pipelines/actions/azure-devops-project-aks?view=vsts
[azure-disk]: ./azure-disks-dynamic-pv.md
[azure-files]: ./azure-files-dynamic-pv.md
[container-health]: ../monitoring/monitoring-container-health.md

