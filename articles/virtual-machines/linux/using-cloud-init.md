---
title: Verwenden von Cloud-Init zum Anpassen einer Linux-VM | Microsoft Docs
description: "Erfahren Sie, wie Sie Cloud-Init zum Anpassen einer Linux-VM während der Erstellung mithilfe von Azure CLI 2.0 verwenden."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 10/03/2017
ms.author: iainfou
ms.openlocfilehash: 5559f258f5c29b07edb5e61be4755d67173019e0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="use-cloud-init-to-customize-a-linux-vm-in-azure"></a>Anpassen einer Linux-VM in Azure mit cloud-init | Microsoft-Dokumentation
In diesem Artikel wird gezeigt, wie Sie mithilfe von [cloud-init](https://cloudinit.readthedocs.io) auf einem virtuellen Computer (VM) den Hostnamen festlegen, Pakete aktualisieren und Benutzerkonten verwalten. Diese cloud-init-Skripts werden beim Systemstart ausgeführt, wenn Sie mit der Azure CLI 2.0 eine VM erstellen. Eine ausführliche Übersicht zum Installieren von Anwendungen, Schreiben von Konfigurationsdateien und Einfügen von Schlüsseln aus Key Vault finden Sie in [diesem Tutorial](tutorial-automate-vm-deployment.md). Sie können diese Schritte auch mit der [Azure CLI 1.0](using-cloud-init-nodejs.md) ausführen.


## <a name="cloud-init-overview"></a>Übersicht zu cloud-init
[Cloud-init](https://cloudinit.readthedocs.io) ist ein weit verbreiteter Ansatz zum Anpassen einer Linux-VM beim ersten Start. Sie können mit cloud-init Pakete installieren und Dateien schreiben oder Benutzer und Sicherheit konfigurieren. Da cloud-init während des ersten Startvorgangs ausgeführt wird, müssen Sie keine zusätzlichen Schritte oder erforderlichen Agents auf Ihre Konfiguration anwenden.

Cloud-init funktioniert auch Distributionen übergreifend. Verwenden Sie z.B. nicht **apt-get install** oder **yum install**, um ein Paket zu installieren. Stattdessen können Sie eine Liste mit zu installierenden Paketen definieren. „cloud-init“ verwendet automatisch das native Paketverwaltungstool für die ausgewählte Distribution.

Wir arbeiten mit unseren Partnern zusammen, damit cloud-init einbezogen wird und in den Images arbeitet, die sie in Azure bereitstellen. In der folgenden Tabelle ist die aktuelle cloud-init-Verfügbarkeit für Azure-Plattformimages aufgeführt:

| Alias | Herausgeber | Angebot | SKU | Version |
|:--- |:--- |:--- |:--- |:--- |:--- |
| UbuntuLTS |Canonical |UbuntuServer |16.04-LTS |neueste |
| UbuntuLTS |Canonical |UbuntuServer |14.04.5-LTS |neueste |
| CoreOS |CoreOS |CoreOS |Stable |neueste |


## <a name="set-the-hostname-with-cloud-init"></a>Festlegen des Hostnamens mit cloud-init
cloud-init-Dateien werden in [YAML](http://www.yaml.org) geschrieben. Um mit [az vm create](/cli/azure/vm#create) beim Erstellen einer VM in Azure ein cloud-init-Skript auszuführen, geben Sie die cloud-init-Datei mit dem Schalter `--custom-data` an. Sehen wir uns einige Beispiele dazu an, welche Konfigurationen Sie mit einer cloud-init-Datei vornehmen können. Ein häufiges Szenario ist das Festlegen des Hostnamens einer VM. Standardmäßig entspricht der Hostname dem Namen der VM. 

Erstellen Sie zunächst mit [az group create](/cli/azure/group#create) eine Ressourcengruppe. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus* erstellt:

```azurecli
az group create --name myResourceGroup --location eastus
```

Erstellen Sie in der aktuellen Shell eine Datei namens *cloud_init_hostname.txt*, und fügen Sie die folgende Konfiguration ein. Erstellen Sie die Datei beispielsweise in Cloud Shell, nicht auf dem lokalen Computer. Dazu können Sie einen beliebigen Editor verwenden. Geben Sie in Cloud Shell `sensible-editor cloud_init_hostname.txt` ein, um die Datei zu erstellen und eine Liste der verfügbaren Editoren anzuzeigen. Stellen Sie sicher, dass die gesamte Datei „cloud-init“ ordnungsgemäß kopiert wird, insbesondere die erste Zeile:

```yaml
#cloud-config
hostname: myhostname
```

Erstellen Sie nun mit dem Befehl [az vm create](/cli/azure/vm#create) eine VM, und geben Sie mit `--custom-data cloud_init_hostname.txt` die cloud-init-Datei an, wie im Folgenden gezeigt wird:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVMHostname \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_init_hostname.txt
```

Nach der Erstellung zeigt die Azure CLI Informationen zur VM an. Verwenden Sie `publicIpAddress`, um eine SSH-Verbindung mit Ihrer VM herzustellen. Geben Sie Ihre eigene Adresse wie folgt ein:

```bash
ssh azureuser@publicIpAddress
```

Um den Namen der VM anzuzeigen, verwenden Sie den Befehl `hostname` wie folgt:

```bash
hostname
```

Die VM sollte den Hostnamen als den in der cloud-init-Datei festgelegten Wert melden, wie in der folgenden Beispielausgabe gezeigt wird:

```bash
myhostname
```

## <a name="update-a-vm-with-cloud-init"></a>Aktualisieren einer VM mit cloud-init
Aus Sicherheitsgründen sollten Sie eine VM so konfigurieren, dass die neuesten Updates beim ersten Systemstart angewendet werden. Da cloud-init in verschiedenen Linux-Distributionen funktioniert, müssen weder `apt` oder `yum` für den Paket-Manager angegeben werden. Stattdessen definieren Sie `package_upgrade` und lassen die geeignete Methode für die verwendete Distribution vom cloud-init-Vorgang auswählen. Bei diesem Workflow können Sie dieselben cloud-init-Skripts für verschiedene Distributionen verwenden.

Um den Upgradevorgang in Aktion zu sehen, erstellen Sie eine cloud-init-Datei namens *cloud_init_upgrade.txt*, und fügen Sie die folgende Konfiguration ein:

```yaml
#cloud-config
package_upgrade: true
```

Erstellen Sie nun mit dem Befehl [az vm create](/cli/azure/vm#create) eine VM, und geben Sie mit `--custom-data cloud_init_upgrade.txt` die cloud-init-Datei an, wie im Folgenden gezeigt wird:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVMUpgrade \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_init_upgrade.txt
```

Stellen Sie eine SSH-Verbindung mit der öffentlichen IP-Adresse Ihrer VM her, die in der Ausgabe über den vorherigen Befehl gezeigt wird. Geben Sie Ihre eigene öffentliche IP-Adresse wie folgt ein:

```bash
ssh azureuser@publicIpAddress
```

Führen Sie das Paketverwaltungstool aus, und suchen Sie nach Updates. Im folgenden Beispiel wird `apt-get` auf einer Ubuntu-VM verwendet:

```bash
sudo apt-get upgrade
```

Bei der Suche nach Updates beim Systemstart sowie deren Installation durch cloud-init wurden keine anzuwendenden Updates gefunden, wie in der folgenden Beispielausgabe gezeigt wird:

```bash
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="add-a-user-to-a-vm-with-cloud-init"></a>Hinzufügen eines Benutzers zu einer VM mit cloud-init
Eine der ersten Aufgaben auf jeder neuen Linux-VM ist das Hinzufügen eines Benutzers für sich selbst, um die Verwendung von *root* zu vermeiden. SSH-Schlüssel stellen eine bewährte Methode dar, um Sicherheit und Benutzerfreundlichkeit bereitzustellen. Schlüssel werden mit diesem cloud-init-Skript zur Datei *~/.ssh/authorized_keys* hinzugefügt.

Um einen Benutzer zu einer Linux-VM hinzuzufügen, erstellen Sie eine cloud-init-Datei namens *cloud_init_upgrade.txt*, und fügen Sie die folgende Konfiguration ein: Geben Sie Ihren eigenen öffentlichen Schlüssel (z.B. den Inhalt von *~/.ssh/id_rsa.pub*) für *ssh-authorized-keys* ein:

```yaml
#cloud-config
users:
  - name: myadminuser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>
```

Erstellen Sie nun mit dem Befehl [az vm create](/cli/azure/vm#create) eine VM, und geben Sie mit `--custom-data cloud_init_add_user.txt` die cloud-init-Datei an, wie im Folgenden gezeigt wird:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVMUser \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_init_add_user.txt
```

Stellen Sie eine SSH-Verbindung mit der öffentlichen IP-Adresse Ihrer VM her, die in der Ausgabe über den vorherigen Befehl gezeigt wird. Geben Sie Ihre eigene öffentliche IP-Adresse wie folgt ein:

```bash
ssh myadminuser@publicIpAddress
```

Um zu bestätigen, dass Ihr Benutzer zur VM und den angegebenen Gruppen hinzugefügt wurde, zeigen Sie den Inhalt der Datei */etc/group* wie folgt an:

```bash
cat /etc/group
```

Die folgende Beispielausgabe zeigt, dass der Benutzer aus der Datei *cloud_init_add_user.txt* zur VM und der entsprechenden Gruppe hinzugefügt wurde:

```bash
root:x:0:
<snip />
sudo:x:27:myadminuser
<snip />
myadminuser:x:1000:
```

## <a name="next-steps"></a>Nächste Schritte
cloud-init ist eine der Standardmethoden, um Ihre Linux-VMs beim Systemstart zu ändern. In Azure können Sie auch die VM-Erweiterungen verwenden, um Ihre Linux-VM beim Systemstart oder während der Ausführung zu ändern. Beispielsweise können Sie mithilfe der Azure-VM-Erweiterung nicht nur beim ersten Start ein Skript auf einer VM ausführen, sondern auch während der Ausführung. Weitere Informationen zu VM-Erweiterungen finden Sie unter [Informationen zu Erweiterungen und Features für virtuelle Computer für Linux](extensions-features.md). Beispiele zur Verwendung der Erweiterung werden unter [Verwalten von Administratoren, SSH und Überprüfen oder Reparieren von Datenträgern auf Linux-VMs mit der VMAccess-Erweiterung und der Azure CLI 2.0](using-vmaccess-extension.md) bereitgestellt.