---
title: Erstellen einer Netzwerksicherheitsgruppe (klassisch) mithilfe von Azure CLI 1.0 | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Netzwerksicherheitsgruppe (klassisch) mit Azure CLI 1.0 erstellen und bereitstellen.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: 17d98950-5fbb-4653-bef6-d822ab37541e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: genli
ms.openlocfilehash: 5468801e56849498d712f51e71cfb31bf068398a
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38696617"
---
# <a name="create-a-network-security-group-classic-using-the-azure-cli-10"></a>Erstellen einer Netzwerksicherheitsgruppe (klassisch) mithilfe von Azure CLI 1.0
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dieser Artikel gilt für das klassische Bereitstellungsmodell. Sie können [NSGs auch im Ressourcen-Manager-Bereitstellungsmodell erstellen](tutorial-filter-network-traffic-cli.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Die folgenden Beispielbefehle für die Azure-Befehlszeilenschnittstelle setzen voraus, dass bereits eine einfache Umgebung erstellt wurde, die auf dem obigen Szenario basiert. Wenn Sie die in diesem Dokument aufgeführten Befehle ohne Veränderungen ausführen möchten, erstellen Sie zunächst eine Testumgebung, indem Sie [ein VNet erstellen](virtual-networks-create-vnet-classic-cli.md).

## <a name="create-an-nsg-for-the-front-end-subnet"></a>Erstellen einer NSG für das Front-End-Subnetz

1. Wenn Sie die Azure-Befehlszeilenschnittstelle noch nicht verwendet haben, lesen Sie [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md).
2. Wechseln Sie in den klassischen Modus:

    ```azurecli
    azure config mode asm
    ```   

3. Erstellen einer NSG:
   
    ```azurecli   
     azure network nsg create -l uswest -n NSG-FrontEnd
    ```
   
4. Erstellen Sie eine Sicherheitsregel, die den Zugriff auf Port 3389 (RDP) über das Internet zulässt:
   
    ```azurecli
    azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   ```

5. Erstellen Sie eine Regel, die den Zugriff auf Port 80 (HTTP) über das Internet zulässt:
   
    ```azurecli
    azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
    ```   

6. Ordnen Sie die NSG dem Front-End-Subnetz zu:
   
    ```azurecli
    azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd
   ```

## <a name="create-the-nsg-for-the-back-end-subnet"></a>Erstellen der NSG für das Back-End-Subnetz

1. Erstellen Sie die NSG:
   
    ```azurecli
    azure network nsg create -l uswest -n NSG-BackEnd
   ```

2. Erstellen Sie eine Regel, die den Zugriff auf Port 1433 (SQL) über das Front-End-Subnetz zulässt:
   
    ```azurecli
    azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   ```

3. Erstellen Sie eine Regel, die den Zugriff auf das Internet verweigert:
   
    ```azurecli
    azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80
   ```

4. Ordnen Sie die NSG dem Back-End-Subnetz zu:
   
    ```azurecli
    azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd
    ```