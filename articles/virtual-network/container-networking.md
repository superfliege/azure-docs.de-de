---
title: "Azure Virtual Network für Container | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über das CNI-Plug-In für Kubernetes-Cluster, das Container für die Kommunikation untereinander und mit anderen Ressourcen in einem virtuellen Netzwerk aktiviert."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: jdial
ms.openlocfilehash: f70afa8ff69b6c79363313c0f2df3b6785da8d81
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2017
---
# <a name="container-networking"></a>Containernetzwerke

Azure stellt ein [CNI-Plug-In (Container Networking Interface)](https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md) zur Verfügung, mit dem Sie über die nativen Azure-Netzwerkfunktionen Ihren eigenen Kubernetes-Cluster bereitstellen und verwalten können. Das Plug-In ist bei der Bereitstellung von Kubernetes-Clustern mit der [Azure Container Service-Engine](https://github.com/Azure/acs-engine) (kurz ACS-Engine) standardmäßig aktiviert.

## <a name="networking-capabilities"></a>Netzwerkfunktionen

Für Container ist der große Funktionsumfang virtueller Netzwerke wie etwa folgende Funktionen vorteilhaft:
-   Sie können ein separates virtuelles Netzwerk für Ihren Cluster erstellen oder Ihren Cluster in einem vorhandenen virtuellen Netzwerk bereitstellen. 
-   Jeder Pod im Cluster erhält eine IP-Adresse vom virtuellen Netzwerk und kann direkt mit anderen Pods im Cluster und auf virtuellen Computern im virtuellen Netzwerk kommunizieren. 
-   Ein Pod kann über ExpressRoute- und Site-to-Site-VPN-Verbindungen eine Verbindung mit anderen Pods und virtuellen Computern in virtuellen Netzwerken und lokalen Netzwerken mittels Peering herstellen. Lokale Ressourcen können eine Verbindung mit Pods herstellen. 
-   Über Azure Load Balancer können Sie einen Kubernetes-Dienst für das Internet verfügbar machen.  
-   Pods in einem Subnetz mit aktivierten Dienstendpunkten können eine sichere Verbindung mit Azure-Diensten (z.B. Storage und SQL-Datenbank) herstellen.
-   Sie können benutzerdefinierte Routen zum Weiterleiten von Datenverkehr von Pods an eine virtuelle Netzwerkappliance verwenden. 
-   Pods können auf öffentliche Ressourcen im Internet zugreifen.
-   Sie können einem Pod eine öffentliche IP-Adresse zuweisen, die sich einem DNS-Namen zuordnen lässt.
 
## <a name="limits"></a>Einschränkungen
Sie können bei der Verwendung des Plug-Ins bis zu 4.000 Knoten in einem Kubernetes-Cluster und bis zu 250 Pods pro Knoten mit einer Gesamtanzahl von maximal 16.000 Pods pro Cluster bereitstellen.

## <a name="constraints"></a>Einschränkungen
- Das Plug-In ist bei der Bereitstellung eines Kubernetes-Clusters mit einem [Azure Container Service](../aks/intro-kubernetes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-Cluster (ACS) bzw. [ACS](../container-service/kubernetes/container-service-intro-kubernetes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-Cluster mit Kubernetes nicht aktiviert.
- Es ist keine native Unterstützung für Kubernetes-Netzwerkrichtlinien, einschließlich DNS- oder Zugriffsrichtlinien, vorhanden.
- Das Plug-In unterstützt keine podspezifischen Netzwerkrichtlinien.

## <a name="pricing"></a>Preise
Die Nutzung des CNI-Plug-Ins ist kostenlos.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [einen Kubernetes-Cluster ](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/deploy.md) in Ihrem [eigenen virtuellen Netzwerk bereitstellen](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/features.md#using-azure-integrated-networking-cni).
