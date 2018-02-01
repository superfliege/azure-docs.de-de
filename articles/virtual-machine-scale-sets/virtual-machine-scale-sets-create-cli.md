---
title: Erstellen einer VM-Skalierungsgruppe mit Azure CLI 2.0 | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie mit Azure CLI 2.0 schnell eine VM-Skalierungsgruppe erstellen können."
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: get-started-article
ms.date: 12/19/2017
ms.author: iainfou
ms.openlocfilehash: 8794ea7d998293e7ea88ba780f67ef8a021f2298
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="create-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Erstellen einer VM-Skalierungsgruppe per Azure CLI 2.0
Mit einer VM-Skalierungsgruppe können Sie eine Gruppe identischer, automatisch skalierender virtueller Computer bereitstellen und verwalten. Sie können die Anzahl der virtuellen Computer in der Skalierungsgruppe manuell skalieren oder basierend auf der Ressourcennutzung gemäß CPU-Auslastung, Speicherbedarf oder Netzwerkdatenverkehr Regeln für die automatische Skalierung definieren. In diesem Artikel zu den ersten Schritten erstellen Sie eine VM-Skalierungsgruppe per Azure CLI 2.0. Sie können eine Skalierungsgruppe auch per [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md) oder mit dem [Azure-Portal](virtual-machine-scale-sets-create-portal.md) erstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.20 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 


## <a name="create-a-scale-set"></a>Erstellen einer Skalierungsgruppe
Vor der Erstellung einer Skalierungsgruppe müssen Sie zunächst mit [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe erstellen. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus* erstellt:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Erstellen Sie dann mit [az vmss create](/cli/azure/vmss#az_vmss_create) eine VM-Skalierungsgruppe. Im folgenden Beispiel wird eine Skalierungsgruppe mit dem Namen *myScaleSet* erstellt, und es werden SSH-Schlüssel generiert, falls sie noch nicht vorhanden sind:

```azurecli-interactive 
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --generate-ssh-keys
```

Die Erstellung und Konfiguration aller Ressourcen und virtuellen Computer der Skalierungsgruppe dauert einige Minuten.


## <a name="install-nginx-webserver"></a>Installieren eines NGINX-Webservers
Verwenden Sie zum Testen Ihrer Skalierungsgruppe die benutzerdefinierte Skripterweiterung, um ein Skript herunterzuladen und auszuführen, mit dem NGINX auf den VM-Instanzen installiert wird. Diese Erweiterung ist hilfreich bei der Konfiguration nach der Bereitstellung, bei der Softwareinstallation oder bei anderen Konfigurations-/Verwaltungsaufgaben. Weitere Informationen finden Sie unter [Übersicht über benutzerdefinierte Skripterweiterungen](../virtual-machines/linux/extensions-customscript.md).

Wenden Sie die benutzerdefinierte Skripterweiterung, mit der NGINX installiert wird, wie folgt an:

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroup \
  --vmss-name myScaleSet \
  --settings '{"fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate_nginx.sh"],"commandToExecute":"./automate_nginx.sh"}'
```


## <a name="allow-web-traffic"></a>Zulassen von Webdatenverkehr
Damit Datenverkehr den Webserver erreicht, erstellen Sie mit [az network lb rule create](/cli/azure/network/lb/rule#az_network_lb_rule_create) eine Regel für den Lastenausgleich. Im folgenden Beispiel wird eine Regel namens *myLoadBalancerRuleWeb* erstellt:

```azurecli-interactive 
az network lb rule create \
  --resource-group myResourceGroup \
  --name myLoadBalancerRuleWeb \
  --lb-name myScaleSetLB \
  --backend-pool-name myScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```


## <a name="test-your-web-server"></a>Testen des Webservers
Um den Webserver in Aktion zu sehen, rufen Sie mit [az network public-ip show](/cli/azure/network/public-ip#az_network_public_ip_show) die öffentliche IP-Adresse des Lastenausgleichsmoduls ab. Im folgenden Beispiel wird die IP-Adresse für *myScaleSetLBPublicIP* abgerufen, die als Teil der Skalierungsgruppe erstellt wurde:

```azurecli-interactive 
az network public-ip show \
  --resource-group myResourceGroup \
  --name myScaleSetLBPublicIP \
  --query [ipAddress] \
  --output tsv
```

Geben Sie die öffentliche IP-Adresse des Lastenausgleichsmoduls in einen Webbrowser ein. Das Lastenausgleichsmodul verteilt den Datenverkehr auf eine Ihrer VM-Instanzen. Dies ist im folgenden Beispiel dargestellt:

![Standardwebseite in NGINX](media/virtual-machine-scale-sets-create-cli/running-nginx-site.png)


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn die Ressourcengruppe, die Skalierungsgruppe und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie wie folgt mit dem Befehl [az group delete](/cli/azure/group#az_group_delete) entfernen:

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel zu den ersten Schritten haben Sie eine einfache Skalierungsgruppe erstellt und die benutzerdefinierte Skripterweiterung verwendet, um einen einfachen NGINX-Webserver auf den VM-Instanzen zu installieren. Erweitern Sie Ihre Skalierungsgruppe, um die Möglichkeiten in Bezug auf die Skalierbarkeit und Automatisierung zu erhöhen, indem Sie die folgenden Anleitungen verwenden:

- [Bereitstellen der App in VM-Skalierungsgruppen](virtual-machine-scale-sets-deploy-app.md)
- Automatisches Skalieren per [Azure CLI](virtual-machine-scale-sets-autoscale-cli.md), [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md) oder [Azure-Portal](virtual-machine-scale-sets-autoscale-portal.md)
- [Automatische Betriebssystemupgrades für Azure-VM-Skalierungsgruppen](virtual-machine-scale-sets-automatic-upgrade.md)