---
title: Erstellen eines öffentlichen Standard-Lastenausgleichs mit einem Zonen-Front-End mit öffentlicher IP-Adresse mithilfe des Azure-Portals | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe des Azure-Portals einen öffentlichen Standard-Lastenausgleich mit einem Zonen-Front-End mit öffentlicher IP-Adresse erstellen.
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2018
ms.author: kumud
ms.openlocfilehash: cc3d1e9f2d87da1ed7014674877212ea0be81bca
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50416765"
---
#  <a name="create-a-public-load-balancer-standard-with-zonal-public-ip-address-frontend-using-azure-portal"></a>Erstellen eines öffentlichen Standard-Lastenausgleichs mit einem Zonen-Front-End mit öffentlicher IP-Adresse mithilfe des Azure-Portals

In diesem Artikel wird die Erstellung eines öffentlichen [Standard-Lastenausgleichs](https://aka.ms/azureloadbalancerstandard) mit einem Zonen-Front-End erläutert. Um zu verstehen, wie Verfügbarkeitszonen mit dem Standard-Lastenausgleich arbeiten, lesen Sie [Standard-Lastenausgleich und Verfügbarkeitszonen](load-balancer-standard-availability-zones.md). 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

> [!NOTE]
> Unterstützung für Verfügbarkeitszonen ist für ausgewählte Azure-Ressourcen und -Regionen sowie VM-Größenkategorien verfügbar. Weitere Informationen zu den ersten Schritten sowie zu den Azure-Ressourcen, -Regionen und VM-Größenkategorien, die mit Verfügbarkeitszonen verwendet werden können, finden Sie unter [Overview of Availability Zones in Azure (Preview) (Übersicht über Verfügbarkeitszonen in Azure (Vorschauversion))](https://docs.microsoft.com/azure/availability-zones/az-overview). Wenn Sie Unterstützung benötigen, können Sie über [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) Kontakt aufnehmen oder [ein Azure-Supportticket erstellen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Anmelden an Azure 

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-a-load-balancer-with-zonal-frontend-ip-address"></a>Erstellen eines Lastenausgleichs mit Front-End-IP-Adresse in einer Zone

1. Navigieren Sie in einem Browser zum Azure-Portal [http://portal.azure.com](http://portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an.
2. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Netzwerk** > **Lastenausgleich**.
3. Geben Sie auf der Seite **Lastenausgleich erstellen** unter **Name** **myLoadBalancer** ein.
4. Wählen Sie unter **Typ** die Option **Öffentlich** aus.
5. Wählen Sie unter „SKU“ **Standard**  aus.
6. Klicken Sie auf **Öffentliche IP-Adresse wählen**, **Neue erstellen**, und geben Sie auf der Seite **Öffentliche IP-Adresse erstellen** unter „Name“ **myPublicIPZonal** ein. Wählen Sie zudem als SKU **Standard** und für die Verfügbarkeitszone **1** aus.
    
>[!NOTE] 
> Die in diesem Schritt erstellte öffentliche IP-Adresse weist standardmäßig die SKU „Standard“ auf.

7. Klicken Sie unter **Ressourcengruppe** auf **Neue erstellen**, und geben Sie als Namen der Ressourcengruppe **myResourceGroupZLB** ein.
8. Wählen Sie für **Standort** die Option **Europa, Westen** aus, und klicken Sie dann auf **OK**. Die Bereitstellung des Load Balancers wird dann gestartet und dauert einige Minuten.

    ![Erstellen eines zonenredundanten Load Balancer Standard mit dem Azure-Portal](./media/load-balancer-get-started-internet-availability-zones-zonal-portal/load-balancer-zonal-frontend.png)


## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr zu [Standard-Lastenausgleich und Verfügbarkeitszonen](load-balancer-standard-availability-zones.md).



