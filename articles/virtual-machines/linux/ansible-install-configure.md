---
title: Installieren von Ansible auf virtuellen Azure-Computern
description: Erfahren Sie, wie Sie Ansible für die Verwaltung von Azure-Ressourcen unter Ubuntu, CentOS und SLES installieren und konfigurieren.
ms.service: ansible
keywords: Ansible, Azure, DevOps, Bash, CloudShell, Playbook, Bash
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: quickstart
ms.date: 08/21/2018
ms.openlocfilehash: abf5857ed105f85e51c7eac4a5d1eb8cabe7a44a
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55815114"
---
# <a name="install-ansible-on-azure-virtual-machines"></a>Installieren von Ansible auf virtuellen Azure-Computern

Ansible ermöglicht die Automatisierung der Bereitstellung und Konfiguration von Ressourcen in Ihrer Umgebung. Sie können mit Ansible Ihre virtuellen Computer (VMs) in Azure wie jede andere Ressource verwalten. In diesem Artikel wird erläutert, wie Sie Ansible und die erforderlichen Python-SDK-Module für Azure unter einigen der am häufigsten verwendeten Linux-Distributionen installieren. Sie können Ansible auf anderen Distributionen installieren, indem Sie die installierten Pakete entsprechend an Ihre Plattform anpassen. Damit Sie Azure-Ressourcen auf sichere Weise erstellen können, erfahren Sie auch, wie Sie Anmeldeinformationen für Ansible erstellen und definieren. Eine Liste von weiteren Tools, die in Cloud Shell verfügbar sind, finden Sie unter [Features und Tools für Bash in der Azure Cloud Shell](../../cloud-shell/features.md#tools).

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement:** Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen.

- **Zugriff auf Linux oder einen virtuellen Linux-Computer:** Falls Sie keinen Linux-Computer besitzen, erstellen Sie einen [virtuellen Linux-Computer](https://docs.microsoft.com/azure/virtual-network/quick-create-cli).

- **Azure-Dienstprinzipal:** Befolgen Sie die Anweisungen im Abschnitt **Erstellen des Dienstprinzipals** des Artikels [Erstellen eines Azure-Dienstprinzipals mit der Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest#create-the-service-principal). Notieren Sie sich die Werte für **appId**, **displayName**, **password** und **tenant**.

## <a name="install-ansible-on-an-azure-linux-virtual-machine"></a>Installieren von Ansible auf einem virtuellen Azure Linux-Computer

Melden Sie sich beim Linux-Computer an, und wählen Sie eine der folgenden Distributionen aus, um Schritte zum Installieren von Ansible anzuzeigen:

- [CentOS 7.4](#centos-74)
- Ubuntu 16.04 LTS
- [SLES 12 SP2](#sles-12-sp2)

### <a name="centos-74"></a>CentOS 7.4

Installieren Sie die erforderlichen Pakete für die Module des Azure SDK für Python und Ansible, indem Sie die folgenden Befehle in ein Terminal- oder Bash-Fenster eingeben:

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

Befolgen Sie die Anweisungen im Abschnitt [Erstellen von Azure-Anmeldeinformationen](#create-azure-credentials).

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

Installieren Sie die erforderlichen Pakete für die Module des Azure SDK für Python und Ansible, indem Sie die folgenden Befehle in ein Terminal- oder Bash-Fenster eingeben:


```bash
## Install pre-requisite packages
sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

Befolgen Sie die Anweisungen im Abschnitt [Erstellen von Azure-Anmeldeinformationen](#create-azure-credentials).

### <a name="sles-12-sp2"></a>SLES 12 SP2

Installieren Sie die erforderlichen Pakete für die Module des Azure SDK für Python und Ansible, indem Sie die folgenden Befehle in ein Terminal- oder Bash-Fenster eingeben:

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 make \
    python-devel libopenssl-devel libtool python-pip python-setuptools

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]

# Remove conflicting Python cryptography package
sudo pip uninstall -y cryptography
```

Befolgen Sie die Anweisungen im Abschnitt [Erstellen von Azure-Anmeldeinformationen](#create-azure-credentials).

## <a name="create-azure-credentials"></a>Erstellen von Azure-Anmeldeinformationen

Die Abonnement-ID wird zusammen mit den beim Erstellen des Dienstprinzipals zurückgegebenen Informationen zum Konfigurieren der Ansible-Anmeldeinformationen verwendet. Hierfür stehen zwei Optionen zur Verfügung:

- [Erstellen einer Datei mit Ansible-Anmeldeinformationen](#file-credentials)
- [Verwenden von Ansible-Umgebungsvariablen](#env-credentials)

Wenn Sie Tools wie Ansible Tower oder Jenkins verwenden möchten, müssen Sie die Option zum Deklarieren der Dienstprinzipalwerte als Umgebungsvariablen nutzen.

### <a name="span-idfile-credentials-create-ansible-credentials-file"></a><span id="file-credentials"/> Erstellen einer Datei mit Ansible-Anmeldeinformationen

In diesem Abschnitt wird erläutert, wie Sie eine lokale Datei mit Anmeldeinformationen erstellen, um Anmeldeinformationen für Ansible bereitzustellen. Weitere Informationen zum Definieren von Ansible-Anmeldeinformationen finden Sie unter [Bereitstellen von Anmeldeinformationen für Azure-Module](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).

Erstellen Sie in einer Entwicklungsumgebung auf dem virtuellen Hostcomputer eine Datei mit *Anmeldeinformationen* für Ansible:

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

Fügen Sie die folgenden Zeilen in die Datei mit den *Anmeldeinformationen* ein. Ersetzen Sie dabei die Platzhalter durch die Informationen aus der Erstellung des Dienstprinzipals.

```bash
[default]
subscription_id=<your-subscription_id>
client_id=<security-principal-appid>
secret=<security-principal-password>
tenant=<security-principal-tenant>
```

Speichern und schließen Sie die Datei.

### <a name="span-idenv-credentialsuse-ansible-environment-variables"></a><span id="env-credentials"/>Verwenden von Ansible-Umgebungsvariablen

In diesem Abschnitt wird erläutert, wie Sie Ihre Anmeldeinformationen für Ansible konfigurieren, indem Sie sie als Umgebungsvariablen exportieren.

Geben Sie in einem Terminal- oder Bash-Fenster die folgenden Befehle ein:

```bash
export AZURE_SUBSCRIPTION_ID=<your-subscription_id>
export AZURE_CLIENT_ID=<security-principal-appid>
export AZURE_SECRET=<security-principal-password>
export AZURE_TENANT=<security-principal-tenant>
```

## <a name="verify-the-configuration"></a>Überprüfen der Konfiguration
Sie können nun mithilfe von Ansible eine Ressourcengruppe erstellen, um zu überprüfen, ob die Konfiguration erfolgreich war.

[!INCLUDE [create-resource-group-with-ansible.md](../../../includes/ansible-create-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"] 
> [Erstellen eines virtuellen Linux-Computers in Azure mithilfe von Ansible](./ansible-create-vm.md)
