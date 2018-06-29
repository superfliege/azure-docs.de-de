---
title: Installieren und Konfigurieren von Ansible für die Verwendung mit virtuellen Azure-Computern | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ansible für die Verwaltung von Azure-Ressourcen unter Ubuntu, CentOS und SLES installieren und konfigurieren.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: na
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/04/2018
ms.author: iainfou
ms.openlocfilehash: 262c72968ddb5985e0217bced1b83af6fdb7c03c
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37052605"
---
# <a name="install-and-configure-ansible-to-manage-virtual-machines-in-azure"></a>Installieren und Konfigurieren von Ansible für das Verwalten von virtuellen Computern in Azure

Ansible ermöglicht die Automatisierung der Bereitstellung und Konfiguration von Ressourcen in Ihrer Umgebung. Sie können mit Ansible Ihre virtuellen Computer (VMs) in Azure wie jede andere Ressource verwalten. In diesem Artikel wird erläutert, wie Sie Ansible und die erforderlichen Python-SDK-Module für Azure unter einigen der am häufigsten verwendeten Linux-Distributionen installieren. Sie können Ansible auf anderen Distributionen installieren, indem Sie die installierten Pakete entsprechend an Ihre Plattform anpassen. Damit Sie Azure-Ressourcen auf sichere Weise erstellen können, erfahren Sie auch, wie Sie Anmeldeinformationen für Ansible erstellen und definieren.

Weitere Installationsoptionen und Anleitungen für zusätzliche Plattformen finden Sie im [Ansible-Installationshandbuch](https://docs.ansible.com/ansible/intro_installation.html).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle (CLI) lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens Version 2.0.30 der Azure CLI ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu.

## <a name="install-ansible"></a>Installieren von Ansible

Eine der einfachsten Möglichkeiten, Ansible mit Azure zu verwenden, ist die Azure Cloud Shell, eine browserbasierte Shell-Umgebung zum Verwalten und Entwickeln von Azure-Ressourcen. Ansible ist bereits in der Cloud Shell installiert, sodass Sie Anweisungen zum Installieren von Ansible überspringen und mit [Erstellen von Azure-Anmeldeinformationen](#create-azure-credentials) fortfahren können. Eine Liste von weiteren Tools, die ebenfalls in der Cloud Shell verfügbar sind, finden Sie unter [Features und Tools für Bash in der Azure Cloud Shell](../../cloud-shell/features.md#tools).

Mithilfe der folgenden Anweisungen können Sie eine Linux-VM für verschiedene Distributionen erstellen und dann Ansible installieren. Wenn Sie keine Linux-VM erstellen müssen, überspringen Sie diesen ersten Schritt, um eine Azure-Ressourcengruppe zu erstellen. Wenn Sie keinen virtuellen Computer erstellen müssen, erstellen Sie zuerst mit [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus* erstellt:

```azurecli
az group create --name myResourceGroup --location eastus
```

Wählen Sie nun eine der folgenden Distributionen aus, um bei Bedarf Schritte zum Erstellen eines virtuellen Computers zu erhalten und anschließend Ansible zu installieren:

- [CentOS 7.4](#centos-74)
- [Ubuntu 16.04 LTS](#ubuntu1604-lts)
- [SLES 12 SP2](#sles-12-sp2)

### <a name="centos-74"></a>CentOS 7.4

Erstellen Sie ggf. mit [az vm create](/cli/azure/vm#az_vm_create) einen virtuellen Computer. Im folgenden Beispiel wird der virtuelle Computer *myVMAnsible* erstellt:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroup \
    --image OpenLogic:CentOS:7.4:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

Stellen Sie per SSH mithilfe der `publicIpAddress` aus der Ausgabe von der Erstellung des virtuellen Computers eine Verbindung mit diesem her:

```bash
ssh azureuser@<publicIpAddress>
```

Installieren Sie die erforderlichen Pakete für die Python-SDK-Module für Azure sowie Ansible wie folgt auf dem virtuellen Computer:

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

Fahren Sie anschließend mit dem [Erstellen von Azure-Anmeldeinformationen](#create-azure-credentials) fort.

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

Erstellen Sie ggf. mit [az vm create](/cli/azure/vm#az_vm_create) einen virtuellen Computer. Im folgenden Beispiel wird der virtuelle Computer *myVMAnsible* erstellt:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroup \
    --image Canonical:UbuntuServer:16.04-LTS:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

Stellen Sie per SSH mithilfe der `publicIpAddress` aus der Ausgabe von der Erstellung des virtuellen Computers eine Verbindung mit diesem her:

```bash
ssh azureuser@<publicIpAddress>
```

Installieren Sie die erforderlichen Pakete für die Python-SDK-Module für Azure sowie Ansible wie folgt auf dem virtuellen Computer:

```bash
## Install pre-requisite packages
sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

Fahren Sie anschließend mit dem [Erstellen von Azure-Anmeldeinformationen](#create-azure-credentials) fort.

### <a name="sles-12-sp2"></a>SLES 12 SP2

Erstellen Sie ggf. mit [az vm create](/cli/azure/vm#az_vm_create) einen virtuellen Computer. Im folgenden Beispiel wird der virtuelle Computer *myVMAnsible* erstellt:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroup \
    --image SUSE:SLES:12-SP2:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

Stellen Sie per SSH mithilfe der `publicIpAddress` aus der Ausgabe von der Erstellung des virtuellen Computers eine Verbindung mit diesem her:

```bash
ssh azureuser@<publicIpAddress>
```

Installieren Sie die erforderlichen Pakete für die Python-SDK-Module für Azure sowie Ansible wie folgt auf dem virtuellen Computer:

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 make \
    python-devel libopenssl-devel libtool python-pip python-setuptools

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]

# Remove conflicting Python cryptography package
sudo pip uninstall -y cryptography
```

Fahren Sie anschließend mit dem [Erstellen von Azure-Anmeldeinformationen](#create-azure-credentials) fort.

## <a name="create-azure-credentials"></a>Erstellen von Azure-Anmeldeinformationen

Ansible kommuniziert mit Azure unter Verwendung von Benutzername und Kennwort oder eines Dienstprinzipals. Ein Azure-Dienstprinzipal ist eine Sicherheitsidentität, die Sie mit Apps, Diensten und Automatisierungstools wie Ansible verwenden können. Sie steuern und definieren die Berechtigungen hinsichtlich der Vorgänge, die der Dienstprinzipal in Azure ausführen können soll. Zur Erhöhung der Sicherheit über die Bereitstellung eines Benutzernamens und Kennworts hinaus wird in diesem Beispiel ein einfacher Dienstprinzipal erstellt.

Erstellen Sie auf dem Hostcomputer oder in der Azure Cloud Shell einen Prinzipal mit [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac). Die für Ansible erforderlichen Anmeldeinformationen werden auf dem Bildschirm ausgegeben:

```azurecli-interactive
az ad sp create-for-rbac --query '{"client_id": appId, "secret": password, "tenant": tenant}'
```

Ein Beispiel der Ausgabe der vorherigen Befehle lautet wie folgt:

```json
{
  "client_id": "eec5624a-90f8-4386-8a87-02730b5410d5",
  "secret": "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47"
}
```

Zur Authentifizierung bei Azure müssen Sie auch Ihre Azure-Abonnement-ID mit [az account show](/cli/azure/account#az-account-show) abrufen:

```azurecli-interactive
az account show --query "{ subscription_id: id }"
```

Die Ausgabe dieser beiden Befehle verwenden Sie im nächsten Schritt.

## <a name="create-ansible-credentials-file"></a>Erstellen einer Ansible-Anmeldeinformationendatei

Zum Angeben von Anmeldeinformationen für Ansible definieren Sie Umgebungsvariablen oder erstellen eine lokale Datei mit den Anmeldeinformationen. Weitere Informationen zum Definieren von Ansible-Anmeldeinformationen finden Sie unter [Bereitstellen von Anmeldeinformationen für Azure-Module](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).

Erstellen Sie für eine Entwicklungsumgebung eine Datei mit *Anmeldeinformationen* für Ansible auf der Host-VM. Erstellen Sie eine Datei mit Anmeldeinformationen auf dem virtuellen Computer, auf dem Sie in einem vorherigen Schritt Ansible installiert haben:

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

Die Datei mit den *Anmeldeinformationen* selbst kombiniert die Abonnement-ID mit der Ausgabe der Erstellung eines Dienstprinzipals. Die Ausgabe aus dem vorherigen Befehl [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) ist identisch mit der erforderlichen Ausgabe von *client_id*, *secret* und *tenant*. Das folgende Beispiel zeigt die Werte aus der vorherigen Ausgabe in der Datei mit den *Anmeldeinformationen*. Geben Sie Ihre eigenen Werte wie folgt ein:

```bash
[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=eec5624a-90f8-4386-8a87-02730b5410d5
secret=531dcffa-3aff-4488-99bb-4816c395ea3f
tenant=72f988bf-86f1-41af-91ab-2d7cd011db47
```

Speichern und schließen Sie die Datei.

## <a name="use-ansible-environment-variables"></a>Verwenden von Ansible-Umgebungsvariablen

Wenn Sie Tools wie Ansible Tower oder Jenkins verwenden möchten, müssen Sie Umgebungsvariablen definieren. Dieser Schritt kann übersprungen werden, wenn Sie nur den Ansible-Client und die im vorherigen Schritt erstellte Datei mit den Azure-Anmeldeinformationen verwenden möchten. In Umgebungsvariablen werden die gleichen Informationen wie in der Datei mit den Azure-Anmeldeinformationen definiert:

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=eec5624a-90f8-4386-8a87-02730b5410d5
export AZURE_SECRET=531dcffa-3aff-4488-99bb-4816c395ea3f
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben nun Ansible und die erforderlichen Python-SDK-Module für Azure installiert und Anmeldeinformationen für Ansible definiert. Erfahren Sie, wie Sie [einen virtuellen Computer mit Ansible erstellen](ansible-create-vm.md). Sie können sich auch darüber informieren, wie Sie [eine vollständige Azure-VM mit den zugehörigen Ressourcen mit Ansible erstellen](ansible-create-complete-vm.md).
