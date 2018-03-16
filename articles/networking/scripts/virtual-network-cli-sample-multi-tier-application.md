---
title: "Azure CLI-Skriptbeispiel – Erstellen eines Netzwerks für Anwendungen mit mehreren Ebenen | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen eines virtuellen Netzwerks für Anwendungen mit mehreren Ebenen."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: jdial
ms.openlocfilehash: aa5fef6e8910a5b0b5fe89d5c8cfb141415d07a6
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="create-a-network-for-multi-tier-applications"></a>Erstellen eines Netzwerks für Anwendungen mit mehreren Ebenen

In diesem Skriptbeispiel wird ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetz erstellt. Der Datenverkehr am Front-End-Subnetz ist auf HTTP und SSH beschränkt, während der Datenverkehr zum Back-End-Subnetz auf MySQL und Port 3306 beschränkt ist. Nach dem Ausführen des Skripts haben Sie zwei virtuelle Computer – einen in jedem Subnetz – denen Sie Webserver- und MySQL-Software bereitstellen können.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Beispielskript


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "Virtual network for multi-tier application")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, ein virtuelles Netzwerk und Netzwerksicherheitsgruppen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) | Erstellt ein virtuelles Azure-Netzwerk und ein Front-End-Subnetz. |
| [az network subnet create](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Erstellt ein Back-End-Subnetz. |
| [az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create) | Erstellt eine öffentliche IP-Adresse für den Zugriff auf den virtuellen Computer aus dem Internet. |
| [az network nic create](/cli/azure/network/nic#az_network_nic_create) | Erstellt virtuelle Netzwerkschnittstellen und fügt sie an das Front-End- und Back-End-Subnetz des virtuellen Netzwerks an. |
| [az network nsg create](/cli/azure/network/nsg#az_network_nsg_create) | Erstellt Netzwerksicherheitsgruppen (NSG), die dem Front-End- und Back-End-Subnetz zugeordnet sind. |
| [az network nsg rule create](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) |Erstellt NSG-Regeln, die bestimmte Ports bei bestimmten Subnetzen zulassen oder blockieren. |
| [az vm create](/cli/azure/vm#az_vm_create) | Erstellt virtuelle Computer und fügt jedem virtuellen Computer eine NIC hinzu. Dieser Befehl legt außerdem das zu verwendende VM-Image und die Administratoranmeldeinformationen fest. |
| [az group delete](/cli/azure/group#az_group_delete) | Löscht eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Zusätzliche Netzwerk-CLI-Skriptbeispiele finden Sie unter [Azure CLI Samples for networking](../cli-samples.md) (Azure CLI-Beispiele für Netzwerke).