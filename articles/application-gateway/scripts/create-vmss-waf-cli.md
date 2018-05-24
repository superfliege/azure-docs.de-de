---
title: Azure CLI-Skriptbeispiel – Einschränken von Webdatenverkehr | Microsoft-Dokumentation
description: Azure CLI-Skriptbeispiel – Erstellen Sie ein Anwendungsgateway mit einer Web Application Firewall und einer VM-Skalierungsgruppe, die OWASP-Regeln verwendet, um den Datenverkehr einzuschränken.
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
ms.openlocfilehash: 5864b77976db2ba953304a17c4622fdb6d4b7cdc
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
---
# <a name="restrict-web-traffic-using-the-azure-cli"></a>Einschränken von Webdatenverkehr mithilfe der Azure-Befehlszeilenschnittstelle

Dieses Skript erstellt ein Anwendungsgateway mit einer Web Application Firewall, das eine VM-Skalierungsgruppe als Back-End-Server verwendet. Die Web Application Firewall schränkt den Webdatenverkehr anhand von OWASP-Regeln ein. Nach dem Ausführen des Skripts können Sie das Anwendungsgateway mit dessen öffentlicher IP-Adresse testen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/application-gateway/create-vmss/create-vmss-waf.sh "Create application gateway")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, das Anwendungsgateway und alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive 
az group delete --name myResourceGroupAG --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen der Bereitstellung. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet#az_net) | Erstellen Sie ein virtuelles Netzwerk. |
| [az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Erstellt ein Subnetz in einem virtuellen Netzwerk. |
| [az network public-ip create](https://docs.microsoft.com/cli/azure/public-ip#az_network_public_ip_create) | Erstellt die öffentliche IP-Adresse für das Anwendungsgateway. |
| [az network application-gateway create](https://docs.microsoft.com/cli/azure/application-gateway#az_application_gateway_create) | Erstellen eines Anwendungsgateways |
| [az vmss create](https://docs.microsoft.com/cli/azure/vmss#az_vmss_create) | Erstellt eine VM-Skalierungsgruppe. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | Erstellt ein Speicherkonto. |
| [az monitor diagnostic-settings create](https://docs.microsoft.com/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_create) | Erstellt ein Speicherkonto. |
| [az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_show) | Ruft die öffentliche IP-Adresse für das Anwendungsgateway ab. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche CLI-Skriptbeispiele zu Anwendungsgateways finden Sie in der [Dokumentation zu Azure Application Gateway](../cli-samples.md).
