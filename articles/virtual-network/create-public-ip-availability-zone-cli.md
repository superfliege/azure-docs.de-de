---
title: "Erstellen einer öffentlichen IP-Adresse in einer Zone mit Azure CLI | Microsoft-Dokumentation"
description: "Erstellen Sie eine öffentliche IP-Adresse in einer Verfügbarkeitszone mit Azure CLI."
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
ms.openlocfilehash: ef93d43bbd0c58950027810c8c335d9076574326
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-public-ip-address-in-an-availability-zone-with-the-azure-cli"></a>Erstellen einer öffentlichen IP-Adresse in einer Verfügbarkeitszone mit Azure CLI

Sie können eine öffentliche IP-Adresse in einer Azure-Verfügbarkeitszone (Vorschau) bereitstellen. Eine Verfügbarkeitszone ist eine physisch separate Zone in einer Azure-Region. In diesem Artikel werden folgende Themen erläutert:

> * Erstellen einer öffentlichen IP-Adresse in einer Verfügbarkeitszone
> * Identifizieren verwandter Ressourcen, die in der Verfügbarkeitszone erstellt wurden

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.17 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

> [!NOTE]
> Die Verfügbarkeitszonen sind als Vorschauversion verfügbar und können in Ihren Entwicklungs- und Testszenarios verwendet werden. Unterstützung ist für ausgewählte Azure-Ressourcen und -Regionen sowie VM-Größenkategorien verfügbar. Weitere Informationen zu den ersten Schritten sowie zu den Azure-Ressourcen, -Regionen und VM-Größenkategorien, die mit Verfügbarkeitszonen verwendet werden können, finden Sie unter [Overview of Availability Zones in Azure (Preview) (Übersicht über Verfügbarkeitszonen in Azure (Vorschauversion))](https://docs.microsoft.com/azure/availability-zones/az-overview). Wenn Sie Unterstützung benötigen, können Sie über [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) Kontakt aufnehmen oder [ein Azure-Supportticket erstellen](../azure-supportability/how-to-create-azure-support-request.md).

## <a name="create-a-zonal-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse in einer Zone

Erstellen Sie eine öffentliche IP-Adresse in einer Verfügbarkeitszone mithilfe des folgenden Befehls:


```azurecli-interactive
az network public-ip create --resource-group myResourceGroup --name myPublicIp --zone 2 --location westeurope
```
> [!NOTE]
> Wenn Sie eine öffentliche IP-Adresse für eine Standard-SKU der Netzwerkschnittstelle eines virtuellen Computers zuweisen, müssen Sie den geplanten Datenverkehr explizit mit einer [Netzwerksicherheitsgruppe](security-overview.md#network-security-groups) zulassen. Die Kommunikation mit der Ressource schlägt fehl, bis Sie eine Netzwerksicherheitsgruppe erstellen und den gewünschten Datenverkehr explizit zulassen.

## <a name="get-zone-information-about-a-public-ip-address"></a>Abrufen von Zoneninformationen über eine öffentliche IP-Adresse

Rufen Sie die Zoneninformationen einer öffentlichen IP-Adresse mithilfe des folgenden Befehls ab:

```azurecli-interactive
az network public-ip show --resource-group myResourceGroup --name myPublicIp
```

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Verfügbarkeitszonen](https://docs.microsoft.com/azure/availability-zones/az-overview).
- Erfahren Sie mehr über [öffentliche IP-Adressen](../virtual-network/virtual-network-public-ip-address.md). 
