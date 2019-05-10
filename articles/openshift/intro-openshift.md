---
title: Einführung in Azure Red Hat OpenShift | Microsoft-Dokumentation
description: Lernen Sie die Features und Vorteile von Microsoft Azure Red Hat OpenShift zur Bereitstellung und Verwaltung containerbasierter Anwendungen kennen.
services: container-service
author: tylermsft
ms.author: twhitney
ms.service: container-service
manager: jeconnoc
ms.topic: overview
ms.date: 05/06/2019
ms.custom: mvc
ms.openlocfilehash: 6121c0f654a61a147e84f0697f3ddb06b7c5db92
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65079948"
---
# <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

Der Microsoft *Azure Red Hat OpenShift*-Dienst ermöglicht Ihnen das Bereitstellen vollständig verwalteter [OpenShift](https://www.openshift.com/)-Cluster.

Azure Red Hat OpenShift stellt eine Erweiterung von [Kubernetes](https://kubernetes.io/) dar. Für das Ausführen von Containern mit Kubernetes in Produktionsumgebungen sind zusätzliche Tools und Ressourcen erforderlich, wie etwa eine Imageregistrierung, Speicherverwaltung, Netzwerklösungen sowie Protokollierungs- und Überwachungstools, alle in abgestimmten Versionen und miteinander getestet. Für das Erstellen containerbasierter Anwendungen ist sogar noch mehr Integrationsarbeit mit Middleware, Frameworks, Datenbanken und CI/CD-Tools zu leisten. Azure Red Hat OpenShift kombiniert all dies in einer einzelnen Plattform und verhilft so IT-Teams zu einem komfortablen Betrieb, während Anwendungsteams erhalten, was sie für die Ausführung benötigen.

Azure Red Hat OpenShift wird von Red Hat und Microsoft gemeinsam konzipiert, verarbeitet und supported, um eine integrierte Supporterfahrung zu bieten. Es müssen keine virtuellen Computer betrieben werden, und es sind keine Patches erforderlich. Master-, Infrastruktur- und Anwendungsknoten werden in Ihrem Auftrag von Red Hat und Microsoft mit Patches und Updates versehen und überwacht. Ihre Azure Red Hat OpenShift-Cluster werden in Ihrem Azure-Abonnement bereitgestellt und sind in Ihrer Azure-Rechnung enthalten.

Sie können für Registrierung, Netzwerk, Speicher und CI/CD eigene Lösungen wählen oder die integrierten Lösungen für automatische Quellcodeverwaltung, Container- und Anwendungsbuilds, Bereitstellungen, Skalierung, Health Management und mehr verwenden. Azure Red Hat OpenShift bietet über Azure Active Directory eine integrierte Anmeldeoberfläche.

Arbeiten Sie zum Einstieg das Tutorial [Erstellen eines Azure Red Hat OpenShift-Clusters](tutorial-create-cluster.md) durch.

## <a name="access-security-and-monitoring"></a>Zugriff, Sicherheit und Überwachung

Für verbesserte Sicherheit und Verwaltung ermöglicht Azure Red Hat OpenShift Ihnen die Integration in Azure Active Directory (Azure AD) und die Verwendung rollenbasierter (RBAC) Kubernetes-Zugriffssteuerungen. Sie können auch die Integrität Ihrer Cluster und Ressourcen überwachen.

## <a name="cluster-and-node"></a>Cluster- und Knoten

Azure Red Hat OpenShift-Knoten werden auf virtuellen Azure-Computern ausgeführt. Sie können eine Verbindung des Speichers mit Knoten und Pods herstellen, Clusterkomponenten aktualisieren und GPUs verwenden.

## <a name="virtual-networks-and-ingress"></a>Virtuelle Netzwerke und Eingang

Sie können einen Azure Red Hat OpenShift-Cluster in einem vorhandenen virtuellen Netzwerk bereitstellen. Bei dieser Konfiguration wird jedem Pod im Cluster eine IP-Adresse im virtuellen Netzwerk zugewiesen, und es ist jeweils eine direkte Kommunikation mit anderen Pods im Cluster und anderen Knoten im virtuellen Netzwerk möglich. Pods können auch eine Verbindung mit anderen Diensten eines mittels Peering verbundenen virtuellen Netzwerks sowie mit lokalen Netzwerken über [ExpressRoute](https://docs.microsoft.com/azure/expressroute/)- oder S2S-VPN-Verbindungen (Site-to-Site) herstellen.

Weitere Informationen finden Sie unter [Erstellen eines Microsoft Red Hat OpenShift-Clusters in Azure](tutorial-create-cluster.md).

## <a name="kubernetes-certification"></a>Kubernetes-Zertifizierung

Der Azure Red Hat OpenShift-Dienst wurde von der CNCF als Kubernetes-konform zertifiziert.

## <a name="next-steps"></a>Nächste Schritte

Lernen Sie die Voraussetzungen für Azure Red Hat OpenShift kennen:

> [!div class="nextstepaction"]
> [Einrichten Ihrer Entwicklungsumgebung](howto-setup-environment.md)