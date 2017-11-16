---
title: "Erstellen eines öffentlichen Load Balancer Standard mit zonenredundantem öffentlichen IP-Adress-Front-End mithilfe von PowerShell | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie einen öffentlichen Load Balancer Standard mit einem zonenredundanten öffentlichen IP-Adress-Front-End mithilfe von PowerShell erstellen."
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
ms.openlocfilehash: 9987fe1273dc05a2ad10c65325ad1d487d38247e
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-public-ip-address-frontend-using-powershell"></a>Erstellen eines öffentlichen Load Balancer Standard mit zonenredundantem öffentlichen IP-Adress-Front-End mithilfe von PowerShell

In diesem Artikel wird die Erstellung eines öffentlichen [Load Balancer Standard](https://aka.ms/azureloadbalancerstandard) mit zonenredundantem Front-End mithilfe einer öffentlichen IP-Standard-Adresse erläutert.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="register-for-availability-zones-load-balancer-standard-and-public-ip-standard-preview"></a>Registrieren für Verfügbarkeitszonen, Load Balancer Standard und die öffentliche IP-Standardvorschauversion

In diesem Artikel wird vorausgesetzt, dass Sie Version 4.4.0 oder höher des AzureRM-Moduls installiert haben. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, installieren Sie die aktuelle Version des AzureRM-Moduls über den [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureRM).

>[!NOTE]
Die [Load Balancer Standard-SKU](https://aka.ms/azureloadbalancerstandard) ist derzeit als Vorschauversion verfügbar. Während der Vorschauphase weist das Feature unter Umständen nicht die gleiche Verfügbarkeit und Zuverlässigkeit wie Features in Releases mit allgemeiner Verfügbarkeit auf. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Verwenden Sie die allgemein verfügbare [Load Balancer Basic-SKU](load-balancer-overview.md) für Ihre Produktionsdienste. 

> [!NOTE]
> Die Verfügbarkeitszonen sind als Vorschauversion verfügbar und können in Ihren Entwicklungs- und Testszenarios verwendet werden. Unterstützung ist für ausgewählte Azure-Ressourcen und -Regionen sowie VM-Größenkategorien verfügbar. Weitere Informationen zu den ersten Schritten sowie zu den Azure-Ressourcen, -Regionen und VM-Größenkategorien, die mit Verfügbarkeitszonen verwendet werden können, finden Sie unter [Overview of Availability Zones in Azure (Preview) (Übersicht über Verfügbarkeitszonen in Azure (Vorschauversion))](https://docs.microsoft.com/azure/availability-zones/az-overview). Wenn Sie Unterstützung benötigen, können Sie über [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) Kontakt aufnehmen oder [ein Azure-Supportticket erstellen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

Bevor Sie eine Zone oder zonenredundante Option für die im Front-End öffentliche IP-Adresse für den Load Balancer auswählen, müssen Sie zuerst die Schritte im Artikel zum [Registrieren für die Vorschauversion der Verfügbarkeitszonen](https://docs.microsoft.com/azure/availability-zones/az-overview) durchführen.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl `Login-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie mithilfe des folgenden Befehls eine Ressourcengruppe:

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location westeurope
```

## <a name="create-a-public-ip-standard"></a>Erstellen eines öffentlichen IP-Standards 
Erstellen Sie mithilfe des folgenden Befehls einen öffentlichen IP-Standard:

```powershell
$publicIp = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Name 'myPublicIP' `
  -Location westeurope -AllocationMethod Static -Sku Standard 
```

## <a name="create-a-front-end-ip-configuration-for-the-website"></a>Erstellen einer Front-End-IP-Konfiguration für die Website

Erstellen Sie mithilfe des folgenden Befehls eine Front-End-IP-Konfiguration:

```powershell
$feip = New-AzureRmLoadBalancerFrontendIpConfig -Name 'myFrontEndPool' -PublicIpAddress $publicIp
```

## <a name="create-the-back-end-address-pool"></a>Erstellen des Back-End-Adresspools

Erstellen Sie mithilfe des folgenden Befehls einen Back-End-Adresspool:

```powershell
$bepool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name 'myBackEndPool'
```

## <a name="create-a-load-balancer-probe-on-port-80"></a>Erstellen eines Lastenausgleichstests auf Port 80

Erstellen Sie für den Lastenausgleich mithilfe des folgenden Befehls einen Integritätstest auf Port 80:

```powershell
$probe = New-AzureRmLoadBalancerProbeConfig -Name 'myHealthProbe' -Protocol Http -Port 80 `
  -RequestPath / -IntervalInSeconds 360 -ProbeCount 5
```

## <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel
 Erstellen Sie mithilfe des folgenden Befehls eine Lastenausgleichsregel:

```powershell
   $rule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $feip -BackendAddressPool  $bepool -Probe $probe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

## <a name="create-a-load-balancer"></a>Einrichten eines Load Balancers
Erstellen Sie mithilfe des folgenden Befehls einen Load Balancer Standard:

```powershell
$lb = New-AzureRmLoadBalancer -ResourceGroupName myResourceGroup -Name 'MyLoadBalancer' -Location westeurope `
  -FrontendIpConfiguration $feip -BackendAddressPool $bepool `
  -Probe $probe -LoadBalancingRule $rule -Sku Standard
```

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie [eine öffentliche IP-Adresse in einer Verfügbarkeitszone erstellen](../virtual-network/create-public-ip-availability-zone-portal.md).



