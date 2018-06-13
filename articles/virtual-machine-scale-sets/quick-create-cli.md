---
title: Schnellstart – Erstellen einer VM-Skalierungsgruppe per Azure CLI 2.0 | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie mit Azure CLI 2.0 schnell eine VM-Skalierungsgruppe erstellen können.
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/27/18
ms.author: iainfou
ms.openlocfilehash: ea2f51d79411c6edbf6ebc92b6abd7942e5bc3f0
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
ms.locfileid: "30247340"
---
# <a name="quickstart-create-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Schnellstart: Erstellen einer VM-Skalierungsgruppe per Azure CLI 2.0
Mit einer VM-Skalierungsgruppe können Sie eine Gruppe identischer, automatisch skalierender virtueller Computer bereitstellen und verwalten. Sie können die Anzahl virtueller Computer in der Skalierungsgruppe manuell skalieren oder basierend auf der Ressourcennutzung, z.B. CPU-Auslastung, Speicherbedarf oder Netzwerkdatenverkehr, Regeln für die automatische Skalierung definieren. Ein Azure Load Balancer verteilt Datenverkehr dann auf die VM-Instanzen in der Skalierungsgruppe. In diesem Schnellstart erstellen Sie eine VM-Skalierungsgruppe und stellen eine Beispielanwendung mit Azure CLI 2.0 bereit.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.29 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 


## <a name="create-a-scale-set"></a>Erstellen einer Skalierungsgruppe
Vor der Erstellung einer Skalierungsgruppe müssen Sie zunächst mit [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe erstellen. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus* erstellt:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Erstellen Sie dann mit [az vmss create](/cli/azure/vmss#az_vmss_create) eine VM-Skalierungsgruppe. Im folgenden Beispiel wird eine Skalierungsgruppe mit dem Namen *myScaleSet* erstellt, die automatisch aktualisiert wird, wenn Änderungen angewendet werden. Außerdem werden SSH-Schlüssel generiert, falls sie unter *~/.ssh/id_rsa* nicht vorhanden sind. Diese SSH-Schlüssel werden verwendet, wenn Sie sich an VM-Instanzen anmelden müssen. Wenn Sie einen vorhandenen Satz mit SSH-Schlüsseln verwenden möchten, können Sie den Parameter `--ssh-key-value` nutzen und den Speicherort Ihrer Schlüssel angeben.

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


## <a name="deploy-sample-application"></a>Bereitstellen der Beispielanwendung
Installieren Sie eine einfache Webanwendung, um Ihre Skalierungsgruppe zu testen. Die benutzerdefinierte Skripterweiterung von Azure wird zum Herunterladen und Ausführen eines Skripts verwendet, mit dem eine Anwendung auf den VM-Instanzen installiert wird. Diese Erweiterung ist hilfreich bei der Konfiguration nach der Bereitstellung, bei der Softwareinstallation oder bei anderen Konfigurations-/Verwaltungsaufgaben. Weitere Informationen finden Sie unter [Übersicht über benutzerdefinierte Skripterweiterungen](../virtual-machines/linux/extensions-customscript.md).

Verwenden Sie die benutzerdefinierte Skripterweiterung, um einen einfachen NGINX-Webserver zu installieren. Wenden Sie die benutzerdefinierte Skripterweiterung für die Installation von NGINX mit [az vmss extension set](/cli/azure/vmss/extension#az_vmss_extension_set) wie folgt an:

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroup \
  --vmss-name myScaleSet \
  --settings '{"fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate_nginx.sh"],"commandToExecute":"./automate_nginx.sh"}'
```


## <a name="allow-traffic-to-application"></a>Zulassen von Datenverkehr für die Anwendung
Nachdem die Skalierungsgruppe erstellt wurde, wird automatisch ein Azure Load Balancer bereitgestellt. Der Load Balancer verteilt Datenverkehr auf die VM-Instanzen der Skalierungsgruppe. Damit Datenverkehr die Beispielwebanwendung erreicht, erstellen Sie mit [az network lb rule create](/cli/azure/network/lb/rule#az_network_lb_rule_create) eine Regel für den Lastenausgleich. Im folgenden Beispiel wird eine Regel namens *myLoadBalancerRuleWeb* erstellt:

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


## <a name="test-your-scale-set"></a>Testen Ihrer Skalierungsgruppe
Greifen Sie in einem Webbrowser auf die Beispielwebanwendung zu, um Ihre Skalierungsgruppe in Aktion zu sehen. Rufen Sie mit [az network public-ip show](/cli/azure/network/public-ip#az_network_public_ip_show) die öffentliche IP-Adresse Ihres Load Balancers ab. Im folgenden Beispiel wird die IP-Adresse für *myScaleSetLBPublicIP* abgerufen, die als Teil der Skalierungsgruppe erstellt wurde:

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroup \
  --name myScaleSetLBPublicIP \
  --query '[ipAddress]' \
  --output tsv
```

Geben Sie die öffentliche IP-Adresse des Lastenausgleichsmoduls in einen Webbrowser ein. Das Lastenausgleichsmodul verteilt den Datenverkehr auf eine Ihrer VM-Instanzen. Dies ist im folgenden Beispiel dargestellt:

![Standardwebseite in NGINX](media/virtual-machine-scale-sets-create-cli/running-nginx-site.png)


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn die Ressourcengruppe, die Skalierungsgruppe und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie wie folgt mit dem Befehl [az group delete](/cli/azure/group#az_group_delete) entfernen. Der Parameter `--no-wait` gibt die Steuerung an die Eingabeaufforderung zurück, ohne zu warten, bis der Vorgang abgeschlossen ist. Der Parameter `--yes` bestätigt ohne eine zusätzliche Aufforderung, dass Sie die Ressourcen löschen möchten.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```


## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung haben Sie eine einfache Skalierungsgruppe erstellt und die benutzerdefinierte Skripterweiterung verwendet, um einen einfachen NGINX-Webserver auf den VM-Instanzen zu installieren. Weitere Informationen erhalten Sie, wenn Sie mit dem nächsten Tutorial zur Erstellung und Verwaltung von Azure-VM-Skalierungsgruppen fortfahren.

> [!div class="nextstepaction"]
> [Erstellen und Verwalten von Azure-VM-Skalierungsgruppen](tutorial-create-and-manage-cli.md)

