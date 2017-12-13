---
title: Verwenden von cloud-init zum Aktualisieren und Installieren von Paketen auf einer Linux-VM in Azure | Microsoft-Dokumentation
description: "Verwenden von cloud-init zum Aktualisieren und Installieren von Paketen auf einer Linux-VM während der Erstellung mit Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: 4209bc270a6d255c8512dd6ccd5551b556da5a6b
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2017
---
# <a name="use-cloud-init-to-update-and-install-packages-in-a-linux-vm-in-azure"></a>Verwenden von cloud-init zum Aktualisieren und Installieren von Paketen auf einer Linux-VM in Azure
In diesem Artikel wird gezeigt, wie Sie [cloud-init](https://cloudinit.readthedocs.io) zum Aktualisieren von Paketen auf einem virtuellen Linux-Computer (VM) oder in VM-Skalierungsgruppen (VMSS) während der Bereitstellung in Azure verwenden. Diese cloud-init-Skripts werden beim erstmaligen Starten ausgeführt, nachdem die Ressourcen von Azure bereitgestellt wurden. Weitere Informationen zur nativen Funktionsweise von cloud-init in Azure und zu den unterstützten Linux-Distributionen finden Sie unter [Übersicht über cloud-init](using-cloud-init.md).

## <a name="update-a-vm-with-cloud-init"></a>Aktualisieren einer VM mit cloud-init
Aus Sicherheitsgründen sollten Sie eine VM so konfigurieren, dass die neuesten Updates beim ersten Systemstart angewendet werden. Da cloud-init in verschiedenen Linux-Distributionen funktioniert, müssen weder `apt` oder `yum` für den Paket-Manager angegeben werden. Stattdessen definieren Sie `package_upgrade` und lassen die geeignete Methode für die verwendete Distribution vom cloud-init-Vorgang auswählen. Bei diesem Workflow können Sie dieselben cloud-init-Skripts für verschiedene Distributionen verwenden.

Um den Upgradevorgang in Aktion zu sehen, erstellen Sie in Ihrer aktuellen Shell eine Datei mit dem Namen *cloud_init_upgrade.txt* und fügen die unten angegebene Konfiguration ein. Erstellen Sie für dieses Beispiel die Datei in der Cloud Shell, nicht auf dem lokalen Computer. Dazu können Sie einen beliebigen Editor verwenden. Geben Sie `sensible-editor cloud_init_upgrade.txt` ein, um die Datei zu erstellen und eine Liste der verfügbaren Editoren anzuzeigen. Wählen Sie #1 aus, um den Editor **Nano** zu verwenden. Stellen Sie sicher, dass die gesamte cloud-init-Datei richtig kopiert wird, insbesondere die erste Zeile.  

```yaml
#cloud-config
package_upgrade: true
packages:
- httpd
```

Vor der Bereitstellung dieses Images müssen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe erstellen. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Erstellen Sie nun mit dem Befehl [az vm create](/cli/azure/vm#create) eine VM, und geben Sie mit `--custom-data cloud_init_upgrade.txt` die cloud-init-Datei an, wie im Folgenden gezeigt wird:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_upgrade.txt \
  --generate-ssh-keys 
```

Stellen Sie eine SSH-Verbindung mit der öffentlichen IP-Adresse Ihrer VM her, die in der Ausgabe über den vorherigen Befehl gezeigt wird. Geben Sie Ihre eigene **öffentliche IP-Adresse** wie folgt ein:

```bash
ssh <publicIpAddress>
```

Führen Sie das Paketverwaltungstool aus, und suchen Sie nach Updates. Im folgenden Beispiel wird `apt-get` auf einer Ubuntu-VM verwendet:

```bash
sudo apt-get upgrade
```

Bei der Suche nach Updates beim Systemstart sowie deren Installation durch cloud-init sollten keine anzuwendenden Updates gefunden werden. Dies ist in der folgenden Beispielausgabe dargestellt:

```bash
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

Sie können auch anzeigen, dass `httpd` installiert wurde, indem Sie `yum history` ausführen und sich die Ausgabe ansehen, in der auf `httpd` verwiesen wird. 

## <a name="next-steps"></a>Nächste Schritte
Weitere cloud-init-Beispiele für Änderungen an der Konfiguration finden Sie unter folgenden Themen:
 
- [Hinzufügen weiterer Linux-Benutzer zu virtuellen Computern](cloudinit-add-user.md)
- [Ausführen eines Paket-Managers zum Aktualisieren vorhandener Pakete beim ersten Start](cloudinit-update-vm.md)
- [Ändern des lokalen Hostnamens virtueller Computer](cloudinit-update-vm-hostname.md) 
- [Installieren von Anwendungspaketen, Aktualisieren von Konfigurationsdateien und Einfügen von Schlüsseln](tutorial-automate-vm-deployment.md)