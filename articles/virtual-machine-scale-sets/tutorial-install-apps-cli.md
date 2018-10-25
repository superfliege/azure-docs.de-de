---
title: 'Tutorial: Installieren von Anwendungen in einer Skalierungsgruppe mit der Azure-Befehlszeilenschnittstelle | Microsoft-Dokumentation'
description: Hier wird beschrieben, wie Sie die Azure-Befehlszeilenschnittstelle zum Installieren von Anwendungen in VM-Skalierungsgruppen mit der benutzerdefinierten Skripterweiterung verwenden.
services: virtual-machine-scale-sets
documentationcenter: ''
author: zr-msft
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2018
ms.author: zarhoads
ms.custom: mvc
ms.openlocfilehash: bfeee293dfb86db2be8e799f60b5e025477293d5
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49465558"
---
# <a name="tutorial-install-applications-in-virtual-machine-scale-sets-with-the-azure-cli"></a>Tutorial: Installieren von Anwendungen in VM-Skalierungsgruppen mit der Azure-Befehlszeilenschnittstelle
Zum Ausführen von Anwendungen auf VM-Instanzen in einer Skalierungsgruppe müssen Sie zuerst die Anwendungskomponenten und erforderlichen Dateien installieren. In einem vorherigen Tutorial wurde beschrieben, wie Sie ein benutzerdefiniertes VM-Image erstellen und verwenden, um Ihre VM-Instanzen bereitzustellen. Dieses benutzerdefinierte Image umfasste manuelle Anwendungsinstallationen und -konfigurationen. Sie können die Installation von Anwendungen auch per Skalierungsgruppe automatisieren, nachdem die einzelnen VM-Instanzen bereitgestellt wurden, oder eine Anwendung aktualisieren, die bereits in einer Skalierungsgruppe ausgeführt wird. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Automatisches Installieren von Anwendungen in Ihrer Skalierungsgruppe
> * Verwenden der benutzerdefinierten Skripterweiterung von Azure
> * Aktualisieren einer ausgeführten Anwendung in einer Skalierungsgruppe

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial mindestens die Azure CLI-Version 2.0.29 ausführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli). 


## <a name="what-is-the-azure-custom-script-extension"></a>Was ist die benutzerdefinierte Skripterweiterung von Azure?
Die benutzerdefinierte Skripterweiterung lädt Skripts auf Azure-VMs herunter und führt sie aus. Diese Erweiterung ist hilfreich bei der Konfiguration nach der Bereitstellung, bei der Softwareinstallation oder bei anderen Konfigurations-/Verwaltungsaufgaben. Skripts können aus Azure Storage oder GitHub heruntergeladen oder dem Azure-Portal zur Laufzeit für die Erweiterung bereitgestellt werden.

Die benutzerdefinierte Skripterweiterung kann in Azure Resource Manager-Vorlagen integriert und auch mit der Azure CLI, Azure PowerShell, dem Azure-Portal oder der REST-API verwendet werden. Weitere Informationen finden Sie unter [Übersicht über benutzerdefinierte Skripterweiterungen](../virtual-machines/linux/extensions-customscript.md).

Zum Verwenden der benutzerdefinierten Skripterweiterung mit der Azure CLI erstellen Sie eine JSON-Datei, mit der definiert wird, welche Dateien beschafft und welche Befehle ausgeführt werden sollen. Diese JSON-Definitionen können für die Bereitstellungen von Skalierungsgruppen übergreifend wiederverwendet werden, um einheitliche Anwendungsinstallationen zu erhalten.


## <a name="create-custom-script-extension-definition"></a>Erstellen der Definition für die benutzerdefinierte Skripterweiterung
Um die benutzerdefinierte Skripterweiterung in Aktion zu sehen, erstellen Sie eine Skalierungsgruppe, mit der der NGINX-Webserver installiert und der Hostname einer VM-Instanz der Skalierungsgruppe ausgegeben wird. Mit der folgenden Definition der benutzerdefinierten Skripterweiterung wird ein Beispielskript von GitHub heruntergeladen, die erforderlichen Pakete werden installiert, und anschließend wird der Hostname der VM-Instanz auf eine einfache HTML-Seite geschrieben.

Erstellen Sie in der aktuellen Shell eine Datei namens *customConfig.json*, und fügen Sie die folgende Konfiguration ein. Erstellen Sie die Datei beispielsweise in Cloud Shell, nicht auf dem lokalen Computer. Dazu können Sie einen beliebigen Editor verwenden. Geben Sie in Cloud Shell `sensible-editor cloudConfig.json` ein, um die Datei zu erstellen und eine Liste mit den verfügbaren Editoren anzuzeigen.

```json
{
  "fileUris": ["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate_nginx.sh"],
  "commandToExecute": "./automate_nginx.sh"
}
```


## <a name="create-a-scale-set"></a>Erstellen einer Skalierungsgruppe
Erstellen Sie mit [az group create](/cli/azure/group#create) eine Ressourcengruppe. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus* erstellt:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Erstellen Sie dann mit [az vmss create](/cli/azure/vmss#create) eine VM-Skalierungsgruppe. Im folgenden Beispiel wird eine Skalierungsgruppe mit dem Namen *myScaleSet* erstellt, und es werden SSH-Schlüssel generiert, falls sie noch nicht vorhanden sind:

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


## <a name="apply-the-custom-script-extension"></a>Anwenden der benutzerdefinierten Skripterweiterung
Wenden Sie die Konfiguration der benutzerdefinierten Skripterweiterung auf die VM-Instanzen in Ihrer Skalierungsgruppe an, indem Sie [az vmss extension set](/cli/azure/vmss/extension#set) verwenden. Im folgenden Beispiel wird die Konfiguration *customConfig.json* auf die *myScaleSet*-VM-Instanzen in der Ressourcengruppe *myResourceGroup* angewendet:

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroup \
  --vmss-name myScaleSet \
  --settings @customConfig.json
```

Jede VM-Instanz in der Skalierungsgruppe wird heruntergeladen und führt das Skript von GitHub aus. In einem komplexeren Beispiel können auch mehrere Anwendungskomponenten und Dateien installiert werden. Wenn die Skalierungsgruppe zentral hochskaliert wird, wenden die neuen VM-Instanzen automatisch dieselbe Definition der benutzerdefinierten Skripterweiterung an und installieren die erforderliche Anwendung.


## <a name="test-your-scale-set"></a>Testen Ihrer Skalierungsgruppe
Damit Datenverkehr den Webserver erreicht, erstellen Sie mit [az network lb rule create](/cli/azure/network/lb/rule#create) eine Regel für den Lastenausgleich. Im folgenden Beispiel wird eine Regel namens *myLoadBalancerRuleWeb* erstellt:

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

Um den Webserver in Aktion zu sehen, rufen Sie mit [az network public-ip show](/cli/azure/network/public-ip#show) die öffentliche IP-Adresse des Lastenausgleichsmoduls ab. Im folgenden Beispiel wird die IP-Adresse für *myScaleSetLBPublicIP* abgerufen, die als Teil der Skalierungsgruppe erstellt wurde:

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroup \
  --name myScaleSetLBPublicIP \
  --query [ipAddress] \
  --output tsv
```

Geben Sie die öffentliche IP-Adresse des Lastenausgleichsmoduls in einen Webbrowser ein. Das Lastenausgleichsmodul verteilt den Datenverkehr auf eine Ihrer VM-Instanzen. Dies ist im folgenden Beispiel dargestellt:

![Einfache Webseite in NGINX](media/tutorial-install-apps-cli/running-nginx.png)

Lassen Sie den Webbrowser geöffnet, damit Sie im nächsten Schritt die aktualisierte Version sehen können.


## <a name="update-app-deployment"></a>Aktualisieren der App-Bereitstellung
Während des Lebenszyklus einer Skalierungsgruppe müssen Sie ggf. eine aktualisierte Version Ihrer Anwendung bereitstellen. Mit der benutzerdefinierten Skripterweiterung können Sie auf ein aktualisiertes Bereitstellungsskript verweisen und die Erweiterung dann erneut auf Ihre Skalierungsgruppe anwenden. Als die Skalierungsgruppe in einem vorherigen Schritt erstellt wurde, wurde `--upgrade-policy-mode` auf *automatic* festgelegt. Mit dieser Einstellung können die VM-Instanzen in der Skalierungsgruppe aktualisiert werden, und die aktuelle Version Ihrer Anwendung kann angewendet werden.

Erstellen Sie in der aktuellen Shell eine Datei namens *customConfigv2.json*, und fügen Sie die folgende Konfiguration ein. Diese Definition führt eine aktualisierte *v2*-Version des Anwendungsinstallationsskripts aus:

```json
{
  "fileUris": ["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate_nginx_v2.sh"],
  "commandToExecute": "./automate_nginx_v2.sh"
}
```

Wenden Sie die Konfiguration der benutzerdefinierten Skripterweiterung wieder auf die VM-Instanzen in Ihrer Skalierungsgruppe an, indem Sie [az vmss extension set](/cli/azure/vmss/extension#set) verwenden. Die Definition *customConfigv2.json* wird verwendet, um die aktualisierte Version der Anwendung anzuwenden:

```azurecli-interactive
az vmss extension set \
    --publisher Microsoft.Azure.Extensions \
    --version 2.0 \
    --name CustomScript \
    --resource-group myResourceGroup \
    --vmss-name myScaleSet \
    --settings @customConfigv2.json
```

Alle VM-Instanzen in der Skalierungsgruppe werden automatisch mit der aktuellen Version der Beispielwebseite aktualisiert. Aktualisieren Sie die Webseite in Ihrem Browser, um die aktualisierte Version anzuzeigen:

![Aktualisierte Webseite in NGINX](media/tutorial-install-apps-cli/running-nginx-updated.png)


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die Ressourcengruppe und alle dazugehörigen Ressourcen mit [az group delete](/cli/azure/group#az_group_delete), um Ihre Skalierungsgruppe und die weiteren Ressourcen zu entfernen. Der Parameter `--no-wait` gibt die Steuerung an die Eingabeaufforderung zurück, ohne zu warten, bis der Vorgang abgeschlossen ist. Der Parameter `--yes` bestätigt ohne eine zusätzliche Aufforderung, dass Sie die Ressourcen löschen möchten.

```azurecli-interactive
az group delete --name myResourceGroup --no-wait --yes
```


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial wurde beschrieben, wie Sie Anwendungen in Ihrer Skalierungsgruppe mit der Azure CLI automatisch installieren und aktualisieren:

> [!div class="checklist"]
> * Automatisches Installieren von Anwendungen in Ihrer Skalierungsgruppe
> * Verwenden der benutzerdefinierten Skripterweiterung von Azure
> * Aktualisieren einer ausgeführten Anwendung in einer Skalierungsgruppe

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihre Skalierungsgruppe automatisch skalieren.

> [!div class="nextstepaction"]
> [Automatisches Skalieren Ihrer Skalierungsgruppen](tutorial-autoscale-cli.md)