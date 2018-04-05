---
title: 'Beispiele für die Azure CLI 2.0: Installieren von Apps | Microsoft-Dokumentation'
description: Beispiele für die Azure CLI 2.0
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b1a8510efccf810bd4fbdd647d58d38cd87d019f
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="install-applications-into-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Installieren von Anwendungen in einer VM-Skalierungsgruppe mit der Azure CLI 2.0
Dieses Skript erstellt eine VM-Skalierungsgruppe unter Ubuntu und installiert eine einfache Webanwendung unter Verwendung der benutzerdefinierten Skripterweiterung. Nach dem Ausführen des Skripts können Sie über einen Webbrowser auf die Web-App zugreifen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript
[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine-scale-sets/install-apps/install-apps.sh "Installieren von Apps in einer Skalierungsgruppe"")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, die Skalierungsgruppe und alle dazugehörigen Ressourcen zu entfernen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts
In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine VM-Skalierungsgruppe und alle dazugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/ad/group#az_ad_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az vmss create](/cli/azure/vmss#az_vmss_create) | Erstellt die VM-Skalierungsgruppe und verbindet sie mit dem virtuellen Netzwerk, dem Subnetz und der Netzwerksicherheitsgruppe. Des Weiteren wird ein Lastenausgleich erstellt, um Datenverkehr auf mehrere VM-Instanzen zu verteilen. Dieser Befehl gibt auch das zu verwendende VM-Image und die Administratoranmeldeinformationen an.  |
| [az vmss extension set](/cli/azure/vmss/extension#az_vmss_extension_set) | Installiert die benutzerdefinierte Skripterweiterung von Azure, um ein Skript auszuführen, das die Datenträger auf den einzelnen VM-Instanzen vorbereitet. |
| [az network lb rule create](/cli/azure/network/lb/rule#az_network_lb_rule_create) | Erstellt eine Lastenausgleichsregel, um den Datenverkehr am TCP-Port 80 auf die VM-Instanzen in der Skalierungsgruppe zu verteilen. |
| [az network public-ip show](/cli/azure/network/public-ip#az_network_public_ip_show) | Ruft Informationen zu der zugewiesenen öffentlichen IP-Adresse ab, die vom Lastenausgleich verwendet wird. |
| [az group delete](/cli/azure/ad/group#delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Azure CLI 2.0 finden Sie in der [Azure CLI 2.0-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Weitere Azure CLI 2.0-Skriptbeispiele für VM-Skalierungsgruppen finden Sie in der [Dokumentation zu Azure-VM-Skalierungsgruppen](../cli-samples.md).