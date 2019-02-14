---
title: Azure CLI-Skriptbeispiel – Verwalten von Webdatenverkehr | Microsoft-Dokumentation
description: Azure CLI-Skriptbeispiel – Verwalten des Webdatenverkehrs mit einem Anwendungsgateway und einer VM-Skalierungsgruppe.
services: application-gateway
documentationcenter: networking
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 99d0939b30d04fbd5c0eb7a287105bb4cf27e9f4
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56116761"
---
# <a name="manage-web-traffic-using-the-azure-cli"></a>Verwalten von Webdatenverkehr mithilfe der Azure-Befehlszeilenschnittstelle

Dieses Skript erstellt ein Anwendungsgateway, das eine VM-Skalierungsgruppe für Back-End-Server verwendet. Das Anwendungsgateway kann dann zur Verwaltung von Webdatenverkehr konfiguriert werden. Nach dem Ausführen des Skripts können Sie das Anwendungsgateway mit dessen öffentlicher IP-Adresse testen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/application-gateway/create-vmss/create-vmss.sh "Create application gateway")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, das Anwendungsgateway und alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive 
az group delete --name myResourceGroupAG --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen der Bereitstellung. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) | Erstellen Sie ein virtuelles Netzwerk. |
| [az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) | Erstellt ein Subnetz in einem virtuellen Netzwerk. |
| [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest) | Erstellt die öffentliche IP-Adresse für das Anwendungsgateway. |
| [az network application-gateway create](https://docs.microsoft.com/cli/azure/network/application-gateway?view=azure-cli-latest) | Erstellen eines Anwendungsgateways |
| [az vmss create](https://docs.microsoft.com/cli/azure/vmss) | Erstellt eine VM-Skalierungsgruppe. |
| [az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip) | Ruft die öffentliche IP-Adresse für das Anwendungsgateway ab. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche Anwendungsgateway-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../cli-samples.md).
