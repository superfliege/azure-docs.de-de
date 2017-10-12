---
title: "Erstellen einer öffentlichen IP-Adresse in einer Zone mit PowerShell | Microsoft-Dokumentation"
description: "Erstellen Sie eine öffentliche IP-Adresse in einer Verfügbarkeitszone mit PowerShell."
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
ms.openlocfilehash: bfeba57036338b4e325d2f122443f2cde0373eed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-public-ip-address-in-an-availability-zone-with-powershell"></a>Erstellen einer öffentlichen IP-Adresse in einer Verfügbarkeitszone mit PowerShell

Sie können eine öffentliche IP-Adresse in einer Azure-Verfügbarkeitszone (Vorschau) bereitstellen. Eine Verfügbarkeitszone ist eine physisch separate Zone in einer Azure-Region. In diesem Artikel werden folgende Themen erläutert:

> * Erstellen einer öffentlichen IP-Adresse in einer Verfügbarkeitszone
> * Identifizieren verwandter Ressourcen, die in der Verfügbarkeitszone erstellt wurden
  

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

In diesem Artikel wird vorausgesetzt, dass Sie Version 4.4.0 oder höher des AzureRM-Moduls installiert haben. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, installieren Sie die aktuelle Version des AzureRM-Moduls über den [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureRM).

> [!NOTE]
> Die Verfügbarkeitszonen sind als Vorschauversion verfügbar und können in Ihren Entwicklungs- und Testszenarios verwendet werden. Unterstützung ist für ausgewählte Azure-Ressourcen und -Regionen sowie VM-Größenkategorien verfügbar. Weitere Informationen zu den ersten Schritten sowie zu den Azure-Ressourcen, -Regionen und VM-Größenkategorien, die mit Verfügbarkeitszonen verwendet werden können, finden Sie unter [Overview of Availability Zones in Azure (Preview) (Übersicht über Verfügbarkeitszonen in Azure (Vorschauversion))](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview). Wenn Sie Unterstützung benötigen, können Sie über [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) Kontakt aufnehmen oder [ein Azure-Supportticket erstellen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl `Login-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Login-AzureRmAccount
```
## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* in der Region *westeurope* erstellt. *westeurope* ist eine der Azure-Regionen, die Verfügbarkeitszonen in der Vorschauversion unterstützen.

```powershell
New-AzureRmResourceGroup -Name AzTest -Location westeurope
```

## <a name="create-a-zonal-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse in einer Zone

Erstellen Sie eine öffentliche IP-Adresse in einer Verfügbarkeitszone mithilfe des folgenden Befehls:

```powershell
    New-AzureRmPublicIpAddress `
        -ResourceGroupName myResourceGroup `
        -Name myPublicIp `
        -Location westeurope `
        -AllocationMethod Static `
        -Zone 2
```

> [!NOTE]
> Wenn Sie eine öffentliche IP-Adresse für eine Standard-SKU der Netzwerkschnittstelle eines virtuellen Computers zuweisen, müssen Sie den geplanten Datenverkehr explizit mit einer [Netzwerksicherheitsgruppe](security-overview.md#network-security-groups) zulassen. Die Kommunikation mit der Ressource schlägt fehl, bis Sie eine Netzwerksicherheitsgruppe erstellen und den gewünschten Datenverkehr explizit zulassen.

## <a name="get-zone-information-about-a-public-ip-address"></a>Abrufen von Zoneninformationen über eine öffentliche IP-Adresse

Rufen Sie die Zoneninformationen einer öffentlichen IP-Adresse mithilfe des folgenden Befehls ab:

```powershell
Get-AzureRmPublicIpAddress ` 
    -ResourceGroup myResourceGroup `
    -Name myPublicIp
```

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Verfügbarkeitszonen](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview).
- Erfahren Sie mehr über [öffentliche IP-Adressen](virtual-network-public-ip-address.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 