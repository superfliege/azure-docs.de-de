---
title: Bereitstellen von OpenShift Container Platform in Azure | Microsoft-Dokumentation
description: Stellen Sie OpenShift Container Platform in Azure bereit.
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
ms.openlocfilehash: 159f30fc59a050b9a4ff983e8ac84e424104b484
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="deploy-openshift-container-platform-in-azure"></a>Bereitstellen von OpenShift Container Platform in Azure

Zum Bereitstellen von Container Platform in Azure können Sie zahlreiche Methoden verwenden:

- Sie können die erforderlichen Azure-Infrastrukturkomponenten manuell bereitstellen und dann die [Dokumentation](https://docs.openshift.com/container-platform/3.6/welcome/index.html) von OpenShift Container Platform befolgen.
- Sie können auch eine vorhandene [Ressourcen-Manager-Vorlage](https://github.com/Microsoft/openshift-container-platform/) verwenden, die die Bereitstellung des OpenShift Container Platform-Clusters vereinfacht.
- Eine weitere Möglichkeit besteht darin, das [Azure Marketplace-Angebot](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview) zu nutzen.

Für alle Optionen ist ein Red Hat-Abonnement erforderlich. Während der Bereitstellung wird die Red Hat Enterprise Linux-Instanz beim Red Hat-Abonnement registriert und an die Pool-ID, die die Berechtigungen für OpenShift Container Platform enthält, angefügt.
Sie benötigen dazu einen gültigen RHSM-Benutzernamen mit Kennwort und die Pool-ID von Red Hat Subscription Manager (RHSM). Sie können diese Informationen überprüfen, indem Sie sich bei „https://access.redhat.com“ anmelden.

## <a name="deploy-by-using-the-openshift-container-platform-resource-manager-template"></a>Bereitstellen mithilfe der Ressourcen-Manager-Vorlage für OpenShift Container Platform

Zum Bereitstellen mithilfe der Ressourcen-Manager-Vorlage werden die Eingabeparameter über eine Parameterdatei angegeben. Wenn Sie Bereitstellungselemente, die nicht über die Eingabeparameter festgelegt werden, anpassen möchten, forken Sie das GitHub-Repository und ändern die entsprechenden Elemente.

Im Folgenden finden Sie einige allgemeine Optionen, die Sie anpassen können:

- VNET-CIDR (Variable in „azuredeploy.json“)
- Größe der geschützten VM (Variable in „azuredeploy.json“)
- Namenskonvention (Variable in „azuredeploy.json“)
- OpenShift-Clusterbesonderheiten, angepasst über „hosts“-Datei („deployOpenShift.sh“)

### <a name="configure-the-parameters-file"></a>Anpassen der Parameterdatei

Verwenden Sie den `appId` Wert des zuvor erstellten Dienstprinzipals für den `aadClientId`-Parameter. 

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
        "enableMetrics": {
            "value": "true"
        },
        "enableLogging": {
            "value": "true"
        },
        "enableCockpit": {
            "value": "false"
        },
        "rhsmUsernamePasswordOrActivationKey": {
            "value": "usernamepassword"
        },
        "rhsmUsernameOrOrgId": {
            "value": "{RHSM Username}"
        },
        "rhsmPasswordOrActivationKey": {
            "value": "{RHSM Password}"
        },
        "rhsmPoolId": {
            "value": "{Pool ID}"
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
        "enableAzure": {
            "value": "true"
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

Ersetzen Sie die in eckigen Klammern angegebenen Elemente durch Ihre spezifischen Informationen.

### <a name="deploy-by-using-azure-cli"></a>Bereitstellen über die Azure-Befehlszeilenschnittstelle

> [!NOTE] 
> Der folgende Befehl erfordert Azure CLI 2.0.8 oder höher. Sie können die CLI-Version mit dem `az --version`-Befehl überprüfen. Informationen zum Aktualisieren der CLI-Version finden Sie unter [Installieren der Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latesti).

Im folgende Beispiel werden der OpenShift-Cluster und alle zugehörigen Ressourcen in der Ressourcengruppe myResourceGroup mit dem Bereitstellungsnamen myOpenShiftCluster bereitgestellt. Auf die Vorlage wird direkt aus dem GitHub-Repository verwiesen. Außerdem wird die lokale Parameterdatei „azuredeploy.parameters.json“ verwendet.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Die Bereitstellung dauert mindestens 30 Minuten – die Dauer hängt von der Gesamtzahl der bereitgestellten Knoten ab. Die URL der OpenShift-Konsole und der DNS-Name des OpenShift-Masters werden am Terminal ausgegeben, wenn die Bereitstellung abgeschlossen ist.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="deploy-by-using-the-openshift-container-platform-azure-marketplace-offer"></a>Bereitstellen über das Azure Marketplace-Angebot für OpenShift Container Platform

Die einfachste Möglichkeit, OpenShift Container Platform in Azure bereitzustellen, stellt das [Azure Marketplace-Angebot](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview) dar.

Auch wenn dies die einfachste Option ist, bietet sie begrenzte Anpassungsmöglichkeiten. Das Angebot umfasst drei Konfigurationsoptionen:

- **Klein**: Stellt einen Cluster ohne Hochverfügbarkeit mit einem Masterknoten, einem Infrastrukturknoten, zwei Anwendungsknoten und einem geschützten Knoten bereit. Alle Knoten haben die VM-Größe Standard DS2v2. Dieser Cluster erfordert insgesamt 10 Kerne und eignet sich ideal für kleine Testumgebungen.
- **Mittel**: Stellt einen Cluster mit Hochverfügbarkeit mit drei Masterknoten, zwei Infrastrukturknoten, vier Anwendungsknoten und einem geschützten Knoten bereit. Alle Knoten mit Ausnahme des geschützten Knotens haben die VM-Größe Standard DS3v2. Der geschützte Knoten ist ein Standard DS2v2. Dieser Cluster erfordert 38 Kerne.
- **Groß**: stellt einen Cluster mit Hochverfügbarkeit mit drei Masterknoten, zwei Infrastrukturknoten, sechs Anwendungsknoten und einem geschützten Knoten bereit. Die Master- und Infrastrukturknoten haben die VM-Größe Standard DS3v2. Der Anwendungsknoten haben die VM-Größe Standard DS4v2 und der geschützte Knoten ist ein Standard DS2v2. Dieser Cluster erfordert 70 Kerne.

Die Konfiguration des Azure-Cloudlösungsanbieters ist für die Clustergrößen Mittel und Groß optional. Die Clustergröße Klein bietet keine Option zum Konfigurieren des Azure-Cloudlösungsanbieters.

## <a name="connect-to-the-openshift-cluster"></a>Herstellen einer Verbindung mit dem OpenShift-Cluster

Wenn die Bereitstellung abgeschlossen ist, stellen Sie eine Verbindung mit der OpenShift-Konsole her, indem Sie `OpenShift Console Uri` über den Browser verwenden. Alternativ können Sie mithilfe des folgenden Befehls eine Verbindung mit dem OpenShift-Master herstellen:

```bash
$ ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Ressourcengruppe, OpenShift-Cluster und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [az group delete](/cli/azure/group#delete) entfernen.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

- [Aufgaben nach der Bereitstellung](./openshift-post-deployment.md)
- [Behandeln von Problemen beim Bereitstellen von OpenShift in Azure](./openshift-troubleshooting.md)
- [Getting Started with OpenShift Container Platform](https://docs.openshift.com/container-platform/3.6/getting_started/index.html) (Erste Schritte mit OpenShift Container Platform)
