---
title: "Hinzufügen von Linux-Images zu Azure Stack"
description: "Hier erfahren Sie, wie Sie Linux-Images zu Azure Stack hinzufügen."
services: azure-stack
documentationcenter: 
author: anjayajodha
manager: byronr
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/25/2017
ms.author: anajod
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: a8763c01cba4e5a9eaa3b7842b627d6eb9661a95
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="add-linux-images-to-azure-stack"></a>Hinzufügen von Linux-Images zu Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit* 

Sie können virtuelle Linux-Computer in Azure Stack bereitstellen, indem Sie ein Linux-basiertes Image im Azure Stack-Marketplace hinzufügen. Am einfachsten wird ein Linux-Image über die Marketplace-Verwaltung zu Azure Stack hinzugefügt.

## <a name="marketplace-management"></a>Marketplace-Verwaltung

Wenn Sie Linux-Images aus Azure Marketplace herunterladen möchten, verwenden Sie die Verfahren im folgenden Artikel. Wählen Sie die Linux-Images aus, die Sie Benutzern in Azure Stack zur Verfügung stellen möchten.

[Herunterladen von Marketplace-Elementen von Azure in Azure Stack](azure-stack-download-azure-marketplace-item.md)

## <a name="download-an-image"></a>Herunterladen von Images

Sie können mit Azure Stack kompatible Linux-Images über die folgenden Links herunterladen und extrahieren:


   * [Bitnami](https://bitnami.com/azure-stack)
   * [CentOS](http://olstacks.cloudapp.net/latest/)
   * [CoreOS](https://stable.release.core-os.net/amd64-usr/current/coreos_production_azure_image.vhd.bz2)
   * [SUSE](https://download.suse.com/Download?buildid=VCFi7y7MsFQ~)
   * [Ubuntu 14.04 LTS](https://partner-images.canonical.com/azure/azure_stack/) / [Ubuntu 16.04 LTS](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)

1. Extrahieren Sie bei Bedarf das VHD-Image, und [fügen Sie das Image zum Marketplace hinzu](azure-stack-add-vm-image.md). Stellen Sie sicher, dass der `OSType`-Parameter auf `Linux` festgelegt ist.
2. Nachdem Sie das Image zum Marketplace hinzugefügt haben, wird ein Marketplace-Element erstellt, und Benutzer können einen virtuellen Linux-Computer bereitstellen.

## <a name="prepare-your-own-image"></a>Vorbereiten eines eigenen Image

Bereiten Sie gemäß den folgenden Anweisungen Ihr eigenes Linux-Image vor:
   
   * [CentOS-basierte Verteilungen](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Debian Linux](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Oracle Linux](../virtual-machines/linux/oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Red Hat Enterprise Linux](../virtual-machines/linux/redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [SLES & openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Ubuntu](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

1. Laden Sie den [Azure Linux-Agent](https://github.com/Azure/WALinuxAgent/) herunter, und installieren Sie ihn.
   
    Für die Bereitstellung Ihrer Linux-VM in Azure Stack ist Azure Linux-Agent Version 2.1.3 oder höher erforderlich. Viele der zuvor aufgeführten Verteilungen sind bereits mit dieser Agent-Version oder einer höheren Version in Form eines Pakets in den jeweiligen Repositorys ausgestattet (in der Regel als `WALinuxAgent` oder `walinuxagent` bezeichnet). Wenn die Version des Azure-Agent-Pakets jedoch niedriger ist als 2.1.3 (z.B. 2.0.18 oder niedriger), müssen Sie den Agent manuell installieren. Die installierte Version kann entweder anhand des Paketnamens oder durch Ausführen von `/usr/sbin/waagent -version` auf der VM ermittelt werden.
   
    Befolgen Sie die nachstehenden Anweisungen zur manuellen Installation des Azure Linux-Agents:
   
   a. Laden Sie zuerst den neuesten Azure Linux-Agent von [GitHub](https://github.com/Azure/WALinuxAgent/releases) herunter. Beispiel:
     
            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.16.tar.gz
   b. Entpacken Sie den Azure-Agent:
     
            # tar -vzxf v2.2.16.tar.gz
   c. Installieren von Python-Setuptools
     
        **Debian / Ubuntu**
     
            # sudo apt-get update
            # sudo apt-get install python-setuptools
     
        **Ubuntu 16.04+**
     
            # sudo apt-get install python3-setuptools
     
        **RHEL / CentOS / Oracle Linux**
     
            # sudo yum install python-setuptools
   d. Installieren Sie den Azure-Agent:
     
            # cd WALinuxAgent-2.2.16
            # sudo python3 setup.py install --register-service
     
     Auf Systemen, auf denen sowohl Python 2.x als auch Python 3.x installiert sind, muss möglicherweise folgender Befehl ausgeführt werden:
     
         sudo python3 setup.py install --register-service
     Weitere Informationen finden Sie in der [Infodatei](https://github.com/Azure/WALinuxAgent/blob/master/README.md) des Azure Linux-Agents.
2. [Fügen Sie das Image zum Marketplace hinzu](azure-stack-add-vm-image.md). Stellen Sie sicher, dass der `OSType`-Parameter auf `Linux` festgelegt ist.
3. Nachdem Sie das Image zum Marketplace hinzugefügt haben, wird ein Marketplace-Element erstellt, und Benutzer können einen virtuellen Linux-Computer bereitstellen.

## <a name="next-steps"></a>Nächste Schritte
[Übersicht über das Anbieten von Diensten in Azure Stack](azure-stack-offer-services-overview.md)


