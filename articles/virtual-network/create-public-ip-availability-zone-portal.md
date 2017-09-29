---
title: "Erstellen einer öffentlichen IP-Adresse in einer Zone mit dem Azure-Portal | Microsoft-Dokumentation"
description: "Erstellen einer öffentlichen IP-Adresse in einer Verfügbarkeitszone mit dem Azure-Portal."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: jdial
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: 7dceb7bb38b1dac778151e197db3b5be49dd568a
ms.openlocfilehash: 0398e9f814614b3dccf849385903a8ed8717eafb
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="create-a-public-ip-address-in-an-availability-zone-with-the-azure-portal"></a>Erstellen einer öffentlichen IP-Adresse in einer Verfügbarkeitszone mit dem Azure-Portal

Sie können eine öffentliche IP-Adresse in einer Azure-Verfügbarkeitszone (Vorschau) bereitstellen. Eine Verfügbarkeitszone ist eine physisch separate Zone in einer Azure-Region. In diesem Artikel werden folgende Themen erläutert:

> * Erstellen einer öffentlichen IP-Adresse in einer Verfügbarkeitszone
> * Identifizieren verwandter Ressourcen, die in der Verfügbarkeitszone erstellt wurden

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

> [!NOTE]
> Die Verfügbarkeitszonen sind als Vorschauversion verfügbar und können in Ihren Entwicklungs- und Testszenarios verwendet werden. Unterstützung ist für ausgewählte Azure-Ressourcen und -Regionen sowie VM-Größenkategorien verfügbar. Weitere Informationen zu den ersten Schritten sowie zu den Azure-Ressourcen, -Regionen und VM-Größenkategorien, die mit Verfügbarkeitszonen verwendet werden können, finden Sie unter [Overview of Availability Zones in Azure (Preview) (Übersicht über Verfügbarkeitszonen in Azure (Vorschauversion))](https://docs.microsoft.com/azure/availability-zones/az-overview). Wenn Sie Unterstützung benötigen, können Sie über [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) Kontakt aufnehmen oder [ein Azure-Supportticket erstellen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter „https://portal.azure.com“ im Azure-Portal an. 

## <a name="create-a-zonal-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse in einer Zone

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.
2. Wählen Sie **Netzwerk** und dann **Öffentliche IP-Adresse** aus.
3. Geben Sie Werte für die folgenden Einstellungen ein, oder wählen Sie diese aus, wählen Sie Ihr Abonnement aus, akzeptieren Sie die Standardwerte für die verbleibenden Einstellungen, und klicken Sie dann auf **Erstellen**:

    |Einstellung|Wert|
    |---|---|
    |SKU| **Basic**: Mit der statischen oder der dynamischen Zuordnungsmethode zugewiesen. Kann jeder Azure-Ressource zugewiesen werden, der eine öffentliche IP-Adresse zugewiesen werden kann, z.B. Netzwerkschnittstellen, VPN Gateways, Application Gateways und Lastenausgleiche mit Internetzugriff. Sie können die öffentliche IP-Adresse einer bestimmten Zone in der Einstellung **Verfügbarkeitszone** zuweisen. Sind nicht zonenredundant. Weitere Informationen zu Verfügbarkeitszonen finden Sie unter [Overview of Availability Zones in Azure (Preview) (Übersicht über Verfügbarkeitszonen in Azure (Vorschauversion))](https://docs.microsoft.com/azure/availability-zones/az-overview). **Standard**: Nur mit der statischen Zuordnungsmethode zugewiesen. Kann Netzwerkschnittstellen oder Standardlastenausgleichen mit Internetzugriff zugewiesen werden. Wenn Sie die öffentliche IP-Adresse einem Standardlastenausgleich mit Internetzugriff zuweisen, müssen Sie „Standard“ auswählen. Weitere Informationen zu SKUs von Azure Load Balancer finden Sie unter [Azure load balancer standard SKU (Standard-SKU von Azure Load Balancer)](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview). Sind standardmäßig zonenredundant. Können in Zonen erstellt und in einer bestimmten Verfügbarkeitszone garantiert werden. Die Standard-SKU befindet sich in der Vorschauversion. Bevor Sie eine öffentliche IP-Adresse für eine Standard-SKU erstellen, müssen Sie sich zunächst für die Vorschauversion registrieren. Unter [Register for the standard SKU preview (Registrieren für die Vorschauversion der Standard-SKU)](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#preview-sign-up) können Sie sich für die Vorschauversion registrieren. Die Standard-SKU kann nur an einem unterstützten Speicherort erstellt werden.  Eine Liste der unterstützten Speicherorte (Regionen) finden Sie unter [Region availability (Regionsverfügbarkeit)](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#region-availability). Auf der Seite [Azure Virtual Network updates (Updates für Azure Virtual Network)](https://azure.microsoft.com/updates/?product=virtual-network) erhalten Sie außerdem zusätzlichen Support für Regionen.|   
    |Name|Der Name muss innerhalb der ausgewählten Ressourcengruppe eindeutig sein.|
    |Ressourcengruppe|Klicken Sie auf „Neu erstellen“, und geben Sie „myResourceGroup“ ein|
    |Standort|Europa, Westen|
    |Verfügbarkeitszone|Wenn Sie die **Standard**-SKU ausgewählt haben, können Sie *Zonenredundant* auswählen, wenn Sie möchten, dass die IP-Adresse zonenübergreifend robust ist. Wenn Sie die **Basic**-SKU ausgewählt haben, ist die IP-Adresse nicht zonenübergreifend robust. Unabhängig von der SKU, die Sie auswählen, können Sie die Adresse einer bestimmten Zone zuweisen, wenn Sie möchten. |

    Die Einstellungen werden wie in der folgenden Abbildung gezeigt im Portal angezeigt:

    ![Erstellen einer öffentlichen IP-Adresse in einer Zone](./media/create-public-ip-availability-zone-portal/public-ip-address.png) 

> [!NOTE]
> Wenn Sie eine öffentliche IP-Adresse für eine Standard-SKU der Netzwerkschnittstelle eines virtuellen Computers zuweisen, müssen Sie den geplanten Datenverkehr explizit mit einer [Netzwerksicherheitsgruppe](security-overview.md#network-security-groups) zulassen. Die Kommunikation mit der Ressource schlägt fehl, bis Sie eine Netzwerksicherheitsgruppe erstellen und den gewünschten Datenverkehr explizit zulassen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Verfügbarkeitszonen](https://docs.microsoft.com/azure/availability-zones/az-overview).
- Erfahren Sie mehr über [öffentliche IP-Adressen](virtual-network-public-ip-address.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
