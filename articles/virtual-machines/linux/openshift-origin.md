---
title: Bereitstellen von OKD in Azure | Microsoft-Dokumentation
description: Bereitstellen von OKD in Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: ''
ms.author: haroldw
ms.openlocfilehash: 0d3a9f05802bef7d6dfc99fcfae6668044f214c8
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190303"
---
# <a name="deploy-okd-in-azure"></a>Bereitstellen von OKD in Azure

Sie können eine von zwei Methoden verwenden, um OKD (vormals OpenShift Origin) in Azure bereitzustellen:

- Sie können manuell die erforderlichen Azure-Infrastrukturkomponenten bereitstellen und dann den Anweisungen in der [Dokumentation](https://docs.okd.io/3.10/welcome/index.html) von OKD folgen.
- Sie können auch eine vorhandene [Resource Manager-Vorlage](https://github.com/Microsoft/openshift-origin) verwenden, die die Bereitstellung des OKD-Clusters vereinfacht.

## <a name="deploy-by-using-the-okd-template"></a>Bereitstellung mithilfe der OKD-Vorlage

Verwenden Sie den Wert `appId` des zuvor erstellten Dienstprinzipals für den `aadClientId`-Parameter.

Das folgende Beispiel erstellt die Parameterdatei „azuredeploy.parameters.json“ mit allen erforderlichen Eingaben.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "masterVmSize": {
            "value": "Standard_E2s_v3"
        },
        "infraVmSize": {
            "value": "Standard_E2s_v3"
        },
        "nodeVmSize": {
            "value": "Standard_E2s_v3"
        },
        "openshiftClusterPrefix": {
            "value": "mycluster"
        },
        "masterInstanceCount": {
            "value": 3
        },
        "infraInstanceCount": {
            "value": 2
        },
        "nodeInstanceCount": {
            "value": 2
        },
        "dataDiskSize": {
            "value": 128
        },
        "adminUsername": {
            "value": "clusteradmin"
        },
        "openshiftPassword": {
            "value": "{Strong Password}"
        },
        "sshPublicKey": {
            "value": "{SSH Public Key}"
        },
        "keyVaultResourceGroup": {
            "value": "keyvaultrg"
        },
        "keyVaultName": {
            "value": "keyvault"
        },
        "keyVaultSecret": {
            "value": "keysecret"
        },
        "aadClientId": {
            "value": "11111111-abcd-1234-efgh-111111111111"
        },
        "aadClientSecret": {
            "value": "{Strong Password}"
        },
        "defaultSubDomainType": {
            "value": "nipio"
        }
    }
}
```

### <a name="deploy-by-using-azure-cli"></a>Bereitstellen über die Azure-Befehlszeilenschnittstelle


> [!NOTE] 
> Der folgende Befehl erfordert Azure CLI 2.0.8 oder höher. Sie können die CLI-Version mit dem `az --version`-Befehl überprüfen. Informationen zum Aktualisieren der CLI-Version finden Sie unter [Installieren der Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

Im folgende Beispiel werden der OKD-Cluster und alle zugehörigen Ressourcen in einer Ressourcengruppe namens „myResourceGroup“ mit dem Bereitstellungsnamen „myOpenShiftCluster“ bereitgestellt. Auf die Vorlage wird direkt aus dem GitHub-Repository verwiesen. Dazu wird eine lokale Parameterdatei namens „azuredeploy.parameters.json“ verwendet.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Die Bereitstellung dauert mindestens 25 Minuten – die Dauer hängt von der Gesamtzahl der bereitgestellten Knoten ab. Die URL der OKD-Konsole und der DNS-Name des OpenShift-Masters werden am Terminal ausgegeben, wenn die Bereitstellung abgeschlossen ist.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="connect-to-the-okd-cluster"></a>Verbindungsherstellung mit dem OKD-Cluster

Wenn die Bereitstellung abgeschlossen ist, stellen Sie eine Verbindung mit der OKD-Konsole her, indem Sie `OpenShift Console Uri` über den Browser verwenden. Alternativ können Sie mithilfe des folgenden Befehls eine Verbindung mit dem OKD-Master herstellen:

```bash
$ ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Ressourcengruppe, OpenShift-Cluster und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [az group delete](/cli/azure/group#az_group_delete) entfernen.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

- [Aufgaben nach der Bereitstellung](./openshift-post-deployment.md)
- [Beheben von Problemen bei der Bereitstellung von OpenShift](./openshift-troubleshooting.md)
- [Erste Schritte mit OKD](https://docs.okd.io/latest/getting_started/index.html)
