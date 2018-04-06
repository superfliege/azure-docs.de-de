---
title: Erstellen eines öffentlichen Load Balancer Standard mit zonenredundantem öffentlichen IP-Adressen-Front-End mithilfe von Azure CLI | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen öffentlichen Load Balancer Standard mit zonenredundantem öffentlichen IP-Adressen-Front-End mithilfe von Azure CLI erstellen.
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
ms.openlocfilehash: 1a430f5c6349741e5d04626158dc89d42169a15b
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-frontend-using-azure-cli"></a>Erstellen eines öffentlichen Load Balancer Standard mit zonenredundantem Front-End mithilfe von Azure CLI

In diesem Artikel wird die Erstellung eines öffentlichen [Load Balancer Standard](https://aka.ms/azureloadbalancerstandard) mit zonenredundantem Front-End mithilfe einer öffentlichen IP-Standard-Adresse erläutert.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="register-for-availability-zones-preview"></a>Registrierung für Verfügbarkeitszonen – Vorschauversion

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.17 oder höher ausführen.  Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

> [!NOTE]
> Die Verfügbarkeitszonen sind als Vorschauversion verfügbar und können in Ihren Entwicklungs- und Testszenarios verwendet werden. Unterstützung ist für ausgewählte Azure-Ressourcen und -Regionen sowie VM-Größenkategorien verfügbar. Weitere Informationen zu den ersten Schritten sowie zu den Azure-Ressourcen, -Regionen und VM-Größenkategorien, die mit Verfügbarkeitszonen verwendet werden können, finden Sie unter [Overview of Availability Zones in Azure (Preview) (Übersicht über Verfügbarkeitszonen in Azure (Vorschauversion))](https://docs.microsoft.com/azure/availability-zones/az-overview). Wenn Sie Unterstützung benötigen, können Sie über [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) Kontakt aufnehmen oder [ein Azure-Supportticket erstellen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

Achten Sie darauf, dass Sie die aktuelle Version von [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) installiert haben und mit [az login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az_login) bei einem Azure-Konto angemeldet sind.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mithilfe des folgenden Befehls eine Ressourcengruppe:

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

## <a name="create-a-public-ip-standard"></a>Erstellen eines öffentlichen IP-Standards

Erstellen Sie mithilfe des folgenden Befehls einen öffentlichen IP-Standard:

```azurecli-interactive
az network public-ip create --resource-group myResourceGroup --name myPublicIP --sku Standard
```

## <a name="create-a-load-balancer"></a>Einrichten eines Load Balancers

Erstellen Sie mithilfe des folgenden Befehls einen öffentlichen Load Balancer Standard mit der öffentlichen Standard-IP-Adresse, die Sie im vorherigen Schritt erstellt haben:

```azurecli-interactive
az network lb create --resource-group myResourceGroup --name myLoadBalancer --public-ip-address myPublicIP --frontend-ip-name myFrontEndPool --backend-pool-name myBackEndPool --sku Standard
```

## <a name="create-an-lb-probe-on-port-80"></a>Erstellen eines LB-Tests an Port 80

Erstellen Sie mithilfe des folgenden Befehls einen Integritätstest des Lastenausgleichs:

```azurecli-interactive
az network lb probe create --resource-group myResourceGroup --lb-name myLoadBalancer \
  --name myHealthProbe --protocol tcp --port 80
```

## <a name="create-an-lb-rule-for-port-80"></a>Erstellen einer LB-Regel für Port 80

Erstellen Sie mithilfe des folgenden Befehls eine Lastenausgleichsregel:

```azurecli-interactive
az network lb rule create --resource-group myResourceGroup --lb-name myLoadBalancer --name myLoadBalancerRuleWeb \
  --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-pool-name myBackEndPool --probe-name myHealthProbe
```

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie [eine öffentliche IP-Adresse in einer Verfügbarkeitszone erstellen](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address).



