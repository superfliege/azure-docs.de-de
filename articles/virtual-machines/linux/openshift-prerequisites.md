---
title: "Voraussetzungen für OpenShift in Azure | Microsoft-Dokumentation"
description: "Voraussetzungen für die Bereitstellung von OpenShift in Azure."
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
ms.openlocfilehash: 0c90b8a6d17fa293b6708d942afd35e1333623cb
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="common-prerequisites-for-openshift-in-azure"></a>Allgemeine Voraussetzungen für OpenShift in Azure

Wenn Sie OpenShift in Azure bereitstellen möchten, müssen einige allgemeine Voraussetzungen erfüllt sein. Diese gelten sowohl für OpenShift Origin als auch für OpenShift Container Plattform.

Die Installation von OpenShift erfolgt über Ansible-Playbooks. Zur Durchführung der Installationsschritte stellt Ansible eine SSH-Verbindung mit allen Hosts her, die dem Cluster angehören sollen.
Bei der Initiierung der SSH-Verbindung mit den Remotehosts kann keine Passphrase eingegeben werden. Daher darf dem privaten Schlüssel keine Passphrase zugeordnet sein. Andernfalls ist die Bereitstellung nicht erfolgreich.
Da alle virtuellen Computer über Resource Manager-Vorlagen bereitgestellt werden, wird der gleiche öffentliche Schlüssel für den Zugriff auf alle virtuellen Computer verwendet. Der entsprechende private Schlüssel muss in den virtuellen Computer eingeschlossen werden, der auch die Playbooks ausführt.
Aus Sicherheitsgründen verwenden wir Azure Key Vault, um den privaten Schlüssel an den virtuellen Computer zu übergeben.

Wenn für Container beständiger Speicher benötigt wird, sind persistente Volumes (PVs) erforderlich. Diesen PVs muss eine Form von persistentem Speicher zugrunde liegen. OpenShift unterstützt hierfür Azure-Datenträger (VHDs), dazu muss Azure jedoch zunächst als Cloudanbieter konfiguriert werden. In diesem Modell führt OpenShift folgende Schritte durch:

- Erstellen eines VHD-Objekts in einem Azure Storage-Konto
- Einbinden der VHD in einen virtuellen Computer und Formatieren des Volumes
- Einbinden des Volumes in den Pod

OpenShift benötigt Berechtigungen, um diese Aufgaben in Azure ausführen zu können. Hierzu wird ein Dienstprinzipal benötigt. Der Dienstprinzipal (Service Principal, SP) ist ein Sicherheitskonto in Azure Active Directory, dem Berechtigungen für Ressourcen gewährt werden.
Der Dienstprinzipal benötigt Zugriff auf die Speicherkonten und virtuellen Computer, die den Cluster bilden. Wenn alle OpenShift-Clusterressourcen in einer einzelnen Ressourcengruppe bereitgestellt werden, können dem Dienstprinzipal Berechtigungen für diese Ressourcengruppe gewährt werden.

In diesem Leitfaden erfahren Sie, wie Sie die Artefakte für die Voraussetzungen erstellen.

> [!div class="checklist"]
> * Erstellen eines KeyVault zum Verwalten von SSH-Schlüsseln für den OpenShift-Cluster
> * Erstellen eines Dienstprinzipals für den Azure-Cloudanbieter

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure 
Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm, oder klicken Sie auf **Ausprobieren**, um Cloud Shell zu verwenden.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Es empfiehlt sich, die Key Vault-Instanz in einer dedizierten Ressourcengruppe zu hosten – getrennt von der Ressourcengruppe, in der die OpenShift-Clusterressourcen bereitgestellt werden. 

Im folgenden Beispiel wird eine Ressourcengruppe namens *keyvaultrg* am Standort *eastus* erstellt.

```azurecli 
az group create --name keyvaultrg --location eastus
```

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors
Erstellen Sie einen KeyVault zum Speichern der SSH-Schlüssel für den Cluster, indem Sie den Befehl [az keyvault create](/cli/azure/keyvault#create) verwenden. Der Key Vault-Name muss global eindeutig sein.

Im folgenden Beispiel wird ein Schlüsseltresor namens *keyvault* in der Ressourcengruppe *keyvaultrg* erstellt:

```azurecli 
az keyvault create --resource-group keyvaultrg --name keyvault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>Erstellen eines SSH-Schlüssels 
Ein SSH-Schlüssel ist erforderlich, um den Zugriff auf den OpenShift Origin-Cluster sicherzustellen. Erstellen Sie mithilfe des Befehls `ssh-keygen` (Linux oder Mac) ein SSH-Schlüsselpaar.
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> Das erstellte SSH-Schlüsselpaar darf keine Passphrase verwenden.

Weitere Informationen zu SSH-Schlüsseln unter Windows finden Sie unter [Gewusst wie: Verwenden von SSH-Schlüsseln mit Windows in Azure](/azure/virtual-machines/linux/ssh-from-windows).

## <a name="store-ssh-private-key-in-key-vault"></a>Speichern des privaten SSH-Schlüssels im Schlüsseltresor
Die OpenShift-Bereitstellung verwendet den erstellten SSH-Schlüssel zum Sicherstellen des Zugriffs auf den OpenShift-Master. Damit die Bereitstellung den SSH-Schlüssel sicher abrufen kann, speichern Sie den Schlüssel im Schlüsseltresor. Verwenden Sie hierzu den folgenden Befehl:

```azurecli
az keyvault secret set --vault-name keyvault --name keysecret --file ~/.ssh/openshift.rsa
```

## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals 
OpenShift kommuniziert mit Azure unter Verwendung von Benutzername und Kennwort oder eines Dienstprinzipals. Ein Azure-Dienstprinzipal ist eine Sicherheitsidentität, die Sie mit Apps, Diensten und Automatisierungstools wie OpenShift verwenden können. Sie steuern und definieren die Berechtigungen hinsichtlich der Vorgänge, die der Dienstprinzipal in Azure ausführen können soll. Zur Erhöhung der Sicherheit über die Bereitstellung eines Benutzernamens und Kennworts hinaus wird in diesem Beispiel ein einfacher Dienstprinzipal erstellt.

Erstellen Sie mit [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) einen Dienstprinzipal, und geben Sie die Anmeldeinformationen aus, die OpenShift benötigt.

Im folgenden Beispiel wird ein Dienstprinzipal erstellt, mit Berechtigungen vom Typ „Mitwirkender“ ausgestattet und einer Ressourcengruppe namens „myResourceGroup“ zugewiesen. Führen Sie ```az group show --name myResourceGroup --query id``` unter Windows separat aus, und verwenden Sie die Ausgabe in der Option „--scopes“.

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {Strong Password} \
          --scopes $(az group show --name myResourceGroup --query id)
```

Notieren Sie die „appId“-Eigenschaft, die der Befehl zurückgibt.
```json
{
  "appId": "11111111-abcd-1234-efgh-111111111111",            
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {Strong Password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```
 > [!WARNING] 
 > Vermeiden Sie es, ein unsicheres Kennwort zu erstellen.  Befolgen Sie die Anleitung unter [Kennwortrichtlinien und -einschränkungen in Azure Active Directory](/azure/active-directory/active-directory-passwords-policy).

Weitere Informationen über Dienstprinzipale finden Sie unter [Erstellen eines Azure-Dienstprinzipals mit Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurden folgende Themen behandelt:
> [!div class="checklist"]
> * Erstellen eines KeyVault zum Verwalten von SSH-Schlüsseln für den OpenShift-Cluster
> * Erstellen eines Dienstprinzipals für den Azure-Cloudanbieter

Als Nächstes können Sie einen OpenShift-Cluster bereitstellen:

- [Bereitstellen von OpenShift Origin](./openshift-origin.md)
- [Bereitstellen von OpenShift Container Platform](./openshift-container-platform.md)

