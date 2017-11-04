---
title: Bereitstellen von OpenShift Origin in Azure | Microsoft-Dokumentation
description: Stellen Sie OpenShift Origin in Azure bereit.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: 1a40c4cc064b32aced7e976f40f6ed6a57e62204
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="deploy-openshift-origin-in-azure"></a>Bereitstellen von OpenShift Origin in Azure

Es gibt mehrere Möglichkeiten, OpenShift Origin in Azure bereitzustellen. Sie können manuell die erforderlichen Azure-Infrastrukturkomponenten bereitstellen und dann die [Dokumentation](https://docs.openshift.org/3.6/welcome/index.html) von OpenShift Origin befolgen.
Sie können auch eine vorhandene Resource Manager-Vorlage verwenden, die die Bereitstellung des OpenShift Origin-Clusters vereinfacht. Eine solche Vorlage finden Sie [hier](https://github.com/Microsoft/openshift-origin).

## <a name="deploy-using-the-openshift-origin-template"></a>Bereitstellen mithilfe der OpenShift Origin-Vorlage

Verwenden Sie den `appId` Wert des zuvor erstellten Dienstprinzipals für den `aadClientId`-Parameter.

Das folgende Beispiel erstellt die Parameterdatei **azuredeploy.parameters.json** mit allen erforderlichen Eingaben.

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

### <a name="deploy-using-azure-cli"></a>Bereitstellen über die Azure-Befehlszeilenschnittstelle


> [!NOTE] 
> Der folgende Befehl erfordert Azure CLI 2.0.8 oder höher. Sie können die Azure CLI-Version mit dem `az --version`-Befehl überprüfen. Informationen zum Aktualisieren der CLI-Version finden Sie unter [Installieren der Azure CLI 2.0]( /cli/azure/install-azure-cli).

Im folgende Beispiel werden der OpenShift-Cluster und alle zugehörigen Ressourcen in der Ressourcengruppe myResourceGroup mit dem Bereitstellungsnamen myOpenShiftCluster bereitgestellt. Auf die Vorlage wird direkt aus dem GitHub-Repository verwiesen. Außerdem wird die lokale Parameterdatei **azuredeploy.parameters.json** verwendet.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Die Bereitstellung dauert mindestens 25 Minuten – die Dauer hängt von der Gesamtzahl der bereitgestellten Knoten ab. Die URL der OpenShift-Konsole und der DNS-Name des OpenShift-Masters werden am Terminal ausgegeben, wenn die Bereitstellung abgeschlossen ist.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="connect-to-the-openshift-cluster"></a>Herstellen einer Verbindung mit dem OpenShift-Cluster

Wenn die Bereitstellung abgeschlossen ist, stellen Sie eine Verbindung mit der OpenShift-Konsole her, indem Sie `OpenShift Console Uri` über den Browser verwenden. Alternativ können Sie mithilfe des folgenden Befehls eine Verbindung mit dem OpenShift-Master herstellen:

```bash
$ ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Ressourcengruppe, OpenShift-Cluster und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [az group delete](/cli/azure/group#delete) entfernen.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

- [Aufgaben nach der Bereitstellung](./openshift-post-deployment.md)
- [Beheben von Problemen bei der Bereitstellung von OpenShift](./openshift-troubleshooting.md)
- [Getting Started with OpenShift Origin (Erste Schritte mit OpenShift Origin)](https://docs.openshift.org/latest/getting_started/index.html)
