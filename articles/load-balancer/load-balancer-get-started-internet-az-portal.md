---
title: Erstellen eines öffentlichen Load Balancer Standard mit zonenredundantem öffentlichen IP-Adressen-Front-End mithilfe des Azure-Portals | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen öffentlichen Load Balancer Standard mit zonenredundantem öffentlichen IP-Adressen-Front-End mithilfe des Azure-Portals erstellen.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: kumud
ms.openlocfilehash: 10a264609469245d4743886b58730304da3df7bb
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-public-ip-address-frontend-using-azure-portal"></a>Erstellen eines öffentlichen Load Balancer Standard mit zonenredundantem öffentlichen IP-Adressen-Front-End mithilfe des Azure-Portals

In diesem Artikel wird die Erstellung eines öffentlichen [Load Balancer Standard](https://aka.ms/azureloadbalancerstandard) mit zonenredundantem Front-End mithilfe einer öffentlichen IP-Standard-Adresse erläutert.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="register-for-availability-zones-preview"></a>Registrierung für Verfügbarkeitszonen – Vorschauversion
 
Die Verfügbarkeitszonen sind als Vorschauversion verfügbar und können in Ihren Entwicklungs- und Testszenarios verwendet werden. Unterstützung ist für ausgewählte Azure-Ressourcen und -Regionen sowie VM-Größenkategorien verfügbar. Weitere Informationen zu den ersten Schritten sowie zu den Azure-Ressourcen, -Regionen und VM-Größenkategorien, die mit Verfügbarkeitszonen verwendet werden können, finden Sie unter [Overview of Availability Zones in Azure (Preview) (Übersicht über Verfügbarkeitszonen in Azure (Vorschauversion))](https://docs.microsoft.com/azure/availability-zones/az-overview). Wenn Sie Unterstützung benötigen, können Sie über [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) Kontakt aufnehmen oder [ein Azure-Supportticket erstellen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Anmelden an Azure 

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-a-zone-redundant-load-balancer"></a>Erstellen eines zonenredundanten Lastenausgleichs

1. Navigieren Sie in einem Browser zum Azure-Portal [http://portal.azure.com](http://portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an.
2. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Netzwerk** > **Lastenausgleich**.
3. Geben Sie bei **„Create load balancer“ (Lastenausgleich erstellen) unter **Name** **myPublicLB** ein.
4. Wählen Sie unter **Typ** die Option **Öffentlich** aus.
5. Wählen Sie unter „SKU“ **Standard (Vorschau)** aus.
6. Klicken Sie auf **Öffentliche IP-Adresse**, **Neu erstellen**, und geben Sie auf der Seite **Create a public IP address** (Öffentliche IP-Adresse erstellen) unter „Name“ **myPublicIPStandard** ein. Wählen Sie zudem für **Verfügbarkeitszone (Vorschau)** **Zonenredundant** aus.
7. Wählen Sie unter **Standort** **East US2** (USA, Osten 2) aus, und klicken Sie dann auf **OK**. Die Bereitstellung des Lastenausgleichs wird dann gestartet und wird einige Minuten dauern.

    ![Erstellen eines zonenredundanten Load Balancer Standard mit dem Azure-Portal](./media/load-balancer-get-started-internet-az-portal/create-zone-redundant-load-balancer-standard.png)


## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie [eine öffentliche IP-Adresse in einer Verfügbarkeitszone erstellen](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address).



