---
title: "Vorbereiten des Azure-VM-Images für die Verwendung mit cloud-init | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie ein bereits vorhandenes Azure-VM-Image für die Bereitstellung mit cloud-init vorbereiten."
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
ms.openlocfilehash: 2eb7510d4e76e4996e83f351a62c0b025b487df2
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2017
---
# <a name="prepare-an-existing-linux-azure-vm-image-for-use-with-cloud-init"></a>Vorbereiten eines vorhandenen Linux Azure-VM-Images für die Verwendung mit cloud-init
In diesem Artikel wird veranschaulicht, wie Sie einen vorhandenen virtuellen Azure-Computer so vorbereiten, dass er wieder bereitgestellt und für die Verwendung von cloud-init genutzt werden kann. Das sich ergebende Image kann verwendet werden, um einen neuen virtuellen Computer oder VM-Skalierungsgruppen bereitzustellen, die dann per cloud-init während der Bereitstellung jeweils weiter angepasst werden können.  Diese cloud-init-Skripts werden beim erstmaligen Starten ausgeführt, nachdem die Ressourcen von Azure bereitgestellt wurden. Weitere Informationen zur nativen Funktionsweise von cloud-init in Azure und zu den unterstützten Linux-Distributionen finden Sie unter [Übersicht über cloud-init](using-cloud-init.md).

## <a name="prerequisites"></a>Voraussetzungen
In diesem Dokument wird vorausgesetzt, dass Sie bereits über einen aktiven virtuellen Azure-Computer verfügen, auf dem eine unterstützte Version des Linux-Betriebssystems ausgeführt wird. Sie haben den Computer bereits gemäß Ihren Anforderungen konfiguriert, die erforderlichen Module installiert, alle erforderlichen Updates verarbeitet und dann einen Test durchgeführt, um sicherzustellen, dass Ihre Anforderungen erfüllt sind. 

## <a name="preparing-rhel-74--centos-74"></a>Vorbereiten von RHEL 7.4/CentOS 7.4
Zum Installieren von cloud-init ist es erforderlich, dass Sie eine SSH-Verbindung mit Ihrem virtuellen Linux-Computer herstellen und die folgenden Befehle ausführen.

```bash
sudo yum install -y cloud-init gdisk
sudo yum check-update cloud-init -y
sudo yum install cloud-init -y
```

Aktualisieren Sie den Abschnitt `cloud_init_modules` in `/etc/cloud/cloud.cfg` so, dass er die folgenden Module enthält:
```bash
- disk_setup
- mounts
```

Hier ist ein Beispiel dafür angegeben, wie der Abschnitt `cloud_init_modules` in der Praxis aussehen kann.
```bash
 cloud_config_modules:
 - mounts
 - locale
 - set-passwords
 - rh_subscription
 - yum-add-repo
 - package-update-upgrade-install
 - timezone
 - puppet
 - chef
 - salt-minion
 - mcollective
 - disable-ec2-metadata
 - runcmd
 - disk_setup
 - mounts
```
Eine Reihe von Aufgaben in Bezug auf das Bereitstellen und Verarbeiten von kurzlebigen Datenträgern muss in `/etc/waagent.conf` aktualisiert werden. Führen Sie die folgenden Befehle aus, um die entsprechenden Einstellungen zu aktualisieren. 
```bash
sed -i 's/Provisioning.Enabled=y/Provisioning.Enabled=n/g' /etc/waagent.conf
sed -i 's/Provisioning.UseCloudInit=n/Provisioning.UseCloudInit=y/g' /etc/waagent.conf
sed -i 's/ResourceDisk.Format=y/ResourceDisk.Format=n/g' /etc/waagent.conf
sed -i 's/ResourceDisk.EnableSwap=y/ResourceDisk.EnableSwap=n/g' /etc/waagent.conf
```
Lassen Sie nur Azure als Datenquelle für den Azure Linux-Agent zu, indem Sie die neue Datei `/etc/cloud/cloud.cfg.d/91-azure_datasource.cfg`, die die folgenden Zeilen enthält, mit einem Editor Ihrer Wahl erstellen:

```bash
# This configuration file is provided by the WALinuxAgent package.
datasource_list: [ Azure ]
```

Wenn für Ihr vorhandenes Azure-Image eine Auslagerungsdatei konfiguriert wurde und Sie die Konfiguration der Auslagerungsdatei für neue Images mit cloud-init ändern möchten, müssen Sie die vorhandene Auslagerungsdatei entfernen.

Befolgen Sie für RedHat-basierte Images die Anleitung im [RedHat-Dokument zur Entfernung der Auslagerungsdatei](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Deployment_Guide/s2-swap-removing-file.html).

Für CentOS-Images mit aktivierter Auslagerungsdatei können Sie den folgenden Befehl ausführen, um die Auslagerungsdatei zu deaktivieren:
```bash
sudo swapoff /mnt/resource/swapfile
```

Stellen Sie sicher, dass der Verweis auf die Auslagerungsdatei aus `/etc/fstab` entfernt wird. Die Ausgabe sollte in etwa wie folgt aussehen:
```text
# /etc/fstab
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=99cf66df-2fef-4aad-b226-382883643a1c / xfs defaults 0 0
UUID=7c473048-a4e7-4908-bad3-a9be22e9d37d /boot xfs defaults 0 0
```

Sie können den folgenden Befehl ausführen, um die Auslagerungsdatei zu entfernen und Speicherplatz zu sparen:
```bash
rm /mnt/resource/swapfile
```
## <a name="extra-step-for-cloud-init-prepared-image"></a>Zusätzlicher Schritt für ein per cloud-init vorbereitetes Image
> [!NOTE]
> Wenn Ihr Image zuvor ein Image war, das per **cloud-init** vorbereitet und konfiguriert wurde, müssen Sie die folgenden Schritte ausführen.

Die folgenden drei Befehle werden nur verwendet, wenn die VM, die Sie als neues spezielles Quellimage anpassen, zuvor über cloud-init bereitgestellt wurde.  Sie müssen die Befehle NICHT ausführen, wenn Ihr Image mit dem Azure Linux-Agent konfiguriert wurde.

```bash
sudo rm -rf /var/lib/cloud/instances/* 
sudo rm -rf /var/log/cloud-init*
sudo waagent -deprovision+user -force
```

## <a name="finalizing-linux-agent-setting"></a>Abschließen der Linux-Agent-Einstellung 
Der Azure Linux-Agent ist unabhängig davon, ob für die Konfiguration cloud-init verwendet wurde, auf allen Azure Platform-Images installiert.  Führen Sie den folgenden Befehl aus, um für den Benutzer das Aufheben der Bereitstellung auf dem Linux-Computer abzuschließen. 

```bash
sudo waagent -deprovision+user -force
```

Weitere Informationen zu den Azure Linux-Agent-Befehlen für die Aufhebung der Bereitstellung finden Sie unter [Azure Linux-Agent](agent-user-guide.md).

Beenden Sie die SSH-Sitzung, und führen Sie dann in Ihrer Bash-Shell die folgenden Azure CLI-Befehle aus, um die Zuordnung aufzuheben, die Generalisierung durchzuführen und ein neues Azure-VM-Image zu erstellen.  Ersetzen Sie `myResourceGroup` und `sourceVmName` durch die entsprechenden Informationen für Ihre Quell-VM (sourceVM).

```bash
az vm deallocate --resource-group myResourceGroup --name sourceVmName
az vm generalize --resource-group myResourceGroup --name sourceVmName
az image create --resource-group myResourceGroup --name myCloudInitImage --source sourceVmName
```

## <a name="next-steps"></a>Nächste Schritte
Weitere cloud-init-Beispiele für Änderungen an der Konfiguration finden Sie unter folgenden Themen:
 
- [Hinzufügen weiterer Linux-Benutzer zu virtuellen Computern](cloudinit-add-user.md)
- [Ausführen eines Paket-Managers zum Aktualisieren vorhandener Pakete beim ersten Start](cloudinit-update-vm.md)
- [Ändern des lokalen Hostnamens virtueller Computer](cloudinit-update-vm-hostname.md) 
- [Installieren von Anwendungspaketen, Aktualisieren von Konfigurationsdateien und Einfügen von Schlüsseln](tutorial-automate-vm-deployment.md)