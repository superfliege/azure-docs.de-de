---
title: "Erstellen eines öffentlichen Load Balancer Standard mit zonenredundantem öffentlichen IP-Adressen-Front-End mithilfe des Azure-Portals | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie einen öffentlichen Load Balancer Standard mit zonenredundantem öffentlichen IP-Adressen-Front-End mithilfe des Azure-Portals erstellen."
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/20/2017
ms.author: kumud
ms.openlocfilehash: e28983f6015f8dcaa546c20fd9bc8d986a15ab5d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-public-ip-address-frontend-using-azure-portal"></a>Erstellen eines öffentlichen Load Balancer Standard mit zonenredundantem öffentlichen IP-Adressen-Front-End mithilfe des Azure-Portals

In diesem Artikel wird die Erstellung eines öffentlichen [Load Balancer Standard](https://aka.ms/azureloadbalancerstandard) mit zonenredundantem Front-End mithilfe einer öffentlichen IP-Standard-Adresse erläutert.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="register-for-availability-zones-load-balancer-standard-and-public-ip-standard-preview"></a>Registrieren für Verfügbarkeitszonen, Load Balancer Standard und die öffentliche IP-Standardvorschauversion
 
>[!NOTE]
Die [Load Balancer Standard-SKU](https://aka.ms/azureloadbalancerstandard) ist derzeit als Vorschauversion verfügbar. Während der Vorschauphase weist das Feature unter Umständen nicht die gleiche Verfügbarkeit und Zuverlässigkeit wie Features in Releases mit allgemeiner Verfügbarkeit auf. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Verwenden Sie die allgemein verfügbare [Load Balancer Basic-SKU](load-balancer-overview.md) für Ihre Produktionsdienste. 

> [!NOTE]
> Die Verfügbarkeitszonen sind als Vorschauversion verfügbar und können in Ihren Entwicklungs- und Testszenarios verwendet werden. Unterstützung ist für ausgewählte Azure-Ressourcen und -Regionen sowie VM-Größenkategorien verfügbar. Weitere Informationen zu den ersten Schritten sowie zu den Azure-Ressourcen, -Regionen und VM-Größenkategorien, die mit Verfügbarkeitszonen verwendet werden können, finden Sie unter [Overview of Availability Zones in Azure (Preview) (Übersicht über Verfügbarkeitszonen in Azure (Vorschauversion))](https://docs.microsoft.com/azure/availability-zones/az-overview). Wenn Sie Unterstützung benötigen, können Sie über [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) Kontakt aufnehmen oder [ein Azure-Supportticket erstellen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

Bevor Sie eine Zone oder zonenredundante Option für die im Front-End öffentliche IP-Adresse für den Load Balancer auswählen, müssen Sie zuerst die Schritte im Artikel zum [Registrieren für die Vorschauversion der Verfügbarkeitszonen](https://docs.microsoft.com/azure/availability-zones/az-overview) durchführen.

## <a name="log-in-to-azure"></a>Anmelden an Azure 

Melden Sie sich unter „https://portal.azure.com“ am Azure-Portal an.

## <a name="create-a-zone-redundant-load-balancer"></a>Erstellen eines zonenredundanten Lastenausgleichs

1. Navigieren Sie in einem Browser zum Azure-Portal [http://portal.azure.com](http://portal.azure.com) , und melden Sie sich mit Ihrem Azure-Konto an.
2. Klicken Sie links oben auf dem Bildschirm auf **Neu** > **Netzwerk** > **Lastenausgleich**.
3. Geben Sie bei **„Create load balancer“ (Lastenausgleich erstellen) unter **Name** **myPublicLB** ein.
4. Wählen Sie unter **Typ** die Option **Öffentlich** aus.
5. Wählen Sie unter „SKU“ **Standard (Vorschau)** aus.
6. Klicken Sie auf **Öffentliche IP-Adresse**, **Neu erstellen**, und geben Sie auf der Seite **Create a public IP address** (Öffentliche IP-Adresse erstellen) unter „Name“ **myPublicIPStandard** ein. Wählen Sie zudem für **Verfügbarkeitszone (Vorschau)** **Zonenredundant** aus.
7. Wählen Sie unter **Standort** **East US2** (USA, Osten 2) aus, und klicken Sie dann auf **OK**. Die Bereitstellung des Lastenausgleichs wird dann gestartet und wird einige Minuten dauern.

    ![Erstellen eines zonenredundanten Load Balancer Standard mit dem Azure-Portal](./media/load-balancer-get-started-internet-az-portal/create-zone-redundant-load-balancer-standard.png)


## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie [eine öffentliche IP-Adresse in einer Verfügbarkeitszone erstellen](../virtual-network/create-public-ip-availability-zone-portal.md).



