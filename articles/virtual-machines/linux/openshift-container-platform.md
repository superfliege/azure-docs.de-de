---
title: Bereitstellen von OpenShift Container Platform in Azure | Microsoft-Dokumentation
description: Stellen Sie OpenShift Container Platform in Azure bereit.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/04/2018
ms.author: haroldw
ms.openlocfilehash: 1d869d822cdeb0051836a5fc5f01eb69c523f9e3
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57995548"
---
# <a name="deploy-openshift-container-platform-in-azure"></a>Bereitstellen von OpenShift Container Platform in Azure

Zum Bereitstellen von Container Platform in Azure können Sie zahlreiche Methoden verwenden:

- Sie können die erforderlichen Azure-Infrastrukturkomponenten manuell bereitstellen und sich dann an der [Dokumentation von OpenShift Container Platform](https://docs.openshift.com/container-platform) orientieren.
- Sie können auch eine vorhandene [Ressourcen-Manager-Vorlage](https://github.com/Microsoft/openshift-container-platform/) verwenden, die die Bereitstellung des OpenShift Container Platform-Clusters vereinfacht.
- Eine weitere Möglichkeit besteht darin, das [Azure Marketplace-Angebot](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview) zu nutzen.

Für alle Optionen ist ein Red Hat-Abonnement erforderlich. Während der Bereitstellung wird die Red Hat Enterprise Linux-Instanz beim Red Hat-Abonnement registriert und an die Pool-ID, die die Berechtigungen für OpenShift Container Platform enthält, angefügt.
Stellen Sie sicher, dass Sie über einen gültigen Benutzernamen, ein gültiges Kennwort und eine gültige Pool-ID für Red Hat Subscription Manager (RHSM) verfügen. Sie können einen Aktivierungsschlüssel, eine Organisations-ID und eine Pool-ID verwenden. Sie können diese Informationen überprüfen, indem Sie sich hier anmelden: https://access.redhat.com.

## <a name="deploy-using-the-openshift-container-platform-resource-manager-template"></a>Bereitstellen mithilfe der Resource Manager-Vorlage für OpenShift Container Platform

Bei der Bereitstellung mithilfe der Resource Manager-Vorlage werden die Eingabeparameter über eine Parameterdatei angegeben. Forken Sie zur weiteren Anpassung der Bereitstellung das GitHub-Repository, und ändern Sie die entsprechenden Elemente.

Im Folgenden finden Sie eine Auswahl allgemeiner Optionen, die Sie anpassen können:

- Größe der geschützten VM (Variable in „azuredeploy.json“)
- Namenskonvention (Variable in „azuredeploy.json“)
- OpenShift-Clusterbesonderheiten, angepasst über „hosts“-Datei („deployOpenShift.sh“)

### <a name="configure-the-parameters-file"></a>Anpassen der Parameterdatei

Für die [Vorlage für OpenShift Container Platform](https://github.com/Microsoft/openshift-container-platform) stehen mehrere Verzweigungen für verschiedene Versionen von OpenShift Container Platform zur Verfügung.  Die Bereitstellung kann direkt über das Repository erfolgen. Bei Bedarf können Sie aber auch das Repository forken und vor der Bereitstellung Änderungen an den Vorlagen oder Skripts vornehmen.

Verwenden Sie den `appId` Wert des zuvor erstellten Dienstprinzipals für den `aadClientId`-Parameter.

Das folgende Beispiel zeigt die Parameterdatei „azuredeploy.parameters.json“ mit allen erforderlichen Eingaben.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "masterVmSize": {
            "value": "Standard_E2s_v3"
        },
        "infraVmSize": {
            "value": "Standard_D4s_v3"
        },
        "nodeVmSize": {
            "value": "Standard_D4s_v3"
        },
        "cnsVmSize": {
            "value": "Standard_E4s_v3"
        },
        "osImageType": {
            "value": "defaultgallery"
        },
        "marketplaceOsImage": {
            "value": {
                "publisher": "RedHat",
                "offer": "RHEL",
                "sku": "7-RAW",
                "version": "latest"
            }
        },
        "storageKind": {
            "value": "managed"
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
            "value": "false"
        },
        "enableCNS": {
            "value": "false"
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
        "rhsmBrokerPoolId": {
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
        "masterClusterDnsType": {
            "value": "default"
        },
        "masterClusterDns": {
            "value": "console.contoso.com"
        },
        "routingSubDomainType": {
            "value": "nipio"
        },
        "routingSubDomain": {
            "value": "routing.contoso.com"
        },
        "virtualNetworkNewOrExisting": {
            "value": "new"
        },
        "virtualNetworkName": {
            "value": "openshiftvnet"
        },
        "addressPrefixes": {
            "value": "10.0.0.0/14"
        },
        "masterSubnetName": {
            "value": "mastersubnet"
        },
        "masterSubnetPrefix": {
            "value": "10.1.0.0/16"
        },
        "infraSubnetName": {
            "value": "infrasubnet"
        },
        "infraSubnetPrefix": {
            "value": "10.2.0.0/16"
        },
        "nodeSubnetName": {
            "value": "nodesubnet"
        },
        "nodeSubnetPrefix": {
            "value": "10.3.0.0/16"
        },
        "existingMasterSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/mastersubnet"
        },
        "existingInfraSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/masterinfrasubnet"
        },
        "existingCnsSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/cnssubnet"
        },
        "existingNodeSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/nodesubnet"
        },
        "masterClusterType": {
            "value": "public"
        },
        "masterPrivateClusterIp": {
            "value": "10.1.0.200"
        },
        "routerClusterType": {
            "value": "public"
        },
        "routerPrivateClusterIp": {
            "value": "10.2.0.201"
        },
        "routingCertType": {
            "value": "selfsigned"
        },
        "masterCertType": {
            "value": "selfsigned"
        },
        "proxySettings": {
            "value": "none"
        },
        "httpProxyEntry": {
            "value": "none"
        },
        "httpsProxyEntry": {
            "value": "none"
        },
        "noProxyEntry": {
            "value": "none"
        }
    }
}
```

Ersetzen Sie die Parameter durch Ihre eigenen Informationen.

Die Parameter können sich je nach Release unterschieden. Überprüfen Sie daher die erforderlichen Parameter für den verwendeten Branch.

### <a name="deploy-using-azure-cli"></a>Bereitstellen über die Azure-Befehlszeilenschnittstelle

> [!NOTE] 
> Der folgende Befehl erfordert Azure CLI 2.0.8 oder höher. Sie können die CLI-Version mit dem `az --version`-Befehl überprüfen. Informationen zum Aktualisieren der CLI-Version finden Sie unter [Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latesti).

Im folgenden Beispiel werden der OpenShift-Cluster und alle dazugehörigen Ressourcen in der Ressourcengruppe „openshiftrg“ mit dem Bereitstellungsnamen „myOpenShiftCluster“ bereitgestellt. Auf die Vorlage wird direkt aus dem GitHub-Repository verwiesen. Außerdem wird die lokale Parameterdatei „azuredeploy.parameters.json“ verwendet.

```azurecli 
az group deployment create -g openshiftrg --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Der Bereitstellungsvorgang dauert mindestens 30 Minuten. Die Dauer hängt von der Gesamtzahl der bereitgestellten Knoten sowie von den konfigurierten Optionen ab. Nach Abschluss der Bereitstellung werden der Bastion-DNS-FQDN und die URL der OpenShift-Konsole im Terminal ausgegeben.

```json
{
  "Bastion DNS FQDN": "bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com",
  "OpenShift Console URL": "http://openshiftlb.eastus.cloudapp.azure.com/console"
}
```

Wenn Sie die Befehlszeile nicht blockieren möchten, bis die Bereitstellung abgeschlossen ist, fügen Sie `--no-wait` als eine der Optionen für die Gruppenbereitstellung hinzu. Die Ausgabe der Bereitstellung kann über das Azure-Portal im Bereitstellungsabschnitt für die Ressourcengruppe abgerufen werden.
 
## <a name="deploy-using-the-openshift-container-platform-azure-marketplace-offer"></a>Bereitstellen über das Azure Marketplace-Angebot für OpenShift Container Platform

Die einfachste Möglichkeit, OpenShift Container Platform in Azure bereitzustellen, stellt das [Azure Marketplace-Angebot](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview) dar.

Auch wenn dies die einfachste Option ist, bietet sie begrenzte Anpassungsmöglichkeiten. Das Marketplace-Angebot umfasst folgende Konfigurationsoptionen:

- **Masterknoten**: Drei Masterknoten mit konfigurierbarem Instanztyp.
- **Infrastrukturknoten:** Drei Infrastrukturknoten mit konfigurierbarem Instanztyp.
- **Knoten**: Knotenanzahl (zwischen zwei und neun) und Instanztyp sind konfigurierbar.
- **Datenträgertyp**: Managed Disks wird verwendet.
- **Netzwerk**: Unterstützung eines neuen oder bereits vorhandenen Netzwerks sowie eines benutzerdefinierten CIDR-Bereichs.
- **CNS:** CNS kann aktiviert werden.
- **Metriken**: Metriken können aktiviert werden.
- **Protokollierung:** Protokollierung kann aktiviert werden.
- **Azure-Cloudanbieter**: Kann aktiviert werden.

## <a name="connect-to-the-openshift-cluster"></a>Herstellen einer Verbindung mit dem OpenShift-Cluster

Rufen Sie nach Abschluss der Bereitstellung die Verbindung aus dem Ausgabeabschnitt der Bereitstellung ab. Stellen Sie über Ihren Browser mithilfe der URL für die OpenShift-Konsole (`OpenShift Console URL`) eine Verbindung mit der OpenShift-Konsole her. Alternativ können Sie auch eine SSH-Verbindung mit dem Bastionhost herstellen. Im folgenden Beispiel lautet der Administratorbenutzername „clusteradmin“, und der DNS-FQDN der öffentlichen Bastion-IP-Adresse lautet „bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com“:

```bash
$ ssh clusteradmin@bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Ressourcengruppe, OpenShift-Cluster und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [az group delete](/cli/azure/group) entfernen.

```azurecli 
az group delete --name openshiftrg
```

## <a name="next-steps"></a>Nächste Schritte

- [Aufgaben nach der Bereitstellung](./openshift-post-deployment.md)
- [Behandeln von Problemen beim Bereitstellen von OpenShift in Azure](./openshift-troubleshooting.md)
- [Getting Started with OpenShift Container Platform](https://docs.openshift.com/container-platform) (Erste Schritte mit OpenShift Container Platform)

### <a name="documentation-contributors"></a>Mitwirkende an der Dokumentation

Danke an Vincent Power (vincepower) und Alfred Sin (asinn826), die dazu beitragen, diese Dokumentation auf dem neuesten Stand zu halten.
