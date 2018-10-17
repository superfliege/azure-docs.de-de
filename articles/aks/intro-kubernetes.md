---
title: Einführung in Azure Kubernetes Service
description: Lernen Sie die Features und Vorteile von Azure Kubernetes Service zur Bereitstellung und Verwaltung containerbasierter Anwendungen in Azure kennen.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: overview
ms.date: 09/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 67b80f539faab40884066f23aaf8a72ad7a6981b
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394404"
---
# <a name="azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS)

Mit Azure Kubernetes Service (AKS) können Sie ganz einfach einen Kubernetes-Cluster in Azure bereitstellen. AKS verringert die Komplexität und den operativen Mehraufwand für die Kubernetes-Verwaltung, indem ein Großteil dieser Verantwortung an Azure übertragen wird. Azure führt als gehosteter Kubernetes-Dienst wichtige Aufgaben für Sie aus, z.B. Systemüberwachung und Wartung. Die Kubernetes-Master werden von Azure verwaltet. Sie verwalten und warten nur die Agent-Knoten. Als verwalteter Kubernetes-Dienst ist AKS kostenlos. Sie zahlen nur für die Agent-Knoten in den Clustern, nicht für den Master.

Sie können einen AKS-Cluster im Azure-Portal, mit der Azure-Befehlszeilenschnittstelle oder vorlagengesteuerten Bereitstellungsoptionen wie etwa Resource Manager-Vorlagen und Terraform erstellen. Wenn Sie einen AKS-Cluster bereitstellen, werden der Kubernetes-Master und alle Knoten für Sie bereitgestellt und konfiguriert. Zusätzliche Features wie erweiterte Netzwerke, Azure Active Directory-Integration und Überwachung können ebenfalls im Rahmen der Bereitstellung konfiguriert werden.

Führen Sie zum Einstieg den AKS-Schnellstart [im Azure-Portal][aks-portal] oder [mit der Azure CLI][aks-cli] durch.

## <a name="access-security-and-monitoring"></a>Zugriff, Sicherheit und Überwachung

Für verbesserte Sicherheit und Verwaltung ermöglicht AKS Ihnen die Integration in Azure Active Directory und die Verwendung rollenbasierter Kubernetes-Zugriffssteuerungen. Sie können auch die Integrität Ihrer Cluster und Ressourcen überwachen.

### <a name="identity-and-security-management"></a>Identitäts- und Sicherheitsverwaltung

Um den Zugriff auf Clusterressourcen zu beschränken, unterstützt AKS [die rollenbasierte Kubernetes-Zugriffssteuerung (RBAC, Role-Based Access Control )][kubernetes-rbac]. Mit RBAC können Sie steuern, wie der Zugriff auf Kubernetes-Ressourcen und -Namespaces erfolgen kann, und welche Berechtigungen für diese Ressourcen bestehen. Sie können auch einen AKS-Cluster für die Azure Active Directory-Integration (AD) konfigurieren. Mit der Azure AD-Integration kann der Kubernetes-Zugriff auf der Grundlage vorhandener Identität und Gruppenmitgliedschaft konfiguriert werden. Ihren vorhandenen Azure AD-Benutzern und -Gruppen kann der Zugriff auf AKS-Ressourcen und eine integrierte Anmeldeerfahrung gewährt werden.

Wie Sie Ihre AKS-Cluster sichern, erfahren Sie unter [Integrieren von Azure Active Directory in AKS – Vorschauversion][aks-aad].

### <a name="integrated-logging-and-monitoring"></a>Integrierte Protokollierung und Überwachung

Um die Leistung Ihrer AKS-Cluster und bereitgestellten Anwendungen zu verstehen, sammelt Azure Monitor für die Containerintegrität Arbeitsspeicher- und Prozessormetriken aus Containern, Knoten und Controllern. Containerprotokolle sind verfügbar, und Sie können auch [die Kubernetes-Master-Protokolle überprüfen][aks-master-logs]. Diese Überwachungsdaten werden in Ihrem Azure Log Analytics-Arbeitsbereich gespeichert und stehen über das Azure-Portal, die Azure CLI und einen REST-Endpunkt zur Verfügung.

Weitere Informationen finden Sie unter [Überwachen der Integrität von Azure Kubernetes Service-Containern (AKS) (Vorschauversion)][container-health].

## <a name="cluster-and-node"></a>Cluster- und Knoten

AKS-Knoten werden auf virtuellen Azure-Computern ausgeführt. Sie können eine Verbindung des Speichers mit Knoten und Pods herstellen, Clusterkomponenten aktualisieren und GPUs verwenden.

### <a name="cluster-node-and-pod-scaling"></a>Clusterknoten und Skalierung von Pods

Wenn sich die Nachfrage nach Ressourcen ändert, kann die Anzahl der Clusterknoten oder Pods, die Ihre Dienste ausführen, automatisch zentral hoch- oder herunterskaliert werden. Sie können sowohl die horizontale automatische Podskalierungsfunktion als auch die Clusterskalierungsfunktion verwenden. Mit diesem Skalierungsansatz wird der AKS-Cluster automatisch den Anforderungen angepasst, sodass nur die erforderlichen Ressourcen ausgeführt werden.

Weitere Informationen finden Sie unter [Skalieren eines Azure Kubernetes Service-Clusters (AKS)][aks-scale].

### <a name="cluster-node-upgrades"></a>Upgrades für Clusterknoten

Azure Kubernetes Service bietet mehrere Kubernetes-Versionen. Wenn in AKS neue Versionen verfügbar werden, können Sie mithilfe des Azure-Portals oder der Azure CLI ein Clusterupgrade ausführen. Während des Upgradevorgangs werden die Knoten sorgfältig isoliert und ausgeglichen, um ausgeführte Anwendungen möglichst wenig zu beeinträchtigen.

Weitere Informationen zu Lebenszyklusversionen finden Sie unter [Unterstützte Kubernetes-Versionen in Azure Kubernetes Service (AKS)][aks-supported versions]. Informationen zu Aktualisierungsschritten finden Sie unter [Durchführen eines Upgrades für einen Azure Kubernetes Service-Cluster (AKS)][aks-upgrade].

### <a name="gpu-enabled-nodes"></a>GPU-fähige Knoten

AKS unterstützt die Erstellung GPU-fähiger Knotenpools. Azure bietet derzeit virtuelle Computer mit einzelner GPU oder mit mehreren GPUs. GPU-fähige virtuelle Computer sind für rechenintensive, grafikintensive und visualisierungsorientierte Workloads vorgesehen.

Weitere Informationen finden Sie unter [Verwenden von GPUs in AKS][aks-gpu].

### <a name="storage-volume-support"></a>Unterstützung von Speichervolumes

Um Workloads von Anwendungen zu unterstützen, können Sie die Speichervolumes für persistente Daten bereitstellen. Statische und dynamische Volumes können verwendet werden. Je nachdem, wie viele verbundene Pods den Speicher gemeinsam nutzen sollen, können Sie entweder für den Zugriff auf einen einzelnen Pod durch Azure-Datenträger unterstützten Speicher oder für den gleichzeitigen Zugriff auf mehrere Pods durch Azure Files unterstützten Speicher verwenden.

Erste Schritte mit dynamischen persistenten Volumes mit [Azure-Datenträgern][azure-disk] oder [Azure Files][azure-files].

## <a name="virtual-networks-and-ingress"></a>Virtuelle Netzwerke und Eingang

Ein AKS-Cluster kann in einem vorhandenen virtuellen Netzwerk bereitgestellt werden. Bei dieser Konfiguration wird jedem Pod im Cluster eine IP-Adresse im virtuellen Netzwerk zugewiesen, und es ist jeweils eine direkte Kommunikation mit anderen Pods im Cluster und anderen Knoten im virtuellen Netzwerk möglich. Pods können auch eine Verbindung mit anderen Diensten eines mittels Peering verbundenen virtuellen Netzwerks sowie mit lokalen Netzwerken über ExpressRoute- oder S2S-VPN-Verbindungen (Site-to-Site) herstellen.

Weitere Informationen finden Sie unter [Netzwerkkonfiguration in Azure Kubernetes Service (AKS)][aks-networking].

### <a name="ingress-with-http-application-routing"></a>Eingang mit HTTP-Anwendungsrouting

Das Add-On für das HTTP-Anwendungsrouting ermöglicht einen einfachen Zugriff auf Anwendungen, die in Ihrem AKS-Cluster bereitgestellt sind. Bei aktiviertem HTTP-Anwendungsrouting konfiguriert die Lösung einen Eingangscontroller in Ihrem AKS-Cluster. Im Rahmen der Anwendungsbereitstellung werden öffentlich zugängliche DNS-Namen automatisch konfiguriert. Das HTTP-Anwendungsrouting konfiguriert eine DNS-Zone und integriert sie in das AKS-Cluster. Anschließend können Sie Kubernetes-Eingangsressourcen wie gewohnt bereitstellen.

Informationen zu den ersten Schritten mit eingehendem Datenverkehr finden Sie unter [HTTP-Anwendungsrouting][aks-http-routing].

## <a name="development-tooling-integration"></a>Integration von Entwicklungstools

Kubernetes verfügt über ein umfangreiches Ökosystem von Entwicklungs- und Verwaltungstools. Hierzu zählen beispielsweise Helm, Draft und die Kubernetes-Erweiterung für Visual Studio Code. Diese Tools funktionieren nahtlos mit AKS.

Darüber hinaus ermöglicht Azure Dev Spaces eine schnelle, iterative Kubernetes-Bereitstellung für Teams. Container lassen sich mit minimalem Konfigurationsaufwand direkt in AKS ausführen und debuggen. Informationen zu ersten Schritten finden Sie unter [Azure Dev Spaces][azure-dev-spaces].

Das Azure DevOps-Projekt bietet eine Lösung, mit der Sie vorhanden Code und das Git-Repository ganz einfach in Azure einbinden können. Mit dem DevOps-Projekt werden automatisch Azure-Ressourcen wie AKS erstellt, in Azure DevOps Services eine Releasepipeline mit einer Buildpipeline für CI erstellt, eine Releasepipeline für CD eingerichtet und anschließend eine Azure Application Insights-Ressource für die Überwachung erstellt.

Weitere Informationen finden Sie unter [Tutorial: Deploy your ASP.NET Core App to Azure Kubernetes Service (AKS) with the Azure DevOps Project][azure-devops] (Tutorial: Bereitstellen Ihrer ASP.NET Core-App in Azure Kubernetes Service (AKS) mit dem Azure DevOps-Projekt).

## <a name="docker-image-support-and-private-container-registry"></a>Docker-Imageunterstützung und private Containerregistrierung

AKS unterstützt das Docker-Imageformat. Zur privaten Speicherung Ihrer Docker-Images können Sie AKS in die Azure Container Registry (ACR) integrieren.

Wie Sie private Imagespeicher erstellen, erfahren Sie unter [Azure Container Registry][acr-docs].

## <a name="kubernetes-certification"></a>Kubernetes-Zertifizierung

Azure Kubernetes Service (AKS) wurde von der CNCF als Kubernetes-konform zertifiziert.

## <a name="regulatory-compliance"></a>Compliance

Azure Kubernetes Service (AKS) erfüllt die Anforderungen von SOC, ISO und PCI-DSS.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Bereitstellung und Verwaltung von AKS mit dem Azure CLI-Schnellstart.

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
[aks-scale]: ./tutorial-kubernetes-scale.md
[aks-upgrade]: ./upgrade-cluster.md
[azure-dev-spaces]: https://docs.microsoft.com/en-us/azure/dev-spaces/azure-dev-spaces
[azure-devops]: https://docs.microsoft.com/en-us/azure/devops-project/overview
[azure-disk]: ./azure-disks-dynamic-pv.md
[azure-files]: ./azure-files-dynamic-pv.md
[container-health]: ../monitoring/monitoring-container-health.md
[aks-master-logs]: view-master-logs.md
[aks-supported versions]: supported-kubernetes-versions.md
